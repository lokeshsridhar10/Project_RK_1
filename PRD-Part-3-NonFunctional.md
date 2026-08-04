# Product Requirements Document

## Commercial Interior Design Firm — Marketing & Lead Generation Website

**Part 3 of 3 — Non-Functional Requirements, Risks & Roadmap**

Version 0.2 (Draft) · 4 August 2026 · Continues from `PRD-Part-2-Functional.md`

---

## 18\. Performance Requirements

Performance is a business requirement here, not an engineering preference. This is an image-heavy site whose primary buyer researches on mobile in the evening. Slow pages lose enquiries.

### 18.1 Core Web Vitals — targets

| Metric | Target | Hard fail | Measured on |
| :---- | :---- | :---- | :---- |
| Largest Contentful Paint (LCP) | ≤ 2.0s | \> 2.5s | Mobile, 4G throttled |
| Interaction to Next Paint (INP) | ≤ 150ms | \> 200ms | Mobile |
| Cumulative Layout Shift (CLS) | ≤ 0.05 | \> 0.1 | All |
| First Contentful Paint | ≤ 1.5s | \> 1.8s | Mobile |
| Time to First Byte | ≤ 400ms | \> 800ms | Edge-cached |

### 18.2 Page budgets

| Page type | HTML+CSS+JS | Total with images | JS (parsed) |
| :---- | :---- | :---- | :---- |
| Homepage | ≤ 150KB | ≤ 1.2MB | ≤ 50KB |
| Project detail | ≤ 150KB | ≤ 1.5MB | ≤ 50KB |
| Service / Process | ≤ 120KB | ≤ 800KB | ≤ 40KB |
| Work index | ≤ 150KB | ≤ 1.0MB initial | ≤ 60KB |

> Budgets are enforced in CI. A build exceeding budget fails rather than warns — otherwise budgets erode silently over time.

**AI assistant carve-out (F-14).** The launcher button counts against the page budget and must be ≤ 2KB. The assistant bundle itself is **excluded from the page budget because it must not load on page render** — it loads only on user interaction. CI must verify both: that the launcher fits the budget, and that no assistant code is requested before first click. A build that loads the assistant eagerly fails, regardless of its size.

### 18.3 Image strategy

Images are 80–90% of page weight on this site. This section carries more performance impact than everything else combined.

| Requirement | Specification |
| :---- | :---- |
| Formats | AVIF primary → WebP fallback → JPEG fallback, via `<picture>` |
| Responsive | `srcset` at 400 / 800 / 1200 / 1600 / 2400px widths |
| Quality | AVIF q50–60, WebP q75 (visually lossless at these sizes) |
| Loading | `loading="eager"` \+ `fetchpriority="high"` for LCP hero only; `loading="lazy"` for everything else |
| Dimensions | Explicit `width`/`height` on every image — non-negotiable for CLS |
| Placeholder | LQIP blur or dominant-colour block |
| Processing | Build-time via Sharp. **No runtime image services** — avoids a recurring cost and a third-party dependency |
| Source discipline | Originals archived outside the repo; only processed derivatives committed |

### 18.4 Delivery

- Static HTML pre-rendered at build; no server-side rendering at request time  
- Global edge CDN (Cloudflare Pages)  
- Immutable, content-hashed asset filenames with 1-year cache headers  
- HTML cached at edge with short TTL and instant purge on deploy  
- Brotli compression  
- Critical CSS inlined; remaining CSS deferred  
- Self-hosted fonts, WOFF2, subset to used glyphs, `font-display: swap`, preloaded — **no Google Fonts CDN** (privacy and performance)  
- Zero render-blocking third-party scripts

### 18.5 JavaScript policy

The site must be **fully functional and readable with JavaScript disabled.** JS progressively enhances: portfolio filtering, mobile menu, gallery lightbox, form validation. Each has a no-JS fallback (all projects visible, menu as anchor links, gallery as a plain grid, server-side validation on submit).

**No framework runtime ships to the browser** unless a specific requirement justifies it. None currently does.

---

## 19\. Accessibility Requirements

**Target: WCAG 2.2 Level AA.** This is a floor, not an aspiration.

Beyond ethics and law: a meaningful share of corporate facility managers work for organisations with procurement accessibility requirements, and public-sector and large-corporate buyers increasingly ask. An inaccessible site is a commercial liability with this specific audience.

### 19.1 Requirements

| Area | Requirement |
| :---- | :---- |
| **Contrast** | Text ≥ 4.5:1; large text ≥ 3:1; UI components and focus indicators ≥ 3:1. **Verified against the final palette — the navy/gold and black/amber directions need explicit checking, as gold-on-dark frequently fails** |
| **Keyboard** | All functionality operable by keyboard. No traps. Visible focus indicator ≥ 2px, ≥ 3:1 contrast. Logical DOM-order tab sequence. |
| **Focus (2.2)** | Focus never entirely obscured by the sticky header — verify with `scroll-margin-top` |
| **Semantics** | Landmark regions on every page. One `<h1>`. No skipped heading levels. Lists as lists, tables as tables. |
| **Images** | Descriptive alt text; decorative images `alt=""`. Alt describes the *space*, not the file. |
| **Forms** | Persistent visible labels. `aria-describedby` for hints and errors. Errors announced via live region. Never colour alone to indicate error. |
| **Motion** | `prefers-reduced-motion: reduce` disables non-essential animation. No auto-playing carousels. No parallax on mobile. |
| **Target size (2.2)** | Interactive targets ≥ 24×24px, ≥ 44×44px recommended on touch |
| **Zoom** | Usable at 200% zoom and 320px width without horizontal scrolling or content loss |
| **Language** | `lang` attribute set |
| **Skip link** | First focusable element on every page |
| **Media** | No auto-play. Captions if video is added later. |

### 19.2 Testing protocol

1. **Automated** — axe-core in CI; zero violations to pass  
2. **Keyboard** — full manual traversal of every template  
3. **Screen reader** — VoiceOver/Safari and NVDA/Firefox on homepage, project page and form at minimum  
4. **Zoom** — 200% and 400% reflow  
5. **Contrast** — every colour pair in the design system verified and documented

>   
> Automated tools catch roughly 30–40% of issues. Manual testing is required, not optional.

---

## 20\. SEO Requirements

### 20.1 Strategy

The realistic play is **local \+ long-tail commercial intent**, not head terms. "Interior design" is unwinnable. `office fit-out [city]`, `office refurbishment [city]`, `workplace strategy consultant [region]` are winnable within 6–12 months with correct execution.

> ⚠️ **Expectation management (RISK-5):** ranking for competitive local terms takes 6–12 months minimum, and depends on factors outside the website — Google Business Profile, citations, and backlinks. The site is necessary but not sufficient. This must be stated to the client in writing before launch to avoid a "the website isn't working" conversation in month three.

### 20.2 Technical SEO

| Requirement | Specification |
| :---- | :---- |
| Rendering | Static HTML — full content in initial response, no client-side rendering of primary content |
| URLs | Clean, lowercase, hyphenated, permanent |
| Canonicals | Self-referencing on every page; filter query params canonicalised to `/work` |
| Sitemap | `sitemap.xml` auto-generated at build, submitted to Search Console |
| Robots | `robots.txt` with sitemap reference |
| Redirects | 301 for any changed URL; redirect map maintained in the repo |
| HTTPS | Enforced, HSTS enabled |
| Structured data | `Organization` \+ `LocalBusiness` (sitewide), `BreadcrumbList`, `Project`/`CreativeWork` (projects), `Service` (services), `FAQPage` (FAQs) — all validated |
| Pagination | Not required at launch volume |
| 404 | Returns a genuine 404 status, not a 200 soft-404 |

### 20.3 On-page

- Unique `<title>` (≤ 60 chars) and meta description (≤ 155 chars) per page — **no templated duplicates**  
- One `<h1>` per page containing the primary term naturally  
- Open Graph and Twitter Card metadata on every page, with a per-project OG image — **directly supports US-C3, since the primary sharing mechanism is Priya emailing a link to her COO**  
- Descriptive internal anchor text; never "click here" or "read more"  
- Alt text serves accessibility first; keyword stuffing is prohibited

### 20.4 Local SEO — off-site, client-owned

Flagged explicitly because these sit outside the build but determine whether KPI-4 is achievable:

- Google Business Profile created, verified, categorised, populated with photos  
- NAP (name, address, phone) consistent across the site and all directories  
- Local and industry directory citations  
- Client review acquisition process

**Owner: client.** The website cannot deliver KPI-4 alone.

---

## 21\. Security Requirements

Attack surface is small — a static site with one form. Requirements are proportionate.

| Area | Requirement |
| :---- | :---- |
| Transport | HTTPS enforced, TLS 1.2+, HSTS with `includeSubDomains` |
| Headers | `Content-Security-Policy` (strict, no `unsafe-inline` scripts), `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `X-Frame-Options: DENY`, `Permissions-Policy` denying unused features |
| Form handling | Server-side validation and sanitisation. Rate limiting per IP. Honeypot \+ timing check. |
| Secrets | Never committed. Environment variables only. Rotated if exposed. |
| Dependencies | Automated vulnerability scanning; minimal dependency count by design |
| Admin surface | **None** — no CMS, no login, no database. This eliminates the majority of realistic attack vectors and is a genuine benefit of the static architecture. |
| DNS | Registrar 2FA enabled. Registered in the **client's** name — see §25. |
| Email | SPF, DKIM, DMARC configured if sending from the domain |
| Backups | Full site in Git; content and original images archived separately by the client |

---

## 22\. Privacy & Legal

> ⚠️ **OQ-12 is unresolved.** Requirements below assume **UK/EU GDPR**. If the client operates in India (DPDP Act) or the US (state-level), this section requires revision before launch.

| Requirement | Specification |
| :---- | :---- |
| Lawful basis | Consent for the enquiry form; legitimate interest for aggregate analytics |
| Consent | Explicit, unticked checkbox with privacy policy link. Never pre-checked. Consent record stored with the submission. |
| Privacy policy | Data collected, purpose, retention period, third parties, data subject rights, contact for requests |
| Cookies | **Recommendation: use cookieless, privacy-respecting analytics and set no non-essential cookies — this removes the consent banner entirely.** A banner is friction on a conversion-focused site. If GA4 is required, a compliant consent banner becomes mandatory. |
| Data minimisation | Collect only what the form specifies; no hidden tracking fields |
| Retention | Defined period for enquiry data, stated in the policy |
| Processors | Hosting, form handler and analytics named in the policy |
| Transfers | Documented if data leaves the jurisdiction |
| Image rights | **Client warrants they hold rights to publish all project photography.** Photographer licences frequently restrict web use — this is a real and commonly-missed legal exposure. |
| Client confidentiality | Named clients require written permission (OQ-7); default to anonymised descriptors |
| **AI assistant (F-14)** | AI status disclosed before first input. Transcripts retained for a defined, limited period stated in the policy. LLM provider named as a processor. Provider configured **not** to train on submitted data — confirm contractually. Data residency verified against OQ-12. Transcripts deletable on request. Assistant must not solicit personal data in free text — it hands off to F-08, which carries proper consent. |

---

## 23\. Analytics & Measurement

### 23.1 Platform

**Recommended: Plausible or Cloudflare Web Analytics** (cookieless, \~£0–9/mo, no consent banner required, negligible performance cost). **Alternative: GA4** (free, richer funnels, but requires a consent banner and adds \~45KB).

|  | Cookieless | GA4 |
| :---- | :---- | :---- |
| Consent banner | Not required | Required |
| Performance cost | \~1KB | \~45KB |
| Funnel analysis | Basic | Advanced |
| Cost | £0–9/mo | Free |
| Recommendation | ✅ **Preferred** — banner friction outweighs the analytical depth at this volume | Use only if the client specifically requires GA4 |

### 23.2 Events to track

| Event | Purpose |
| :---- | :---- |
| `enquiry_submitted` | **Primary conversion** — KPI-1 |
| `enquiry_started` | First field focus — gives abandonment rate |
| `cta_clicked` | With page and position — identifies which CTA band copy works |
| `project_viewed` | With project slug — reveals which work drives enquiries |
| `filter_applied` | With filter values — **reveals actual buyer sector and scale, valuable business intelligence for the client** |
| `phone_clicked` | Partial attribution for Daniel's journey (§15.2) |
| `capability_pdf_downloaded` | Sarah engagement |
| `scroll_depth_75` | Content engagement proxy |
| `chat_opened` | F-14 engagement rate |
| `chat_message_sent` | Conversation depth |
| `chat_handoff_offered` | How often the assistant reaches its limits |
| `chat_to_form` | **Chat-attributed conversion** — the metric that justifies F-14's existence |
| `chat_abandoned` | Drop-off without handoff |
| `chat_no_answer` | **Logs the unanswered question text.** Operationally the most valuable event on the site — it tells the client exactly what buyers ask that the site fails to answer, and feeds the FAQ corpus. |

### 23.3 Measurement gaps — stated honestly

- **Phone enquiries are not attributable** without call tracking (not in scope). Daniel's journey will be under-counted against KPI-1. Mitigation: client asks "how did you hear about us" on every call and logs it.  
- **Referral-driven visits appear as direct traffic** and will understate the site's contribution to closed work.  
- **KPI-2 (enquiry → call rate) requires manual logging** by the client. Without it, the metric is unmeasurable.

---

## 24\. Browser & Device Support

| Tier | Browsers | Standard |
| :---- | :---- | :---- |
| **Full** | Chrome, Edge, Firefox, Safari — last 2 versions; iOS Safari 16+; Chrome Android | Pixel-accurate |
| **Functional** | Safari 15, Firefox ESR, Samsung Internet | Fully usable; minor visual variance acceptable |
| **Basic** | Older / unknown | Content readable and form submittable |
| **Unsupported** | Internet Explorer | No support |

**Breakpoints:** 320 (min) · 640 · 768 · 1024 · 1280 · 1536+

**Mobile-first.** Priya's initial research is mobile (Persona 1); desktop is where she submits. Both matter, but mobile is where the site is judged.

**Physical device testing required before launch** — one real iOS device and one real Android device. Emulators do not surface font rendering, touch target, or scroll performance issues.

---

## 25\. Operational & Commercial Requirements

These are non-technical but determine whether the project succeeds commercially.

| ID | Requirement | Rationale |
| :---- | :---- | :---- |
| OPS-1 | **Domain registered in the client's name**, with the client holding registrar credentials | Developer-owned domains are a recurring source of serious disputes. Non-negotiable. |
| OPS-2 | **Hosting account owned by the client**; developer granted access | Same reasoning |
| OPS-3 | **Source code ownership defined in writing** before build begins | Ambiguity here surfaces at the worst moment |
| OPS-4 | **Written maintenance agreement** — scope, turnaround, cost, exclusions | RISK-4. Without it, "you edit on request" becomes indefinite unpaid work. |
| OPS-5 | **Content questionnaire returned in full before build starts** | RISK-3 mitigation; the single highest-leverage process control in this project |
| OPS-6 | **Handover pack** — how to request changes, where things live, credentials, backup locations | Protects both parties |
| OPS-7 | Uptime expectation stated (static \+ CDN ≈ 99.9%, no SLA offered) | Prevents unrealistic expectations |

---

## 26\. Risk Register

Severity \= Impact × Likelihood. Owner is who must act.

| ID | Risk | Sev | Owner | Mitigation |
| :---- | :---- | :---- | :---- | :---- |
| **RISK-1** | **Thin photography undermines credibility and conversion** | 🔴 High | Shared | Design a template that looks complete with 1 hero \+ 1 plan \+ 3 details (F-03). Substitute floor plans, material close-ups, before/afters, process imagery. Prioritise a single professional shoot of the best project over thin coverage of several. Quantified stats replace client logos in the proof strip. |
| **RISK-2** | **No brand identity — delays start, risks rework** | 🔴 High | Client | Derive identity from the three existing homepage samples rather than commissioning a full brand exercise (OQ-2). Lock the direction before any page design begins. If the client later wants full branding, treat as separate scope with its own cost and timeline. |
| **RISK-3** | **Client cannot supply content — project stalls indefinitely** | 🔴 High | Client | **The most common cause of death for projects like this.** Single structured questionnaire, returned in full, treated as a hard gate before build (OPS-5). Draft copy from an interview if the client cannot write. Agree a content deadline with a stated consequence. |
| **RISK-4** | **Developer becomes an indefinite unpaid dependency** | 🟠 Med | You | Written maintenance agreement before launch (OPS-4). Consider a lightweight Git-based CMS in Phase 2 to hand over routine edits. |
| **RISK-5** | **Unrealistic SEO expectations** | 🟠 Med | You | State the 6–12 month horizon in writing pre-launch. Report on leading indicators (impressions, position) not just enquiries. Make clear that Google Business Profile and citations are client-owned and necessary. |
| **RISK-6** | **Fewer than four publishable projects at launch** | 🟠 Med | Client | Hard launch gate (Part 1 §8.3). If unmet, pivot to a capability-and-process-led site with a "selected work" framing rather than launching a thin portfolio. |
| **RISK-7** | **Client refuses any budget indication** | 🟠 Med | Client | Present conversion evidence for ranges. Fallback: explain *how* cost is determined and what drives it — partially satisfies the buyer's need without quoting numbers. |
| **RISK-8** | **No permission to name clients or publish photography** | 🟠 Med | Client | Anonymised descriptors by default. Verify photographer licences early — web publication rights are commonly restricted (§22). |
| **RISK-9** | **Scope creep — "can we add a blog / portal / booking"** | 🟠 Med | You | Explicit non-objectives (Part 1 §4.2) and deferred list (Part 2 §17). Change requests are re-scoped and re-priced, not absorbed. |
| **RISK-10** | **Jurisdiction unconfirmed — wrong privacy regime** | 🟡 Low | Client | Resolve OQ-12 before writing the privacy policy. Cheap to fix now, expensive to fix after launch. |
| **RISK-11** | **Image weight destroys mobile performance** | 🟡 Low | You | CI-enforced page budgets (§18.2), build-time processing, AVIF/WebP pipeline. Low likelihood only because it is explicitly engineered against. |
| **RISK-12** | **Single-person dependency — you are the only maintainer** | 🟡 Low | You | Handover pack (OPS-6), plain-standards codebase, documented build. No exotic tooling that only you understand. |
| **RISK-13** | **AI assistant fabricates a price, deadline or capability** — creating a claim the firm must honour or retract, in front of a risk-averse buyer | 🔴 High | You \+ Client | Strict retrieval-only grounding (F-14 §14.2), explicit refusal categories (§14.1), 30-prompt adversarial suite passing with zero fabrications before launch (§14.9). **Mitigated, not eliminated** — requires monthly transcript review, not a one-off test. Client must be told in writing that residual risk remains. |
| **RISK-14** | **Assistant grounded on thin content produces thin or invented answers** | 🟠 Med | Client | Hard gate: ≥ 25-pair FAQ corpus plus all P0 pages complete before F-14 ships (§14.7). Compounds RISK-1 and RISK-3 — if content is late, F-14 must slip rather than launch under-grounded. |
| **RISK-15** | **Off-the-shelf vendor chosen, silently breaking the §18.2, §19 and §22 commitments** | 🟠 Med | You | Option B recommended (§14.4). If Option A is selected, formally relax those sections in writing before build — do not leave the PRD asserting commitments the build cannot meet. |
| **RISK-16** | **Assistant becomes an unmonitored liability** — quality drifts, nobody reviews transcripts, stale answers persist after services change | 🟠 Med | Client | Monthly review of `chat_no_answer` and transcripts written into the maintenance agreement (OPS-4). Index rebuilds at every deploy, so page edits propagate automatically — but the *review* is a human commitment someone must own. |

---

## 27\. Future Enhancements — post-launch roadmap

Sequenced by value-to-effort, not by novelty.

### Phase 2 — first 3–6 months post-launch

| Enhancement | Value | Effort |
| :---- | :---- | :---- |
| **Git-based CMS** (Decap/Tina/Sveltia) for client self-editing | High — removes RISK-4 permanently | Medium |
| **Additional projects** as work completes | High — directly compounds RISK-1 mitigation | Low |
| **Automated enquiry acknowledgement email** | Medium — improves perceived responsiveness | Low |
| **Downloadable case study PDFs** | Medium — strong B2B internal-sharing asset (OBJ-4) | Low |
| **Named testimonials** once permission is obtained | High | Low |
| **Guided enquiry (F-13)** | Medium — lifts form completion; requires ≥ 8 weeks of F-08 baseline data first | Low-Medium |

### Phase 3 — 6–12 months

| Enhancement | Value | Effort |
| :---- | :---- | :---- |
| **Insights / content programme** | High for SEO — *only if a content commitment is genuinely staffed* | High (ongoing) |
| **Workplace strategy lead magnet** (e.g. hybrid-working space calculator) | High — top-of-funnel capture | Medium |
| **Sector landing pages** (healthcare, education, tech fit-out) | Medium-high for long-tail SEO | Medium |
| **CRM integration** | Medium — only once enquiry volume justifies it | Medium |

### Phase 4 — 12 months+

Video walkthroughs · 360°/virtual tours · client project portal · multi-language (if geography expands) · A/B testing (only once traffic supports significance — at 250–600 sessions/month, it does not).

> 💡 **Deliberately excluded from all phases:** live chat (requires staffing) and pop-up newsletter modals (friction, and no content programme to justify them). The AI assistant was initially excluded and is now **in scope as F-14** — see the decision record in §27.1.

### 27.1 AI assistant — decision record

|  |  |
| :---- | :---- |
| **Initial recommendation** | Exclude |
| **Final decision** | **Include as F-14, P1** — client direction, 4 August 2026 |
| **Status of original objections** | Retained as mandatory pass/fail constraints in F-14 §14.5–14.6 |

This record exists so the reasoning survives, and so the constraints are not quietly dropped during build. **The decision is settled; the constraints are not negotiable.**

#### Objections raised, and how F-14 answers each

| Objection | Resolution in F-14 |
| :---- | :---- |
| **SEO** — content inside a widget is invisible to search, so it cannot serve OBJ-3 | §14.7 requires the FAQ corpus to be **published as visible page content as well as** indexed for the assistant. The content earns long-tail traffic *and* grounds the assistant. Objection resolved. |
| **Performance** — 150–300KB of third-party JS breaks the §18.2 budget and the LCP ≤2.0s target | §14.5 caps the initial payload at a ≤2KB launcher, with the assistant loading only on click. CI verifies zero LCP/CLS/INP impact. Objection resolved **only under Option B** (self-hosted). |
| **Privacy** — third-party widgets set cookies, forcing the consent banner avoided in §22 | §14.5 prohibits cookies; §14.6 requires the LLM provider to be named as a processor and configured not to train on submitted data. Objection resolved under Option B. |
| **Accessibility** — vendor widgets commonly fail WCAG 2.2 AA, undermining a claim with procurement value | §14.5 and §14.9 impose full keyboard operation, focus management, live-region announcements and zero axe violations as pass/fail criteria. Objection resolved under Option B. |
| **Liability** — a bot inventing prices, deadlines or sector experience creates claims the firm must honour or retract | §14.1 refusal rules, §14.2 grounding rules, and a mandatory **30-prompt adversarial test suite with zero fabrications** (§14.9). **This is mitigated, not eliminated — see RISK-13.** |
| **Economics** — £30–100/mo vendor cost against 4–8 enquiries/month | Option B runs \~£5–20/mo at projected volume, below any vendor subscription. Objection resolved under Option B. |

> ⚠️ **Every resolution above except SEO depends on choosing Option B (self-hosted).** Selecting an off-the-shelf vendor (Option A) reinstates the performance, privacy, accessibility and cost objections in full. If the client chooses Option A, §18.2, §19 and §22 must be formally relaxed and that relaxation recorded in writing — otherwise the PRD contains commitments the build cannot meet.

#### Residual risk that cannot be designed away

Grounding, refusal rules and adversarial testing substantially reduce fabrication; they do not eliminate it. A career-exposed buyer (Persona 1\) receiving one confidently wrong answer about cost or capability is a live commercial risk for as long as the assistant is running. This is carried as **RISK-13** and requires ongoing monitoring, not a one-off pre-launch test.

---

## 28\. Sign-Off & Next Steps

### 28.1 PRD approval checklist

- [ ] Buyer prioritisation accepted (facility managers primary — Part 1 §6.1)  
- [ ] Service hierarchy accepted (turnkey headline — Part 1 §6.2)  
- [ ] MVP scope accepted (Part 1 §8.1)  
- [ ] Four-project launch gate accepted (Part 1 §8.3)  
- [ ] KPI targets accepted as provisional pending market data (Part 1 §5)  
- [ ] **OQ-1 to OQ-4 answered** (Part 1 §9.1)  
- [ ] Analytics platform chosen (§23.1)  
- [ ] Jurisdiction confirmed (OQ-12)  
- [ ] Commercial terms agreed (OPS-1 to OPS-4)  
- [ ] **AI assistant (F-14) build-vs-buy decision made** (§14.4) — and, if Option A, §18.2/§19/§22 formally relaxed in writing  
- [ ] **RISK-13 residual fabrication risk acknowledged by the client in writing**  
- [ ] F-13 vs F-14 overlap resolved — one dropped

### 28.2 Immediate blocking actions

| \# | Action | Owner | Blocks |
| :---- | :---- | :---- | :---- |
| 1 | Confirm client name, city, service radius | Client | Everything |
| 2 | Decide brand direction (derive from samples vs. full branding) | Client | Phase 4 design |
| 3 | Audit photography — count publishable projects, verify licences | Client | Launch viability |
| 4 | Decide on publishing investment bands | Client | F-04, F-10 |
| 5 | Confirm jurisdiction | Client | §22, F-14 data residency |
| 6 | Issue content questionnaire | You | Build start |
| 7 | Agree commercial terms in writing | Both | Build start |
| 8 | Decide F-14 build-vs-buy (Option A/B/C) | Both | F-14 build; NFR commitments |
| 9 | Commission the ≥ 25-pair FAQ corpus | Client | F-14 launch (§14.7) |
| 10 | Confirm who owns monthly transcript review | Client | RISK-16; maintenance agreement |

### 28.3 Phase sequence from here

✅ Phase 1  Discovery              (folded into PRD Part 1\)

✅ Phase 2  PRD                    (this document set)

⬜ Phase 3  UX Planning            — flows, wireframes, responsive behaviour

⬜ Phase 4  UI Design System       — palette, type, components, states, dark mode

⬜ Phase 5  TRD                    — lightweight; static architecture, build, deploy, CI

⬜ Phase 6  Project Planning       — milestones and sprints

⬜ Phase 7  Development

⬜ Phase 8  Code Review

⬜ Phase 9  Testing

> **Recommendation:** Phase 3 (UX) can begin immediately using the current assumptions — flows and wireframes do not depend on the client's name or final photography. **Phase 4 (UI Design System) should not begin until OQ-2 is resolved**, as the brand direction determines the entire palette and type system, and starting early risks throwing the work away.

---

## Appendix A — Requirement Index

| Range | Type | Location |
| :---- | :---- | :---- |
| OBJ-1 – OBJ-6 | Objectives | Part 1 §4.1 |
| KPI-1 – KPI-7 | Success metrics | Part 1 §5 |
| OQ-1 – OQ-12 | Open questions | Part 1 §9 |
| F-01 – F-14 | Features | Part 2 §13 |
| US-A1 – US-E3 | User stories | Part 2 §14 |
| OPS-1 – OPS-7 | Operational | Part 3 §25 |
| RISK-1 – RISK-16 | Risks | Part 3 §26 |

---

**End of Part 3\. PRD complete.**  
