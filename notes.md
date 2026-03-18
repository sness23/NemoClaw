# NemoClaw Notes

## What it does

NemoClaw creates a sandboxed environment for running AI coding agents (OpenClaw) safely. It:

- Installs the NVIDIA OpenShell runtime (a secure container environment)
- Creates a sandbox with strict security policies: network egress control, filesystem isolation, process restrictions, and inference routing
- Routes all inference through NVIDIA's cloud (Nemotron 3 Super 120B) instead of Anthropic — the agent never makes direct external calls
- Provides operator oversight — unauthorized network requests get blocked and surfaced for human approval

## Cost

- The software is free and open source (Apache 2.0)
- NVIDIA cloud inference requires an API key from build.nvidia.com — check their site for pricing/free tier

## NemoClaw vs standalone OpenClaw

NemoClaw runs OpenClaw but replaces the Anthropic/Claude inference backend with NVIDIA's Nemotron model. The agent code doesn't know the difference — API calls are intercepted and rerouted transparently.

If you're just using Claude Code as a CLI for your own development, you don't need NemoClaw. It adds value when you want:

- **Sandboxing** — the agent can't escape its container or make unauthorized network calls
- **NVIDIA inference** instead of Anthropic
- **Operator oversight** — approve/deny the agent's external requests

NemoClaw is most relevant for running always-on, autonomous agents in environments where you need guardrails (e.g. production access), not for typical interactive CLI usage.
