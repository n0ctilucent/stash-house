# Project Stash House

[![markdown](https://github.com/n0ctilucent/stash-house/actions/workflows/markdown.yml/badge.svg)](https://github.com/n0ctilucent/stash-house/actions/workflows/markdown.yml)

> **Stash House V2:** Zero-Trust Runtime Credential Containment and the Materialization Boundary for Autonomous Agents.

---

## Overview

Modern software engineering and cloud operations increasingly rely on **autonomous LLM agents** (e.g., OpenClaw, AutoGPT, Claude Code) capable of executing multi-hop shell commands, modifying infrastructure, authoring deployment scripts, and inspecting runtime state.

Traditional secret managers (HashiCorp Vault, OpenBao, 1Password) assume either interactive human operators or static microservices. When applied to autonomous agents, traditional patterns fail catastrophically:

- **Context Window Leakage:** Verbose error traces (Terraform, curl) and commands like `env` echo live tokens directly into LLM prompt context.
- **Memory & Session Sprawl:** Ephemeral credentials persist in agent daily logs (`~/.openclaw/memory/daily/`), vector memory stores, and session trajectories.
- **Scratchpad Pollution:** Agents generate temporary helper scripts with hardcoded API keys in staging folders.
- **Indirect Prompt Injection:** Adversarial input can manipulate an active agent into dumping environment variables or exfiltrating credentials to third-party endpoints.

**Project Stash House V2** introduces a zero-trust containment architecture built around the **Zero-Agent-Read Principle**: *an autonomous agent must never possess read or decrypt access to plaintext credentials.* Instead, authority is materialized strictly in-memory at execution time via a dedicated execution envelope (`stash-exec`), and all output streams are scrubbed before reaching agent perception context.

---

## The 4-Layer Containment Architecture

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   Layer 1: Encrypted Storage at Rest                   │
│   - POSIX UNIX pass / GPG Keyrings (AES-256 / Ed25519)                 │
│   - Zero direct read/decrypt authority for the LLM agent               │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ Authorized Fetch via Broker
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│             Layer 2: The Materialization Boundary (stash-exec)         │
│   - Ephemeral in-memory injection via POSIX execve                     │
│   - Short-lived downscoped token minting (AWS STS / GCP OAuth, <= 15m) │
│   - No plaintext secrets written to disk or passed as CLI arguments    │
└──────────────────┬─────────────────────────────────┬───────────────────┘
                   │ Child Process Execution         │ Output Streams
                   ▼                                 ▼
  ┌─────────────────────────────────┐ ┌──────────────────────────────────┐
  │   Layer 3: Ephemeral Worker     │ │ Layer 3: Context Scrubbing Gate  │
  │   (Terraform, Cloud CLI, curl)  │ │ - Regex signature matchers       │
  │   - Ephemeral memory execution  │ │ - Shannon entropy detection      │
  │   - Terminated on completion    │ │ - Emits [REDACTED_SECRET]        │
  └─────────────────────────────────┘ └──────────────────┬───────────────┘
                                                         │ Clean Stream
                                                         ▼
                                      ┌──────────────────────────────────┐
                                      │ Layer 4: Workspace Invariants    │
                                      │ - Agent context & prompt memory  │
                                      │ - Scratchpad linters fail writes │
                                      │ - Zero plaintext keys on disk    │
                                      └──────────────────────────────────┘
```

1. **The Enclave (Layer 1):** Encrypted storage at rest using standard POSIX `pass` / GPG. The agent never has direct decryption authority.
2. **The Execution Envelope (Layer 2):** In-memory ephemeral injection via `execve`. Master keys exchange for short-lived downscoped tokens (TTL $\le$ 15m). No CLI flag leakage.
3. **The Scrubbing Membrane (Layer 3):** Real-time `stdout`/`stderr` stream filtering combining regex heuristics and Shannon entropy thresholds to replace secrets with `[REDACTED_SECRET]` before output reaches the LLM context.
4. **Workspace Invariants (Layer 4):** Pre-execution linters and POSIX `trap` cleanup routines ensuring zero disk persistence in `/workspace/scratch/`.

---

## Quickstart & Usage Model

Instead of giving agents static API keys or credentials in `.env` files, the agent interacts solely through the `stash-exec` wrapper:

```bash
# Agent invokes target tools through stash-exec:
stash-exec --service aws -- terraform apply

# Downscoped ephemeral Python execution:
stash-exec --env-file ~/.stash/prod.env.gpg -- python3 deploy.py
```

### Stream Sanitization in Action

When an invoked tool encounters an error or returns output containing a secret, the Scrubbing Membrane catches and neutralizes it before output reaches the model's perception context:

```text
# Raw Tool Trace:
Error: 403 Forbidden - Invalid credentials for key AKIAIOSFODNN7EXAMPLE

# Sanitized Output Returned to Agent:
Error: 403 Forbidden - Invalid credentials for key [REDACTED_SECRET]
```

---

## Research & Grant Alignment

Stash House V2 provides the experimental software baseline for two active grant tracks:

- **Google AI for Privacy, Safety & Security (PSS) Award ($100K):**
  - **Focus:** Mitigating adversarial usage and preventing credential exfiltration in autonomous agent workflows.
  - **Research Thrusts:** Empirical measurement of agentic secret exposure vectors, real-time Shannon entropy stream filtering trade-offs, and mid-execution token revocation resilience.
- **NSF Secure and Trustworthy Cyberspace (SaTC):**
  - **Focus:** Enterprise identity federation and the runtime Materialization Boundary between on-premises key infrastructure and cloud-native workloads.

---

## Repository & Documentation Structure

```text
stash-house/
├── README.md               # Project overview and architectural summary
├── bin/                    # Installation scripts and execution tools
├── demo/                   # Scenarios and scanner validation suites
├── docs/                   # Active documentation suite
│   ├── HLD-TRACK1-GOOGLE-AI-PSS.md       # Track 1 HLD (Google AI PSS Award)
│   ├── HLD-TRACK2-NSF-SATC-FEDERATION.md # Track 2 HLD (NSF SaTC / GCP Bridge)
│   ├── HLD.md              # Unified High-Level Design (v2.0)
│   ├── README.md           # Documentation map and decision matrix
│   └── archive/v1-legacy/  # Historical 2025 developer pass tutorials and notes
└── paper/                  # Research paper drafts and formal LaTeX source
```

- **[docs/HLD-TRACK1-GOOGLE-AI-PSS.md](file:///home/franklin/Documents/research/stash-house/docs/HLD-TRACK1-GOOGLE-AI-PSS.md):** Track 1 High-Level Design (Google AI PSS Award).
- **[docs/HLD-TRACK2-NSF-SATC-FEDERATION.md](file:///home/franklin/Documents/research/stash-house/docs/HLD-TRACK2-NSF-SATC-FEDERATION.md):** Track 2 High-Level Design (NSF SaTC / GCP Infrastructure).
- **[docs/README.md](file:///home/franklin/Documents/research/stash-house/docs/README.md):** Documentation index and comparative decision matrix.

---

## Authors & Citation

- **Franklin Diaz** (`franklin@bitsmasher.net` / `franklin.diaz@ucdenver.edu`)
- **Institution:** University of Colorado Denver / Bitsmasher Labs
- **Repository:** [https://github.com/n0ctilucent/stash-house](https://github.com/n0ctilucent/stash-house)
