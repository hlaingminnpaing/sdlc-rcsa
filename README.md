# SDLC RCSA — Web Edition

A single-file web version of the SDLC Risk &amp; Control Self-Assessment template
(adapted from the Kosli secure-SDLC reference model). No build step, no server.

- **Risk Register** — edit inherent Likelihood &amp; Impact; residual risk is calculated automatically.
- **Risk–Control Matrix** — the risk↔control mapping.
- **Controls** — set implementation status &amp; effectiveness; this drives every mapped risk's residual score.
- **Dashboard** — live residual heat map and posture breakdown.
- **Scoring &amp; Methodology** — the 1–5 scales, rating bands, and the residual formula.

Entries are saved in your browser (localStorage). Use **Export data** to keep a JSON
backup and **Import** to restore it on another machine. **Export CSV** dumps the risk register.

## Deploy to GitHub Pages (free)

1. Create a new repository on GitHub (e.g. `sdlc-rcsa`).
2. Add `index.html` (and this README) and commit to the `main` branch.
3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   pick `main` / `root`, and Save.
4. Your app goes live at `https://<your-username>.github.io/sdlc-rcsa/` within a minute or two.

## Edit / extend

Everything lives in `index.html`. The seed data is the `SEED` object near the top of the
`<script>` block — adjust risks, controls, mappings, or scoring weights there.

# Secure SDLC Risk & Control Self-Assessment (RCSA) Template

An automated, data-driven **Risk & Control Self-Assessment (RCSA)** framework aligned to the **Kosli Secure SDLC model**. This template separates risks from controls, mapping them via a central matrix where residual risk scores are automatically calculated based on the assessed effectiveness of underlying controls. First, I created an Excel template based on the Kosli Secure SDLC model. Because the original Kosli RCSA template separates the risk register from the controls, I modified it by adding six new controls and linking them directly to the Risk Register alongside their control statuses and then I convert the template to web app. The original Excel template is uploaded as well.

---

## 📋 Overview & Model Structure

The framework encapsulates the entire software delivery lifecycle across **9 Core SDLC Risks** and **22 Security Controls** distributed across four key stages:

```
                          ┌──────────────────────────┐
                          │    Secure SDLC Model     │
                          └─────────────┬────────────┘
                                        │
         ┌──────────────────┬───────────┴───────────┬──────────────────┐
         ▼                  ▼                       ▼                  ▼
   [ Build Stage ]  [ Release Stage ]       [ Runtime Stage ]  [ Lifecycle Stage ]
   • Version Ctrl   • Code Review           • Change Records   • Role Training
   • Provenance     • QA Testing            • Deploy Controls  • Pen Testing
   • Hardened CI    • SAST / SCA / Img      • Secrets Mgmt     • Ownership
   • SBOM / IaC     • Deploy Approvals      • Least-Privilege  • GenAI Guardrails
```

### The 9 Core Risks
1. **RISK-0001: Supply Chain Compromise** (Mitigated by: Artifact Provenance, Dependency Firewall, SCA, etc.)
2. **RISK-0002: Insider Threat** (Mitigated by: Version Control, Code Review, Deployment Approvals, System Access Controls)
3. **RISK-0003: Unauthorised Deployment** (Mitigated by: Binary Provenance, Deployment Approvals & Controls, Workload Monitoring)
4. **RISK-0004: Credential and Secret Exposure** (Mitigated by: Pre-commit Secrets Scanning, Secrets Management, Auto-Revocation)
5. **RISK-0005: Vulnerable Software in Production** (Mitigated by: Dependency Management, QA, SAST, SCA, Container Scans, IaC Analysis)
6. **RISK-0006: Audit and Compliance Failure** (Mitigated by: Artifact Provenance, Code Review, Change Records, GenAI Guardrails)
7. **RISK-0007: Unauthorised System Access** (Mitigated by: Secrets Management, System Access Controls)
8. **RISK-0008: Configuration Drift** (Mitigated by: Version Control, IaC, Change Records, Deployment Controls, Drift Detection)
9. **RISK-0009: Environment Breach** (Mitigated by: System Access Controls, Runtime Workload Monitoring, Penetration Testing)

---

## 🗂️ Workbook Architecture

The template contains the following interconnected worksheets. Work through them in this general order:

| Tab Name | Role & Purpose |
| :--- | :--- |
| **`Instructions`** | Quick-start guidance for new users and onboarding info. |
| **`Dashboard`** | High-level summary view aggregating residual risk counts, risk heatmaps, and control coverage. |
| **`Risk Register`** | The master list where assessors input inherent risks and view live formula-driven residual risk ratings. |
| **`Risk-Control Matrix`** | A matrix grid mapping controls (`CTRL`) to risks (`RISK`) using an `X` relationship indicator. |
| **`Build / Release / Runtime / Lifecycle`** | Four distinct stage inventories to track control implementation status, effectiveness ratings, and evidence. |
| **`Scoring Legend`** | Defines 1–5 qualitative scales, the 5x5 heatmap matrix, and underlying math weights. |
| **`Definitions`** | A deep-dive data dictionary explaining columns, allowed statuses, and workflows. |
| **`Model Reference`** | Static reference library maintaining the baseline Secure SDLC taxonomy. |

---

## ⚙️ How the Scoring Methodology Works

Unlike legacy assessments where residual risk is estimated manually, this model uses an **automated, control-driven mathematical reduction engine**.

### 1. Inherent Risk Rating
Assessors define the raw, unmitigated hazard level by assigning scores on two 1–5 axes:
* **Likelihood (L):** 1 (Rare) to 5 (Almost Certain)
* **Impact (I):** 1 (Insignificant) to 5 (Severe)
* $	ext{Inherent Score} = 	ext{Inherent L} 	imes 	ext{Inherent I}$ (Range: 1 to 25)
  * **Critical:** 15–25
  * **High:** 8–14
  * **Medium:** 4–7
  * **Low:** 1–3

### 2. Control Effectiveness Weighting
Every control mapped to a risk on the **Risk-Control Matrix** is assigned a mathematical credit based on its assessed health:
* **Effective:** `1.0` (Full mitigation credit)
* **Partially Effective:** `0.5` (50% mitigation credit)
* **Ineffective:** `0.0` (No credit)
* **Not Tested:** `0.0` (Conservative safety default)

### 3. Automated Residual Risk Calculation
The model calculates the **Average Control Effectiveness ($	ext{Avg Ctrl Eff}$)** for all controls explicitly mapped to that specific risk. The residual parameters are then derived as:

$$	ext{Residual L} = 	ext{ROUND}\left( 	ext{Inherent L} - (	ext{Inherent L} - 1) 	imes 	ext{Avg Ctrl Eff},\, 0 
ight)$$
$$	ext{Residual I} = 	ext{ROUND}\left( 	ext{Inherent I} - (	ext{Inherent I} - 1) 	imes 	ext{Avg Ctrl Eff},\, 0 
ight)$$
$$	ext{Residual Score} = 	ext{Residual L} 	imes 	ext{Residual I}$$

> 💡 **Behavior:** If all mapped controls are 100% **Effective**, the residual metrics scale completely down to the floor value of `1`. If no controls are effective or remain untested, the residual risk stays perfectly equal to its raw inherent risk score.

---

## 🚀 Operating Guide: Step-by-Step Workflow

To conduct an assessment cycle using the template, follow these guidelines:

### Step 1: Initialize Setup
Complete the metadata section on the cover sheet:
* Identify the **Organization / Product** and **In-scope services/systems**.
* Document the **Assessment period**, **Risk Owner / Assessor**, and set your **Next review due** date.

### Step 2: Establish the Inherent Baseline
1. Open the **`Risk Register`** tab.
2. Review the 9 pre-loaded SDLC risks.
3. Complete the **blue text inputs** for **Inherent Likelihood** and **Inherent Impact**. Assume absolutely zero security controls are functioning when assigning these values.

### Step 3: Grade Your Security Controls
1. Navigate across the four control stage tabs (**Build**, **Release**, **Runtime**, **Lifecycle**).
2. For each control, update the following fields:
   * **Implementation Status:** `Implemented` or `Not Implemented`.
   * **Control Effectiveness:** Select `Effective`, `Partially Effective`, `Ineffective`, or `Not Tested`.
   * **Evidence Reference:** Paste documentation links, CI/CD YAML paths, architecture diagrams, or audit logging queries to prove the rating.
   * **Control Owner:** Assign a specific platform, security, or engineering stakeholder.

### Step 4: Map Relationships
If you customize or add a control relationship, go to the **`Risk-Control Matrix`** tab and log an `X` at the intersecting cell. The template will automatically adjust the calculated `# Ctrls` and `Avg Ctrl Eff` properties.

### Step 5: Address Risk Appetite Violations
1. Review the final calculated metrics on the **`Dashboard`** and **`Risk Register`**.
2. **Critical Directive:** Any residual risk rating evaluating to **High** or **Critical** is automatically flagged as outside organizational risk appetite.
3. For these gaps, the risk owner must formulate an active remediation blueprint within the **Action Plan** columns, defining clear milestones, remediation owners, and target resolution dates.

---

## 🛠️ Customization & Maintenance

This framework is built to be an evolving, living repository.

* **To Add a New Risk:** Append a row to the `Risk Register` and a corresponding row inside the `Risk-Control Matrix`. Copy the underlying formulas for `Avg Ctrl Eff` and residual scores downwards.
* **To Add a Tailored Control:** Add a column to the `Risk-Control Matrix` and a matching tracker row inside its corresponding stage tab (`Build`, `Release`, etc.).
* **To Modify Weightings:** Open the `Scoring Legend` tab and alter the numerical mappings assigned to effectiveness properties. All formulas across the workbook dynamically reference these cells.

---
*Disclaimer: Ensure control definitions are audited against your production infrastructure, CI/CD orchestrators, and internal security policies before finalizing quarterly risk signs-offs.*

---
