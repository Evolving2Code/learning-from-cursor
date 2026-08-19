# Lesson 01 — Working with Cursor (and keeping good habits)

**Goal:** Use Cursor as a force multiplier, not a shortcut around thinking. By the end you'll have a repeatable workflow for scoped tasks, clear prompts, and disciplined review.

**Time:** One sitting to read; exercises spread across real work this week.

---

## 1. The mindset shift

Cursor is best at **well-defined, bounded work** inside a codebase you understand. It struggles when:

- The goal is vague ("make it better", "fix the app")
- The scope is huge ("rebuild the dashboard")
- Constraints are unstated (design system, auth model, RLS policies)

**Habit:** Treat every agent turn like a small PR you'd assign a teammate — title, acceptance criteria, and context.

### Good vs fuzzy requests

| Fuzzy | Better |
|-------|--------|
| "Add auth" | "Add Supabase email/password sign-in using `@supabase/ssr`. Login page at `/login`, redirect to `/dashboard` on success. Use server components for session check on `/dashboard`." |
| "Fix the bug" | "After submitting the form on `/settings`, the profile name doesn't update until refresh. Relevant files: `app/settings/page.tsx`, `lib/actions/update-profile.ts`. Expected: optimistic UI or revalidate after mutation." |
| "Refactor" | "Extract the Supabase query in `app/projects/page.tsx` into `lib/data/projects.ts`. Keep the same return type; no behavior change." |

---

## 2. Scoping: one outcome per session

**Habit:** One logical change per agent conversation (or per branch). If you need three features, do three passes.

Why this matters for Next.js + Supabase:

- Auth touches middleware, cookies, and RLS — easy to break in a mega-diff
- Server vs client component boundaries are subtle; large refactors hide mistakes
- Supabase migrations and TypeScript types should stay in sync incrementally

### A useful task template

Copy this into Cursor when starting work:

```text
## Outcome
[One sentence: what should be true when we're done?]

## Acceptance criteria
- [ ] ...
- [ ] ...

## Context
- Stack: Next.js App Router, Supabase
- Relevant paths: app/..., lib/supabase/...
- Constraints: [e.g. no new dependencies, must work with existing RLS]

## Out of scope
- [What we're explicitly NOT doing this turn]
```

---

## 3. Modes: when to use what

| Mode | Use when |
|------|----------|
| **Agent** | Implementing, refactoring, running commands, fixing errors with file access |
| **Ask** | Understanding code, comparing approaches, architecture questions — no edits |
| **Plan** (if available) | Larger feature: design approach before coding |

**Habit:** Explore in Ask, commit in Agent. Don't let the agent rewrite half the app while you're still deciding the approach.

---

## 4. Giving the agent the right context

You don't need to paste your whole repo. You **do** need:

1. **File paths** — `@app/dashboard/page.tsx` or drag files into chat
2. **Error output** — full stack trace or terminal log, not "it doesn't work"
3. **Constraints** — "server action only", "must respect RLS", "use existing `Button` component"
4. **Examples** — "match the pattern in `app/projects/[id]/page.tsx`"

For Supabase specifically, mention:

- Whether the code runs on **server** (RSC, route handlers, server actions) or **client**
- If **RLS** applies — the agent should not bypass security with service role keys in client code
- Whether types come from **generated** `Database` types (`supabase gen types`)

---

## 5. Project rules (`.cursor/rules`)

Rules are persistent instructions the agent sees every session. They're how you encode **team habits** once instead of repeating them in every prompt.

This repo includes an example: [`.cursor/rules/nextjs-supabase.mdc`](../../.cursor/rules/nextjs-supabase.mdc).

**Habit:** Add rules for things you correct repeatedly:

- "Prefer server components; add `'use client'` only when needed"
- "Never commit `.env.local` or put `SUPABASE_SERVICE_ROLE_KEY` in client bundles"
- "Match existing file layout under `lib/` and `components/`"

Keep rules **short and enforceable**. A 200-line rule file gets ignored.

---

## 6. Review discipline (the most important habit)

The agent is fast; **you** are responsible for correctness.

### Before you accept a diff, check:

- [ ] **Scope** — Did it only change what you asked for?
- [ ] **Secrets** — No API keys, no service role in client code
- [ ] **Supabase auth** — Session/cookies handled via `@supabase/ssr` patterns, not ad-hoc localStorage hacks
- [ ] **RLS** — Data access still goes through policies you expect
- [ ] **Types** — No `any` sprawl; Supabase rows match your types
- [ ] **Server/client boundary** — No importing server-only modules into client components
- [ ] **Dependencies** — New packages justified? Versions aligned with your stack?
- [ ] **Deletes** — Nothing removed that you still need (agents sometimes "clean up" aggressively)

### How to review efficiently

1. Skim the **file list** first — surprise files are a red flag
2. Read **changed functions** before boilerplate
3. Run the app (`npm run dev`) and hit the affected route
4. If something feels wrong, **revert scope**: "Undo changes to X; only fix Y"

**Habit:** Say "explain why you chose this approach" before merging non-trivial changes. Good for learning; catches weak reasoning early.

---

## 7. Git habits with an agent

| Habit | Why |
|-------|-----|
| **Branch per task** | `feat/login-form`, not everything on `main` |
| **Small commits** | Easier to bisect when the agent introduces a regression |
| **Meaningful messages** | `feat(auth): add login page with Supabase SSR` not `fix stuff` |
| **PR even solo** | Forces a review pass; Cursor can summarize the diff |

When using Cloud Agents, the agent may open PRs for you — still read them like someone else wrote the code.

---

## 8. Engineering habits that pair well with Cursor

These keep quality high while you move fast:

### Prefer boring, explicit code

Ask for minimal diffs. "Smallest change that works" beats clever abstractions the agent invents once.

### Types are documentation

Generate Supabase types; pass `Database` into `createClient`. The agent makes fewer mistakes when types are strict.

### Environment variables

- `NEXT_PUBLIC_*` — browser-safe only
- `SUPABASE_SERVICE_ROLE_KEY` — server-only, never `NEXT_PUBLIC_`
- Document required vars in `.env.example` (no real values)

### Errors and logging

Ask the agent to **surface errors to the UI** or return typed `Result` objects — not silent `catch {}` blocks.

### Tests where regressions hurt

You don't need 100% coverage. Prioritize: auth flows, billing, permission checks, critical server actions.

Tell the agent: "Add a test for the happy path and one failure case" — not "add tests for everything".

---

## 9. Common failure patterns (and fixes)

| Symptom | Likely cause | What to do |
|---------|--------------|------------|
| "Works locally" but auth broken in prod | Cookie domain / middleware misconfig | Share `middleware.ts` + deployment URL; compare `@supabase/ssr` docs |
| Hydration errors | Client/server markup mismatch | Isolate `'use client'` boundary; don't fetch random data in client that server already has |
| Empty data in production | RLS blocking anon/authenticated role | Test with same user role; verify policies in Supabase dashboard |
| Giant diff | Prompt too broad | Stop, revert, rescope with template in §2 |
| Same bug "fixed" three times | Root cause not identified | Ask mode: "trace the data flow from form submit to DB" |

---

## 10. Your first week (practice plan)

| Day | Practice |
|-----|----------|
| 1 | Add/adapt `.cursor/rules` on a real project; run one scoped Agent task using the template |
| 2 | One Ask-mode session: understand an unfamiliar file before changing it |
| 3 | Deliberately review a diff with the checklist in §6 before accepting |
| 4 | Fix one bug using **full error output** + file paths in the prompt |
| 5 | One "out of scope" line in a prompt — notice how it reduces scope creep |

---

## Next lesson

**Lesson 02** will scaffold a Next.js App Router + Supabase starter in this repo (typed client, env layout, folder conventions) so later lessons have a shared lab.

When you're ready, tell Cursor: *"Let's do Lesson 02 — scaffold the Next.js + Supabase starter."*

---

## Exercises

Hands-on tasks for this lesson: [exercises.md](./exercises.md)
