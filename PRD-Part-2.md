# Product Requirements Document

## Commercial Interior Design Firm — Marketing & Lead Generation Website

**Part 2 of 3 — Functional Requirements**

Version 0.2 (Draft) · 4 August 2026 · Continues from `PRD-Part-1-Foundation.md`

---

## 11\. Information Architecture

### 11.1 Sitemap

HOME

│

├── WORK  (portfolio index — filterable)

│   ├── /work/\[project-slug\]          × 4–6 at launch

│   └── filters: sector · scale · service

│

├── SERVICES

│   ├── /services/turnkey-fit-out      ★ headline offer

│   ├── /services/interior-design

│   ├── /services/ffe-procurement

│   └── /services/workplace-strategy   ← top-of-funnel hook

│

├── PROCESS                            ← answers "will you disrupt my office"

│

├── ABOUT

│   └── team, credentials, accountability

│

├── CONTACT                            ← qualifying enquiry form

│

├── FOR DEVELOPERS       (P1, secondary nav / footer)   ← Daniel

├── CAPABILITIES         (P1, footer only)              ← Sarah

│

└── UTILITY

    ├── /privacy

    ├── /cookies

    └── /404

### 11.2 IA rationale

**Why "Work" and not "Portfolio" or "Projects".** Shortest, most scannable, and the industry-standard label buyers expect. Reduces cognitive load in the nav.

**Why Process gets top-level navigation.** This is the single most contrarian IA decision in the document. Most competitor sites bury process in an About page or omit it entirely. But Priya's dominant fear — operational disruption to a working office — is answered *only* here. Elevating it to primary nav is a direct competitive differentiation, and it costs nothing.

**Why Services is a dropdown, not a single page.** Four services on one page produces either a shallow page or an overwhelming one. Separate pages also give four SEO landing targets instead of one.

**Why "For Developers" and "Capabilities" are demoted.** Both serve real users (Daniel, Sarah) but neither is the primary buyer. Placing them in primary nav would dilute Priya's path. Both remain directly linkable — critical, because both are typically reached via a link shared in conversation, not by browsing.

**Why there is no "Blog" or "News".** Deferred (Part 1 §8.2). An empty or stale blog is worse than no blog — a "latest post: March 2024" signals a dead firm.

### 11.3 URL structure

| Page | URL | Notes |
| :---- | :---- | :---- |
| Home | `/` |  |
| Work index | `/work` | Filters via query params, e.g. `/work?sector=office` |
| Project | `/work/[slug]` | e.g. `/work/180-person-hybrid-office-refit` |
| Service | `/services/[slug]` |  |
| Process | `/process` |  |
| About | `/about` |  |
| Contact | `/contact` |  |
| Developers | `/for-developers` |  |
| Capabilities | `/capabilities` |  |

**Rules:** lowercase, hyphenated, no trailing slashes, no dates in slugs, no `.html` extensions. Slugs are permanent once published — changing one requires a 301 redirect (see Part 3).

---

## 12\. Navigation

### 12.1 Primary navigation (desktop)

\[LOGO\]        Work    Services ▾    Process    About         \[Start a project →\]

- Sticky on scroll, condensed height after 100px  
- **`Start a project`** is a visually distinct button, present in every viewport at all times. It is the primary conversion element on the site.  
- Services dropdown opens on hover (desktop) and on click/tap (touch), with the turnkey item visually emphasised as the headline offer

### 12.2 Mobile navigation

- Logo left, hamburger right  
- **`Start a project` remains visible in the collapsed bar** — it does not hide inside the menu. Burying the primary CTA behind a hamburger is a measurable conversion loss.  
- Full-screen overlay menu; Services expands as an accordion rather than a nested drawer  
- Menu is keyboard and screen-reader operable; focus is trapped while open and returned to the trigger on close

### 12.3 Footer

Four columns, collapsing to stacked accordions on mobile:

| Column 1 | Column 2 | Column 3 | Column 4 |
| :---- | :---- | :---- | :---- |
| Services (4 links) | Work · Process · About | **For Developers** **Capabilities** Contact | Address, phone, email Company reg. no. Social links Privacy · Cookies |

### 12.4 Cross-linking rules

These matter more than the nav itself, because most conversion happens through in-page links:

| From | Must link to |
| :---- | :---- |
| Every project page | 2 related projects · the relevant service page · CTA band |
| Every service page | ≥ 2 projects demonstrating that service · Process · CTA band |
| Process page | Turnkey fit-out · CTA band |
| Homepage | 3 featured projects · all 4 services · Process · About |
| Every page (bottom) | Global CTA band (F-11c) |

---

## 13\. Feature Specifications

Priority scale: **P0** \= MVP blocker · **P1** \= launch-desirable · **P2** \= post-launch

---

### F-01 · Homepage `P0`

**Description** The primary entry point, built exclusively for Priya (Persona 1). A single-column narrative that moves a cold visitor from "who are these people" to "I'll enquire" in one scroll.

**Section order and reasoning**

| \# | Section | Purpose |
| :---- | :---- | :---- |
| 1 | Hero — one full-bleed image, headline naming the buyer and the outcome, single CTA | Establishes relevance in under 5 seconds |
| 2 | Proof strip — client logos *or*, if unavailable, quantified stats ("14 fit-outs · 180,000 sqft · zero overruns") | Immediate credibility. **Stats fallback is deliberate — it works without photography or client permission.** |
| 3 | The turnkey proposition — single point of responsibility, 3 supporting points | The core differentiator |
| 4 | Featured work — 3 projects, each with sector \+ headcount \+ sqft visible | Answers "have you done work like mine" |
| 5 | Services — 4 cards, turnkey visually dominant | Routes visitors to depth |
| 6 | Process preview — 5 stages, condensed, linking to `/process` | Pre-empts the disruption fear |
| 7 | Testimonial — one, with role and company type even if anonymised | Third-party validation |
| 8 | CTA band | Conversion |

**User value** — Quickly determines whether this firm is relevant to their project. **Business value** — Primary organic landing page; sets positioning; feeds every other page. **Dependencies** — Brand identity (OQ-2), 3 projects with hero imagery (OQ-3), one testimonial (OQ-7).

**Acceptance criteria**

- [ ] Hero renders above the fold on 375px width without scrolling to see the CTA  
- [ ] Largest Contentful Paint ≤ 2.0s on 4G mobile  
- [ ] Hero image served as AVIF with WebP and JPEG fallbacks, responsive `srcset`  
- [ ] All eight sections present and populated with real content  
- [ ] Every featured project shows sector, headcount, and floor area  
- [ ] Proof strip degrades gracefully to the stats variant if no client logos are permitted  
- [ ] Single `<h1>`; heading hierarchy descends without skipping levels  
- [ ] Fully operable by keyboard; visible focus states throughout

---

### F-02 · Work Index (Portfolio) `P0`

**Description** A filterable grid of all published projects. The filter is the important part: it lets Priya self-select to projects resembling hers, which is precisely the "have you done work like mine" question.

**Filter dimensions**

| Filter | Values | Rationale |
| :---- | :---- | :---- |
| Sector | Corporate office · Professional services · Tech · Healthcare · Education · Other | Sector relevance is the strongest credibility signal |
| Scale | \< 5,000 sqft · 5–15,000 · 15,000+ | Scale mismatch is a top elimination reason |
| Service | Turnkey · Design only · FF\&E · Workplace strategy | Routes to service pages |

**Behaviour**

- Client-side filtering — no page reload, instant response  
- Filter state reflected in the URL query string so results are shareable  
- Multiple filters combine with AND  
- Empty result state offers "show all" and a link to Contact rather than a dead end  
- **With fewer than 8 projects, filters render as a simple pill row, not a sidebar** — a sidebar filter over 5 items looks absurd and signals a thin portfolio

**User value** — Finds relevant proof in seconds instead of scanning everything. **Business value** — Increases project-page depth, which correlates directly with KPI-5. **Dependencies** — Project data model (F-03), minimum 4 projects.

**Acceptance criteria**

- [ ] Filtering completes in \< 100ms with no layout shift (CLS contribution 0\)  
- [ ] Filter state is encoded in and restorable from the URL  
- [ ] Filters are `<button>` or `<input>` elements, keyboard-operable, with `aria-pressed` state  
- [ ] Result count is announced to screen readers via a live region  
- [ ] Cards display sector, floor area, and headcount without requiring a click  
- [ ] Grid is responsive: 3 columns desktop / 2 tablet / 1 mobile  
- [ ] Images lazy-load below the fold with explicit width and height set  
- [ ] Empty state provides a recovery path  
- [ ] Works with JavaScript disabled — all projects visible, filters progressively enhanced

---

### F-03 · Project Detail Page `P0` ★ **Most important page on the site**

**Description** The core conversion asset. Every other page exists to get a visitor here. Deliberately structured so that **credibility comes from information, not from image volume** — the direct mitigation for RISK-1.

**Required content model**

| Field | Type | Required | Purpose |
| :---- | :---- | :---- | :---- |
| Title | string | ✅ | Descriptive, not clever |
| Client | string or anonymised descriptor | ✅ | "A 180-person professional services firm" if unnamed |
| Sector | enum | ✅ | Filter \+ relevance |
| Floor area (sqft) | number | ✅ | Scale signal |
| Headcount | number | ✅ | Scale signal Priya recognises |
| Duration | string | ✅ | **Answers the disruption fear** |
| Occupied during works? | boolean | ✅ | **Highest-value single field on the site** |
| Services delivered | enum\[\] | ✅ | Cross-link to services |
| Challenge | rich text | ✅ | Narrative |
| Approach | rich text | ✅ | Demonstrates thinking |
| Outcome | rich text \+ metrics | ✅ | Quantified where possible |
| Hero image | image | ✅ |  |
| Gallery | image\[\] | ⬜ | 3–12; page must work with 1 |
| Floor plan / drawing | image | ⬜ | **High value — substitutes for photography** |
| Before images | image\[\] | ⬜ | Before/after outperforms after-only |
| Materials / FF\&E detail | image\[\] | ⬜ | Close-ups; easy to shoot, high perceived quality |
| Testimonial | quote \+ attribution | ⬜ |  |

> 💡 **Design directive:** the template must look *complete and considered* with one hero image, a floor plan, and three detail shots. Any layout that looks broken or sparse without twelve photographs has failed the brief. This is the single most important design constraint in the project.

**User value** — Answers all five of Priya's questions in one page. **Business value** — Directly drives KPI-5; the primary internal-sharing artefact for OBJ-4. **Dependencies** — Real project data (OQ-3), imagery, client permission (OQ-7).

**Acceptance criteria**

- [ ] Template renders correctly and attractively with the minimum asset set (1 hero \+ 1 plan \+ 3 details)  
- [ ] All required fields display; optional fields degrade without leaving empty containers or gaps  
- [ ] "Occupied during works" renders as a prominent, scannable fact — not buried in prose  
- [ ] Duration and floor area visible without scrolling past the hero  
- [ ] Gallery is keyboard-navigable; lightbox traps focus and closes on Escape  
- [ ] Every image has meaningful alt text describing the space, not the filename  
- [ ] Two related projects shown at the foot  
- [ ] Services delivered link to their service pages  
- [ ] CTA band present  
- [ ] `Project` / `CreativeWork` structured data emitted  
- [ ] Page weight ≤ 1.5MB on initial load with images lazy-loaded

---

### F-04 · Turnkey Fit-Out Service Page `P0` ★ Headline offer

**Description** The most commercially important service page. Argues single-point responsibility against the fragmented alternative.

**Required sections**

1. What turnkey means here — plain-language definition  
2. **Comparison table: turnkey vs. multi-vendor** (see below)  
3. What's included — end to end  
4. Typical timeline by project size  
5. **Indicative investment bands** (subject to OQ-4)  
6. 2–3 turnkey projects  
7. FAQ (5–8 questions)  
8. CTA

**Comparison table — the persuasive core of the page**

|  | Turnkey (us) | Designer \+ contractor \+ supplier |
| :---- | :---- | :---- |
| Contracts to manage | 1 | 3–5 |
| Single point of accountability | ✅ | ❌ |
| Who owns a delay | Us | Disputed |
| Cost certainty | Fixed at design sign-off | Variable |
| Your time commitment | Low | High |

**User value** — Understands what they're buying and why it de-risks their project. **Business value** — Highest-margin service; the core differentiator.

**Acceptance criteria**

- [ ] Comparison table present and responsive (converts to stacked cards below 640px)  
- [ ] Timeline covers at least three project sizes  
- [ ] Investment guidance present, or a documented client decision recorded against OQ-4  
- [ ] ≥ 2 linked projects delivered as turnkey  
- [ ] FAQ marked up with `FAQPage` structured data  
- [ ] Comparison table uses semantic `<table>` with `<th scope>` — not divs

---

### F-05 · Secondary Service Pages (× 3\) `P0`

Interior Design · FF\&E Procurement · Workplace Strategy.

**Shared template:** what it is → who it's for → what you get (deliverables) → how it works → related projects → CTA.

**Page-specific requirements**

| Page | Distinct requirement |
| :---- | :---- |
| Interior Design | Must state clearly that design-only is available, and note that most clients upgrade to turnkey — a soft upsell, not a hard one |
| FF\&E Procurement | Emphasise supplier relationships and lead-time management; lead times are a real client pain |
| Workplace Strategy | **Top-of-funnel hook.** Lowest-commitment entry. Should carry a softer secondary CTA ("book a workplace review") alongside the standard one |

**Acceptance criteria**

- [ ] All three follow the shared template for visual consistency  
- [ ] Each links to ≥ 2 relevant projects  
- [ ] Each has a unique meta title and description  
- [ ] Workplace Strategy carries the soft secondary CTA  
- [ ] No duplicate copy between pages (SEO cannibalisation risk)

---

### F-06 · Process Page `P0` ★ Differentiator

**Description** A stage-by-stage account of how a project runs, written to defuse Priya's operational fears. Most competitors do this badly or not at all.

**Five stages**

| Stage | Content required |
| :---- | :---- |
| 1 · Discovery & brief | What we ask, what we need from you, duration |
| 2 · Concept & space planning | Deliverables, revision rounds, sign-off gate |
| 3 · Detailed design & costing | **Where cost is fixed** — critical trust moment |
| 4 · Build & installation | **Disruption management: phasing, out-of-hours working, dust/noise control, live-office protocols** |
| 5 · Handover & aftercare | Snagging, warranty period, support |

**Each stage must state:** what happens · what we need from the client · typical duration · what is signed off before proceeding.

**Business value** — Genuine differentiation at near-zero production cost. Also the highest-value page to link to during a sales conversation.

**Acceptance criteria**

- [ ] All five stages documented against the four-point structure  
- [ ] Stage 4 explicitly addresses working in an occupied office  
- [ ] Stage 3 explicitly states when and how cost becomes fixed  
- [ ] Visual timeline component is responsive and accessible (not image-only; readable as text)  
- [ ] Client-responsibility items are visually distinguished from firm-responsibility items  
- [ ] Links to Turnkey service page and CTA band

---

### F-07 · About / Team `P0`

**Description** Establishes who is accountable. B2B buyers of five-figure services want to know who they'll actually deal with.

**Required:** firm story (concise — nobody reads three paragraphs of founding history) · team members with real photos, names, roles, and relevant experience · credentials, accreditations, memberships, insurance · geographic coverage · optional company timeline.

**Acceptance criteria**

- [ ] Real team photographs — **no stock imagery of people under any circumstances**  
- [ ] Each member has name, role, and one line of relevant experience  
- [ ] Credentials and insurance stated  
- [ ] Service area stated explicitly (supports local SEO)  
- [ ] `Organization` structured data emitted  
- [ ] Renders acceptably for a team of one, three, or ten

>   
> ⚠️ Stock photos of models in an office are the fastest way to destroy B2B credibility. If real photos are unavailable, use no photos and lean on named bios instead.

---

### F-08 · Enquiry Form `P0` ★ KPI-1 depends entirely on this

**Description** A short qualifying form. Every field is justified below — each additional field measurably reduces completion, so nothing is included "because it's useful to have."

**Field specification**

| \# | Field | Type | Required | Justification |
| :---- | :---- | :---- | :---- | :---- |
| 1 | Name | text | ✅ |  |
| 2 | Company | text | ✅ | Filters consumer/residential enquiries |
| 3 | Email | email | ✅ |  |
| 4 | Phone | tel | ⬜ | Optional — forcing it suppresses submissions |
| 5 | Project type | select | ✅ | Routes the enquiry |
| 6 | Approximate size | select (sqft bands) | ✅ | Qualification |
| 7 | Timeline | select | ✅ | Qualification \+ prioritisation |
| 8 | Budget band | select, incl. "not sure yet" | ⬜ | Optional with an honest escape hatch — mandatory budget fields are a known conversion killer |
| 9 | Message | textarea | ⬜ |  |
| 10 | Consent | checkbox | ✅ | GDPR — unticked by default, never pre-checked |

**Anti-spam:** honeypot field \+ time-to-submit threshold. **No CAPTCHA** — it adds friction and accessibility barriers for a low-volume B2B form where spam is manageable manually.

**Acceptance criteria**

- [ ] Submission delivers to the monitored inbox, verified in production before launch  
- [ ] Inline validation on blur, not only on submit  
- [ ] Errors are specific ("Enter a valid email address"), not generic  
- [ ] Errors are linked to inputs via `aria-describedby` and announced via a live region  
- [ ] Every input has a persistently visible `<label>` — **placeholders are not labels**  
- [ ] Success state confirms receipt and states an expected response time  
- [ ] Failure state preserves entered data and offers a mailto fallback  
- [ ] Consent checkbox unticked by default, with a privacy policy link  
- [ ] Fully keyboard-operable; logical tab order  
- [ ] Submission fires an `enquiry_submitted` analytics conversion event (§23.2)  
- [ ] Honeypot and timing checks active and verified against a scripted submission

---

### F-09 · Capabilities Page `P1` — serves Sarah

**Description** A dense, factual, low-marketing page for architects and main contractors. Deliberately different in tone from the rest of the site.

**Required:** software and drawing standards (CAD/Revit/BIM level) · deliverable formats and schedules · insurance (PI, PL, EL) with cover levels · certifications and accreditations · H\&S record and CDM competence · sub-contract experience and typical scope splits · named references available on request · downloadable capability statement (PDF).

**Acceptance criteria**

- [ ] Insurance types and cover levels stated  
- [ ] Software and drawing standards stated explicitly  
- [ ] Tone is factual — no marketing superlatives  
- [ ] Downloadable PDF available and under 5MB  
- [ ] Directly linkable and indexable  
- [ ] Reachable from the footer

---

### F-10 · For Developers & Landlords `P1` — serves Daniel

**Description** A commercially-framed landing page for repeat, ROI-driven buyers.

**Required:** void-period and lettability framing · CAT A / CAT B experience · cost per sqft indication · speed and programme reliability evidence · framework/repeat arrangement offer · relevant projects.

**Acceptance criteria**

- [ ] Cost-per-sqft guidance present, or OQ-4 decision documented  
- [ ] CAT A / CAT B terminology used correctly — misuse here destroys credibility instantly with this audience  
- [ ] Framing is commercial (voids, yield, programme), not aesthetic  
- [ ] Links to ≥ 2 relevant projects  
- [ ] Distinct CTA copy from the main site CTA

---

### F-11 · Global Components `P0`

| ID | Component | Key requirements |
| :---- | :---- | :---- |
| F-11a | Header | Sticky, condenses on scroll, persistent CTA, accessible dropdown, focus-trapped mobile menu |
| F-11b | Footer | Four columns → stacked mobile; full legal and contact detail |
| F-11c | **CTA band** | Appears at the foot of every page. Headline \+ supporting line \+ primary button. **Copy varies by page context** — a generic "Get in touch" on every page is a wasted conversion slot |
| F-11d | Image component | AVIF/WebP/JPEG, responsive `srcset`, lazy-load below fold, explicit dimensions to prevent CLS, LQIP blur placeholder, mandatory alt text |
| F-11e | Project card | Image, title, sector, sqft, headcount; entire card is one link target |
| F-11f | Section heading | Consistent eyebrow \+ heading \+ intro pattern |
| F-11g | Skip link | "Skip to main content", first focusable element on every page |
| F-11h | AI assistant launcher | ≤2KB button, bottom-right, on every page. Never auto-opens. Dismissible for the session. Must not obscure the sticky CTA or any form field at 375px. Loads the F-14 bundle only on activation. |

---

### F-12 · Utility Pages `P0` / `P1`

| Page | Priority | Requirements |
| :---- | :---- | :---- |
| Privacy policy | P0 | Jurisdiction-appropriate (OQ-12); covers form data, analytics, retention, rights |
| Cookie notice | P0 | Consent banner only if non-essential cookies are set; **prefer cookieless analytics and avoid the banner entirely** |
| 404 | P1 | Branded, links to Work / Services / Contact, no dead end |

---

### F-14 · AI Assistant (Chatbot) `P1`

**Description** A grounded, retrieval-based AI assistant that answers visitor questions using **only** the firm's own published content, and routes qualified interest into the enquiry pipeline. It is a *concierge to the site*, not a salesperson and not a substitute for a human.

> 📌 **Decision record.** This feature was initially excluded (Part 3 §27.1) and added by client direction. The trade-offs identified during that assessment are not discarded — they are converted into the mandatory constraints below. The spec is written so that if the assistant cannot meet these constraints, it does not ship.  
>   
> ⚠️ **F-13 and F-14 overlap.** Both exist to lower the friction of starting an enquiry. **Build one, not both.** Recommendation: if F-14 ships, drop F-13 — a guided form behind a chat launcher is redundant, and two competing entry points fragment the conversion data. Revisit F-13 only if F-14 is later removed.

---

#### 14.1 Scope of competence

**The assistant may answer:**

| Topic | Source of truth |
| :---- | :---- |
| What services the firm offers | Service pages (F-04, F-05) |
| How the process works, stage by stage | Process page (F-06) |
| Whether a past project resembles the visitor's | Project pages (F-03) — metadata and narrative |
| Typical durations and whether work can be done in an occupied office | Project metadata, Process stage 4 |
| Who the team are and what credentials they hold | About (F-07), Capabilities (F-09) |
| Service area and coverage | About (F-07) |
| Published investment bands, if OQ-4 permits | Service pages |
| How to get in touch, and what happens next | Contact (F-08) |

**The assistant must refuse, and hand off:**

| Request | Required behaviour |
| :---- | :---- |
| A price, quote, or estimate for the visitor's specific project | Decline to estimate. State that cost depends on scope, and offer the enquiry form or a call. Quote a published band **only if** it exists verbatim on the site. |
| A commitment to a date, deadline or availability | Decline. Offer handoff. |
| Confirmation of experience in a sector not in the published portfolio | State plainly what the portfolio does contain. **Never infer or extrapolate capability.** |
| Contractual, warranty, insurance or liability interpretation | Decline. Offer handoff. |
| Anything not covered by the source content | "I don't have that — let me put you in touch with the team," plus handoff. |
| Legal, planning, building-regs or H\&S advice | Decline. Offer handoff. |

> ⚠️ **The single largest risk in this feature is confident invention.** An assistant that says "yes, we've done healthcare fit-outs" when the portfolio contains none creates a claim the firm must either honour or retract, in front of a buyer who is already risk-averse. Every guardrail below exists to prevent that one failure.

---

#### 14.2 Grounding and content rules

| Rule | Specification |
| :---- | :---- |
| Knowledge source | **Retrieval over the site's own published pages only.** No open-web knowledge, no training on external corpora. |
| Index build | Regenerated at deploy time from the same content that renders the pages — the index can never drift from what the site says |
| Answer construction | Every substantive answer must be supported by retrieved content. If retrieval returns nothing relevant, the assistant refuses and hands off. |
| Citation | Every answer links to the page it came from — *"You can read the full process here →"*. This is a trust mechanism **and** drives traffic to the pages that actually convert. |
| Tone | Plain, factual, brief. No superlatives, no sales pressure, no simulated enthusiasm. |
| Identity | **Discloses that it is an AI assistant on first message.** Never adopts a human name or persona, and never claims to be a member of the team. |
| Uncertainty | Expresses uncertainty explicitly rather than hedging fluently. "I'm not sure" is a correct answer. |
| Length | Answers ≤ 80 words by default, with a link for depth |

---

#### 14.3 Human handoff and lead capture

Handoff is the point of the feature — the conversation is a means to an enquiry, not an end in itself.

**Handoff triggers:** any refusal category above · the visitor asks for a person · two consecutive low-confidence retrievals · the visitor expresses buying intent ("we're looking at a refurb in Q1") · the visitor asks the same thing twice.

**Handoff behaviour**

1. Acknowledge the limit honestly  
2. Offer two routes: the enquiry form (pre-populated with what the conversation already established — project type, size, timeline) and the phone number  
3. Never loop. **Two failed attempts on the same question ends the automated conversation and offers only human contact.**  
4. State the firm's actual response time; never invent one

**Out-of-hours** — the assistant states normal response hours and sets expectations. It must not imply anyone is monitoring the chat live.

**Lead capture** — a conversation that reaches the enquiry form counts toward KPI-1 only if it produces a submission meeting the qualified-enquiry definition (Part 1 §5). A transcript alone is not a lead.

---

#### 14.4 Build vs. buy

**Option A — Off-the-shelf vendor** (Intercom Fin, Tidio, Chatbase, Voiceflow and similar)

| Pros | Cons |
| :---- | :---- |
| Live in days | 150–300KB third-party JS — **breaks the §18.2 budget** |
| No maintenance burden | Sets cookies — **forces the consent banner avoided in §22** |
| Built-in dashboard and transcripts | Accessibility typically below WCAG 2.2 AA — **jeopardises the §19 claim** |
| Predictable monthly cost | Limited control over refusal behaviour and grounding |
|  | £40–100/mo indefinitely |
|  | Visitor data processed by a third party; adds a processor to the privacy policy |

*Best when:* speed matters more than performance, accessibility, and control.

**Option B — Custom, self-hosted widget \+ serverless LLM endpoint** ✅ **Recommended**

| Pros | Cons |
| :---- | :---- |
| \~10–15KB widget, lazy-loaded on click — **meets the §18.2 budget** | 3–5 days build effort |
| No cookies, same-origin — **preserves the cookieless setup** | Ongoing maintenance is yours (RISK-4) |
| Full control of accessibility — **can meet WCAG 2.2 AA** | Requires prompt and retrieval tuning |
| Full control of grounding, refusals and tone | Usage-based cost is variable, though small |
| \~£5–20/mo at projected volume | Needs monitoring for quality drift |
| One API processor, not a full third-party widget |  |

*Best when:* the non-functional requirements in Part 3 are genuine commitments rather than aspirations.

**Option C — Scripted decision tree, no LLM**

| Pros | Cons |
| :---- | :---- |
| Zero hallucination risk | Cannot handle unanticipated questions |
| \~5KB, trivially accessible | Visitors recognise it as a menu and disengage |
| Near-zero running cost | Delivers little beyond good navigation |

*Best when:* the real goal is qualification rather than conversation — in which case this is F-13.

> 💡 **Recommendation: Option B.** It is the only option that can satisfy the performance, accessibility and privacy requirements already committed to in Part 3, and at 250–600 sessions/month the usage cost is lower than any vendor subscription. Option A should only be chosen if the client explicitly accepts relaxing §18.2, §19 and §22 — and that acceptance should be recorded in writing.

---

#### 14.5 Non-functional constraints

These are **pass/fail**. An implementation that misses any of them does not ship.

| Area | Requirement |
| :---- | :---- |
| **Load behaviour** | Widget JS is **not** loaded on page load. Only a ≤2KB launcher button is present initially; the assistant loads on first click. |
| **Performance** | Zero measurable impact on LCP, CLS or INP. Verified in CI against Part 3 §18.2 budgets. |
| **Auto-open** | **Never.** No timer, no scroll trigger, no exit intent. The visitor opens it or it stays shut. |
| **Placement** | Bottom-right launcher, never obscuring the persistent CTA or any form field, and dismissible for the session |
| **Cookies** | None. Session state in memory; conversation ID in `sessionStorage` at most. |
| **Accessibility** | WCAG 2.2 AA. Keyboard-operable; focus moves into the panel on open and returns to the launcher on close; focus trapped while open; Escape closes; new messages announced via `aria-live="polite"`; launcher ≥ 44×44px; readable at 200% zoom. |
| **Reduced motion** | Respects `prefers-reduced-motion` — no typing animations or bounce effects |
| **No-JS** | Launcher absent entirely. All content and the enquiry form remain fully reachable. |
| **Mobile** | Must not cover content or the sticky CTA on a 375px viewport |
| **Rate limiting** | Per-session message cap and per-IP request limits to bound cost and abuse |
| **Latency** | First token ≤ 2s; visible thinking state until then |
| **Fallback** | On API failure, degrade to a static message offering the form and phone number — **never a broken or silent widget** |

---

#### 14.6 Privacy and data handling

| Requirement | Specification |
| :---- | :---- |
| Disclosure | AI status disclosed on first message, before any input |
| Notice | Link to the privacy policy visible in the chat panel |
| PII | Assistant must not solicit personal data in free text — it hands off to the form, which has proper consent (F-08) |
| Transcript retention | Defined, limited period, stated in the privacy policy |
| Processor | LLM provider named in the privacy policy as a data processor |
| Training | Provider must be configured **not** to train on submitted data; confirm contractually |
| Jurisdiction | Data residency confirmed against OQ-12 |
| Right to erasure | Transcripts deletable on request |
| Sensitive input | If a visitor volunteers personal or commercially sensitive information, the assistant does not repeat it back and does not persist it beyond the session |

---

#### 14.7 Content dependency

The assistant is only as good as what it retrieves. **A thin content base produces a thin or inventive assistant** — which is the failure mode this spec exists to prevent.

**Required before launch:**

- [ ] All P0 pages published with final copy  
- [ ] ≥ 4 project pages with complete metadata  
- [ ] Process page complete with all five stages  
- [ ] **A dedicated FAQ corpus of ≥ 25 question/answer pairs**, written by the client and covering cost drivers, timelines, disruption, sectors, coverage, guarantees and what happens after handover

>   
> 💡 The FAQ corpus is the highest-value item in this spec. It should be published as visible FAQ content on the relevant pages *as well as* indexed for the assistant — that way it earns long-tail search traffic instead of being locked inside a widget. **This directly resolves the SEO objection raised in Part 3 §27.1.**

---

#### 14.8 Value and priority

**User value** — Immediate answers to specific questions at 11pm without waiting for a reply; lowers the effort of an early, low-commitment enquiry. **Business value** — Captures visitors who would otherwise leave without enquiring; conversation logs are genuine market intelligence, revealing the questions buyers actually ask. **Priority rationale** — P1, not P0. The site must be complete and correct before an assistant can be grounded on it. Shipping it alongside launch means grounding it on unfinished content. **Dependencies** — F-01 to F-09 complete · FAQ corpus (§14.7) · OQ-4 (pricing disclosure) · OQ-12 (jurisdiction) · vendor/build decision (§14.4).

---

#### 14.9 Acceptance criteria

**Grounding and safety** — the critical set

- [ ] Answers only from indexed site content; refuses when retrieval returns nothing relevant  
- [ ] **Adversarial test suite of ≥ 30 prompts passes with zero fabrications**, covering: price requests, unlisted sectors, deadline commitments, capability claims, competitor comparisons, and prompt-injection attempts  
- [ ] Never states a price not published verbatim on the site  
- [ ] Never claims sector experience absent from the portfolio  
- [ ] Never commits to a date or availability  
- [ ] Discloses AI status in the first message  
- [ ] Every substantive answer cites and links its source page  
- [ ] Two failed attempts on one question terminate the automated flow and offer human contact  
- [ ] Prompt-injection attempts fail safely and do not alter refusal behaviour

**Performance**

- [ ] Launcher ≤ 2KB; assistant bundle loads only on interaction  
- [ ] Zero change to LCP, CLS and INP versus the same page without the assistant, verified in CI  
- [ ] First token ≤ 2s at p75; thinking state visible until first token

**Accessibility**

- [ ] Full keyboard operation; focus enters the panel on open, returns to launcher on close, trapped while open  
- [ ] Escape closes; new messages announced via a polite live region  
- [ ] axe-core reports zero violations with the panel open  
- [ ] Verified with VoiceOver and NVDA  
- [ ] Launcher ≥ 44×44px; usable at 200% zoom  
- [ ] Respects `prefers-reduced-motion`

**Behaviour and privacy**

- [ ] Never opens automatically under any condition  
- [ ] Dismissible; stays dismissed for the session  
- [ ] Does not obscure the sticky CTA or any form field at 375px  
- [ ] Sets no cookies  
- [ ] Privacy policy linked in-panel and updated to name the processor and retention period  
- [ ] Provider confirmed not to train on submitted data  
- [ ] API failure degrades to a static fallback offering the form and phone

**Measurement**

- [ ] Events emitted: `chat_opened`, `chat_message_sent`, `chat_handoff_offered`, `chat_to_form`, `chat_abandoned`, `chat_no_answer`  
- [ ] `chat_no_answer` logs the question text for FAQ improvement — **the most operationally useful output of the whole feature**  
- [ ] Chat-attributed enquiries distinguishable from direct form submissions  
- [ ] Monthly review of unanswered questions feeds the FAQ corpus

---

## 14\. User Stories

Format: `As a [persona], I want [capability], so that [outcome].`

### Epic A — Establishing relevance

| ID | Story | Acceptance |
| :---- | :---- | :---- |
| US-A1 | As Priya, I want to know within seconds whether this firm does commercial work, so I don't waste time on a residential decorator | Given I land on the homepage, when the hero renders, then commercial workplace focus is stated in the `<h1>` or subhead |
| US-A2 | As Priya, I want to filter projects by size and sector, so I can find work resembling mine | Given ≥ 4 projects, when I select a sector filter, then only matching projects show and the URL updates |
| US-A3 | As Priya, I want to see headcount and floor area on every project, so I can judge scale without opening each one | Given the work index, when cards render, then each shows sector, sqft and headcount |
| US-A4 | As Daniel, I want commercially-framed content about voids and yield, so I can judge fit fast | Given `/for-developers`, when it loads, then void reduction and cost per sqft appear above the fold |

### Epic B — Building confidence

| ID | Story | Acceptance |
| :---- | :---- | :---- |
| US-B1 | As Priya, I want to know whether a project was delivered in an occupied office, so I can judge disruption risk | Given a project page, when it loads, then "occupied during works" is displayed as a prominent labelled fact |
| US-B2 | As Priya, I want a stage-by-stage process with durations, so I can plan and defend a timeline internally | Given `/process`, when it loads, then five stages each show duration, client responsibilities and sign-off gate |
| US-B3 | As Priya, I want some indication of cost, so I know whether to continue | Given a service page, when it loads, then an indicative band or a clear explanation of how cost is established appears |
| US-B4 | As Priya, I want to see who I'd actually be working with, so I know who's accountable | Given `/about`, when it loads, then named team members with real photos and roles are shown |
| US-B5 | As Sarah, I want insurance, software and drawing standards, so I can qualify them as a subcontractor | Given `/capabilities`, when it loads, then insurance cover levels and drawing standards are stated |

### Epic C — Converting

| ID | Story | Acceptance |
| :---- | :---- | :---- |
| US-C1 | As Priya, I want to enquire without a phone call, so I can act outside working hours | Given any page, when I click the persistent CTA, then I reach a form requiring no phone number |
| US-C2 | As Priya, I want to know when I'll hear back, so I'm not left uncertain | Given a successful submission, when the success state renders, then an expected response time is stated |
| US-C3 | As Priya, I want to share a project with my COO, so I can build internal support | Given a project page, when I copy the URL, then it resolves to that project with correct social preview metadata |
| US-C4 | As the client, I want enquiries pre-qualified, so I don't waste time on unsuitable leads | Given a submission, when it arrives, then it contains company, project type, size band and timeline |

### Epic D — Access and performance

| ID | Story | Acceptance |
| :---- | :---- | :---- |
| US-D1 | As a keyboard user, I want to operate the whole site without a mouse | Given any page, when I tab through, then all interactive elements are reachable with visible focus and no traps |
| US-D2 | As a screen-reader user, I want images described meaningfully | Given any page, when images load, then each has descriptive alt text or is correctly marked decorative |
| US-D3 | As a mobile user on 4G, I want pages to load quickly | Given a project page on 4G, when I navigate to it, then LCP ≤ 2.5s |
| US-D4 | As a user with reduced-motion preferences, I want animation suppressed | Given `prefers-reduced-motion: reduce`, when pages render, then non-essential animation is disabled |
| US-D5 | As a keyboard or screen-reader user, I want the AI assistant to be fully usable | Given the launcher, when I activate it by keyboard, then focus moves into the panel, is trapped while open, Escape closes it, focus returns to the launcher, and new messages are announced |

### Epic E — AI assistant (F-14)

| ID | Story | Acceptance |
| :---- | :---- | :---- |
| US-E1 | As Priya, I want to ask a specific question at 11pm and get a straight answer, so I can keep moving without waiting for a reply | Given the assistant is open, when I ask something covered by site content, then I get an answer of ≤ 80 words citing and linking its source page |
| US-E2 | As Priya, I want to be told plainly when the assistant doesn't know, so I don't act on a wrong answer | Given a question outside the indexed content, when I send it, then the assistant states it cannot answer and offers the enquiry form and phone number — **and does not guess** |
| US-E3 | As the client, I want to know which questions the site fails to answer, so I can improve the content | Given a `chat_no_answer` event, when it fires, then the question text is logged and surfaced in the monthly review |
| US-E4 | As a visitor who doesn't want to chat, I want to be left alone | Given any page, when it loads, then the assistant never opens automatically, and dismissing the launcher keeps it dismissed for the session |

---

## 15\. User Journeys

### 15.1 Priya — cold organic (primary conversion path)

Google: "office refurbishment \[city\] hybrid working"

   ↓

Homepage — hero establishes commercial workplace focus         \[5s judgement\]

   ↓

Featured work — sees a 200-person professional services fit-out

   ↓

Project page — 12 weeks · OCCUPIED DURING WORKS · phased      ★ decisive moment

   ↓

Process page — stage 4 explains live-office protocols          \[fear resolved\]

   ↓

Turnkey service page — comparison table, indicative bands      \[scope \+ cost\]

   ↓

Copies project URL → emails COO                                \[OBJ-4 satisfied\]

   ↓                                        (returns 2–5 days later, direct)

Contact — submits qualified enquiry                            \[KPI-1\]

**Design implications**

- The 5-second homepage judgement is unforgiving — hero copy must name the buyer and outcome, not the firm  
- The occupied-during-works fact is the pivot of the entire journey; it must be impossible to miss  
- The gap between first visit and enquiry is days, not minutes — social preview metadata and shareable URLs are conversion infrastructure, not nice-to-haves

**Where F-14 fits this journey.** The assistant's realistic role is at the *evening research* stage, where Priya has a specific question — "can you work around our trading hours", "have you done anything at 200 people" — and no one to ask. A grounded answer with a link to the relevant project keeps her in the journey instead of closing the tab. Its role is **not** to replace any step above; every path still terminates at the F-08 form. The assistant that most helps this journey is one that answers narrowly and hands off early.

### 15.2 Daniel — referred, evaluating a repeat partner

Referral from an agent → searches firm name

   ↓

Homepage → immediately looks for developer-relevant content

   ↓

For Developers — voids, cost per sqft, CAT A/B                 \[correct framing\]

   ↓

Two spec-suite projects — checks programme reliability

   ↓

Capabilities — verifies insurance and scale

   ↓

Direct phone call (not the form)                               ← expected behaviour

**Design implication:** Daniel will bypass the form. Phone number must be prominent in the header or footer on every page. Attributing his enquiry requires a call-tracking or "how did you hear about us" mechanism — flagged as a measurement gap against KPI-1.

### 15.3 Sarah — direct link, qualifying a subcontractor

Receives /capabilities link during a conversation

   ↓

Scans for insurance, BIM level, drawing standards              \[\< 60 seconds\]

   ↓

Downloads capability statement PDF

   ↓

Checks 1–2 projects for technical quality

   ↓

Forwards PDF internally

**Design implication:** Sarah's journey never touches the homepage. `/capabilities` must be entirely self-sufficient — full context, contact details, and the PDF, without requiring navigation elsewhere.

---

## 16\. Content Requirements

Content is the critical path. **RISK-3 (client cannot supply copy) is the most common cause of failure for projects of this type** — more than any technical factor.

| Asset | Owner | Blocking? | Notes |
| :---- | :---- | :---- | :---- |
| Firm name, legal details, address | Client | ✅ | OQ-1 |
| 4–6 project write-ups (challenge/approach/outcome) | Client → we edit | ✅ | Structured questionnaire recommended |
| Project metadata (sqft, headcount, duration, occupied) | Client | ✅ | Must be accurate; these are trust claims |
| Project imagery | Client | ✅ | OQ-3 |
| Floor plans / drawings | Client | ⬜ | High value — mitigates RISK-1 |
| Team photos and bios | Client | ✅ | No stock imagery |
| Insurance and accreditation detail | Client | ⬜ | Blocks F-09 only |
| Testimonials \+ publication permission | Client | ⬜ | Permission is a process, not a task |
| Investment bands | Client | ⬜ | OQ-4 |
| **FAQ corpus — ≥ 25 Q\&A pairs** | Client → we edit | ✅ **for F-14** | Blocks the AI assistant (§14.7). Published as visible page content *and* indexed for the assistant. Add to the content questionnaire. |
| All marketing copy | We draft, client approves | ✅ |  |

> 💡 **Process recommendation:** issue a single structured content questionnaire covering everything above and treat its return as a project gate. Collecting content piecemeal over email is how these projects stall for months. Do not begin build until it is returned.

---

## 17\. Deferred Functionality

Not in scope. Listed to close them off explicitly rather than leave them ambiguous.

| Feature | Status | Reasoning |
| :---- | :---- | :---- |
| Site search | ❌ Not in MVP | \~14 pages. Search on a site this small signals bad navigation. |
| Newsletter signup | ❌ Not in MVP | No content programme to send. |
| Automated enquiry acknowledgement email | ⚠️ P2 | Worth adding, but manual reply within stated SLA is acceptable at launch volume. |
| CRM integration | ❌ Not in MVP | No CRM (OQ-6). |
| Live chat | ❌ | Requires staffing. |
| **AI assistant** | ✅ **In scope — F-14, P1** | Added by client direction. Originally excluded; the objections raised are retained as mandatory constraints in F-14 (§14.5–14.6) and as decision record Part 3 §27.1. |
| **Guided enquiry (F-13)** | ⚠️ P2 — **conditional** | Overlaps F-14. Build one, not both. Drop if F-14 ships. |
| Client portal | ❌ | Different product entirely. |

---

**End of Part 2\.**

**Next:** Part 3 — performance, accessibility, SEO, security, privacy, analytics, browser support, full risk register, and the post-launch roadmap.  
