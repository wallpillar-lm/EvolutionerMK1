# Evolutioner MK1 🧠⚡

> **Open-Source Evolutionary Search Harness, Verification Engine, and Tiered Reasoning Framework — running wallpillar-lm models through Ollama on plain CPU.**

Evolutioner MK1 scales small-footprint language models (≤ 5B parameters) into
high-capacity reasoning engines. By shifting the workload from pure parametric
memory to **Test-Time Compute (TTC) scaling** (MCTS), **deterministic rule-based
verifiers (RLVR)**, and **external stateful memory**, it enables edge-class
hardware to execute complex, step-by-step reasoning with verified output on
computable tasks.

---

## ⚡ Quickstart (one line)

```bash
curl -fsSL https://raw.githubusercontent.com/wallpiller-lm/EvolutionerMK1/main/get.sh | bash
```

Downloads the repo, creates the venv, installs deps, detects/recreates your
Ollama models, builds the integrated model, and runs the selftest.
Idempotent — safe to re-run to update.

Then:

```bash
bash launch.sh        # menu 1–9: TUI · query · agent · status · usage ·
                      # selftest · tests · ollama controls · chat REPL
```

Chat REPL (opencode-style, streaming, persistent history):

```bash
PYTHONPATH=src env/bin/python -m evolutioner.cli chat
#  /model 5B        switch tier      /solve <q>   full MCTS + RLVR harness
#  /history /sessions /resume /new   session management
```

Headless:

```bash
PYTHONPATH=src env/bin/python -m evolutioner.cli query "What is 37*43? Verify with python."
PYTHONPATH=src env/bin/python -m evolutioner.cli agent "Inventory ./workspace and write notes.md" --tier 3B
```

---

#### The 4 Core Pillars

1. **Quantized Inference Engine** — tiered Ollama portfolio (1.5B rollouts +
   3B/5B reasoner), AVX2 CPU, no GPU required.
2. **Deterministic RLVR Verifier** — executes the model's ` ```python ` blocks
   in a sandboxed runtime (bubblewrap when available, rlimits otherwise);
   outputs/errors feed back for real-time self-correction.
3. **MCTS Search** — evaluates candidate trajectories at step boundaries,
   pruning dead paths early; backprop corrected per spec deviation notes.
4. **Virtual Context Engine** — rolling retrieval (RAG) + persistent memory to
   simulate long context within tight RAM.

---


## 🛠️ Manual Setup (if not using get.sh)

```bash
git clone https://github.com/wallpiller-lm/EvolutionerMK1.git
cd EvolutionerMK1
python -m venv env && source env/bin/activate
pip install -r requirements.txt
PYTHONPATH=src python -m evolutioner.cli status      # verify ollama + models
PYTHONPATH=src python -m evolutioner.cli selftest    # mock pipeline check
```

---


## 📁 Repository Structure

```
EvolutionerMK1/
├── get.sh                   # dedicated downloader / bootstrap
├── launch.sh                # unified launcher (menu 1–9)
├── Modelfile                # integrated-model build (methodology baked in)
├── config/default.json      # search + runtime configuration
├── src/evolutioner/
│   ├── cli.py               # query / chat / agent / status / usage / selftest
│   ├── harness.py           # orchestration: memory → MCTS → answer → verify
│   ├── search.py            # MCTS implementation
│   ├── ollama_backend.py    # tiered backend + tag resolver
│   ├── verifier.py          # RLVR sandbox (bwrap / rlimit)
│   ├── agents.py            # tool-using agents + sub-agent spawning
│   ├── context.py           # virtual context (RAG)
│   ├── memory.py            # persistent memory store
│   ├── usage.py             # SQLite token/call tracker
│   ├── router.py            # API fallback router (optional)
│   ├── engine.py            # native llama.cpp backend (GGUF direct)
│   └── tui_app.py           # cyberpunk TUI dashboard
├── tests/                   # unit + TUI tests (headless)
├── docs/DISTILL.md          # phase-2 LoRA distillation pipeline
└── data/                    # sessions, memories, usage DB (git-ignored)
```

---

## 🗺️ Roadmap

- [ ] **Native Ternary / BitNet CPU Acceleration** — 1.58-bit kernels for 100+ tok/s on AVX2.
- [ ] **ToT Visualizer** — real-time MCTS branch/verifier telemetry (TUI exists; deepen it).
- [ ] **Multi-Modal Verifier Sandboxing** — RLVR checks beyond math/code (vision-tier integration via `EvoluMK1:vision-3B`).
- [ ] **Autonomous Edge Federation** — peer-to-peer MCTS across low-power devices.

---

## 📄 License

Apache 2.0 — see [LICENSE](LICENSE).
