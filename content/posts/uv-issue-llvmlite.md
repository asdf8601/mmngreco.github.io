---
title: "uv: issue with llvmlite"
date: 2024-11-25T19:03:00+01:00
draft: false
categories: ["prgramming"]
lables: ["english", "uv", "python"]
---


I've found an issue with `uv` while creating a simple `venv`. Nothing fancy
with my dependencies. I've condensed everything in this issue:

- https://github.com/astral-sh/uv/issues/9413


But here is the minimal reproducible example of it:


```bash
rm -rf test
uv init test
cd test
uv version
# uv venv && uv pip install numpy statsforecast
uv add numpy statsforecast
```

For the time being, I'm using the latest release.

```bash
$ uv version
uv 0.5.4 (c62c83c37 2024-11-20)
```


This is what the error looks like:

```bash
$ uv init test
Initialized project `test` at `/Users/mgreco/github.com/asdf8601/blog/test`
$ cd test
$ uv version
uv 0.5.4 (c62c83c37 2024-11-20)
$ uv add numpy statsforecast
Using CPython 3.12.4
Creating virtual environment at: .venv
Resolved 15 packages in 9ms
  × Failed to download and build `llvmlite==0.36.0`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()`
      (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File
      "/Users/mgreco/.cache/uv/builds-v0/.tmpgRNnS8/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "/Users/mgreco/.cache/uv/builds-v0/.tmpgRNnS8/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 304, in _get_build_requires
          self.run_setup()
        File
      "/Users/mgreco/.cache/uv/builds-v0/.tmpgRNnS8/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File
      "/Users/mgreco/.cache/uv/builds-v0/.tmpgRNnS8/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 320, in run_setup
          exec(code, locals())
        File "<string>", line 55, in <module>
        File "<string>", line 52, in _guard_py_ver
      RuntimeError: Cannot install on Python version 3.12.4; only versions
      >=3.6,<3.10 are supported.

  help: `llvmlite` (v0.36.0) was included because `test` (v0.1.0) depends
        on `statsforecast` (v0.7.1) which depends on `numba` (v0.53.1) which
        depends on `llvmlite`
```


## Workaround

If you are lucky, you can try adding dependencies sequentially.

```bash
# the order is important
uv add statsforecast
uv add numpy
```

In my case, it solved the problem.

You can also keep using `pip` for this specific case. The good thing about this
is that despite the limitation, we cannot simply use:

```bash
uv venv
# inverted order of dependencies (simpler at the end)
uv pip install statsforecast numpy
```

I know that this can be time-consuming on a large dependency list, but maybe it
is something worth trying.
