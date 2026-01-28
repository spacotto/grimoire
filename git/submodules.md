# Git Submodules
Git submodules let you **include one Git repository inside another as a subdirectory** while keeping their histories separate. The parent repository tracks which specific commit of the submodule to use.

## Common Use Cases
- Sharing code libraries across multiple projects
- Including third-party dependencies
- Managing monorepo components independently

## Basic Commands
Adding a Submodule:
```bash
git submodule add <repository-url> <path>
git commit -m "Add submodule"
```

Cloning a Repository with Submodules:
```bash
# Option 1: Clone and initialize in one step
git clone --recursive <repository-url>

# Option 2: Initialize after cloning
git clone <repository-url>
git submodule init
git submodule update
```

Updating Submodules:
```bash
# Update to latest commit on tracked branch
cd <submodule-path>
git pull origin main

# Or update all submodules at once
git submodule update --remote
```

Removing a Submodule:
```bash
git submodule deinit <path>
git rm <path>
git commit -m "Remove submodule"
```

## How Submodules Work
When you add a submodule, Git creates:

- A `.gitmodules` file in your repository root (tracks submodule URLs)
- An entry in `.git/config`
- A special commit reference (not the actual files)

The parent repository only stores a pointer to a specific commit in the submodule, not the submodule's files.
