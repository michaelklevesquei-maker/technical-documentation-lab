# Conflict Resolution and Document Recovery with GitHub Desktop for Windows

> **Document status:** Demonstration user guide  
> **Validated environment:** Windows, GitHub Desktop 3.6.6 (x64), GitHub Free personal account  
> **Last validated:** September 17, 2026

## Purpose

This guide explains how to protect, recover, and reconcile documentation work in a Git repository by using GitHub Desktop, a plain-text editor, and GitHub. It covers uncommitted changes, incorrect local commits, work committed to the wrong branch, published mistakes, merge conflicts, discarded files, and sensitive-data incidents.

The goal is not merely to make Git accept a change. The goal is to preserve the correct content, maintain a trustworthy history, and leave the local and remote repositories in a state that another contributor can understand.

## Intended audience

This guide is for technical writers, reviewers, trainers, and other contributors who use GitHub Desktop on Windows and need a repeatable recovery process without depending on command-line Git.

Readers should already know how to create branches, review diffs, commit changes, publish branches, open pull requests, and synchronize a repository. See [Getting Started with GitHub Desktop for Windows](getting-started.md) and [Branch and Review Workflow with GitHub Desktop for Windows](branch-and-review.md) for those procedures.

## Outcomes

After completing this guide, you will be able to:

- Classify a recovery incident before changing repository state.
- Preserve uncommitted work by moving it or placing it in GitHub Desktop's stash.
- Discard unwanted local changes with appropriate precautions.
- Undo, amend, or reset unpushed commits.
- Move a commit to the correct branch by cherry-picking it.
- Reverse a published commit without rewriting shared history.
- Resolve simple text conflicts locally or in a GitHub pull request.
- Recognize conflicts that require a subject-matter expert or repository administrator.
- Validate the repository after recovery.
- Respond safely when a committed file contains a credential or other sensitive data.

## Before you begin

### Requirements

| Requirement | Details |
|---|---|
| Repository access | Write access to the repository or an approved contribution workflow |
| Local repository | A repository already added to GitHub Desktop |
| GitHub account | The intended account authenticated in GitHub Desktop and the browser |
| Editor | A plain-text editor configured as the default editor in GitHub Desktop |
| Recovery context | The affected branch, files, approximate time, and intended final result |
| Network access | HTTPS access to GitHub for fetching, pushing, and pull-request work |
| Project guidance | Applicable contribution, retention, security, and incident-response policies |

### Recovery principles

Use these principles in the order shown:

1. **Stop.** Do not keep editing, switching branches, pulling, or pushing while the state is unclear.
2. **Identify.** Record the current repository, branch, changed files, and latest visible commit.
3. **Preserve.** Protect content that may still be needed before using a discard, reset, or delete command.
4. **Choose.** Select the least disruptive operation that produces the required result.
5. **Validate.** Review the content, diff, history, branch, and remote state after the operation.
6. **Document.** Use clear commit and pull-request messages when the recovery changes shared history.

> **Important:** A clean **Changes** tab does not prove that the repository is correct. The work may already be committed, may exist on another branch, or may have been removed from the working tree.

### Stop conditions

Stop and obtain help from the repository owner, content owner, or security contact when:

- You cannot explain which version of the content is authoritative.
- The operation would require a force push to a shared or protected branch.
- The conflict involves generated, binary, encrypted, or regulated files.
- A commit may contain a password, token, private key, recovery code, or other secret.
- Multiple contributors are actively changing the same branches during recovery.
- A branch, tag, release, automation, or deployment depends on the commits being changed.
- Repository rules prohibit the recovery operation you intended to use.

## Recovery decision matrix

| Situation | Preferred action | History effect | Primary risk |
|---|---|---|---|
| Uncommitted work is needed later | Stash changes | No commit created | GitHub Desktop keeps only one stash set at a time |
| Uncommitted work belongs on another branch | Switch and bring changes, or stash then restore | No commit created | Changes may conflict with files on the target branch |
| Uncommitted work is unwanted | Discard selected changes | Working files are changed | Needed content may be lost |
| Latest local commit should become editable again | Undo commit | Removes the unpushed commit; restores its changes | Available only for the latest unpushed commit |
| Latest local commit needs a small correction | Amend commit | Replaces the latest commit | Amending a pushed commit requires force-pushing |
| Several unpushed commits need to be reorganized | Reset to an earlier commit | Removes later local commits; restores their changes | Selecting the wrong target changes more history than intended |
| A commit was made on the wrong branch | Cherry-pick to the correct branch | Creates a new commit on the target branch | The source branch still needs deliberate cleanup |
| A pushed or shared commit is wrong | Revert the commit | Adds a new commit that reverses the selected commit | Later changes may depend on the reverted content |
| A pull request conflicts with its base branch | Merge the base into the topic branch and resolve | Adds a merge resolution to the topic branch | Incorrect content can appear valid after markers are removed |
| A simple line conflict appears in a pull request | Use GitHub's conflict editor when permitted | Merges the base branch into the head branch | The web editor is unsuitable for complex conflicts |
| A secret was committed | Revoke or rotate it, then escalate | Incident-specific | Deleting the text later does not remove it from history |

> **Rule of thumb:** Use **Undo**, **Amend**, and **Reset** only for local, unpushed work. Use **Revert** for a commit that has already been pushed or shared.

## Key terms

| Term | Meaning in this guide |
|---|---|
| Working tree | The local files currently available for editing |
| Uncommitted change | A saved file change that has not been recorded in a commit |
| Stash | One temporary set of uncommitted changes stored by GitHub Desktop |
| Local commit | A commit stored in the local repository, whether or not it has been pushed |
| Remote commit | A commit available from the repository hosted on GitHub |
| HEAD | Git's reference to the currently checked-out commit or branch position |
| Undo | Return the latest unpushed commit's changes to the working tree |
| Amend | Replace the latest commit with an updated version |
| Reset | Move the current branch to an earlier local commit and restore later changes to the working tree |
| Cherry-pick | Copy one commit's changes onto another branch as a new commit |
| Revert | Create a new commit that reverses the changes introduced by an earlier commit |
| Merge conflict | A difference Git cannot combine automatically without a decision |
| Conflict marker | Text such as `<<<<<<<`, `=======`, and `>>>>>>>` that separates competing versions |
| Base branch | The branch intended to receive a pull request, commonly `main` |
| Head branch | The branch that contains the changes proposed by a pull request |
| Detached HEAD | A state in which a commit, rather than a branch, is checked out |

## 1. Pause and record the current state

Do not begin recovery from memory alone.

1. Stop editing affected files.
2. In GitHub Desktop, confirm the **Current Repository**.
3. Record the name shown under **Current Branch**.
4. Select the **Changes** tab and record the changed-file count.
5. Review every file listed under **Changes**.
6. Select the **History** tab and record the latest relevant commit summary and short identifier.
7. Note whether the repository bar shows **Fetch origin**, **Pull origin**, **Push origin**, **Publish branch**, or another action.
8. If a pull request exists, open it and record its base branch, compare branch, merge status, and checks.

When organizational policy permits, capture a screenshot of this state before a high-risk recovery. Do not include exposed credentials or regulated information in the screenshot.

> **Verification:** You can state which repository and branch are active, whether the affected work is uncommitted or committed, and whether it has been pushed.

<!-- Screenshot placeholder: GitHub Desktop showing the active branch, Changes list, and repository action -->

## 2. Protect work before changing it

If content may still be needed, preserve it before using a destructive operation.

Choose one approved method:

- Place the uncommitted changes in GitHub Desktop's stash.
- Commit the work to a temporary recovery branch when a durable Git record is appropriate.
- Copy only the affected text into an approved secure temporary location outside the repository.
- Ask the repository owner to preserve the branch or commit before coordinated recovery begins.

Do not copy secrets into notes, email, chat, screenshots, or an unprotected temporary file. A suspected secret requires the incident procedure in [Respond to exposed sensitive data](#14-respond-to-exposed-sensitive-data).

### Create a temporary recovery branch

Use a recovery branch when the current content is coherent enough to commit and policy permits retaining it.

1. In GitHub Desktop, select **Current Branch**.
2. Select **New Branch**.
3. Enter a descriptive name such as `recovery/draft-before-reset`.
4. Confirm the branch is based on the current branch.
5. Select **Create Branch**.
6. Review the diff again.
7. Commit the preserved work with a message that explains why it was captured.
8. Publish the recovery branch only if remote preservation or collaboration is required and the content is safe to share.

> **Important:** A temporary branch preserves repository content but is not a substitute for security, retention, or backup policy.

## 3. Stash uncommitted changes

Stashing removes uncommitted changes from the working tree without creating a commit. It is useful when you must synchronize or inspect another branch before the work is ready to commit.

### Stash all current changes

1. Select the **Changes** tab.
2. Right-click the changed-files header.
3. Select **Stash All Changes**.
4. Confirm that the changed files disappear from the normal **Changes** list.
5. Confirm that a **Stashed Changes** entry is available.

> **Limitation:** GitHub Desktop supports one stash set at a time. Restore, commit, or discard the existing stash before attempting to create another.

### Restore stashed changes

1. Select the branch on which the work belongs.
2. Confirm that the branch is the intended destination.
3. Open **Stashed Changes** in the **Changes** pane.
4. Review the files represented by the stash.
5. Select **Restore**.
6. Review every restored change in the diff.
7. Resolve any conflicts before committing.

### Discard a stash

Discard a stash only when you have confirmed that no content in it is needed.

1. Open **Stashed Changes**.
2. Review the listed work.
3. Select **Discard**.
4. Read the confirmation prompt.
5. Confirm only when the target is correct.

> **Stop condition:** If GitHub Desktop reports a conflict while restoring the stash, do not discard either version. Resolve the content as described in [Resolve a text merge conflict locally](#11-resolve-a-text-merge-conflict-locally).

## 4. Move uncommitted work to the correct branch

GitHub Desktop may offer to leave changes on the current branch, bring them to the selected branch, or stash them when you switch branches.

### Move changes during the branch switch

Use this method when the target branch is known and the changed files do not conflict with it.

1. Review all current changes.
2. Select **Current Branch**.
3. Select the branch where the work belongs.
4. When prompted, choose the option to bring the changes to the selected branch.
5. Confirm that **Current Branch** displays the target branch.
6. Review the entire diff again.
7. Commit only after confirming that the changes still have the intended meaning on the target branch.

### Stash before switching

Use this method when you need to inspect or synchronize the target branch first.

1. Stash the changes.
2. Switch to the target branch.
3. Select **Fetch origin**.
4. If offered, select **Pull origin**.
5. Restore the stash.
6. Resolve any conflicts.
7. Review and commit the recovered work.

> **Verification:** The intended branch is active, all needed changes are present, and no unrelated changes were carried into the branch.

## 5. Discard unwanted uncommitted changes

Discarding changes edits the files on the computer. Use it only after classifying the work and preserving anything that may be needed.

### Discard selected lines

Use line-level discard when only part of a text file is unwanted.

1. Select the file in the **Changes** pane.
2. Select only the unwanted lines when line selection is available.
3. Right-click the selection.
4. Select **Discard Selected Lines**.
5. Review the confirmation prompt.
6. Confirm the operation.
7. Review the remaining diff from beginning to end.

### Discard one file

1. Right-click the unwanted file in the **Changes** pane.
2. Select **Discard Changes**.
3. Confirm that the displayed filename is correct.
4. Confirm the operation.

### Discard all changes

Use a repository-wide discard only when every listed change is unwanted.

1. Review the complete changed-file list.
2. Right-click the changed-files header.
3. Select **Discard All Changes**.
4. Read the confirmation prompt carefully.
5. Confirm only when every target is correct.

GitHub Desktop normally sends discarded changes to a dated item in the Windows Recycle Bin. Treat this as a possible recovery aid, not a guaranteed backup. The item may be unavailable if the Recycle Bin was emptied, storage policy removed it, or the change type could not be preserved in that form.

## 6. Undo the latest unpushed commit

Use **Undo** when the latest commit has not been pushed and you want its changes to become editable again.

1. Confirm that the affected commit is the latest commit on the current branch.
2. Confirm that it has not been pushed.
3. Select the **Changes** tab.
4. At the bottom of the pane, select **Undo**.
5. Confirm that the commit's files reappear as uncommitted changes.
6. Review the complete diff.
7. Edit, move, divide, or recommit the changes as required.

Undo does not mean discard. It removes the latest local commit while keeping its file changes in the working tree.

> **If Undo is unavailable:** The commit may already be pushed, may not be the latest commit, or the current branch may not contain it. Use **Revert** for a pushed commit or consider **Reset** for several unpushed commits.

## 7. Amend the latest unpushed commit

Use **Amend commit** when the latest local commit is almost correct but needs a small content or message change.

1. Confirm that the commit is the latest commit on the branch.
2. Confirm that the commit has not been pushed or shared.
3. Make and save the required file changes.
4. In GitHub Desktop, review every changed line.
5. Select the **History** tab.
6. Right-click the latest commit.
7. Select **Amend commit**.
8. Update the summary or description if necessary.
9. Confirm the amended commit.
10. Review the replacement commit in **History**.

Amending replaces the commit and gives it a new identifier.

> **Warning:** Amending a pushed commit requires a force push. Do not force-push a shared or protected branch unless the repository owner explicitly authorizes and coordinates the history rewrite.

## 8. Reset several unpushed commits

Use **Reset to commit** when several local, unpushed commits need to be recombined, moved, or edited.

1. Confirm that none of the commits to be removed has been pushed or shared.
2. Select the **History** tab.
3. Identify the last commit that should remain recorded.
4. Review its summary, date, author, and identifier.
5. Right-click that commit.
6. Select **Reset to commit**.
7. Confirm the operation.
8. Select **Changes**.
9. Confirm that the changes from the later commits are present as uncommitted work.
10. Review, edit, and recommit the work in the desired organization.

The selected commit remains in history. Commits after it are removed from the current local branch, and their changes return to the working tree.

> **Stop condition:** Do not select a reset target based only on its date or position. Confirm the commit identifier and intended boundary first.

## 9. Move a commit made on the wrong branch

Use cherry-pick when a completed commit belongs on another branch.

### Copy the commit to the correct branch

1. Record the commit summary and identifier.
2. Synchronize the intended target branch if doing so will not disturb local work.
3. Select the branch that currently contains the commit.
4. Select **History**.
5. Right-click the commit.
6. Select **Cherry pick commit**.
7. Select the correct target branch.
8. Confirm the operation.
9. Switch to the target branch.
10. Review the new commit and its diff.
11. Validate the affected content in its new branch context.

Cherry-picking creates a new commit on the target branch. The original commit remains on the source branch.

### Clean up the source branch

Choose the cleanup method according to publication state:

- If the original commit is the latest unpushed commit, switch to the source branch and use **Undo** or reset to the previous commit.
- If the source branch was pushed or shared, use **Revert** or ask the repository owner for the approved correction method.
- If the source branch is temporary and contains no other needed work, delete it only after verifying that the correct branch contains the copied commit.

> **Verification:** The target branch contains the intended commit, the source branch has been handled deliberately, and no duplicate change will be merged twice.

## 10. Revert a pushed or shared commit

Use **Revert Changes in Commit** to reverse a published change without erasing the original commit from shared history.

1. Select the branch that contains the commit.
2. Select **Fetch origin** and pull any remote changes when appropriate.
3. Select **History**.
4. Locate and inspect the commit to reverse.
5. Right-click the commit.
6. Select **Revert Changes in Commit**.
7. Review the newly created revert commit.
8. Review the affected files and validate the resulting content.
9. Push the revert commit or propose it through the repository's branch-and-review workflow.
10. In the pull request or commit description, explain why the change was reverted and what follow-up is required.

The original commit remains in history. The revert adds a new commit containing an inverse change.

When reverting several related commits, revert them from newest to oldest unless the repository owner directs otherwise. Later commits may depend on earlier changes, and reversing them in the opposite order can create avoidable conflicts.

> **Important:** A successful revert means that Git applied an inverse patch. It does not prove that the resulting document is accurate, complete, or internally consistent.

## 11. Resolve a text merge conflict locally

A merge conflict occurs when Git cannot safely determine the final content. Common examples include competing edits to the same lines and a file edited on one branch but deleted on another.

### Prepare the topic branch

1. Stop unrelated editing.
2. Confirm that the current branch is the pull request's topic branch, not `main`.
3. Confirm that uncommitted work is either committed, stashed, or otherwise protected.
4. Select **Fetch origin**.
5. Pull updates for the topic branch if GitHub Desktop offers **Pull origin**.
6. Select **Current Branch**.
7. Select **Choose a branch to merge into BRANCH**.
8. Select the pull request's base branch, commonly `main`.
9. Select **Merge BASE into BRANCH**.

If GitHub Desktop detects conflicts, it identifies the affected files and prevents completion until they are resolved.

### Interpret conflict markers

A text conflict commonly appears in this form:

```text
<<<<<<< HEAD
Text from the currently checked-out branch.
=======
Text from the branch being merged.
>>>>>>> main
```

The labels can vary. Do not assume that the upper block is correct merely because it appears first. The required result may use the upper block, the lower block, a combination of both, or newly written content.

### Resolve each conflicting file

1. From GitHub Desktop, open the conflicting file in the configured editor.
2. Locate the first `<<<<<<<` marker.
3. Read the entire upper and lower blocks in context.
4. Determine the intended final content by using the source requirements, issue, pull request, style guide, and content-owner guidance.
5. Edit the file so that only the final intended content remains.
6. Delete the `<<<<<<<`, `=======`, and `>>>>>>>` marker lines.
7. Repeat for every conflict block in the file.
8. Search the entire repository file for the three marker patterns.
9. Save the file.
10. Return to GitHub Desktop.
11. Mark the file as resolved if GitHub Desktop requests that action.
12. Repeat for each conflicting file.
13. Select the action to continue or complete the merge when it becomes available.
14. Review the merge commit and complete diff.
15. Push the topic branch.
16. Reopen the pull request and confirm that the conflict warning is gone.

> **Content rule:** Resolve meaning, not markers. Removing marker lines without deciding which content is correct can create a clean-looking but inaccurate document.

<!-- Screenshot placeholder: GitHub Desktop listing conflicting files during a merge -->

<!-- Screenshot placeholder: Plain-text editor showing one conflict block before resolution -->

### Validate the resolved document

Review more than the edited lines. Confirm all of the following:

- No conflict markers remain.
- No paragraph, heading, list item, table row, link, or code example is duplicated.
- No required content from either branch was accidentally removed.
- Heading hierarchy and numbered procedures remain sequential.
- Relative links and filenames still point to valid targets.
- Terminology and menu labels are consistent.
- The rendered document is readable.
- The diff contains no unrelated changes.
- The pull request targets the correct base branch.

## 12. Resolve a simple conflict on GitHub

GitHub's web conflict editor can resolve simple competing line changes when the **Resolve conflicts** button is available. Complex conflicts must be handled locally with an appropriate Git client or command-line workflow.

> **Important:** Resolving a conflict on GitHub merges the entire base branch into the head branch. Confirm both branches before continuing.

1. Open the affected pull request.
2. Near the merge section, select **Resolve conflicts**.
3. Select a conflicting file in the left pane.
4. Review each conflict block and its surrounding content.
5. Keep, combine, or rewrite the competing content to produce the required final result.
6. Delete the `<<<<<<<`, `=======`, and `>>>>>>>` markers.
7. Select **Mark as resolved** after the entire file is correct.
8. Repeat for every conflicting file.
9. Select **Commit merge**.
10. If prompted, confirm the branch that GitHub will update.
11. Review the pull request's **Files changed** tab again.
12. Wait for required checks and approvals before merging the pull request.

Do not use the web editor when:

- **Resolve conflicts** is unavailable.
- The conflict involves a file rename, deletion, binary file, generated output, or large structural rewrite.
- The correct result requires local rendering, testing, or comparison with other files.
- The repository requires a local validation tool before committing.

## 13. Recover discarded or missing document changes

### Check whether the work is committed elsewhere

Before assuming that content is lost:

1. Search the **History** tab on the current branch.
2. Inspect recently used branches under **Current Branch**.
3. Check for **Stashed Changes**.
4. Fetch the remote repository and inspect relevant remote branches.
5. Review recent pull requests and commits on GitHub.
6. Search the affected files for the missing text.

### Check the Windows Recycle Bin

If GitHub Desktop discarded the file changes:

1. Open the Windows Recycle Bin.
2. Look for a recently created, dated recovery item associated with GitHub Desktop.
3. Inspect the item without overwriting current repository files.
4. Restore it to a safe temporary location when possible.
5. Compare the recovered content with the current branch.
6. Copy only the intended content into the repository through the normal editor.
7. Review the resulting diff before committing.

### Check approved external recovery sources

When local Git recovery does not contain the text, use only sources allowed by policy:

- An editor's local history or autosave files.
- An approved backup system.
- A content-management export.
- A reviewed email or collaboration attachment.
- A previous published artifact.

Do not overwrite the repository file with an entire recovered copy until you compare both versions. A recovered file may be older and may silently remove valid recent work.

## 14. Respond to exposed sensitive data

A normal revert, deletion, or replacement commit does not remove a secret from earlier repository history.

If a password, token, private key, recovery code, connection string, or similar credential is committed:

1. Stop pushing, merging, sharing links, or copying the exposed value.
2. Notify the designated security contact or repository owner through the approved channel.
3. Revoke or rotate the credential immediately through its issuing system.
4. Record the affected repository, branches, pull requests, files, and approximate commits without repeating the secret.
5. Follow the organization's incident-response and sensitive-data-removal procedure.
6. Coordinate any history rewrite with all repository users.
7. Add prevention controls such as appropriate `.gitignore` entries, secret management, or push protection when directed.

History rewriting has broad side effects: it changes commit identifiers, can invalidate signatures and review references, may disrupt open pull requests, and can be undone accidentally when an old clone pushes contaminated history again. Do not improvise a force-push cleanup.

> **Important:** Credential rotation is the first containment action. History remediation is a separate, coordinated decision.

## 15. Handle special conflict types

### Edit-versus-delete conflict

One branch changed a file while another deleted it.

Determine whether the file should exist in the final repository. If it should exist, preserve and update the correct content. If it should remain deleted, confirm that links, navigation, indexes, and dependent files are also updated. Ask the content owner when deletion intent is unclear.

### Rename conflict

Different branches may rename the same file or rename and edit it differently.

Confirm the final path, then check every inbound relative link, navigation entry, and reference. A conflict can be technically resolved while leaving broken links behind.

### Binary-file conflict

Images, archives, office files, and other binary formats cannot be reconciled line by line.

Identify the authoritative version, preserve both candidates outside the repository when policy permits, and obtain owner approval before replacing either one. Validate the chosen file in its native application.

### Generated-file conflict

Do not manually combine generated output unless the project explicitly requires it. Resolve the source files first, then regenerate the output with the approved tool and version.

### Line-ending-only conflict

If the diff shows widespread changes with no visible content difference, stop and check repository line-ending rules, `.gitattributes`, and editor configuration. Do not accept a whole-file rewrite merely to clear the conflict.

### Cloud-sync or file-lock conflict

When the repository is stored in a synchronized folder, another application may lock a file or create an external conflict copy.

1. Save and close the file in all editors.
2. Allow the synchronization client to finish its current operation.
3. Confirm which file is inside the repository and which is an external conflict copy.
4. Compare content before deleting or renaming either file.
5. Return to GitHub Desktop and review the resulting diff.

Escalate repeated locking or duplicate-file behavior before continuing repository recovery.

## 16. Avoid detached-HEAD recovery mistakes

GitHub Desktop can check out an individual historical commit for inspection. This creates a detached-HEAD state rather than placing you on a normal branch.

- Use detached HEAD for inspection, not ongoing authoring.
- Do not assume a commit created in detached HEAD belongs to a durable branch.
- Before editing, create or switch to an appropriate branch.
- If work was accidentally committed in detached HEAD, record the commit identifier and ask the repository owner to help preserve it on a branch before switching away.

> **Warning:** Work committed without an associated branch can become difficult to find after another branch is checked out.

## 17. Validate the final repository state

Complete this validation after every recovery, even when the operation appeared successful.

### Local validation

1. Confirm the **Current Repository**.
2. Confirm the **Current Branch**.
3. Review **Changes** and account for every remaining file.
4. Review **History** and identify each recovery-related commit.
5. Open each affected document in the editor.
6. Search for conflict markers.
7. Render or preview the documentation.
8. Test links, filenames, headings, lists, tables, and examples.
9. Confirm that no recovery copies or temporary files were added accidentally.

### Remote validation

1. Push the intended branch when required.
2. Open the repository or pull request on GitHub.
3. Confirm the correct base and head branches.
4. Review the complete **Files changed** diff.
5. Confirm that checks and review requirements are satisfied.
6. Confirm that the branch is mergeable or that any remaining blocker is understood.
7. After merging, synchronize local `main` and verify the final files.

### Recovery record

For significant incidents, record:

- What happened.
- Which branches, files, and commits were affected.
- What content was preserved.
- Which recovery operation was used.
- Who approved ambiguous content decisions.
- What validation was performed.
- What preventive action was added.

## Troubleshooting

### Undo is unavailable

Confirm that the commit is the latest commit, belongs to the current branch, and has not been pushed. Use **Revert** for a pushed commit. Use **Reset to commit** only for local unpushed history.

### Amend requires a force push

The original commit was pushed. Cancel the force push unless the repository owner has explicitly authorized a coordinated rewrite. Create a normal corrective commit or revert instead.

### Reset does not offer the expected target

Fetch the repository, verify the current branch, and inspect **History** again. Do not reset while unsure which branch or commit is active.

### A cherry-pick creates conflicts

Resolve each file using the same content-review process as a merge conflict. If the target branch's context has changed substantially, cancel or escalate rather than forcing the old change into an incompatible location.

### The same conflict returns

Confirm that the resolution was saved, marked resolved, committed, and pushed to the same head branch used by the pull request. Then fetch and verify that the base branch did not change again after the resolution.

### Conflict markers remain in the rendered document

Search the repository for `<<<<<<<`, `=======`, and `>>>>>>>`. Correct the source file, review the diff, commit the fix, and push it to the pull request branch.

### Push is rejected

Select **Fetch origin** and inspect the remote changes before acting. Do not force-push merely to bypass the rejection. Pull or coordinate with the contributor who updated the remote branch, then resolve any resulting conflicts.

### Stash is unavailable

Confirm that uncommitted changes exist. Check whether GitHub Desktop already contains a stash; only one stash set is supported. Restore, commit, or discard the existing stash before creating another.

### The pull request still reports a conflict

Confirm that the resolution commit was pushed to the pull request's head branch. Refresh the pull request and verify that its base branch has not changed. If the base branch advanced, merge the current base into the topic branch and validate again.

### The resolved document contains duplicate sections

Both branch versions may have been retained without editorial reconciliation. Compare the base and topic versions, identify the authoritative structure, remove duplication, and have the content owner review the result.

### A discarded file is not in the Recycle Bin

Check commits, branches, stash, editor history, and approved backups. Do not install unapproved recovery software on a managed computer. Escalate according to data-recovery policy.

### GitHub Desktop reports no local changes, but work is missing

Inspect **History**, other branches, the stash, the remote repository, and pull requests. A clean working tree reports only that the checked-out files match the current commit.

## Recovery checklist

- [ ] Correct repository identified.
- [ ] Correct current branch identified.
- [ ] Publication state determined.
- [ ] Needed work preserved before destructive action.
- [ ] Least disruptive recovery method selected.
- [ ] Shared history was not rewritten without authorization.
- [ ] Every conflict was resolved for meaning, not only syntax.
- [ ] No conflict markers remain.
- [ ] Affected documents were rendered and reviewed.
- [ ] Links, headings, tables, lists, and filenames were validated.
- [ ] Local history and working tree were reviewed.
- [ ] Remote branch or pull request was reviewed.
- [ ] Sensitive data was handled through the incident process.
- [ ] Final `main` branch was synchronized after merge.

## Practice scenario

Use a private demonstration repository with non-sensitive sample text.

1. Create a branch named `practice/conflict-recovery` from a synchronized `main` branch.
2. Add a short Markdown file and commit it.
3. Make another local edit and practice stashing and restoring it.
4. Commit a deliberate typo, then use **Undo** to return it to the working tree.
5. Correct the typo and recommit it.
6. Create a second practice branch and cherry-pick the corrected commit into it.
7. Revert the copied commit and inspect both commits in **History**.
8. Review how each operation changed the working tree and history.
9. Delete the practice branches only after confirming that no needed work remains.

Do not simulate a credential incident. Never place a real or realistic secret in a repository for training.

## References

- [Options for managing commits in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/options-for-managing-commits-in-github-desktop)
- [Undoing a commit in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/undoing-a-commit-in-github-desktop)
- [Resetting to a commit in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/resetting-to-a-commit-in-github-desktop)
- [Amending a commit in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/amending-a-commit-in-github-desktop)
- [Reverting a commit in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/reverting-a-commit-in-github-desktop)
- [Cherry-picking a commit in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/cherry-picking-a-commit-in-github-desktop)
- [Stashing changes in GitHub Desktop](https://docs.github.com/en/desktop/making-changes-in-a-branch/stashing-changes-in-github-desktop)
- [Managing branches in GitHub Desktop](https://docs.github.com/en/desktop/making-changes-in-a-branch/managing-branches-in-github-desktop)
- [Checking out a commit in GitHub Desktop](https://docs.github.com/en/desktop/managing-commits/checking-out-a-commit-in-github-desktop)
- [Syncing your branch in GitHub Desktop](https://docs.github.com/en/desktop/working-with-your-remote-repository-on-github-or-github-enterprise/syncing-your-branch-in-github-desktop)
- [About merge conflicts](https://docs.github.com/en/pull-requests/reference/merge-conflicts)
- [Resolving a merge conflict on GitHub](https://docs.github.com/en/pull-requests/how-tos/merge-and-close-pull-requests/resolving-a-merge-conflict-on-github)
- [Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)
