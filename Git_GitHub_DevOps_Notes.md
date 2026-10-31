# Git & GitHub Notes

## 1. Git and GitHub

### Git
Git is a distributed version control system (DVCS). I use it to track changes in source code, maintain version history, and collaborate with other developers.

### GitHub
GitHub is a cloud-based platform used to host Git repositories and collaborate through pull requests, code reviews, issues, and GitHub Actions.

| Git | GitHub |
|---|---|
| Version control tool | Repository hosting and collaboration platform |
| Works locally and offline for many operations | Hosts repositories and provides collaboration features |
| Tracks changes and commit history | Supports pull requests, reviews, issues, and automation |

### Types of Version Control Systems

**CVCS — Centralized Version Control System**
- A central server stores the repository.
- Developers access the central repository to collaborate.
- Example: SVN.

**DVCS — Distributed Version Control System**
- Each developer has a local copy of the repository and its history.
- Many Git operations can be performed offline.
- Changes can later be pushed to a remote repository.
- Example: Git.

---

## 2. Git Workflow

Git has four main areas:

1. **Working directory:** The files I create or modify.
2. **Staging area:** The changes I select for the next commit.
3. **Local repository:** The committed history stored locally in `.git`.
4. **Remote repository:** A shared repository hosted on GitHub or another Git server.

**Basic workflow:**

`Working Directory → git add → Staging Area → git commit → Local Repository → git push → Remote Repository`

- `git add` stages changes.
- `git commit` saves staged changes in the local repository.
- `git push` sends local commits to the remote repository.

---

## 3. Initial Git Configuration

```bash
git --version

git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

git config --list
```

- `git --version` checks the installed Git version.
- `git config` sets or displays Git configuration.
- `--global` applies the setting to all repositories for my user account.

---

## 4. Create and Initialize a Repository

```bash
mkdir my-project
cd my-project
git init
```

`git init` initializes a new local Git repository by creating a `.git` directory.

### Check repository status

```bash
git status
```

Shows the current branch and whether files are untracked, modified, staged, or committed.

### Add files to staging

```bash
git add README.md
git add .
```

- `git add README.md` stages one file.
- `git add .` stages changes under the current directory.

### Commit changes

```bash
git commit -m "Initial commit"
```

A commit records the staged changes in the local repository.

### View commit history

```bash
git log
git log --oneline
git log --oneline --graph --all
```

### View changes

```bash
git diff
git diff --staged
```

- `git diff` shows changes in the working directory that are not staged.
- `git diff --staged` shows changes that are staged for the next commit.

---

## 5. Connect a Local Repository to GitHub

First, create an empty repository on GitHub. Then connect my local repository:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git remote -v
```

- `origin` is the conventional name for the remote repository.
- `git remote -v` displays the configured remote URLs.

### Push code to GitHub

```bash
git push -u origin main
```

- `origin` is the remote name.
- `main` is the branch being pushed.
- `-u` sets the upstream tracking branch, so future pushes can usually use `git push`.

```bash
git push
```

### Clone an existing repository

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

`git clone` downloads the repository and its history to my local machine.

---

## 6. Fetch vs Pull

### Fetch

```bash
git fetch origin
```

Downloads updates from the remote repository but does not integrate them into my current branch.

### Pull

```bash
git pull origin main
```

Fetches and integrates changes from the specified remote branch into my current branch. The integration may use merge or rebase depending on the Git configuration.

| `git fetch` | `git pull` |
|---|---|
| Downloads remote updates | Downloads and integrates remote updates |
| Does not merge/rebase into the current branch | Integrates updates into the current branch |
| Useful when I want to inspect changes first | Useful when I want to synchronize my branch |

---

## 7. Branching

A branch is a separate line of development. I can work on a feature or bug fix without directly changing the main branch.

### Important branch commands

```bash
git branch
git branch feature-login
git switch feature-login
git switch -c feature-login
git branch -a
git branch -d feature-login
```

- `git branch` lists local branches.
- `git branch feature-login` creates a branch.
- `git switch feature-login` switches to an existing branch.
- `git switch -c feature-login` creates and switches to a new branch.
- `git branch -a` lists local and remote-tracking branches.
- `git branch -d feature-login` deletes a merged local branch.

### Push a new branch to GitHub

```bash
git push -u origin feature-login
```

### Typical feature-branch workflow

```bash
git switch main
git pull origin main

git switch -c feature-login

# Make code changes

git add .
git commit -m "Add login feature"
git push -u origin feature-login
```

After pushing, I can open a Pull Request on GitHub for review.

### Common branch names

- `main` — production-ready code.
- `develop` — integration branch, if the team uses one.
- `feature/*` — new functionality.
- `release/*` — release preparation.
- `hotfix/*` — urgent production fixes.

Branching strategies depend on the team's process. Not every team uses all these branches.

---

## 8. Pull Request (PR)

A Pull Request is a proposal to merge changes from one branch into another.

Typical PR process:

1. Create a feature branch.
2. Make changes and commit them.
3. Push the branch to GitHub.
4. Open a Pull Request.
5. Request code review.
6. Address review comments.
7. Ensure required checks pass.
8. Merge the PR according to the team's process.

PRs help teams review code and control what gets merged into important branches.

---

## 9. Merge and Merge Conflict

### Merge

A merge combines changes from one branch into another.

```bash
git switch main
git pull origin main
git merge feature-login
git push origin main
```

### Merge conflict

A merge conflict happens when Git cannot automatically combine changes, for example when two branches modify the same lines in a file.

### Steps to resolve a conflict

1. Check which files have conflicts.
2. Open the conflicted files.
3. Review the conflict markers.
4. Keep the correct final content and remove the markers.
5. Stage the resolved files.
6. Complete the merge.

```bash
git status
git add <filename>
git commit
```

The final `git commit` completes a merge when Git has paused for conflict resolution. Some merges may complete automatically without requiring a manual commit.

---

## 10. Merge vs Rebase

### Merge

```bash
git switch feature
git merge main
```

- Combines branch histories.
- May create a merge commit.
- Preserves the existing commit ancestry.

### Rebase

```bash
git switch feature
git rebase main
```

- Replays feature-branch commits on top of another base.
- Produces a more linear history.
- Rewrites the rebased commits, so their commit IDs change.

**Important:** I should avoid rebasing commits that have already been shared with others unless the team has agreed to that workflow.

---

## 11. Undo Changes: Restore, Reset, and Revert

### Restore

```bash
git restore filename.txt
```

Discards unstaged changes in a file. This can permanently discard those edits.

```bash
git restore --staged filename.txt
```

Unstages a file but keeps its working-directory changes.

### Reset

```bash
git reset --soft HEAD~1
```

Moves the current branch pointer back one commit and keeps the changes staged.

```bash
git reset --mixed HEAD~1
```

Moves the branch pointer back one commit and keeps the changes in the working directory, unstaged. This is the default reset mode.

```bash
git reset --hard HEAD~1
```

Resets the commit, staging area, and working directory. This can discard work, so I should use it carefully.

### Revert

```bash
git revert <commit-id>
```

Creates a new commit that reverses the changes introduced by an earlier commit.

**Interview point:** For a commit that has already been pushed and shared, `git revert` is generally safer than rewriting shared history with `git reset`.

---

## 12. Stash, Cherry-pick, Tags, and Git Ignore

### Stash

Temporarily saves working changes so I can switch tasks without committing them.

```bash
git stash
git stash list
git stash pop
git stash apply
```

- `git stash` saves changes.
- `git stash list` lists saved stashes.
- `git stash pop` applies the latest stash and removes it from the stash list.
- `git stash apply` applies the latest stash but keeps it in the stash list.

### Cherry-pick

Applies a specific commit from another branch to my current branch.

```bash
git cherry-pick <commit-id>
```

### Tags

Tags mark specific commits, commonly for releases.

```bash
git tag v1.0
git tag
git push origin v1.0
git push origin --tags
```

### `.gitignore`

`.gitignore` contains patterns for untracked files that Git should ignore, such as logs, build output, local environment files, and generated folders.

Example:

```gitignore
*.log
.env
node_modules/
build/
```

A `.gitignore` rule does not automatically stop tracking a file that has already been committed.

---

## 13. Useful GitHub Features

| Feature | Purpose |
|---|---|
| Pull Requests | Review and approve code before merging |
| Branch protection / rulesets | Require reviews or status checks and restrict direct pushes |
| GitHub Actions | Automate build, test, and deployment workflows |
| Issues | Track bugs, tasks, and enhancements |
| Fork | Create my own copy of another user's repository |
| Tags and Releases | Mark versions and publish release information |
| GitHub Secrets | Store sensitive values used by workflows |
| README.md | Document the project, setup, and usage |

---

## 14. GitHub Authentication

### HTTPS with Personal Access Token (PAT)

- A PAT is used instead of an account password for Git operations over HTTPS when authentication is requested.
- I should give the token only the permissions required.
- I should store credentials securely using a credential manager.
- I should never share or commit the token.

### SSH authentication

1. Generate an SSH key pair.
2. Add the public key to GitHub.
3. Keep the private key secure.
4. Test the connection.

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
ssh -T git@github.com
```

**Security point:** Never commit passwords, tokens, private keys, or sensitive `.env` files. If a credential is exposed, revoke or rotate it immediately. Deleting it from the latest commit does not remove it from Git history.

---

## 15. Git in a DevOps CI/CD Pipeline

Typical workflow:

`Developer → GitHub → CI Pipeline → Build → Test → Deploy`

1. A developer commits and pushes code to GitHub.
2. A GitHub Actions workflow or another CI tool detects the change.
3. The pipeline builds the application.
4. Automated tests are executed.
5. If the required checks pass, the application can be deployed to the target environment.

### CI — Continuous Integration

Developers frequently integrate code changes into a shared repository. Automated builds and tests help detect problems early.

### Continuous Delivery

The application is kept ready for release, but production deployment may require manual approval.

### Continuous Deployment

Changes that pass the required pipeline checks are automatically deployed to production.

**GitHub Actions:** A GitHub automation and CI/CD service. Workflows are commonly defined as YAML files under `.github/workflows/`.

---

## 16. Important Interview Questions

**1. What is Git?**  
Git is a distributed version control system used to track changes and collaborate on code.

**2. What is GitHub?**  
GitHub is a platform for hosting Git repositories and collaborating through pull requests, reviews, and automation.

**3. What is the difference between Git and GitHub?**  
Git is the version-control tool; GitHub hosts Git repositories and provides collaboration features.

**4. What is a commit?**  
A commit is a saved snapshot of staged changes in the local repository.

**5. What is the staging area?**  
It is the area where I select changes that will be included in the next commit.

**6. What is the difference between fetch and pull?**  
Fetch downloads remote updates without integrating them. Pull fetches and integrates updates into the current branch.

**7. What is a branch?**  
A branch is a separate line of development represented by a movable pointer to a commit.

**8. What is a Pull Request?**  
A Pull Request proposes merging changes and allows code review and automated checks.

**9. What is a merge conflict?**  
It occurs when Git cannot automatically combine changes from different branches.

**10. What is the difference between merge and rebase?**  
Merge combines branch histories; rebase replays commits on another base and rewrites those commits.

**11. What is git stash?**  
It temporarily saves working changes so I can switch tasks.

**12. What is git revert?**  
It creates a new commit that reverses an earlier commit.

**13. What is git reset?**  
It moves the current branch pointer, with options controlling the staging area and working directory.

**14. What is cherry-pick?**  
It applies a specific commit from another branch to the current branch.

**15. What is `.gitignore`?**  
It specifies untracked files and patterns that Git should ignore.

**16. How is Git used in DevOps?**  
Git manages source-code history and integrates with CI/CD tools to automate building, testing, and deployment.

---

## 17. Real-World Scenario Questions

### Scenario 1: I changed files but need to switch to another task without committing.

```bash
git stash
```

When I am ready to continue:

```bash
git stash pop
```

### Scenario 2: I committed changes locally but have not pushed them. I want to undo the commit and keep the changes staged.

```bash
git reset --soft HEAD~1
```

### Scenario 3: I pushed incorrect code to a shared branch. How should I undo it?

I would normally use:

```bash
git revert <commit-id>
git push origin main
```

This creates a new commit that reverses the incorrect changes. I would coordinate with the team and test the correction.

### Scenario 4: My feature branch is behind main. What should I do?

I would fetch the latest changes and integrate them according to the team's merge or rebase policy.

```bash
git fetch origin
git switch feature
git merge origin/main
```

I would resolve any conflicts and run the required tests.

### Scenario 5: I want to push a new feature branch to GitHub.

```bash
git push -u origin feature-branch
```

Then I would open a Pull Request.

### Scenario 6: I want to check who changed a particular line.

```bash
git blame filename.txt
```

### Scenario 7: I accidentally committed an API key or password.

- Revoke or rotate the exposed credential immediately.
- Check access logs and possible exposure.
- Remove the secret from the code.
- Clean repository history if required.
- Store the replacement securely, for example in GitHub Secrets or a secret manager.

Deleting the secret from the latest version alone is not enough.

---

## 18. Hands-on Practice Lab

I can use this lab to practice the basic Git workflow on my local machine.

```bash
# Create a project directory
mkdir devops-git-lab
cd devops-git-lab

# Initialize Git
git init

# Create a README file
echo "Learning Git and DevOps" > README.md

# Check repository status
git status

# Stage and commit
git add README.md
git commit -m "Initial commit"

# Create and switch to a feature branch
git switch -c feature-branch

# Modify the file
echo "Learning GitHub" >> README.md

# Stage and commit the changes
git add README.md
git commit -m "Update README"

# Switch back to main
git switch main

# Merge the feature branch
git merge feature-branch

# Check commit history
git log --oneline --graph --all
```

### Push the repository to GitHub

First, I create an empty repository on GitHub. Then I connect and push:

```bash
git remote add origin https://github.com/USERNAME/devops-git-lab.git
git push -u origin main
```

I replace `USERNAME` with my GitHub username. If my local branch is named `master` instead of `main`, I use the actual branch name.

---

## 19. Quick Revision Checklist

- [ ] Git vs GitHub
- [ ] CVCS vs DVCS
- [ ] Working directory, staging area, local and remote repository
- [ ] `git status`, `add`, `commit`, `log`, and `diff`
- [ ] `clone`, `remote`, `push`, `fetch`, and `pull`
- [ ] Create, switch, push, and merge branches
- [ ] Pull Requests and merge-conflict resolution
- [ ] Merge vs rebase
- [ ] Restore vs reset vs revert
- [ ] Stash, cherry-pick, tags, and `.gitignore`
- [ ] GitHub Actions and CI/CD
- [ ] PAT and SSH authentication
- [ ] Complete the hands-on lab without copying commands

---

## Interview Answer I Should Be Able to Explain

"In a typical team workflow, I pull the latest main branch, create a feature branch, make and test my changes, stage and commit them, and push the branch to GitHub. I then raise a Pull Request for review, address feedback and required checks, and merge it according to the team's process. The push or merge can trigger a CI/CD pipeline to build, test, and deploy the application."

---

**My preparation focus:** I should understand the Git workflow and practice the important commands instead of memorizing every Git command. I should be able to explain what each command does, when I would use it, and how I would handle common real-world issues.
