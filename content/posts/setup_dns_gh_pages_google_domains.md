---
title: "enh(dns): how to setup DNS using GH pages and google domains"
date: 2023-03-25
draft: false
comments: true
tags: ['dns', 'issues', 'github']
categories: ['hacking']
---

I had struggled setting up the DNS configuracion of my custom domain in GH
pages. Luckly, I found a video which make the process very straght forward.

Ingredients
-----------

- Github repo
- Github pages
- Google domains

Here's the step by step video:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/rKz6kIW4Uos" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Summary
-------

1. Create a GH page
1. Create a Domain
1. Add a CNAME file (no sure if this is mandatory)
1. Add DNS rules (A, CNAME and AAAA)
1. Test it with dig
   `$ dig mmngreco.dev +noall +answer -t A`

