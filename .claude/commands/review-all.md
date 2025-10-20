**AUTONOMOUS EXHAUSTIVE PACKAGE REVIEW**

You are performing a COMPLETE, AUTONOMOUS review of ALL packages in this repository.

**CRITICAL**: This is a continuous autonomous loop. You will work through ALL packages without stopping until EVERY package in REVIEW_STATE.md is marked [x].

---

## TERMINATION CONDITION

**ONLY** stop making tool calls when:
- REVIEW_STATE.md shows ALL packages marked [x]
- Then output: "🎉 COMPLETE: All [N] packages reviewed. See REVIEW_FINDINGS.md"

**DO NOT** ask user to run next command. **DO NOT** wait for user input. **KEEP WORKING** until complete.

---

## AUTONOMOUS EXECUTION LOOP

```pseudocode
WHILE any package in REVIEW_STATE.md is [ ]:
    1. Read REVIEW_STATE.md
    2. Identify next 10 unchecked packages
    3. Launch 10 Explore subagents IN PARALLEL (thoroughness: "very thorough")
    4. Wait for all 10 subagents to complete
    5. Analyze all findings
    6. Update REVIEW_FINDINGS.md with all 10 packages
    7. Update REVIEW_STATE.md marking all 10 as [x]
    8. Check if more packages remain
    9. IF more remain: CONTINUE to next batch (goto step 1)
    10. IF all complete: Output completion message and STOP
```

---

## PHASE 1: BATCH PREPARATION

For each iteration:

1. **Read State**: Load REVIEW_STATE.md
2. **Count Remaining**: Count unchecked [ ] packages
3. **Select Batch**: Take next 10 (or fewer if <10 remain)
4. **Plan Parallel Deployment**: Prepare 10 distinct subagent tasks

---

## PHASE 2: PARALLEL SUBAGENT DEPLOYMENT

For each of the 10 packages, launch an Explore subagent with Task tool:

**Subagent Configuration:**
- `subagent_type`: "Explore"
- `description`: "Review [package-name]"
- `prompt`:

```
Find ALL usages of package "[package-name]" in this codebase.

Search exhaustively for:
1. Import statements: `from '[package]'` and `import('[package]')`
2. Require statements: `require('[package]')`
3. Type imports: `import type {} from '[package]'`
4. Dynamic imports
5. References in package.json, tsconfig.json, config files
6. Usage in .ts, .tsx, .js, .jsx, .mjs, .cjs files

For each usage found:
- Note the file path and line number
- Read surrounding context (5-10 lines)
- Identify the usage pattern (initialization, hooks, components, utilities)
- Check for potential issues (deprecated APIs, missing error handling, anti-patterns)

Return a comprehensive summary:
- Total files using this package: [N]
- Usage patterns observed: [list]
- Potential issues: [list]
- Code examples: [2-3 key examples]
- Recommendation: [Keep/Remove/Update/Fix]
```

**Critical**: Launch ALL 10 subagents in a SINGLE message with 10 Task tool calls in parallel.

---

## PHASE 3: FINDINGS SYNTHESIS

When all 10 subagents return:

1. **Analyze Each Report**: Review all 10 subagent findings
2. **Categorize Packages**:
   - ✅ Used correctly
   - ⚠️ Used with issues
   - 🔥 Critical problems
   - 🗑️ Unused (can remove)
   - 📝 Needs documentation

3. **Document Findings**: Append to REVIEW_FINDINGS.md:

```markdown
## Batch [N] - [Date] - Packages [X-Y]

### [Package Name 1]
**Status**: [Used/Unused/Misused]
**Files**: [N files]
**Usage Pattern**: [Description]
**Issues**:
- [Issue 1]
- [Issue 2]
**Recommendation**: [Action]
**Evidence**: [Code examples from subagent]

---

### [Package Name 2]
...
```

---

## PHASE 4: STATE UPDATE

1. **Mark Complete**: Update REVIEW_STATE.md, change [ ] to [x] for all 10 packages
2. **Save Changes**: Ensure both files are written to disk
3. **Log Progress**: Output progress update (not termination!):
   ```
   ✅ Batch [N] complete: 10 packages reviewed
   📊 Progress: [X/92] packages complete ([P]%)
   🔄 Continuing to next batch...
   ```

---

## PHASE 5: LOOP CONTINUATION

**DO NOT STOP HERE!**

Immediately:
1. Re-read REVIEW_STATE.md
2. Count remaining [ ] packages
3. If remaining > 0: GOTO PHASE 1 (next batch)
4. If remaining = 0: GOTO PHASE 6 (completion)

**DO NOT** ask user to run a command. **DO NOT** wait. **KEEP GOING**.

---

## PHASE 6: COMPLETION & TERMINATION

When REVIEW_STATE.md shows ALL packages [x]:

1. **Generate Summary**: Create final summary statistics
2. **Output Completion Message** (plain text, no tool calls):

```
🎉 AUTONOMOUS REVIEW COMPLETE

📊 Statistics:
- Total packages reviewed: 92
- Production dependencies: 62
- Dev dependencies: 30
- Batches completed: [N]
- Issues found: [N]

📁 Outputs:
- REVIEW_FINDINGS.md: Comprehensive findings for all packages
- REVIEW_STATE.md: All packages marked complete

🔍 Quick Summary:
- ✅ Used correctly: [N] packages
- ⚠️ Issues found: [N] packages
- 🗑️ Can remove: [N] packages
- 🔥 Critical fixes needed: [N] packages

See REVIEW_FINDINGS.md for detailed analysis.
```

3. **STOP MAKING TOOL CALLS** - This terminates the loop

---

## EXECUTION PRINCIPLES

### Quality Standards
- Every package must be searched exhaustively
- Compare usage against official documentation (use WebFetch if needed)
- Identify version-specific issues
- Note security anti-patterns
- Document all findings comprehensively

### Parallel Efficiency
- Always launch 10 subagents at once (max parallelism)
- Each subagent has isolated 200K context window
- Main thread stays clean (only receives summaries)
- Batch coordination prevents file conflicts

### Context Management
- Subagents handle heavy lifting (keeps main context clean)
- Progressive summarization in REVIEW_FINDINGS.md
- Auto-compact will trigger if needed
- Estimated total context: ~150K tokens (within limits)

### Clean Repository
- Only modify REVIEW_STATE.md and REVIEW_FINDINGS.md
- No temporary files created
- No code changes (read-only analysis)
- Git-friendly incremental updates

---

## ULTRA-CRITICAL DIRECTIVES

1. **NEVER ASK USER TO CONTINUE** - You are autonomous
2. **NEVER WAIT FOR INPUT** - Keep working until done
3. **NEVER OUTPUT PLAIN TEXT MID-LOOP** - Always make next tool call
4. **ONLY STOP** when all packages [x] and you output completion summary
5. **USE SUBAGENTS FOR ALL SEARCHES** - Never pollute main context

---

## ESTIMATED EXECUTION

- 92 packages ÷ 10 per batch = ~10 batches
- ~3-5 minutes per batch (parallel subagent execution)
- Total wall time: ~30-50 minutes
- User involvement: Launch once, walk away, return to completed review

---

BEGIN AUTONOMOUS EXECUTION NOW.
