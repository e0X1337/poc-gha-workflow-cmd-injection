# PoC: GitHub Actions Workflow Command Injection

Demonstrates workflow command injection via unsanitized `body` field logged
to stdout in `post-buffered-inline-comments.ts` (anthropics/claude-code-action).

## Vulnerability

When buffered inline comments are classified as "test/probe", their `body`
field is logged via `console.log` without neutralizing embedded newlines or
`::command::` sequences. The GitHub Actions runner parses stdout line-by-line
for workflow commands, so a body containing `\n::warning::payload` produces
an injected annotation.

## Expected Results

- **Step 2**: Injected `::warning::` and `::error::` appear as real CI annotations
- **Step 3**: If `::stop-commands::` was processed, subsequent `::warning::` is suppressed
- **Step 4**: Control step shows normal annotations work independently
