# X_eTaL-Coder

X_eTaL-Coder is a local coding-agent training project for
[X_eTaL](https://github.com/softwarewrighter/X_eTaL), an experimental,
statically typed array language. The project brings together validated
training examples, fine-tuning, and benchmarks for an OpenCode agent
that can write idiomatic array code, discover library APIs, and repair
code using compiler, linter, and test feedback.

## Status

This repository contains the project plan, research notes, and source audit.
The plan defines how to measure the trained agent against its original
coding model and how to verify the quantized model on a 12 GB GPU.

## Documentation

- [Project plan](docs/plan.md) covers hardware, Agentrail sagas, training,
  and evaluation.
- [Repository audit](docs/repository-audit.md) records source revisions,
  available training examples, and verified tooling.
- [Experiment architecture](docs/architecture.md) describes the corpus,
  tool loop, and grading interfaces.
- [Research notes](docs/research.txt) preserve the design discussion.

## Architecture

Rust is the planned home for corpus validation, tool adapters, and
benchmarking. An isolated training backend produces model adapters, and a
local OpenAI-compatible endpoint serves the base and tuned models to
OpenCode. The benchmark runs both models against the same held-out tasks.

## Development

The project plan defines the prerequisites, step acceptance checks, and
handoff process for Agentrail-driven development.

## The X_eTaL repositories

- [X_eTaL](https://github.com/softwarewrighter/X_eTaL) provides the
  language, compiler, and command-line tools.
- [X_eTaL-libraries](https://github.com/softwarewrighter/X_eTaL-libraries)
  provides reusable libraries and API examples.
- [X_eTaL-demos](https://github.com/softwarewrighter/X_eTaL-demos)
  provides example programs.
- [X_eTaL-ML](https://github.com/softwarewrighter/X_eTaL-ML)
  explores machine learning primitives and visualizations.
- [X_eTaL-fine-tuning](https://github.com/softwarewrighter/X_eTaL-fine-tuning)
  explores model adaptation methods.

## Links

- [X_eTaL live editor](https://softwarewrighter.github.io/X_eTaL/)
- [Software Wrighter Lab](https://software-wrighter-lab.github.io/)

## Copyright

Copyright (c) 2026 Michael A Wright

## License

MIT. See [LICENSE](LICENSE) and [COPYRIGHT](COPYRIGHT).
