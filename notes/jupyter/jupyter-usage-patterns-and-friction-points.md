---
title: "Jupyter usage patterns and friction points"
date: 2026-05-18
captured: 2026-05-18T01:39:06.468Z
tags: ["jupyter", "workflow", "data-science"]
source: "Claude.ai chat"
---
Jupyter spans wildly different workflows. The fast way to understand the ecosystem is to look at it by persona, because the workflow, friction points, and "what they reach for next" are different for each.

![Jupyter personas and their workflows](https://assets.han-ws.workers.dev/i/2026/05/jupyter-personas.svg)

## Personas and workflows

**ML researcher.** Trains models, evaluates, plots learning curves. Heavy GPU cells. Uses remote kernels on H100 boxes. Pain points: kernel state inconsistency, OOM crashes mid-training. Next moves: Colab, Vast.ai for cheap GPU.

**Data scientist.** EDA, modeling, classic pandas + sklearn workflow. Eventually has to convert notebook to production script. Pain: the prod handoff is always messy. Next moves: Hex, Deepnote for collaboration-heavy work.

**Quant.** Backtests, signal research, lots of plots. Fast iteration cycle. Pain: kernel restart cost when loading large historical datasets. Next moves: Marimo, refactor into proper Python modules.

**Data analyst.** SQL plus light Python. Reports, dashboards, recurring scheduled runs. Pain: sharing results with non-technical business users. Next moves: Hex, Mode, Metabase for the polished "internal data product" path.

**Educator.** Course materials with step-by-step examples. Inline math, plots, narrative. Pain: getting 30 students to install the right Python environment. Next moves: Colab (free GPU, no install), Binder (throwaway env from any GitHub repo), JupyterHub (self-hosted for a class).

**Scientific researcher.** Experiment notebooks, scipy/numpy heavy, plots. Wants reproducible analysis tied to a paper. Pain: reproducibility and sharing with collaborators on different systems. Next moves: Quarto (publication pipeline), Pluto.jl (reactive notebooks for Julia).

## The shared workflow loop

People don't write notebooks top-to-bottom. The real pattern is: load data in one cell, then mess around. You write a cell, run it, look at the output, scroll up, edit cell 4, run it, scroll back, write cell 12, realize cell 6 needs updating, run it. By hour two you have 40 cells in execution order [3, 5, 4, 7, 6, 12, 9, 11, ...] and the kernel state no longer matches reading the notebook top-to-bottom.

**This is the original sin of Jupyter.** Every persona hits it. The mature-team patterns below exist to work around it.

## Mature team patterns

**1. Notebook for exploration, script for production.**

Use `jupyter nbconvert --to script analysis.ipynb` to get a `.py`, then refactor into modules. Notebook is the lab; `src/` is the codebase. Some teams use `jupytext` to keep `.ipynb` and `.py` in sync automatically so the notebook itself becomes version-controllable.

**2. Parameterized runs with papermill.**

Write one notebook, execute it N times with different parameters from a CI job or scheduler. Each run produces a dated `.ipynb` artifact with embedded outputs. This is the standard way to turn an exploration notebook into a recurring report.

**3. Scheduled notebooks.**

Run nightly via cron/Airflow/Prefect. Output goes to Slack, email, or S3 as HTML via `nbconvert --to html`. This is most of the "internal data report" world.

**4. Restart-and-run-all as a discipline.**

Before committing, run `Kernel → Restart & Run All` and verify the notebook executes cleanly. If it doesn't, the notebook is lying about its state. Most teams enforce this in PR review.

> A notebook that runs top-to-bottom after "Restart & Run All" is the only notebook that works.

## Friction points (why people leave Jupyter)

**Hidden state.** Cells out of order producing fictional results. Studies suggest over 75% of Jupyter notebooks on GitHub don't run, and 96% don't reproduce. Directional truth even if the exact numbers are debated.

**Git diffs are unreadable.** The `.ipynb` is JSON with embedded base64 images and execution counts. A meaningful one-line code change produces a 500-line diff. Code review on notebooks via standard git tools is brutal.

**No real package management per notebook.** "Works on my machine" is the default state. Reproducing a colleague's notebook usually means manually figuring out which packages and versions they had.

**Sharing is awkward.** Sending a teammate a `.ipynb` requires them to have the right kernel, the right packages, and the right data paths.

**Long-running cells block the UI.** Want to launch a 6-hour training run? You're tying up a browser tab. Most teams build separate scripts or use jobs systems for anything longer than a coffee break.

## Key takeaway

Notebooks are fantastic for exploration and terrible for production. The mature pattern is: explore in notebook, refactor into modules, run via papermill or schedulers, enforce restart-and-run-all in code review. If you can't enforce that discipline, the notebook is a write-only scratchpad.