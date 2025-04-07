---
title: "UV is Shaking Up the Production Game"
date: 2024-12-03
draft: false
categories: ["programming"]
tags: ["english", "uv"]
---

I'm diving headfirst into using `uv` in my daily grind to be more efficient in
my pile of work. So, I'm throwing myself into all sorts of random scenarios.

On this journey, I've stumbled upon some [bugs][uv-bug] and nifty features like
a super-powered cache. But, hold up! The real point of this post is how an
innovation affects to the production process.

## Intro

Surprise, surprise! I'm an economist. In econ 101, we talk about the law of
diminishing returns, which, in plain-speak, means you can't boost productivity
just by tossing more resources at something. Like, hiring a bunch more people
won't help if the room's already crowded and everyone's squished together like
sardines.

But hey, remote work's a game-changer! It lets us crank up productivity without
being limited by physical space. Yet, there are other constraints like our code
repositories (our virtual goldmines).

Many cool innovations only happened because some earlier innovation paved the
way. And without the former, thinking about the possibilities is like trying to
catch smoke with a butterfly net (not always, but very often).

That's how I see `uv`. It's totally the future of Python package managing.

## So far...

Until now, starting with Python meant jumping through the same old hoops:
setting up a virtual environment (venv), activating it, installing all your
dependencies, and then cranking out code. We just accepted this as normal, like
a hidden tax.

Some of you might have a generic venv with your fave libs all set up, ready to
roll. But that's just putting off the inevitable task of defining a proper venv
+ requirements when you share the code.

You do this because tinkering with environments is such a time-suck!

## Too lazy for this...

Let me share a fun example of this:

Recently, I wanted to whip up a newsletter for this blog. But none of the
existing solutions tickled my fancy, so I thought, "Hey, why not make a monthly
summary post with links?" It was a grand idea but not quite a newsletter (no
auto emails or subscriptions). Just when I was ready to scrap the idea, I
stumbled on [Thorsten Ball's blog][thorsten]. His clever fix was using
[Substack][substack]. Brilliant!

So, I only needed to grab links from my blog programmatically 'cause spending
hours on this isn't my jam.

I wrote something like this:

```python
import sys
import urllib.request
import webbrowser
import xml.etree.ElementTree as ET
from datetime import datetime
from email.utils import parsedate_to_datetime
from textwrap import dedent

from jinja2 import Template

def recent_articles(rss_url, month):
    # Fetch the RSS feed
    response = urllib.request.urlopen(rss_url)
    rss_feed = response.read()
    # Parse it as XML
    root = ET.fromstring(rss_feed)
    # Iterate over each item in the feed
    for item in root.findall(".//item"):
        if item is None:
            continue

        if item.find("pubDate") is None:
            continue

        # Convert the publication date to a datetime object
        pub_date = parsedate_to_datetime(item.find("pubDate").text)
        pub_date = pub_date.replace(tzinfo=None)
        title = item.find("title").text
        url = item.find("link").text
        month_floor = {"day": 1, "hour": 0, "minute": 0, "second": 0, "microsecond": 0}
        if pub_date.replace(**month_floor) == month:
            yield {"title": title, "url": url}

if __name__ == "__main__":
    import argparse

    now = datetime.now()
    month = now.month - 1

    cli = argparse.ArgumentParser(description="Generate a newsletter with the latest posts")
    cli.add_argument("month", type=int, default=month, nargs="?", help="The month to generate the newsletter for")
    args = cli.parse_args()

    one_month_ago = now.replace(month=args.month, day=1, hour=0, minute=0, second=0, microsecond=0)
    posts = recent_articles("https://asdf0x2199.dev/rss", one_month_ago)
    month_str = one_month_ago.strftime("%B")
    htmlfile = f"/tmp/newsletter-{month_str.lower()}.html"

    with open(htmlfile, "w") as f:
        template = dedent("""
        <!DOCTYPE html>
        <html>
        <body>
           <style>body { font-family: sans-serif; }</style>
           <p>Lista de artículos:</p>
           <ul>
               {% for post in posts %}
               <li><a href="{{ post["url"] }}">{{ post["title"] }}</a></li>
               {% endfor %}
           </ul>
        </body>
        </html>
        """)
        tpl = Template(template)
        f.write(tpl.render(posts=posts, month=month_str))

    sys.exit(webbrowser.open_new(f"file://{htmlfile}"))
```

That code pulls the RSS feed from my blog, filters posts for a specific month,
and builds a simple HTML file that I can copy-paste into my Substack post.

I had all the bits but didn’t want to piece them together because setting up a
venv for such a simple task sounds like work (I only needed one external
dependency, `jinja2`).

## Let's try `uv` for this...

But with `uv`, all the puzzle pieces fall into place:

```bash
uv add --script rss-monthly.py jinja2
uv run rss-monthly
```

Abracadabra! Done!


```bash
uv add --script rss-monthly.py jinja2
```

the first line adds script dependencies to my Python script,
in something called [PEP-723][pep723], like so at the top of the file:

```python
# /// script
# requires-python = ">=3.11"
# dependencies = [
#     "jinja2",
# ]
# ///
...
```

And, with the second command, it creates a venv using the right Python version,
installs the dependencies, and runs the script, all in a snap!

```bash
uv run rss-monthly
```

I foresee a time where updating script dependencies will be as easy as one
command, but for now, this is pure magic!

## Wrap Up

With these examples, I'm just trying to show you that you don’t have to stick
to the same old ways unless there's a darn good reason. `uv` opens up this
groovy new way of running Python scripts and supercharges all you Python peeps!

I used to write loads of bash and Go scripts due to dependency chaos (thanks a
lot, macOS—I never had these issues on Linux). Also, `pipx` wasn't my thing
since it plays [badly on macOS][pipx].

`uv` is mind-blowing; it works beautifully on Mac and Linux.

### Share Like a Boss

It gives you an awesome way to share scripts and (venvs) all packed in one neat
file. Picture this: you whip up a little code, slap on the dependencies, toss
it into a GitHub gist, share it, and then run:

```bash
$ uv run https://gist.githubusercontent.com/asdf8601/aeb6d3b894bfcb52aed7d8ff1edaa7f5/raw/hello-world.py
Reading inline script metadata from remote URL
hello, world from my gist!
```

Boom! Magic!


## Extra

And let's wrap it up with one last cheeky note. It also lets you play around
with testing algorithm performance on different setups (Python 3.8, 3.9,
3.10..., with all sorts of numpy, pandas versions...) so you can flex your
benchmarking muscles and pick the top-performing champ.

Picture a script like this where we can mix and match the Python and numpy
versions used:

```python
# /// script
# requires-python = "={{ PYVER }}"
# dependencies = [
#     "numpy=={{ NPVER }}",
# ]
# ///

# your code goes here
```

Let's roll with this:

1. Whip up a template
1. Cook up multiple versions of that template
1. Race against the clock

```bash
# create template
cat <<EOS > /tmp/script.tpl
# /// script
# requires-python = "=={{ PYVER }}"
# dependencies = [
#     "numpy=={{ NPVER }}",
# ]
# ///
import sys
import numpy as np
npver = np.__version__
pyver = sys.version
print(f"{pyver=}\n{npver=}")
EOS

# install python
uv python install -r 3.{10..13}

uvx --from jinja2-cli jinja2 --help

# run versions of the template
uvx --from jinja2-cli jinja2 -D PYVER=3.13 -D NPVER=2.1.3 /tmp/script.tpl | time uv run --python 3.13 -
uvx --from jinja2-cli jinja2 -D PYVER=3.12 -D NPVER=2.1.3 /tmp/script.tpl | time uv run --python 3.12 -
uvx --from jinja2-cli jinja2 -D PYVER=3.11 -D NPVER=2.1.3 /tmp/script.tpl | time uv run --python 3.11 -
uvx --from jinja2-cli jinja2 -D PYVER=3.10 -D NPVER=2.1.3 /tmp/script.tpl | time uv run --python 3.10 -
```

The beauty of this? You don't even need the tool pre-installed anymore. We can
just whip up an abstract alias and let `uv` do all the heavy lifting! 🎉

```bash
# create template
cat <<EOS > /tmp/script.tpl
# /// script
# requires-python = "=={{ PYVER }}"
# dependencies = [
#     "numpy=={{ NPVER }}",
# ]
# ///
import sys
import numpy as np
npver = np.__version__
pyver = sys.version
print(f"{pyver=}\n{npver=}")
EOS

alias j2='uvx --from jinja2-cli jinja2'

j2 -D PYVER=3.10 -D NPVER=2.1.3 /tmp/script.tpl | uv run --python 3.10 -
```


[thorsten]: https://thorstenball.com
[substack]: https://thorstenball.com/register-spill/
[pep723]: https://asdf0x2199.dev/posts/probando-pep-723/
[pipx]: https://asdf0x2199.dev/posts/pipx-with-conda/
