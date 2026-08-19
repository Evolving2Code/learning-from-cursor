# Engineering habits checklist (use during Cursor sessions)

Pin or keep this open when an agent is editing your Next.js + Supabase project.

## Before you prompt

- [ ] One clear outcome (not a wish list)
- [ ] Acceptance criteria listed
- [ ] Relevant file paths attached
- [ ] Out of scope stated
- [ ] Ask mode used if you don't understand the area yet

## While the agent works

- [ ] Diff staying roughly on scope?
- [ ] No surprise new dependencies?
- [ ] No new env vars without `.env.example` update?

## Before you merge / accept

- [ ] Reviewed file list — no unrelated deletes
- [ ] No secrets in client code or committed files
- [ ] Supabase: RLS still enforced; no service role in browser
- [ ] Server vs `'use client'` boundaries correct
- [ ] Types look right — minimal `any`
- [ ] Ran locally on affected routes
- [ ] Commit message describes *why*, not just *what*

## Supabase quick checks

- [ ] Server client: `createServerClient` / cookies pattern
- [ ] Browser client: anon key only, `NEXT_PUBLIC_SUPABASE_URL`
- [ ] Mutations: server actions or route handlers, not raw client bypass of RLS
- [ ] Types regenerated if schema changed

## When something goes wrong

1. Stop and narrow scope — don't pile fixes on a bad diff
2. Paste **full** error output + file paths
3. Ask mode: trace data flow before another big Agent attempt
