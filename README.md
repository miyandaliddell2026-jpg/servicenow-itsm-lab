# ServiceNow Platform Fundamentals 

A hands-on lab completed on a live ServiceNow Personal Developer Instance covering the full ITSM lifecycle: incident management, service catalog configuration, change management approval workflows, and platform reporting.

---

## Lab Overview

| | |
|---|---|
| **Platform** | ServiceNow (Zurich release) |
| **Instance** | dev208431 |
| **Role** | Admin |
| **Environment** | Personal Developer Instance (PDI) |
| **Framework** | ITIL v4 |

---

## Table of Contents

1. [Environment Setup](#1-environment-setup)
2. [Incident Management](#2-incident-management)
3. [SLA Tracking and Resolution](#3-sla-tracking-and-resolution)
4. [Service Catalog Configuration](#4-service-catalog-configuration)
5. [Change Management Workflow](#5-change-management-workflow)
6. [Reporting and Analytics](#6-reporting-and-analytics)
7. [Key Takeaways](#7-key-takeaways)
8. [Full Screenshot Walkthrough](#8-full-screenshot-walkthrough)

---

## 1. Environment Setup

Provisioned a Personal Developer Instance (PDI) through the ServiceNow Developer Portal — a free, fully-functional sandbox running the Zurich release with full admin access, 46 pre-loaded plugins, and demo data.

![PDI Dashboard — dev208431 Online](screenshots/step1.png)

| Field | Value |
|---|---|
| URL | `https://dev208431.service-now.com` |
| Username | `admin` |
| Status | Online |
| ServiceNow Studio | Installed |
| App Engine Studio | Installed |
| Version | Zurich |

---

## 2. Incident Management

Created and worked a full incident using the Service Desk → Incidents module following an ITIL-aligned triage process.

**Scenario:** User Abel Tuter reports Microsoft Outlook has stopped working with error: *"Cannot connect to the Exchange server."* Issue is isolated to their Wi-Fi connected laptop — other users in the same building are unaffected.

![Incident Form — INC0010002](screenshots/step3.png)

| Field | Value |
|---|---|
| Caller | Abel Tuter |
| Category | Software |
| Subcategory | Email |
| Short description | User cannot access Outlook — error: Cannot connect to server |
| Impact | 2 - Medium |
| Urgency | 2 - Medium |
| Priority | 3 - Moderate *(auto-calculated)* |
| Assignment group | Service Desk |
| State | New |

> Priority is automatically calculated from Impact × Urgency via a lookup matrix — not manually set. Setting both Impact and Urgency to 2-Medium causes the platform to auto-calculate Priority 3-Moderate — no manual input required.

### Working the ticket

Work notes document investigation steps internally — not visible to the caller — keeping the audit trail separate from customer-facing communication.

![Work Notes and Activities Audit Trail](screenshots/step4.png)

**Work notes posted:**
> *"Contacted user. Confirmed error message. Outlook profile appears corrupted. Attempting profile repair. Instructed user to use OWA (webmail) in the interim. Resolution ETA: 30 minutes."*

The Activities panel captures every field change with a timestamp and author — a complete, tamper-evident audit trail for the life of the ticket.

---

## 3. SLA Tracking and Resolution

Resolved the incident through the Resolution Information tab with full documentation of the fix. ServiceNow enforces a mandatory Resolution Code before closure — attempting to close without one blocks the save.

![Resolution Notes and SLA Panel — INC0010002](screenshots/step5.png)

**Resolution notes:**
> *"Rebuilt Outlook profile. Removed and re-added the Exchange account. User confirmed Outlook is working. Issue was a corrupted OST file. Closed with user confirmation."*

**Task SLAs — real-time compliance tracking:**

| SLA | Type | Target | Business time left | Elapsed |
|---|---|---|---|---|
| Priority 3 resolution | Resolution | 1 day | 23 hrs 58 min | 1 minute |
| Priority 3 response | Response | 4 hours | 3 hrs 58 min | 1 minute |

> SLAs start automatically when the incident is created and count down in real time — this is how service desks measure whether teams are meeting contractual response and resolution commitments.

---

## 4. Service Catalog Configuration

Built a complete Service Catalog item from scratch — transforming a static catalog entry into a dynamic self-service request form with four configured variables.

**Why this matters:** Catalog items let users request common IT services without calling the help desk. Each request auto-generates a structured ticket, reducing ticket volume for routine work and speeding up fulfillment.

![Catalog Item Variables — New Laptop Request](screenshots/step6.png)

**Variables added to the request form:**

| Type | Question shown to user | Field name | Mandatory |
|---|---|---|---|
| Select Box | Laptop Model Preference | `laptop_model_preference` | No |
| Multi Line Text | Business Justification | `business_justification` | Yes |
| Single Line Text | Requester Name | `requester_name` | Yes |
| Date | Required By Date | `required_by_date` | Yes |

> The Select Box variable was configured with options `Standard, Developer, Executive` — demonstrating form design beyond basic text fields. Variables tab shows 4 of 4 configured and active.

---

## 5. Change Management Workflow

Created a Normal change request for a security patch deployment and walked it through the full ITIL approval pipeline: New → Assess → Authorize → Scheduled → Implement → Review → Closed.

**Why this matters:** Unlike incidents which are reactive, change requests are proactive and require structured authorization before work begins — protecting production stability from uncoordinated modifications.

**Change request — CHG0030001:**

| Field | Value |
|---|---|
| Type | Normal |
| Category | Software |
| Risk | Low |
| Impact | 2 - Medium |
| Short description | Deploy security patch MS24-001 to all Windows workstations |
| Planned start | 2026-05-30 02:00:00 |
| Planned end | 2026-05-30 06:00:00 |
| Backout plan | Uninstall patch via WSUS if issues reported post-deployment |
| Test plan | Verify via WSUS console. Test on 2 pilot machines before full rollout. |

### Approval request — Change Manager

After submitting, an approval request was sent to the Change Manager. The approval record shows the approver, the change request being approved, and the full activity log.

![Change Manager Approval Record — CHG0030001](screenshots/step7.png)

### Full approval chain — CAB triggered

Once the Change Manager approved, the workflow automatically escalated to a full CAB (Change Advisory Board) review — triggering approval requests to 6 additional cross-functional stakeholders.

![Full Approvers Tab — 8 approvers, CAB triggered](screenshots/step8.png)

| Approver | Group | State |
|---|---|---|
| Change Manager | Change Management | Approved |
| Chuck Tomasi | Change Management | No longer required |
| Luke Wilson | CAB Approval | Requested |
| Bernard Laboy | CAB Approval | Requested |
| Christen Mitchell | CAB Approval | Requested |
| cab approver | CAB Approval | Requested |
| Ron Kettering | CAB Approval | Requested |
| Howard Johnson | CAB Approval | Requested |

> The two-stage approval design reflects real enterprise change control: one person confirms the change is ready for broader review, then a cross-functional group assesses impact before the maintenance window opens.

---

## 6. Reporting and Analytics

Built a custom report using the Reports module to visualize incident volume across priority levels — demonstrating the ability to extract operational insight directly from platform data.

**Report configuration:**

| Field | Value |
|---|---|
| Report name | Incident Volume by Priority - Last 30 Days |
| Source type | Table |
| Table | Incident [incident] |
| Chart type | Bar |
| Group by | Priority |
| Aggregation | Count |

![Incident Volume by Priority — Saved Bar Chart](screenshots/step9.png)

**Results:**

| Priority | Count |
|---|---|
| 1 - Critical | ~27 |
| 5 - Planning | ~19 |
| 3 - Moderate | ~13 |
| 4 - Low | ~5 |
| 2 - High | ~4 |

> The majority of open incidents are Critical or Planning state — a pattern that would prompt a real service desk manager to investigate whether critical incidents are being resolved promptly or accumulating as a backlog.

---

## 7. Key Takeaways

- **Priority is auto-calculated** from Impact × Urgency — not manually set — ensuring consistent triage across all agents.
- **Work notes and comments are separate channels** — internal notes for the team, comments for the customer — both captured in the Activities audit trail.
- **SLAs track automatically** from ticket creation, giving real-time visibility into whether response and resolution targets are being met.
- **Resolution codes are mandatory** before incident closure — ServiceNow enforces this at the form level, preventing incomplete closures.
- **Catalog variables** are the engine behind self-service forms — field types, mandatory flags, and dropdown options are all configurable per item.
- **Change approval workflows are multi-stage** — an initial manager sign-off triggers a broader CAB review, ensuring cross-functional stakeholder input before production changes are authorized.
- **Reports can be built against any table** in the system using a 4-step wizard with an Analytics Q&A natural language option for rapid prototyping.

---

## 8. Full Screenshot Walkthrough

All 25 lab screenshots are available in the [/screenshots](./screenshots) folder covering every step from PDI provisioning through report creation.

---

## Technologies

- ServiceNow (Zurich release)
- ITIL v4 incident and change management framework
- ServiceNow Service Desk, Service Catalog, Change Management, and Reports modules
- Personal Developer Instance (PDI)

---

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [PDI Guide](https://developer.servicenow.com/dev.do#!/guides/zurich/now-platform/pdi-guide/personal-developer-instance-guide-introduction)
- [Incident Management Docs](https://docs.servicenow.com/bundle/zurich-it-service-management/page/product/incident-management/concept/c_IncidentManagement.html)
- [Service Catalog Docs](https://docs.servicenow.com/bundle/zurich-it-service-management/page/product/service-catalog-management/concept/c_ServiceCatalogManagement.html)
- [Change Management Docs](https://docs.servicenow.com/bundle/zurich-it-service-management/page/product/change-management/concept/c_ITILChangeManagement.html)
