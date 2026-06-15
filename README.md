# OpenClaude — Portable AI Coding Agent

> **Run a full-featured AI coding agent from a USB drive or any folder — no installation required.**  
> Plug in. Launch. Code. Take it anywhere.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey.svg)]()


**🎥 Watch the Setup & Demo Video:** [https://youtu.be/9Dh3kKWFFjg](https://youtu.be/9Dh3kKWFFjg)

[![OpenClaude Portable Demo](https://img.youtube.com/vi/9Dh3kKWFFjg/maxresdefault.jpg)](https://youtu.be/9Dh3kKWFFjg)

---

## What Is This?

**OpenClaude Multi-Platform** is a fully portable AI coding agent powered by the open-source [OpenClaude](https://github.com/gitlawb/openclaude) engine. It bundles a self-contained Node.js runtime, a smart system-prompt proxy for local models, and a web-based dashboard — all configurable from a single `START.bat` (Windows) or `start.sh` (Linux/macOS).

Everything runs strictly inside the project folder. No files are written to the host machine.

---

## Key Features

| Feature | Details |
|---|---|
| **7 AI Providers** | NVIDIA NIM · OpenRouter · Google Gemini · Anthropic Claude · OpenAI · Ollama (offline) · LM Studio (LM Link) |
| **Zero Footprint** | All data, keys, and logs stay inside `data/` — nothing touches the host system |
| **Cyber Analyst Mode** | Dedicated security-analyst persona with offline forensics tools (DFIR, malware triage, IoC sweep, entropy analysis) |
| **Air-Gap / Offline First** | After first setup, the drive never phones home on launch — safe to plug into untrusted machines |
| **Local Speed Proxy** | Trims system prompts for CPU inference; auto-disabled on high-VRAM GPUs (total VRAM ≥ 16 GB across all GPUs) |
| **GPU-Accelerated Local AI** | Ollama launched with `OLLAMA_NUM_GPU=999` and full CUDA enabled — all available GPUs share the load; supports 32B-class models on 24 GB VRAM |
| **LM Studio / LM Link** | Connects directly to LM Studio's LM Link local server (`localhost:1234`) — no proxy needed, full GPU acceleration |
| **Auto-Update Cache** | Checks for engine updates once per day (skipped entirely after `OFFLINE_READY` flag is written) |
| **Session Resume** | Resume any interrupted session with `RESUME.bat <session-id>` |
| **Web Dashboard** | ChatGPT-style browser UI with agent mode, tool cards, and thinking visualisation |
| **Limitless Mode** | Optional full-autonomy mode — the agent runs without asking for approval |
| **Cross-Platform** | Shared `data/` folder works across Windows, Linux, and macOS |

---

## Quick Start

### Windows
```
.\START.bat
```
On first run it automatically downloads Node.js (~25 MB) and the OpenClaude engine (~5 MB), then walks you through provider selection. Every subsequent launch skips setup and goes straight to the menu.

### Linux / macOS
```bash
chmod +x start.sh
./start.sh
```

> **First-time setup requires internet.** After that, only API calls need a connection (or none at all if you use Ollama offline mode).

---

## Project Structure

```
OpenClaude-Multi-Platform/
│
├── START.bat                  Windows entry point — handles everything
├── start.sh                   Linux/macOS entry point
├── RESUME.bat                 Resume a previous session by ID (Windows)
│
├── prompts/                   System prompt templates
│   └── cyber_analyst.txt      Cybersecurity Analyst persona (loaded by Cyber Analyst Mode)
│
├── data/                      All persistent data (shared across platforms)
│   ├── ai_settings.env        Active provider, model, API key, and OFFLINE_READY flag
│   ├── openclaude/            Session history and agent memory
│   ├── ollama/                Local Ollama binary and model storage
│   ├── work/cyber/            Cyber Analyst Mode working directory + CLAUDE.md
│   └── proxy.log              Speed proxy activity log (silent background)
│
├── engine/                    Node.js runtime + OpenClaude npm package
│   ├── node-win-x64/          Bundled Node.js (Windows)
│   └── node_modules/
│       └── @gitlawb/openclaude/
│
├── tools/                     Helper scripts
│   ├── local-proxy.js         System-prompt trimming proxy for local models
│   ├── setup_local_models.ps1 Ollama model downloader (Windows)
│   ├── setup_local_models.sh  Ollama model downloader (Linux/macOS)
│   ├── Change_Provider.bat    Switch AI provider or API key (Windows)
│   ├── change_provider.sh     Switch AI provider or API key (Linux/macOS)
│   ├── Open_Dashboard.bat     Launch web dashboard (Windows)
│   ├── open_dashboard.sh      Launch web dashboard (Linux/macOS)
│   └── Setup_Local_Models.bat Wrapper launcher for local model setup
│
└── dashboard/                 Web dashboard UI
    ├── server.mjs             Dashboard Node.js server
    └── index.html             Chat interface
```

---

## Main Menu Options

When you run `START.bat`, you are presented with:

```
1) Launch AI        — Normal Mode      (asks before writing files or running commands)
2) Cyber Analyst    — Security Mode    (DFIR, malware triage, IoC hunting — air-gap safe)
3) Limitless Mode   — Auto-executes    (fully autonomous, no approval prompts)
4) Open Dashboard   — Web UI at http://localhost:3000
5) Change Provider  — Switch model or API key
6) Setup Offline    — Download local Ollama models
7) Check for Updates — Manually fetch the latest engine version
```

The menu auto-selects **Normal Mode** after 10 seconds if no key is pressed.

### Cyber Analyst Mode

Select option **2** to launch as a Senior Cybersecurity Analyst. This mode:

- Loads the analyst persona from `prompts/cyber_analyst.txt` as a `CLAUDE.md` file in `data/work/cyber/`
- Activates DFIR, malware analysis, network forensics, and IoC-hunting capabilities
- Enforces air-gap discipline — no external calls, minimal footprint, evidence preservation
- Makes the following dashboard tools available: `hash_file`, `check_entropy`, `grep_iocs`, `read_binary_strings`, `parse_log`

**Structured output format:** every finding is reported with Severity, Confidence, Indicator, Evidence, MITRE ATT&CK TTP, and Recommended Action.

---

## Air-Gap / Offline Use

After your first provider setup, the drive writes an `OFFLINE_READY=1` flag to `data/ai_settings.env`. On every subsequent launch:

- **No update checks happen automatically** — zero outbound connections on startup
- To manually check for engine updates, choose option **7 — Check for Updates** from the menu
- The flag is set automatically when you run setup; you can also add `OFFLINE_READY=1` to `data/ai_settings.env` manually

This makes the drive safe to plug into **air-gapped or untrusted networks** without the risk of any network traffic being generated on launch.

---

| Provider | Cost | API Key |
|---|---|---|
| **NVIDIA NIM** | Free tier (1 000 credits/month) | [build.nvidia.com](https://build.nvidia.com) |
| **OpenRouter** | Free + paid models | [openrouter.ai](https://openrouter.ai) |
| **Google Gemini** | Free tier available | [aistudio.google.com](https://aistudio.google.com) |
| **Anthropic Claude** | Paid | [console.anthropic.com](https://console.anthropic.com) |
| **OpenAI** | Paid | [platform.openai.com](https://platform.openai.com) |
| **Ollama** | Free, fully offline | [ollama.com](https://ollama.com) |
| **LM Studio (LM Link)** | Free, fully offline | [lmstudio.ai](https://lmstudio.ai) |

---

## Local Model Performance (Ollama / LM Studio)

Running a local model on CPU or USB 2.0 is inherently slower than a cloud API. The built-in **speed proxy** (`tools/local-proxy.js`) intercepts every request and trims the OpenClaude system prompt from ~10 000 tokens down to ~300 tokens before it reaches Ollama.

**Typical result:** first-token latency drops from 60–120 s to 5–20 s on CPU-only hardware.

On launch, the proxy detects GPU VRAM via `nvidia-smi` and **sums the total across all GPUs** to support full-CUDA multi-GPU load splitting. If the combined VRAM reaches 16 GB or more, prompt trimming is automatically disabled — the full system prompt is passed through.

Ollama is started with `OLLAMA_NUM_GPU=999` and without any `CUDA_VISIBLE_DEVICES` restriction, so all available NVIDIA GPUs participate in inference and split the model load evenly. This applies whether you have one high-VRAM card or multiple smaller cards.

**LM Studio (LM Link):** Select provider **7 — LM Studio** to connect directly to the LM Link local API server at `http://localhost:1234/v1`. LM Studio manages its own GPU scheduling, so the speed proxy is bypassed entirely.

Proxy activity is logged silently to `data/proxy.log` — it never writes to the terminal.

### CPU-tier models (≤8 GB VRAM or CPU-only)

| Model | Size | Speed |
|---|---|---|
| `gemma3:1b` | ~800 MB | Fastest |
| `qwen2.5:1.5b` | ~1 GB | Fast |
| `phi3:mini` | ~2.3 GB | Moderate |

### Standard models (8–16 GB VRAM or fast CPU)

| # | Model | Size | Best for |
|---|---|---|---|
| 1 | Gemma 4 E2B Q4_K_M | ~3.1 GB | Balanced speed/quality |
| 2 | Gemma 4 E2B Q6_K | ~4.5 GB | Stronger reasoning |
| 3 | Gemma 4 E4B Q4_K_M | ~5.0 GB | Most users |
| 4 | Qwen 3.5 9B | ~6.6 GB | Multimodal |
| 5 | Ministral 3 8B | ~6.0 GB | Daily driver |
| 6 | Qwen3 8B | ~5.2 GB | Fast, latest generation |
| 7 | Qwen3 14B | ~9.3 GB | High quality |

### High-VRAM GPU tier (16 GB+ VRAM — full CUDA multi-GPU splitting)

| # | Model | Size | Best for |
|---|---|---|---|
| 8 | `qwen2.5-coder:32b` | ~19 GB | Code generation |
| 9 | `deepseek-r1:32b` | ~19 GB | Deep reasoning |
| 10 | `qwen2.5:32b` | ~19 GB | General purpose |

> For best performance, copy `data/ollama/` to your local SSD if USB 2.0 read speeds are the bottleneck.

---

## Security & Privacy

- **Zero Footprint** — `XDG_CONFIG_HOME`, `XDG_DATA_HOME`, and `CLAUDE_CONFIG_DIR` are all redirected to `data/`, keeping the host system clean.
- **No Telemetry** — Nothing is sent anywhere except your chosen AI provider.
- **API Key Safety** — Keys are stored only in `data/ai_settings.env` on your drive.
- **Approval Mode** — In Normal Mode the agent asks before any file write or shell command.
- **Offline First** — After first setup, zero outbound connections are made on launch (`OFFLINE_READY` flag). Safe to use on air-gapped or untrusted networks.
- **CORS Hardened** — The dashboard server only accepts requests from `http://localhost:3000`.
- **Path Sandbox** — File read/write tools are restricted to the `data/work/` directory; path traversal attempts are blocked.
- **No eval()** — Shell scripts use Bash arrays instead of `eval` for model selection to prevent injection.

---

## System Requirements

| Platform | Requirement |
|---|---|
| **Windows** | Windows 10 or later — Node.js is bundled, nothing else needed |
| **Linux** | `curl` (pre-installed on most distros) |
| **macOS** | `curl` (pre-installed) |

**Disk space:** ~150 MB for Node.js + engine. Local Ollama models require additional space (800 MB–8 GB depending on model).

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `Node.js not found` | Run `START.bat` first — it downloads Node automatically |
| `EADDRINUSE: port 11435` | The speed proxy from a previous session is still running. Restart `START.bat` — it kills it automatically |
| `'D_ARGS' is not recognized` | Old version of START.bat with nested if-blocks. Pull the latest version |
| Ollama response is very slow | Use a smaller model (`gemma3:1b`), or copy models to a local SSD |
| API key rejected | Verify your key at the provider's website; re-run option 4 to update it |
| Port 3000 already in use | The dashboard is already running — open `http://localhost:3000` directly |
| `openclaude` not found in PowerShell | Use `.\RESUME.bat <session-id>` instead of calling `openclaude` directly |
| LM Studio connection refused | Open LM Studio, load a model, and enable the LM Link server (Developer tab → Start Server) |

---

## License

MIT — use it, fork it, ship it.
