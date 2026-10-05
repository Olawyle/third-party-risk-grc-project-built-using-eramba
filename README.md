# Vendor Security Evidence & Remediation Management

A third-party risk management (TPRM) program built for **Solvane Cloud Technologies**, a fictional B2B SaaS company, to demonstrate a working risk assessment, evidence-tracking, and remediation workflow aligned to ISO/IEC 27001:2022 supplier-relationship controls.

> **All vendors, contacts, findings, and data in this repository are fictional and created for portfolio demonstration purposes only.**

---

## 1. Business Scenario

Solvane Cloud Technologies relies on 13 third-party vendors spanning cloud infrastructure, payment processing, customer data platforms, and professional services. Without a structured process, vendor risk goes unmanaged until an incident forces the issue. This project builds that process from the ground up: how vendors are tiered, what evidence each tier requires, how risk is scored, and how gaps get tracked to closure.

**What this demonstrates:**
- Designing a risk appetite framework and 5×5 risk matrix from first principles
- Configuring a real GRC platform (eramba) rather than just describing one on paper
- Bridging tool limitations (eramba Community's lack of custom fields) with a practical operational workaround
- Mapping vendor evidence requirements to a recognized control framework (ISO/IEC 27001:2022)
- Running findings through a full detect → assess → remediate → validate lifecycle

---

## Project Status

- [x] Risk classifications (Likelihood / Impact) configured
- [x] Risk calculation method (Likelihood × Impact) configured
- [x] Full 5×5 risk appetite matrix built and saved
- [x] All 13 vendors created and individually risk-scored (inherent + residual)
- [x] Evidence workbench built and populated (Vendor Inventory, Findings Log, Remediation Tracker)
- [ ] Evidence collection in progress (Evidence Tracker — 0/46 items received)
- [ ] Power BI reporting layer

---

## 2. Methodology

| Component | Approach |
|---|---|
| **Framework anchor** | ISO/IEC 27001:2022 Annex A supplier-relationship controls (A.5.19–A.5.24) |
| **Risk scoring** | 5×5 matrix, Likelihood × Impact, scores 1–25 |
| **Risk appetite bands** | Low (1–5) · Medium (6–12) · High (13–19) · Critical (20–25) |
| **Vendor tiering** | Critical / High / Medium / Low, based on data sensitivity and operational dependency |
| **Tools** | eramba Community (risk register + scoring) · Excel (evidence tracker + operational metadata) · Power BI (reporting layer) |

**Why an even (rather than skewed) score distribution:** with no historical scoring data yet, an even four-way split of the 1–25 range is simpler to explain and defend than pre-guessing how scores would cluster. This can be revisited once enough real risk records exist to justify recalibration.

**Why Excel alongside eramba:** eramba Community edition does not support custom fields (this is an Enterprise-only feature), so fields like Business Owner, Contract Owner, Renewal Date, and Reassessment Date are tracked in a companion Excel workbook rather than forced into eramba's default schema.

---

## 3. Vendor Population

| Vendor | Function | Tier |
|---|---|---|
| AWS | Cloud hosting/infrastructure | Critical |
| Stripe | Payment processing | Critical |
| Salesforce | CRM/customer data | Critical |
| Datadog | Monitoring/logging | High |
| GitHub | Source code repository | High |
| Brightline Dev Partners | Contract engineering / production access | High |
| Zendesk | Customer support / PII | High |
| Gusto | HR/payroll / employee PII | High |
| SendGrid | Transactional email | Medium |
| DocuSign | E-signature/contracts | Medium |
| Redshield Security | Annual penetration testing | Medium |
| Pulsemetric | Web/product analytics | Low |
| Nexus Office Supply | Hardware procurement | Low |

---

## 4. Implementation Walkthrough

### 4.1 Risk Classifications
Defined Likelihood (Rare → Almost Certain) and Impact (Insignificant → Severe) scales, each 1–5, in eramba.

`![Risk Classifications](screenshots/01-classifications.png)`

### 4.2 Risk Calculation Method
Configured Single Matrix – Multiplication (Likelihood × Impact) as the scoring formula.

`![Calculation Method](screenshots/02-calculation-method.png)`

### 4.3 Risk Appetite Matrix
Built the full 5×5 threshold matrix, mapping all 25 Likelihood/Impact combinations to Low/Medium/High/Critical bands.

`![Risk Matrix](screenshots/03-risk-matrix.png)`

### 4.4 Full Vendor Population — Risk Scored
All 13 vendors created as Third Party records and individually risk-scored (inherent and residual, via a documented Risk Treatment decision per vendor). The population spans the full tier range:

| Tier | Vendors | Typical Likelihood × Impact pattern |
|---|---|---|
| Critical | AWS, Stripe, Salesforce | Unlikely × Severe/Major → Medium inherent, Low–Medium residual |
| High | Datadog, GitHub, Brightline Dev Partners, Zendesk, Gusto | Possible × Major/Moderate → Medium–High inherent |
| Medium | SendGrid, DocuSign, Redshield Security | Unlikely–Possible × Minor/Moderate → Low–Medium |
| Low | Pulsemetric, Nexus Office Supply | Rare/Unlikely × Insignificant → Low |

Two vendors illustrate the model deliberately **not** improving residual risk where no real control exists yet (Zendesk, Gusto — both tied to open, unremediated findings), rather than optimistically assuming "Mitigate" alone reduces risk.

`![Third Party Risks list](screenshots/04-full-vendor-list.png)`
`![Brightline risk detail](screenshots/04b-brightline-high-risk.png)`

### 4.5 Evidence Workbench
Built and populated a companion Excel workbook (`Vendor_Evidence_Remediation_Workbench.xlsx`) with six tabs — covering everything eramba Community can't natively structure (custom fields are an Enterprise-only feature):

- **Vendor Inventory** — all 13 vendors with owner assignments and review cadence
- **Evidence Tracker** — 46 required-evidence line items, auto-derived from each vendor's tier
- **Control Mapping** — evidence types mapped to ISO/IEC 27001:2022 clauses
- **Findings Log** — all 7 findings with inherent/residual risk and status
- **Remediation Tracker** — one action per finding, with owner and target date

`![Evidence Workbench](screenshots/05-evidence-workbench.png)`

### 4.6 Review Automation
Confirmed eramba's automatic review-record generation, tying each vendor's Next Review Date to a recurring review cycle — shorter cadences (3–6 months) for vendors with open findings, longer cadences (18–24 months) for Low-tier vendors.

`![Review Cycle](screenshots/06-reviews.png)`

---

## 5. Sample Findings & Remediation

| ID | Vendor | Finding | Severity | Residual Risk | Status |
|---|---|---|---|---|---|
| F1 | Brightline Dev Partners | Expired SOC 2 | High | Medium | Open — target 2026-10-25 |
| F2 | Zendesk | No signed DPA despite PII processing | High | Medium | Open — target 2026-11-09 |
| F3 | Salesforce | Subprocessor list not reviewed for 18 months | Medium | Medium | Open — target 2026-11-25 |
| F4 | Redshield Security | No incident-notification clause in MSA | Medium | Medium | Open — target 2026-11-25 |
| F5 | Gusto | Reassessment overdue by 6 months | Medium | Medium | Open — target 2026-10-25 |
| F6 | AWS | No internal shared-responsibility review record | Low | Low | Open — target 2026-10-10 |
| F7 | Nexus Office Supply | Incorrectly classified as Medium; should be Low | Low | Low | Closed — reclassified in eramba |

Full detail — including remediation actions, owners, and target dates — lives in `Vendor_Evidence_Remediation_Workbench.xlsx` under **Findings Log** and **Remediation Tracker**.

---

## 6. Results / Dashboard

*(To be added once the Power BI reporting layer is built.)*

`![Dashboard](screenshots/06-dashboard.png)`

---

## 7. Tech Stack

- **eramba Community Edition** — GRC platform, self-hosted via Docker on Ubuntu 26.04 (VMware)
- **Microsoft Excel** — evidence tracking, vendor operational metadata
- **Power BI** — reporting/visualization *(planned)*
- **ISO/IEC 27001:2022** — control framework reference

---

## 8. Repository Structure

```
├── README.md
├── Vendor_Evidence_Remediation_Workbench.xlsx
├── screenshots/
│   ├── 01-classifications.png
│   ├── 02-calculation-method.png
│   ├── 03-risk-matrix.png
│   ├── 04-aws-risk.png
│   ├── 05-evidence-workbench.png
│   └── 06-dashboard.png
└── docs/
    └── Project_3_Implementation_Roadmap.docx
```

---

## Disclosure

This is a fictional portfolio project. Solvane Cloud Technologies, its 13 vendors, all named contacts, and all findings are invented for demonstration purposes and do not represent any real organization, individual, or incident.