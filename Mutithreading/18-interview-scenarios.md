# 18. Interview Scenarios and Quick Revision

## Scenario 1: Two threads update the same counter

**Problem:** Final value is lower than expected.

**Likely cause:** Race condition caused by non-atomic `count++`.

**Possible fixes:** `synchronized`, `AtomicInteger`, or another suitable concurrency design.

## Scenario 2: Application creates too many threads

**Problem:** High memory usage, context switching and unstable response times.

**Approach:** Use a bounded and configured thread pool, define queue capacity and rejection behavior, monitor pool metrics, and tune using workload measurements.

## Scenario 3: One service call is slow

**Problem:** A worker thread stays blocked while waiting for a remote API.

**Approach:** Set connection/read/overall timeouts, limit concurrency, consider asynchronous I/O where appropriate, and use circuit breakers or bulkheads where justified. Do not assume adding threads alone solves the bottleneck.

## Scenario 4: Two threads are stuck

**Possible cause:** Deadlock.

**Approach:** Take a thread dump, inspect which locks threads own and wait for, identify cycles, and fix lock ordering or lock scope.

## Scenario 5: Background task must return a value

Use `Callable<T>` with `ExecutorService.submit()` and inspect the `Future`, or use `CompletableFuture<T>` if you need to compose asynchronous steps.

## Scenario 6: Stop a worker safely

Use interruption or a clear cancellation flag, and make the worker cooperate. Avoid deprecated forced-stop mechanisms.

## Rapid revision table

| Topic | Remember |
|---|---|
| `start()` vs `run()` | `start()` starts a new thread; direct `run()` does not |
| `synchronized` | Monitor-based mutual exclusion and visibility |
| Race condition | Result depends on timing/interleaving |
| Deadlock | Threads wait cyclically for locks |
| `wait()` | Releases the object's monitor |
| `sleep()` | Does not release monitors |
| `volatile` | Visibility/order guarantees, not atomic increment |
| `AtomicInteger` | Atomic counter operations |
| `Runnable` | No return value from task |
| `Callable` | Can return a value and throw checked exceptions |
| `execute()` | No Future returned |
| `submit()` | Returns a Future |
| `Future.get()` | Blocks until result or failure is available |
| `CompletableFuture` | Compose async stages |
| Thread pool | Reuse workers; configure capacity |
| Daemon thread | JVM need not wait for it |
| `interrupt()` | Cooperative signal, not forced termination |
| `ForkJoinPool` | Split tasks and work-stealing |


