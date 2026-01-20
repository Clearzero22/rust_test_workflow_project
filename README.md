# 🚀 GitHub CLI Complete Workflow Guide

> A comprehensive reference for mastering GitHub CLI (`gh`) and Git workflows 📚

[![gh version](https://img.shields.io/badge/gh-2.83.2-blue)](https://github.com/cli/cli)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/Clearzero22/rust_test_workflow_project?style=social)](https://github.com/Clearzero22/rust_test_workflow_project)

---

## ✨ Features

This repository contains **complete workflow documentation** for GitHub CLI, covering everything from basic operations to advanced workflows.

### 📖 Documentation

| Document | Description | Link |
|----------|-------------|------|
| **GitHub CLI Reference** | Complete `gh` command reference | [WORKFLOW_CREATE_GITHUB_PROJECT.md](WORKFLOW_CREATE_GITHUB_PROJECT.md) |
| **Tag & Release Guide** | Git tags and GitHub releases | [WORKFLOW_TAG_AND_RELEASE.md](WORKFLOW_TAG_AND_RELEASE.md) |
| **GitHub Actions** | CI/CD workflow management | [WORKFLOW_GITHUB_ACTIONS.md](WORKFLOW_GITHUB_ACTIONS.md) |
| **Merge Strategies** | Git merge types explained | [MERGE_STRATEGIES.md](MERGE_STRATEGIES.md) |
| **PR Examples** | Pull request examples | [PR_EXAMPLES.md](PR_EXAMPLES.md) |
| **Git Push Guide** | Git push commands | [GIT_PUSH_GUIDE.md](GIT_PUSH_GUIDE.md) |

---

## 🎯 What You'll Learn

### 🔐 Authentication
- Login and logout
- Check authentication status
- Manage tokens

### 📦 Repository Operations
- Create repositories
- Clone and fork
- View and delete

### 🔀 Pull Requests
- Create and checkout PRs
- Review and merge
- Close and reopen

### 🐛 Issues
- Create and manage issues
- Add comments
- Close and reopen

### 🏷️ Tags & Releases
- Create annotated tags
- Publish releases
- Upload assets

### ⚡ GitHub Actions
- Manage workflows
- View runs and logs
- Handle artifacts
- Cache management

---

## 🚀 Quick Start

### Installation

```bash
# On macOS
brew install gh

# On Windows
winget install --id GitHub.cli

# On Linux
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
```

### Authenticate

```bash
gh auth login
```

### Create a Repository

```bash
gh repo create my-project --public --add-readme --clone
```

---

## 📚 Common Commands

### Repository
```bash
gh repo list                              # List repositories
gh repo create <name> --public --clone    # Create & clone
gh repo view                              # View repo info
```

### Pull Requests
```bash
gh pr list                                # List PRs
gh pr create --title "Feature" --body "..." # Create PR
gh pr merge 123 --squash                  # Merge PR
```

### Issues
```bash
gh issue list                             # List issues
gh issue create --title "Bug"             # Create issue
gh issue close 123                        # Close issue
```

### Tags & Releases
```bash
git tag v1.0.0 -m "Release"               # Create tag
git push origin v1.0.0                    # Push tag
gh release create v1.0.0 --notes "..."    # Create release
```

### GitHub Actions
```bash
gh workflow list                          # List workflows
gh run list                               # List runs
gh run view <id> --log                    # View logs
```

---

## 🎓 Examples

### Complete Workflow

```bash
# 1. Create repository
gh repo create my-project --public --clone

# 2. Initialize project
cd my-project
cargo init  # or npm init, etc.

# 3. Make changes
git add .
git commit -m "Initial commit"
git push

# 4. Create feature branch
git checkout -b feature/new-feature
# ... make changes ...
git commit -am "Add new feature"
git push -u origin feature/new-feature

# 5. Create Pull Request
gh pr create --title "Add new feature" --body "Implements #123"

# 6. Merge PR
gh pr merge 1 --squash --delete-branch

# 7. Create Release
git tag v1.0.0 -m "First release"
git push origin v1.0.0
gh release create v1.0.0 --notes "First stable release"
```

---

## 📖 Workflow Guides

### 🔄 Merge Strategies

| Strategy | Description | When to Use |
|----------|-------------|-------------|
| **Squash** | 🎯 Compress all commits into one | Most cases |
| **Merge** | 🔀 Preserve full history | Need complete history |
| **Rebase** | 📿 Linear history | Clean history |

See [MERGE_STRATEGIES.md](MERGE_STRATEGIES.md) for details.

### ⚡ GitHub Actions

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: cargo test
```

See [WORKFLOW_GITHUB_ACTIONS.md](WORKFLOW_GITHUB_ACTIONS.md) for more.

---

## 🛠️ GitHub Actions Workflows

This repository includes example workflows:

| Workflow | Description |
|----------|-------------|
| **hello.yml** | Basic "Hello World" workflow |
| **cache-demo.yml** | Demonstrates caching |

---

## 📊 Project Stats

- 📁 **Documentation Files**: 6
- 🔄 **Workflows**: 2
- 🏷️ **Releases**: [v1.0.0](https://github.com/Clearzero22/rust_test_workflow_project/releases/tag/v1.0.0)
- 📦 **Artifacts**: Automated builds

---

## 🔗 Useful Links

- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Semantic Versioning](https://semver.org/)
- [Conventional Commits](https://www.conventionalcommits.org/)

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

1. 🍴 Fork the repository
2. 🔀 Create a feature branch
3. 📝 Make your changes
4. ✅ Commit your changes
5. 📤 Push to the branch
6. 🔄 Create a Pull Request

---

## 💡 Tips

### 💬 Use Interactive Mode
```bash
gh pr create    # Will prompt for details
gh issue create  # Interactive creation
```

### 🌐 Open in Browser
```bash
gh pr view --web
gh browse --settings
```

### 📋 JSON Output
```bash
gh repo list --json name,visibility
gh pr list --json number,title
```

### 🔍 Search
```bash
gh search repos topic:rust
gh search issues author:@me
```

---

<div align="center">

**Made with ❤️ using GitHub CLI**

[⬆ Back to Top](#-github-cli-complete-workflow-guide)

</div>
