# X_eTaL-Coder training plan

Train a local OpenCode agent that writes idiomatic X_eTaL, discovers library
APIs, and uses compiler, linter, and test feedback to repair its work.
The first release milestone is a reproducible paired evaluation showing
that specialization improves verified task completion over the same
unmodified base model.

This roadmap follows the later hardware, memory, and MoE decisions in
[the research notes](research.txt). Keep the repository name
`train-xetal` for the workspace and use X_eTaL-Coder for the model project.

## Hardware and deployment

| Device | Role |
| --- | --- |
| RTX 5060-class, 16 GB | Start here with CUDA and runtime bring-up, corpus and benchmark development, baseline inference, and a small training smoke test once the baseline works. |
| RTX 3090, 24 GB | Reproduce the pilot and run the first full QLoRA experiments after the Git handoff. |
| RTX 3060, 12 GB | Validate the release inference profile on the smallest planned deployment card. Repeat on the 5060 for comparison. |
| M1 Max, 64 GB unified, and M3 Pro, 18 GB unified | Optional independent MLX, teacher-data, and validation experiments. |

Record the exact GPU SKU, reported memory, driver, and backend build on
each host. The initial local `nvidia-smi` probe could not reach the NVIDIA
driver, so the local GPU and CUDA runtime still need verification.

The primary deployment budget is at most 8 GiB for resident weights and
model metadata. On the 12 GB card, measure peak device memory across
load, prompt processing, generation, and full tool loops, including KV
cache and runtime buffers. Target 16K usable context and profile 32K if
it fits with headroom. Record context, output budget, cache precision,
concurrency, offload, and existing GPU usage. Training memory has its
own per-device budget.

## Architecture

Use Rust for corpus extraction, validation, the evaluation harness,
tool adapters, and reporting. Run the first full training experiment through
an isolated CUDA training backend, with a pinned Python ML stack behind a
CLI if needed. Keep training specifications and small algorithm checks
aligned with the educational experiments in X_eTaL-fine-tuning, and use
X_eTaL-ML for reusable primitives and experiment visualization where the
audit confirms a fit.

Serve the original and adapted model through a local OpenAI-compatible
endpoint as separate OpenCode model IDs. Use the same agent instructions,
tool definitions, and retrieval corpus for matched runs. Start with file
and search tools and verified X_eTaL CLI interfaces for documentation,
build, lint, tests, and any available macro expansion or tracing.
Normalize tool requests, diagnostics, and exit status into versioned
records. Develop LSP symbol, type, diagnostic, and code-action support
and DAP stack, variable, and stepping support in parallel through the
same tool boundary. The CLI path enables baseline evaluation during
that work.

## Agentrail roadmap

Initialize the first saga when implementation starts. Use the steps
below as independently verifiable units. Record each step's scope and
acceptance check in Agentrail, inspect `agentrail next`, and run
`agentrail begin` before working. Run the applicable checks and commit
the work with its saga metadata before `agentrail complete` so Agentrail
can record the implementation commit. Commit the resulting completion
metadata before handoff, use `--done` at the end of a saga, and stop at
the step boundary. Use `agentrail status`, `agentrail plan`, and
`agentrail history` to resume. Add or insert uncovered work through
Agentrail commands.

### Saga 1 - Audit and baseline on the RTX 5060

1. Audit X_eTaL, X_eTaL-libraries, X_eTaL-demos, X_eTaL-ML, and
   X_eTaL-fine-tuning, plus registry and language-service tooling.
   Pin inspected revisions and document working capabilities,
   educational demonstrations, planned features, reusable code, source
   licensing, and actual tool interfaces. Produce the architecture,
   capability audit, and initial corpus inventory.
2. Verify GPU access, CUDA kernels, the local inference runtime, and
   structured OpenCode tool calls. Screen a compact, coding-capable MoE
   first, using Granite 4.0 7B-A1B as the research shortlist candidate.
   Screen a 7-8B dense coder at a comparable quantized footprint as a
   control and a 3-4B dense model as a lightweight reference. Verify
   model and redistribution terms, chat template, tool reliability,
   training support, export support, and measured memory before locking
   a checkpoint. Use the dense control for the first usable model if
   the MoE cannot meet these gates.
3. Specify the benchmark, build a minimal runner, and execute a small
   held-out slice using the unmodified model. Run the same tasks with
   prompt only, documentation retrieval, and full OpenCode tools.
   Deliver a report with reproducible commands and pinned inputs.

### Saga 2 - Corpus and benchmark on the RTX 5060

1. Build a validated corpus from language specs and tests, libraries,
   demos, documentation, historical fixes, and verified synthetic tasks.
   Record source revision, license, transformation, compiler version,
   validation results, and idiom review for each record. Validate against
   the compiler, relevant tests, and linter. Cover array operations,
   shapes, macros, composition, library discovery, and minimal repairs.
   Keep structured trajectories for inspect, edit, build, diagnose, fix,
   and verify. Use successful trajectories for supervised targets and
   retain failed runs for analysis or later preference pairs.
2. Split by repository where practical and by implementation, API,
   task family, and synthetic ancestor. Deduplicate near matches and
   freeze hidden tests, rubrics, and holdout manifests before training.
   Include unseen problems, unfamiliar APIs with supplied docs, and
   unseen diagnostics. Keep solutions and hidden tests out of both
   training and retrieval. Start with a curated pilot and grow toward
   roughly 200-300 benchmark tasks as validated coverage allows.
3. Freeze the base-model baseline and perform a short QLoRA smoke test
   on the 5060. Verify a repeatable dataset build, checkpoint save and
   resume, adapter export, quantized load, and a tool-call replay.
   Save commands, versions, seeds, and memory measurements for the move.

### Saga 3 - Git handoff and full training on the RTX 3090

1. After the user sets up the remote, push the agreed branch from the
   5060 host. Clone or pull that branch on the 3090 host and verify the
   exact commit and saga state. Transfer or regenerate larger datasets,
   base checkpoints, adapters, and run logs by pinned revisions and
   checksums. Keep those large artifacts and credentials out of Git.
   Recreate the pinned environment and rerun the pilot and a baseline
   slice before committing GPU time to the full run.
2. Train the first QLoRA supervised adapter with a progression from
   language and idioms to library use and verified tool trajectories.
   Start with shared layers and a frozen router for the MoE. Test expert
   adapters only after confirming backend and export support. Record
   model and tokenizer revisions, data hashes, adapter targets, sequence
   length, hyperparameters, seeds, runtime versions, and peak VRAM.
   Select settings on validation tasks. Export and verify the exact
   quantized artifact intended for deployment.
3. Evaluate base and tuned checkpoints under all three baseline modes.
   Report the primary full-OpenCode comparison and qualify deployment
   on the RTX 3060. Publish a reproducible run manifest and paired
   report with the first adapter.

### Saga 4 - Agent and model improvements

Use failure analysis and validated trajectories to guide data-size
learning curves, repair training, and preference optimization. Evaluate
DoRA and other adapters only after the QLoRA comparison is established.
Compare dense and MoE artifacts at matched memory budgets using task
success, tool validity, and successful tasks per hour. Treat router
adaptation, expert pruning, and distillation as separate experiments.
Integrate LSP and DAP incrementally as their interfaces stabilize and
replay matched tasks when tool behavior changes.

A separate Lite track can test 4 GB and 6 GB Emacs or IDE deployments
with bounded completion, diagnostic repair, and debugger explanation
tasks. Use LSP/DAP context and measure end-to-end latency and memory on
the target hardware.

## Evaluation and release criteria

The primary score is the fraction of held-out tasks passing all required
checks. Compare the tuned and original versions of the same checkpoint
with matched quantization, context, tools, retrieval, decoding, time,
and call budgets. Use the same tasks and seeds, repeat stochastic runs,
and report the difference in percentage points with a paired bootstrap
confidence interval. Claim improvement when the paired interval supports
a positive gain. Report individual differences for retrieval-only and
prompt-only runs as well.

Track parse and compile success, hidden tests, library use, compiler
repair, lint fixes without regressions, blind idiom review, valid tool
calls, and general tool-use regressions. Record end-to-end latency,
retries, tokens, throughput, and peak VRAM. The initial release evidence
is the reproducible baseline and trained comparison, a loadable local
artifact, and a full OpenCode task run on the 12 GB inference profile.
