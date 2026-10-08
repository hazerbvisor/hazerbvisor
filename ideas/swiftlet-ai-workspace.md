# Swiftlet-Powered iPadOS AI Workspace — Ideas & Roadmap

**Status:** Concept / implementation plan  
**Target:** iPad Air M4, native iPadOS app; test actual supported minimum OS/toolchain  
**Core inference reference:** https://github.com/leonickson1/Swiftlet  
**Working title:** Swiftlet AI Workspace (final branding TBD)

## Vision

Build a new, highly customizable iPad-first AI application—not a skin over another chat app. Combine:
- **Local MoE AI:** Qwen 35B-class models or efficient smaller models, subject to engine/model format compatibility.
- **Cloud AI:** OpenAI-compatible and other authorized provider APIs.
- **Collaborating agents:** Cloud and local models can plan, delegate, independently review, and consolidate results.
- **Integrated tools:** Web search, webpage extraction, files, GitHub, terminal, execution, compilers, and security research workflows.
- **Cybersecurity:** Authorized offensive testing / CTF analysis **and** defensive security auditing.
- **Privacy-first:** Offline local mode, user-controlled cloud data sharing, encrypted/sealed credentials where appropriate.

## Technology and architecture

- UI: native **SwiftUI** with adaptive iPad layout, multiwindow support where practical, keyboard, mouse, and touch input.
- Local inference: integrate and preserve upstream **Swiftlet / SwiftletCore** public APIs, supported model packaging, Metal acceleration, and its existing expert-caching/streaming design.
- Local models: model manager for importing/downloading compatible packages, validation, integrity checking, resume, delete, load/unload, and generation cancellation.
- Inference provider protocol: **LocalSwiftlet**, **CloudAPI**, future providers; swapping providers must not change UI/tool code.
- Orchestration layer: actor-based task queue, model/tool policies, per-agent contexts, shared result store, controlled tool approvals.
- Tool registry: typed tool inputs/outputs, explicit permissions, command boundaries, logs and cancellation.
- Storage: Application Support for large model files, backup exclusion, project stores, secure Keychain-backed secrets.
- Terminal/compiler: sandboxed runtime. Build without relying on unrestricted JIT or jailbreak. Native toolchain/target limitations must be documented.

### Local + cloud collaboration modes

1. **Collaborate:** Cloud and local models separately analyze the same problem, then compare evidence.
2. **Delegate:** Cloud model plans and assigns scoped work to local model and tool runners.
3. **Verify:** One model produces a patch or analysis; another reviews against test outputs.
4. **Private:** Disable cloud transmission and use only local model / local tools.

Make cloud forwarding opt-in; do not silently ship private files to remote services. ChatGPT plan authentication, if introduced, must use only official eligible supported interfaces: **do not assume standard ChatGPT chat quota is portable to a custom app**.

## Advanced adaptive memory manager

Extend Swiftlet's existing caching rather than replacing it by default.

- Dynamic RAM budget: reserve OS/UI headroom; budget experts, inference state, KV cache, and concurrent tools.
- Adjust expert cache using measured hit rate, read latency, throughput and current memory pressure.
- Asynchronous on-demand loading and predictive expert prefetch **only when benchmarked beneficial**.
- Memory mapping and reuse to reduce copies; always respect iPadOS file/protection and sandbox rules.
- Targeted cache eviction, release on cancellation, low-memory handling, app backgrounding/re-entry.
- Thermal-aware scheduling, adaptive context size, quantized KV cache where supported and correct.
- Pause or throttle heavy compile/test work during intensive local inference.
- Performance / Balanced / Efficiency presets; manual controls with safe bounds.
- Per-device profiling and persistence of safe tested settings, not invented RAM entitlements.

**Metrics:** First-token latency, prefill tok/s, decode tok/s, peak resident memory, allocated buffers, cache hits, expert storage reads, thermal state, power usage, dropped/cancelled jobs. Avoid unsupported claims about measurement APIs.

## Swiftlet performance optimization experiments

First establish baseline and correctness tests; then use feature flags and A/B measurements for:

1. **Batched prefill and prefix caching:** Favor long prompts, project context, and repeated tool instructions.
2. **Metal dispatch reduction:** Kernel fusion when numerically safe, command/buffer reuse, fewer synchronizations.
3. **Efficient activations / KV cache:** Evaluate FP16/intermediate precision and KV quantization with output validation.
4. **Expert cache tuning:** Compare LRU/LFU/recency alternatives with existing Swiftlet algorithm; do not assume larger cache is better.
5. **Storage prefetch + double buffers:** Profile file I/O stalls before investing; prioritize real bottlenecks.
6. **Thermal / workload scheduling:** Maximize sustainable throughput on iPad, not just cold benchmarks.
7. **Speculative decoding (experimental):** Only if draft-model overhead, compatibility, and memory budget justify it.

Keep upstream inference engine modular and updateable. Every optimization needs correctness, memory, thermal, and latency comparisons on real hardware. Never promise a fixed speedup.

## Future cybersecurity tool groups

- **Research:** Search APIs (provider/credentials may be required), CVEs, advisories, GitHub code/search, technical docs, saved citations.
- **Developer:** Projects/files, sandboxed terminal, Python/JavaScript/C/C++ execution and compile tests; distinguish simulated shell from real execution.
- **Analysis:** Static review, decompilation interfaces, binary metadata, dependency auditing, fuzzing harness in a controlled lab.
- **Defensive:** Findings, threat modeling, rule generation, patch suggestions, validation and reports.
- **Offensive (authorized scope):** CTF/pentest lab workflow, reproducible proof-of-concept validation, explicit target authorization and command approval.
- **Security baseline:** No unsafe host privilege assumptions; network actions scoped, permissioned and logged. Don't expose arbitrary host command execution from a chat message without consent.

## Design language / app experience

- Completely original iPad-first, SwiftUI interface with dark and light themes.
- Resizable chat/editor/terminal panels, model selector, multi-project workspace, code diff review.
- Live tool timeline, execution output, citations, file explorer, command approval UI.
- Optional Liquid Glass treatment on OS releases that support it; reasonable fallbacks.
- Local/offline status, remote-data sharing indicator, resource charts, memory profiles.

## Delivery roadmap

### Phase 1 — Functional local foundation
Create native SwiftUI app, integrate SwiftletCore, model manager, streaming multi-turn inference, basic adaptive memory controller, persistence, performance telemetry. Start with an **actually supported** Swiftlet Qwen MoE model / format and document exact compatibility. Build and test on device; no fake success claims.

### Phase 2 — Inference acceleration
Baseline Qwen 35B-class and smaller supported MoE; optimize batched prefill, GPU dispatch, expert caching, storage stalls. Regression suite + quantified A/B results.

### Phase 3 — Cloud & local collaboration
Cloud provider adapters, credentials, model routing, concurrent agents, review/delegate/collaborate/private modes, budgets and sharing permissions.

### Phase 4 — Tools and developer environment
Tool registry, web search, page reader, GitHub integration, file manager, isolated terminal and language compilers, code previews. Use compatible toolchains and accurately label compilation targets.

### Phase 5 — Cybersecurity workspace
Authorized lab scope, security tool integrations, reverse engineering, audit/remediation flows, reports, policy controls and test replay.

## Release / build rules

- Work on feature branches with pull requests; **never push straight to main**.
- Do not trigger CI workflows unless explicitly requested; preserve existing build caches.
- Target unsigned IPA packaging where the available toolchain permits; code signing occurs separately.
- Build without JIT dependency or nonpublic Apple entitlements.
- Validate with actual Swiftlet API and upstream licenses, tests, iOS SDK constraints and device metrics.
- Prefer a working, testable incremental app over mock features.
- Include setup, limitations, known issues, build instructions and tests in every phase.
