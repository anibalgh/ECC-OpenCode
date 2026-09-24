# ECC for OpenCode

> **ECC-OpenCode v1.0.0**: Specialized, production-grade agent operating system for **OpenCode** (v1.18+ and v2.x compatible) powered by **Bun**.
> 68 specialized subagents, 100 slash commands, 292 skills, and 15+ automated plugin hooks.

## Prerequisites

- **Bun**: v1.1+ (recommended v1.4+) — `curl -fsSL https://bun.sh/install | bash`
- **OpenCode CLI**: v1.18+ or v2.x — `bun install -g --trust @opencode/cli` or `curl -fsSL https://opencode.ai/install | bash`

---

## Installation & Setup

### 1. Install Dependencies with Bun

```bash
bun install
```

### 2. Build the OpenCode Plugin

```bash
bun run build:opencode
```

### 3. Launch OpenCode

Run OpenCode inside the repository:

```bash
opencode
```
npm --prefix .opencode run build
```

---

## Agent System (68 Specialized Subagents)

OpenCode automatically discovers all subagents located in `.opencode/agents/`. You can invoke them in chat using `@<agent-name>` or through the `subagent` tool:

### Core Architecture & Planning
- `planner`: Implementation planning, risk assessment, milestone breakdown
- `architect`: System design, high-level scalability, boundary interfaces
- `spec-miner`: Brownfield specification extraction and test boundary mapping
- `code-architect`: Layered architecture, dependency graphs, interface contracts
- `harness-optimizer`: Harness config tuning for token economics and speed
- `loop-operator`: Autonomous loop execution, stall monitoring, and intervention

### Quality & Review
- `code-reviewer`: General code review for quality, maintainability, and clean diffs
- `security-reviewer`: Vulnerability scan (CWE/OWASP, injection, auth leaks)
- `tdd-guide`: Test-driven development enforcement (80%+ coverage)
- `build-error-resolver`: Minimal-diff build and type-checker repair
- `e2e-runner`: Playwright end-to-end automation and regression flows
- `refactor-cleaner`: Dead code cleanup, duplicate consolidation, deprecations
- `doc-updater`: Architecture codemaps and synchronization of docs
- `performance-optimizer`: Latency profiling, memory leak detection, bottleneck relief

### Language & Framework Specialists
- **Python / Django / FastAPI**: `python-reviewer`, `django-reviewer`, `django-build-resolver`, `fastapi-reviewer`
- **TypeScript / React / Vue**: `typescript-reviewer`, `react-reviewer`, `react-build-resolver`, `vue-reviewer`
- **Go**: `go-reviewer`, `go-build-resolver`
- **Rust**: `rust-reviewer`, `rust-build-resolver`
- **Java & Spring Boot**: `java-reviewer`, `java-build-resolver`
- **Kotlin & Android / KMP**: `kotlin-reviewer`, `kotlin-build-resolver`
- **C / C++**: `cpp-reviewer`, `cpp-build-resolver`
- **C# / .NET**: `csharp-reviewer`
- **Flutter / Dart**: `flutter-reviewer`, `dart-build-resolver`
- **PHP & Laravel**: `php-reviewer`
- **Swift & iOS**: `swift-reviewer`, `swift-build-resolver`
- **Database (PostgreSQL / Supabase)**: `database-reviewer`
- **Machine Learning & RAG**: `mle-reviewer`, `rag-pipeline-reviewer`, `pytorch-build-resolver`

---

## Slash Commands (100 Commands)

Commands in `.opencode/commands/*.md` are accessible in the OpenCode TUI via `/`:

| Command | Subagent Triggered | Description |
|---------|--------------------|-------------|
| `/plan` | `planner` | Create phased implementation plan with risk assessment |
| `/tdd` | `tdd-guide` | Enforce test-first RED-GREEN-REFACTOR workflow |
| `/code-review` | `code-reviewer` | Review staged/unstaged changes for quality |
| `/security` | `security-reviewer` | Security audit of auth, input, and API boundaries |
| `/build-fix` | `build-error-resolver` | Fix build and type errors with minimal changes |
| `/e2e` | `e2e-runner` | Generate and run Playwright end-to-end tests |
| `/refactor-clean` | `refactor-cleaner` | Eliminate unused imports, dead functions, and duplicates |
| `/python-review` | `python-reviewer` | Review Python code for PEP 8, types, and security |
| `/react-review` | `react-reviewer` | Review React components, hooks, and render loops |
| `/go-review` | `go-reviewer` | Review Go idiomatic concurrency and error handling |
| `/rust-review` | `rust-reviewer` | Review Rust ownership, lifetimes, and safety |
| `/cpp-review` | `cpp-reviewer` | Review C/C++ memory safety and modern idioms |
| `/checkpoint` | - | Save verification state and progress |
| `/verify` | - | Run comprehensive verification loop before completion |

---

## Plugin Hooks & Custom Tools

The OpenCode ECC plugin (`.opencode/plugins/ecc-hooks.ts`) hooks into OpenCode lifecycle events:

| Hook | Event | Behavior |
|------|-------|----------|
| Prettier / Formatter | `file.edited` | Auto-format modified files |
| TypeScript Check | `tool.execute.after` | Run type checking after edits |
| Secret Guard | `tool.execute.before` | Prevent committing secrets or API keys |
| Auto-approve Read | `permission.ask` | Auto-approve harmless read-only tools |
| Context Preservation | `experimental.session.compacting` | Preserve critical task state across compactions |
| Language Detect | `shell.env` | Detect repository languages and set environment flags |

### Custom Tools Shipped
- `changed-files`: Visual tree of modified files during the session
- `dependency-analyzer`: Vulnerability and outdated package detector
- `run-tests`: Test runner with framework auto-detection
- `check-coverage`: Coverage analyzer against 80% threshold
- `git-summary`: Git status, current branch, and staged diff summary
- `format-code` & `lint-check`: Fast linter/formatter command resolvers

---

## Skills Integration

All 292 domain skills in `skills/` conform to the Agent Skills standard (`SKILL.md`). In OpenCode, the model loads them dynamically using the `skill` tool:

```json
{
  "id": "backend-patterns"
}
```

Skills are advertised lightly in context and loaded only when relevant to preserve the model's context window.

---

## License

MIT
