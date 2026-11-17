# CLAUDE.md

Roomzy: two-person ML course project (bagiicpp, JovanObr), each working with Claude.

## 1. Read Before Working

**Shared context lives in two files. Load them first.**

- `CONTEXT.md`: conventions, ownership, decision log. Source of truth; wins over `documentation/`.
- `GLOSSARY.md`: domain terms. Use them in code, UI and commits.

## 2. Protected Files

**Some files are shared contracts. Don't touch them alone.**

- Never edit `CLAUDE.md`, `CONTEXT.md`, `GLOSSARY.md` or `backend/schemas.py` without explicit permission from a teammate in the current session.

## 3. Stay in Scope

**One Issue = one branch = one PR.**

- Work only inside the GitHub Issue you were given.

## 4. Don't Invent Decisions

**If it isn't in `CONTEXT.md`, ask.**

- Don't invent decisions. If something isn't in CONTEXT.md, ask.

---

**These rules are working if:** both teammates' Claudes stay in sync, protected files change only with consent, and every PR maps to exactly one Issue.
