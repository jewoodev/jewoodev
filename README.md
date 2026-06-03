I'm a backend engineer working with **Java / Kotlin / Spring Boot** and an open-source contributor across the Spring, Micrometer, and JetBrains AI ecosystems.

> 🌱 **6 merged PRs across 4 projects** — Spring AI, JetBrains/koog, Micrometer, Spring Data JPA

---

## 🌱 Open Source Contributions

### [spring-projects/spring-ai](https://github.com/spring-projects/spring-ai/pulls?q=is%3Apr+author%3Ajewoodev+is%3Aclosed) — *An Application Framework for AI Engineering*

#### ⭐ [#6035](https://github.com/spring-projects/spring-ai/pull/6035) — Fix recursive tool input schema by hoisting `$defs` to root ![merged](https://img.shields.io/badge/status-merged-blueviolet) ![milestone](https://img.shields.io/badge/milestone-2.0.0--M7-blue)

- **Problem**: Spring AI inlined parameter schemas under `properties.<param>`, but their generated `$ref`s still pointed to root-level `$defs`. For recursive parameter types, this produced an **unresolvable `$ref`** in the tool input schema.
- **Fix**: Hoist each parameter schema's `$defs` up to the wrapper schema root before inlining. Reuse structurally equal definitions, rename simple-name collisions, and rewrite peer `$ref`s inside the hoisted schema.
- **Scope**: Applied symmetrically to both `JsonSchemaGenerator` (`spring-ai-model`) and `McpJsonSchemaGenerator` (`mcp/mcp-annotations`).
- **Tests**: Added regression coverage for transitive recursive parameter schemas, duplicate equal definitions, and colliding simple-name definitions with ref rewrites.

#### [#6093](https://github.com/spring-projects/spring-ai/pull/6093) — Complete documentation update for OpenAI SDK base URL new behavior ![merged](https://img.shields.io/badge/status-merged-blueviolet) ![milestone](https://img.shields.io/badge/milestone-2.0.0--M7-blue)

Follow-up to [#6036](https://github.com/spring-projects/spring-ai/issues/6036): aligned OpenAI-compatible base URLs across 8 documentation pages (DMR, Ollama, Groq, Mistral AI, Embeddings, Speech, Transcriptions, Moderation) to include `/v1`, and removed properties dropped by the `openai-java` SDK migration. Merged in [`d7f3c6c`](https://github.com/spring-projects/spring-ai/commit/d7f3c6c0d0cbf2ffb2513b1cc79c1caf96de792c).

---

### [JetBrains/koog](https://github.com/JetBrains/koog/pulls?q=is%3Apr+author%3Ajewoodev+is%3Aclosed) — *JVM framework for building enterprise-ready AI agents*

#### ⭐ [#2052](https://github.com/JetBrains/koog/pull/2052) — `fix(prompt)`: Don't start a new tool call on repeated tool call id ![merged](https://img.shields.io/badge/status-merged-blueviolet)

- **Problem**: `StreamFrameFlowBuilder.emitToolCallDelta()` treated every chunk with a non-null tool-call `id` as a new tool call. For OpenAI-compatible providers that repeat the same `id` on every streaming chunk, this fragmented one logical tool call into multiple premature `ToolCallComplete` frames.
- **Fix**: A new tool call now begins only when a present `id` or `index` differs from the pending call (mirroring the existing logic in `emitReasoningDelta`). Absent / blank ids still continue the pending call.
- **Scope**: The fix lives in the shared `StreamFrameFlowBuilder`, so it propagates across **9 streaming clients** — OpenAI, OpenRouter, Mistral, Anthropic, DeepSeek, Ollama, Bedrock, Dashscope, Google.
- **Tests**: Added two regression tests covering "same id continuation" and "distinct id starts a new tool call".

---

### [micrometer-metrics/micrometer](https://github.com/micrometer-metrics/micrometer/pulls?q=is%3Apr+author%3Ajewoodev+is%3Aclosed) — *An application observability facade*

#### [#7546](https://github.com/micrometer-metrics/micrometer/pull/7546) — `docs`: clarify Prometheus LongTaskTimer output ![merged](https://img.shields.io/badge/status-merged-blueviolet) ![branch](https://img.shields.io/badge/branch-1.15.x-blue)

Added a Timer/LongTaskTimer mapping table to the Prometheus implementation page documenting the default `_seconds_count` / `_sum` / `_max` suffixes and the gauge-histogram `_seconds_gcount` / `_seconds_gsum` series exposed when `publishPercentileHistogram()` is enabled, plus an xref from the registry-neutral concepts page. Addresses [#6507](https://github.com/micrometer-metrics/micrometer/issues/6507).

#### [#7549](https://github.com/micrometer-metrics/micrometer/pull/7549) — Document `MeterBinder`s that need to be closed ![merged](https://img.shields.io/badge/status-merged-blueviolet) ![branch](https://img.shields.io/badge/branch-1.15.x-blue)

Added a `close()` lifecycle Javadoc note across 9 `AutoCloseable` binders (`JvmGcMetrics`, `JvmHeapPressureMetrics`, `Log4j2Metrics`, `LogbackMetrics`, `KafkaClientMetrics`, `KafkaStreamsMetrics`, `JettyServerThreadPoolMetrics`, `CommonsObjectPool2Metrics`, …) and a matching reference-doc NOTE on 6 implementation pages. Closes [#4624](https://github.com/micrometer-metrics/micrometer/issues/4624).


---

### [spring-projects/spring-data-jpa](https://github.com/spring-projects/spring-data-jpa/pulls?q=is%3Apr+author%3Ajewoodev+is%3Aclosed) — *JPA-based data access for Spring Data*

#### [#4262](https://github.com/spring-projects/spring-data-jpa/pull/4262) — Document fetch behavior for streaming query results ![merged](https://img.shields.io/badge/status-merged-blueviolet) ![milestone](https://img.shields.io/badge/milestone-3.5.12-blue)

Added a JPA-specific Query Hints note clarifying that a `Stream<T>` return type does not guarantee incremental row fetching — actual behavior depends on the JPA provider and JDBC driver. Closes [#1346 (DATAJPA-1007)](https://github.com/spring-projects/spring-data-jpa/issues/1346), an issue carried over from the Spring Data JIRA era. Merged and backported to maintenance branches.

---

<sub>For the full PR history, see my author search on each repo (linked in the project headers above).</sub>
