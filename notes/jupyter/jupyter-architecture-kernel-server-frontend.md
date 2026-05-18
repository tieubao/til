---
title: "Jupyter architecture: kernel, server, frontend"
date: 2026-05-18
captured: 2026-05-18T01:38:36.691Z
tags: ["jupyter", "architecture", "python"]
source: "Claude.ai chat"
---
## Overview

Jupyter is not one program. It's three processes talking to each other over a network protocol. Once you see the split, everything else about Jupyter clicks: extensions, remote kernels, language support, the difference between Notebook and Lab.

![Jupyter three-part architecture](https://assets.han-ws.workers.dev/i/2026/05/jupyter-architecture.svg)

## The three components

**Frontend (browser).** The UI you click around in. It's a JavaScript app loaded by your browser. It doesn't run any of your code.

**Jupyter Server (Python process on your machine).** Serves the frontend, manages files on disk, and starts/stops kernels. This is what `jupyter notebook` or `jupyter lab` actually launches.

**Kernel (separate process per notebook).** The thing that actually executes your code. For Python it's `ipykernel`. There are kernels for R, Julia, Rust, Go, Bash — roughly 100 languages. The kernel is language-specific. The server is language-agnostic.

## Wire protocol

- **Frontend ↔ Server:** HTTP + WebSocket
- **Server ↔ Kernel:** ZeroMQ (5 channels: shell, iopub, stdin, control, heartbeat)

This decoupling is why you can:
- Have remote kernels (kernel on a GPU box, frontend on your laptop)
- Run kernels in Docker containers
- Attach a different frontend (VS Code, PyCharm, nteract) to the same kernel

The `.ipynb` file is just JSON containing cells, outputs, and metadata.

## Cell execution flow

When you press Shift+Enter:

1. Frontend sends the cell's source code to Jupyter Server over WebSocket.
2. Server forwards it to the kernel on the **shell** ZeroMQ channel as an `execute_request` message.
3. Kernel runs the code in its persistent namespace. Variables defined in earlier cells are still there.
4. Kernel streams output (stdout, stderr, display data, errors) back on the **iopub** channel.
5. Server relays those messages to the frontend over WebSocket.
6. Frontend renders the output below the cell. Server writes the updated state to `.ipynb` on disk.

The kernel's namespace persists for the life of the kernel process. Restart the kernel and you lose everything. **This is the source of the most common Jupyter footgun: cells run out of order produce a notebook whose state doesn't match what a top-to-bottom read of the code would suggest.**

## Notebook vs Lab

People conflate these constantly. They're different products from the same project.

| Dimension | Notebook v7 | JupyterLab |
|-----------|-------------|------------|
| UI style | Single notebook per tab, simple | Multi-document tabs, splits, IDE-like |
| File browser | Minimal | Full sidebar |
| Terminals, editors | Not built-in | Built-in |
| Extension ecosystem | Smaller | Richer (prebuilt extensions) |
| Underlying code | Built on JupyterLab since 2022 | Original |
| Launch command | `jupyter notebook` | `jupyter lab` |
| Best for | Beginners, teaching, minimal UI | Real project work |

**History.** Classic Notebook was the original UI, written before single-page-app conventions. JupyterLab was the rewrite (~2018), modeled on an IDE. By 2022, the Jupyter project rebuilt Notebook v7 on top of the Lab codebase, so they now share most of the same code. Both read and write the same `.ipynb`. You can switch between them with no conversion.

There is no "Notebook is faster / lighter" argument anymore since v7 shares Lab's runtime.

## Configuration and kernels

**Installation:**

```bash
pip install jupyterlab           # gets `jupyter lab`
pip install notebook             # gets `jupyter notebook` (v7)
conda install -c conda-forge jupyterlab
uv tool install jupyterlab       # isolated install
```

**Config files** live in `~/.jupyter/`. Generate them with:

```bash
jupyter lab --generate-config    # creates ~/.jupyter/jupyter_lab_config.py
jupyter server --generate-config # creates ~/.jupyter/jupyter_server_config.py
```

The config files are Python. You assign to attributes on a `c` object:

```python
# ~/.jupyter/jupyter_server_config.py
c.ServerApp.ip = '0.0.0.0'              # listen on all interfaces (careful)
c.ServerApp.port = 8888
c.ServerApp.root_dir = '/home/han/notebooks'
c.ServerApp.open_browser = False
c.ServerApp.password = 'argon2:...'     # set with `jupyter server password`
```

**Kernels per virtualenv.** Installing JupyterLab does NOT give your venvs separate kernels. By default Lab sees one kernel: whatever Python it was installed in. To use a project's virtualenv as its own kernel, inside that venv:

```bash
pip install ipykernel
python -m ipykernel install --user --name=myproject --display-name="Python (myproject)"
```

List/remove:

```bash
jupyter kernelspec list
jupyter kernelspec remove myproject
```

For other languages: install `IRkernel` (R), `IJulia` (Julia), `gophernotes` (Go). Each registers itself the same way.

## Extension system

This is where Notebook and Lab diverge sharply, and where most online tutorials are outdated.

**Three categories, in order from old to current:**

### 1. Classic nbextensions (legacy, mostly dead)

If you Google "Jupyter extensions" you'll hit a wall of tutorials about `jupyter_contrib_nbextensions`: collapsible headings, variable inspector, table of contents. These targeted Classic Notebook v6. Most are unmaintained and broken on Notebook v7 / JupyterLab. **Don't start here.** If a tutorial tells you to run `jupyter contrib nbextension install`, the tutorial is stale.

### 2. Server extensions

Backend functionality for Jupyter Server: HTTP endpoints, websocket handlers, file format converters. Pure Python.

```bash
pip install jupyter-resource-usage    # registers itself automatically
jupyter server extension list         # verify
```

### 3. JupyterLab extensions (modern)

Plug into the Lab frontend (Notebook v7 too, since it's built on Lab). Themes, file viewers, side panels, autocomplete providers, AI assistants.

**Key shift:** modern Lab extensions are "prebuilt" (also called "federated"). You `pip install` them and they work immediately. No more `jupyter labextension install` + node.js + rebuild step. That rebuild step was a major pain point pre-Lab 3 and is the source of half the confusion in old docs.

Most useful extensions install with one pip command:

```bash
pip install jupyterlab-git              # git panel in sidebar
pip install jupyterlab-lsp python-lsp-server  # autocomplete, jump-to-def
pip install jupyterlab_code_formatter black isort
pip install jupyter-ai                  # AI chat panel
pip install jupyter-resource-usage      # RAM/CPU bar
pip install ipywidgets                  # interactive widgets
```

The **Extension Manager** in Lab's sidebar (puzzle-piece icon) is a GUI for browsing and installing.

**Combo extensions.** Many modern extensions ship a frontend piece AND a server piece in one pip package (jupyterlab-git needs a server to shell out to git, plus the UI). The pip install handles both halves.

## Key takeaway

Frontend, server, kernel are three independent processes. The kernel-server split (using ZeroMQ) is what enables remote kernels, Docker kernels, and alternate frontends. The kernel's stateful namespace combined with arbitrary cell execution order is the root cause of Jupyter's reproducibility problem.