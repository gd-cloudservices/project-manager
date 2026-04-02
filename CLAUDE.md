# Healthcare Project — CLAUDE.md

## Project Overview

This is a multi-platform healthcare system split into two distinct applications:

- **CMS** — Admin/config platform for developers and hospital administrators (tenant management, RBAC, platform billing, system settings)
- **Hospital Project** — Hospital-facing clinical application (patient management, OPD, orders, billing, PhilHealth eClaims)

## ClickUp Workspace

- **Workspace:** healthcare (ID: `9016176559`)
- **Space:** CMS (ID: `90166704644`)
- **Wiki:** Hospital Project Wiki → `docs/hospital-project-wiki.md`

## Docs

| File | Description |
|---|---|
| `docs/hospital-project-wiki.md` | Module & feature scope for the Hospital Project |

## Key Modules (Hospital Project)

1. **MPI** — Master Patient Index (patient records, search, deduplication)
2. **OPD** — Outpatient Department (visits, queues, consultation notes)
3. **Order Management** — Lab, radiology, pharmacy orders
4. **Billing** — Patient billing, HMO claims
5. **PhilHealth eClaims** — XML generation, submission, eligibility checks

## Key Modules (CMS)

1. **Auth & Access Control** — Login, RBAC, delegation chain
2. **User Management** — Invite-only onboarding, profiles
3. **Hospital/Tenant Management** — Registration, config, feature toggles
4. **Doctor Management** — Profiles, specialties, HMO affiliations, scheduling
5. **Clinical Forms** — Form builder, templates, submissions
6. **PhilHealth Config** — CMS-level configuration and monitoring
7. **i18n** — Translation management, language config
8. **Platform Billing** — Developer → hospital subscription/licensing
9. **System & Utilities** — Audit trails, settings, data exports

## Decision Rule

> "Is this a hospital operation or a platform admin operation?"
> - Hospital operations → Hospital Project
> - Platform/tenant admin → CMS
