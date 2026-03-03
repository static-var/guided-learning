# guided-learning/references/transcripts/02-kotlin-flow.md

## Transcript: Kotlin Flow (bridge from coroutines)

---

## Turn 1

### User
I know basic coroutines. Teach me Kotlin Flow step by step.

### Assistant
### Lesson TODO
- [ ] What Flow represents (stream of values over time)
- [ ] Cold vs hot (why it matters)
- [ ] Common operators (`map`, `filter`, `debounce`)
- [ ] Collecting safely (lifecycle)
- [ ] Error handling + cancellation
- [ ] When to use Flow vs suspend vs LiveData

### Chunk
You already know that a `suspend fun` gives you **one result** — you call it, wait, get a value back. But what about things that produce **multiple values over time**?

Imagine a search box in your app. As the user types, you want to react to each keystroke: `"k"` → `"ko"` → `"kot"` → `"kotl"` → `"kotlin"`. That's not one result — it's a stream of values arriving over time. A `suspend fun` can't model this (it returns once and it's done).

That's what `Flow` is for. Think of it like a pipe: values go in one end, transformations happen in the middle, and something collects them at the other end.

```
suspend fun getUser(): User          → one value, done
Flow<String>: "k" → "ko" → "kot"    → many values, over time
```

Here's the critical part: a Flow is **cold** by default. That means the pipe doesn't start flowing until someone puts a bucket at the end (calls `collect`). You can set up all the transformations you want — filter, map, debounce — but nothing actually runs until collection starts.

```
val searchFlow = queryChanges       // nothing happening yet
    .debounce(300)                  // still nothing
    .filter { it.length > 2 }      // still nothing
    .map { query -> search(query) } // still nothing

searchFlow.collect { results ->     // NOW everything starts
    showResults(results)
}
```

It's like writing a recipe vs. cooking it. The `flow { ... }` block is the recipe. `collect` turns on the stove.

### Question
If a Flow is cold and nothing runs until `collect` is called — what happens if you set up a Flow but nobody ever collects it?

<!-- LESSON_STATE: topic=flow step=1 mastery=0.25 last_q=cold-no-collect -->

---

## Turn 2

### User
It won't execute / emit anything.

### Assistant
### Lesson TODO
- [x] What Flow represents (stream of values over time)
- [x] Cold vs hot (why it matters)
- [ ] Common operators (`map`, `filter`, `debounce`)
- [ ] Collecting safely (lifecycle)
- [ ] Error handling + cancellation
- [ ] When to use Flow vs suspend vs LiveData

### Chunk
✅ Correct — zero work happens. The flow builder lambda never even executes. No emissions, no side effects, nothing. Cold means "lazy until collected."

Now let's walk through operators by building a real search pipeline step by step. Imagine the user is typing in a search box and each keystroke emits the current query text:

```
Emissions: "k" → "ko" → "kot" → "kotl" → "kotlin"
           (each one ~100ms apart as user types)
```

First problem: you don't want to fire a network request on every single keystroke. That's 5 requests in 500ms — wasteful. So you add `debounce(300)`, which waits 300ms of silence before letting a value through:

```
"k"(0ms) → "ko"(100ms) → "kot"(200ms) → "kotl"(300ms) → "kotlin"(500ms)
                                                                    ↓
debounce(300ms) only lets through:                            "kotlin"
(because the user paused after typing "kotlin")
```

Second problem: searching for `"k"` or `"ko"` is useless — too short. Add `filter { it.length > 2 }` to drop anything with 2 or fewer characters.

Third: turn the query string into actual search results with `map { query -> searchApi.search(query) }`.

The pipeline reads like a recipe — each step feeds into the next, and nothing runs until someone collects:

```
queryChanges → debounce(300ms) → filter(length > 2) → map(→ API call) → collect
```

### Question
In this pipeline, if the user types "kotlin" and pauses, then immediately types "kotlin coroutines" — what does `debounce(300)` do with the "kotlin" emission that already passed through?

<!-- LESSON_STATE: topic=flow step=2 mastery=0.4 last_q=debounce-already-passed -->
