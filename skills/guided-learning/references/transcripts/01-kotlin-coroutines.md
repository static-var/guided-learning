# guided-learning/references/transcripts/01-kotlin-coroutines.md

## Transcript: Kotlin Coroutines (beginner)

---

## Turn 1

### User
Teach me Kotlin coroutines from scratch (Android context).

### Assistant
### Lesson TODO
- [ ] Coroutines vs threads (why they exist)
- [ ] `suspend` + "suspension" intuition
- [ ] Scope + structured concurrency (lifecycle tie-in)
- [ ] Dispatchers (`Main/IO/Default`) and blocking vs suspending
- [ ] `launch` vs `async` + cancellation basics
- [ ] Common pitfalls (`GlobalScope`, leaks, blocking main)

### Chunk
Imagine your Android app needs to fetch a user profile from the network. On a normal thread, the code would look like:

```
fun loadProfile() {
    val user = api.getUser()  // blocks thread, waits for network
    textView.text = user.name  // update UI
}
```

If you run this on the main thread, the whole UI freezes while waiting for the network — no scrolling, no button presses, nothing. The classic fix is to spin up a new thread for the network call, then switch back to the main thread to update the UI. But threads are expensive — each one eats ~1MB of stack memory, and creating/destroying them isn't free.

A coroutine is a lightweight alternative. Instead of blocking a whole thread while waiting, it **suspends** — it steps aside, frees the thread, and picks back up later when the result arrives. Think of it like putting a bookmark in your code. The thread can go do other work, and when the network responds, your code resumes from the bookmark.

The result: you can have thousands of coroutines running on just a handful of threads, and your code still reads top-to-bottom like normal sequential code — no callback nesting.

```
Thread approach:     1 thread per task → 1000 tasks = 1000 threads (~1GB RAM)
Coroutine approach:  1000 coroutines → shared thread pool (a few MB)
```

### Question
If a coroutine "suspends" and frees the thread, does that mean coroutines don't use threads at all?

<!-- LESSON_STATE: topic=coroutines step=1 mastery=0.2 last_q=threads-needed -->

---

## Turn 2

### User
They still use threads, just managed by dispatchers/schedulers. Cheap means not 1:1 thread.

### Assistant
### Lesson TODO
- [x] Coroutines vs threads (why they exist)
- [ ] `suspend` + "suspension" intuition
- [ ] Scope + structured concurrency (lifecycle tie-in)
- [ ] Dispatchers (`Main/IO/Default`) and blocking vs suspending
- [ ] `launch` vs `async` + cancellation basics
- [ ] Common pitfalls (`GlobalScope`, leaks, blocking main)

### Chunk
✅ Correct. Coroutines run **on top of** threads — a dispatcher decides which thread pool to use, and coroutines hop between threads as needed.

Now let's walk through what `suspend` actually means. Say you write this:

```kotlin
suspend fun getUser(): User {
    val response = httpClient.get("/user")  // suspends here
    return response.toUser()
}
```

When execution hits `httpClient.get(...)`, the coroutine doesn't spin-wait or block. Instead, it saves its state (local variables, where it was in the function) and **releases the thread**. The thread is now free to run other coroutines.

Later, when the HTTP response arrives, the dispatcher picks up a thread (maybe the same one, maybe different) and resumes your coroutine right where it left off — `response` now holds the result, and `toUser()` runs.

Here's the key insight: `suspend` doesn't mean "runs in the background." It means "this function **might pause here**." The function itself doesn't decide which thread it runs on — that's the dispatcher's job. You could call a `suspend fun` on `Dispatchers.Main` and it would still suspend/resume on the main thread.

```
Before suspend:    Thread-1 running coroutine-A
During suspend:    Thread-1 free → picks up coroutine-B
After resume:      Thread-3 resumes coroutine-A (dispatcher chose Thread-3)
```

### Question
If `suspend` doesn't control which thread you're on, is a `suspend fun` that does network I/O automatically safe to call from the main thread?

<!-- LESSON_STATE: topic=coroutines step=2 mastery=0.35 last_q=suspend-main-safety -->
