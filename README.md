## Hi there 👋

### Publications

**Accurate Simulation of Distributed Training Jobs with Network Contention Modeling**, **[IEEE MASCOTS 2026](https://mascots26.iitis.pl/)** (Genova, Italy, Oct 2026, acceptance rate 23%), co-first author. A GPU cluster simulator for distributed training jobs under dynamic network contention. [[paper](https://arxiv.org/pdf/2609.23278)] [[code](https://github.com/OSSS-KU/MoSim)]

### Open source

**[vllm-project/vllm](https://github.com/vllm-project/vllm)**

- [#57343](https://github.com/vllm-project/vllm/pull/57343): Made the KV load path follow the actual copy size instead of the page size, cutting CPU prefix-hit TTFT from 88 to 46 ms. In review.
- [#57569](https://github.com/vllm-project/vllm/pull/57569): Added an opt-in calibration that sets the DMA/Triton load cutoff per GPU instead of using constants tuned on one H100. Stacked on #57343, in review.
- [#59999](https://github.com/vllm-project/vllm/pull/59999): Found a mismatch between the LHBNC KV layout and the FlashInfer attention backend, and prevented the startup crash it caused. Merged.
- [#60005](https://github.com/vllm-project/vllm/pull/60005): Found the same layout silently corrupting CPU KV offload, and blocked it at startup. In review.

  
**[vllm-project/vllm-metal](https://github.com/vllm-project/vllm-metal)** (vLLM on Apple Silicon)

- [#747](https://github.com/vllm-project/vllm-metal/pull/747): Rejected quantized KV cache dtypes at startup, which were accepted and logged as a memory saving while the pool stayed in the model dtype. Merged.
- [#529](https://github.com/vllm-project/vllm-metal/pull/529): Fixed the KV cache spec for MLA and YOCO models so vLLM plans against the full pool instead of about half, raising usable capacity from 0.50 to 1.00 (MLA) and 0.43 to 1.00 (YOCO). Merged.
- [#527](https://github.com/vllm-project/vllm-metal/pull/527): Added a startup check for `--num-gpu-blocks-override`, which reached the scheduler but not the Metal allocator and let the engine write to unallocated blocks. Merged, my first PR to the project.

**[vllm-project/vllm-omni](https://github.com/vllm-project/vllm-omni)**

- [#5295](https://github.com/vllm-project/vllm-omni/issues/5295): Found AR-Diffusion KV blocks never being released, which stopped the DreamZero OpenPI server after about four sessions while `/health` still returned 200. My fix ([#5296](https://github.com/vllm-project/vllm-omni/pull/5296)) was superseded by a session lifecycle rewrite; follow-up in [#5300](https://github.com/vllm-project/vllm-omni/pull/5300).

**[LMCache/LMCache](https://github.com/LMCache/LMCache)**

- [#2420](https://github.com/LMCache/LMCache/issues/2420): Found a `KeyError` crash in the local disk backend under heavy eviction, and verified the maintainer's fix with a multi-hour run on 4×A100.
  
### In review

- vllm-project/vllm: KV offload load path ([#57343](https://github.com/vllm-project/vllm/pull/57343), [#57569](https://github.com/vllm-project/vllm/pull/57569)), KV tensor shape bugfix ([#59999](https://github.com/vllm-project/vllm/pull/59999), [#60005](https://github.com/vllm-project/vllm/pull/60005))
- vllm-project/vllm-omni: GR00T-N1.7 request-level batching ([#6568](https://github.com/vllm-project/vllm-omni/pull/6568)) and compile setup for the action head ([#6450](https://github.com/vllm-project/vllm-omni/pull/6450))

[![MASCOTS 2026](https://img.shields.io/badge/IEEE%20MASCOTS%202026-accepted%20(23%25)-2ea44f)](https://mascots26.iitis.pl/)
[![vLLM #59999](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm/59999)](https://github.com/vllm-project/vllm/pull/59999)
[![vLLM-metal #747](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/747)](https://github.com/vllm-project/vllm-metal/pull/747)
[![vLLM-metal #529](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/529)](https://github.com/vllm-project/vllm-metal/pull/529)
[![vLLM-metal #527](https://img.shields.io/github/pulls/detail/state/vllm-project/vllm-metal/527)](https://github.com/vllm-project/vllm-metal/pull/527)
[![LMCache #2420](https://img.shields.io/github/issues/detail/state/LMCache/LMCache/2420)](https://github.com/LMCache/LMCache/issues/2420)
