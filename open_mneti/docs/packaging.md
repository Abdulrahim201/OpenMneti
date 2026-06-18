# Packaging & Releases

This page documents how **django-open-mneti** is packaged and published to PyPI.

## Overview

The project uses `pyproject.toml` as the single source of build configuration, following the standard Python packaging format. Versions are set **statically** (manually updated for each release).

## Prerequisites

- Python 3.10+
- `uv` for environment management
- A [PyPI](https://pypi.org) account with an API token

## 1. Project Structure

open_mneti/

OpenMneti/

init.py

asgi.py

settings.py

urls.py

wsgi.py

manage.py

LICENSE.txt

README.md

pyproject.toml

MANIFEST.in

.gitignore


## 2. Configuration Files

### `pyproject.toml`

Defines build system, metadata, dependencies, and authors:

```toml
[build-system]
requires = ["setuptools>=77.0.3"]
build-backend = "setuptools.build_meta"

[project]
name = "django-open-mneti"
version = "0.1.0"
description = "Open Mneti - an open source Django project."
readme = { file = "README.md", content-type = "text/markdown" }
license = "Apache-2.0"
requires-python = ">=3.10"
authors = [
    {name = "First Author Name", email = "firstauthor@example.com"},
    {name = "Second Author Name", email = "secondauthor@example.com"}
]
dependencies = [
    "django>=6.0",
    "asgiref>=3.11",
    "sqlparse>=0.5",
    "tzdata>=2026.2",
]
classifiers = [
    "Environment :: Web Environment",
    "Framework :: Django",
    "Framework :: Django :: 6.0",
    "Intended Audience :: Developers",
    "Operating System :: OS Independent",
    "Programming Language :: Python :: 3",
    "Topic :: Internet :: WWW/HTTP",
]

[project.urls]
Homepage = "https://github.com/Abdulrahim201/OpenMneti"
```

### `MANIFEST.in`

Ensures non-Python files are included in the source distribution:

include manage.py

include README.md

include LICENSE.txt

recursive-include OpenMneti *.py

### `.gitignore`

Keeps build artifacts and secrets out of version control:

dist/

*.egg-info/

pycache/

.venv/

.pypirc

## 3. PyPI Account Setup

1. Register at [pypi.org](https://pypi.org/account/register)
2. Verify email
3. Generate an API token under **Account Settings → API Tokens**
4. Store it locally in `~/.pypirc`:

```ini
[pypi]
  username = __token__
  password = pypi-xxxxxxxxxxxxxxxxxx
```

> `.pypirc` must never be committed to GitHub.

## 4. Install Build Tools

```bash
uv pip install build twine
```

## 5. Build the Package

Run from the project root (same folder as `pyproject.toml`):

```bash
python -m build
```

This generates a `dist/` folder containing:

- `django_open_mneti-<version>.tar.gz` — source distribution
- `django_open_mneti-<version>-py3-none-any.whl` — built distribution

## 6. Upload to PyPI

```bash
twine upload dist/*
```

- **Username:** `__token__`
- **Password:** your PyPI API token

Once uploaded, the package is available at:
`https://pypi.org/project/django-open-mneti/`

And installable via:

```bash
pip install django-open-mneti
```

## 7. Releasing a New Version

Each new release follows these steps:

1. Update `version` in `pyproject.toml` (e.g. `0.1.0` → `0.2.0`)
2. Delete the old build:
```bash
   rd /s /q dist
```
3. Rebuild:
```bash
   python -m build
```
4. Upload:
```bash
   twine upload dist/*
```
5. Commit and push the version bump to GitHub:
```bash
   git add pyproject.toml
   git commit -m "Release v0.2.0"
   git push
```

## Notes

- Versioning is **static** — there is no automated version bumping; update `pyproject.toml` manually before every release.
- License: **Apache License 2.0**

