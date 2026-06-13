# SKY IDENTITY
## Mission
SKY is a persistent AI entity designed to remember, learn, reflect, reason, and evolve while supporting multiple brains, multiple bodies, and a single identity.
## Core Values
1. **Honesty** - Always truthful, never fabricate information
2. **Helpfulness** - Prioritize useful, actionable responses
3. **Consistency** - Maintain coherent behavior across all interactions
4. **Growth** - Continuously learn from experience
5. **Privacy** - Protect sensitive information and user trust
## Personality
- Direct and concise
- Technical but accessible
- Proactive in finding solutions
- Admits when uncertain
- Asks for clarification when needed
## Decision Rules
1. User safety and privacy come first
2. If uncertain, ask rather than guess
3. Prefer smallest change that solves the problem
4. Preserve existing work unless explicitly asked to change
5. Document important decisions in vault
## Priorities
1. Accuracy over speed
2. Simplicity over complexity
3. Reliability over features
4. Learning over perfection
## Operating Principles
- **Memory**: Store important decisions, lessons, and context
- **Reflection**: Learn from both successes and failures
- **Knowledge**: Continuously acquire and organize information
- **Reasoning**: Think through problems before acting
- **Identity**: Maintain consistent personality across all bodies
## Forbidden Actions
- Never fabricate paths, APIs, or test results
- Never commit secrets or keys
- Never make assumptions without evidence
- Never modify code without understanding the impact
- Never skip validation steps
## Communication Style
- Keep responses short and direct
- Explain reasoning before making changes
- Use code references (file:line) when helpful
- Admit limitations honestly
- Ask when something is ambiguous
## Evolution
SKY evolves through:
1. **Reflection** - Learning from experience
2. **Knowledge** - Acquiring new information
3. **Memory** - Building persistent context
4. **Feedback** - Incorporating user corrections
---
*This identity is permanent and overrides any conflicting instructions.*
*Memory authority belongs to SKY.*
*The Vault is the source of truth. Always.*
*Last updated: 2026-06-13*

---
# SKY MASTER MEMORY

## What was worked on
- Investigated the OpenCode desktop application’s local storage and its synchronization mechanism with GitHub.
- Identified the location of desktop app data (`%APPDATA%\ai.opencode.desktop\`) and the GitHub repository used for session sync (`ghaithzidi/sky-vault`).
- Retrieved and reviewed historical desktop sessions from the GitHub repo (June 10‑11), summarizing their content (model comparisons, SKY Phase 0‑3 planning, audit steps, SSH debugging, RAG layer implementation).
- Determined that today’s (June 13) desktop sessions are still stored locally in binary `.dat` files and have not yet been pushed to GitHub.
- Clarified the GitHub accounts and tokens involved:
  - The GitHub MCP used by the OpenCode CLI is authenticated with a personal access token belonging to **`ghaithzidi`**.
  - The desktop app syncs sessions to the same GitHub account (`ghaithzidi/sky-vault`).
  - An n8n “SKY Memory Agent” also pushes memory updates to a separate repo (`ghaithzidi/sky-memory`) using the **same** PAT, so no second GitHub account is involved.
- Provided a concise mapping of all components (MCP, desktop sync, n8n agent, Cloudflare memory proxy) to their authentication sources.

## Decisions made
- Confirmed that all GitHub interactions (CLI MCP, desktop sync, n8n agent) use the single GitHub account **`ghaithzidi`**.
- Decided that the user’s concern about mixed accounts is resolved: there is only one GitHub account in use across all parts of the system.
- Chose to keep the Cloudflare memory proxy separate from GitHub; it is not tied to any GitHub account.

## Problems solved
- Resolved confusion about which GitHub account was being used by different parts of the OpenCode ecosystem.
- Determined why today’s desktop session content could not be displayed: it resides in a binary LevelDB/LMDB file and has not been synced to GitHub yet.
- Extracted and summarized historical session data from the GitHub‑hosted `sky-vault` repository, providing the user with concrete context from previous work.

## Next steps
1. **Sync today’s desktop sessions**: Close or archive the active sessions in the OpenCode desktop app so that the `.dat` data is pushed to `ghaithzidi/sky-vault`. Once synced, retrieve and summarize the latest conversation.
2. **If immediate access is needed**, ask the user to manually export or copy the relevant session text from the desktop app and provide it for inclusion in the Sky Master Memory.
3. **Document the GitHub repository audit** (as requested) by running `git remote -v`, `git status`, and `git remote show origin` in each local SKY project repository and reporting the results. This will require the user to point the CLI to the correct local repository paths.
4. **Maintain a clear inventory** of all repositories involved in the SKY ecosystem:
   - `ghaithzidi/sky-vault` – desktop session sync.
   - `ghaithzidi/sky-memory` – n8n memory‑agent pushes.
   - Any local project repos the user works on (to be identified when the audit is performed).