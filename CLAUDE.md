# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What is NemoClaw

NemoClaw is an NVIDIA open-source stack for running OpenClaw always-on assistants inside sandboxed OpenShell environments with NVIDIA cloud inference. It has two main components: a TypeScript CLI plugin (`nemoclaw/`) and a Python blueprint runner (`nemoclaw-blueprint/`).

## Build & Development Commands

```bash
# Initial setup
npm install                                      # root deps (OpenClaw + CLI entry point)
cd nemoclaw && npm install && npm run build       # TypeScript plugin
cd nemoclaw-blueprint && uv sync                  # Python blueprint deps

# TypeScript plugin (in nemoclaw/)
npm run build          # compile once
npm run dev            # watch mode

# Linting & formatting (from repo root)
make check             # run all linters (TS + Python)
make format            # auto-format all source

# Tests
npm test               # root-level integration tests (node --test test/*.test.js)
cd nemoclaw && npm test      # plugin unit tests (Vitest)
cd nemoclaw && npm run test:watch  # Vitest watch mode

# Documentation
make docs              # build Sphinx/MyST docs
make docs-live         # serve with auto-rebuild
```

## Architecture

The repo is a monorepo with two packages sharing a single git root:

- **`nemoclaw/`** — TypeScript CLI plugin (Commander-based). Compiles with `tsc` to `nemoclaw/dist/`. Entry point: `bin/nemoclaw.js` → `nemoclaw/dist/index.js`. CLI commands live in `nemoclaw/src/commands/` (onboard, launch, connect, status, logs, migrate, eject). The onboard wizard is in `nemoclaw/src/onboard/`. Registers as an OpenClaw plugin via `openclaw.plugin.json`.

- **`nemoclaw-blueprint/`** — Python package (requires Python 3.11+, managed by `uv`). Contains `blueprint.yaml` (declarative sandbox spec), `orchestrator/runner.py` (applies the blueprint through OpenShell), `policies/` (network/filesystem security policies), and `migrations/` (versioned schema migrations). Linted with `ruff`.

- **`test/`** — Root-level integration tests run with Node.js built-in test runner (`node --test`). Tests cover CLI behavior, install preflight, registry, policies, NIM inference, and service environment.

- **`scripts/`** — Shell scripts for installation, OpenShell setup, Telegram bridge, Brev deployment, and inference testing.

## Conventions

- **Commits**: Conventional Commits format required — `feat(cli):`, `fix(blueprint):`, `docs:`, etc.
- **TypeScript**: ESLint + Prettier. Check with `cd nemoclaw && npm run check`.
- **Python**: Ruff for linting and formatting. Line length 100. Check with `cd nemoclaw-blueprint && make check`.
- **Node.js**: Requires Node 20+. Python: Requires 3.11+.
