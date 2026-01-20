# GitHub CLI (gh) Complete Workflow Documentation

## Overview
This document describes the complete workflow for using GitHub CLI (`gh`) to manage GitHub repositories, issues, pull requests, and more.

---

## 1. Authentication Commands

### 1.1 Login
```bash
gh auth login
```
**Process:**
1. Select account (GitHub.com or GitHub Enterprise)
2. Select protocol (HTTPS or SSH)
3. Authenticate via browser or token

### 1.2 Check Status
```bash
gh auth status
```
**Example Output:**
```
github.com
  ✓ Logged in to github.com account Clearzero22 (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
```

### 1.3 Logout
```bash
gh auth logout
```

### 1.4 Refresh Token
```bash
gh auth refresh -s project  # Add project scope
```

---

## 2. Repository Operations

### 2.1 List Repositories
```bash
gh repo list
gh repo list <username>      # List specific user's repos
gh repo list --limit 10      # Limit results
```
**Example Output:**
```
Clearzero22/rust_test_workflow_project    public    2026-01-20T17:11:30Z
Clearzero22/my-egui-pro                   public    2026-01-18T08:04:35Z
Clearzero22/gruvbox-theme                 public    2026-01-18T08:00:51Z
```

### 2.2 Create Repository
```bash
gh repo create <name> [flags]
```
**Flags:**
| Flag | Description |
|------|-------------|
| `--public` | Public repository |
| `--private` | Private repository |
| `--add-readme` | Add README.md |
| `--clone` | Clone after creation |
| `--description "text"` | Repository description |
| `--gitignore <lang>` | .gitignore template |
| `--license <license>` | License type |

**Examples:**
```bash
# Basic public repo
gh repo create my-project --public --add-readme --clone

# With description and gitignore
gh repo create my-rust-project --public --description "My Rust app" \
  --gitignore Rust --license MIT --add-readme --clone

# In organization
gh repo create my-org/my-project --public --clone

# From existing local directory
gh repo create my-project --private --source=. --remote=upstream
```

### 2.3 Clone Repository
```bash
gh repo clone <owner/repo>
gh repo clone <owner/repo> <directory>  # Clone to specific directory
```
**Example:**
```bash
gh repo clone Clearzero22/rust_test_workflow_project
```

### 2.4 View Repository
```bash
gh repo view                    # View current repo
gh repo view <owner/repo>       # View specific repo
gh repo view --web              # Open in browser
```
**Example Output:**
```
name:    Clearzero22/rust_test_workflow_project
description:
--
# rust_test_workflow_project
```

### 2.5 Delete Repository
```bash
gh repo delete <owner/repo>
```

### 2.6 Fork Repository
```bash
gh repo fork <owner/repo> --clone
```

---

## 3. Pull Request Operations

### 3.1 List Pull Requests
```bash
gh pr list                         # List all PRs
gh pr list --limit 10              # Limit results
gh pr list --state open            # Filter by state
gh pr list --author <username>     # Filter by author
```
**Example Output:**
```
No pull requests found
```

### 3.2 Create Pull Request
```bash
gh pr create [flags]
```
**Flags:**
| Flag | Description |
|------|-------------|
| `--title "text"` | PR title |
| `--body "text"` | PR description |
| `--fill` | Auto-fill from commits |
| `--base <branch>` | Target branch |
| `--head <branch>` | Source branch |
| `--draft` | Create as draft |
| `--label <name>` | Add labels |
| `--assignee <login>` | Assign to user |
| `--reviewer <handle>` | Request review |
| `--web` | Open in browser |

**Examples:**
```bash
# Interactive
gh pr create

# With title and body
gh pr create --title "Add new feature" --body "Closes #123"

# Auto-fill from commits
gh pr create --fill

# Draft PR with labels
gh pr create --draft --label enhancement --label "needs review"

# With reviewers
gh pr create --reviewer monalisa,hubot --reviewer myorg/team-name
```

### 3.3 Checkout Pull Request
```bash
gh pr checkout <number>          # Checkout by PR number
gh pr checkout <url>             # Checkout by URL
gh pr checkout <branch>          # Checkout by branch name
gh pr checkout                   # Interactive selection
```
**Flags:**
| Flag | Description |
|------|-------------|
| `--branch <name>` | Custom local branch name |
| `--force` | Reset to latest state |
| `--detach` | Detached HEAD checkout |

**Examples:**
```bash
gh pr checkout 123
gh pr checkout https://github.com/OWNER/REPO/pull/123
gh pr checkout feature
```

### 3.4 View Pull Request
```bash
gh pr view                        # View current branch's PR
gh pr view <number>              # View specific PR
gh pr view --web                 # Open in browser
```

### 3.5 View Pull Request Diff
```bash
gh pr diff                        # Diff of current PR
gh pr diff <number>              # Diff of specific PR
gh pr diff --web                 # Open diff in browser
gh pr diff --name-only           # Show changed files only
```

### 3.6 Merge Pull Request
```bash
gh pr merge <number> [flags]
```
**Merge Methods:**
| Flag | Description |
|------|-------------|
| `--merge` | Merge commit (default) |
| `--squash` | Squash and merge |
| `--rebase` | Rebase and merge |
| `--delete-branch` | Delete branch after merge |
| `--subject` | Custom commit subject |
| `--body` | Custom commit body |
| `--auto` | Auto-merge when ready |

**Examples:**
```bash
# Simple merge
gh pr merge 123

# Squash merge and delete branch
gh pr merge 123 --squash --delete-branch

# With custom message
gh pr merge 123 --subject "Fix bug" --body "Resolves issue #456"
```

### 3.7 Close Pull Request
```bash
gh pr close <number>
```

### 3.8 Reopen Pull Request
```bash
gh pr reopen <number>
```

---

## 4. Issue Operations

### 4.1 List Issues
```bash
gh issue list                    # List all issues
gh issue list --limit 20         # Limit results
gh issue list --state open       # Filter by state
gh issue list --label bug        # Filter by label
```
**Example Output:**
```
No issues found
```

### 4.2 Create Issue
```bash
gh issue create [flags]
```
**Flags:**
| Flag | Description |
|------|-------------|
| `--title "text"` | Issue title |
| `--body "text"` | Issue description |
| `--label <name>` | Add labels |
| `--assignee <login>` | Assign to user |
| `--milestone <name>` | Add to milestone |
| `--project <title>` | Add to project |
| `--web` | Open in browser |

**Examples:**
```bash
# Interactive
gh issue create

# With title and body
gh issue create --title "Bug found" --body "Description of the bug"

# With labels and assignee
gh issue create --title "Feature request" --label enhancement \
  --assignee "@me"

# Multiple labels
gh issue create --label bug --label "high priority"
```

### 4.3 View Issue
```bash
gh issue view <number>           # View specific issue
gh issue view --web              # Open in browser
```

### 4.4 Close Issue
```bash
gh issue close <number>
```

### 4.5 Reopen Issue
```bash
gh issue reopen <number>
```

### 4.6 Comment on Issue
```bash
gh issue comment <number> --body "Comment text"
```

---

## 5. Search Operations

### 5.1 Search Repositories
```bash
gh search repos <query>          # Search repositories
gh search repos topic:rust       # Search by topic
gh search repos language:python  # Search by language
gh search repos stars:>1000      # Search by stars
gh search repos --limit 10       # Limit results
```
**Examples:**
```bash
gh search repos topic:rust --limit 5
gh search repos language:javascript stars:>1000
gh search repos "web framework" in:name,description
```
**Example Output:**
```
rust-lang/rust        Empowering everyone to build reliable and efficient software.      public    2026-01-20T17:00:44Z
rustdesk/rustdesk     An open-source remote desktop application...                         public    2026-01-20T16:27:04Z
denoland/deno         A modern runtime for JavaScript and TypeScript.                     public    2026-01-20T17:04:44Z
tauri-apps/tauri      Build smaller, faster, and more secure desktop applications...      public    2026-01-20T17:03:25Z
```

### 5.2 Search Issues
```bash
gh search issues <query>         # Search issues
gh search issues state:open      # Filter by state
gh search issues author:<user>   # Filter by author
```

### 5.3 Search Pull Requests
```bash
gh search prs <query>            # Search PRs
gh search prs state:open         # Filter by state
```

### 5.4 Search Code
```bash
gh search code <query>           # Search code
gh search code language:rust     # Filter by language
```

### 5.5 Search Commits
```bash
gh search commits <query>        # Search commits
gh search commits author:<user>  # Filter by author
```

**Note:** To exclude qualifiers, use `--` separator:
```bash
gh search issues -- "query -label:bug"
```

---

## 6. Additional Useful Commands

### 6.1 Status
```bash
gh status                        # Show notifications and activity
```

### 6.2 Browse
```bash
gh browse                        # Open repo in browser
gh browse --branch <branch>      # Open specific branch
gh browse issues/123             # Open specific issue/PR
```

### 6.3 API Requests
```bash
gh api /user                    # Get user info
gh api /repos/<owner>/<repo>    # Get repo info
gh api /user/repos              # List user repos
gh api -X POST <endpoint>       # POST request
gh api -f name=value <endpoint> # Form data
```

### 6.4 Workflow/Actions
```bash
gh workflow list                # List workflows
gh workflow view <name>         # View workflow
gh run list                     # List workflow runs
gh run view <run-id>            # View run details
```

---

## 7. Complete Workflow Example

### Creating a new Rust project with GitHub integration

```bash
# 1. Check authentication
gh auth status

# 2. Create repository
gh repo create my-rust-project --public \
  --description "A Rust application" \
  --gitignore Rust \
  --license MIT \
  --add-readme \
  --clone

# 3. Initialize Rust project
cd my-rust-project
cargo init

# 4. Make initial commit
git add .
git commit -m "Initial Rust project setup"
git push

# 5. Create a feature branch
git checkout -b feature/add-logging
# ... make changes ...
git add .
git commit -m "Add logging functionality"
git push -u origin feature/add-logging

# 6. Create pull request
gh pr create --title "Add logging" --body "Implements basic logging"

# 7. Create an issue
gh issue create --title "Add tests" --label "enhancement"
```

---

## 8. Quick Reference Card

| Category | Command | Description |
|----------|---------|-------------|
| **Auth** | `gh auth status` | Check login status |
| **Auth** | `gh auth login` | Login to GitHub |
| **Repo** | `gh repo list` | List repositories |
| **Repo** | `gh repo create <name>` | Create repository |
| **Repo** | `gh repo clone <repo>` | Clone repository |
| **Repo** | `gh repo view` | View repository info |
| **PR** | `gh pr list` | List pull requests |
| **PR** | `gh pr create` | Create pull request |
| **PR** | `gh pr checkout <n>` | Checkout PR |
| **PR** | `gh pr merge <n>` | Merge PR |
| **PR** | `gh pr diff` | View PR changes |
| **Issue** | `gh issue list` | List issues |
| **Issue** | `gh issue create` | Create issue |
| **Issue** | `gh issue close <n>` | Close issue |
| **Search** | `gh search repos <q>` | Search repos |
| **Search** | `gh search issues <q>` | Search issues |
| **Search** | `gh search code <q>` | Search code |

---

## 9. Tips and Best Practices

1. **Use `--help`**: All commands support `--help` for detailed usage
   ```bash
   gh pr create --help
   ```

2. **Interactive Mode**: Many commands work interactively without flags
   ```bash
   gh pr create    # Will prompt for required info
   ```

3. **JSON Output**: Use `--json` for scriptable output
   ```bash
   gh repo list --json name,visibility
   ```

4. **Repository Target**: Use `-R` for any repo command
   ```bash
   gh pr list -R owner/repo
   ```

5. **Web Browser**: Use `--web` to open in browser
   ```bash
   gh pr view --web
   gh browse issues/123
   ```

6. **Piping**: Use with other tools
   ```bash
   gh search repos topic:rust --limit 100 | grep framework
   ```

---

## 10. Troubleshooting

### Authentication Issues
```bash
gh auth logout
gh auth login
```

### Permission Issues
```bash
gh auth refresh -s project  # Add missing scopes
```

### Repository Not Found
- Check you have access to the repository
- Verify the repository name/owner is correct

### Branch Issues
```bash
git fetch origin          # Update remote branches
gh pr checkout --force    # Force checkout
```

---

## Document Version
- **Version**: 1.0
- **Last Updated**: 2026-01-21
- **gh Version**: 2.83.2
- **Author**: Generated via GitHub CLI testing
