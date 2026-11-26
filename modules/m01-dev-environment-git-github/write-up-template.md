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

I created the repo in the GitHub web interface and then ran `git clone`.

### Files on `main`

**`hello-node.js`**, the script:

```js
console.log("Hello, Node!");
```

Run it with:

```bash
node hello-node.js
# Hello, Node!
```

**`.nvmrc`**, pinning the cohort's Node version:

```text
24
```

**`.gitignore`**:

```gitignore
node_modules
.env
*.db
src/generated/prisma
dist
coverage
```

**`.env.example`**:

```bash
# Copy to .env and fill in real values. Never commit .env.
```

### Pull requests

| PR | Branch | Role | Commits |
| --- | --- | --- | --- |
| [#1](https://github.com/christine-baradero/node-js-training/pull/1) | `feat/hello-node` → `main` | Deliverable | `feat: add hello-node script`<br>`chore: add .nvmrc pinning Node 24`<br>`chore: add .gitignore and .env.example` |
| #2 | `feat/greeting-b` → `main` | Helper, merged first | `feat: add hello-node script on branch b` |

### The conflict

Both branches added `hello-node.js` with different content (an add/add conflict). I merged `main`
into `feat/hello-node`, resolved it, then merged PR #1 with a merge commit, so the resolution is in
`main`'s history.

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

1. **Create the repository in the GitHub web interface.**

2. **Clone it** to your machine:

   ```bash
   git clone git@github.com:<your-username>/node-js-training.git
   cd node-js-training
   ```

3. **Create the first branch from `main`** and add `hello-node.js`, `.nvmrc`, `.gitignore`, and
   `.env.example`, committing each change with a Conventional Commit:

   ```bash
   git switch -c feat/hello-node
   # add hello-node.js
   git add hello-node.js
   git commit -m "feat: add hello-node script"
   # add .nvmrc
   git add .nvmrc
   git commit -m "chore: add .nvmrc pinning Node 24"
   # add .gitignore and .env.example
   git add .gitignore .env.example
   git commit -m "chore: add .gitignore and .env.example"
   ```

4. **Push it and open the first PR** into `main`:

   ```bash
   git push -u origin feat/hello-node
   ```

5. **Create a second branch from `main`** and add the same file, `hello-node.js`, with different
   content. Commit it, push it, and open the second PR into `main`:

   ```bash
   git switch main
   git switch -c feat/greeting-b
   # add hello-node.js with different content
   git add hello-node.js
   git commit -m "feat: add hello-node script on branch b"
   git push -u origin feat/greeting-b
   ```

6. **Merge the second PR first.** `main` now has a `hello-node.js` that the first branch doesn't
   know about, so GitHub detects a merge conflict on the first PR.

7. **Resolve the conflict on GitHub** using the *Resolve conflicts* button. GitHub shows both
   versions between conflict markers:

   ```text
   <<<<<<< feat/hello-node
   console.log("Hello, Node!");
   =======
   console.log("Hello from branch B!");
   >>>>>>> main
   ```

   The part between `<<<<<<<` and `=======` is the current change from my branch. The part between
   `=======` and `>>>>>>>` is the incoming change from `main`. I accepted the current change and
   deleted the three marker lines, leaving:

   ```js
   console.log("Hello, Node!");
   ```

   Then I clicked *Mark as resolved* and *Commit merge*.

8. **Merge the first PR.** The resolved conflict is now part of the history of `main`.

9. **Sync locally:**

   ```bash
   git switch main
   git pull
   ```

---

## Concepts worth explaining

> *Pick 1-2 ideas — the working-tree/staging/commit model, `revert` vs `reset`, or merge vs
> rebase — and explain each in your own words.*

### Resolving a merge conflict depending on complexity

GitHub's conflict editor can resolve simple conflicts, such as competing changes to the same lines.
For other conflicts, or when the *Resolve conflicts* button is disabled, you resolve it locally with
the command line or another Git client. Resolving on GitHub merges the whole base branch into the
head branch.

### Merge vs rebase

Merge adds a merge commit and keeps history as it was, so you can see where the branches joined.
Rebase replays a branch's commits on top of another branch, giving a straight history, but each
commit gets a new hash. Use rebase only on local commits, and merge once they're shared, because
rewriting shared commits needs a force-push and breaks other people's copies. This applies whether
the repo is public or private.

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

**`git revert`** creates a new commit to undo changes introduced in a previous commit. This
preserves the history, so it is best to be used when undoing change on public branches.

**`git reset`** goes back to a specific previous commit and removing all the commits after it. It
is best to be used on your local repo, right before pushing to the remote branch.

---

## What I'd do differently

> *If you started this module over, what would you do differently?*

I'd write the README commit as a Conventional Commit from the start. My first commit message
had no prefix, so I amended it to `docs: add README`:

```bash
git commit --amend -m "docs: add README"
```
