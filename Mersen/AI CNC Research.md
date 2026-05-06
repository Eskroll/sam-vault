# AI CNC Research

**Last updated:** 2026-05-05
**Status:** Delivered — March 2026
**Owner:** Sam Blakeley (Mersen intern)

---

## Overview

Research project evaluating AI-assisted CNC programming software for Mersen's manufacturing operations. Deliverables presented to leadership (boss + GM). Full package: Excel workbook, PDF executive brief, and PowerPoint presentation.

---

## Context — Mersen's CNC Situation

- CNC programming team previously had 5 members; 3rd shift person was let go
- Remaining 4 programmers feeling overwhelmed / capacity-constrained
- Workflow: receive prints + CAD models → programming team creates toolpaths
- **Software stack:** SolidWorks + Mastercam
- **Scope:** Mills only — lathes are handled manually (G-code done by operators) due to time constraints

---

## Research Scope

Evaluated AI/CAM automation tools across two categories:

**Category 1 — G-code generating tools (true automation)**
Actually output toolpaths or G-code from CAD/model input.

**Category 2 — Assistant-only tools**
Provide suggestions, parameter guidance, or conversational help — do not generate G-code directly.

---

## Vendors Evaluated

### Major / Established Companies

| Vendor | Tool | Mastercam Fit | Notes |
|---|---|---|---|
| CloudNC | CAM Assist | ✅ Native plugin | 1,000+ live shops; mills only (3-axis & 3+2); 80% auto-programming claim; free trial available at cloudnc.com/mastercam |
| Mastercam | Copilot | ✅ Built-in | Free on CONNECT plan; assistant-style, not full G-code generation |
| Hexagon | ProPlanAI | Partial | Enterprise-tier; broader CAM ecosystem |
| Autodesk | Fusion AI | ❌ Different stack | Relevant if migrating away from Mastercam |

### Smaller Companies / Startups

| Vendor | Tool | Mastercam Partner? | Notes |
|---|---|---|---|
| Up2parts | AutoCAM for Mastercam | ✅ Official partner | up2parts.com/en/products/up2parts-autocam-for-mastercam |
| LimitlessCNC | LimitlessCNC | ✅ Official partner | limitlesscnc.ai |
| Lambda Function | Lambda Function | ✅ Official partner | lambdafunction.ai |
| Toolpath.com | Toolpath | Watch list | Emerging |

---

## Top Recommendation — CloudNC CAM Assist

**Why #1:**
- Native Mastercam integration (no workflow change)
- Mills-only scope matches Mersen exactly (3-axis + 3+2)
- 1,000+ shops live in production
- Claims 80% of operations auto-programmed
- Free trial available — same-day setup
- Strongest ROI case vs. new programmer hire ($60–80K/year)

---

## Phased Recommendation

**Phase 1 — Immediate (no cost)**
- Deploy Mastercam Copilot (free on CONNECT plan)
- Start CloudNC CAM Assist free trial

**Phase 2 — Evaluate (30-day pilot)**
- Full CAM Assist pilot with time-savings measurement
- Request demos from Up2parts and LimitlessCNC
- Compare results across tools

**Phase 3 — Deploy**
- Full rollout of winning tool
- Generate ROI report for leadership
- Assess feasibility of extending to lathe programming (CloudNC has it on roadmap)

---

## Deliverables Produced

1. **Excel workbook** — Vendor comparison matrix with fit scores, demo links, cost/ROI analysis
2. **PDF executive brief** — Q&A format addressing boss's questions; business case section; prepared March 2026
3. **PowerPoint presentation** — 8-slide deck built via Claude in PowerPoint; covers problem → recommendation → roadmap

---

## Key Boss Questions Addressed

- **Cost/ROI:** Even 30% time savings across 4 programmers is significant. $60–80K/year new hire vs. SaaS subscription.
- **Mastercam compatibility:** CloudNC, Up2parts, LimitlessCNC all integrate natively — no workflow change needed.
- **Lathe scope:** Mills only for now; CloudNC has lathe on roadmap.
- **How to try:** Free trials available for CloudNC and Mastercam Copilot; demo requests for Up2parts/LimitlessCNC.
- **Is the tech mature enough?** Yes — 1,000+ production shops on CAM Assist alone; not experimental.
