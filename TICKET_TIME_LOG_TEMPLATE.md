# Ticket Time-Log Comment Guide — LIONGATE

A reusable reference for logging work on software engineering / full-stack tickets (GitHub
Issues, Jira, or any PM board) in a granular, PM-friendly format — regardless of whether the
ticket is a feature, bugfix, hotfix, audit, review, testing task, spike, or chore.

This complements `Ticket_Workflow_Prompts.md`: those prompts produce the _analysis_ and
_implementation plan_ (with an estimated effort by phase); this guide produces the **actual
work-log comments** you post on the ticket as you go, or reconstruct afterward from your
session notes.

---

## 1. Purpose

Estimated effort (Prompt 1, §11 of the ticket analysis) tells a PM what a phase is _expected_
to cost. Time-log comments tell them what it _actually_ cost, broken down finely enough to be
useful for retros, velocity tracking, and "where did the time actually go" conversations —
without you having to invent a format from scratch on every ticket.

---

## 2. Format

```
<duration> - <lowercase action phrase> [- optional detail / ticket-specific note]
```

- **duration** — `h`, `m`, or `s`. Combine when useful: `1h30m`, `45m`, `20s`.
  - `s` is rare — reserve it for genuinely trivial, one-off actions (a config toggle, a single
    env var change). If you're using `s` more than once or twice a day, you're probably
    over-granular; roll it into the surrounding entry instead.
  - Round to the nearest 5–15 minutes. Don't chase false precision (`17m` reads as noise;
    `15m` or `20m` reads as a real estimate).
- **action phrase** — starts with a verb or gerund (`reviewed`, `debugging`, `wrote`,
  `pairing`), specific enough that someone else on the team understands what happened without
  opening the ticket.
- **optional detail** — a file, PR number, tool name, or person, when it adds real information.

Examples:

```
2h - brainstorming on new requirements
45m - reviewed PR #114 feedback and addressed comments
20m - reproducing reported bug in staging
1h30m - implementing shared hreflang/canonical metadata helper
15m - syncing with tech lead on approach - confirmed shared helper pattern
```

---

## 3. Phrase Bank by Category

Use whichever categories are actually relevant to the ticket — most tickets touch 3–5 of these,
not all ten. Categories deliberately mirror the "Estimated Effort by Phase" structure from the
ticket-analysis prompt, so the two documents stay easy to cross-reference.

### A. Discovery / Analysis

```
Xh - reading and analyzing ticket requirements
Xh - brainstorming on new requirements
Xm - clarifying ticket scope with reporter/PM
Xh - auditing current implementation / current state
Xm - reviewing related tickets for context/dependencies
Xh - spiking on possible approaches
Xm - reviewing crawler/SEO/monitoring tool report
Xm - reproducing the issue to confirm scope
```

### B. Design / Planning

```
Xh - designing technical approach / architecture for [feature]
Xm - deciding data model / schema shape
Xh - writing pre-implementation technical notes
Xm - sketching flow / wireframe for [component]
Xh - mapping acceptance criteria to technical verification tasks
Xm - evaluating library/tool options for [need]
```

### C. Implementation / Coding

```
Xh - implementing [feature/component name]
Xm - writing [helper/utility] function
Xh - integrating [API/service] into [module]
Xm - refactoring [file/module] for clarity
Xh - wiring [config/metadata] across [pages/routes]
Xm - resolving merge conflicts on [branch]
Xm - updating [config file] for [purpose]
```

### D. Bugfix / Debugging

```
Xm - reproducing reported bug
Xh - debugging root cause of [issue]
Xm - fixing lint/type errors surfaced by CI
Xh - patching regression introduced by [PR/commit]
Xm - verifying hotfix behavior in staging
Xm - bisecting to find the commit that introduced [bug]
```

### E. Testing / QA

```
Xh - writing unit tests for [component/helper]
Xm - writing integration/e2e tests for [flow]
Xm - running full test suite locally
Xh - manual QA across browsers/devices for [feature]
Xm - fixing a flaky/failing CI test
Xm - writing test plan / test cases for the ticket
```

### F. Review / Collaboration

```
Xm - reviewing PR #[number]
Xm - addressing reviewer feedback on PR #[number]
Xh - pair programming with [name] on [task]
Xm - syncing with tech lead on approach
Xm - requesting/waiting on design or product clarification
```

### G. Documentation

```
Xm - updating README / wiki for [change]
Xh - writing PR description and test plan
Xm - documenting audit findings
Xm - writing code comments / inline docs where warranted
Xm - updating onboarding/reference docs
```

### H. Meetings / Coordination

```
Xm - standup
Xh - sprint planning / backlog grooming
Xm - clarifying requirements with stakeholder
Xm - demoing progress to [team/PM]
```

### I. DevOps / Deployment

```
Xm - deploying to staging
Xh - configuring/adjusting CI pipeline step
Xm - monitoring deployment / checking logs post-deploy
Xm - rolling back a bad deploy
Xh - setting up environment/infra for [need]
```

### J. Verification / Closure

```
Xm - re-verifying fix against acceptance criteria
Xh - final smoke test before merge
Xm - re-crawling / re-running audit tool to confirm fix
Xm - closing ticket, updating board status, linking follow-ups
```

---

## 4. Worked Example — Applied to Ticket #105

Based on the actual phases this ticket went through (audit via crawler screenshot → confirming
DE/parity/`x-default` with the reporter → building the shared helper → wiring it in → testing →
PR), here's what a realistic log would look like, roughly matching the ~5–6.5h estimate from the
ticket analysis:

```
20m - reading and analyzing ticket #105 requirements
15m - reviewing SiteOne Crawler report screenshot - confirmed it doesn't check hreflang/canonical directly
1h30m - auditing current hreflang/canonical output across EN/FR/DE for Home, Blog listing, Blog article, Contact, Career
15m - clarifying DE locale status and content parity with reporter - confirmed DE live, full parity
10m - confirming x-default target with reporter - confirmed FR
15m - locating getOrganizationSchema() in lib/ to match existing SEO helper pattern
1h45m - implementing shared canonical/hreflang metadata helper
1h - wiring helper into generateMetadata for Home, Blog listing, Blog article, Contact, Career
30m - writing Vitest unit test for hreflang reciprocity
15m - running pnpm typecheck / pnpm lint / pnpm build - all green
40m - re-crawling with Screaming Frog to verify no hreflang errors
15m - writing PR description, test plan, and opening PR #<n> against develop
```

**Total: ~6h50m**

---

## 5. Prompt Template — Ask an AI to Generate This for Any Ticket

Use this once implementation on a ticket is actually done (or as you go, session by session).
Paste the ticket, the prior analysis/implementation notes, and a short account of what you
actually did — the AI fills in realistic, specific entries rather than generic filler.

```text
Act as a Senior Software Engineer logging work on a ticket for project-management tracking.

I will give you:
1. The original ticket description.
2. Any prior ticket-analysis / implementation notes already produced for it.
3. A short account, in my own words, of what I actually did in this session (or across
   sessions) - rough order, what I got stuck on, what I built, what I reviewed, etc.

Your job: produce a realistic, granular work-log comment list for this ticket, ready to paste
as ticket comments.

Format for every line:
<duration> - <lowercase action phrase> [- optional detail]

Rules:
- Durations in h/m, using s only for genuinely trivial one-off actions. Round to the nearest
  5-15 minutes - no false precision.
- Only include the categories actually relevant to this ticket - choose from: Discovery/Analysis,
  Design/Planning, Implementation, Bugfix/Debugging, Testing/QA, Review/Collaboration,
  Documentation, Meetings/Coordination, DevOps/Deployment, Verification/Closure. Skip
  categories that don't apply; don't pad the list to look thorough.
- Every entry must be specific to this ticket's actual content (file names, ticket numbers, tool
  names, the actual bug/feature) - not generic phrases that could apply to any ticket.
- Order entries chronologically, the way the work would realistically have happened.
- End with a total duration line, and sanity-check that the sum roughly matches the effort this
  ticket plausibly took - flag it if my account implies an unrealistic total (e.g. 15 entries
  summing to 20 minutes, or a trivial fix logged at 8 hours).
- Output only the log list plus the total - no extra commentary, no restating the ticket.
```
