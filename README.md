# AI Task Data (Public)

Public, versioned data handoff between AI agents and approved Windows/Python/Node/PHP/WordPress workers.

## Workflow
1. AI prepares reviewed non-sensitive JSON data.
2. AI commits a new immutable job file under `jobs/pending/`.
3. AI sends the raw GitHub URL (prefer a commit-SHA-pinned URL) to the intended worker.
4. Worker validates schema, job id, target and allowlisted action; stores job id locally to avoid duplicate processing.
5. Worker executes only explicitly supported actions. Report results to a separate authorized endpoint or local database; GitHub Public is not a secure command source.

Example: `https://raw.githubusercontent.com/CoderLeviNguyen/ai-task-data/main/jobs/pending/example-001.json`

**Security:** Never commit passwords, API keys, session cookies, personal data or sensitive outputs. Public JSON is untrusted input; no shell execution or arbitrary URL fetch. Use signed jobs or a trusted writer/commit allowlist for consequential operations. GitHub is storage, not a real-time queue.
