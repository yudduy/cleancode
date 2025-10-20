# CleanCode - Autonomous Code Quality System

**Production-ready autonomous code review, security auditing, testing, and performance optimization for Claude Code.**

CleanCode is a portable, framework-agnostic plugin that brings enterprise-grade code quality automation to any project. Install once, use everywhere.

## Features

- **Autonomous Quality Loops** - Continuous code review, security scanning, test fixing until production-ready
- **Multi-Agent Orchestration** - 5 specialized AI agents run in parallel for comprehensive analysis
- **Adaptive Skills System** - Auto-detects your tech stack and loads only relevant expertise (Next.js, React, TypeScript, Drizzle, Vitest, Web3, etc.)
- **Framework-Agnostic** - Works with any programming language and framework
- **Security-First** - OWASP Top 10 compliance, SQL injection detection, authentication audits
- **Zero Configuration** - Inspects your codebase and adapts automatically

## Installation

```bash
# Install from Claude Code marketplace
/plugin marketplace add yudduy/cleancode
```

That's it. CleanCode will auto-detect your tech stack and activate relevant skills.

## Quick Start

### Run Full Autonomous Quality Loop

```bash
/quality-loop
```

This will autonomously:
1. Launch 3 parallel agents (code reviewer, security auditor, performance analyst)
2. Run your test suite
3. Fix all issues prioritized by severity (security > tests > performance)
4. Validate with quality gates
5. Loop until all tests pass and no critical issues remain

**No user intervention required.** Claude will work autonomously until production-ready.

### Review a Pull Request

```bash
/review-pr 123
# or
/review-pr feature-branch
```

Multi-agent PR review with findings categorized as:
- 🔥 **Critical** (must fix before merge)
- ⚠️ **Warnings** (should fix)
- 💡 **Suggestions** (nice to have)

### Fix All Failing Tests

```bash
/fix-tests
```

Autonomous test fixing loop (max 20 iterations):
- Auto-detects test framework (Vitest, Jest, Pytest)
- Analyzes root causes (test outdated vs implementation wrong)
- Applies fixes and re-runs until passing
- Max 3 attempts per test

### Security Audit

```bash
/security-scan
```

Comprehensive OWASP Top 10 security audit:
- SQL injection detection
- Authentication/authorization issues
- XSS and CSRF vulnerabilities
- Cryptographic failures
- Security misconfigurations
- Framework-specific security (Next.js, Web3)

## Architecture

### 5 Specialized Agents

**code-reviewer** (Sonnet)
- Framework-agnostic code review
- Auto-detects tech stack via file inspection
- Universal quality checklist (type safety, error handling, security, performance)

**security-auditor** (Sonnet)
- OWASP Top 10 compliance checking
- SQL injection, XSS, CSRF detection
- Authentication pattern validation
- Web3 security (signature verification, replay protection)

**test-runner** (Haiku)
- Autonomous test execution and fixing
- Root cause analysis
- Auto-detects testing framework

**performance-analyst** (Haiku)
- N+1 query detection
- React re-render analysis
- Bundle size optimization
- Database query optimization

**bug-hunter** (Sonnet)
- Deep debugging for complex issues
- 7-phase methodology (reproduce, gather evidence, hypothesis, test, identify, fix, document)

### Adaptive Skills System

**Core Skills** (always active):
- `code-quality-fundamentals` - Clean Code, SOLID, DRY principles
- `security-owasp-top10` - Security best practices
- `performance-principles` - Database and API optimization

**Adaptive Skills** (auto-activate based on file detection):

| Skill | Activates When | Provides |
|-------|----------------|----------|
| `nextjs-adaptive` | `app/`, `page.tsx`, `layout.tsx` detected | Next.js 15 patterns, Server Actions, RSC |
| `react-adaptive` | `.tsx` files with React imports | Hooks best practices, performance optimization |
| `typescript-adaptive` | `.ts`, `.tsx` files | Type safety patterns, strict mode |
| `drizzle-adaptive` | `drizzle.config.ts` or drizzle-orm imports | N+1 prevention, prepared statements |
| `vitest-adaptive` | `vitest.config.ts` detected | Async tests, mocking, cleanup |
| `web3-security` | `wagmi`, `viem`, `ethers` detected | Signature verification, anti-replay |
| `api-security` | `app/api/`, `route.ts` detected | Auth, input validation, rate limiting |

**Token Efficiency**: Only loads relevant skills (~13K tokens vs ~50K for all skills, 74% savings)

### Universal Commands

All commands work autonomously with minimal user intervention:

| Command | Purpose | Termination Condition |
|---------|---------|----------------------|
| `/quality-loop` | Full quality cycle | All tests pass + no critical issues |
| `/review-pr` | Multi-agent PR review | Review complete |
| `/fix-tests` | Fix failing tests | All tests pass or max iterations (20) |
| `/security-scan` | OWASP security audit | Scan complete |

## Usage Examples

### Next.js + TypeScript Project

```bash
cd my-nextjs-app
/quality-loop
```

**Auto-detected skills**: Next.js, React, TypeScript, API Security
**Output**: Reviews Server Components, validates Server Actions security, checks type safety

### Python + Django Project

```bash
cd my-django-app
/quality-loop
```

**Auto-detected skills**: Python fundamentals, API security patterns
**Output**: Reviews ORM queries, validates middleware security, checks template injection

### Rust Project

```bash
cd my-rust-app
/quality-loop
```

**Auto-detected skills**: Core code quality fundamentals
**Output**: Reviews memory safety patterns, error handling, performance

## How It Works

### Autonomous Loop Pattern

Commands use natural language instructions with state-driven termination:

```markdown
WHILE (quality_gates_failing OR tests_failing):
    PHASE 1: Multi-Agent Review (3 parallel subagents)
    PHASE 2: Test Execution
    PHASE 3: Fix Loop (prioritized: security > tests > performance)
    PHASE 4: Validation

IF all_gates_passed:
    OUTPUT completion message (terminates loop)
ELSE:
    CONTINUE (no user intervention needed)
```

**Key Directive**: "DO NOT ask user to continue. KEEP WORKING until complete."

### Multi-Agent Orchestration

Agents run in parallel using Claude Code's Task tool (max 10 simultaneous):

```typescript
// Launch 3 agents in parallel for PR review
Task(code-reviewer, "Review changed files for code quality")
Task(security-auditor, "Audit for OWASP Top 10 vulnerabilities")
Task(performance-analyst, "Check for performance issues")

// Wait for all to complete, then synthesize findings
```

Each agent has:
- Isolated 200K token context window
- Specialized tools (Read, Grep, Glob, Bash)
- Custom model (Sonnet for complex reasoning, Haiku for speed)

## Real-World Results

**Tree-Connect Project** (Next.js + TypeScript + Drizzle + Vitest):
- Reviewed 92 package dependencies autonomously
- Identified 12 unused packages, 3 critical security issues
- Fixed 8 failing tests in 2 iterations
- Total runtime: ~35 minutes (fully autonomous)

## Configuration

### Zero Configuration Default

CleanCode works out-of-the-box by inspecting your codebase:

1. Scans file extensions (`.ts`, `.tsx`, `.py`, `.rs`, etc.)
2. Reads config files (`package.json`, `drizzle.config.ts`, `vitest.config.ts`)
3. Loads relevant adaptive skills automatically
4. Runs appropriate quality checks

### Custom Hook Configuration (Optional)

Add quality gates via `.claude-hooks/pre-tool-use.sh`:

```bash
#!/bin/bash
# Block commits if tests are failing
if [[ "$TOOL_NAME" == "Bash" && "$COMMAND" =~ "git commit" ]]; then
    npm run test
    if [ $? -ne 0 ]; then
        echo "BLOCK: Tests must pass before committing"
        exit 1
    fi
fi
```

## Architecture Benefits

### Portability
- **Single installation** works across all projects
- **No per-project configuration** required
- **Persistent across sessions** (survives restarts)

### Token Efficiency
- **Adaptive loading** reduces context by 74%
- **Isolated subagent contexts** prevent main thread pollution
- **Progressive disclosure** (3-tier: metadata → instructions → resources)

### Scalability
- **Parallel execution** (10 agents simultaneously)
- **Batched processing** for large codebases
- **Stateless agents** (no cross-contamination)

## Development

### Project Structure

```
cleancode/
├── .claude-plugin/
│   └── marketplace.json          # Plugin manifest
├── plugins/autonomous-quality/
│   ├── agents/                   # 5 specialized agents
│   │   ├── code-reviewer.md
│   │   ├── security-auditor.md
│   │   ├── test-runner.md
│   │   ├── performance-analyst.md
│   │   └── bug-hunter.md
│   ├── skills/
│   │   ├── core/                 # Always-active skills
│   │   │   ├── code-quality-fundamentals.md
│   │   │   ├── security-owasp-top10.md
│   │   │   └── performance-principles.md
│   │   ├── frameworks/           # Auto-activate skills
│   │   │   ├── nextjs-adaptive.md
│   │   │   └── react-adaptive.md
│   │   ├── languages/
│   │   │   └── typescript-adaptive.md
│   │   ├── databases/
│   │   │   └── drizzle-adaptive.md
│   │   ├── testing/
│   │   │   └── vitest-adaptive.md
│   │   └── domains/
│   │       ├── web3-security.md
│   │       └── api-security.md
│   └── commands/                 # Universal commands
│       ├── quality-loop.md
│       ├── review-pr.md
│       ├── fix-tests.md
│       └── security-scan.md
└── README.md
```

### Adding New Skills

Create a new skill file with activation description:

```markdown
---
skill: django-patterns
description: Django ORM patterns, middleware, security. Activates when manage.py or django imports detected.
category: framework
activation: auto
priority: 8
---

# Django Best Practices

## ORM Security
[Your content here]
```

Place in appropriate category:
- `skills/frameworks/` - Web frameworks
- `skills/languages/` - Programming languages
- `skills/databases/` - Database ORMs
- `skills/testing/` - Testing frameworks
- `skills/domains/` - Domain-specific (security, performance)

### Adding New Agents

Create a new agent file:

```markdown
---
name: accessibility-auditor
description: WCAG compliance and accessibility testing
tools: Read, Grep, Glob, Bash
model: sonnet
---

# Accessibility Auditor

[Your agent instructions here]
```

Add to `.claude-plugin/marketplace.json`:

```json
{
  "agents": [
    "./agents/code-reviewer.md",
    "./agents/accessibility-auditor.md"
  ]
}
```

## Contributing

Contributions welcome! Please:

1. **Fork the repository**
2. **Create feature branch** (`git checkout -b feature/new-skill`)
3. **Add your skill/agent** following existing patterns
4. **Test in multiple projects** (different tech stacks)
5. **Submit pull request**

**Guidelines**:
- Skills must be framework-agnostic with clear activation descriptions
- Agents must specify required tools and model
- Commands must have clear termination conditions
- All content must use defensive security practices only

## License

MIT License - see LICENSE file for details

## Credits

Built with [Claude Code](https://claude.com/claude-code) by Anthropic.

Inspired by:
- [wshobson/agents](https://github.com/wshobson/agents) - Multi-agent patterns
- [ralph-dev-kit](https://github.com/anthropics/ralph-dev-kit) - Autonomous loop patterns
- [claude-code-test-runner](https://github.com/anthropics/claude-code-test-runner) - Test execution patterns

## Support

- **Issues**: [GitHub Issues](https://github.com/yudduy/cleancode/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yudduy/cleancode/discussions)
- **Documentation**: [Claude Code Docs](https://docs.claude.com/claude-code)

---

**Install now**: `/plugin marketplace add yudduy/cleancode`
