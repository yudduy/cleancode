# Contributing to CleanCode

Thank you for your interest in contributing to CleanCode! This document provides guidelines and best practices for contributing.

## Code of Conduct

- **Defensive security only** - No malicious code, credential harvesting, or offensive security tools
- **Framework-agnostic** - Skills and agents must work across different tech stacks
- **Production-ready** - All contributions should follow enterprise-grade quality standards
- **Token-efficient** - Use progressive disclosure and adaptive loading patterns

## Getting Started

1. **Fork the repository**
   ```bash
   gh repo fork yudduy/cleancode
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/cleancode.git
   cd cleancode
   ```

3. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

## Contribution Types

### 1. Adding New Skills

**When to add a skill**:
- Popular framework (React, Vue, Angular, Django, Rails, etc.)
- Programming language (Python, Rust, Go, Java, etc.)
- Database ORM (Prisma, TypeORM, SQLAlchemy, etc.)
- Testing framework (Jest, Pytest, RSpec, etc.)
- Domain expertise (GraphQL, gRPC, message queues, etc.)

**Skill structure**:

```markdown
---
skill: framework-name-adaptive
description: Brief description. Activates when [detection criteria].
category: framework|language|database|testing|domain
activation: auto
priority: 1-10 (10 = highest)
---

# Skill Name

## Auto-Detection
List files, imports, or patterns that trigger this skill.

## Best Practices
### Pattern Name
**Good example**:
```code
// Good code here
```

**Bad example**:
```code
// Anti-pattern here
```

## Common Mistakes
### Mistake Name
**Issue**: Description
**Fix**: Solution
```

**Placement**:
- Frameworks → `skills/frameworks/`
- Languages → `skills/languages/`
- Databases → `skills/databases/`
- Testing → `skills/testing/`
- Domains → `skills/domains/`

**Update manifest**:
Add to `.claude-plugin/marketplace.json`:

```json
{
  "skills": [
    "./skills/frameworks/your-skill.md"
  ]
}
```

### 2. Adding New Agents

**When to add an agent**:
- Specialized task requiring focused analysis (accessibility, i18n, SEO, etc.)
- Complex multi-step workflow (deployment validation, migration safety, etc.)
- Domain-specific expertise (machine learning, data science, DevOps, etc.)

**Agent structure**:

```markdown
---
name: agent-name
description: What this agent does (1-2 sentences)
tools: Read, Grep, Glob, Bash (specify which tools needed)
model: sonnet|haiku (sonnet for complex reasoning, haiku for speed)
---

# Agent Name

## Responsibilities
- What this agent does
- Key focus areas

## Auto-Detection (if applicable)
How agent determines if it's relevant to the project.

## Analysis Process
1. Step 1
2. Step 2
3. Step 3

## Output Format
What the agent returns to the user or orchestrator.
```

**Update manifest**:

```json
{
  "agents": [
    "./agents/your-agent.md"
  ]
}
```

### 3. Adding New Commands

**When to add a command**:
- Common workflow that benefits from automation
- Multi-agent orchestration for specific use case
- Autonomous loop for quality gate enforcement

**Command structure**:

```markdown
**COMMAND NAME**

Brief description of what this command does.

---

## TERMINATION CONDITION (if autonomous loop)

ONLY stop when:
- Condition 1
- Condition 2

**DO NOT** ask user to continue. **KEEP WORKING** until done.

---

## Process

### 1. Step Name

```bash
# Example commands
```

### 2. Next Step

```
Use specific-agent subagent for task.

Return: What the agent should return.
```

---

## Output Format

```markdown
# Output Title

**Metric 1:** Value
**Metric 2:** Value

## Section
[Content]
```

---

**Context:** $ARGUMENTS
```

**Update manifest**:

```json
{
  "commands": [
    "./commands/your-command.md"
  ]
}
```

## Testing Your Contribution

### Test Across Multiple Tech Stacks

**Minimum test coverage**:
1. Next.js/React project (TypeScript)
2. Python project (Django or FastAPI)
3. Rust or Go project

**Test checklist**:
- [ ] Skill auto-activates correctly
- [ ] Agent produces expected output
- [ ] Command terminates appropriately
- [ ] No errors in execution
- [ ] Token usage is reasonable
- [ ] Works in parallel with other agents/skills

### Test in CleanCode Itself

```bash
cd cleancode
/quality-loop
```

If your contribution is high-quality, the quality loop should pass!

## Pull Request Process

1. **Test thoroughly** across different projects
2. **Update documentation** if adding features
3. **Follow existing patterns** for consistency
4. **Write clear commit messages**:
   ```
   feat(skills): add Django ORM security patterns

   - Auto-detects django imports and manage.py
   - Covers SQL injection prevention
   - Includes queryset optimization patterns

   Tested on 3 Django projects.
   ```

5. **Submit PR** with description:
   - What does this add/fix?
   - How was it tested?
   - Any breaking changes?

6. **Review process**:
   - Automated quality loop runs on PR
   - Manual review by maintainers
   - Address feedback
   - Merge when approved

## Style Guidelines

### Markdown Formatting

- **Headings**: Use `##` for main sections, `###` for subsections
- **Code blocks**: Always specify language (```typescript, ```python, etc.)
- **Examples**: Show both good (✅) and bad (❌) patterns
- **Emojis**: Use sparingly, only for categorization (🔥 Critical, ⚠️ Warning, 💡 Suggestion)

### Content Guidelines

- **Be concise** - Developers scan documentation quickly
- **Show examples** - Code speaks louder than words
- **Link to official docs** - Reference authoritative sources
- **Version-specific** - Mention version if patterns changed (e.g., "Next.js 15+")
- **Security-first** - Always highlight security implications

### Agent/Skill Voice

- **Directive tone** - "Check for X", "Validate Y", "Ensure Z"
- **No uncertainty** - Avoid "might", "could", "maybe"
- **Action-oriented** - Focus on what to do, not theory
- **Tool-aware** - Specify which tools to use (Read, Grep, Bash)

## Defensive Security Requirements

**MUST**:
- Only defensive security tools and analysis
- Document security vulnerabilities to fix them
- Provide remediation steps for issues found

**MUST NOT**:
- Create or improve malicious code
- Assist with credential harvesting (bulk SSH key scanning, cookie theft, etc.)
- Build offensive security tools (exploit frameworks, etc.)

**Gray areas** (allowed with context):
- Penetration testing your own infrastructure
- Security research with proper disclosure
- Vulnerability scanners for defensive purposes

When in doubt, ask: "Does this help defenders protect systems?"

## Recognition

Contributors will be:
- Listed in README.md contributors section
- Credited in release notes
- Mentioned in plugin marketplace listing (if significant contribution)

## Questions?

- **General questions**: [GitHub Discussions](https://github.com/yudduy/cleancode/discussions)
- **Bug reports**: [GitHub Issues](https://github.com/yudduy/cleancode/issues)
- **Security vulnerabilities**: Email security@cleancode.dev (create this if needed)

Thank you for contributing to CleanCode!
