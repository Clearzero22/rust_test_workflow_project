# GitHub Project Creation Workflow

## Overview
This document describes the standard workflow for creating a new GitHub repository using the `gh` CLI tool.

## Prerequisites

### 1. Check gh Installation
```bash
gh --version
```
Expected output: `gh version X.X.X (YYYY-MM-DD)`

### 2. Check Authentication Status
```bash
gh auth status
```
If not authenticated, run:
```bash
gh auth login
```

## Workflow Steps

### Step 1: Review Repository Creation Options
```bash
gh repo create --help
```

### Step 2: Create Repository

#### Basic Syntax
```bash
gh repo create <repository-name> [flags]
```

#### Recommended Command
```bash
gh repo create <repository-name> --public --add-readme --clone
```

#### Flags Explanation
| Flag | Description |
|------|-------------|
| `--public` | Make repository public (use `--private` for private repos) |
| `--add-readme` | Initialize with a README.md file |
| `--clone` | Clone the repository to current directory after creation |
| `--description "text"` | Add repository description |
| `--gitignore <language>` | Add .gitignore template (e.g., Rust, Python, Node) |
| `--license <license>` | Add license (e.g., MIT, Apache-2.0) |

#### Additional Options
```bash
# Create with description
gh repo create <name> --public --description "Project description" --add-readme --clone

# Create with gitignore and license
gh repo create <name> --public --gitignore Rust --license MIT --add-readme --clone

# Create in organization
gh repo create <org>/<name> --public --clone

# Create from existing local directory
gh repo create <name> --private --source=. --remote=upstream
```

### Step 3: Verify Creation

#### Check Repository Contents
```bash
cd <repository-name>
ls -la
```

#### Check Git Remote
```bash
git remote -v
```

Expected output:
```
origin  https://github.com/<username>/<repository-name>.git (fetch)
origin  https://github.com/<username>/<repository-name>.map (push)
```

### Step 4: View Repository in Browser
```bash
gh repo view --web
```

## Common Post-Creation Tasks

### Initialize Rust Project
```bash
cd <repository-name>
cargo init
```

### Initialize Git Repository (if not using --clone)
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<username>/<repository-name>.git
git push -u origin main
```

## gh Command Reference

### Repository Commands
| Command | Description |
|---------|-------------|
| `gh repo create <name>` | Create new repository |
| `gh repo clone <owner/repo>` | Clone repository |
| `gh repo list` | List repositories |
| `gh repo view` | View repository details |
| `gh repo delete <owner/repo>` | Delete repository |
| `gh repo fork <owner/repo>` | Fork repository |

### Authentication Commands
| Command | Description |
|---------|-------------|
| `gh auth login` | Login to GitHub |
| `gh auth status` | Check authentication status |
| `gh auth logout` | Logout from GitHub |

### Pull Request Commands
| Command | Description |
|---------|-------------|
| `gh pr list` | List pull requests |
| `gh pr create` | Create pull request |
| `gh pr checkout <number>` | Checkout PR |
| `gh pr merge <number>` | Merge PR |
| `gh pr view` | View PR details |

### Issue Commands
| Command | Description |
|---------|-------------|
| `gh issue list` | List issues |
| `gh issue create` | Create issue |
| `gh issue view <number>` | View issue |
| `gh issue close <number>` | Close issue |

## Project Completion Checklist

- [ ] gh is installed and authenticated
- [ ] Repository created on GitHub
- [ ] Repository cloned locally
- [ ] README.md exists
- [ ] Git remote configured correctly
- [ ] Can access repository via browser

## Example Session

```bash
# Check gh installation
gh --version
# Output: gh version 2.83.2 (2025-12-10)

# Check authentication
gh auth status

# Create repository
gh repo create rust_test_workflow_project --public --add-readme --clone
# Output: https://github.com/Clearzero22/rust_test_workflow_project

# Verify
cd rust_test_workflow_project
ls -la
git remote -v

# Open in browser
gh repo view --web
```

## Output Format

When workflow completes successfully, provide:

| Field | Value |
|-------|-------|
| **Repository Name** | <name> |
| **URL** | https://github.com/<username>/<name> |
| **Visibility** | Public/Private |
| **Local Path** | <full-path-to-repo> |
