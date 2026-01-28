# Git Submodules
Git submodules let you **include one Git repository inside another as a subdirectory** while keeping their histories separate. The parent repository tracks which specific commit of the submodule to use.

## Common Use Cases
- Sharing code libraries across multiple projects
- Including third-party dependencies
- Managing monorepo components independently

## Basic Commands
Adding a Submodule
```bash
git submodule add <repository-url> <path>
git commit -m "Add submodule"
```
