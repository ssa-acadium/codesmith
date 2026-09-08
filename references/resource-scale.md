# Resources and Realistic Scale

Do not confuse premature optimization with avoiding obviously wasteful ownership and I/O patterns.

## Know the shape of the workload

Before choosing a data path, identify plausible:

- input/file size;
- item cardinality;
- concurrency;
- operation frequency;
- latency sensitivity;
- memory ceiling;
- whether data is bounded by contract or merely small in development.

A design does not need production benchmarking to avoid an unnecessary whole-file copy or quadratic scan when an equally simple bounded alternative exists.

## File handling

Prefer streaming or bounded chunks when files may grow. Reading an entire file is fine when the contract genuinely bounds it and the bound is safe.

For writes:

- close/flush resources structurally;
- define partial-failure behavior;
- use temporary file + atomic replace when a half-written destination would be harmful and the platform supports it;
- avoid repeated full-file rescans inside loops;
- do not keep file handles or buffers alive beyond their useful scope.

## Memory and queues

Tracked collections, caches, pending-work maps, subscriber lists, and queues need a bound, eviction/expiry, backpressure, or a demonstrated natural ceiling.

If producers can outpace consumers indefinitely, memory is the buffer. Make that choice explicit rather than accidental.

## Databases and remote calls

Prefer native bulk/set operations over per-item loops when the database/service supports them. Watch for:

- N+1 queries;
- one network request per row when batching exists;
- loading all rows before filtering/paginating;
- retry storms with no bound/backoff/idempotency plan.

Do not introduce a distributed batching subsystem for ten rows. Use the simplest native batching mechanism that matches the real workload.

## Complexity

Choose an algorithm whose growth is sane for plausible inputs. Do not replace clear O(n) code with intricate micro-optimization without evidence; likewise, do not keep accidental O(n²) behavior because test fixtures contain 20 items.

The Codesmith standard is **cheap correctness at scale**: take the scalable form when it is equally or nearly as simple; measure before paying substantial complexity for further optimization.
