# Learning from Cursor

An ongoing, hands-on course for building with **Next.js (App Router)** and **Supabase** — while learning how to work effectively with Cursor and keep good engineering habits.

## Who this is for

You're already building things. This course focuses on **how to collaborate with an AI coding agent** without letting quality slip: scoping work, reviewing output, structuring repos, and shipping safely.

## Course map

| Lesson | Topic | Status |
|--------|-------|--------|
| [01 — Working with Cursor](./lessons/01-working-with-cursor/README.md) | Prompting, scoping, review habits, project rules | **Start here** |
| 02 — Next.js + Supabase project skeleton | App Router layout, env vars, typed Supabase client | Coming next |
| 03 — Auth & data access patterns | RLS, server vs client components, route handlers | Planned |
| 04 — Testing & CI habits | What to test, when to let the agent write tests | Planned |

## How each lesson works

1. **Read** the lesson (concepts + examples tied to your stack).
2. **Do** the exercises in your repo or a scratch project.
3. **Ask** follow-up questions in Cursor — that's part of the course.
4. **Commit** what you learn; this repo is your notebook.

## Quick reference

- [Engineering habits checklist](./docs/habits-checklist.md) — pin this during agent sessions
- [Example Cursor rules](./.cursor/rules/nextjs-supabase.mdc) — copy/adapt for your projects

## Suggested workflow with Cursor

```
You define the outcome  →  Agent proposes a small change  →  You review the diff
        ↑                                                        ↓
   You clarify constraints                              You run/test locally
        ↑                                                        ↓
   Next scoped task  ←  You merge when satisfied  ←  Fix or iterate
```

Start with [Lesson 01](./lessons/01-working-with-cursor/README.md).
