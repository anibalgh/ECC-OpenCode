# OpenCode ECC — Agent Instructions & Workflow Operating System

This is a **production-ready AI coding system** specialized for the **OpenCode** harness and powered by **Bun**, providing 68 specialized subagents, 292 skills, 100 commands, custom plugin hooks, and automated workflows for software engineering.

**Harness:** OpenCode (v1.18+ and v2.x compatible)  
**Runtime & Package Manager:** Bun (v1.4+)  
**Version:** 1.0.0

---

## Core Principles

1. **Agent-First** — Delegate to specialized subagents for domain tasks via the `subagent` tool or `@<agent>` mention.
2. **Test-Driven (TDD)** — Write failing tests before implementation; 80%+ coverage required.
3. **Security-First** — Never compromise on security; validate all inputs, parameters, and boundary conditions.
4. **Immutability** — Always create new objects, never mutate existing state.
5. **Plan Before Execute** — Plan complex features, assess risks, and confirm steps before touching code.

---

## OpenCode Agent Architecture

OpenCode operates with a primary agent and on-demand subagents:

- **Primary Agent (`build`)**: The active development agent handling conversations, file editing, terminal commands, and subagent delegation.
- **Planning Agent (`plan`)**: Explores and drafts plans without directly modifying production code.
- **Specialized Subagents (`subagent` mode)**: Independent child sessions executed via OpenCode's `subagent` tool with dedicated prompts and constrained permissions.

### Available Agents

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| `planner` | Implementation planning | Complex features, refactoring, risk analysis |
| `architect` | System design and scalability | Architectural decisions, API contracts |
| `tdd-guide` | Test-driven development | New features, bug fixes, test coverage |
| `code-reviewer` | Code quality and maintainability | After writing or modifying code |
| `security-reviewer` | Vulnerability detection | Before commits, auth, sensitive data |
| `build-error-resolver` | Fix build and type errors | When build fails or types break |
| `e2e-runner` | End-to-end testing with Playwright | Critical user flows, browser regression |
| `refactor-cleaner` | Dead code cleanup & consolidation | Code maintenance, deprecation removal |
| `doc-updater` | Documentation & codemaps | Syncing docs, architecture codemaps |
| `database-reviewer` | PostgreSQL & schema optimization | Schema changes, indexes, query plans |
| `docs-lookup` | Up-to-date documentation lookup | Framework/library API questions |
| `harness-optimizer` | Harness config tuning | Reliability, cost, and token optimization |
| `loop-operator` | Autonomous loop execution | Running multi-step loops safely |
| `spec-miner` | Brownfield spec extraction | Reverse-engineering specs from existing code |
| `python-reviewer` | Python code review | Pythonic patterns, PEP 8, typing |
| `django-reviewer` | Django & DRF code review | Django ORM, migrations, views, signals |
| `django-build-resolver` | Django setup & migration errors | Migration conflicts, manage.py errors |
| `go-reviewer` | Go code review | Concurrency, interfaces, idioms |
| `go-build-resolver` | Go build & compilation errors | Go compiler, vet, module fixes |
| `rust-reviewer` | Rust code review | Ownership, lifetimes, concurrency |
| `rust-build-resolver` | Rust build errors | Cargo, borrow checker, compilation fixes |
| `java-reviewer` | Java & Spring Boot review | Spring patterns, JPA, concurrency |
| `java-build-resolver` | Java/Maven/Gradle build errors | Compilation and dependency issues |
| `kotlin-reviewer` | Kotlin/Android/KMP review | Coroutines, Compose, idiomatic Kotlin |
| `kotlin-build-resolver` | Kotlin build errors | Gradle and Kotlin compiler errors |
| `cpp-reviewer` | C/C++ code review | Memory safety, modern idioms, templates |
| `cpp-build-resolver` | C/C++ build errors | CMake, compiler, and linker errors |
| `mle-reviewer` | Production ML pipeline review | ML pipelines, evals, monitoring, rollback |
| `rag-pipeline-reviewer` | RAG pipeline review | Chunking, embeddings, reranking, evals |
| `typescript-reviewer` | TypeScript/JavaScript review | Strict typing, async patterns, module design |

---

## Agent Orchestration in OpenCode

Use subagents proactively when encountering specific tasks:

- Complex feature requests → `@planner` (or `/plan`)
- Code just written or modified → `@code-reviewer` (or `/code-review`)
- Bug fix or new feature → `@tdd-guide` (or `/tdd`)
- Architectural decision → `@architect`
- Security-sensitive code → `@security-reviewer` (or `/security`)
- Build failure or type error → `@build-error-resolver` (or `/build-fix`)
- E2E testing flows → `@e2e-runner` (or `/e2e`)
- Dead code / messy diffs → `@refactor-cleaner` (or `/refactor-clean`)
- ML / RAG pipeline changes → `@mle-reviewer` / `@rag-pipeline-reviewer`

In OpenCode, delegate using the `subagent` tool:
```json
{
  "agent": "code-reviewer",
  "description": "Review staged changes for security and quality",
  "prompt": "Review all staged changes in git diff --staged"
}
```

---

## Skills Integration

OpenCode natively loads skills dynamically via the `skill` tool. There are **292 skills** in `skills/`.
When a task involves a specific domain, load the corresponding skill:

- REST API design → `skill: { id: "api-design" }`
- Backend development → `skill: { id: "backend-patterns" }`
- Frontend development → `skill: { id: "frontend-patterns" }`
- Security audit → `skill: { id: "security-review" }`
- Verification loop → `skill: { id: "verification-loop" }`
- Playwright E2E → `skill: { id: "e2e-testing" }`

Do not load full skill files manually into context when not needed; rely on OpenCode's on-demand skill advertising.

---

## Security Guidelines

**Before ANY commit:**
1. **Zero Hardcoded Secrets**: No API keys, passwords, bearer tokens, or sensitive credentials.
2. **Input Validation**: All external inputs must be validated with schemas (Zod, Pydantic, etc.).
3. **Injection Prevention**: Parameterized database queries, sanitized shell executions, safe HTML handling.
4. **Least Privilege**: Only request required permissions and files.
5. **No Credential Leaks**: Never log secrets, authorization headers, or private user data.

---

## Coding Style & Standards

- **Immutability**: Always return new copies of objects/arrays with changes applied (`{ ...obj, prop: value }`), never mutate.
- **Focused Files**: 200–400 lines typical, 800 maximum. High cohesion, low coupling.
- **Small Functions**: Keep functions under 50 lines with single responsibility.
- **Error Handling**: Handle errors explicitly at every level; never swallow exceptions silently.
- **Types**: Strict type checking; avoid `any` in TypeScript or unannotated signatures in Python.

---

## Testing Requirements (Mandatory)

**Minimum coverage: 80%**

- **Unit tests**: Individual functions, pure utilities, domain entities.
- **Integration tests**: API endpoints, database queries, inter-service calls.
- **E2E tests**: Critical user journeys using Playwright.

**TDD Workflow:**
1. **RED**: Write the test first and verify it fails.
2. **GREEN**: Write the minimal code to make the test pass.
3. **IMPROVE**: Refactor for clarity and performance while maintaining passing tests.

---

## Git Workflow

- **Commit Format**: `<type>: <description>` (e.g. `feat: add user auth`, `fix: handle null pointer in parser`, `test: add coverage for billing`).
- **Review before Push**: Run `@code-reviewer` and verify that `git diff` contains no debug logs or formatting inconsistencies.
