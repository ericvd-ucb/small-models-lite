# Small Models in the Browser

This repository hosts a JupyterLite site for teaching with small language models. Every model runs entirely in the student's browser using WebLLM, so there are no servers, no API keys, and no local installs.

## Live site

Try it here: <https://ericvd-ucb.github.io/small-models-lite/>

## What students need

- A recent Chrome or Edge browser with WebGPU support
- A laptop or desktop computer with enough memory for small models
- Patience for the first model download, since models are cached in the browser after that

## What this repo contains

- [content/](/Users/ericvandusen/Documents/GitHub/small-models-lite/content): notebooks and files copied into the JupyterLite site
- [repl/jupyter-lite.json](/Users/ericvandusen/Documents/GitHub/small-models-lite/repl/jupyter-lite.json): JupyterLite site configuration
- [.github/workflows/deploy.yml](/Users/ericvandusen/Documents/GitHub/small-models-lite/.github/workflows/deploy.yml): GitHub Pages build and deploy workflow

Edits made inside the live JupyterLite site stay in that browser unless they are copied back into this repository. The repository is the master copy.

## Notebook guide

### Current notebooks

- [content/00_webllm_starter.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/00_webllm_starter.ipynb): numbered copy of the starter notebook so the series sorts in teaching order.
- [content/webllm_starter.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/webllm_starter.ipynb): original starter notebook, kept temporarily as the broad sampler while the notebook series is being built.

### Planned notebook series

- `00_webllm_starter.ipynb`: renamed starter notebook that stays as the broad sampler.
- `01_finding_models.ipynb`: how to browse models, read model names, inspect model cards, and manage the browser cache.
- `02_talking_to_models.ipynb`: the OpenAI-style chat format, prompt settings, multi-turn chat, and response JSON.
- `03_numbers_all_the_way_down.ipynb`: tokens and tokenization, adapted for browser-based WebLLM models.
- `04_memory.ipynb`: what a model remembers, plus browser-only chat memory stored in SQLite.
- `05_sat_test_taker.ipynb`: optional experiment on how well a small browser model handles SAT-style questions.

## Development notes

- The site is built with JupyterLite and deployed to GitHub Pages.
- Notebook code targets the Python (Pyodide) kernel.
- WebLLM is loaded from a CDN at runtime, so models run in the browser instead of on a server.
