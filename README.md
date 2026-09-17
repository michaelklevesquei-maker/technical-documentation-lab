# Technical Documentation Lab

A practical documentation portfolio demonstrating how to plan, write, validate, review, and maintain technical user guidance through a controlled Git and GitHub workflow.

## Portfolio overview

This repository contains a connected set of task-based guides for GitHub Desktop on Windows. The documentation is written for contributors who need to work with version-controlled files without relying on command-line Git.

The project demonstrates more than finished prose. Its repository history records the documentation lifecycle: scoped branches, focused commits, pull-request descriptions, rendered-content checks, merges, branch cleanup, and continued maintenance.

## Documentation

| Guide | Purpose | Primary topics |
|---|---|---|
| [Getting Started with GitHub Desktop for Windows](docs/getting-started.md) | Prepare and validate a Windows workstation for GitHub-based documentation work | Installation, authentication, commit identity, editor configuration, repository creation and cloning, security precautions, validation, and troubleshooting |
| [Branch and Review Workflow with GitHub Desktop for Windows](docs/branch-and-review.md) | Move a controlled change from a synchronized local repository into the default branch | Branch preparation, local editing, diff review, commits, publication, pull requests, review feedback, merging, cleanup, and final synchronization |
| [Conflict Resolution and Document Recovery with GitHub Desktop for Windows](docs/conflict-resolution.md) | Protect, recover, and reconcile documentation work when something goes wrong | Stash, discard, undo, amend, reset, cherry-pick, revert, merge conflicts, missing-file recovery, sensitive-data incidents, and final validation |

## Skills demonstrated

- Audience analysis and task-based information design.
- Structured Markdown authoring for web-based delivery.
- Procedural writing with prerequisites, ordered steps, verification points, warnings, and stop conditions.
- Technical research using official product documentation.
- Security-conscious guidance for credentials, repository visibility, and sensitive data.
- Troubleshooting organized around observable symptoms and safe recovery actions.
- Terminology control and consistent user-interface references.
- Git-based documentation management through branches, commits, pull requests, reviews, merges, and cleanup.
- Quality assurance through source review, rendered-content inspection, link checking, and repository-state validation.

## Documentation approach

Each guide is designed to help a reader complete a real task while understanding the state changes that matter.

The documentation uses:

- Clear purpose, audience, requirements, and outcomes.
- Numbered procedures written in execution order.
- Tables for decisions, comparisons, requirements, and validation evidence.
- Callouts for security concerns, destructive actions, limitations, and stop conditions.
- Verification statements at important workflow boundaries.
- Troubleshooting sections that distinguish symptoms from corrective actions.
- Links to authoritative references for product-specific behavior.

## Repository workflow

Documentation changes are developed through a branch-and-review process:

1. Synchronize the local `main` branch.
2. Create a task-specific branch.
3. Draft and review the documentation locally.
4. Inspect every changed file and line in GitHub Desktop.
5. Commit the intended change with a meaningful message.
6. Publish the branch and open a pull request.
7. Review the rendered Markdown, file scope, and merge status.
8. Merge approved work and delete the completed branch.
9. Synchronize the local repository and verify the final state.

The repository's commit and pull-request history provides evidence of this workflow.

## Validation environment

The current guides were validated against:

- Windows
- GitHub Desktop 3.6.6 (x64)
- GitHub Free personal account
- GitHub's browser interface
- Plain-text Markdown files

Product interfaces and menu labels can change. Each guide records its validation date and includes links to official references where applicable.

## Project status

The initial documentation set is complete:

- [x] Project purpose, audience, and workflow overview
- [x] GitHub Desktop setup and repository preparation
- [x] Branch, commit, pull-request, and review workflow
- [x] Conflict resolution and document recovery
- [x] Repository-based review and validation history

Future revisions may add screenshots, accessibility review, automated link validation, release notes, or additional task guides.

## Use of this repository

This repository is a demonstration project created to show technical-writing, information-design, documentation-operations, and quality-assurance capabilities. It contains portfolio material rather than client work.
