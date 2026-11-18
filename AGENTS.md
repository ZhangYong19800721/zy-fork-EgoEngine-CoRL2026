# Repository Guidelines

## Project Structure & Module Organization

`egoengine/` contains the CLI, visual adapters, and dataset export; `spider/` contains simulation, optimization, and RL code. Experiment runners and configs live in `action/examples/`. The retained visual replay, retargeting, and compositor code is under `visual/`. Prepared cases and robot configs are in `examples/prepared/` and `examples/robots/`; shared validation and reproduction scripts are in `scripts/`. Tests are grouped in `tests/`, `action/tests/`, and `visual/tests/`. Generated artifacts belong under `outputs/`, which is git-ignored.

## Build, Test, and Development Commands

Use the Python version and dependency profile for the code being changed: visual/export uses Python 3.10 and `requirements/visual-tested.txt`; action optimization uses Python 3.12 on Linux with NVIDIA CUDA and `requirements/action-tested.txt`.

```bash
python -m pip install --no-deps -e .       # install visual/base package
python -m pip install -e '.[dev]'           # include Ruff
egoengine validate                          # validate prepared inputs
python -m unittest discover -s tests -v      # core tests
bash scripts/test_all.sh                    # core, action, and visual tests
```

For a local visual run, use `egoengine demo --case mustard_1`. Set `MUJOCO_GL=egl` for headless Linux rendering. `scripts/reproduce_cpu.sh` runs broader validation and regenerates the included Aria examples.

## Coding Style & Naming Conventions

Use four spaces for Python indentation and follow the existing module patterns. Ruff is the configured linter, with a 100-character line limit (`ruff check .`). Use `snake_case` for Python modules, functions, and variables; tests use `test_*.py` files and `test_*` methods. Keep configuration and prepared data formats consistent with their neighboring examples.

## Testing Guidelines

Tests use the standard-library `unittest` framework; there is no configured coverage threshold. Run the focused discovery command for the affected area, then `bash scripts/test_all.sh` when the relevant optional dependencies are installed. Visual changes should also be checked with a prepared case and generated media.

## Commit & Pull Request Guidelines

Recent commits use concise, imperative sentence-case subjects (for example, “Preserve original scene lighting…”); no Conventional Commit prefixes are used. Pull requests should explain user-visible behavior, list validation commands and results, link related issues when applicable, and include before/after media for rendering changes. Keep generated outputs and local environment files out of commits.

## Data and Configuration Changes

Preserve prepared reference trajectories and their provenance when changing loaders, simulation, or rendering. Document new input fields and update the relevant example configs or manifests. Do not silently pair visual frames and action states with different reference-frame ordering; see `docs/INPUTS.md` for the input contract.
