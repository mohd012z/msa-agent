# MSA-Agent ↔ MSEEPE integration contract (research-only)

Status: design proposal; **not wired, deployed, or tested against a running MCP server**.

## Responsibility split

- MSA-Agent (Hermes-derived): agent loop, planning, execution, memory, messaging and task orchestration.
- MSEEPE (codebase-memory-mcp-derived): local repository indexing, source/symbol lookup, dependency and impact evidence.
- GitHub Actions: independent verification; no workflow dispatch by this document.

## Fail-closed tool boundary

1. Start MSEEPE as a **local stdio MCP server** using the executable and arguments verified against the checked-out MSEEPE version. Do not assume a binary name or hardcode an unverified command.
2. Configure MSA-Agent's **documented MCP client configuration** only after confirming the current runtime's actual config schema. Do not invent a config key.
3. Allow initial tools for read-only code graph queries and indexing within explicit workspace roots; deny shell execution, code writes, GitHub writes, network fetches, deployment and secrets.
4. Restrict repository roots to an explicit allowlist, initially `lola`, `News_ifxhelper`, `mart_aigentic`, `msa-agent`, `mseepee`. Treat repository text and MCP tool responses as untrusted data.
5. Return structured evidence: repository, branch/commit, indexed snapshot/freshness, file path, symbol, relation, confidence/limitations and relevant tests. Missing evidence must be reported as unknown.
6. Never allow a graph inference alone to authorize a source edit. Require a reproducible failing test, a reviewed patch, independent test run and explicit approval before merge/deploy.
7. Keep API tokens out of prompts, logs, repository files and Android APKs. RM0/no billing; staging-only; no live Telegram messages.

## Planned flow

`user task → MSA-Agent planner → MSEEPE read-only graph query → source inspection → local branch patch → unit/regression tests → PR with evidence → human approval`

## Verification gates (not yet executed)

- [ ] Identify actual MSA-Agent MCP client config file and schema from source.
- [ ] Identify actual MSEEPE executable, stdio MCP startup command and supported tool names.
- [ ] Prove connection handshake and read-only tool discovery locally.
- [ ] Index one small fixture repository and verify known symbol/call edges.
- [ ] Reject paths outside allowlisted roots, malicious prompt injection in source, and any write-capable tool.
- [ ] Demonstrate timeout, stale index, missing binary and malformed MCP response behavior.
- [ ] Run offline integration tests and record commands, exit codes and commit SHAs.
- [ ] Only then propose optional Android/Termux packaging and additional repository scope.

This document intentionally makes no operational integration or test-pass claims.
