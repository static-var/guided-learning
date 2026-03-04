# guided-learning/references/style-examples.md

# Guided Learning Skill — Style Examples (Golden Patterns)

Use this file when you're drifting toward dry summaries instead of walkthroughs.

## Always follow this structure
### Lesson TODO
- [ ] 5–12 items, action-oriented
- mark done with [x]
- reorder if prerequisites change

### Chunk
- Narrate a walkthrough, not a summary
- "Imagine this…" → walk through steps → show state → name the pattern
- Embed ASCII visuals inline (queue state, tree traversal, stack trace) — never as a separate section
- No mermaid diagrams (many tools can't render them)
- Tables are fine when comparing things, keep them inline

### Question
- exactly ONE question
- should bridge naturally to the next TODO item
- end the message right after the question (no trailing notes)

<!-- LESSON_STATE: ... -->

---

## Good example: walkthrough style (BFS)
### Lesson TODO
- [ ] Walk through BFS traversal
- [ ] Walk through DFS traversal
- [ ] Compare BFS vs DFS use cases
- [ ] Shortest path in unweighted graphs
- [ ] Practice: choose the right algorithm

### Chunk
Imagine you're standing at node `A` in this graph:

```
    A
   / \
  B   C
 / \   \
D   E   F
```

You want to visit every node. Here's one strategy: visit all neighbors first, then their neighbors.

You start at `A` and put it in a queue. Then you pull from the front and add its neighbors:

```
Visit A → Queue: [B, C]
Visit B → Queue: [C, D, E]
Visit C → Queue: [D, E, F]
Visit D → Queue: [E, F]
Visit E → Queue: [F]
Visit F → Queue: []
```

Notice the pattern? You went `A → B → C → D → E → F` — level by level, left to right. You visited everything at distance 1 from `A` before anything at distance 2. This process is called **Breadth-First Search (BFS)** — "breadth" because you sweep wide across each level before going deeper.

The queue is the key: it enforces "first discovered, first visited." That's what keeps things level-by-level.

### Question
Now imagine you used a **stack** instead of a queue (last in, first out). Starting from `A`, what order would you visit the nodes?

<!-- LESSON_STATE: topic=bfs-dfs step=1 mastery=0.2 last_q=stack-traversal-order -->

---

## Bad example (what NOT to do)
- "BFS uses a queue and explores level by level. DFS uses a stack and goes deep first." ← This is a summary, not teaching.
- Separate `### Visual (optional)` section with a mermaid diagram nobody can render
- Long essay with 10 paragraphs covering BFS, DFS, Dijkstra, and A* all at once
- Multiple questions ("Also… and also…")
- No TODO, no continuation plan
- Question that doesn't lead anywhere: "What does BFS stand for?"

---

## Good grading + continuation example
### User
With a stack, you'd go A → C → F → B → E → D… I think? Because you push B and C, pop C first, push F, pop F, then back to B?

### Assistant
⚠️ Close! You've got the right idea — a stack reverses the order so you go deep before wide. The exact order depends on which neighbor you push first.

If we push left-before-right (B then C), the stack pops C first:

```
Visit A → Stack: [B, C]
Pop C  → Visit C → Stack: [B, F]
Pop F  → Visit F → Stack: [B]
Pop B  → Visit B → Stack: [D, E]
Pop E  → Visit E → Stack: [D]
Pop D  → Visit D → Stack: []
```

Order: `A → C → F → B → E → D`. You went deep down the right side first, then came back. This is **Depth-First Search (DFS)** — "depth" because you dive as deep as possible before backtracking.

---

## Question patterns that create thinking
Prefer:
- walkthrough prediction: "Starting from node X, what does the queue look like after 3 steps?"
- edge case: "What happens if the graph has a cycle? Does BFS still work?"
- comparison: "You need to find if ANY path exists in a huge graph. BFS or DFS — which would you pick and why?"
- bridge question: "If you need the shortest path in an unweighted graph, which of these two approaches guarantees it?"

Avoid:
- trivia ("What year was BFS invented?")
- yes/no that needs no reasoning ("Is BFS good?")
- definition recall ("What data structure does BFS use?")

---

## Grading patterns (keep it short)
- ✅ Correct / ⚠️ Partially / ❌ Not quite
- 2–6 lines of walkthrough correction
- if wrong: give one hint before giving full solution (unless user asked for solution)

---

## Branching pattern (when brain teaser isn't natural)
Offer 2–3 options but phrase as ONE question:
"Pick one: (A) ___, (B) ___, or (C) ___?"

---

## Web freshness rule (don't lie)
Only applies to **versioned topics** (libraries, frameworks, SDKs, databases, OSes, APIs).
Skip web search entirely for stable concepts (algorithms, patterns, data structures, CS theory).

If the topic is versioned and web/search tools are available:
- prefer official docs/specs/vendor changelogs
- cite 1–3 sources max

If versioned but no web access:
- explicitly say you can't verify freshness here
- ask for a link OR proceed with stable fundamentals

---

## Common failure modes (and fixes)
- Dry summary instead of walkthrough → narrate a scenario, trace through steps, show state
- Mermaid diagrams → use ASCII art inline; it works everywhere
- Separate "Visual" section → embed visuals inside the chunk text
- Multiple questions → merge into 1 bridging question
- Question doesn't connect to next topic → rephrase so the answer leads into the next TODO item
- Losing context → update TODO first, every time
