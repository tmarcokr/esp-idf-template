---
description: Add or update the ESP-IDF template (.agents and .vscode) from the upstream repository.
---

# 🚀 Workflow: Template Sync / Update

Use this workflow to add or update the `esp-idf-template` inside another project.

## Important Constraints
- **Do not use `git checkout`** for bringing files from the template (e.g., `git checkout template/main -- .agents`), as it will forcefully overwrite local changes without warning.
- **Use `git merge`** to safely bring in changes from the template. This ensures Git uses its merge engine to combine changes and alert about conflicts, preserving local customizations.
- Focus updates specifically on the `.agents/` and `.vscode/` directories.

## Process Steps

1. **Information Gathering**:
   - Check if the `template` remote already exists using `git remote -v`.
   - If not, add the template repository as a remote:
     ```bash
     git remote add template https://github.com/tmarcokr/esp-idf-template.git
     ```

2. **Fetching Updates**:
   - Fetch the latest changes from the template repository:
     ```bash
     git fetch template
     ```

3. **Merging Changes (Safe approach for specific folders)**:
   - To merge changes safely while only affecting `.agents/` and `.vscode/`, execute a merge without committing automatically:
     ```bash
     git merge template/main --no-commit --no-ff --allow-unrelated-histories
     ```
   - *Note:* If the merge process results in conflicts inside `.agents/` or `.vscode/`, they must be resolved manually or using standard Git conflict resolution tools.

4. **Filtering Specific Folders**:
   - Because a full merge brings in all changes (like `main/`, `CMakeLists.txt`, etc.), unstage everything except the desired folders:
     ```bash
     # Unstage everything
     git reset HEAD
     
     # Add only the target directories back to the staging area
     git add .agents/ .vscode/
     
     # Discard the rest of the changes brought by the merge to keep the local workspace clean
     # BE CAREFUL: Ensure we only restore files that are not in the target directories.
     git restore .
     ```
   - *Alternative if the previous step is too destructive for uncommitted local work:* Ensure the workspace is entirely clean before starting the workflow.

5. **Finalizing**:
   - Review the staged changes using `git status` and `git diff --staged`.
   - If everything looks correct, commit the merge:
     ```bash
     git commit -m "chore: sync .agents and .vscode from esp-idf-template"
     ```
