---
title: Installing Python on macOS
date: 2026-07-15 10:00:00 -0700
categories: [Tutorial, Python]
tags: [python, macos, setup]
---

macOS ships with a system version of Python, but you shouldn't use it for development — it's outdated, tied to the OS, and modifying it can break system tools. Here's the recommended way to install and manage Python on macOS.

## Option 1: Homebrew (recommended)

If you don't have [Homebrew](https://brew.sh) installed yet:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then install Python:

```bash
brew install python
```

Verify the installation:

```bash
python3 --version
pip3 --version
```

Homebrew keeps Python up to date independently of macOS updates, and `brew upgrade python` will bump it going forward.

## Option 2: pyenv (best for managing multiple versions)

If you need to switch between Python versions across projects, [pyenv](https://github.com/pyenv/pyenv) is a better fit.

```bash
brew install pyenv
```

Add this to your shell config (`~/.zshrc` for the default macOS shell):

```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

Restart your terminal, then install a version:

```bash
pyenv install 3.12.4
pyenv global 3.12.4
```

Check it took effect:

```bash
python --version
```

## Option 3: uv (fastest, modern all-in-one)

[uv](https://github.com/astral-sh/uv) is a newer tool from Astral that handles Python version management, virtual environments, and package installation all in one, and it's significantly faster than pip.

Install uv:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Or via Homebrew:

```bash
brew install uv
```

Install a Python version with it:

```bash
uv python install 3.12
```

uv can also create and manage virtual environments per project (see below) and resolve/install dependencies much faster than `pip`. If you're starting a new project today, this is worth trying first.

## Option 4: Official installer

You can also download the installer directly from [python.org/downloads](https://www.python.org/downloads/) and run through the standard macOS `.pkg` install wizard. This works fine, but you lose the easy upgrade/version-switching workflow that Homebrew or pyenv give you.

## A note on virtual environments

Whichever method you choose, avoid installing packages globally. Use a virtual environment per project:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Or, if you installed uv, it handles this in one step:

```bash
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

This keeps project dependencies isolated and avoids version conflicts down the line.

## Summary

- Quick and simple: Homebrew
- Need multiple Python versions: pyenv
- Fastest, modern all-in-one: uv
- Prefer a GUI installer: python.org

For most day-to-day development on macOS, Homebrew is the easiest starting point.
