# Product Requirements Document

## Commercial Interior Design Firm — Marketing & Lead Generation Website

**Part 1 of 3 — Foundation**

---

## 1\. Document Control

| Field | Value |
| :---- | :---- |
| Document | PRD — Part 1: Foundation |
| Version | 0.2 (Draft) |
| Date | 4 August 2026 |
| Author | Product Team |
| Status | Awaiting client input on flagged assumptions |
| Related docs | `PRD-Part-2-Functional.md`, `PRD-Part-3-NonFunctional.md` |
| Supersedes | — |

**Change log**

| Version | Date | Change |
| :---- | :---- | :---- |
| 0.1 | 2026-08-04 | Initial draft. Phase 1 Discovery folded into this document at client request. |
| 0.2 | 2026-08-04 | Added F-13 Guided Enquiry (P2). Added F-14 AI Assistant (P1) by client direction, with decision record at Part 3 §27.1 and RISK-13 to RISK-16. |

---

## 2\. Executive Summary

`[CLIENT NAME]` is a commercial interior design and fit-out firm serving `[REGION]`. They currently win work through referral and direct pitch, and have no web presence capable of supporting or extending that pipeline.

This project delivers a **portfolio-led, conversion-oriented marketing website** whose job is to convert cold and warm commercial prospects into qualified enquiries, and to legitimise the firm when prospects research them after a referral or pitch.

The site is a **static marketing site** — no user accounts, no database, no transactional functionality. It will be hand-built for performance and image quality, deployed on a zero-cost hosting tier, and maintained by the developer on client request.

**The defining constraint of this project is a thin photography library.** Commercial buyers require visual proof before enquiring. The entire content and layout strategy is therefore designed to produce credibility from a small number of assets, rather than assuming a large portfolio. This constraint drives more decisions in this document than any other factor.

**The defining dependency is the absence of brand assets.** No logo, colour system, or typography exists. This is addressed in §9 as a blocking decision.

---

## 3\. Problem Statement

### 3.1 The client's problem

The firm sells a high-consideration, high-value service — commercial fit-outs are five- and six-figure decisions with long procurement cycles and multiple stakeholders. Buyers at this level will not engage without evidence of competence.

Today:

- **Prospects who hear about the firm cannot verify it.** A referral from a colleague leads to a search that returns nothing credible. The referral leaks.  
- **The firm cannot be discovered.** Buyers searching "office fit-out `[CITY]`" find only competitors.  
- **The pitch does no work between meetings.** After a first conversation, there is nothing for the prospect to circulate internally to the other three people who must approve the spend.  
- **The firm cannot control its own positioning.** Without a site, the firm is described by whoever is describing it — usually inaccurately, usually as "an interior decorator."

### 3.2 The buyer's problem

A facility manager tasked with refurbishing an office faces a genuinely difficult, infrequent decision. They will do this perhaps twice in a career. They are risk-exposed: a bad fit-out means visible disruption, blown budget, and their name on it.

They need to answer, quickly:

1. Has this firm done work like mine, at my scale, in my sector?  
2. Will they disrupt my operations, and for how long?  
3. Do they handle everything, or will I be co-ordinating five vendors?  
4. Is this going to cost roughly what I have?  
5. Are they still going to be here in eighteen months?

A conventional "beautiful photo grid" portfolio site answers only the first, and only partially. **The site's competitive advantage will be answering questions 2–5, which competitors largely ignore.** This is also fortunate: those answers are text and diagrams, not photography — which directly mitigates our primary constraint.

---

## 4\. Objectives

### 4.1 Product objectives

| ID | Objective | Rationale |
| :---- | :---- | :---- |
| OBJ-1 | Convert qualified commercial prospects into structured enquiries | Primary commercial purpose |
| OBJ-2 | Establish credibility sufficient to survive post-referral scrutiny | Protects existing pipeline |
| OBJ-3 | Rank for high-intent local commercial fit-out search terms | Creates new pipeline |
| OBJ-4 | Equip prospects to sell the firm internally to other stakeholders | Shortens B2B procurement cycles |
| OBJ-5 | Present a coherent, ownable brand identity | Currently absent entirely |
| OBJ-6 | Remain cheap to host and simple to maintain | Client is not paying for platform overhead |

### 4.2 Explicit non-objectives

Stating these prevents scope creep later:

- ❌ Not an e-commerce or booking platform  
- ❌ Not a client portal or project-tracking system  
- ❌ Not a CMS the client edits themselves (Phase 2 candidate — see Part 3\)  
- ❌ Not a blog on day one (see §8.3)  
- ❌ Not multi-language on day one  
- ❌ Not a residential interior design site — commercial only, and the site should actively signal this

---

## 5\. Business Goals & Success Metrics

Vanity metrics (pageviews, "impressions") are excluded deliberately. Every metric below ties to revenue or to a decision we would actually make.

| ID | Metric | Baseline | Target (6 months post-launch) | Measured via |
| :---- | :---- | :---- | :---- | :---- |
| KPI-1 | Qualified enquiries per month | 0 (site) | 4–8 | Form submissions, deduplicated |
| KPI-2 | Enquiry → discovery call rate | n/a | ≥ 50% | Client CRM / manual log |
| KPI-3 | Organic sessions per month | 0 | 250–600 | Analytics platform (§23) |
| KPI-4 | Ranking for `office fit-out [CITY]` | unranked | Top 10 | Search Console |
| KPI-5 | Project detail page → enquiry rate | n/a | ≥ 3% | Analytics event funnel (§23) |
| KPI-6 | Mobile Lighthouse performance | n/a | ≥ 90 | Automated in CI |
| KPI-7 | Bounce rate, project pages | n/a | \< 55% | Analytics platform (§23) |

> ⚠️ **Targets are provisional.** KPI-1 and KPI-3 depend on `[REGION]` market size and competitive density, which have not been researched. These must be revisited once the client's city is confirmed. Do not treat them as commitments.

**Definition of "qualified enquiry"** — a submission with a named commercial organisation, a project type, and either a stated budget band or a stated timeline. Anything else is unqualified and should not count against KPI-1. The form is designed in Part 2 to enforce this.

---

## 6\. Target Audience

### 6.1 Buyer prioritisation and rationale

Three buyer groups were identified. They are **not equal**, and the site must not treat them as such — designing for all three simultaneously produces a site that converts none of them.

| Priority | Segment | Why this ranking |
| :---- | :---- | :---- |
| **Primary** | Corporate facility & office managers | Highest volume. Actively search online. Will submit a web enquiry cold. Shortest path from site visit to revenue. **The homepage is built for this person.** |
| **Secondary** | Property developers & landlords | Higher contract value and repeat potential, but a smaller pool and more relationship-driven. Served by a dedicated landing page and ROI-oriented content, not by the homepage. |
| **Tertiary** | Architects & main contractors | **Do not buy from websites.** They award subcontracts on relationships, prior job performance, and credentials. Targeting them with the homepage wastes the homepage. Served by a Capabilities page — drawing standards, insurance, certifications, references — reachable from the footer and linkable directly during a pitch. |

> 💡 **Recommendation:** Retail and hospitality were deliberately excluded. Adding them fragments the positioning and dilutes the workplace-specialist story that differentiates the firm. Revisit only if the client's actual pipeline contradicts this.

### 6.2 Service positioning

The client offers four services. Presenting them as four equal peers reads as unfocused generalism — the most common positioning failure among small design firms.

**Recommended hierarchy:**

HEADLINE OFFER

└── Full Turnkey Fit-Out

    │   "One firm, one contract, one point of responsibility,

    │    from first sketch to handover."

    │

    ├── Interior Design & Space Planning   (capability)

    ├── FF\&E Curation & Procurement        (capability)

    └── Workplace Strategy & Consulting    (capability \+ top-of-funnel hook)

**Reasoning:**

- **Turnkey is the differentiator.** The buyer's genuine fear is co-ordinating a designer, a contractor, and a furniture supplier who blame each other when things slip. Single-point responsibility directly answers that fear. Design-only competitors cannot claim it.  
- **Design-only must remain purchasable** — some buyers already have a contractor — but it is presented as an entry point, not the headline.  
- **Workplace strategy is the top-of-funnel hook.** It is the service a buyer wants *before* they know they need a fit-out, making it the natural anchor for SEO content and the lowest-commitment first conversation.  
- **FF\&E is a margin service**, rarely sold standalone. Presented as included value.

---

## 7\. User Personas

### Persona 1 — Priya, Facility & Workplace Manager `PRIMARY`

|  |  |
| :---- | :---- |
| **Role** | Facility Manager, 180-person professional services firm |
| **Reports to** | COO |
| **Trigger** | Lease renewal in 9 months; hybrid working has left 40% of desks empty daily |
| **Budget authority** | Recommends; COO and CFO approve |
| **Frequency** | Has never run a fit-out before |

**Goals**

- Reconfigure the floor for hybrid working without moving premises  
- Deliver with minimal disruption to a working office  
- Present a defensible recommendation to the COO and CFO

**Pain points & fears**

- Has no benchmark for what this should cost, and fears being overcharged  
- Terrified of a project overrunning while 180 people work around it  
- Cannot evaluate design competence — everything looks nice in photos  
- Will personally own the blame if it goes wrong

**Behaviour**

- Searches `office refurbishment [city]`, `hybrid office redesign`  
- Opens 5–7 firms in tabs, eliminates most within 30 seconds  
- Screenshots and pastes into a comparison deck for the COO  
- Researches on mobile in the evening, enquires from desktop next day

**What makes her enquire**

- Evidence of a project at similar headcount and sector  
- An explicit, honest statement of process and timeline  
- Any indication of budget range — even a broad one  
- Clear statement that the firm handles everything  
- A form that does not demand a phone call as the only option

**What makes her leave**

- Residential work mixed into the portfolio  
- No indication of scale on any project  
- "Contact us for a quote" as the only pricing signal  
- Slow-loading image galleries on mobile

---

### Persona 2 — Daniel, Commercial Property Developer `SECONDARY`

|  |  |
| :---- | :---- |
| **Role** | Development Manager, regional commercial landlord |
| **Portfolio** | 11 buildings, \~400,000 sqft |
| **Trigger** | Two floors vacant 14 months; needs fitted suites to let |
| **Budget authority** | Full, within approved capex |
| **Frequency** | Commissions fit-outs 3–5× per year |

**Goals**

- Reduce void periods by offering move-in-ready suites  
- Predictable cost per sqft  
- A repeatable partner he doesn't have to re-procure each time

**Pain points**

- Every void month is direct lost revenue  
- Bespoke design is irrelevant to him — lettability is everything  
- Has been burned by firms who over-designed and over-ran

**What makes him enquire**

- Cost-per-sqft indication  
- Speed and schedule reliability evidence  
- Spec suite / CAT A → CAT B experience specifically  
- Evidence of repeat client relationships

**Note:** Daniel is a *sophisticated repeat buyer*, unlike Priya. He needs less education and more commercial data. **He must not be served the same page as Priya** — hence a dedicated developer landing page in Part 2\.

---

### Persona 3 — Sarah, Project Architect `TERTIARY — SERVED, NOT TARGETED`

|  |  |
| :---- | :---- |
| **Role** | Project Architect, mid-size practice |
| **Trigger** | Needs an interiors subcontractor for a mixed-use scheme |
| **Budget authority** | Recommends to client |

**Goals**

- A subcontractor who produces coordinated drawings and doesn't create RFIs  
- Someone who will not embarrass her in front of her client

**What she needs from the site**

- Technical credentials: software used, drawing standards, BIM capability  
- Insurance and certification detail  
- Named references she can call  
- Evidence of working *under* an architect rather than competing with one

**Design implication:** Sarah arrives with intent, usually via a direct link shared during a conversation. She needs a dense, factual **Capabilities** page. She does not need — and will be mildly put off by — marketing language. Do not optimise the homepage for her.

---

## 8\. Scope & MVP Definition

### 8.1 MVP — launch scope

| \# | Page / Feature | Priority | Notes |
| :---- | :---- | :---- | :---- |
| 1 | Homepage | P0 | Built for Priya |
| 2 | Portfolio index | P0 | Filterable by sector and scale |
| 3 | Project detail template (× 4–6 projects) | P0 | The core conversion asset |
| 4 | Services — Turnkey Fit-Out | P0 | Headline offer |
| 5 | Services — Design, FF\&E, Workplace Strategy | P0 | Three lighter pages |
| 6 | Our Process | P0 | Directly answers "will you disrupt my office" |
| 7 | About / Team | P0 | Credibility; who is accountable |
| 8 | Contact \+ qualifying enquiry form | P0 | KPI-1 depends entirely on this |
| 9 | Capabilities (for architects/contractors) | P1 | Serves Sarah; footer-linked |
| 10 | For Developers & Landlords | P1 | Serves Daniel |
| 11 | Privacy policy / cookie notice | P0 | Legal requirement |
| 12 | 404 page | P1 |  |
| 13 | **AI assistant (F-14)** | P1 | Added by client direction. Requires all P0 pages complete plus a ≥25-pair FAQ corpus before it can be grounded. See Part 3 §27.1 decision record. |

### 8.2 Deliberately deferred to Phase 2

| Feature | Why deferred |
| :---- | :---- |
| CMS / self-editing | Client is not editing; adds cost and complexity for no launch-day value |
| Blog / insights | Requires sustained content commitment nobody has agreed to staff |
| Case study PDF downloads | Valuable for B2B, but needs the projects to exist first |
| Testimonials with named clients | Requires client permission — a process, not a build task |
| Interactive 3D / virtual tours | High cost, unproven return at this scale |
| Multi-language | No stated need |
| Live chat | Requires someone to answer it |

### 8.3 Definition of Done for MVP

The MVP ships only when **all** of the following are true:

- [ ] All P0 pages built, populated with real content — no lorem ipsum, no placeholder images  
- [ ] Minimum **four** real projects published with genuine imagery  
- [ ] Enquiry form delivers to a monitored inbox, verified end-to-end in production  
- [ ] Mobile Lighthouse ≥ 90 on performance, accessibility, best practices, SEO  
- [ ] WCAG 2.2 AA verified (see Part 3\)  
- [ ] Analytics and Search Console live and confirmed receiving data  
- [ ] Client has signed off on all copy  
- [ ] Site is responsive and verified on real devices, not just emulators

>   
> ⚠️ **Hard gate:** fewer than four real projects means the site should not launch. A commercial portfolio with two projects actively damages credibility — it reads as a firm with no track record. If the client cannot supply four, we change strategy (see §9, RISK-1), not the launch date.

---

## 9\. Assumptions & Open Questions

Every item here is a **decision the client owes us**. Each carries a consequence if it goes unanswered.

### 9.1 Blocking — must be resolved before design begins

| ID | Item | Assumption made | Consequence if wrong |
| :---- | :---- | :---- | :---- |
| **OQ-1** | **Client name, legal entity, location, service radius** | Placeholder `[CLIENT NAME]` / `[CITY]` used throughout | Blocks all copy, domain, and SEO work. Nothing real can be written. |
| **OQ-2** | **Brand identity** — none exists | **Recommendation:** derive identity from the three existing homepage samples (navy/gold, charcoal/blue, black/amber) rather than commission a full brand exercise. Faster, cheaper, sufficient for launch. | A full brand project is separate scope with separate cost and a 3–5 week timeline. If the client wants this, the website timeline extends accordingly. |
| **OQ-3** | **Number of projects with usable imagery** | Assumed 4–6 | Below four, MVP cannot launch as scoped (see RISK-1) |
| **OQ-4** | **Willingness to publish any budget indication** | Assumed a broad range ("typical projects £X–£Y") | Refusal materially reduces conversion. Priya's most-searched question goes unanswered. |

### 9.2 Non-blocking — needed before build

| ID | Item | Assumption made |
| :---- | :---- | :---- |
| OQ-5 | Domain owned? | Assumed not; \~£15/yr, registered in **client's** name, not the developer's |
| OQ-6 | Enquiry destination | Assumed a monitored client inbox; no CRM |
| OQ-7 | Client permission to name past clients | Assumed **not** granted — anonymised as "a 180-person professional services firm" |
| OQ-8 | Existing floor plans / drawings usable as content | Assumed yes — important, as these substitute for missing photography |
| OQ-9 | Sectors to emphasise | Assumed corporate office primary |
| OQ-10 | Team photos and bios available | Assumed yes; About page credibility depends on it |
| OQ-11 | Deadline | **None stated.** Assumed no hard deadline. |
| OQ-12 | Jurisdiction for privacy compliance | Assumed UK/EU GDPR. **If India or US, requirements differ** — affects Part 3\. |

### 9.3 Key risks carried into Part 3

| ID | Risk | Severity |
| :---- | :---- | :---- |
| **RISK-1** | Thin photography undermines credibility and conversion | **High** |
| **RISK-2** | No brand identity delays start and risks rework | **High** |
| **RISK-3** | Client cannot supply copy, stalling the project indefinitely | **High** — the most common cause of death for projects like this |
| **RISK-4** | Developer becomes an unpaid permanent dependency for edits | Medium |
| **RISK-5** | Unrealistic SEO expectations in a competitive local market | Medium |

Full risk register with mitigations and owners appears in Part 3\.

---

## 10\. Traceability

| Objective | Served by | Measured by |
| :---- | :---- | :---- |
| OBJ-1 Convert enquiries | Pages 1, 3, 8 | KPI-1, KPI-2, KPI-5 |
| OBJ-2 Credibility | Pages 3, 6, 7, 9 | KPI-7 |
| OBJ-3 Search visibility | All pages; Part 3 SEO | KPI-3, KPI-4 |
| OBJ-4 Internal selling | Pages 3, 6; Phase 2 PDF export | Qualitative |
| OBJ-5 Brand identity | Design System (Phase 4\) | Qualitative |
| OBJ-6 Low maintenance cost | Static architecture (TRD) | Hosting spend |

---

**End of Part 1\.**

**Next:** Part 2 — Information architecture, navigation, full feature specification with acceptance criteria, user stories, and user journeys.  
