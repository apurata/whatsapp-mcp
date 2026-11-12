# Branching strategy (Apurata fork)

Repo: [apurata/whatsapp-mcp](https://github.com/apurata/whatsapp-mcp)  
Upstream: [lharries/whatsapp-mcp](https://github.com/lharries/whatsapp-mcp) (track via `main`).

## Branches

| Branch | Role |
|---|---|
| **`main`** | Mirror of **upstream**. Keep in sync. No Apurata-only commits here. |
| **`apurata`** | **Internal use branch** for Cursor / Apurata. All local improvements live here. Clones and MCP wrappers should track this branch. |
| **`feature/*`**, **`fix/*`** | Short-lived work. Always branch from **`main`**. |

Do **not** open PRs to upstream from `apurata` tip (it accumulates Apurata-only history). Cherry-pick or re-branch from `main` when contributing back.

**Upstream contribution (issue-first):** when filing work on this fork, if the delta is **generalizable** (not Apurata-only paths or policy), also open a **GitHub issue** on [`lharries/whatsapp-mcp`](https://github.com/lharries/whatsapp-mcp). Link **Linear → GitHub only** (never put Apurata Linear URLs in the upstream issue). Offer a PR in the GitHub issue; **do not open the PR** until the maintainer asks (or the human explicitly OK’s). Prefer issues over unsolicited PRs. Keep `main` in sync via merge (not rebase of published tips).

## Workflow

```text
upstream ──► main (sync only)
                │
                └── feature/foo
                        │
                        └── merge local → apurata + push
                              keep origin/feature/foo
                              open upstream issue (offer PR; wait)
```

1. **Daily Cursor MCP:** `git checkout apurata && git pull`.
2. **New work:** `git checkout main && git pull` → `git checkout -b feature/…` (or `fix/…`).
3. **Ship internally (default):** merge locally into `apurata`, push `apurata`, **delete the local** `feature/*`/`fix/*` branch, **leave the remote** branch (so it can become a PR to the original later). GitHub PRs on this fork are allowed, not the usual path.
4. **Upstream:** open GitHub **issue** when the delta is generalizable; Linear → GitHub link only; PR only if they ask or the human OK’s — same remote feature branch. After accept, sync `main` and keep `apurata` current.
5. **Sync main:** periodically `git fetch upstream` and **merge** (prefer `--ff-only` when possible; otherwise a merge commit) into `main` — never rewrite remote history / no rebase of published tips. Then merge `main` (or `upstream/main`) into `apurata` via a short-lived `chore/sync-upstream-*` branch.

## Upstream sync — definition of done

1. Tag **before** merge: `apurata-pre-sync-YYYY-MM-DD` on current `apurata`.
2. Merge upstream into `chore/sync-upstream-*` → resolve → merge into `apurata`.
3. Smoke: bridge starts, MCP lists read tools only (when read-only is on), search/list against local SQLite.
4. Tag **after**: `apurata-synced-upstream-YYYY-MM-DD` (or upstream tag) on the new `apurata` tip.
5. Push `main`, `apurata`, and the `chore/…` branch (no rebase / no force-push).

Rollback: `git checkout apurata && git reset --hard apurata-pre-sync-YYYY-MM-DD` (or the last `apurata-synced-*`), then restart Cursor MCP.

## Apurata delta on this tip

- **Read-only:** MCP omits `send_*` tools; bridge `/api/send` returns 403.
- **Localhost bind** for the Go bridge REST API (`127.0.0.1`).
- **Current whatsmeow** + companion name **Comet** (`PlatformType` CHROME) so pairing works.
- **Optional drafts** (local files only): still open if we want them later.

## Cursor / ops pointer

- Clone path (Apurata monorepo): **`apurata/whatsapp-mcp/`** (inner git repo; see monorepo `download_repos.sh`).
- Runbook: `docs/whatsapp/whatsapp_mcp_cursor.md` in the [apurata monorepo](https://github.com/apurata/apurata) (or local root).
