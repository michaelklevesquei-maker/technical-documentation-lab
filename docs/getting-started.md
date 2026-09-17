# Getting Started with GitHub Desktop for Windows

> **Document status:** Demonstration user guide  
> **Validated environment:** Windows, GitHub Desktop 3.6.6 (x64), GitHub Free personal account  
> **Last validated:** September 17, 2026

## Purpose

This guide explains how to prepare a Windows computer for documentation work with GitHub Desktop. It covers installation, browser-based authentication, commit identity, editor configuration, repository creation or cloning, and verification of the completed setup.

The procedures use GitHub Desktop's graphical interface. Command-line Git is not required.

## Intended audience

This guide is for technical writers, reviewers, trainers, and other contributors who need to work with files stored in GitHub but do not routinely use Git from a command prompt.

You should be comfortable with basic Windows tasks such as downloading software, navigating folders, and saving files. Previous Git experience is not required.

## Outcomes

After completing this guide, you will be able to:

- Install GitHub Desktop on a supported Windows computer.
- Authenticate GitHub Desktop through a web browser.
- Configure the name and email address recorded in your commits.
- Select an external editor for repository files.
- Create a new repository or clone an existing repository.
- Confirm that the local repository and its GitHub remote are connected correctly.

## Before you begin

### Requirements

| Requirement | Details |
|---|---|
| Operating system | Windows 10 64-bit or later |
| GitHub account | A personal or organization-managed account with access to the required repositories |
| Web browser | A browser in which you can sign in to the intended GitHub account |
| Network access | HTTPS access to GitHub and permission to complete browser-based authentication |
| Local storage | Permission to install GitHub Desktop and create or clone repository folders |
| Editor | Notepad, Visual Studio Code, or another editor appropriate for the repository's file types |

> **Important:** Confirm which GitHub account is active in your browser before authenticating GitHub Desktop. If you use multiple personal or organizational accounts, signing in with the wrong browser session can connect GitHub Desktop to the wrong account.

### Security precautions

- Download GitHub Desktop only from the official GitHub Desktop website.
- Do not place passwords, personal access tokens, private keys, recovery codes, or other secrets in a repository.
- Do not paste credentials into a commit message, issue, pull request, or documentation example.
- Verify a repository's visibility before publishing it. A public repository can be viewed by anyone.
- Follow your organization's authentication, device-management, data-handling, and repository-access policies.

## Key terms

| Term | Meaning in this guide |
|---|---|
| Repository | A project folder whose files and revision history are managed by Git. |
| Local repository | The working copy stored on your computer. |
| Remote repository | The corresponding repository hosted on GitHub. |
| `origin` | The conventional name Git assigns to the remote repository associated with a local clone. |
| Branch | An isolated line of work used to develop or review changes without immediately changing the default branch. |
| Commit | A recorded set of changes with an author, timestamp, and summary. |
| Push | Send local commits to the remote repository. |
| Pull | Download and integrate remote commits into the current local branch. |
| Clone | Create a local copy of an existing remote repository. |

## 1. Install GitHub Desktop

1. In a web browser, go to [https://desktop.github.com/](https://desktop.github.com/).
2. Select **Download for Windows**.
3. Open the downloaded setup file from the browser or the Windows **Downloads** folder.
4. If Windows displays a security prompt, verify that the application identifies GitHub as its source before continuing.
5. Allow the installation to finish. GitHub Desktop opens automatically when installation is complete.

<!-- SCREENSHOT: GitHub Desktop download page with Download for Windows identified. -->

### Installation verification

1. In GitHub Desktop, select **Help > About GitHub Desktop**.
2. Confirm that a version number appears.
3. Select **Check for Updates** if that option is available.
4. Close the **About GitHub Desktop** window.

GitHub Desktop normally downloads updates automatically and installs them when the application restarts. A manual update check is useful before validating or documenting a workflow because labels and interface behavior can change between releases.

## 2. Authenticate to GitHub

GitHub Desktop uses HTTPS for communication with GitHub. Authentication begins in the desktop application and is completed in a web browser.

1. In GitHub Desktop, select **File > Options**.
2. Select **Accounts**.
3. Next to **GitHub.com**, select **Sign In**.
4. In the sign-in window, select **Continue With Browser**.
5. In the browser, confirm that the displayed GitHub account is the account you intend to connect.
6. Complete any password, passkey, two-factor authentication, or organization authorization prompts required for that account.
7. Approve the request to open or return to GitHub Desktop if the browser asks for permission.
8. Return to GitHub Desktop and confirm that the intended account appears under **File > Options > Accounts**.

<!-- SCREENSHOT: Accounts pane showing a signed-in GitHub.com account. Do not expose private account data. -->

> **Security note:** GitHub Desktop should direct authentication through GitHub in your browser. Do not provide your GitHub password to an unrelated application or website.

### Verify repository access

Authentication confirms your identity, but repository permissions are managed separately. Organization membership, single sign-on, repository roles, or enterprise policies may limit which repositories you can see or modify.

To perform a quick access check:

1. Select **File > Clone Repository**.
2. Select the **GitHub.com** tab.
3. Confirm that repositories you expect to access appear in the list.
4. Select **Cancel** unless you are ready to clone a repository.

If a required private repository is missing, confirm your organization access before repeating authentication.

## 3. Configure commit identity

Every commit records an author name and email address. Configure these values deliberately so GitHub can attribute work to the correct account and reviewers can identify the contributor.

1. Select **File > Options**.
2. Select **Git**.
3. Review the displayed **Name** and **Email** values.
4. Use the identity approved for the repository or organization.
5. Select **Save**.

> **Privacy consideration:** A commit email can become part of repository history. If you do not want a personal email address recorded in commits, review GitHub's email-privacy settings and use an approved GitHub-provided `noreply` address or an organization-approved address.

### Confirm attribution after the first commit

After you create and push your first commit, open it on GitHub and confirm that it is associated with the intended profile. Correct an attribution problem before producing additional commits; changing the local setting does not automatically rewrite existing repository history.

## 4. Configure an external editor

GitHub Desktop shows differences between file versions, but repository content is normally created and edited in a separate application.

1. Select **File > Options**.
2. Select **Integrations**.
3. Under **External Editor**, select an installed editor.
4. Select **Save**.

You can later open the current repository in that editor by selecting **Repository > Open in Default Editor**. On Windows, the shortcut is **Ctrl+Shift+A**.

Choose an editor that preserves plain text and UTF-8 encoding for Markdown, configuration, and source files. Word processors may add formatting or metadata that is inappropriate for plain-text repository content.

## 5. Choose a repository path

Continue with the procedure that matches your situation:

- Use **Create a new repository** when the project does not yet exist in Git or on GitHub.
- Use **Clone an existing repository** when a remote repository already exists on GitHub.

Do not create a new repository as a substitute for cloning an existing one. Two unrelated repositories can have similar files while maintaining separate histories.

## 6A. Create a new repository

1. Select **File > New Repository**.
2. Enter a concise repository **Name**. Use a stable name that reflects the project rather than a temporary task.
3. Enter a **Description** that states the repository's purpose.
4. Under **Local Path**, select the parent folder in which GitHub Desktop will create the repository folder.
5. Select **Initialize this repository with a README** when the project should begin with a landing page and basic project information.
6. Select a `.gitignore` template only when the project contains generated, temporary, credential, build, or environment files that Git should not track.
7. Select a license only when you understand the permissions it grants and the project owner has approved it.
8. Select **Create Repository**.

### Review the local repository before publishing

1. Select **Repository > Show in Explorer**.
2. Confirm that the repository was created in the intended location.
3. Review the initial files.
4. Return to GitHub Desktop.

### Publish the repository

Publishing creates the remote GitHub repository and connects it to the local repository.

1. Select **Publish repository**.
2. Verify the repository name and description.
3. Select the correct owner or organization.
4. Review **Keep this code private** carefully:
   - Leave it selected when access must be restricted.
   - Clear it only when the repository is approved for public access.
5. Select **Publish Repository**.
6. When publication finishes, select **Repository > View on GitHub** and confirm that the correct repository opens.

> **Caution:** Repository visibility is a data-exposure decision, not merely a display preference. Remove secrets and restricted information before publishing, even when the repository is intended to remain private.

## 6B. Clone an existing repository

Cloning downloads the remote repository's files and history and configures the connection between the local and remote copies.

1. Select **File > Clone Repository**.
2. Select the **GitHub.com** tab.
3. Select the required repository.
4. If the repository is not listed, select the **URL** tab and enter its approved HTTPS URL.
5. Under **Local Path**, select the parent folder in which the repository folder should be created.
6. Confirm that the destination will not overwrite an unrelated folder.
7. Select **Clone**.
8. Wait for the operation to finish before editing files.

<!-- SCREENSHOT: Clone a Repository window showing repository selection and Local Path. Use demonstration data only. -->

### Verify the clone

1. Confirm that GitHub Desktop displays the expected repository under **Current repository**.
2. Confirm that **Current branch** shows the repository's default branch, commonly `main`.
3. Select **Repository > Show in Explorer** and confirm that the expected files are present.
4. Select **Fetch origin**.
5. Confirm that GitHub Desktop reports no connection or authentication error.
6. Select **Repository > View on GitHub** and confirm that the expected remote repository opens.

## 7. Validate the completed setup

The setup is complete when all applicable statements are true:

- GitHub Desktop opens without an installation error.
- The intended GitHub account appears under **File > Options > Accounts**.
- The configured commit name and email are correct.
- The selected external editor opens repository files successfully.
- The repository is stored in the intended local folder.
- GitHub Desktop displays the expected current repository and default branch.
- **Fetch origin** completes without a connection or authentication error.
- **View on GitHub** opens the matching remote repository.
- The repository visibility and owner are correct.

Record the GitHub Desktop version, Windows version, account type, repository visibility, and validation date when the environment is being used to test formal documentation.

## Troubleshooting

### The browser authenticates the wrong GitHub account

1. Cancel the authorization if it has not completed.
2. Sign out of the unintended GitHub browser session or use an approved browser profile for the correct account.
3. In GitHub Desktop, return to **File > Options > Accounts**.
4. Sign out of the incorrect account if necessary.
5. Repeat browser-based authentication and verify the account before authorizing access.

### A repository does not appear in the clone list

- Confirm that the signed-in account has access to the repository.
- Confirm any required organization membership or single sign-on authorization.
- Select the **URL** tab and use the approved HTTPS repository URL.
- Ask a repository administrator to verify your role if authentication succeeds but access is denied.

### GitHub Desktop reports an authentication or connection error

- Confirm general network access in a browser.
- Verify that a proxy, firewall, VPN, or security product is not blocking GitHub's HTTPS traffic.
- Sign out and authenticate again if the session has expired or the wrong account is connected.
- If the repository was originally configured with an SSH remote, review the remote URL. GitHub Desktop uses HTTPS for GitHub connections, and an SSH-configured remote can cause connection problems.
- Check [GitHub Status](https://www.githubstatus.com/) for a service incident before changing a working local configuration.

### The expected editor is not available

1. Confirm that the editor is installed.
2. Return to **File > Options > Integrations**.
3. Select the editor or choose **Configure Custom Editor**.
4. Verify the executable path and any required arguments.
5. Save the integration and test **Repository > Open in Default Editor** again.

### A file was saved with an unintended `.txt` extension

1. In File Explorer, enable **View > Show > File name extensions**.
2. Rename the file with the intended extension, such as `.md`.
3. Confirm the extension-change warning.
4. Return to GitHub Desktop and verify that the correct filename appears in **Changes**.

## Next step

After validating the environment, continue to the branch-and-review workflow. That procedure should cover synchronizing the default branch, creating a task-specific branch, editing and reviewing changes, writing an effective commit message, publishing the branch, opening a pull request, merging approved work, deleting the completed branch, and synchronizing the local repository again.

## References

- [GitHub Desktop documentation](https://docs.github.com/en/desktop)
- [Installing GitHub Desktop](https://docs.github.com/en/desktop/installing-and-authenticating-to-github-desktop/installing-github-desktop)
- [Authenticating to GitHub in GitHub Desktop](https://docs.github.com/en/desktop/installing-and-authenticating-to-github-desktop/authenticating-to-github-in-github-desktop)
- [About connections to GitHub in GitHub Desktop](https://docs.github.com/en/desktop/installing-and-authenticating-to-github-desktop/about-connections-to-github-in-github-desktop)
- [Configuring a default editor](https://docs.github.com/en/desktop/configuring-and-customizing-github-desktop/configuring-a-default-editor-in-github-desktop)
- [Creating your first repository using GitHub Desktop](https://docs.github.com/en/desktop/overview/creating-your-first-repository-using-github-desktop)
- [Cloning and forking repositories from GitHub Desktop](https://docs.github.com/en/desktop/adding-and-cloning-repositories/cloning-and-forking-repositories-from-github-desktop)
- [Managing branches in GitHub Desktop](https://docs.github.com/en/desktop/making-changes-in-a-branch/managing-branches-in-github-desktop)
