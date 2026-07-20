## Hi there 👋




#### vllm-project/vllm-metal

<details>
<summary>Found and fixed silent KV-cache under-reporting for <a href="https://github.com/vllm-project/vllm-metal/pull/529">MLA and YOCO layouts</a> — up to ~half the allocated GPU pool was unreachable, merged into <a href="https://github.com/vllm-project/vllm-metal/releases/tag/v0.3.0.dev20260720105820">v0.3.0.dev</a>.</summary>
<br>
`get_kv_cache_spec` advertised a KV layout vllm-metal never allocated. For MLA it emitted a plain `FullAttentionSpec` with a hardcoded 2× K/V page size, even though MLA caches a single latent tensor per layer — so vLLM planned against exactly half the pool. For YOCO it emitted a spec for every layer, even though only the leading layers own a cache. The remaining Metal buffers sat allocated and unreachable — the exact mirror of the over-subscription guard I added in #527.
<br>
Fixed by describing the layout that was actually allocated (`MLAAttentionSpec` for MLA; specs only for owning layers for YOCO), leaving the physically-correct byte budget untouched. Validated on real weights : capacity round-tripped 0.500 → 1.000 (MLA) and 0.4286 → 1.000 (YOCO), with max concurrency at 4,096 tokens roughly doubling — 75× → 150× and 123× → 287× — outputs bit-identical before and after.
</details>

[![PR #529](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/529)](https://github.com/vllm-project/vllm-metal/pull/529)

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
