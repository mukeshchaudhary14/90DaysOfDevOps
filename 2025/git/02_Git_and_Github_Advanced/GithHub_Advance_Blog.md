

---

## **Task 1: Working with Pull Requests (PRs)**

### Steps to Create a PR:
1. **Fork the repository** on GitHub.
2. **Clone your forked repository** locally:
   ```bash
   git clone <your-forked-repo-url>
   cd <repo-name>
   ```
3. **Create a feature branch** and make changes:
   ```bash
   git checkout -b feature-branch
   echo "New Feature" >> feature.txt
   git add .
   git commit -m "Added a new feature"
   ```
4. **Push the changes** to your forked repository:
   ```bash
   git push origin feature-branch
   ```
5. **Open a Pull Request** on GitHub:
   - Go to your forked repository on GitHub.
   - Click on "Compare & Pull Request."
   - Add a title and description for the PR.
   - Request a review from a teammate.
   - Once approved, merge the PR.

### Best Practices for Writing PR Descriptions:
- Clearly explain the purpose of the PR.
- Mention the changes made and why they are necessary.
- Include screenshots or logs if applicable.
- Reference related issues or tickets (e.g., "Fixes #123").

### Handling Review Comments:
- Address each comment by making the necessary changes.
- Push the updated changes to the same branch.
- Reply to the comments to confirm the changes.

---

## **Task 2: Undoing Changes – Reset & Revert**

### Differences Between Reset and Revert:
- **Reset**: Moves the branch pointer to a specific commit, altering the commit history.
  - `--soft`: Keeps changes staged.
  - `--mixed`: Unstages changes but keeps files.
  - `--hard`: Discards all changes.
- **Revert**: Creates a new commit that undoes the changes of a previous commit, preserving history.

### When to Use Each Method:
- Use **reset** when you want to rewrite history (e.g., for local changes).
- Use **revert** when you want to safely undo changes in a shared branch.

---

## **Task 3: Stashing - Save Work Without Committing**

### When to Use `git stash`:
- Use `git stash` when you need to switch branches but don’t want to commit incomplete work.

### Difference Between `git stash pop` and `git stash apply`:
- `git stash pop`: Applies the stashed changes and removes them from the stash list.
- `git stash apply`: Applies the stashed changes but keeps them in the stash list.

---

## **Task 4: Cherry-Picking - Selectively Apply Commits**

### How Cherry-Picking is Used in Bug Fixes:
- Cherry-picking allows you to apply a specific commit from one branch to another, which is useful for applying bug fixes to multiple branches.

### Risks of Cherry-Picking:
- Cherry-picking can lead to duplicate commits and conflicts if not used carefully.

---

## **Task 5: Rebasing - Keeping a Clean Commit History**

### Difference Between Merge and Rebase:
- **Merge**: Combines changes from two branches and creates a merge commit.
- **Rebase**: Moves the entire branch to the tip of another branch, creating a linear history.

### Best Practices for Rebasing:
- Use rebase for feature branches to keep the commit history clean.
- Avoid rebasing shared branches to prevent conflicts for others.

---

## **Task 6: Branching Strategies Used in Companies**

### Git Workflows:
1. **Git Flow**:
   - Uses `feature`, `release`, and `hotfix` branches.
   - Suitable for projects with scheduled releases.
2. **GitHub Flow**:
   - Uses `main` and `feature` branches.
   - Focuses on continuous delivery.
3. **Trunk-Based Development**:
   - Uses a single `main` branch with short-lived feature branches.
   - Emphasizes continuous integration.

### Which Strategy is Best for DevOps and CI/CD:
- **Trunk-Based Development** is ideal for DevOps and CI/CD as it promotes frequent integration and faster delivery.

---

## **solution.md**

```markdown
# Git & GitHub Advanced Challenge - Solution

## Task 1: Working with Pull Requests (PRs)
### Steps to Create a PR:
1. Fork the repository on GitHub.
2. Clone your forked repository locally.
3. Create a feature branch and make changes.
4. Push the changes to your forked repository.
5. Open a Pull Request on GitHub.

### Best Practices for Writing PR Descriptions:
- Clearly explain the purpose of the PR.
- Mention the changes made and why they are necessary.
- Include screenshots or logs if applicable.
- Reference related issues or tickets.

### Handling Review Comments:
- Address each comment by making the necessary changes.
- Push the updated changes to the same branch.
- Reply to the comments to confirm the changes.

---

## Task 2: Undoing Changes – Reset & Revert
### Differences Between Reset and Revert:
- **Reset**: Moves the branch pointer to a specific commit, altering the commit history.
- **Revert**: Creates a new commit that undoes the changes of a previous commit, preserving history.

### When to Use Each Method:
- Use **reset** for local changes.
- Use **revert** for shared branches.

---

## Task 3: Stashing - Save Work Without Committing
### When to Use `git stash`:
- Use `git stash` when you need to switch branches but don’t want to commit incomplete work.

### Difference Between `git stash pop` and `git stash apply`:
- `git stash pop`: Applies and removes stashed changes.
- `git stash apply`: Applies stashed changes but keeps them in the stash list.

---

## Task 4: Cherry-Picking - Selectively Apply Commits
### How Cherry-Picking is Used in Bug Fixes:
- Cherry-picking allows you to apply a specific commit from one branch to another.

### Risks of Cherry-Picking:
- Can lead to duplicate commits and conflicts.

---

## Task 5: Rebasing - Keeping a Clean Commit History
### Difference Between Merge and Rebase:
- **Merge**: Combines changes and creates a merge commit.
- **Rebase**: Moves the entire branch to the tip of another branch.

### Best Practices for Rebasing:
- Use rebase for feature branches.
- Avoid rebasing shared branches.

---

## Task 6: Branching Strategies Used in Companies
### Git Workflows:
1. **Git Flow**: Uses `feature`, `release`, and `hotfix` branches.
2. **GitHub Flow**: Uses `main` and `feature` branches.
3. **Trunk-Based Development**: Uses a single `main` branch with short-lived feature branches.

### Which Strategy is Best for DevOps and CI/CD:
- **Trunk-Based Development** is ideal for DevOps and CI/CD.
```

---

## **How to Submit**
1. Push your work to GitHub:
   ```bash
   git add .
   git commit -m "Completed Git & GitHub Advanced Challenge"
   git push origin main
   ```
2. Create a Pull Request:
   - Title: `Git & GitHub Advanced Challenge - Completed`
   - PR Description: Include the steps followed for each task and any screenshots or logs.

---

