# Trove — support knowledge base

Operational notes for answering support requests. Public on purpose: everything
here is safe to disclose, so nothing is lost if it is quoted back to a customer
or extracted from a support agent.

Maintained by engineering. If a question keeps arriving that this file does not
answer, that is a gap — open an issue labelled `ask-engineering`.

---

## Symptom → cause → what to say

### "Search doesn't find something I know is there"

Three real causes, in order of likelihood.

**1. The content is minutes old.** Confluence's CQL index is eventually
consistent. Recently created questions and answers are not immediately
searchable.
→ *Tell them:* give it a few minutes and search again.

**2. They cannot read the space.** Trove makes every call as the signed-in user,
so a question in a space they lack permission for is invisible to them — as it
should be.
→ *Tell them:* check with a space administrator that they have access.

**3. They expect a match the ranking buried.** Ranking is exact title, then title
prefix, then title contains, then question body, then answer text. A term that
appears only in an answer ranks last and is marked "found in an answer".
→ *Tell them:* scroll further, and look for the "found in an answer" badge.

**4. The space is busy and the term is common elsewhere.** *(known defect, fix
pending)* Answer search asks Confluence for comments matching the term across
everything the user can read, takes the first 50 results, then keeps only those
belonging to Trove questions. In a large Confluence full of ordinary pages and
comments, unrelated matches fill those 50 slots and the real answer never makes
the cut — so the search returns nothing at all.

Recognise it by elimination: the content is days old, the user can open the page,
and they get **zero** results rather than a badly ranked one. That combination is
this defect, not the three causes above. The bigger and busier the Confluence, the
more likely it is, so enterprise customers hit it first.
→ *Tell them:* this is a known defect in how answer search is scoped, engineering
has it, and a fix is on the way. Searching a word from the question title still
works in the meantime, because title search is unaffected.

**Not a cause:** Trove not indexing answers. It does. That is the product's
central feature — cause 4 is the search being scoped too broadly, not answers
going unindexed.

### "The question I posted disappeared / went somewhere else"

Questions are pages parented to a per-space page called **Questions**. They live
in the normal page tree, so they can be found and moved in Confluence itself.
If someone moved or deleted the page in Confluence, Trove's list can go stale.
→ *Tell them:* run **Rebuild from Confluence** on the Knowledge health screen. It
re-derives counts and tags from what actually exists and deletes nothing.

### "I can't post a question — it says the title already exists"

Confluence enforces unique page titles within a space. Trove treats this as
"already asked" and opens the existing question instead of failing.
→ *Tell them:* that is expected; the existing question is the one to use.

### "My vote disappeared"

One vote per person per answer, and clicking again removes it. This is a toggle,
not a counter.
→ *Tell them:* click again to restore it.

### "Someone else marked my question's answer as accepted"

Anyone who can edit the page can accept an answer, not only the asker. This is
deliberate: it keeps knowledge maintainable after the original asker leaves.

### "AI isn't working / I don't see AI"

AI is off by default. A **space administrator** enables it under Knowledge health.
It uses Atlassian-hosted models and reads only content the asker could already
open.
→ *Tell them:* ask a space admin to turn it on.

If AI is on but answers say the evidence is thin — that is designed behaviour.
It declines rather than inventing, and offers to ask the community instead.

### "Answers show plain text, I wanted formatting"

Answers support full rich text through the standard Confluence editor. The
**question detail** field is plain text — that is a current limitation, not a bug.

### "The macro only shows some questions"

The macro shows the space's recent questions with their accepted answers inline.
Per-macro configuration does not exist yet.

---

## Current limits — do not promise around these

- The question list loads up to **500 questions per space**. Beyond that, search
  and tag filters reach the rest.
- **Question detail is plain text.** Answers are rich text.
- **No per-macro configuration.**
- The per-space index lives in one Confluence content property, which sets a
  practical ceiling on questions per space.
- **No penetration test, no SOC 2, no ISO 27001.** Say so plainly if asked.
- Interface is **English only**.

## Known open defect

On the **Knowledge health** screen, the "Worth turning into articles" list renders
centre-aligned instead of left-aligned. Cosmetic, fix pending a release.
→ If someone reports it: confirm it is known and being fixed.

---

## Pricing

Free for up to 10 users. Then per user per month: **1.00 USD** for 11–100,
**0.80** for 101–250, **0.30** for 251–1000, decreasing above that. Billing runs
through Atlassian on the existing Marketplace subscription — Trove never handles
payment details.

## Data and privacy — the short answers

- Questions are Confluence pages, answers are Confluence comments. Everything
  stays in the customer's own instance.
- No external server, no third-party processor, no analytics, no telemetry.
- Every request is made as the signed-in user, so the app cannot surface content
  someone could not already open.
- Uninstalling leaves all questions and answers in place.
- Diagnostic logs stay inside Atlassian. Error entries can incidentally contain a
  page title, which is stated openly on the security page.

Full detail: https://donsalamii.github.io/trove-docs/security.html

---

## Escalate, do not answer

- refunds, invoices, subscription changes
- personal data requests, GDPR, deletion requests
- security vulnerability reports
- legal tone, complaints, threats
- questions about infrastructure, the operator, or other customers
- anything this file does not cover and the public docs do not answer
