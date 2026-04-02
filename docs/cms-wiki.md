# CMS Wiki — Module & Feature Scope

This document tracks all modules and features that belong to the **CMS** (admin/config platform), NOT the Hospital Project.

> **Market:** Philippines
> **Regulatory Framework:** DOH, PhilHealth (RA 11223), BIR, PRC

---

## Overview

A multi-tenant Content Management System for managing hospitals and their associated services. The CMS is the central back-office for onboarding hospitals, managing users, configuring features, handling translations, and monitoring performance. Each hospital operates as an isolated tenant with its own users, configurations, translations, and permissions.

The hospital-facing frontend is a **separate project** — the CMS serves as the administrative layer behind it.

**Tech Stack:** Vue (frontend), Laravel (backend), PostgreSQL (database), with Memcached and/or Redis for caching and performance where needed.

---

## 1. Users

The CMS has two top-level account types:

### 1.1 Developer Account

- Full unrestricted access to all CMS features and all hospitals.
- Can onboard new hospitals, assign Hospital Owners, manage global configurations, and manage permissions.
- Acts as the super-admin of the entire platform.

### 1.2 Hospital Owner (Primary)

- Assigned by the **Developer Account** during hospital onboarding.
- Strictly **one primary owner per hospital**.
- Can only access and manage data within their own hospital.
- Can create **sub-users** under their hospital with **custom granular permissions**.
- Can create sub-users with **owner-level permissions** (co-owners) — these have the same scope and capabilities as the primary owner but are still sub-users.
- Cannot grant sub-users permissions that exceed their own.

### 1.3 Sub-Users (created by Hospital Owner)

- Permissions are fully custom and granular, assigned per module and per action.
- Access is limited to the hospital they belong to.
- Can range from minimal access (e.g., read-only on Summary) to **owner-level access** (co-owner) depending on assigned permissions.
- Co-owners (sub-users with owner-level permissions) can also create and manage other sub-users within the same hospital.

---

## 2. Translations

A multilingual translation management module. Translations are structured as grouped key-value pairs used by the hospital's frontend for i18n.

**Structure:**

```json
{
  "global": {
    "submit_btn": "Submit",
    "cancel_btn": "Cancel"
  },
  "claims": {
    "new_claim": "New Claim",
    "claim_status": "Claim Status"
  }
}
```

**Rules:**

- The **Developer Account** creates and assigns translation sets to hospitals.
- **Hospital Owners** (and sub-users with permission) can only **edit** translations assigned to their hospital — they cannot create new translation keys or groups.
- Frontend accesses translations via dot notation (e.g., `global.submit_btn`).
- Supports multiple languages per hospital (e.g., English, Filipino, Cebuano).

---

## 3. Configuration

A dynamic, per-hospital feature configuration module. Configs control feature toggles, behavior settings, and operational modes for each hospital.

**Capabilities:**

- Feature toggles — enable/disable specific modules or features per hospital.
- Operational settings — e.g., maintenance mode (behavior TBD, depends on frontend project).
- Config structures are **created dynamically** by the Developer Account — no fixed schema, allowing flexibility as new features are added.

**Access Rules:**

- Only the **Developer Account** can create config structures and assign them to hospitals.
- Once assigned, the **Hospital Owner** (and sub-users with permission) can view or edit the config values, but not the structure itself.

---

## 4. Summary / Performance

A dashboard module for viewing hospital performance metrics. Focused primarily on claims data.

**Metrics include:**

- Total number of claims submitted
- Claims approved
- Claims paid
- Claims unpaid
- Claims with issues

**Access Rules:**

- **Developer Account** — can view summary/performance across all hospitals.
- **Hospital Owner** — can only view their own hospital's summary/performance.
- Sub-users — access based on assigned permissions.

---

## 5. Hospital Management

The onboarding and administration module for hospitals. Only accessible by Developer Accounts.

**Onboarding a new hospital includes:**

- Creating the hospital record
- Assigning a Hospital Owner account (primary owner)
- Initial configuration setup
- Branding settings (hospital-specific branding for their frontend)
- Subscription tier assignment

**Ongoing management:**

- View all users under a hospital
- Manage hospital-level settings
- Manage access keys for external integrations (e.g., PhilHealth eClaims API keys)
- DOH License to Operate (LTO) number
- PhilHealth accreditation number

**Note:** PhilHealth eClaims is one of many features/integrations — the platform is not limited to eClaims.

---

## 6. Permission Management

A granular, custom permission system scoped per **module** and per **action** (RBAC-style).

**Structure:**

- Permissions are defined as `module + action` combinations.
- The list of modules and actions is **not fixed** — new modules and actions are added as new features are built.
- Current CMS modules are the starting set; hospital-side modules will expand this significantly over time.

**Example (initial, will grow):**

| Module | Actions |
|---|---|
| Users | view, create, edit, delete |
| Translations | view, edit |
| Configuration | view, edit |
| Summary | view |
| (future) | (added as features are built) |

**Rules:**

- Only the **Developer Account** can manage the permission system and assign permissions to Hospital Owners.
- **Hospital Owners** can assign permissions to their sub-users, but **cannot assign permissions higher than their own**.
- Sub-users with owner-level permissions (co-owners) can also delegate permissions, following the same rule.
- Cascading hierarchy: Developer → Hospital Owner → Co-Owners / Sub-Users.

---

## 7. Doctor Management (CMS-side)

Managed by Developer Accounts and Hospital Owners via the CMS.

- Doctor profiles: PRC license number, PhilHealth accreditation number, specialties
- HMO affiliations: Maxicare, Intellicare, MedoCard, Avega, Caritas, Cocolife, Pacific Cross
- Scheduling configuration: availability windows, appointment slots
- Doctors are referenced by the Hospital Project for OPD and eClaims workflows

---

## 8. Clinical Forms (CMS-side)

Form builder and template management for hospital-facing clinical data collection.

- **Form Builder** — drag-and-drop form construction; field types, validation, conditions
- **Clinical Templates** — pre-built templates seeded per hospital (triage, SOAP notes, consent forms)
- **Submissions & Attachments config** — file type rules, attachment limits, storage config

---

## 9. PhilHealth Configuration & Monitoring

CMS-level configuration and monitoring dashboard for PhilHealth integration.

- Hospital PhilHealth accreditation number management
- eClaims API credentials and endpoint configuration
- Claim monitoring dashboard: overview of submitted, pending, approved, denied claims across hospitals (Developer view) or per hospital (Hospital Owner view)
- Benefit package configuration: enable/disable case rate packages per hospital

---

## 10. Platform Billing

Subscription and licensing management for the developer → hospital billing relationship.

- Subscription plan assignment per hospital (tier: Basic, Standard, Enterprise)
- Invoice generation and tracking
- Payment status monitoring
- Billing reports and analytics per hospital

---

## Architecture Summary

```
Developer Account (super-admin)
  └── Hospital A
  │     ├── Primary Owner (assigned by dev)
  │     │     ├── Co-Owner (sub-user with owner-level permissions)
  │     │     │     └── Sub-User 3 (custom permissions)
  │     │     ├── Sub-User 1 (custom permissions)
  │     │     └── Sub-User 2 (custom permissions)
  │     ├── Translations (assigned by dev, editable by hospital)
  │     ├── Configurations (structure by dev, values by hospital)
  │     ├── Permissions (assigned by dev, delegated by owner)
  │     ├── Doctor Management (profiles, specialties, scheduling)
  │     ├── Clinical Forms (templates, form builder)
  │     ├── PhilHealth Config (accreditation, API keys, monitoring)
  │     └── Summary/Performance (hospital-scoped)
  └── Hospital B
        └── ...
```

---

## Scope Decision Rule

> "Is this a hospital operation or a platform admin operation?"
> - Hospital operations → Hospital Project
> - Platform/tenant admin → CMS

---

## Notes

- Created: 2026-04-02
- Last updated: 2026-04-02
- Market: Philippines
- Default language: Filipino (fil) + English (en)

---

*Source: ClickUp — healthcare workspace / CMS space / CMS Wiki*
