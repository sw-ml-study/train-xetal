# Experiment architecture

The first experiment uses a pinned X_eTaL toolchain and two local
model variants derived from one checkpoint. A shared evaluation runner
scores both variants against the same held-out tasks. The
[repository audit](repository-audit.md) identifies the starting
fixtures and verified tool interfaces.

## Data and validation

A Rust extractor reads revision-pinned worktrees. Keep spec fragments,
library examples, complete programs, and tool trajectories as distinct
record types with their source path, revision, license, dependency
revisions, and validation evidence. Replay upstream examples with
their own dependencies, then rerun against the experiment toolchain.
Group related examples by implementation, task family, API, and
synthetic ancestor before assigning splits. Build retrieval from
permitted documentation and keep hidden answers and tests outside the
agent's workspace.

## Tool loop and scoring

An OpenCode task starts in a fresh fixture workspace with file and
search tools and an explicit X_eTaL tool set. Rust adapters collect
status, structured diagnostics when available, bounded output, and
runtime for documentation, type checking, lint, and tests. Keep the
grader and hidden tests outside that tool surface. Record the model
variant, fixture revision, deterministic inputs, limits, tool calls,
and final outputs. Run the grader independently and calculate paired
task success. LSP and DAP can supply richer diagnostics and debug
context through the same tool adapters when their interfaces are
ready.

## Training and deployment

The host launcher passes a pinned run manifest to an isolated trainer.
The pilot verifies the backend's training modules, checkpoint resume,
adapter export, and quantized inference with a matching chat template.
The same local OpenAI-compatible runtime serves distinct base and
adapted model IDs for evaluation. Record separate weight and peak
runtime memory measurements. Transfer configurations and manifests
through Git and resolve large artifacts by revision and checksum on
the RTX 3090 host. The [project plan](plan.md) defines the sagas and
release criteria.
