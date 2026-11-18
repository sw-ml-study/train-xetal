# Repository capability audit and corpus inventory

These revisions identify the sources inspected for the first Agentrail
step. They are audit inputs. The experiment must record the compatible
toolchain and library revisions it actually runs.

| Repository | Inspected commit | Corpus candidates |
| --- | --- | --- |
| [X_eTaL](https://github.com/softwarewrighter/X_eTaL) | `2f24ae212571ead113ca1f89f5570e55a7b5b83e` | 480 `spec/**/*.case` files, 258 reg-rs cases, 155 tracked `.xtl` files, and the language reference |
| [X_eTaL-libraries](https://github.com/softwarewrighter/X_eTaL-libraries) | `9132a1c4c8b3782f3a6477729f1b969dd586c0e4` | 534 tracked `.xtl` files and 729 reg-rs cases, including Matrix, Sets, Strings, Dates, Graphs, Check, and Equations |
| [X_eTaL-demos](https://github.com/softwarewrighter/X_eTaL-demos) | `e3a0fb33061e689039b2fc18e5ca63ab2a1492a2` | 25 tracked `.xtl` files, including visual and scientific examples such as cellular automata and N-body gravity |
| [X_eTaL-ML](https://github.com/softwarewrighter/X_eTaL-ML) | `1ffdc20201a3c91d4ac57af4a5a2d55d31742b9d` | 130 tracked `.xtl` files and 200 reg-rs cases, including array kernels, quantization, attention, and routing |
| [X_eTaL-fine-tuning](https://github.com/softwarewrighter/X_eTaL-fine-tuning) | `213afb1145df3176dea6b0bf141834f005b82c65` | 12 tracked `.xtl` files, including LoRA forward, gradient, rank, and LoRA-FA lessons |

All five repositories declare MIT at the inspected revisions. Counts
come from tracked filenames, include fixtures and templates, and may
overlap across repositories. Retain upstream notices and source
provenance. Review any separately licensed inputs before including
them. Preserve each repository's dependency pins when replaying its
examples. For example, the inspected ML checkout pins X_eTaL at
`abdd371dd4ae644f00e04d04e09d67576056f031` and the library
repository at `6b09aad52c22c3e28e8b0b4180c59cac4bbcfa63`.
Validate examples again with the experiment's chosen toolchain before
accepting them into a shared corpus.

## Capabilities confirmed in source

- X_eTaL implements lexing, parsing, macro expansion, type checking,
  evaluation, documentation, and linting. Its spec cases include
  dedicated rejection, ambiguity, macro, and spatial tests. Use those
  for language semantics and diagnostic tasks. Review passing examples
  for idioms before using them as preferred solutions.
- X_eTaL-libraries provides public functions and macros with tests.
  Start library discovery examples from source and generated API
  docs, including imports and signatures. Use multi-file demos for
  composition and test fixture design.
- X_eTaL-ML supplies small array implementations and visualizations
  for training, optimization, quantization, attention, and routing.
  Reuse these for reference calculations, corpus examples, and
  experiment visualization. Its MoE fine-tuning plan describes
  prospective educational implementations.
- X_eTaL-fine-tuning composes pinned library functions in runnable
  lessons with checked-in CLI and browser goldens. The LoRA training
  lesson includes a small gradient update and an Adam training run.
  LoRA-FA has a comparative lesson. QLoRA, DoRA, VeRA, and applied
  open-weight coding-model training appear on its roadmap. Use the
  existing lessons as numerical references. Qualify a separate
  full-checkpoint backend in the next step.

## CLI and language services

The inspected CLI defines `parse`, `type`, `expand`, `run`,
`doc --json`, `doc --test`, and `lint`. The available release
binary reports build `c5750cec`, distinct from the inspected source
checkout. Its help also exposes JSON-line lint findings through
`lint --format json`. A smoke check returned `3` for
`eval -e '1 + 2'` and a source-spanned `type-mismatch` diagnostic
for `type -e '1 + "x"'`. Rebuild or pin an exact binary before
recording experiment baselines.

Use `doc --json` and pinned library source as the initial symbol
and API index. Use the repository spec and regression harnesses for
upstream tests, and explicit grader commands for individual tasks.
The inspected CLI surface has no LSP or DAP command. Server releases,
registry services beyond the local documentation path, and live
debugger integrations were outside the confirmed smoke checks.

## Local environment

- A host-level `nvidia-smi` query verified a GeForce RTX 5060 Ti,
  16,311 MiB VRAM, and driver `620.70.06`. The sandbox does not
  expose the NVIDIA device files.
- `nvcc --version` reported CUDA toolkit 13.4, build `V13.4.92`.
  Training and inference kernels still need an execution check.
- The installed `opencode` launcher reports a missing postinstall
  step. Qualify a working pinned OpenCode installation and a structured
  round trip before the model screen.
