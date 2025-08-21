# GitHub Actions Workflows

This directory contains automated workflows for the Tab Limiter extension.

## Workflows

### 1. Release Workflow (`release.yml`)

**Trigger:** When a new release is created on GitHub

**Actions:**
- Extracts version from the release tag (e.g., `v0.3.5` → `0.3.5`)
- Checks if manifest.json needs updating (only commits if necessary)
- Updates manifest.json with the new version (if needed)
- Commits the version change back to the repository (if needed)
- Creates a ZIP package of the extension with updated version
- Excludes all unnecessary files (docs, git files, node_modules, etc.)
- Generates SHA256 checksum for integrity verification
- Attaches both files to the release
- Displays build summary in workflow logs

**How to use:**
1. Go to the repository's "Releases" page
2. Click "Create a new release"
3. Create a new tag with version (e.g., `v0.3.5`, `v1.0.0`)
   - The 'v' prefix is optional and will be removed
4. Fill in release notes
5. Publish the release
6. Wait for the workflow to complete (~1-2 minutes)
7. The workflow will:
   - Update manifest.json to match your tag version
   - Commit the change with message "chore: bump version to X.Y.Z"
   - Create and attach the ZIP file to the release
8. Download the ZIP from release assets

**Version Synchronization:**
- The manifest.json version is automatically kept in sync with your Git tags
- No need to manually update version before releases
- Ensures consistency between Git tags and extension version

### 2. Validation Workflow (`validate.yml`)

**Trigger:** On every push to master/main and on pull requests

**Actions:**
- Validates manifest.json syntax
- Checks for required extension files
- Verifies Manifest V3 compatibility
- Tests ZIP creation process
- Checks package size constraints

**Purpose:**
- Ensures the extension is always in a publishable state
- Catches issues before they reach the main branch
- Validates PR submissions

## Required Secrets

No additional secrets needed. Workflows use the default `GITHUB_TOKEN`.

## Files Included in Release ZIP

The release workflow includes only essential files:
- `manifest.json`
- `background.js`
- `options.html`
- `options.js`
- `options.css`
- `icons/` directory (48.png, 128.png)
- Any other files referenced in manifest.json

## Files Excluded from Release ZIP

The following are automatically excluded:
- `.git/` and `.github/` directories
- `docs/` directory
- `*.md` files (README, etc.)
- `node_modules/` directory
- Package files (package.json, lock files)
- Development files (.env, .eslintrc, etc.)
- Test files (*.test.js, *.spec.js)
- Build artifacts and logs
- OS-specific files (.DS_Store, Thumbs.db)

## Troubleshooting

### Release workflow not triggering
- Ensure you're creating a release, not just a tag
- Check Actions tab for any disabled workflows

### ZIP file not appearing
- Check the Actions tab for workflow errors
- Ensure all required files exist in the repository

### Validation failures
- Review the workflow output for specific errors
- Ensure manifest.json is valid JSON
- Verify all required icon files are present