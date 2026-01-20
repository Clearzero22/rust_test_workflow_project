# GitHub Actions Workflow Documentation

## Overview
This document describes how to manage GitHub Actions workflows using the `gh` CLI tool.

---

## Table of Contents
1. [What is GitHub Actions?](#1-what-is-github-actions)
2. [Workflow Basics](#2-workflow-basics)
3. [Managing Workflows](#3-managing-workflows)
4. [Managing Workflow Runs](#4-managing-workflow-runs)
5. [Managing Caches](#5-managing-caches)
6. [Workflow Examples](#6-workflow-examples)
7. [Best Practices](#7-best-practices)

---

## 1. What is GitHub Actions?

GitHub Actions is a CI/CD (Continuous Integration/Continuous Deployment) platform that allows you to:

- **Automate builds, tests, and deployments**
- **Run workflows on specific events** (push, PR, schedule, etc.)
- **Use actions** from GitHub Marketplace
- **Cache dependencies** for faster builds
- **Deploy** to various platforms

---

## 2. Workflow Basics

### Workflow File Location
```
.github/
└── workflows/
    ├── workflow1.yml
    ├── workflow2.yml
    └── workflow3.yml
```

### Basic Workflow Structure
```yaml
name: Workflow Name

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:  # Enable manual trigger

jobs:
  job-name:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello World"
```

---

## 3. Managing Workflows

### List Workflows
```bash
gh workflow list
```
**Example Output:**
```
Cache Demo	active	225377039
Hello World	active	225376266
```

### View Workflow Details
```bash
# View summary
gh workflow view <workflow-name>

# View YAML content
gh workflow view <workflow-name> --yaml

# View specific workflow
gh workflow view hello.yml
```

### Enable/Disable Workflows
```bash
# Disable a workflow
gh workflow disable <workflow-name>

# Enable a workflow
gh workflow enable <workflow-name>
```

### Run Workflow Manually
```bash
# Trigger workflow_dispatch event
gh workflow run <workflow-name>

# Run with specific branch
gh workflow run <workflow-name --ref develop

# Run with inputs (if workflow defines inputs)
gh workflow run <workflow-name -f input1=value1 -f input2=value2
```

---

## 4. Managing Workflow Runs

### List Runs
```bash
# List all recent runs
gh run list

# List with limit
gh run list --limit 10

# List runs for specific workflow
gh run list --workflow hello.yml

# List by branch
gh run list --branch main

# List by status
gh run list --status failure
gh run list --status success
gh run list --status pending
```
**Columns:** Status | Workflow | Event | Branch | Triggered | Run ID | Duration | Date

### View Run Details
```bash
# View run summary
gh run view <run-id>

# View in browser
gh run view <run-id> --web

# View job details
gh run view <run-id> --job <job-id>

# View logs
gh run view <run-id> --log

# View log for specific job
gh run view --job <job-id> --log
```

### Watch Run Progress
```bash
# Watch until completion
gh run watch <run-id>

# Watch with interval
gh run watch <run-id> --interval 5
```

### Rerun Runs
```bash
# Rerun failed run
gh run rerun <run-id>

# Rerun all jobs
gh run rerun <run-id> --failed
```

### Cancel Runs
```bash
# Cancel running workflow
gh run cancel <run-id>
```

### Delete Runs
```bash
# Delete run
gh run delete <run-id>

# Delete multiple runs
gh run delete <run-id1> <run-id2>
```

### Download Artifacts
```bash
# Download all artifacts
gh run download <run-id>

# Download to specific directory
gh run download <run-id> -D ./artifacts

# Download specific artifact
gh run download <run-id> -n artifact-name

# Download matching pattern
gh run download <run-id> -p "*.zip"
```

---

## 5. Managing Caches

GitHub Actions cache stores dependencies between workflow runs to speed up builds.

### List Caches
```bash
gh cache list
```
**Example Output:**
```
ID      Key                Size    Created At          Last Used
2546628764  my-cache-21181564209  329 B  2026-01-20T17:44:26Z  2026-01-20T17:44:26Z
```

### Delete Caches
```bash
# Delete specific cache
gh cache delete <cache-id>

# Delete all caches
gh cache delete --all
```

### Cache in Workflow
```yaml
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: |
      ~/.cargo/registry
      ~/.cargo/git
      target
    key: ${{ runner.os }}-cargo-${{ hashFiles('**/Cargo.lock') }}
    restore-keys: |
      ${{ runner.os }}-cargo-
```

---

## 6. Workflow Examples

### Example 1: Basic CI
```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run tests
        run: |
          echo "Running tests..."
          # Add your test commands here
```

### Example 2: Rust Project CI
```yaml
name: Rust CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Cache cargo registry
        uses: actions/cache@v4
        with:
          path: ~/.cargo/registry
          key: ${{ runner.os }}-cargo-registry-${{ hashFiles('**/Cargo.lock') }}

      - name: Cache cargo index
        uses: actions/cache@v4
        with:
          path: ~/.cargo/git
          key: ${{ runner.os }}-cargo-index-${{ hashFiles('**/Cargo.lock') }}

      - name: Cache cargo build
        uses: actions/cache@v4
        with:
          path: target
          key: ${{ runner.os }}-cargo-build-target-${{ hashFiles('**/Cargo.lock') }}

      - name: Build
        run: cargo build --verbose

      - name: Run tests
        run: cargo test --verbose

      - name: Run clippy
        run: cargo clippy -- -D warnings

      - name: Check formatting
        run: cargo fmt -- --check
```

### Example 3: Deploy to GitHub Pages
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      pages: write
      id-token: write

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './public'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### Example 4: Create Release on Tag
```yaml
name: Create Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest

    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v4

      - name: Build release
        run: |
          cargo build --release
          mkdir dist
          cp target/release/myapp dist/

      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          files: dist/*
          generate_release_notes: true
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Example 5: Scheduled Workflow
```yaml
name: Daily Backup

on:
  schedule:
    - cron: '0 2 * * *'  # Runs at 2 AM UTC daily
  workflow_dispatch:

jobs:
  backup:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Create backup
        run: |
          echo "Creating backup..."
          # Add backup commands here

      - name: Upload backup
        uses: actions/upload-artifact@v4
        with:
          name: backup-${{ github.run_number }}
          path: backup/
          retention-days: 30
```

### Example 6: Matrix Build
```yaml
name: Test Matrix

on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        rust: [stable, beta, nightly]

    steps:
      - uses: actions/checkout@v4

      - name: Install Rust
        uses: dtolnay/rust-toolchain@master
        with:
          toolchain: ${{ matrix.rust }}

      - name: Build
        run: cargo build --verbose

      - name: Test
        run: cargo test --verbose
```

### Example 7: Docker Build and Push
```yaml
name: Docker

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

jobs:
  docker:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      packages: write

    steps:
      - uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:${{ github.ref_name }}
```

---

## 7. Best Practices

### Workflow Design
1. **Use specific triggers** - Only run when needed
2. **Cache dependencies** - Speed up builds
3. **Use matrix builds** - Test across multiple configurations
4. **Set timeouts** - Prevent hanging jobs
5. **Use environments** - Separate staging, production

### Security
1. **Use secrets** - Never hardcode credentials
2. **Pin action versions** - Use `@v4` instead of `@main`
3. **Limit permissions** - Only grant what's needed
4. **Review third-party actions** - Check source code

### Performance
1. **Use caching** - Cache dependencies and build artifacts
2. **Parallel jobs** - Run independent jobs simultaneously
3. **Conditional steps** - Skip unnecessary steps
4. **Use artifacts wisely** - Clean up old artifacts

### Monitoring
1. **Set up notifications** - Get alerts on failures
2. **Use status badges** - Show build status in README
3. **Review logs** - Debug failed runs
4. **Track duration** - Optimize slow workflows

---

## Quick Reference

| Command | Description |
|---------|-------------|
| `gh workflow list` | List all workflows |
| `gh workflow view <name>` | View workflow details |
| `gh workflow run <name>` | Trigger workflow manually |
| `gh workflow enable <name>` | Enable workflow |
| `gh workflow disable <name>` | Disable workflow |
| `gh run list` | List recent runs |
| `gh run view <id>` | View run details |
| `gh run view <id> --log` | View run logs |
| `gh run watch <id>` | Watch run progress |
| `gh run rerun <id>` | Rerun failed run |
| `gh run cancel <id>` | Cancel running run |
| `gh run download <id>` | Download artifacts |
| `gh cache list` | List caches |
| `gh cache delete <id>` | Delete cache |
| `gh cache delete --all` | Delete all caches |

---

## Common Issues

### Workflow Not Triggering
- Check trigger conditions (branch, event)
- Verify workflow file is in `.github/workflows/`
- Check syntax errors in YAML

### Cache Not Working
- Verify cache key matches
- Check cache path is correct
- Ensure cache isn't expired (7-day default)

### Secrets Not Available
- Check secret name matches
- Verify secret is set at correct level (repo/org)
- Ensure workflow has permissions

---

## Document Version
- **Version**: 1.0
- **Last Updated**: 2026-01-21
- **gh Version**: 2.83.2
- **Example Workflows**: https://github.com/Clearzero22/rust_test_workflow_project/tree/main/.github/workflows
