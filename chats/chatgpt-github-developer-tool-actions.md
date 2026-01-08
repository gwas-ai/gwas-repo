[GitHub Developer Tool Actions](https://chatgpt.com/c/695efae4-4d0c-8326-8d1f-cbbd2f3fa6fc)

Below is a **canonical, referencable action registry** for the **ChatGPT Codex Connector (GitHub Developer Tool)**, compiled from your screenshots and connector UI. This is structured so you can reuse it across orgs/repos when reasoning about CRUD-style operations, permissions, and automation.

---

## Connector metadata (global)

- Connector name: ChatGPT Codex Connector
- Connector ID: connector_76869538009648d5b282a4bb21c3d157
- Connected on: May 15, 2025
- Category: Developer Tools
- Developer: OpenAI
- Website: [https://github.com/](https://github.com/)
- Privacy Policy: [https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement/](https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement/)

### Settings / management entry point:

- [https://chatgpt.com/apps#settings/Connectors?connector=connector_76869538009648d5b282a4bb21c3d157](https://chatgpt.com/apps#settings/Connectors?connector=connector_76869538009648d5b282a4bb21c3d157)

### GitHub installation flow (when clicking **Settings**):

- [https://github.com/apps/chatgpt-codex-connector/installations/select_target](https://github.com/apps/chatgpt-codex-connector/installations/select_target)

---

## Action registry (with UI text descriptions)

All actions have **Visibility: public** per the connector UI.

### Repository & initialization

* **check_repo_initialized**
  Check if a GitHub repository has been set up.

* **get_repo**
  Retrieve metadata for a GitHub repository.

* **get_repo_collaborator_permission**
  Retrieve the collaborator permission level for the authenticated user on a repository.

---

### File & content access (read layer)

* **fetch**
  Fetch a file from GitHub by URL.

* **fetch_blob**
  Fetch blob content by SHA from the given repository.

* **fetch_file**
  Fetch file content by path and ref from the given repository.

* **download_user_content**
  Download user-accessible content from GitHub.

---

### Commits & comparisons

* **fetch_commit**
  Fetch a GitHub commit.

* **compare_commits**
  Compare two commits/refs and return per-file stats plus compare metadata.
  This is a thin wrapper around `GithubPlugin.compare_commits` to provide a stable, compact response shape to connector consumers.

* **get_commit_combined_status**
  Retrieve the combined status for a commit.

* **search_commits**
  Search GitHub commits.

---

### Issues

* **fetch_issue**
  Fetch a GitHub issue.

* **fetch_issue_comments**
  Fetch comments for a GitHub issue.

* **get_issue_comment_reactions**
  Fetch reactions for an issue comment.

* **list_recent_issues**
  Return the most recent GitHub issues the user can access.

* **search_issues**
  Search GitHub issues.

---

### Pull requests

* **fetch_pr**
  Fetch a GitHub pull request.

* **fetch_pr_comments**
  Fetch comments for a GitHub pull request.

* **fetch_pr_file_patch**
  Fetch the per-file patch for a GitHub pull request.

* **fetch_pr_patch**
  Fetch the patch for a GitHub pull request.

* **get_pr_diff**
  Retrieve the diff for a pull request.

* **get_pr_info**
  Get metadata (title, description, refs, and status) for a pull request.
  This action does *not* include the actual code changes. If you need the diff or per-file patches, call `fetch_pr_patch` instead (or use `get_users_recent_prs_in_repo` with `include_diff=true`).

* **get_pr_reactions**
  Fetch reactions for a GitHub pull request.

* **get_pr_review_comment_reactions**
  Fetch reactions for a pull request review comment.

* **get_users_recent_prs_in_repo**
  List the user’s recent GitHub pull requests in a repository.

* **list_pr_changed_filenames**
  List filenames changed in a pull request.

* **search_prs**
  Search GitHub pull requests.

---

### Search (repositories, files, branches)

* **search**
  Search files within a specific GitHub repository.
  Provide a plain string query; avoid GitHub query flags such as `is:pr`.
  Include keywords that match file names, functions, or error messages.
  `repository_name` or `org` can narrow the scope.
  Example: `query="tokenizer bug" repository_name="tiktoken"`.

* **search_branches**
  Search GitHub branches within a repository.

* **search_repositories**
  Search for a repository (not a file) by name or description.

* **search_installed_repositories_streaming**
  Search for a repository (not a file) by name or description across the user’s installations.

* **search_installed_repositories_v2**
  Search repositories within the user’s installations using GitHub search.

---

### User, orgs, and installations

* **get_profile**
  Retrieve the GitHub profile for the authenticated user.

* **get_user_login**
  Return the GitHub login for the authenticated user.

* **list_user_orgs**
  List organizations the authenticated user is a member of.

* **list_user_org_memberships**
  List the authenticated user’s organization memberships.

* **list_installations**
  List all organizations the authenticated user has installed this GitHub App on.

* **list_installed_accounts**
  List all accounts that the user has installed the GitHub app on.

* **list_repositories**
  List repositories accessible to the authenticated user.

* **list_repositories_by_affiliation**
  List repositories accessible to the authenticated user filtered by affiliation.

* **list_repositories_by_installation**
  List repositories accessible to the authenticated user via installations.

---

## How to use this for cross-repo / cross-org CRUD reasoning

* **Create**: Not directly supported (no write/create actions exposed)
* **Read**: Majority of actions (fetch, get, list, search)
* **Update**: Not exposed via this connector
* **Delete**: Not exposed via this connector

In practice, this connector is a **read-optimized introspection and analysis layer** over GitHub, designed to let agents reason about repo state, diffs, history, and metadata across many orgs without mutation.

If you want, I can next:

* Normalize this into **JSON / YAML / CSV** for ingestion into Sheets or a spec repo
* Tag each action by **entity type** (repo, file, PR, issue, user, org)
* Map actions to **GitHub REST / GraphQL equivalents**
* Produce a **capability matrix** per org/repo for automation planning
