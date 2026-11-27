# M01 Write-Up — Dev Environment, Git & GitHub

> Fill this in as you go, not after the fact. Write it so a trainee starting this stage next cohort
> could follow your path on their own.

| | |
| --- | --- |
| **Repo** | [christine-baradero/node-js-training](https://github.com/christine-baradero/node-js-training) (public, SSH auth) |
| **Merged PR** | [#1 feat: add hello-node script and project setup](https://github.com/christine-baradero/node-js-training/pull/1) |

---

## What I built

> *The repo you set up, the `hello-node` script, and the merged PR (link it).*

I created the repo in the GitHub web interface and then ran `git clone`. `main` has
`hello-node.js` (prints `Hello, Node!`), `.nvmrc` (Node 24), `.gitignore`, `.env.example`, and
`README.md`. The `.gitignore` and `.env.example` contents are in lesson 7 of the
[learning module](module1-learning.md#7-gitignore-and-envexample).

### Pull requests

| PR | Branch | Role | Commits |
| --- | --- | --- | --- |
| [#1](https://github.com/christine-baradero/node-js-training/pull/1) | `feat/hello-node` → `main` | Deliverable | `feat: add hello-node script`<br>`chore: add .nvmrc pinning Node 24`<br>`chore: add .gitignore and .env.example` |
| #2 | `feat/greeting-b` → `main` | Helper, merged first | `feat: add hello-node script on branch b` |

---

## Why it's built this way (key decisions)

### Why did you structure your branches and commits the way you did?

I structured the branches as two separate ones that commit the same file, to showcase a merge
conflict. I created the first branch (`feat/hello-node`), added the `hello-node.js` file there and
committed it, as well as the `.nvmrc`, `.gitignore`, and `.env.example` files for project setup,
and then created the first PR. Afterwards, I created another feature branch (`feat/greeting-b`),
added the same file with different content, committed it, and created the second PR. Both PRs
pointed to `main`. To simulate resolving a merge conflict, I merged the second PR first, and after
that, a merge conflict was detected on the first PR.

### The repo is public, by program convention — what would have changed if it had been private with a no-direct-push rule instead?

On a private repo, branch protection depends on the GitHub plan, so "no direct pushes to `main`"
might only be a team agreement instead of a rule GitHub enforces. The branch, PR, and merge
workflow stays the same.

### How did you resolve the conflict, and would merge vs rebase have changed it?

Both branches added `hello-node.js` with different content, so Git reported an add/add conflict.
I resolved it through GitHub and accepted my current changes on the branch, which kept:

```js
console.log("Hello, Node!");
```

That produced the merge commit `Merge branch 'main' into feat/hello-node`. With a rebase, my
commits would have been replayed on top of `main` instead, giving a linear history with new hashes
and no merge commit. The merge keeps both lines of history and shows where the conflict was
resolved.

---

## How to build it (teach it to the next trainee)

> *Write a guide to taking a change through branch → PR → merge, including a real conflict, using
> your own example to show the reasoning, not just the Git commands.*

The full step-by-step guide is the
[final exercise](module1-learning.md#9-final-exercise-your-first-pr-with-a-conflict) in the
learning module. It follows my own build: one feature branch with the script and setup files, a
second branch that adds the same file, and merging the second PR first so `main` moves ahead and
creates a real conflict on the first PR.

---

## Concepts worth explaining

> *Pick 1-2 ideas — the working-tree/staging/commit model, `revert` vs `reset`, or merge vs
> rebase — and explain each in your own words.*

I explained two ideas in
[lesson 6](module1-learning.md#6-branches-merges-and-conflicts) of the learning module:
resolving a merge conflict on GitHub vs locally, and merge vs rebase.

---

## What tripped me up

> *Anything that didn't behave the way you expected the first time.*

My first commit message didn't follow Conventional Commits, so I amended it to `docs: add README`.

---

## Checkpoint evidence

> *Show the merged PR link, your `.gitignore`/`.env.example`, and your own explanation of
> `git revert` vs `git reset`.*

Attached the PR link, and the two mentioned files.

| Evidence | Link |
| --- | --- |
| Merged PR | [#1](https://github.com/christine-baradero/node-js-training/pull/1) |
| `.gitignore` and `.env.example` | [node-js-training on `main`](https://github.com/christine-baradero/node-js-training) |

### `git revert` vs `git reset`

**`git revert`** creates a new commit that undoes the changes from an earlier commit. The original
commit stays in the history, so nothing is rewritten. That makes it the safe choice for undoing
changes on shared branches like `main`, where others may already have the commit.

**`git reset`** moves the branch back to an earlier commit, which removes all the commits after it
from the branch. Depending on the mode, the undone changes are kept staged (`--soft`), kept
unstaged (`--mixed`, the default), or discarded (`--hard`). Because it rewrites history, it is best
used on local commits that haven't been pushed yet, for example to tidy up my work before pushing.

---

## What I'd do differently

> *If you started this module over, what would you do differently?*

I'd write the README commit as a Conventional Commit from the start. My first commit message
had no prefix, so I amended it to `docs: add README`:

```bash
git commit --amend -m "docs: add README"
```
