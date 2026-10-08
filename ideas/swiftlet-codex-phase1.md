# Codex Prompt — Phase 1: Swiftlet iPadOS AI Workspace

Copy the text below into Codex. The **feature branch/PR requirement applies to the destination app repository**, not this ideas/documentation repository.

```text
/goal
Build Phase 1 of an original native iPadOS AI coding workspace.
Engine: https://github.com/leonickson1/Swiftlet
Plan: https://github.com/hazerbvisor/hazerbvisor/blob/main/ideas/swiftlet-ai-workspace.md (or this PR branch until merged)

TARGET: iPad Air M4, Swift 6+, SwiftUI, Metal, SwiftPM. Verify Swiftlet's real minimum iOS version and public APIs. Aim for unsigned IPA/XTool Mobile compatibility, but document unsupported host SDK/build steps.

GIT: Refresh main; use a new feature branch; never push to main. No GitHub Actions unless requested. Preserve caches. Commit code and open a PR if allowed.

1) SWIFTLET
- Add SwiftletCore through a real supported integration.
- Use verified API names; do not invent SwiftletSession APIs.
- Implement streaming chat, cancellation, multi-turn state, loading/unloading, robust failure states.
- Target an actual Swiftlet-supported Qwen MoE .qpack; 35B-A3B is the long-term goal, not a guarantee.
- Preserve upstream Metal/streaming/caching initially. No bundled huge weights.

2) MODEL MANAGER
- Import, download/resume, progress, checksum when provided, space checks, delete, model selection.
- Store weights in Application Support, exclude from backup; handle interrupted imports.
- Support additional compatible models without UI rewrites.

3) ADAPTIVE MEMORY
- Actor-based memory coordinator alongside Swiftlet's native expert cache.
- Budget memory dynamically; preserve system/UI headroom.
- Profiles: Performance, Balanced, Efficiency.
- Observe memory pressure and thermal state using supported iPadOS APIs.
- Shrink/evict caches and cancel work safely, without assuming extra memory entitlements.
- Collect actual cache stats only where Swiftlet exposes them; label unavailable metrics.

4) BENCHMARKS
- Measure time-to-first-token, prefill/decode throughput, peak memory, model-load time, thermal state.
- Build reproducible baseline benchmarks with correctness tests.
- Document later experiments: batched prefill, prefix caching, Metal kernel fusion, FP16 intermediates, async expert prefetch. Do not implement speculative low-level rewrites prematurely.

5) ORIGINAL UI
- Custom SwiftUI chat, streamed messages, sidebar, conversation history, model manager, memory/performance panel, settings.
- Dark/light modes, elegant responsive iPad design, keyboard/mouse/touch; tasteful adaptive materials.
- No fake working buttons or placeholder features labeled as done.

6) EXTENSIBILITY
- Protocols/interfaces for cloud AI providers, local/cloud agent orchestration, permissioned tool registry, web search, filesystem/terminal, compilers, GitHub, cybersecurity labs.
- Future interfaces only in this phase; avoid nonfunctional stubs posing as implemented tools.
- Future cloud provider keys in Keychain. Local inference remains fully offline.

7) RELIABILITY / SECURITY
- Handle backgrounding, low disk, thermal pressure, cancellation, memory pressure and app lifecycle.
- Sandboxed I/O; no secret tokens in repo; no JIT, jailbreak, private APIs, or unsafe privilege assumptions.
- Preserve third-party licenses.

VALIDATE & DELIVER:
Implement real code and tests; compile with available Xcode/iOS toolchain. If iOS SDK or signing is unavailable, run meaningful available checks, describe exact blockers, and never claim IPA/device tests passed. Supply setup/build commands, files changed, tests, known limitations, baseline measurement method, and PR link.

Definition of done: a genuine SwiftUI app that can manage a compatible model, load it through Swiftlet, stream a conversation, cancel generation, persist chats and display truthful memory/performance telemetry.
```

## Phase 1 output checklist

- [ ] App project / SwiftPM dependencies
- [ ] Swiftlet integrated using verified current APIs
- [ ] On-device compatible model loading and streaming
- [ ] Model import/download/storage manager
- [ ] Adaptive safe memory profiles
- [ ] Benchmark/telemetry screen with real readings
- [ ] Tests, build instructions, known limitations
- [ ] Feature branch + PR; no direct commits to main
