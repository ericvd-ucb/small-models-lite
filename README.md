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
- [content/01_finding_models.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/01_finding_models.ipynb): focused notebook on browsing WebLLM models, reading model names, checking model cards, and managing the browser cache.
- [content/02_talking_to_models.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/02_talking_to_models.ipynb): explains the OpenAI-style chat format, multi-turn conversations, response JSON, and the main generation settings.
- [content/03_numbers_all_the_way_down.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/03_numbers_all_the_way_down.ipynb): browser-friendly version of the tokens notebook, focusing on token counts, chunking, and embeddings as numbers.
- [content/04_memory.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/04_memory.ipynb): shows that models only remember the messages you send, then builds browser-only memory with SQLite and optional embedding search.
- [content/05_sat_test_taker.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/05_sat_test_taker.ipynb): tests a small browser model on SAT-style multiple-choice questions and scores the results.
- [content/webllm_starter.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/webllm_starter.ipynb): original starter notebook, kept temporarily as the broad sampler while the notebook series is being built.

### Broad sampler

- [content/webllm_starter.ipynb](/Users/ericvandusen/Documents/GitHub/small-models-lite/content/webllm_starter.ipynb): sampler notebook that still mixes together the major ideas before they were split into the numbered series.

## Development notes

- The site is built with JupyterLite and deployed to GitHub Pages.
- Notebook code targets the Python (Pyodide) kernel.
- WebLLM is loaded from a CDN at runtime, so models run in the browser instead of on a server.
