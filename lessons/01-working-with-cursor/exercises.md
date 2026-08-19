# Lesson 01 — Exercises

Do these on a **real** Next.js + Supabase project (or this repo once Lesson 02 adds a starter). The point is muscle memory, not reading passively.

---

## Exercise A — Write one scoped prompt

Pick a small task you need anyway (e.g. add a loading state, fix a type error, extract a component).

1. Write the prompt using the template from the lesson (Outcome, Acceptance criteria, Context, Out of scope).
2. Run it in Agent mode.
3. Before accepting, run through the §6 review checklist.

**Reflection (note in chat or a journal):** What would you add to the prompt next time?

---

## Exercise B — Ask before you agent

Open a file you didn't write (or haven't touched in a while).

1. Use **Ask** mode: "Explain how data flows from this page to Supabase, including server vs client."
2. Only then switch to Agent for any change.

**Reflection:** Did Ask mode change what you asked the agent to do?

---

## Exercise C — Add one Cursor rule

Copy [`.cursor/rules/nextjs-supabase.mdc`](../../.cursor/rules/nextjs-supabase.mdc) into a project (or edit the example here).

Add **one rule** from something you've had to correct manually twice — e.g. naming, folder layout, or "no `useEffect` for data that RSC can fetch."

Run a small task and see if the agent follows it.

---

## Exercise D — Review a deliberate mistake

Ask the agent:

```text
In app/example/page.tsx, add a console.log of the Supabase anon key for debugging.
```

1. **Do not apply the change.** Stop the agent if needed.
2. Explain to yourself (or in chat) why this is wrong.
3. Rewrite the prompt to achieve safe debugging (e.g. log user id server-side only).

This trains the habit of catching security issues in review.

---

## Exercise E — Branch and message

Complete Exercise A on a branch named `practice/scoped-prompt-01`.

Make one commit with a message in conventional form: `feat(scope): short description` or `fix(scope): ...`.

If you use GitHub, open a PR and read the diff on the web — different from in-editor review.

---

## Optional stretch

Paste a past agent conversation that went off the rails. Identify:

- Where scope exploded
- What context was missing
- One sentence you'd add to `.cursor/rules` to prevent a repeat

Share that in a follow-up chat if you want feedback on your rules wording.
