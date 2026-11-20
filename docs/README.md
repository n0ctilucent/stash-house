# Project Stash House Documentation

> **Stash House V2:** Runtime Credential Containment and Identity Federation across the Materialization Boundary.

Welcome to the documentation suite for **Project Stash House**. The project is organized into two distinct research and engineering tracks tailored for specific grant vehicles and deployment scales.

---

## 1. Documentation Map

| Document | Target Program | Focus Area | Status |
| :--- | :--- | :--- | :--- |
| **[HLD-TRACK1-GOOGLE-AI-PSS.md](file:///home/franklin/Documents/research/stash-house/docs/HLD-TRACK1-GOOGLE-AI-PSS.md)** | **Google AI for Privacy, Safety & Security (PSS) Award** ($100K) | **Agentic Runtime Credential Containment & Perception Membrane (`stash-exec`):** In-memory secret injection, PTY stream interception, and real-time Shannon entropy output scrubbing for autonomous LLM agents. | **Ready for Review** |
| **[HLD-TRACK2-NSF-SATC-FEDERATION.md](file:///home/franklin/Documents/research/stash-house/docs/HLD-TRACK2-NSF-SATC-FEDERATION.md)** | **NSF Secure and Trustworthy Cyberspace (SaTC)** / **GCP Credits** ($5,000.00) | **Enterprise Identity Federation & Cloud-Native Boundary:** Bridging campus Kerberos/LDAP roots to GKE Autopilot, Cloud HA VPN, Secret Manager, and CURC Alpine HPC with non-repudiable audit trails. | **Ready for Review** |
| **[archive/v1-legacy/](file:///home/franklin/Documents/research/stash-house/docs/archive/v1-legacy)** | Historical Archives (2025–2026) | Original human-developer `pass` tutorials, early LaTeX paper drafts, and raw exploratory notes. | **Archived** |

---

## 2. Track Comparison & Decision Matrix

To evaluate which track to pursue first, review the structural comparisons below:

| Dimension | **Track 1: Google AI PSS Award** | **Track 2: NSF SaTC / GCP Infrastructure** |
| :--- | :--- | :--- |
| **Core Problem** | Autonomous agents leaking credentials into prompt context windows, vector memories, and scratchpads. | Lateral privilege escalation and broken authority chains across hybrid campus-cloud networks. |
| **Primary Technology** | `stash-exec` subshell envelope + Shannon entropy stream redactor. | Kerberos-to-OIDC bridge daemon + Cloud HA VPN + GKE Workload Identity. |
| **Funding Target** | **$100,000** unrestricted corporate research gift. | **$600K–$1.2M** federal NSF grant + **$5,000.00** GCP cloud credits. |
| **Proposal Length** | Short (2–3 page narrative + 1-page budget). | Full federal dossier (15-page project description + data management plan). |
| **Review Window** | Fast turnaround (annual/semi-annual Google Research cycles). | Formal 6–9 month federal peer-review panel cycle. |
| **Institutional Route** | Corporate research gift routing with ~6% university fee via Stefan Reiss (RDO). | Full OGC sponsored project routing requiring PI eligibility waiver or Co-PI partner. |
| **Implementation Complexity** | **Low to Moderate:** Standalone Go binary, OpenClaw plugin hook, and benchmark evaluation suite. | **High:** Complex infrastructure orchestration (IPsec VPN, Kerberos KDC, Kubernetes cluster). |

---

## 3. Core Principles Shared Across Both Tracks

Both tracks adhere to the **Zero-Agent-Read Principle**:

1. **Storage at Rest:** All credentials remain encrypted in POSIX keyrings (`pass` / GPG). The agent never possesses direct decryption privileges.
2. **Ephemeral Materialization:** Authority is conferred strictly at the moment of execution in volatile process memory, never persisted to disk or passed via CLI flags.
3. **Continuous Auditing:** Token exchanges and process invocations emit cryptographically signed, non-repudiable audit records.

---

## 4. Historical Archive (`v1-legacy`)

The [archive/v1-legacy](file:///home/franklin/Documents/research/stash-house/docs/archive/v1-legacy) directory preserves foundational assets from the 2025 project inception:

- `abstract.md` & `stash.md`: Initial local credential hygiene guides for human developers.
- `keyringer.md`: Keyringer repository reference.
- `stash.drawio`: Original visual architecture diagram.
- `stash.txt`: Strategic analysis formulating the V2 agentic pivot and Google grant evaluation.
- `stash1.txt`: Deployment and web publishing runbook for `bitsmasher.net`.
