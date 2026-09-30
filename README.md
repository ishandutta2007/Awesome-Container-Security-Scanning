# Awesome-Container-Security-Scanning

# Top Container Security Scanning Platforms Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**
*Focused on Image Vulnerability Scanning, SBOM Generation, Registry Enforcement & Runtime Context*
**Last updated: September 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Container Security Scanning**. These tools help platform and security teams detect vulnerabilities in container images, generate Software Bills of Materials (SBOMs), enforce registry policies, and correlate image findings with runtime context.

**Examples** include Trivy, Anchore, Docker Scout, Snyk Container, Prisma Cloud, Aqua Security, Sysdig Secure, Qualys Container Security, NeuVector, and Tenable Cloud Security (the category leaders).

**Open-source emphasis**: Container security scanning is one of the **most mature open-source categories** in cloud-native security. **Trivy** (Aqua Security) and **Grype** (Anchore) dominate detection, with **Syft** for SBOM generation. **Clair** serves as the registry-embedded engine. **Jacked** and **Drydock** add lightweight, focused alternatives. The detection layer is essentially solved by open source — the commercial value lies in prioritization, workflow integration, and runtime context .

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents

- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[Snyk Container](https://snyk.io/product/container-vulnerability-management/)**
  Developer-first container scanning integrated into IDEs, pull requests, and CI/CD pipelines. Differentiator is **actionable remediation**: base image upgrade recommendations (e.g., `node:16` → `node:16-alpine`) that turn scan results into concrete fixes. Also scans Kubernetes manifests for configuration issues like containers running as root or missing resource limits . SOC 2 and ISO 27001 certified . Best for developer-led teams wanting guided fixes where they already work.

- **[Docker Scout](https://docs.docker.com/scout/)**
  Docker's native scanner, built into Docker Desktop and the CLI with zero setup for Docker-centric teams. Uses PURL-based matching across advisory sources (NVD, GitHub/GitLab advisories, distro trackers) and provides **base-image recommendations** suggesting less-vulnerable tags . Constraint: full functionality requires a Docker subscription; free Personal plan caps continuous analysis at one repository .

- **[Aqua Security](https://www.aquasec.com/)**
  Comprehensive CNAPP covering scanning, runtime, and compliance from a single vendor. The **Aqua Enforcer** runtime sensor uses userspace + kernel-module hybrid architecture. MalwareScan engine detects trojanized base images and crypto-miners. SOC 2 Type II certified.

- **[Sysdig Secure](https://sysdig.com/)**
  CNAPP built by the creators of Falco. Provides **runtime security and visibility** for container and Kubernetes workloads with eBPF instrumentation, syscall-level forensics, and managed Falco rule tuning . Gartner Peer Insights: 4.9 stars across 91 reviews. Strong detection quality with low false positives, though alerts require careful filtering . Users note lack of native multi-team/multi-environment synchronization requires custom tooling .

- **[Palo Alto Prisma Cloud](https://www.paloaltonetworks.com/prisma/cloud)**
  Comprehensive CNAPP with continuous image scanning, Kubernetes CIS Benchmark checks, and deeper cluster awareness including CRI-O runtime mapping. Enterprise-focused.

- **[Qualys Container Security](https://www.qualys.com/)**
  Container scanning within Qualys' broader cloud security platform. Provides vulnerability detection, compliance monitoring, and integration with CI/CD pipelines.

- **[NeuVector](https://www.suse.com/)**
  Full lifecycle container security platform (now SUSE Security). Provides runtime detection with process/file/network behavioral learning. **Caution**: Recent CVEs include CVE-2025-46808 (sensitive information in log files, versions before 5.4.5)  and CVE-2026-78427 (admission webhook bypass via hardcoded sidecar image paths) . CVE-2025-54469 (command injection in enforcer, patched in 5.4.7) . **Evaluate security posture carefully before deployment.**

- **[Tenable Cloud Security](https://www.tenable.com/)**
  Cloud security platform with container scanning capabilities. Provides vulnerability detection and compliance across container images and Kubernetes clusters.

- **[Anchore Enterprise](https://anchore.com/)**
  Commercial platform built on the open-source **Syft** (SBOM generation) and **Grype** (vulnerability scanning) engines. Adds policy management, reporting, and integrations aimed at larger organizations with compliance obligations. Enterprise tier provides policy-as-code gates, audit-ready reports, and centralized management. Smaller teams rarely need it; regulated enterprises often do .

## Open-Source GitHub Projects

### Image Scanners & SBOM

- **[Trivy](https://github.com/aquasecurity/trivy)**
  **The default open-source container scanner and the most widely adopted tool in the cloud-native ecosystem.** One fast binary scans **container images, filesystems, IaC, and Kubernetes manifests**, generates SBOMs in CycloneDX and SPDX formats, and detects secrets. Consumes **distribution security advisories** (Debian, Ubuntu, RHEL, Alpine, etc.) to filter OS-package findings against what each distro has actually patched, avoiding NVD false positives . Integrates with effectively every CI system. Tradeoff: the vulnerability database is large and requires regular updates; air-gapped environments need database mirroring . **Apache 2.0**. Best for teams wanting broad coverage with zero licensing friction .

- **[Grype](https://github.com/anchore/grype)**
  **Best for local, privacy-conscious, SBOM-first scanning.** Pairs with **Syft** (same project) for SBOM generation. Design philosophy is **separation of concerns**: Syft catalogs what's in the image, Grype matches that inventory against vulnerability data. The SBOM-first workflow is its strongest feature — generate the SBOM once at build time, store it, and **re-scan that SBOM against updated vulnerability data** without pulling the image again. Valuable when an image has been promoted through several environments and you want to re-evaluate risk cheaply . Supports **EPSS, KEV, and risk scoring** for threat prioritization, plus **OpenVEX** for filtering results . **Apache 2.0**. Narrower than Trivy by design — pair with additional tools for IaC/secret scanning .

- **[Syft](https://github.com/anchore/syft)**
  **The SBOM generator that Grype pairs with.** In head-to-head testing against 60 container images across Node, Python, Java, Go, and Erlang/Elixir ecosystems, Syft was the **most complete at 96% of expected components**, ahead of Trivy (94%) and Docker Scout (87%). Strong on transitive dependency depth, particularly for Java and Go images . Generates SBOMs in **SPDX, CycloneDX, and Syft JSON** formats. **Apache 2.0**.

- **[Jacked](https://github.com/carbonetes/jacked)**
  **Lightweight, all-in-one CLI scanner** for Docker images, code repositories, and tarballs. Uses **CycloneDX internally** as the SBOM format for processing and analysis. Supports **severity thresholds** (`--fail-criteria high`) to fail CI pipelines, **SBOM analysis**, and multiple output formats (table, JSON, SPDX-JSON, SPDX-XML, SPDX-tag, snapshot-json). Install via Curl, Homebrew, or Scoop. **Go-based** . Best for teams wanting a simple, focused CLI with CI-friendly exit codes.

- **[Clair](https://github.com/quay/clair)**
  **The registry-embedded scanning engine.** API-driven, open-source engine that performs static, **layer-by-layer analysis**. Frequently embedded into container registries to scan images **as they are pushed**, rather than requiring a separate CI job. Takes more effort to stand up and operate than a single binary, but once running it **updates its vulnerability data continuously** and serves results over an API. Best for **platform teams building scanning directly into a registry workflow** . **Apache 2.0**.

### Registry-Specific & Cloud-Native Tools

- **[Drydock](https://github.com/hiro-o918/drydock)**
  **Lightweight CLI to audit container vulnerabilities in Google Cloud Artifact Registry.** Fetches vulnerability data directly from **Google Cloud's Container Analysis API**, allowing you to filter out noise and focus on High/Critical threats across repositories . Features: severity filtering (`-s CRITICAL`), **fixable-only mode** (`--fixable`), CSV/JSON/TSV output, concurrency control, and inference of project ID from gcloud environment. Go-based, usable as a library for custom exporters . **Best for teams standardized on Google Cloud.**

- **[Argus Security](https://github.com/huntridge-labs/argus)**
  **Unified security scanning CLI covering SAST, containers, IaC, secrets, dependencies, and DAST.** Wraps 14+ scanners including **Trivy, Grype, Syft, Gitleaks, Bandit, CodeQL, Checkov, OSV Scanner, and ClamAV** behind a single `argus scan` command . Features: severity thresholds, Docker-backed execution, **MCP server for AI integration** (Claude, Copilot, Cursor can run scans directly), and GitHub Actions integration. **AGPL-3.0**. Best for teams wanting a single entry point across multiple security domains.

### Additional Strong Open-Source Options

- **Image Scanners**: **Trivy** (all-in-one, default choice), **Grype** (SBOM-first, privacy-conscious), **Jacked** (lightweight CLI, CI-friendly), **Clair** (registry-embedded).
- **SBOM Generation**: **Syft** (96% completeness, best transitive depth for Java/Go).
- **Cloud-Specific**: **Drydock** (Google Cloud Artifact Registry).
- **Unified CLI**: **Argus Security** (14+ scanners, MCP support).

**Frameworks for building custom systems**: Combine **Trivy** for broad all-in-one scanning in CI, **Syft + Grype** for SBOM-first workflows with cheap re-scanning, **Clair** for registry-embedded scanning, and **Jacked** for lightweight CLI enforcement. Add **Argus Security** for unified multi-domain scanning with AI/MCP integration.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Container security scanners handle sensitive image and vulnerability data; ensure proper access controls and secure storage of findings.
- **Open-source reality**: The detection layer for container security is **essentially solved by open source**. **Trivy** and **Grype** are production-grade, fast, and free . **Syft** leads in SBOM completeness at 96% . **Clair** provides registry-native scanning . The **commercial value** lies in **prioritization** (VEX, reachability, exploitability), **workflow integration** (ticket routing, SLA reporting), and **runtime context** (correlating image findings with running workloads) . The honest decision rule from the field: if your finding volume is small enough that a human can read every result, stay open source; once you cross into thousands of findings across dozens of services, the cost of triage exceeds the cost of a platform . **NeuVector** users should note recent CVEs and verify patched versions (5.4.7+ for command injection, 5.4.5+ for sensitive info in logs) .

---

**Made for platform engineers, DevSecOps practitioners, container security architects, and software supply chain teams.**
Let's make container security scanning more open, transparent, and actionable.
