# GitHub Tag and Release Workflow

## Overview
This document describes the complete workflow for creating Git tags and GitHub releases using the `gh` CLI tool.

---

## Table of Contents
1. [What are Tags and Releases?](#1-what-are-tags-and-releases)
2. [Basic Workflow](#2-basic-workflow)
3. [Creating Tags](#3-creating-tags)
4. [Creating Releases](#4-creating-releases)
5. [Uploading Assets](#5-uploading-assets)
6. [Managing Releases](#6-managing-releases)
7. [Managing Tags](#7-managing-tags)
8. [Version Numbering](#8-version-numbering)
9. [Complete Examples](#9-complete-examples)

---

## 1. What are Tags and Releases?

### Tags
- **Tag**: A reference to a specific point in Git history
- Used to mark release points (v1.0.0, v2.0.0, etc.)
- Two types: lightweight and annotated

### Releases
- **Release**: A Git tag plus additional information
- Includes release notes, binaries, and metadata
- Users can download assets from releases
- Marked as "Latest", "Pre-release", or "Draft"

---

## 2. Basic Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    Development Phase                         │
│  git commit → git commit → git commit                        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Create Tag                               │
│  git tag v1.0.0 -m "Release v1.0.0"                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Push Tag to Remote                       │
│  git push origin v1.0.0                                     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Create Release                           │
│  gh release create v1.0.0 --notes "Release notes"           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    (Optional) Upload Assets                 │
│  gh release upload v1.0.0 ./dist/*.zip                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Creating Tags

### View Existing Tags
```bash
git tag                          # List all tags
git tag -l "v1.*"               # List tags matching pattern
git show v1.0.0                  # Show tag details
```

### Create Tags

#### Lightweight Tag (not recommended)
```bash
git tag v1.0.0
```

#### Annotated Tag (recommended)
```bash
# Basic annotated tag
git tag v1.0.0 -m "Release v1.0.0"

# Detailed annotated tag
git tag -a v1.0.0 -m "Release v1.0.0

- Feature A complete
- Bug fixes for issues #123, #456
- Performance improvements"
```

#### Signed Tag (for security)
```bash
# Requires GPG key configured
git tag -s v1.0.0 -m "Signed release v1.0.0"
```

### Delete Tags
```bash
# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin :refs/tags/v1.0.0
# or
git push origin --delete v1.0.0
```

---

## 4. Creating Releases

### Method 1: Create Tag First, Then Release (Recommended)

```bash
# Step 1: Create local tag
git tag -a v1.0.0 -m "Release v1.0.0"

# Step 2: Push tag to remote
git push origin v1.0.0

# Step 3: Create release
gh release create v1.0.0 \
  --title "v1.0.0 - First Release" \
  --notes "Release notes here"
```

### Method 2: Auto-Create Tag with Release (Quick)

```bash
# Tag and release in one command
gh release create v1.0.0 --notes "Auto-generated release"

# Specify target branch
gh release create v1.0.0 --target develop

# Only create if tag exists (safety check)
gh release create v1.0.0 --verify-tag
```

### Release Creation Options

```bash
# Interactive (prompts for info)
gh release create

# With title and notes
gh release create v1.0.0 \
  --title "v1.0.0 - Version Title" \
  --notes "## What's New

- New feature X
- Bug fix Y
- Performance improvement Z"

# Use tag annotation as notes
gh release create v1.0.0 --notes-from-tag

# Auto-generate notes from commits
gh release create v1.0.0 --generate-notes

# Read notes from file
gh release create v1.0.0 -F RELEASE_NOTES.md

# Read notes from stdin
echo "Release notes" | gh release create v1.0.0 -F -

# Create as draft (edit before publishing)
gh release create v1.0.0 --draft

# Mark as pre-release
gh release create v1.0.0 --prerelease

# Don't mark as latest
gh release create v1.0.0 --latest=false

# Create release only if there are new commits
gh release create v1.0.0 --fail-on-no-commits
```

---

## 5. Uploading Assets

### Upload Files

```bash
# Upload single file
gh release create v1.0.0 ./dist/app.zip

# Upload multiple files
gh release create v1.0.0 ./dist/*.zip ./dist/*.tar.gz

# Upload with custom display label
gh release create v1.0.0 './build/app.exe#Windows Installer'

# Upload multiple files with labels
gh release create v1.0.0 \
  './dist/linux-binary#Linux 64-bit' \
  './dist/mac-binary#macOS ARM64' \
  './dist/windows-binary#Windows x64'

# Upload to existing release
gh release upload v1.0.0 ./new-file.zip

# Upload and replace existing asset
gh release upload v1.0.0 ./app.zip --clobber
```

### Download Assets

```bash
# Download all assets from release
gh release download v1.0.0

# Download to specific directory
gh release download v1.0.0 -D ./downloads

# Download specific asset
gh release download v1.0.0 -p "*.zip"

# Download archive
gh release download v1.0.0 --archive zip
```

---

## 6. Managing Releases

### View Releases
```bash
# List all releases
gh release list

# View specific release
gh release view v1.0.0

# View in browser
gh release view v1.0.0 --web

# View latest release
gh release view latest
```

### Edit Releases
```bash
# Edit title
gh release edit v1.0.0 --title "New Title"

# Edit notes
gh release edit v1.0.0 --notes "Updated release notes"

# Convert draft to published
gh release edit v1.0.0 --draft=false

# Mark as latest
gh release edit v1.0.0 --latest=true

# Add/remove pre-release flag
gh release edit v1.0.0 --prerelease=true
```

### Delete Releases
```bash
# Delete release (keeps tag)
gh release delete v1.0.0

# Delete release and tag
gh release delete v1.0.0 --yes
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0

# Delete specific asset
gh release delete-asset v1.0.0 app.zip
```

---

## 7. Managing Tags

### Push Tags
```bash
# Push single tag
git push origin v1.0.0

# Push all tags
git push origin --tags

# Push tags matching pattern
git push origin --tags 'v1.*'
```

### Fetch Tags
```bash
# Fetch all tags from remote
git fetch --tags

# Fetch specific tag
git fetch origin tag v1.0.0

# Checkout tag (view specific version)
git checkout v1.0.0

# Create branch from tag
git checkout -b hotfix-1.0 v1.0.0
```

### Compare Tags
```bash
# Compare two tags
git diff v1.0.0 v2.0.0

# Show commits between tags
git log v1.0.0..v2.0.0 --oneline

# Show changelog between tags
git log v1.0.0..v2.0.0 --pretty=format:"- %s"
```

---

## 8. Version Numbering

Follow [Semantic Versioning](https://semver.org/):

### Format
```
vMAJOR.MINOR.PATCH

MAJOR:    Incompatible API changes
MINOR:    Backwards-compatible functionality
PATCH:    Backwards-compatible bug fixes
```

### Examples
| Version | Type | Description |
|---------|------|-------------|
| `v1.0.0` | Stable | First stable release |
| `v1.0.1` | Patch | Bug fix |
| `v1.1.0` | Minor | New feature |
| `v2.0.0` | Major | Breaking changes |
| `v1.0.0-beta.1` | Pre-release | Beta version |
| `v1.0.0-rc.1` | Pre-release | Release candidate |
| `v1.0.0-alpha` | Pre-release | Alpha version |

### Pre-release Priority
```
1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-rc.1 < 1.0.0
```

---

## 9. Complete Examples

### Example 1: Basic Release
```bash
#!/bin/bash
VERSION="v1.0.0"

# Create and push tag
git tag -a $VERSION -m "Release $VERSION"
git push origin $VERSION

# Create release
gh release create $VERSION \
  --title "$VERSION - Initial Release" \
  --notes "First stable release"
```

### Example 2: Release with Assets
```bash
#!/bin/bash
VERSION="v1.0.0"

# Build project
cargo build --release

# Create tag
git tag -a $VERSION -m "Release $VERSION"
git push origin $VERSION

# Create release with assets
gh release create $VERSION \
  --title "$VERSION" \
  --notes "## Release Notes

- Feature X
- Bug fix Y

## Downloads

Choose the appropriate binary for your system." \
  ./target/release/myapp-linux#Linux 64-bit \
  ./target/release/myapp-macos#macOS \
  ./target/release/myapp.exe#Windows
```

### Example 3: Draft Release with Manual Publishing
```bash
#!/bin/bash
VERSION="v1.0.0"

# Create as draft first
gh release create $VERSION \
  --title "$VERSION - Draft" \
  --notes "Draft release for review" \
  --draft

# Upload assets
gh release upload $VERSION ./dist/*

# Review in browser
gh release view $VERSION --web

# When ready, publish
gh release edit $VERSION --draft=false
```

### Example 4: Automated Release from CI
```bash
#!/bin/bash
# .github/scripts/release.sh

VERSION=$1

# Validate version
if ! [[ $VERSION =~ ^v[0-9]+\.[0-9]+\.[0-9]+$ ]]; then
  echo "Invalid version format. Use vX.Y.Z"
  exit 1
fi

# Check tag doesn't exist
if git rev-parse $VERSION >/dev/null 2>&1; then
  echo "Tag $VERSION already exists"
  exit 1
fi

# Create tag
git tag -a $VERSION -m "Release $VERSION"
git push origin $VERSION

# Generate release notes
gh release create $VERSION \
  --generate-notes \
  --title "$VERSION"

# Upload build artifacts
gh release upload $VERSION ./build/*
```

### Example 5: Pre-release Version
```bash
#!/bin/bash
VERSION="v1.0.0-beta.1"

# Create pre-release
git tag -a $VERSION -m "Beta release $VERSION"
git push origin $VERSION

gh release create $VERSION \
  --title "$VERSION - Beta" \
  --notes "## Beta Release

This is a pre-release version for testing.

**Please report any issues!**" \
  --prerelease
```

### Example 6: Release with Discussion
```bash
#!/bin/bash
VERSION="v2.0.0"

gh release create $VERSION \
  --title "$VERSION - Major Update" \
  --notes "Major release with breaking changes" \
  --discussion-category "Announcements"
```

---

## Quick Reference

| Command | Description |
|---------|-------------|
| `git tag v1.0.0 -m "msg"` | Create annotated tag |
| `git push origin v1.0.0` | Push tag to remote |
| `gh release create v1.0.0` | Create release |
| `gh release list` | List releases |
| `gh release view v1.0.0` | View release |
| `gh release edit v1.0.0` | Edit release |
| `gh release delete v1.0.0` | Delete release |
| `gh release download v1.0.0` | Download assets |
| `gh release upload v1.0.0 file` | Upload asset |

---

## Best Practices

1. **Always use annotated tags** for releases (they include metadata)
2. **Follow semantic versioning** (vX.Y.Z)
3. **Write meaningful release notes** - users rely on them
4. **Use draft releases** for review before publishing
5. **Tag important milestones** - not just every commit
6. **Sign tags** with GPG for security-critical projects
7. **Include binaries** in releases for easy user access
8. **Use pre-release** for beta/alpha versions
9. **Keep release notes consistent** in format
10. **Test the tag** before creating the release

---

## Troubleshooting

### Tag Already Exists
```bash
# Delete and recreate
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0
git tag -a v1.0.0 -m "New message"
git push origin v1.0.0
```

### Release Creation Fails
```bash
# Check if tag exists remotely
git ls-remote --tags origin

# Verify tag locally
git show v1.0.0

# Use --verify-tag flag
gh release create v1.0.0 --verify-tag
```

### Asset Upload Fails
```bash
# Check file size limit (GitHub: 2GB per file)
ls -lh ./dist/file.zip

# Use --clobber to overwrite
gh release upload v1.0.0 ./file.zip --clobber
```

---

## Document Version
- **Version**: 1.0
- **Last Updated**: 2026-01-21
- **gh Version**: 2.83.2
- **Example Release**: https://github.com/Clearzero22/rust_test_workflow_project/releases/tag/v1.0.0
