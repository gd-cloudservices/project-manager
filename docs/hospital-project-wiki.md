# Hospital Project — Module & Feature Scope

This document tracks all modules and features that belong to the **Hospital Project** (patient-facing/hospital-facing), NOT the CMS (admin/config platform).

> **Market:** Philippines
> **Regulatory Framework:** DOH, PhilHealth (RA 11223 — Universal Health Care Act), BIR, PRC

---

## Guiding Principle

- **CMS** = Developer/admin platform for managing tenants, users, permissions, config, platform billing, and system-level settings.
- **Hospital Project** = Hospital-facing application for clinical operations, patient management, billing to patients/HMOs, and integrations.

---

## Modules in Hospital Project

### 1. Master Patient Index (MPI)

#### Core Patient Identity

| Feature | Description |
|---|---|
| Patient data model & schema | Design and implement patient entity with PH-specific fields |
| Patient CRUD API | Create, read, update, delete patient records |
| Patient registration form UI | Frontend for registering new patients |
| Patient list/directory UI | Searchable patient directory |
| Patient search & matching algorithm | Fuzzy matching, deduplication logic |
| Patient merge/deduplication | Merge duplicate patient records |
| Medical history API | Patient medical history endpoints |
| Patient profile & medical history UI | View patient profile and history |

#### Philippine-Specific Patient Fields

| Field | Description |
|---|---|
| PhilHealth Member Number (PCN) | PhilHealth Card Number — required for eClaims |
| PhilSys Number (PSN) | National ID under Philippine Identification System (RA 11055) |
| UMID / SSS / GSIS Number | Unified Multi-Purpose ID or government employee IDs |
| Senior Citizen ID & 20% discount flag | RA 9994 — Expanded Senior Citizens Act |
| PWD ID & 20% discount flag | RA 10754 — PWD discount entitlement |
| 4Ps beneficiary flag | Pantawid Pamilyang Pilipino Program — indigent household tagging |
| NHTS-PR / Listahanan tag | National Household Targeting System for indigent classification |
| Malasakit Center beneficiary flag | DOH Malasakit Center entitlement for indigent patients |
| Filipino name structure | Last name, first name, middle name (mother's maiden), suffix (Jr./Sr./III) |
| Civil status | Single, Married, Widowed, Separated, Annulled |
| Religion | Field for hospital/cultural context |
| Nationality | Filipino by default; foreign nationals require passport tracking |
| Philippine address | Barangay, Municipality/City, Province, Region, Zip — using PSGC codes |
| PhilHealth membership type | Employed, Self-Employed, Indigent, Sponsored, Lifetime, OFW |
| HMO affiliations | Link to HMO member records (Maxicare, Intellicare, MedoCard, etc.) |

---

### 2. Outpatient Department (OPD)

#### Core OPD

| Feature | Description |
|---|---|
| Visit/encounter data model | Design visit entity and relationships |
| Visit registration API | Register new visits/encounters |
| Visit registration UI | Frontend for visit check-in |
| Queue management API | Manage patient queues per department/doctor |
| Queue display board UI | Real-time queue display for waiting areas |
| Doctor queue management UI | Doctor's view of their patient queue |
| Consultation/encounter notes API | Save clinical notes per encounter |
| Consultation notes UI | Frontend for writing/viewing clinical notes |

#### Philippine-Specific OPD Features

| Feature | Description |
|---|---|
| Senior Citizen / PWD priority lane | Flag and queue prioritization per RA 9994 and RA 10754 |
| 4Ps / Malasakit triage tagging | Tag visits for indigent benefit routing at registration |
| Chief complaint in Filipino/English | Support bilingual entry for clinical notes |
| ICD-10 diagnosis coding | Required by PhilHealth for all claims; WHO ICD-10 with PH extensions |
| DOH disease surveillance tagging | Notifiable disease flags per DOH AO 2008-0009 (dengue, TB, COVID, etc.) |
| SOAP/DAR consultation note format | Structured notes: Subjective, Objective, Assessment, Plan |
| Referral slip generation | Inter-facility referral per DOH referral system guidelines |
| DOH Sentrong Sigla / PhilHealth accreditation display | Hospital classification banner (Level 1/2/3) |

---

### 3. Order Management

#### Core Orders

| Feature | Description |
|---|---|
| Lab order API | Create and manage laboratory orders |
| Lab order entry & results UI | Frontend for lab order workflow |
| Radiology order API | Create and manage radiology/imaging orders |
| Radiology order & results UI | Frontend for radiology workflow |
| Pharmacy/medication order API | Prescriptions and dispensing |
| Prescription & dispensing UI | Frontend for pharmacy workflow |

#### Philippine-Specific Order Features

| Feature | Description |
|---|---|
| Generic drug name requirement | RA 9502 — prescriptions must show generic name first, brand name in parentheses |
| DOH Essential Medicines List (NEML) | Formulary reference aligned with DOH National Essential Medicines List |
| Drug interaction checking | Reference Philippine MIMS / local drug database |
| S1 / S2 / S3 / OTC drug classification | PDEA dangerous drug classification; S1/S2 require special prescription pad |
| PDEA special prescription for controlled drugs | Dangerous Drugs Board (DDB) regulated substances (RA 9165) |
| Lab test mapping to PhilHealth benefit packages | Map orders to covered lab tests under PhilHealth case rates |
| DOH-required lab panels | Mandatory tests per DOH admission protocols (CBC, UA, etc.) |
| Chest X-ray pre-admission requirement | DOH-mandated for inpatient admission in accredited hospitals |

---

### 4. Billing (Patient & HMO)

#### Core Billing

| Feature | Description |
|---|---|
| Billing data model | Charges, invoices, payments schema |
| Charge capture & invoicing API | Generate bills from services rendered |
| Billing & invoicing UI | Frontend for billing staff |
| Payment processing API | Record and process payments |
| Payment processing UI | Frontend for payment collection |
| HMO claims API | Submit and track HMO claims |
| HMO claims management UI | Frontend for HMO claim workflows |

#### Philippine-Specific Billing Features

| Feature | Description |
|---|---|
| Official Receipt (OR) generation | BIR-accredited OR format; required for all payments under NIRC |
| BIR CAS / POS compliance | Cash Register Machine (CRM) / Point-of-Sale machine integration requirements |
| VAT exemption — Senior Citizens | RA 9994: 20% discount + VAT exemption on medicines, services, and fees |
| VAT exemption — PWD | RA 10754: 20% discount + VAT exemption |
| PhilHealth deduction on SOA | Automatic PhilHealth case rate deduction from Statement of Account |
| PhilHealth case rate packages | All Case Rates (ACR): C1–C6 packages; Z-benefit packages; TB-DOTS; maternity |
| Philhealth outpatient benefit (OPB) | Primary Care Benefit (PCB) package deduction for accredited facilities |
| HMO pre-authorization tracking | Record LOA (Letter of Authorization) number, coverage limits, co-pay |
| Supported HMOs | Maxicare, Intellicare, MedoCard, Avega, Caritas Health Shield, Cocolife, Pacific Cross |
| Malasakit Center zero-billing | Route indigent patient charges through Malasakit Center subsidy ledger |
| PhilHeath No Balance Billing (NBB) | Zero out-of-pocket for sponsored/indigent members in DOH-retained hospitals |
| Charge master using Philippine pesos (PHP) | All billing in PHP; no multi-currency required |
| Professional fee (PF) splitting | Doctor's professional fee tracked separately from hospital charges |
| Magna Carta for Nurses (RA 9173) | Separate tracking of nurse overtime/hazard pay as cost center |

---

### 5. PhilHealth eClaims (Operations)

#### Core eClaims

| Feature | Description |
|---|---|
| XML generation for eClaims | Generate PhilHealth-compliant XML documents |
| eClaims submission API | Submit claims to PhilHealth electronically |
| eClaims management UI | Track submission status and responses |
| Eligibility check API | Verify patient PhilHealth eligibility |

#### Philippine-Specific eClaims Features

| Feature | Description |
|---|---|
| PhilHealth Claim Form 1 (CF1) | Employer / facility data form |
| PhilHealth Claim Form 2 (CF2) | Member / patient data form |
| PhilHealth Claim Form 3 (CF3) | Attending physician data form |
| ICD-10 principal & secondary diagnoses | PhilHealth requires ICD-10 coded diagnoses on all claims |
| RVS / CPT procedure code mapping | Relative Value Scale (RVS) codes for procedures — PhilHealth requirement |
| PHIE (PhilHealth Integrated Electronic) system | Transmit claims via PHIE portal or EDR (Electronic Data Reporting) |
| MDR (Member Data Record) retrieval | Pull member contribution history and eligibility from PhilHealth |
| Z-benefit claim workflow | Special workflow for catastrophic conditions (cancer, stroke, dialysis) |
| TB-DOTS claim workflow | DOH-PhilHealth TB program claim routing |
| Maternity (MCPS) claim workflow | Maternity Care Package (normal delivery, caesarean, miscarriage) |
| eClaims status tracking | Pending, Transmitted, Received, Returned, Approved, Denied |
| Reason code management | Map PhilHealth denial/return reason codes to corrective actions |
| Accreditation number on claims | Hospital PhilHealth accreditation number stamped on all submissions |
| Attending physician PRC license number | PRC license and PhilHealth accreditation number required on CF3 |
| 45-day claim filing deadline | System warning when claims approach the 45-calendar-day filing window |

---

## What Stays in CMS

For reference, the CMS handles:

- Auth & access control (login, RBAC, delegation chain)
- User management (invite-only onboarding, profiles)
- Hospital/tenant management (registration, config, feature toggles, DOH license to operate number, PhilHealth accreditation number)
- Doctor management (profiles, specialties, HMO affiliations, PRC license number, PhilHealth accreditation number, scheduling config)
- Clinical forms (form builder, templates, submission config)
- PhilHealth config & monitoring (CMS-level configuration and claim monitoring dashboard)
- i18n (translation management — Filipino/English language support)
- Platform billing (developer → hospital subscription/licensing)
- System & utilities (audit trails, settings, exports)

---

## Philippine Regulatory Reference

| Regulation | Relevance |
|---|---|
| RA 11223 — Universal Health Care Act | PhilHealth coverage for all Filipinos; mandatory integration |
| RA 9502 — Cheaper Medicines Act | Generic drug name required first on all prescriptions |
| RA 9994 — Expanded Senior Citizens Act | 20% discount + VAT exemption; priority lanes |
| RA 10754 — PWD Act | 20% discount + VAT exemption for PWDs |
| RA 9165 — Comprehensive Dangerous Drugs Act | PDEA special prescription for S1/S2 controlled substances |
| RA 9173 — Philippine Nursing Act | Nurse professional fee / labor cost tracking |
| RA 11055 — Philippine ID System Act | PhilSys Number (PSN) as national patient identifier |
| DOH AO 2008-0009 | Notifiable disease surveillance and reporting |
| NIRC / BIR regulations | Official receipts, VAT exemptions, CRM/POS compliance |
| PhilHealth Circulars | Case rate packages, benefit schedules, eClaims filing rules |

---

## Notes

- When in doubt, ask: "Is this a hospital operation or a platform admin operation?"
- Hospital operations = Hospital Project
- Platform/tenant admin = CMS
- All monetary values in Philippine Peso (PHP)
- Date format: MM/DD/YYYY (Philippine standard)
- Default language: Filipino (fil) + English (en)
- Created: 2026-04-02
- Last updated: 2026-04-02 (Philippine market alignment)

---

*Source: ClickUp — healthcare workspace / CMS space / Hospital Project Wiki*
