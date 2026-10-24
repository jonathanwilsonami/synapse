# SYNAPSE

SYNAPSE is an MVP "kernel" for agentic AI in cyber-physical systems. A simulated world runs in SimPy, an LLM operator agent acts on it through a single, well-defined tool interface, and every decision is traced end to end. The goal is a small, inspectable baseline for studying how agents perceive, decide, and act in a physical-style environment.

## Architecture

![SYNAPSE MVP architecture](project-site/images/design/agentic_cps_kernel_mvp.svg)

| Component | Package | Role |
|---|---|---|
| World simulator | `synapse.sim` | SimPy discrete-event simulation of the world |
| MCP server | `synapse.mcp_server` | Exposes sim tools: `move`, `pick`, `scan`, `get_state` |
| Agents | `synapse.agents` | Operator agent (LangGraph) that acts; read-only chat agent that answers questions about the system |
| Observability | `synapse.observability` | LangSmith + OpenTelemetry tracing |
| UI | `synapse.ui` | Streamlit app |

**Core rule: MCP is the only door to the world.** Agents never import or call the simulator directly. Every observation and action goes through the MCP server, so it can be traced, validated, and swapped out. The chat agent is read-only: it may only use non-mutating tools (e.g. `get_state`).

## Setup

1. Install [uv](https://docs.astral.sh/uv/getting-started/installation/):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
2. Create the virtual environment (shows up as `(synapse)` when activated):
   ```bash
   uv venv --python 3.14 --prompt synapse
   ```
3. Install dependencies:
   ```bash
   uv sync
   ```
4. Configure secrets:
   ```bash
   cp .env.example .env   # then fill in your API keys
   ```

## Activation

```bash
source .venv/bin/activate      # macOS / Linux
.venv\Scripts\activate         # Windows
```

Alternatively, prefix commands with `uv run` and skip activation.

## Running

Placeholder commands; entry points are not implemented yet.

```bash
uv run python -m synapse.mcp_server          # MCP server (wraps the simulator)
uv run python -m synapse.agents.operator     # operator agent
uv run python -m synapse.agents.chat         # read-only chat agent
uv run streamlit run src/synapse/ui/app.py   # Streamlit UI
uv run pytest                                # tests
uv run ruff check .                          # lint
```

## Repo structure

```
synapse/
├── pyproject.toml          # uv package definition
├── uv.lock
├── .env.example
├── src/synapse/
│   ├── sim/                # SimPy world simulator
│   ├── mcp_server/         # MCP server exposing sim tools
│   ├── agents/             # operator agent + read-only chat agent
│   ├── observability/      # LangSmith / OpenTelemetry setup
│   └── ui/                 # Streamlit app
├── tests/
├── project-site/           # Quarto project site and paper (see project-site/README.md)
└── .github/workflows/      # publishes project-site/docs/ to GitHub Pages
```
