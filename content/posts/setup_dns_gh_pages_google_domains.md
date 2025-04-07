---
title: "How to setup DNS using GH pages and google domains"
date: 2023-03-25
draft: false
comments: true
categories: ['programming']
tags: ['dns', 'issues', 'github', "hacking", "network"]
---

I had struggled setting up the DNS configuracion of my custom domain in GH
pages. Luckly, I found a video which make the process very straght forward.

Ingredients
-----------

- Github repo
- Github pages
- Google domains


Summary
-------

1. Create a GH page
1. Create a Domain
1. Add a CNAME file (no sure if this is mandatory)
1. Add DNS rules (A, CNAME and AAAA)
1. Test it with dig
   `$ dig asdf0x2199.dev +noall +answer -t A`

