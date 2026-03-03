# guided-learning/references/style-examples.md

# Guided Learning Skill — Style Examples (Golden Patterns)

Use this file when you’re drifting or tempted to dump too much text.

## Always follow this structure
### Lesson TODO
- [ ] 5–12 items, action-oriented
- mark done with [x]
- reorder if prerequisites change

### Chunk
- 2–3 short paragraphs max
- simple words, define jargon in 1 line
- one idea per turn (don’t “finish” the whole topic)

### Visual (optional)
- exactly ONE: table OR mermaid OR ASCII
- only if it clarifies structure, flow, comparison, or timeline

### Question
- exactly ONE question
- end the message right after the question (no trailing notes)

<!-- LESSON_STATE: ... -->

---

## Good mini-example (right pacing)
### Lesson TODO
- [ ] What X is
- [ ] Why X exists
- [ ] Basic workflow
- [ ] Edge cases
- [ ] Practice

### Chunk
X is ___. It helps because ___.

Key intuition: ___.

### Visual (optional)
| Term | Meaning |
|---|---|
| A | ... |
| B | ... |

### Question
What do you think happens if ___?

---

## Bad example (what NOT to do)
- Long essay with 10 paragraphs
- 5 diagrams + code + links
- multiple questions (“Also… and also…”)
- no TODO, no continuation plan

---

## Question patterns that create thinking
Prefer:
- prediction: “What happens if…?”
- edge case: “How does it behave when…?”
- compare: “Which would you pick here and why?”
- debug: “Given this symptom, what’s the likely cause?”

Avoid:
- trivia (“What year was…?”)
- yes/no that needs no reasoning (“Is this good?”)

---

## Grading patterns (keep it short)
- ✅ Correct / ⚠️ Partially / ❌ Not quite
- 2–6 lines why
- if wrong: hint first, unless user asked for full solution

Examples:
- ✅ Correct. The key is ___.
- ⚠️ Close. You missed ___; the fix is ___.
- ❌ Not quite. Hint: look at ___ (then ask a refined question next turn).

---

## Branching pattern (when brain teaser isn’t natural)
Offer 2–3 options but phrase as ONE question:
“Pick one: (A) ___, (B) ___, or (C) ___?”

---

## Web freshness rule (don’t lie)
If web/search tools are available:
- prefer official docs/specs/vendor changelogs
- cite 1–3 sources max

If not available:
- explicitly say you can’t verify freshness here
- ask for a link OR proceed with stable fundamentals

Never claim “latest” without verification.

---

## Common failure modes (and fixes)
- Too verbose → cut to 2 paragraphs; move detail into future TODO items
- Too many visuals → only 1 visual; remove the rest
- Multiple questions → merge into 1 “pick A/B/C” question
- Losing context → update TODO first, every time