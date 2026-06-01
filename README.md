I'm a backend engineer working with **Java / Kotlin / Spring Boot** and open-source contributor on `spring-projects/spring-ai`, currently exploring schema generation, transport-layer plumbing, and provider documentation consistency. I wo

- 📝 Tech blog: [jewoodev.github.io](https://jewoodev.github.io)

---

## 🌱 Open Source Contributions

### [spring-projects/spring-ai](https://github.com/spring-projects/spring-ai/pulls?q=is%3Apr+author%3Ajewoodev)

> An Application Framework for AI Engineering

#### [#6035](https://github.com/spring-projects/spring-ai/pull/6035) — Fix recursive tool input schema by hoisting `$defs` to root ![status](https://img.shields.io/badge/status-merged-blueviolet)

- **Problem**: Spring AI inlined parameter schemas under `properties.<param>`, but their generated `$ref`s still pointed to root-level `$defs`. For recursive parameter types, this produced an **unresolvable `$ref`** in the tool input schema.
- **Fix**: Hoist each parameter schema's `$defs` up to the wrapper schema root before inlining. Reuse structurally equal definitions, rename simple-name collisions, and rewrite peer `$ref`s inside the hoisted schema.
- **Scope**: Applied symmetrically to both `JsonSchemaGenerator` (`spring-ai-model`) and `McpJsonSchemaGenerator` (`mcp/mcp-annotations`).
- **Tests**: Added regression coverage for transitive recursive parameter schemas, duplicate equal definitions, and colliding simple-name definitions with ref rewrites.

#### [#6093](https://github.com/spring-projects/spring-ai/pull/6093) — Complete documentation update for OpenAI SDK base URL new behavior ![status](https://img.shields.io/badge/status-merged-blueviolet) ![milestone](https://img.shields.io/badge/milestone-2.0.0--M7-blue)

Follow-up to [#6036](https://github.com/spring-projects/spring-ai/issues/6036), completing the corrections started in commit `192eb5c`.

- **Problem**: After the `openai-java` SDK migration, OpenAI-compatible base URLs and several `*-path` properties were inconsistent across 8 documentation files (DMR, Ollama, Groq, Mistral AI, Embeddings, Speech, Transcriptions, Moderation). Some still missed `/v1`; some still documented properties that had been dropped from the SDK.
- **Fix**: Aligned all OpenAI-compatible endpoints to include `/v1` (e.g. `http://localhost:12434/engines/v1`, `https://api.groq.com/openai/v1`), removed table rows and sections for properties dropped by the SDK migration, and verified AsciiDoc structure stayed intact.
- **Outcome**: Merged into milestone **2.0.0-M7** ([`d7f3c6c`](https://github.com/spring-projects/spring-ai/commit/d7f3c6c0d0cbf2ffb2513b1cc79c1caf96de792c)).
