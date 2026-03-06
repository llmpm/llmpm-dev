# Changelog

All notable changes to llmpm are documented in this file.

Format: [Keep a Changelog](https://keepachangelog.com) · Versioning: [SemVer](https://semver.org)

## [2.2.1] - 2026-03-06

### Documentation

- Replace videos with CDN served gifs in Readme.ms

## [2.2.0] - 2026-03-06

### Added

- **Chat UI redesign** — pixel/retro-terminal aesthetic with orange accent (`#FF6A00`), Space Grotesk body font, Press Start 2P logo, JetBrains Mono for code; new CSS variable design system with dark, light, matrix, and amber themes
- **Conversation history sidebar** — left rail lists all past conversations newest-first; click any entry to resume it; delete individual conversations with the × button; collapsible via the hamburger toggle in the header
- **IndexedDB persistence** — conversations are stored in `llmpm-chat` IndexedDB; history survives page refresh and browser restart
- **Auto-title** — conversation title is set from the first 55 characters of the opening user message
- **Image attachments for text-generation models** — the paperclip button now appears for text-generation (instead of image-generation); attached images are forwarded to the LLM as OpenAI-style vision content parts (`image_url`)
- Subtle pixel-grid background across the chat UI

### Fixed

- Arrow-key navigation broken in Windows CMD/PowerShell for `install`, `run`, and `serve` model pickers — all three now bypass `questionary` entirely on Windows and fall back to the reliable numbered `click.prompt` list

### Documentation

- Added demo video to Quick Start section
- Added demo video to `llmpm run` section
- Added demo video to `llmpm serve` section
- Documented response shapes for chat completions (non-streaming and streaming SSE) and image generation in the `llmpm serve` API reference

## [2.1.3] - 2026-03-02

### Added

- Fix Picker selection in windows

## [2.1.1] - 2026-03-02

### Added

- Fix loading spinner for variant env setup for windows environment

## [2.1.0] - 2026-03-02

### Added

- **Mandatory isolated environment** — all `llmpm` commands now always run inside `~/.llmpm/venv`, eliminating conflicts between system Python installs (Homebrew, conda, editable installs, etc.)
- **Automatic venv bootstrap** — on first invocation, the managed venv is created and `llmpm` (+ all backends) is installed into it transparently; subsequent runs are instant
- **Verbose setup output** — environment creation prints a clear 3-step progress log (create venv → upgrade pip → install llmpm) with full pip output so nothing is hidden
- **Dev-checkout detection** — when running from a source tree (`pyproject.toml` present), the venv gets an editable (`pip install -e .`) install of the local code automatically
- **npm postinstall creates the venv** — `npm install -g llmpm` now creates `~/.llmpm/venv` and installs all backends in 4 explicit steps instead of installing into system Python
- **npm shim prefers venv Python** — `bin/llmpm.js` checks `~/.llmpm/venv/bin/python` first, avoiding the re-exec overhead on every call after the first
- `LLPM_NO_VENV=1` env var to bypass venv isolation (CI, Docker, containers)

### Changed

- All ML backends (`torch`, `transformers`, `accelerate`, `diffusers`, `llama-cpp-python`, `Pillow`, `numpy`, `soundfile`) are now included in the default `pip install llmpm` — no need for `pip install llmpm[all]`
- Optional extras (`[gguf]`, `[transformers]`, `[diffusion]`, `[vision]`, `[audio]`) remain for selective installs via `pip install llmpm --no-deps` + the desired extra
- `[all]` extra removed (it is now equivalent to the default install)
- Namespace-package collision fix in `cli.py` pins `llmpm.__path__` before any submodule imports so that editable + system installs can coexist without `"unknown location"` errors

## [2.0.2] - 2026-03-01

### Added

- Fix npm install

## [2.0.1] - 2026-03-01

### Added

- Add SEO key keywords for better package visibility

## [2.0.0] - 2026-03-01

### Added

- `llmpm init` — creates `llmpm.json` in the current directory (npm-style project manifest)
- `llmpm install` (no args, inside a project) — installs all models listed in `llmpm.json` dependencies, mirroring `npm install`
- `llmpm install <repo> --save` — installs a model and records it in `llmpm.json` dependencies
- Local mode: when `llmpm.json` is found in CWD or a parent, all commands use `.llmpm/` next to the manifest instead of `~/.llmpm/`

## [1.2.0] - 2026-03-01

### Added

- `llmpm serve model-a model-b` — serve multiple models on a **single** HTTP server
- `llmpm serve` (no args) — automatically serves every installed model
- Terminal start-up banner lists all models being loaded before any weights are pulled into memory
- `/v1/models` returns all loaded models with full registry metadata: `category`, `backend`, `path`, `files`, `size_bytes`, `installed_at`, `tags`
- Request-body `model` field on all POST endpoints routes to the correct backend
- Chat UI model list is now fetched live from `/v1/models` on mount (seeded from injected config to avoid flash)
- Chat UI model dropdown in header (`<select>`) when multiple models are loaded; switching model preserves conversation history
- `connected · <model>` system message updates dynamically when the active model is switched
- Chat UI header metadata strip: category badge, backend badge, and tag pills with icons shown to the right of the model selector — updates on model switch
- Assistant message prefix now shows `ai(<model_name>) ›` on its own line above the response
- Copy button on every completed assistant text response; flashes "copied" for 1.5 s on click
- `{{MODELS_JSON}}` placeholder injected into the chat UI config for multi-model awareness

## [1.1.0] - 2026-02-28

### Added

- Add support for fuzzy matching of model names with run & serve commands
- Update docs (pip & npm readme)

## [1.0.0] - 2026-02-28

### Added

- `llmpm install` — download models from HuggingFace Hub with interactive quantisation picker
- `llmpm run` — interactive chat or single-prompt inference
- `llmpm serve` — OpenAI-compatible HTTP API server (`/v1/chat/completions`, `/v1/images/generations`, `/v1/audio/*`)
- `llmpm push` — upload models to HuggingFace Hub
- `llmpm list`, `llmpm info`, `llmpm uninstall` — local model management
- GGUF backend via llama-cpp-python
- Transformers backend for text-generation, vision, ASR, TTS, and diffusion models
- Built-in chat UI (React + Vite) served at `/static/` with dark/light/matrix/amber themes
- npm wrapper — `npm install -g llmpm` installs the CLI globally
- Fuzzy model name matching for `run` and `serve` commands
- Local registry at `~/.llmpm/registry.json`
