# CUA-Agent-Debugger

CUA-Agent-Debugger is a minimal open-source release of the debugger code used for analyzing computer-use agent trajectories. It focuses on root-cause analysis, taxonomy tagging, annotation review, and lightweight evaluation for OSWorld-style trajectory logs.

This repository does not vendor OSWorld runtime code, VM providers, third-party agent implementations, private experiment logs, paper drafts, or large rerollout artifacts. Bring your own trajectories, or start with the tiny synthetic sample under `sample_data/`.

## What Is Included

- `debugger/`: RCA pipeline, trajectory loading, taxonomy, evaluation, memory helpers, and annotation UI.
- `debugger/vis/debugger_app.py`: Streamlit app for inspecting RCA outputs and human annotations.
- `tests/`: focused unit tests for the minimal release.
- `sample_data/`: one synthetic trajectory plus matching RCA and human annotation fixtures.

## What Is Not Included

- OSWorld runtime and VM management code.
- Third-party agent runner implementations.
- Full paper experiment trajectories and results.
- Private API keys, private base URLs, or local config files.

## Quickstart

```bash
python -m venv .venv
. .venv/Scripts/activate  # Windows PowerShell users can run: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m pytest tests -q
```

Run the sample accuracy check:

```bash
python -c "from debugger.eval import quick_acc; print(quick_acc('sample_data/debugger_results/sample/gpt-4o-mini'))"
```

Launch the annotation UI:

```bash
streamlit run debugger/vis/debugger_app.py
```

## Running RCA

Copy the example config and edit paths/model settings:

```bash
cp debugger/config/debugger.example.json debugger/config/debugger.json
```

Set API keys through environment variables only:

```bash
export OPENAI_API_KEY=...
```

Then run:

```bash
python -m debugger --trajectory-dir sample_data/trajectories --output-dir sample_data/debugger_results --trial-name sample --provider openai --model gpt-4o-mini
```

The sample data is synthetic and meant for smoke tests. Real RCA runs require real trajectories with `traj.jsonl` or `trajectory.jsonl` files.

## Repository Policy

Do not commit `.env`, `debugger/config/debugger.json`, full `results/`, or large logs. Use external artifacts such as Hugging Face Datasets or GitHub Releases for full paper data.
