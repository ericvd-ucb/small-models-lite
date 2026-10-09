# Small Models in the Browser: project brief

This repo is a JupyterLite site (made from the `jupyterlite/demo` template, deployed to GitHub Pages) that hosts a series of teaching notebooks about small language models. Every model runs **entirely in the student's browser** using WebLLM. No servers, no API keys, no installs.

The owner is Eric, who teaches data science at UC Berkeley. He is not a command-line person: explain each step briefly in plain language, and say clearly when you need him to do something.

## Extra context from Eric's past work

Use Eric's past course materials in [ds-modules/Small_Models_SP26](https://github.com/ds-modules/Small_Models_SP26/tree/main) as background context when planning or adapting notebooks for this repo, especially for the token and SAT notebooks.

## Audience and style

Students are beginners. Every notebook should follow the style of `content/webllm_starter.ipynb`:

- Numbered steps (Step 1, Step 2, ...) with sub-steps (7a, 7b, ...) when an idea needs breaking down.
- Small code cells that each do one thing, with a short markdown cell before each one saying what it does and why.
- Plain language. Define any new term (token, temperature, embedding) the first time it appears.
- Settings students are meant to change go in CAPITALS at the top of a cell (`MAX_SIZE_MB = 4000`, `CHOICE = 1`).
- Show the raw thing first, then the shortcut (for example, build a request by hand before wrapping it in `ask()`).
- Friendly messages instead of tracebacks for predictable mistakes (for example, "No model is loaded yet. Run Step 6.").
- Writing style: no emdashes, no horizontal rules, no emojis in headers.

## Technical setup

- Kernel: **Python (Pyodide)**. Top-level `await` works in cells.
- WebLLM is loaded from a CDN: `webllm = await js.eval("import('https://esm.run/@mlc-ai/web-llm')")`
- Python dicts must be converted before passing to JavaScript: `to_js(d, dict_converter=js.Object.fromEntries)` (wrapped as `js_obj()`).
- Progress callbacks need `pyodide.ffi.create_proxy`.
- Requires WebGPU (recent Chrome or Edge on a laptop). Each notebook should check for it at the start.
- Models are cached in the browser after the first download. `webllm.hasModelInCache(id)` checks; `webllm.deleteModelAllInfoInCache(id)` removes.

### No hidden helper modules

Every notebook is **self-contained**: all code a student runs is visible in the notebook itself. Do not move code into a separate `.py` file that notebooks import, because students cannot see or learn from code hidden in another file.

- Each notebook has its own setup step near the top (loading WebLLM, `js_obj`, the progress bar, loading a model, the window-size retry). Keep it as short as possible and explain what each part does in plain language.
- Only include the setup a notebook actually needs. For example, a notebook that does not download models does not need the download code.
- Repeating setup code across notebooks is fine and expected. When a fix is needed in shared setup code, apply it to **every** notebook that contains it, and tell Eric which notebooks changed.

### Problems already found and fixed

- **`WindowSizeConfigurationError`** on some models: retry loading with chat options `{"sliding_window_size": -1}`. Already handled in `start_model()`.
- **Repetition loops**: small models can repeat one line hundreds of times, which also makes answers seem very slow. Always send `max_tokens` (around 300) and `frequency_penalty` (around 0.5).
- **Thinking models** (Qwen3, Qwen3.5, DeepSeek R1 Distill, Ministral 3 Reasoning) emit `<think>` blocks and are slow. Avoid them for plain chat. `/no_think` at the end of a prompt turns thinking off for Qwen3 but output quality drops.
- **Release dates** are not in WebLLM's model list, so the starter keeps a hand-made `RELEASED` table of model families. Keep it visible in `01_finding_models.ipynb` too, and explain that students can add new families to it.
- Edits made on the live site are saved only in that browser. The repo is the master copy.

### Model recommendations

For plain chat explanations: **Gemma 3 1B** (`gemma3-1b-it-q4f16_1-MLC`) as the default, **Llama 3.2 1B Instruct** as the most predictable, **Qwen2.5 0.5B Instruct** for showing mistakes, **SmolLM2** (135M, 360M, 1.7B) and **OLMo 2 1B** as fully open models. Show only `q4f16` variants to students.

## Tasks

### 1. Rewrite the README

Replace the template README with one that describes this project (what it is, that everything runs in the browser, which browsers work, the live site link) and lists the notebooks with one line each. Update it as notebooks are added.

### 2. Remove the template's default content

Delete the example notebooks and files that came with the `jupyterlite/demo` template from `content/`. Before deleting anything, list what you plan to remove and confirm with Eric. Do not touch the build configuration (`.github/workflows`, `requirements.txt`, `jupyter-lite.json` and similar) unless needed.

### 3. Build the notebook series

Keep `content/webllm_starter.ipynb` as a sampler of everything. Then build these, numbered so they sort in order:

**00_webllm_starter.ipynb (rename the existing starter)**

**01_finding_models.ipynb: What models are there, and how do I get one?**
The model-browsing parts of the starter, expanded: list available models with the date, size and type filters; explain model names (family, size in parameters, `q4f16` compression, `Instruct`); look up a model's **model card** on Hugging Face (link pattern for the original model, plus the `mlc-ai` converted version) and what to look for in it (who made it, training data, license, intended use, limitations); download, check the cache, delete. Point to WebLLM's `src/config.ts` and the WebLLM Chat demo as other places to browse.

**02_talking_to_models.ipynb: The OpenAI-style chat format**
WebLLM uses the same request and response format as OpenAI's Chat Completions API, which most AI tools now share. Cover: the `messages` list and the three roles (`system`, `user`, `assistant`); multi-turn conversations by appending to the list; the main settings (`temperature`, `top_p`, `max_tokens`, `frequency_penalty`, `presence_penalty`, `seed`, `stop`) with a small experiment for each; then the JSON that comes back, field by field (`choices`, `message.content`, `finish_reason`, `usage` with token counts and speed). End by noting the same code shape works with cloud models.

**03_numbers_all_the_way_down.ipynb: Tokens**
An adaptation of Eric's existing "numbers all the way down" token notebook. **Ask Eric for the original notebook before starting.** Adapt it to run in the browser with WebLLM models.

**04_memory.ipynb: A model that remembers you**
Answer the question "does the model know me and remember our last chats?" Show first that the model itself remembers nothing between requests (it only sees the `messages` you send). Then build memory: save each conversation to a small database in the browser (Python's built-in `sqlite3` works in Pyodide; save the file in the JupyterLite file system so it persists in that browser), and on each new chat, look up past exchanges and add the relevant ones to the messages. Start with "include the last few chats," then optionally find relevant past chats by meaning using an embedding model (WebLLM has `snowflake-arctic-embed` models). Be clear that the memory lives only in this browser.

**05_sat_test_taker.ipynb (maybe): Can a small model pass a test?**
An adaptation of Eric's existing SAT test taker notebook. **Ask Eric for the original notebook before starting.** Small browser models will score poorly, which is itself a good discussion point.

## Working agreements

- Build one notebook at a time and let Eric try it on the live site before moving on.
- After each change: commit with a clear message, push to `main`, wait for the GitHub Actions build, and tell Eric when the site has updated.
- Code cannot be fully tested outside a browser with WebGPU. Check syntax locally, then ask Eric to run the notebook and paste back any errors.
