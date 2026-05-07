---
name: delete-merged-pr-branches
description: Deletes all local git branches that have been merged into the default branch. Use when the user asks to clean up merged branches, prune branches, or delete old branches.
---

# Delete Merged Branches

Run the project script to delete all local branches merged into the default branch.

## Steps

1. Confirm the user is in a git repository.
2. Determine the default branch — ask if ambiguous, otherwise default to `master`.
3. Run the script:

```bash
./scripts/delete-merged-branches.sh [default-branch]
```

If the script is not present (e.g. user is in a different repo), run the equivalent inline:

```bash
git fetch --prune
git branch --merged <default-branch> \
  | grep -v -E "^\*|^\s*(master|main|develop|<default-branch>)$" \
  | xargs -r git branch -d
```

4. Report which branches were deleted, or confirm none were found.

## Notes

- Uses `-d` (safe delete) — branches with unmerged commits are never deleted.
- Always fetches and prunes remotes first so the merged list is up to date.
- Never deletes `master`, `main`, `develop`, or the current branch.