---
title: "Markdown preview PDF friendly"
date: 2024-05-09
draft: false
categories: ["programming"]
labels: ["nvim", "markdown", "pdf"]
---

# How to print a PDF


Edit the following files:

- `~/.local/share/nvim/lazy/markdown-preview.nvim/app/_static/markdown.css`
- `~/.local/share/nvim/lazy/markdown-preview.nvim/app/_static/page.css`


add the following lines at the bottom:


```css
@media print {
  #page-header {
    display: none;
  }
  .markdown-body {
    background-color: #fff;
    border: none;
    color: none;
  }
  main {
    background-color: #fff;
  }
}
```


Mermaid example


```mermaid
flowchart TD

a --> b
```

