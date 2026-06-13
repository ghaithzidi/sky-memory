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
- Added subdomain detection with fallback to the first active storefront in `src/app/api/reports/sales/route.ts`.
- Updated `storeUrl` construction to correctly include the subdomain (`http://${subdomain}.${host}`) or default to host only.
- Fixed a missing closing brace in `prisma/seed.ts` and ensured the file ends with a proper newline.
- Synchronized the Prisma schema with the database using `npx prisma db push --accept-data-loss`, which created the missing `RolePermission` table.
- Ran the seed script to populate initial data (governorates, shipping companies, admin user, role permissions, and the `itech` storefront).
- Started the Next.js development server for testing.
- Began implementation of a Git health monitoring system for the `sky-core` service:
  - Designed a lightweight `GitHealthCollector` to gather metrics (`sky_git_dirty_files`, `sky_git_last_commit_age_hours`, `sky_git_behind_commits`, `sky_vault_file_count`).
  - Integrated the collector into the existing `sky-core` service to run every 5 minutes without creating a new daemon.
  - Added Prometheus metric definitions and ensured they are exposed via the existing scrape endpoint.
  - Created Grafana panel definitions and alert rules (critical and warning thresholds).
  - Performed debugging of the collector (fixed branch detection, corrected behind‑commits calculation).

## Decisions made
- Use the `X-Subdomain` header set by middleware; if absent, query the first active storefront for a fallback subdomain.
- Build `storeUrl` with the detected subdomain to ensure dashboard links point to the correct storefront.
- Prefer `prisma db push` over a new migration to quickly sync the missing `RolePermission` table in the development DB.
- Implement Git health monitoring as an internal scheduled task within `sky-core` rather than a separate daemon, following existing architecture patterns.
- Use HTTP scheme for URL construction in development, but note the need to switch to the request’s protocol for production.

## Problems solved
- Dashboard store links were incorrect because `storeUrl` lacked subdomain handling.
- Seed script failed with “table `RolePermission` does not exist” due to unsynced schema.
- `prisma/seed.ts` had a missing closing brace, causing a syntax error.
- Git health collector initially returned incorrect behind‑commit values due to improper branch name handling; fixed with dynamic upstream detection.

## Next steps
1. **Run integration tests** to verify that `storeUrl` is correctly generated for both admin and storefront requests.
2. **Commit and push** the changes to the repository.
3. **Validate Prometheus scraping** of the new Git health metrics and confirm they appear in Grafana.
4. **Fine‑tune Grafana panels** and alert rule configurations as needed.
5. **Add unit tests** for the `GitHealthCollector` to ensure metric accuracy.
6. **Consider HTTPS handling** for `storeUrl` in production (use request protocol or `x-forwarded-proto` header).