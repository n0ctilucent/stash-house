# Project Stash House Documentation

> **Stash House V2:** Runtime Credential Containment and the Materialization Boundary for Autonomous Agents.

Welcome to the documentation suite for **Project Stash House**. This project establishes zero-trust security invariants for autonomous agentic systems (e.g., OpenClaw, LLM execution harnesses) operating in engineering environments.

---

## 1. Documentation Map

| Document | Description | Status |
| :--- | :--- | :--- |
| **[HLD.md](file:///home/franklin/Documents/research/stash-house/docs/HLD.md)** | **High-Level Design Document (v2.0):** Comprehensive architectural specification, threat modeling, 4-layer containment framework, and Google AI PSS grant strategy. | **Active / Primary** |
| **[archive/v1-legacy/](file:///home/franklin/Documents/research/stash-house/docs/archive/v1-legacy)** | Historical V1 documentation, original human-developer `pass` tutorials, early LaTeX paper drafts, and raw exploratory notes. | **Archived** |

---

## 2. Core Concepts: The V2 Pivot

Traditional credential management solutions (HashiCorp Vault, 1Password, cloud IAM) are designed for human interactive shells or static microservices. When autonomous LLM agents execute commands on a host, traditional models collapse:
<<<<<<< HEAD

- Agents echo terminal outputs and error traces directly into conversational contexts.
- Sensitive credentials persist across session logs (`~/.openclaw/memory/daily/`) and vector memory stores.
- Indirect prompt injection can hijack active agent permissions to exfiltrate keys.

Stash House V2 introduces **`stash-exec`**, enforcing the **Zero-Agent-Read Principle**:

1. **The Enclave (Layer 1):** Encrypted storage at rest using standard POSIX `pass` / GPG. The agent never has direct decryption authority.
2. **The Execution Envelope (Layer 2):** In-memory ephemeral injection via `execve`. Master keys exchange for short-lived downscoped tokens (TTL $\le$ 15m). No CLI flag leakage.
3. **The Scrubbing Membrane (Layer 3):** Real-time `stdout`/`stderr` stream filtering combining regex heuristics and Shannon entropy thresholds to replace secrets with `[REDACTED_SECRET]` before output reaches the LLM context.
4. **Workspace Invariants (Layer 4):** Pre-execution linters and POSIX `trap` cleanup routines ensuring zero disk persistence in `/workspace/scratch/`.

---

## 3. Grant & Research Alignment

Stash House V2 serves as the concrete software implementation for two primary research thrusts:

- **Google AI for Privacy, Safety & Security (PSS) Award ($100K):** Focuses on empirical modeling of agent credential exposure vectors, entropy-based output redaction, and open-source containment tooling.
- **NSF Secure and Trustworthy Cyberspace (SaTC):** Serves as the experimental software artifact demonstrating runtime identity federation and the Materialization Boundary.

=======
- Agents echo terminal outputs and error traces directly into conversational contexts.
- Sensitive credentials persist across session logs (`~/.openclaw/memory/daily/`) and vector memory stores.
- Indirect prompt injection can hijack active agent permissions to exfiltrate keys.

Stash House V2 introduces **`stash-exec`**, enforcing the **Zero-Agent-Read Principle**:
1. **The Enclave (Layer 1):** Encrypted storage at rest using standard POSIX `pass` / GPG. The agent never has direct decryption authority.
2. **The Execution Envelope (Layer 2):** In-memory ephemeral injection via `execve`. Master keys exchange for short-lived downscoped tokens (TTL $\le$ 15m). No CLI flag leakage.
3. **The Scrubbing Membrane (Layer 3):** Real-time `stdout`/`stderr` stream filtering combining regex heuristics and Shannon entropy thresholds to replace secrets with `[REDACTED_SECRET]` before output reaches the LLM context.
4. **Workspace Invariants (Layer 4):** Pre-execution linters and POSIX `trap` cleanup routines ensuring zero disk persistence in `/workspace/scratch/`.

---

## 3. Grant & Research Alignment

Stash House V2 serves as the concrete software implementation for two primary research thrusts:
- **Google AI for Privacy, Safety & Security (PSS) Award ($100K):** Focuses on empirical modeling of agent credential exposure vectors, entropy-based output redaction, and open-source containment tooling.
- **NSF Secure and Trustworthy Cyberspace (SaTC):** Serves as the experimental software artifact demonstrating runtime identity federation and the Materialization Boundary.

>>>>>>> b7617a2 (major overhaul)
---

## 4. Historical Archive (`v1-legacy`)

The [archive/v1-legacy](file:///home/franklin/Documents/research/stash-house/docs/archive/v1-legacy) directory preserves foundational assets from the 2025 project inception:
<<<<<<< HEAD

=======
>>>>>>> b7617a2 (major overhaul)
- `abstract.md` & `stash.md`: Initial local credential hygiene guides for human developers.
- `keyringer.md`: Keyringer repository reference.
- `stash.drawio`: Original visual architecture diagram.
- `stash.txt`: November 20, 2026 strategic analysis formulating the V2 agentic pivot and Google grant evaluation.
- `stash1.txt`: Deployment and web publishing runbook for `bitsmasher.net`.
