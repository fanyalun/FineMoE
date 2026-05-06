# Repository Guidelines

## Project Structure & Module Organization
`finemoe/` contains the Python package, including model adapters, runtime logic, memory management, and distributed helpers. `core/` holds lower-level C++/CUDA-facing components for memory, prefetch, tracing, and parallel execution. `op_builder/` defines native extension builders. Use `demo/` for runnable examples and artifacts: configs live in `demo/configs/`, generated CSVs in `demo/results/`, figures in `demo/figures/`, and sample inputs in `demo/states/`.

## Build, Test, and Development Commands
Install dependencies with `pip install -r requirements.txt`. Use `./setup.sh` when you need the repository’s guided environment and Hugging Face token flow from the README. Build the package locally with `python -m build`; build precompiled ops with `BUILD_OPS=1 python -m build` or `BUILD_OPS=1 python setup.py build_ext --inplace` when iterating on native code. Run the demo flow from the repo root with `python demo/prepare_data.py`, `python demo/process_data.py`, and `python demo/eval.py`. Generate figures with `python demo/plot_entropy.py`.

## Coding Style & Naming Conventions
Follow existing Python style: 4-space indentation, module names in `lower_snake_case`, and concise single-line comments only where logic is non-obvious. Keep runtime-facing APIs explicit and avoid broad refactors. For native code under `core/`, use the repository’s `.clang-format` settings (`IndentWidth: 4`, `ColumnLimit: 100`) before submitting changes.

## Testing Guidelines
This repository currently relies on build verification and demo execution rather than a dedicated `tests/` suite. At minimum, validate Python syntax with `python -m py_compile <file>` for touched modules and rebuild native ops if you changed `core/` or `op_builder/`. For behavior changes, rerun the smallest relevant demo step and note which output files under `demo/results/` or `demo/figures/` changed.

## Commit & Pull Request Guidelines
Git history currently starts with a single `Init commit`, so there is no mature in-repo convention yet. Prefer short, imperative commit subjects such as `runtime: disable tracer in online mode` or `demo: document build-only validation`. PRs should describe the user-visible effect, list touched paths, note required rebuild steps, and include representative logs or output file paths when behavior changes.

## Security & Configuration Tips
Do not commit Hugging Face tokens, local cache paths, generated `.so` binaries, or large demo outputs. Keep environment-specific settings in local shells or ignored files, and mention any required GPU/CUDA assumptions in the PR description.
