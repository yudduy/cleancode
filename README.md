# CleanCode - Autonomous Code Quality Commands for Claude Code

**Production-ready autonomous code review, security auditing, testing, and performance optimization.**

CleanCode provides four powerful slash commands that bring autonomous quality assurance to any project. Each command uses multi-agent orchestration to work continuously until your code meets production standards.

## Commands

### `/quality-loop` - Full Autonomous Quality Cycle

Runs a comprehensive quality improvement cycle until all gates pass:
- Multi-agent code review (3 agents in parallel)
- Security audit (OWASP Top 10)
- Performance analysis
- Test execution and fixing
- Autonomous fixing loop (max 10 iterations)

**Termination:** Only stops when all tests pass, no critical issues, and quality gates cleared.

```bash
/quality-loop
```

### `/review-pr` - Multi-Agent PR Review

Comprehensive pull request review using 3 parallel agents:
- Code quality analysis
- Security vulnerability scanning
- Performance impact assessment

Outputs categorized findings: 🔥 Critical, ⚠️ Warnings, 💡 Suggestions

```bash
/review-pr 123
# or
/review-pr feature-branch
```

### `/fix-tests` - Autonomous Test Fixing

Fixes all failing tests autonomously:
- Auto-detects test framework (Vitest, Jest, Pytest)
- Analyzes root causes
- Applies fixes and re-runs
- Max 20 iterations, 3 attempts per test

```bash
/fix-tests
```

### `/security-scan` - OWASP Security Audit

Comprehensive security audit covering:
- SQL injection detection
- XSS and CSRF vulnerabilities
- Authentication/authorization issues
- Cryptographic failures
- Security misconfigurations
- Web3-specific security (if applicable)

```bash
/security-scan
```

## Installation

### Quick Setup (Copy Commands)

```bash
# Clone the repository
git clone https://github.com/yudduy/cleancode.git

# Copy commands to your project
cp cleancode/.claude/commands/*.md your-project/.claude/commands/

# Or use this one-liner from within your project:
curl -sL https://github.com/yudduy/cleancode/archive/main.tar.gz | tar xz --strip=2 cleancode-main/.claude/commands -C .claude/

# Commands available immediately (no restart needed)
```

### Manual Installation

1. Create `.claude/commands/` directory in your project:
   ```bash
   mkdir -p .claude/commands
   ```

2. Download commands:
   ```bash
   cd .claude/commands
   wget https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/quality-loop.md
   wget https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/review-pr.md
   wget https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/fix-tests.md
   wget https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/security-scan.md
   ```

3. Commands will be available immediately

### Global Installation (All Projects)

To use these commands across all projects, copy them to your Claude Code user commands directory:

```bash
# macOS/Linux
mkdir -p ~/.claude/commands
cp cleancode/.claude/commands/*.md ~/.claude/commands/

# Windows
mkdir %USERPROFILE%\.claude\commands
copy cleancode\.claude\commands\*.md %USERPROFILE%\.claude\commands\
```

## How It Works

### Autonomous Loop Pattern

Commands use natural language instructions with state-driven termination:

```
WHILE (quality_gates_failing OR tests_failing):
    PHASE 1: Multi-Agent Analysis (parallel execution)
    PHASE 2: Issue Detection
    PHASE 3: Autonomous Fixing
    PHASE 4: Validation

IF all_gates_passed:
    OUTPUT completion message (terminates)
ELSE:
    CONTINUE (no user intervention)
```

**Key Directive**: "DO NOT ask user to continue. KEEP WORKING until complete."

### Multi-Agent Orchestration

Commands launch multiple subagents in parallel using Claude Code's Task tool:

```typescript
// Example: /quality-loop launches 3 agents simultaneously
Task(general-purpose, "Code quality review")
Task(general-purpose, "Security audit")
Task(general-purpose, "Performance analysis")

// Each agent has isolated 200K token context
// Results synthesized after all complete
```

**Benefits**:
- Parallel execution (3-10x faster than sequential)
- Isolated contexts (no cross-contamination)
- Specialized focus areas (quality, security, performance)
- Comprehensive coverage

### Framework-Agnostic Design

All commands auto-detect your tech stack:
- **Next.js**: Server Components, Server Actions security, RSC patterns
- **React**: Hooks optimization, re-render prevention
- **TypeScript**: Type safety, strict mode compliance
- **Python**: ORM security, async patterns
- **Rust**: Memory safety, error handling
- **Any framework**: Core quality principles apply universally

## Usage Examples

### Next.js + TypeScript Project

```bash
cd my-nextjs-app
/quality-loop
```

**What happens**:
1. Detects Next.js, React, TypeScript, Vitest (if present)
2. Reviews Server Components and Server Actions
3. Checks type safety and React optimization
4. Scans for Web3 security issues (if wagmi/viem detected)
5. Runs tests and fixes failures
6. Loops until production-ready

**Typical runtime**: 5-15 minutes (fully autonomous)

### Python + Django Project

```bash
cd my-django-app
/security-scan
```

**What happens**:
1. Scans ORM queries for SQL injection
2. Checks template rendering for XSS
3. Validates middleware and auth patterns
4. Reviews CSRF protection
5. Checks for exposed secrets
6. Outputs comprehensive security report

**Typical runtime**: 3-8 minutes

### Pull Request Review

```bash
git checkout feature-branch
/review-pr main
```

**What happens**:
1. Fetches diff between feature-branch and main
2. Launches 3 agents in parallel
3. Each agent reviews changed lines only
4. Synthesizes findings into categorized report
5. Provides approve/reject recommendation

**Typical runtime**: 2-5 minutes

## Real-World Results

**Tree-Connect Project** (Next.js + TypeScript + Drizzle):
- `/review-all`: Reviewed 92 packages, found 12 unused, 3 critical security issues (~35 min)
- `/quality-loop`: Fixed 8 failing tests in 2 iterations (~8 min)
- `/security-scan`: Identified 2 SQL injection vectors, 1 auth bypass (~5 min)

**Estimated time savings**: ~3-5 hours of manual code review per run

## Command Details

### `/quality-loop`

**Phases**:
1. **Multi-Agent Review** - 3 agents analyze entire codebase in parallel
2. **Test Execution** - Runs test suite, captures failures
3. **Fix Loop** - Prioritizes and fixes issues (security → tests → performance → style)
4. **Validation** - Re-runs all quality gates

**Termination conditions**:
- ✅ All tests passing
- ✅ No critical/warning code issues
- ✅ No security vulnerabilities
- ✅ Performance checks passed

**Max iterations**: 10 (prevents infinite loops)

**Output**: Comprehensive summary with statistics and remaining minor items

### `/review-pr`

**Inputs**: PR number or branch name
**Process**:
1. Fetches PR diff (via `gh pr diff` or `git diff`)
2. Launches 3 parallel agent reviews
3. Synthesizes findings

**Output format**:
- 🔥 **Critical Issues** (must fix before merge)
- ⚠️ **Warnings** (should fix)
- 💡 **Suggestions** (nice to have)
- Approve/Reject recommendation

### `/fix-tests`

**Process**:
1. Run test suite (auto-detects Vitest/Jest/Pytest)
2. Identify failing tests
3. For each failure:
   - Analyze error and stack trace
   - Determine root cause (implementation bug vs outdated test)
   - Apply fix
   - Re-run test
4. Loop until all pass or max iterations (20)

**Safety limits**:
- Max 3 attempts per test (prevents thrashing)
- Max 20 total iterations

### `/security-scan`

**Scan coverage**:
1. **OWASP Top 10 manual audit** (via subagent)
2. **Automated scans**:
   - `npm audit` (dependency vulnerabilities)
   - Pattern matching for SQL injection
   - Hardcoded secret detection
   - XSS risk identification

**Output**:
- Vulnerabilities categorized by severity
- OWASP compliance checklist (A01-A10)
- Code examples and exploit scenarios
- Fix recommendations
- Overall security score

## Architecture

### Why Slash Commands?

CleanCode uses slash commands (not a plugin) because:
1. **Simplicity** - Just copy `.md` files to `.claude/commands/`
2. **Portability** - Works across all projects immediately
3. **Customization** - Edit command logic directly in markdown
4. **No dependencies** - Uses built-in Claude Code features only

### Token Efficiency

Commands use aggressive context management:
- **Subagents** have isolated 200K token contexts (main thread stays clean)
- **Progressive disclosure** - Only load relevant details when needed
- **Batched operations** - Process multiple items per iteration
- **Result summarization** - Subagents return summaries, not full context

**Typical context usage per command**:
- `/quality-loop`: ~50K tokens (main) + 3x200K (subagents)
- `/review-pr`: ~20K tokens (main) + 3x200K (subagents)
- `/fix-tests`: ~30K tokens (main, iterative)
- `/security-scan`: ~25K tokens (main) + 200K (subagent)

### Extensibility

Add your own commands by creating `.claude/commands/your-command.md`:

```markdown
**YOUR COMMAND NAME**

Brief description.

---

## Process

### Step 1
Instructions for Claude to follow...

### Step 2
Use Task tool to launch subagents if needed...

---

**Context:** $ARGUMENTS
```

## Contributing

Contributions welcome! To add a command:

1. Fork the repository
2. Create new command file in `.claude/commands/`
3. Test across 3+ different tech stacks
4. Submit pull request

**Guidelines**:
- Commands must be framework-agnostic
- Use general-purpose subagents (not custom agent types)
- Include clear termination conditions
- Document expected runtime
- Follow defensive security practices only

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Troubleshooting

### Commands not showing up

1. Ensure files are in `.claude/commands/` (not `.claude-plugin/` or other location)
2. Check file names end with `.md`
3. Restart Claude Code if needed
4. Verify files are readable: `ls -la .claude/commands/`

### Command fails with "agent not found"

This means an old version of the command is referencing custom agents. Update to latest:

```bash
cd .claude/commands
wget -N https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/quality-loop.md
wget -N https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/review-pr.md
wget -N https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/fix-tests.md
wget -N https://raw.githubusercontent.com/yudduy/cleancode/main/.claude/commands/security-scan.md
```

### Subagent takes too long

This is normal for large codebases. Subagents may take 3-10 minutes for comprehensive analysis.

If timeout occurs:
- Reduce scope (e.g., use `/review-pr` instead of `/quality-loop`)
- Break into smaller chunks manually
- Use `/fix-tests` for just test failures

## License

MIT License - see [LICENSE](LICENSE) for details.

## Credits

Built with [Claude Code](https://claude.com/claude-code) by Anthropic.

Inspired by:
- [Anthropic Developer Documentation](https://docs.anthropic.com) - Multi-agent patterns
- [Claude Code Autonomous Patterns](https://docs.claude.com/claude-code) - Loop design
- Real-world usage in Tree-Connect project

## Support

- **Issues**: [GitHub Issues](https://github.com/yudduy/cleancode/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yudduy/cleancode/discussions)
- **Documentation**: This README + inline command documentation

---

**Install now**:
```bash
cp cleancode/.claude/commands/*.md your-project/.claude/commands/
```

**Use immediately**:
```bash
/quality-loop
```
