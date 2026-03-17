# AGENTS — Login Test

## Agent roles

- **Coding agent:** Implements and updates the codebase (e.g. `index.html`), project docs (PRD, ARCHITECTURE, PLAN, DOCUMENTATION, TIMELINE), and this file when roles change. Responsible for following INSTRUCTIONS.md (required files, bootstrap order, update rules).

## Tool and environment constraints

- Allowed: read/write project files, run static local server or one-off commands for verification.
- No backend or database runs in this repo; Supabase is the only external service.
- No deployment automation required; deployment is documented (GitHub Pages) and done by the user.

## Coordination

- Single agent; no handoff protocol. If multiple agents are introduced later, AGENTS.md will be updated and conflict resolution (e.g. who owns ARCHITECTURE vs DOCUMENTATION) will be defined.

## Conflict resolution

- Architectural and scope decisions follow PRD.md and ARCHITECTURE.md. Discrepancies between code and docs are flagged rather than silently changed.
