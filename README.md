# Secure SDLC Risk & Control Self-Assessment (RCSA) Template

An automated, data-driven **Risk & Control Self-Assessment (RCSA)** framework aligned to the **Kosli Secure SDLC model**. This template separates risks from controls, mapping them via a central matrix where residual risk profiles are automatically derived from the evaluated effectiveness of underlying engineering and lifecycle controls.

Live Interactive Dashboard: [https://hlaingminnpaing.github.io/sdlc-rcsa/#dash](https://hlaingminnpaing.github.io/sdlc-rcsa/#dash)

---

## 📋 Overview & Core Architecture

The assessment workspace maps structural development risks against defensive checkpoints across four logical stages of a secure Software Delivery Lifecycle (SDLC):

```
                          ┌──────────────────────────┐
                          │    Secure SDLC Model     │
                          └─────────────┬────────────┘
                                        │
         ┌──────────────────┬───────────┴───────────┬──────────────────┐
         ▼                  ▼                       ▼                  ▼
   [ Build Stage ]  [ Release Stage ]       [ Runtime Stage ]  [ Lifecycle Stage ]
   • Version Ctrl   • Code Reviews          • Change Records   • Role Training
   • Build Guard    • SAST/SCA/Img          • Gate Controls    • Threat Modeling
   • SBOM/Firewalls • API/Fuzz Testing      • Drift/Workload   • GenAI Guardrails
```

### 1. The 12 Core SDLC Risks
*   **RISK-0001: Supply Chain Compromise** – Malicious or tampered components (open-source libraries, base images, build tools, third-party services) enter the SDLC, resulting in compromised applications reaching production.
*   **RISK-0002: Insider Threat** – Authorised personnel (or attackers using their credentials) misuse legitimate access to repositories, environments or production to compromise confidentiality, integrity or availability.
*   **RISK-0003: Unauthorised Deployment** – Software is deployed to production without passing required quality gates, security scans or approval checks.
*   **RISK-0004: Credential and Secret Exposure** – Leaked or poorly managed secrets enable unauthorised access to production systems, customer data or pipelines.
*   **RISK-0005: Vulnerable Software in Production** – Known vulnerabilities in code or dependencies are exploited in the production environment.
*   **RISK-0006: Audit and Compliance Failure** – Inability to evidence controls to regulators, auditors or enterprise customers, resulting in reputational or commercial damage.
*   **RISK-0007: Unauthorised System Access** – Production environments are accessed by personnel without appropriate authorisation or full audit trails.
*   **RISK-0008: Configuration Drift** – Infrastructure or application configuration diverges from its known, approved state, causing undetected changes in security posture or behaviour.
*   **RISK-0009: Environment Breach** – An external attacker runs unauthorised workloads within the production environment.
*   **RISK-0010: GenAI & Code Assistant Exploitation** – Developers using unvetted public LLMs or autonomous AI coding agents introduce package hallucinations, unauthorized license liabilities, or inadvertently leak proprietary source code/secrets into public models.
*   **RISK-0011: Insecure Infrastructure as Code (IaC)** – Cloud deployment configurations, container orchestration manifest files (Terraform, Helm charts, Kubernetes YAML), or environment parameters contain systemic access-control vulnerabilities or overly permissive IAM footprints committed directly into the code.
*   **RISK-0012: Insecure Architectural Design** – Core functional flaws in application logic, business processing flows, data tenancy boundaries, or upstream API design contracts are introduced during early product specification phases, passing downstream syntactical pipeline scans undetected.

---

## 🛠️ The 29 Security Controls Grid

Controls are cataloged by their point of execution. Each control is explicitly cross-referenced inside the `Risk-Control Matrix` tab.

### Build Controls (9 Checkpoints)
*   **CTRL-0001: Version Control** – All source code and configuration is managed in version control with full, traceable history.
*   **CTRL-0002: Artifact Binary Provenance** – Build artifacts are traceable to their source commit and build; provenance is attested and verifiable.
*   **CTRL-0003: Controlled Build Environment** – Builds run in a controlled, reproducible environment using approved, hardened toolchains.
*   **CTRL-0004: Dependency Management** – Third-party and open-source dependencies are inventoried, approved and kept current; SBOM maintained.
*   **CTRL-0005: Infrastructure and Configuration as Code** – Infrastructure and configuration are defined as code, version-controlled and peer-reviewed.
*   **CTRL-0006: Secrets Scanning** – Code and artifacts are scanned to detect secrets/credentials before reaching shared repositories.
*   **CTRL-0024: Software Bill of Materials (SBOM) Generation & Attestation** – Enforce that every build automatically generates a standard SBOM (e.g., CycloneDX or SPDX format) and cryptographically signs it to prevent tampering.
*   **CTRL-0025: Dependency Firewall & Vetted Registries** – Restrict CI/CD runners from pulling packages directly from public registries without routing through a proxy that blocks malicious or unvetted packages.
*   **CTRL-0026: Infrastructure as Code (IaC) Static Analysis** – Automated pre-deployment security scanning of cloud configuration files (Terraform templates, Helm charts, Kubernetes manifests) to catch misconfigurations before deployment.

### Release Controls (8 Checkpoints)
*   **CTRL-0007: Code Review** – Changes are peer-reviewed and approved before being merged or released.
*   **CTRL-0008: Quality Assurance** – Automated and manual testing validates functionality and quality before release.
*   **CTRL-0020: Vulnerability Scanning — SAST** – Static application security testing detects code-level vulnerabilities before release.
*   **CTRL-0021: Vulnerability Scanning — SCA** – Software composition analysis detects vulnerable dependencies before release.
*   **CTRL-0022: Vulnerability Scanning — Containers** – Container images are scanned for vulnerabilities before release.
*   **CTRL-0010: Deployment Approvals** – Production deployments require defined approvals and must pass required gates.
*   **CTRL-0023: Feature Flags** – Functionality is controlled via feature flags to enable safe, reversible release.
*   **CTRL-0030: Automated API Schema & Contract Verification** – Validate live staging application network interfaces against strict OpenAPI schemas and execute automated behavioral fuzz tests to discover object-level authorization vulnerabilities.

### Runtime Controls (7 Checkpoints)
*   **CTRL-0012: Change Records** – Every production change is captured in a traceable, auditable change record.
*   **CTRL-0013: Deployment Controls** – Only verified, approved artifacts can be deployed; deployment mechanisms are controlled.
*   **CTRL-0014: Secrets Management** – Runtime secrets are stored, rotated and accessed through a managed secrets solution.
*   **CTRL-0015: System Access Controls** – Production access is least-privilege, authenticated, time-bound and logged.
*   **CTRL-0016: Runtime Workload Monitoring** – Running workloads are monitored to detect anomalous or unauthorised activity.
*   **CTRL-0018: Drift Detection** – Deviations from the approved, declared state are detected and alerted.
*   **CTRL-0027: Post-Commit Secrets Detection & Auto-Revocation** – Active background monitors continuously check code repositories for leaked secrets and automatically trigger alert/rotation sequences if an accidental merge occurs.

### Lifecycle Controls (5 Checkpoints)
*   **CTRL-0017: Training** – Personnel receive security and process training relevant to their role.
*   **CTRL-0019: Penetration Testing** – Independent penetration testing periodically identifies exploitable weaknesses.
*   **CTRL-0011: Service Ownership** – Every service has a defined owner accountable for its security and operation.
*   **CTRL-0028: Continuous Threat Modeling & Architecture Review** – For major releases or structural API changes, a formal threat-modeling exercise (e.g., STRIDE) must be documented and approved before engineering work begins.
*   **CTRL-0029: GenAI Code Assistance Guardrails** – Ensure code generated by AI assistants (GitHub Copilot, Cursor) passes identical SAST/SCA gates as human code; policies prohibit pasting sensitive customer data or credentials into external LLMs.

---

## 🗂️ Workbook Architecture & Navigation

The `SDLC_RCSA_Template.xlsx` sheet structures tasks across specialized tabs:

| Tab Name | Group Category | Purpose | Operational Input Type |
| :--- | :--- | :--- | :--- |
| **`Instructions`** | Guidance | Quick-start playbook for engineering managers. | Read-Only |
| **`Dashboard`** | Executive Reporting | Heatmap matrices and control coverage aggregations. | Auto-Generated |
| **`Risk Register`** | Threat Profiling | Active master register tracking inherent vs residual scores. | **Blue Text Fields** |
| **`Risk-Control Matrix`**| Mapping Grid | Intersecting matrix defining control-to-risk alignments. | **`X` Markers** |
| **`Build Controls`** | Phase 1 Assessment | Operational inventory for CI pipeline configuration. | **Status & Effectiveness** |
| **`Release Controls`** | Phase 2 Assessment | Operational inventory for CD pipeline gates. | **Status & Effectiveness** |
| **`Runtime Controls`** | Phase 3 Assessment | Operational inventory for infrastructure protection. | **Status & Effectiveness** |
| **`Lifecycle Controls`**| Phase 4 Assessment | Governance, training, and architectural reviews. | **Status & Effectiveness** |
| **`Scoring Legend`** | Core Engine | Heatmap thresholds, impact bounds, and math weights. | Reference / Custom |
| **`Definitions`** | Glossary | Data dictionary explaining formatting conventions. | Reference |
| **`Model Reference`** | Core Taxonomy | Master library tracking reference secure SDLC goals. | Reference |
| **`Methodology`** | System Documentation| Algorithmic approach defining automated calculations. | Reference |

---

## ⚙️ Mathematical Risk Scoring Methodology

Residual scores update programmatically based on actual measured control conditions, removing manual bias from assessments.

### 1. Inherent Risk Profiling
Assessors define baseline threat levels by evaluating worst-case exposure using a qualitative 1–5 scale:
*   **Likelihood ($L$):** `1` (Rare) to `5` (Almost Certain)
*   **Impact ($I$):** `1` (Insignificant) to `5` (Severe)
*   $$\text{Inherent Score} = \text{Inherent } L \times \text{Inherent } I \quad \text{(Bounds: 1 to 25)}$$

### 2. Control Credit Weights
Control scoring evaluates execution status to prevent inflation before verification:
*   **Effective:** `1.0` (Full mitigation credit)
*   **Partially Effective:** `0.5` (50% risk reduction weight)
*   **Ineffective / Not Tested:** `0.0` (Zero defensive value credited)

### 3. Formula-Driven Residual Risk
The sheet aggregates weights for all controls mapped to a threat inside the `Risk-Control Matrix` to find the **Average Control Effectiveness ($\text{Avg Ctrl Eff}$)**. Residual values are derived as:

$$\text{Residual } L = \text{ROUND}\left( \text{Inherent } L - (\text{Inherent } L - 1) \times \text{Avg Ctrl Eff},\, 0 \right)$$
$$\text{Residual } I = \text{ROUND}\left( \text{Inherent } I - (\text{Inherent } I - 1) \times \text{Avg Ctrl Eff},\, 0 \right)$$
$$\text{Residual Score} = \text{Residual } L \times \text{Residual } I$$

> 📊 **Engine Behavior:** If all mapped controls are fully **Effective**, residual risk reduces directly to its floor score of `1`. If controls are broken, missing, or undocumented, the residual risk retains its maximum inherent score.

---

## 🚀 Step-by-Step Operating Guide

### Step 1: Baseline Context Setup
Open the spreadsheet and fill out the metadata section on the front page: Define the **Organization / Product**, targeted **In-scope services/systems**, and the **Assessment period**.

### Step 2: Inherent Threat Assessment
1. Navigate to the **`Risk Register`** tab.
2. Review the 12 pre-loaded software supply chain risks.
3. Edit only the **blue text columns** for `Inh. L` and `Inh. I`. Evaluate the threat assuming no pipelines or firewalls are running.

### Step 3: Operational Control Grading
1. Move across the four phase trackers (**Build Controls**, **Release Controls**, **Runtime Controls**, **Lifecycle Controls**).
2. For each checkpoint item, provide operational inputs:
    *   **Implementation Status:** Select `Implemented`, `Partially Implemented`, or `Not Implemented`.
    *   **Control Effectiveness:** Grade as `Effective`, `Partially Effective`, `Ineffective`, or `Not Tested`.
    *   **Evidence / Reference:** Document specific validation proof (e.g., *Link to SonarQube profile*, *Repo branch protection settings*, *AWS KMS key rotation policies*).

### Step 4: Map Intersections
If you implement local workarounds or tailor threat coverages, map relationship links on the **`Risk-Control Matrix`** by adding an `X` to the coordinate cell. This automatically adjusts target calculations.

### Step 5: Remediate Appetite Gaps
1. Monitor the live results on your **`Dashboard`** or web viewer.
2. Any calculated residual rating evaluating to **High** or **Critical** requires active mitigation.
3. Use the **Action Plan** columns to document specific remediation blueprints, assign owners, and track target completion deadlines.

---

## 🛠️ Customization & Maintenance

*   **Adding New Risks:** Append a row to the bottom of the `Risk Register` and a corresponding row inside the `Risk-Control Matrix`. Extend formulas down for `Avg Ctrl Eff` and residual scores.
*   **Adding Tailored Checkpoints:** Insert a column inside the `Risk-Control Matrix` grid and append a tracking row into its respective lifecycle tab.
*   **Modifying Calibration:** Adjust structural weights or risk appetite rating boundaries entirely inside the `Scoring Legend` tab without breaking formulas.

***
*Disclaimer: This self-assessment framework represents a snapshot of your SDLC posture. Definitions and mappings should be verified against active cloud architecture blueprints, branch configurations, and organizational compliance mandates quarterly.*
