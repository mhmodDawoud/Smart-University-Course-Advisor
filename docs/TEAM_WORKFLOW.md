# Team workflow (Git / GitHub)

## Branches
- `main` always contains working, reviewed work. Do not push to `main` directly.
- Create a branch per task: `feature/<short-task-name>-<your-name>`, e.g. `feature/csp-prereqs-ahmed`.

## Day-to-day
1. Pick (or get assigned) an **Issue** and say you are working on it.
2. `git checkout main && git pull`, then create your branch.
3. Commit small and often with clear messages (e.g. `search: reject sections with time overlap`).
4. Push your branch and open a **Pull Request** using the template.
5. At least **one teammate reviews** before merging. Resolve comments, then merge.
6. Delete the branch after merging.

## Avoiding conflicts
- One owner per file or notebook where possible.
- Pull `main` into your branch often.
- Do not edit another member's module without telling them.
- Clear notebook outputs before committing big notebooks.

## Definition of done
- Code runs from a clean checkout.
- Has a short explanation in the matching folder README or in `docs/`.
- Has basic tests where it makes sense.
- Reviewed by a teammate.

## Meetings
- Short regular sync (agree on a day), update the status checklist in the README.
