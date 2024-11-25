---
title: "uv: Quickstart"
date: 2024-09-19
draft: false
categories: ["programming"]
labels: ["english", "dependencies", "python", "uv"]
---

## How to quickstart with uv


```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install 3.10 3.11 3.12
uv python pin 3.11
uv venv
uv pip install -r requirements.txt
```


## References

- https://docs.astral.sh/uv/
- https://mkennedy.codes/posts/python-docker-images-using-uv-s-new-python-features/
