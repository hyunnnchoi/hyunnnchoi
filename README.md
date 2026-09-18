## Hi there 👋

### Publications

**Accurate Simulation of Distributed Training Jobs with Network Contention Modeling**, **[IEEE MASCOTS 2026](https://mascots26.iitis.pl/)** (Genova, Italy, Oct 2026, acceptance rate 23%), co-first author. A GPU cluster simulator for distributed training jobs under dynamic network contention. [[code](https://github.com/OSSS-KU/MoSim)]

### Open source

**[vllm-project/vllm-metal](https://github.com/vllm-project/vllm-metal)** (vLLM on Apple Silicon)

- [#747](https://github.com/vllm-project/vllm-metal/pull/747): `--kv-cache-dtype fp8` and the other quantized dtypes were accepted and logged as a memory saving, but the KV pool was still allocated in the model dtype. They are now rejected at startup with a pointer to the options that do work. Merged.
- [#529](https://github.com/vllm-project/vllm-metal/pull/529): for MLA and YOCO models the reported KV cache spec didn't match what was actually allocated, so vLLM planned against about half of the pool. With the fix, usable capacity on the models I tested went from 0.50 to 1.00 (MLA) and from 0.43 to 1.00 (YOCO), with identical outputs. Merged.
- [#527](https://github.com/vllm-project/vllm-metal/pull/527): `--num-gpu-blocks-override` reached the scheduler but not the Metal KV allocator, so a large override let the engine write to blocks that were never allocated. Added a check at startup. Merged, and my first PR to the project.

**[vllm-project/vllm-omni](https://github.com/vllm-project/vllm-omni)**

- [#5295](https://github.com/vllm-project/vllm-omni/issues/5295): the DreamZero OpenPI server stopped serving after about four sessions because AR-Diffusion KV blocks were never released, while `/health` kept returning 200. I reported it with the root cause and sent a fix ([#5296](https://github.com/vllm-project/vllm-omni/pull/5296)). A rewrite of the session lifecycle landed in the meantime and covers it, so I closed mine. Follow-up in [#5300](https://github.com/vllm-project/vllm-omni/pull/5300).

**[LMCache/LMCache](https://github.com/LMCache/LMCache)**

- [#2420](https://github.com/LMCache/LMCache/issues/2420): under heavy eviction the local disk backend crashed the engine with a `KeyError`. `read_file` dropped a missing key from the index and the caller then used that key without checking. I reported it with the failing code path and checked the maintainer's fix with a multi-hour run on 4×A100.

### In review

- vllm-project/vllm: KV offload load path ([#57343](https://github.com/vllm-project/vllm/pull/57343), [#57569](https://github.com/vllm-project/vllm/pull/57569))
- vllm-project/vllm-omni: GR00T-N1.7 request-level batching ([#6568](https://github.com/vllm-project/vllm-omni/pull/6568)) and compile setup for the action head ([#6450](https://github.com/vllm-project/vllm-omni/pull/6450))

[![MASCOTS 2026](https://img.shields.io/badge/IEEE%20MASCOTS%202026-accepted%20(23%25)-2ea44f)](https://mascots26.iitis.pl/)
[![PR #747](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/747)](https://github.com/vllm-project/vllm-metal/pull/747)
[![PR #529](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/529)](https://github.com/vllm-project/vllm-metal/pull/529)
[![PR #527](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/527)](https://github.com/vllm-project/vllm-metal/pull/527)
[![Issue #2420](https://img.shields.io/github/issues/detail/state/LMCache/LMCache/2420)](https://github.com/LMCache/LMCache/issues/2420)
