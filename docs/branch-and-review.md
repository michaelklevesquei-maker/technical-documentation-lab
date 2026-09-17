# Branch and Review Workflow with GitHub Desktop for Windows

> **Document status:** Demonstration user guide  
> **Validated environment:** Windows, GitHub Desktop 3.6.6 (x64), GitHub Free personal account  
> **Last validated:** September 17, 2026

## Purpose

This guide explains how to move a controlled change from a synchronized local repository into the default branch by using GitHub Desktop and a GitHub pull request. It covers branch preparation, local editing, change review, commits, publication, pull-request validation, merging, cleanup, and final synchronization.

The workflow is appropriate for documentation, configuration, source code, training material, and other text-based repository content. It emphasizes traceability and review rather than direct changes to the default branch.

## Intended audience

This guide is for technical writers, reviewers, trainers, and other contributors who use GitHub Desktop on Windows and need a repeatable branch-and-review process without relying on command-line Git.

Readers should already know how to install and authenticate GitHub Desktop, configure commit identity, and create or clone a repository. See [Getting Started with GitHub Desktop for Windows](getting-started.md) if the workstation has not been prepared.

## Outcomes

After completing this guide, you will be able to:

- Synchronize the local default branch with its GitHub remote.
- Create a task-specific branch from the correct starting point.
- Review file and line changes before committing them.
- Organize related changes into meaningful commits.
- Publish a branch and push later commits to GitHub.
- Create and validate a pull request.
- Respond to review feedback without opening a replacement pull request.
- Merge approved work and remove completed branches safely.
- Resynchronize the local repository and verify the final state.

## Before you begin

### Requirements

| Requirement | Details |
|---|---|
| Repository access | Write access to the repository, or an approved fork-based contribution workflow |
| Local repository | A repository already added to GitHub Desktop |
| GitHub account | The intended account authenticated in GitHub Desktop and the browser |
| Editor | A plain-text editor appropriate for the repository's file types |
| Working tree | No unexplained or unrelated local changes |
| Network access | HTTPS access to GitHub for fetching, publishing, pushing, and pull requests |
| Project guidance | Applicable contribution instructions, branch rules, templates, and validation requirements |

> **Important:** Before changing branches, account for every uncommitted file. Switching branches with unidentified work can move, hide, overwrite, or mix changes in ways that complicate recovery.

### Security and content precautions

- Do not commit passwords, tokens, private keys, recovery codes, connection strings, or other secrets.
- Review generated files, exports, logs, screenshots, and configuration files for sensitive data.
- Verify repository visibility before publishing material that may contain internal information.
- Do not assume that deleting a secret in a later commit removes it from repository history.
- If a secret is committed, stop publishing, revoke or rotate the credential, and follow the organization's incident and history-remediation procedures.
- Follow repository-specific requirements for branch names, signed commits, issue references, approvals, and status checks.

## Workflow at a glance

| Phase | Primary location | Completion evidence |
|---|---|---|
| Prepare | GitHub Desktop | Default branch is selected, fetched, pulled, and clean |
| Isolate | GitHub Desktop | Task branch exists and is based on the intended branch |
| Author | External editor | Intended files are saved in supported plain-text formats |
| Review | GitHub Desktop | Diff contains only expected changes and no sensitive data |
| Record | GitHub Desktop | One or more focused commits describe the completed work |
| Share | GitHub Desktop and GitHub | Branch is published and all commits are pushed |
| Validate | GitHub pull request | Scope, files, checks, and merge status have been reviewed |
| Integrate | GitHub | Pull request is merged using the approved method |
| Clean up | GitHub and GitHub Desktop | Completed branches are removed and local `main` is current |

## Key terms

| Term | Meaning in this guide |
|---|---|
| Default branch | The repository's primary integration branch, commonly named `main` |
| Base branch | The branch that will receive the proposed changes |
| Topic branch | A short-lived branch used for one defined task or related set of changes |
| Working tree | The local files currently available for editing |
| Diff | A comparison showing added, removed, or modified lines |
| Commit | A recorded set of changes with an identifier, author, timestamp, and message |
| Publish | Create the remote copy of a local branch on GitHub |
| Push | Send local commits to an existing remote branch |
| Pull request | A proposal to review and merge one branch into another |
| Merge | Integrate approved changes from the topic branch into the base branch |

## 1. Synchronize the default branch

Begin each task from a known current state.

1. Open the repository in GitHub Desktop.
2. Confirm that the **Changes** tab reports no unexplained local changes.
3. Select **Current Branch**.
4. Select the repository's default branch, such as `main`.
5. Select **Fetch origin** to check GitHub for commits that are not present locally.
6. If the button changes to **Pull origin**, select it.
7. Confirm that **Current Branch** still displays the intended default branch.
8. Confirm that GitHub Desktop reports **No local changes**.

> **Verification:** The default branch is selected, the latest remote commits are present locally, and the working tree is clean.

<!-- Screenshot placeholder: GitHub Desktop on main with No local changes after Fetch/Pull origin -->

## 2. Create a task-specific branch

Use one branch for one defined purpose. A focused branch produces a clearer diff, commit history, and pull request.

1. Select **Current Branch**.
2. Select **New Branch**.
3. Enter a concise branch name that identifies the work.
4. Under **Create branch based on**, select the synchronized default branch.
5. Select **Create Branch**.
6. Verify that **Current Branch** displays the new branch.

Use a consistent naming convention when the repository defines one. Examples include:

- `docs/branch-and-review`
- `fix/broken-navigation-link`
- `feature/export-report`
- `training/password-awareness-update`

Use lowercase characters and hyphens unless project rules specify another format. Avoid vague names such as `changes`, `update`, or `test`.

> **Important:** Creating a branch does not publish it. The branch remains local until you select **Publish branch** or push a commit to its remote branch.

## 3. Make and save the intended changes

1. Select **Repository** > **Open in Default Editor**, or select **Repository** > **Show in Explorer** and open the required file manually.
2. Confirm that you are editing files inside the intended repository folder.
3. Make only the changes required for the current task.
4. Save each file in the format and encoding expected by the repository.
5. Return to GitHub Desktop.

For Markdown and other plain-text documentation:

- Preserve the repository's heading hierarchy and style conventions.
- Use meaningful link text rather than raw URLs when appropriate.
- Keep filenames stable unless renaming is part of the task.
- Confirm that the editor did not add an unintended `.txt` extension.
- Avoid rich-text formatting or hidden metadata that does not belong in a text repository.

## 4. Review the local diff

GitHub Desktop detects saved changes and lists them under **Changes**.

1. Confirm that the branch shown in **Current Branch** is the task branch.
2. Review the changed-file count.
3. Select each changed file in the left pane.
4. Review every added, removed, and modified line in the diff.
5. Use the diff settings to select **Unified** or **Split** view when one is easier to evaluate.
6. If formatting changes obscure the substantive edits, temporarily select **Hide Whitespace Changes**.
7. To inspect more context, use the arrows beside the line numbers or right-click the diff and select **Expand Whole File**.

Confirm all of the following before committing:

- Every changed file belongs to the current task.
- No required file is missing.
- No unrelated formatting or line-ending rewrite is included.
- No credential, personal data, internal identifier, or confidential content is exposed.
- Links, filenames, commands, menu labels, and examples are accurate.
- The content is complete enough to review as a coherent unit.

> **Stop condition:** If an unexpected file or change appears, determine its source before committing. Do not include it merely because GitHub Desktop selected it automatically.

<!-- Screenshot placeholder: GitHub Desktop Changes tab showing a reviewed documentation diff -->

## 5. Select changes for the commit

The checkboxes in the **Changes** pane control what the next commit will contain.

1. Leave selected only the files that belong in the current commit.
2. Clear the checkbox beside any file that should remain uncommitted.
3. If a file contains unrelated edits, select only the lines that belong in the current commit when GitHub Desktop permits a partial commit.
4. Re-read the resulting selected diff as a single unit of work.

Create separate commits when changes have different purposes or would be reviewed, reverted, or reused independently. Do not split a single logical correction into artificial fragments merely to increase the commit count.

### Discarding unwanted changes

Discarding changes alters the files on the computer. Use it only after confirming that the edits are not needed.

1. Select the unwanted file or lines.
2. Right-click the selection.
3. Select the applicable **Discard** command.
4. Review the confirmation prompt carefully.
5. Select **Discard Changes** only when the target is correct.

GitHub Desktop sends discarded file changes to a dated item in the Windows Recycle Bin, where they may remain recoverable until the bin is emptied. Recovery should not replace deliberate review.

## 6. Write a meaningful commit message

A commit message should help another person understand what changed without opening every file.

Use the **Summary** field for a short, action-oriented description. Use the optional **Description** field to explain scope, rationale, validation, or constraints that are not obvious from the diff.

Effective summaries:

- `Add branch-and-review workflow guide`
- `Correct authentication menu path`
- `Clarify repository publication warning`

Weak summaries:

- `Changes`
- `Update file`
- `Stuff`
- `Final version`

Prefer a summary that completes this sentence: "If applied, this commit will..." For example, "If applied, this commit will **add branch-and-review workflow guidance**."

Before committing, verify that the displayed author name and email belong to the intended GitHub identity.

## 7. Commit and inspect the result

1. Enter the commit **Summary**.
2. Add a **Description** when it improves traceability.
3. Confirm that only the intended files and lines remain selected.
4. Select **Commit to BRANCH**, where `BRANCH` is the current topic branch.
5. Select the **History** tab.
6. Confirm that the new commit appears on the topic branch with the intended author and message.
7. Return to **Changes**.

If the task is incomplete, continue editing and create another focused commit. If the task is complete, GitHub Desktop should report **No local changes**.

> **Verification:** The task branch contains the intended commit, and no uncommitted work has been overlooked.

## 8. Publish the branch and push later commits

Publishing creates the remote branch on GitHub. Pushing sends additional local commits to a remote branch that already exists.

### First publication

1. Confirm that the current branch is the topic branch.
2. Select **Publish branch**.
3. Wait for publication to complete.
4. Confirm that GitHub Desktop offers **Preview Pull Request** or another remote-aware action.

### Later commits

1. Commit the additional local changes.
2. If GitHub Desktop reports that the remote branch contains newer commits, select **Fetch origin** and synchronize before pushing.
3. Select **Push origin**.
4. Confirm that the push completes without an error.

> **Important:** A local commit is not available to reviewers until it has been published or pushed to GitHub.

## 9. Create the pull request

1. Select **Preview Pull Request** in GitHub Desktop.
2. Confirm that the **base** branch is the intended destination, commonly `main`.
3. Review the previewed branch comparison.
4. Select **Create Pull Request** to open GitHub in the default browser.
5. Confirm the comparison again: the base branch should receive changes from the topic branch.
6. Enter a concise title describing the outcome of the change.
7. Enter a structured description that states what changed and how it was validated.
8. Select **Create pull request**.

Use a draft pull request when the work is intentionally incomplete but needs early visibility or discussion. A draft cannot be merged until it is marked ready for review.

### Recommended pull-request description

```markdown
## Summary

- Describe the primary change.
- Identify important supporting changes.
- State relevant scope or exclusions.

## Validation

- List the review or test performed.
- Record the environment when it matters.
- Confirm that only intended files are included.
```

<!-- Screenshot placeholder: GitHub Open a pull request page with base and compare branches verified -->

## 10. Validate the pull request

Treat the pull request as the review record, even when the author and merger are the same person.

1. Review the **Conversation** tab for the title, description, comments, approvals, and merge status.
2. Review the **Commits** tab for unexpected commits or unclear history.
3. Review the **Checks** tab for automated validation results, when checks are configured.
4. Review the **Files changed** tab line by line.
5. Confirm that the file count and addition/deletion totals are plausible.
6. Open rendered documentation or another available preview when formatting matters.
7. Confirm that the pull request reports no conflicts with the base branch.
8. Confirm that required approvals, checks, conversations, and project policies are satisfied.

Do not treat **Able to merge** as proof that the content is correct. It means GitHub can combine the branches automatically; human and automated validation still determine whether the change should be accepted.

### Respond to review feedback

Do not create a replacement pull request for ordinary revisions.

1. Keep the pull request open.
2. In GitHub Desktop, return to the same topic branch.
3. Make and review the requested changes.
4. Commit the revision with a message that describes the correction.
5. Select **Push origin**.
6. Return to the existing pull request and confirm that it now includes the new commit.
7. Reply to or resolve review conversations according to project policy.

## 11. Merge the approved pull request

Merge only after the review is complete and the pull request is ready to become part of the base branch.

1. Reconfirm the base and topic branches shown in the pull-request header.
2. Confirm that the merge box shows no unresolved conflicts or required checks.
3. Select the merge method approved for the repository.
4. Review the resulting commit message when GitHub provides an editable confirmation.
5. Select the final confirmation button.
6. Confirm that GitHub marks the pull request **Merged** and **Closed**.

Common merge methods include:

| Method | Result | Typical consideration |
|---|---|---|
| Merge pull request | Preserves the branch's commits and adds a merge commit | Retains the branch history and merge event |
| Squash and merge | Combines the pull request into one commit on the base branch | Produces a compact base-branch history |
| Rebase and merge | Replays individual commits onto the base branch without a merge commit | Produces a linear history but changes commit identifiers |

Use the repository's established policy. Do not change merge strategy solely for personal preference.

## 12. Delete the completed remote branch

After the pull request is merged:

1. Confirm that GitHub reports the pull request as merged.
2. Select **Delete branch** near the bottom of the pull-request timeline, if the repository does not delete merged branches automatically.
3. Confirm that the topic branch is shown as deleted.

Deleting the remote topic branch does not remove the merged content from the base branch. It removes the completed branch reference so that active branch lists remain meaningful.

> **Warning:** Do not delete an unmerged branch unless the work is intentionally abandoned or preserved elsewhere. Branch deletion may be reversible in some GitHub workflows, but recovery should not be the operating plan.

## 13. Resynchronize GitHub Desktop

The browser merge updates GitHub, not the local `main` branch.

1. Return to GitHub Desktop.
2. Select **Current Branch**.
3. Select `main` or the repository's applicable default branch.
4. Select **Fetch origin**.
5. If prompted, select **Pull origin**.
6. Confirm that GitHub Desktop reports **No local changes**.
7. Select **Repository** > **Show in Explorer**.
8. Confirm that the merged files and changes are present locally.

### Remove the completed local branch

If the local topic branch remains after its remote branch is deleted:

1. Confirm that the pull request was merged successfully.
2. Confirm that the current branch is `main`, not the branch being removed.
3. Select **Current Branch**, then select the completed local branch.
4. Select **Branch** > **Delete**.
5. Review the branch name in the confirmation window.
6. Confirm deletion.
7. Return to `main` if GitHub Desktop does not do so automatically.

## 14. Perform final verification

The workflow is complete when all of the following are true:

- The pull request is merged and closed.
- The merged files render or function as intended on GitHub.
- The remote topic branch is deleted when project policy calls for cleanup.
- GitHub Desktop is on the default branch.
- The default branch has been fetched and pulled after the merge.
- The local repository contains the merged changes.
- GitHub Desktop reports no unexplained local changes.
- The completed local branch has been removed when it is no longer needed.

## Troubleshooting

### Changes were made on the wrong branch

Do not commit until the target branch is corrected.

1. Select **Current Branch** and choose the intended branch.
2. When GitHub Desktop asks how to handle uncommitted changes, choose to bring the changes to the intended branch only after confirming the source and destination names.
3. Review the diff again before committing.

If the changes were already committed, avoid improvising a history rewrite. Preserve the commit identifier and follow the project's approved recovery method, such as reverting, cherry-picking, or opening a corrective pull request.

### GitHub Desktop will not switch branches

1. Review the **Changes** tab for uncommitted work.
2. Commit the changes to the current branch, move them to the intended branch when prompted, or stash them temporarily.
3. Retry the branch switch.

Do not discard changes merely to clear the warning unless the work is confirmed unnecessary.

### The diff contains an unexpected full-file rewrite

1. Check whether the editor changed line endings, encoding, indentation, or automatic formatting.
2. Use **Hide Whitespace Changes** only to diagnose the difference, not to conceal it from the commit.
3. Restore the expected settings and save the file again.
4. Confirm that the diff now contains only intentional changes.

### A push is rejected

1. Read the complete GitHub Desktop message.
2. Select **Fetch origin** when the remote branch contains commits that are not present locally.
3. Resolve any divergence or conflicts according to project policy.
4. Check branch rules, naming conventions, commit-message requirements, file-size limits, and permissions.
5. Push again only after the local branch is valid and current.

GitHub rejects ordinary Git pushes containing an individual file larger than 100 MiB or a total push larger than 2 GiB. Projects that require large files should use an approved Git Large File Storage workflow.

### Preview Pull Request is unavailable

1. Confirm that the current branch is not the base branch.
2. Confirm that the branch contains at least one commit not present in the base branch.
3. Publish the branch or push its commits to GitHub.
4. Retry **Preview Pull Request**.

### GitHub reports merge conflicts

Do not merge until the conflict is understood and resolved.

1. Identify which files conflict with the base branch.
2. Update the topic branch using the repository's approved merge or rebase procedure.
3. Resolve each conflict in an appropriate editor.
4. Review the resolved file in full, not only the conflict markers.
5. Commit and push the resolution.
6. Recheck the pull request and its automated checks.

### The merge button is unavailable

Review the merge box for the blocking condition. Common causes include:

- The pull request is still a draft.
- Required reviews are missing.
- Required checks are pending or failing.
- Review conversations remain unresolved.
- The branch is behind the base branch.
- A branch rule or ruleset blocks the proposed action.
- The pull request has merge conflicts.
- The user lacks permission to merge.

Resolve the named condition rather than attempting to bypass repository policy.

### The merged file is missing locally

1. Confirm on GitHub that the pull request was merged into the intended base branch.
2. In GitHub Desktop, switch to that base branch.
3. Select **Fetch origin**.
4. Select **Pull origin** when available.
5. Open the repository folder and check the file again.

### A secret was committed

Treat the credential as compromised even if the repository is private.

1. Stop sharing or merging the branch.
2. Revoke or rotate the exposed credential immediately.
3. Notify the appropriate security or repository owner.
4. Follow the approved process for removing sensitive data from repository history.
5. Review logs and access records when required by incident-response policy.

Deleting the text in a later commit is not sufficient because the original value remains in Git history.

## Next step

After mastering this workflow, continue with conflict handling and controlled document revision. That guide should cover concurrent edits, merge-conflict diagnosis, safe resolution, review-comment incorporation, corrective commits, reversions, and recovery from common authoring mistakes.

## References

- [Managing branches in GitHub Desktop](https://docs.github.com/en/desktop/making-changes-in-a-branch/managing-branches-in-github-desktop)
- [Committing and reviewing changes in GitHub Desktop](https://docs.github.com/en/desktop/making-changes-in-a-branch/committing-and-reviewing-changes-to-your-project-in-github-desktop)
- [Pushing changes to GitHub from GitHub Desktop](https://docs.github.com/en/desktop/making-changes-in-a-branch/pushing-changes-to-github-from-github-desktop)
- [Creating an issue or pull request from GitHub Desktop](https://docs.github.com/en/desktop/working-with-your-remote-repository-on-github-or-github-enterprise/creating-an-issue-or-pull-request-from-github-desktop)
- [Syncing your branch in GitHub Desktop](https://docs.github.com/en/desktop/working-with-your-remote-repository-on-github-or-github-enterprise/syncing-your-branch-in-github-desktop)
- [Pull requests](https://docs.github.com/en/pull-requests/reference/pull-requests)
- [Deleting and restoring branches in a pull request](https://docs.github.com/en/pull-requests/managing-pull-request-branches-and-merges/managing-pull-request-branches/deleting-and-restoring-branches-in-a-pull-request)
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
