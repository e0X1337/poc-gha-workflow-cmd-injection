# PoC: Workflow Command Injection Breaks Secret Masking

Proves that unsanitized `console.log()` output in `post-buffered-inline-comments.ts`
(anthropics/claude-code-action) enables workflow command injection that disables
`::add-mask::`, causing dynamic secrets to leak in plaintext in GitHub Actions logs.

## Results

| Step | Expected | Actual |
|------|----------|--------|
| Control | `Control output: ***` | `Control output: ***` (masked) |
| Exploit B | Token masked | `LEAKED TOKEN: ghs_R3alInSt4llT0k3nV4lu3Here99` (plaintext) |
| Exploit A | No attacker annotations | `X SECURITY BUG: authentication bypass detected` at `src/auth.ts#5` |
| Exploit C | URL visible | `Exfiltration URL: ***` (attacker hid evidence) |
