I'm a backend engineer working with **Java / Kotlin / Spring Boot** and an open-source contributor across the Spring and JetBrains ecosystems.

> 📌 Curated by impact. The full PR history on each project — including documentation and maintenance work — is one click away via the author-search links in the project headers below.

---

## 🌱 Open Source Contributions

### [spring-projects/spring-data-jpa](https://github.com/spring-projects/spring-data-jpa/pulls?q=is%3Apr+author%3Ajewoodev) — *JPA-based data access for Spring Data*

#### ⭐ JPQL / EQL / HQL query renderer — fix 4 round-trip defects ![merged](https://img.shields.io/badge/status-merged%20%2B%20backported-blueviolet) ![issue](https://img.shields.io/badge/issue-%234272-blue) ![pr](https://img.shields.io/badge/pr-%234273-blue)

- **Problem**: Spring Data JPA's query renderers (`JpqlQueryRenderer`, `EqlQueryRenderer`, `HqlQueryRenderer`) re-emitted certain grammar elements differently from the original input during the `parse → AST → render` round-trip. I opened [#4272](https://github.com/spring-projects/spring-data-jpa/issues/4272) reproducing 3 defects in JPQL/EQL, and the maintainer's follow-up ("please also review the HQL renderer") surfaced a 4th defect in HQL.
- **Fix**: Corrected the affected `visit*()` implementations in each renderer **without changing the ANTLR grammar definitions** — the fix lives entirely in the visitor layer.
- **Scope**: All three renderers — JPQL, EQL, HQL — via visitor-implementation changes only.
- **Tests**: Added round-trip regression tests covering all four defects so future grammar/visitor changes are caught immediately.
- **Outcome**: Merged via [#4273](https://github.com/spring-projects/spring-data-jpa/pull/4273) and backported to the maintenance branches.

---

### [spring-projects/spring-ai](https://github.com/spring-projects/spring-ai/pulls?q=is%3Apr+author%3Ajewoodev) — *An Application Framework for AI Engineering*

#### ⭐ [#6035](https://github.com/spring-projects/spring-ai/pull/6035) — Fix recursive tool input schema by hoisting `$defs` to root ![merged](https://img.shields.io/badge/status-merged-blueviolet) ![milestone](https://img.shields.io/badge/milestone-2.0.0--M7-blue)

- **Problem**: Spring AI inlined parameter schemas under `properties.<param>`, but their generated `$ref`s still pointed to root-level `$defs`. For recursive parameter types, this produced an **unresolvable `$ref`** in the tool input schema.
- **Fix**: Hoist each parameter schema's `$defs` up to the wrapper schema root before inlining. Reuse structurally equal definitions, rename simple-name collisions, and rewrite peer `$ref`s inside the hoisted schema.
- **Scope**: Applied symmetrically to both `JsonSchemaGenerator` (`spring-ai-model`) and `McpJsonSchemaGenerator` (`mcp/mcp-annotations`).
- **Tests**: Added regression coverage for transitive recursive parameter schemas, duplicate equal definitions, and colliding simple-name definitions with ref rewrites.

---

### [JetBrains/koog](https://github.com/JetBrains/koog/pulls?q=is%3Apr+author%3Ajewoodev) — *JVM framework for building enterprise-ready AI agents*

#### ⭐ [#2052](https://github.com/JetBrains/koog/pull/2052) — `fix(prompt)`: Don't start a new tool call on repeated tool call id ![merged](https://img.shields.io/badge/status-merged-blueviolet)

- **Problem**: `StreamFrameFlowBuilder.emitToolCallDelta()` treated every chunk with a non-null tool-call `id` as a new tool call. For OpenAI-compatible providers that repeat the same `id` on every streaming chunk, this fragmented one logical tool call into multiple premature `ToolCallComplete` frames.
- **Fix**: A new tool call now begins only when a present `id` or `index` differs from the pending call (mirroring the existing logic in `emitReasoningDelta`). Absent / blank ids still continue the pending call.
- **Scope**: The fix lives in the shared `StreamFrameFlowBuilder`, so it propagates across **9 streaming clients** — OpenAI, OpenRouter, Mistral, Anthropic, DeepSeek, Ollama, Bedrock, Dashscope, Google.
- **Tests**: Added two regression tests covering "same id continuation" and "distinct id starts a new tool call".
