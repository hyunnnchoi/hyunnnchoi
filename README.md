## Hi there 👋




#### vllm-project/vllm-metal
<details>
<summary>Found and fixed a silent out-of-bounds GPU write in <a href="https://github.com/vllm-project/vllm-metal/pull/527">vLLM's Apple Silicon backend</a> — merged into <a href="https://github.com/vllm-project/vllm-metal/releases/tag/v0.3.0.dev20260720064936">v0.3.0.dev</a>.</summary>

<br>

`--num-gpu-blocks-override` was honored by the vLLM scheduler but never reached vllm-metal's paged KV allocator, so an override above the profiled capacity let the engine address blocks that were never allocated on the Metal side.

The result was a silent out-of-bounds GPU write — the server started and reported healthy while the scheduler oversubscribed the pool. Added fail-fast validation that rejects any engine KV config larger than the allocated pool.

My first contribution to the project.
</details>

[![PR #527](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/527)](https://github.com/vllm-project/vllm-metal/pull/527)

#### lmcache/lmcache
<details>
<summary>Found and reported a race-condition crash in <a href="https://github.com/LMCache/LMCache/issues/2420">LMCache</a>'s local disk backend.</summary>

<br>

Under heavy cache eviction, `read_file` removed a missing key from the index on `FileNotFoundError`, but the caller then accessed that same key without checking — a `KeyError` that took down the whole engine.

Diagnosed the root cause, reported it with the failing code path, and verified the maintainer's fix on A100×4 over a multi-hour repro.
</details>

[![Issue #2420](https://img.shields.io/github/issues/detail/state/LMCache/LMCache/2420)](https://github.com/LMCache/LMCache/issues/2420)
