# Publishing Guide - Tab Limiter

This document describes the step-by-step process for publishing a new version of the Tab Limiter extension to the Chrome Web Store.

## Prerequisites

- Developer account on [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/developer/dashboard)
- One-time developer registration fee paid ($5 USD)
- Extension source code prepared and tested

## Step by Step

### 1. Prepare the Extension

1. **Version Management**
   
   **Automated (via GitHub Release):**
   - Version is automatically synchronized with your release tag
   - Create a release with tag like `v0.3.5` or `v1.0.0`
   - The workflow will:
     - Check if manifest.json needs updating
     - Only update and commit if version differs
     - Skip commit if version is already correct
   
   **Manual:**
   - Update version in manifest.json before creating release
   ```json
   "version": "X.Y.Z"
   ```
   - X: Major version (significant changes)
   - Y: Minor version (new features)
   - Z: Patch (bug fixes)

2. **Test locally**
   - Open `chrome://extensions/`
   - Enable "Developer mode"
   - Click "Load unpacked"
   - Select the extension folder
   - Test all features thoroughly

### 2. Create ZIP Package

#### Automated Method (Recommended)

**Using GitHub Releases:**
1. Go to your repository on GitHub
2. Click on "Releases" → "Create a new release"
3. Choose a tag with version (e.g., `v0.3.5`, `v1.0.0`)
   - The 'v' prefix is optional and will be removed automatically
4. Fill in release title and description
5. Click "Publish release"
6. GitHub Actions will automatically:
   - Extract version from tag (removes 'v' prefix if present)
   - Check if manifest.json needs updating:
     - If version differs: Updates manifest.json and commits the change
     - If version matches: Skips the commit (no unnecessary changes)
   - Create the ZIP file with the correct version
   - Calculate SHA256 checksum for integrity
   - Attach both files to the release
   - Add a comment indicating what was done
7. Download the ZIP from release assets

**Smart Version Management:**
- The workflow only commits when necessary
- Prevents duplicate commits when version is already correct
- Release comment shows whether version was updated or already correct

#### Manual Methods

**Required files:**
- `manifest.json`
- `background.js`
- `options.html`
- `options.js`
- `options.css`
- `icons/48.png`
- `icons/128.png`
- Any other files referenced in the manifest

**Create the ZIP:**
   
   **Option A: Using system tools**
   
   On Linux/WSL without zip installed:
   ```bash
   # Install zip first
   sudo apt-get update && sudo apt-get install zip
   # Then create the ZIP
   zip -r tab-limiter.zip . -x ".*" -x "docs/*" -x "*.md" -x "node_modules/*" -x "*.git*"
   ```
   
   On Windows (PowerShell):
   ```powershell
   # Create a temporary folder and copy files
   $files = @("manifest.json", "background.js", "options.html", "options.js", "options.css", "icons")
   Compress-Archive -Path $files -DestinationPath tab-limiter.zip
   ```
   
   On macOS:
   ```bash
   # From project root
   zip -r tab-limiter.zip . -x ".*" -x "docs/*" -x "*.md" -x "node_modules/*" -x "*.git*"
   ```
   
   **Option B: Using Node.js/npm**
   ```bash
   # Install a zip utility
   npm install -g bestzip
   # Create the ZIP
   bestzip tab-limiter.zip manifest.json background.js options.html options.js options.css icons/
   ```
   
   **Option C: Manual compression**
   - Open file explorer
   - Select all required files and folders:
     - manifest.json
     - background.js
     - options.html
     - options.js
     - options.css
     - icons/ folder (with all contents)
   - Right-click and choose "Compress" or "Send to > Compressed folder"
   - Rename to: `tab-limiter-vX.Y.Z.zip`
   
   **Important**: Do NOT include:
   - Hidden files/folders (starting with .)
   - docs/ folder
   - README or other .md files
   - node_modules/ folder
   - .git/ folder
   - Any development/build files

### 3. Publish to Chrome Web Store

1. **Access the Dashboard**
   - Go to [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/developer/dashboard)
   - Sign in with your Google account

2. **Create new item or update existing**
   
   **For first publication:**
   - Click "New item"
   - Upload the ZIP file
   
   **For updates:**
   - Find the extension in the list
   - Click on the extension name
   - Go to "Package" in the sidebar
   - Click "Upload new package"
   - Select the ZIP file

3. **Fill in store listing information**
   
   **Basic information:**
   - **Name**: Tab Limiter
   - **Short description** (132 characters max): Limits the number of open tabs in Chrome
   - **Detailed description**: Explain features, benefits, and how to use
   - **Category**: Productivity
   - **Language**: English

   **Visual assets:**
   - **Store icon**: 128x128 pixels (PNG)
   - **Screenshots**: Minimum 1, maximum 5 (1280x800 or 640x400)
   - **Small promotional tile**: 440x280 pixels (optional)
   - **Large promotional tile**: 1400x560 pixels (optional)
   - **YouTube video**: Demonstration video URL (optional)

4. **Privacy settings**
   - Declare permissions used
   - Justify the use of each permission
   - Privacy policy (if applicable)

5. **Distribution settings**
   - **Visibility**: Public or Unlisted
   - **Countries**: All countries or specific ones
   - **Users**: Everyone or specific group

### 4. Review and Publication

1. **Review all information**
   - Check spelling and grammar
   - Confirm screenshots are correct
   - Test the ZIP file one last time

2. **Submit for review**
   - Click "Submit for review"
   - Wait for the review process (usually 1-3 business days)

3. **Track status**
   - You'll receive email about the status
   - Possible statuses:
     - **Pending**: Under review
     - **Published**: Approved and available
     - **Rejected**: Requires fixes

### 5. After Publication

1. **Monitor feedback**
   - Respond to user reviews
   - Track bug reports
   - Monitor installation statistics

2. **Future updates**
   - Version is managed automatically via release tags
   - Test extensively before creating a release
   - Maintain a changelog of changes
   - Use semantic versioning (major.minor.patch)

## Important Tips

- **Manifest Version**: Currently using Manifest V3
- **Testing**: Always test in incognito mode and with different configurations
- **Backups**: Keep backups of previous versions
- **Communication**: Inform users about significant changes
- **Compliance**: Follow the [developer program policies](https://developer.chrome.com/docs/webstore/program-policies/)

## Troubleshooting

### Extension rejected
- Carefully read the rejection reason
- Fix the issues mentioned
- Resubmit for review

### Permissions questioned
- Clearly justify each permission
- Remove unnecessary permissions
- Use optional permissions when possible

### Manifest issues
- Validate the JSON
- Check Manifest V3 compatibility
- Consult the [official documentation](https://developer.chrome.com/docs/extensions/mv3/)

## Useful Links

- [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/developer/dashboard)
- [Chrome Extensions Documentation](https://developer.chrome.com/docs/extensions/)
- [Manifest V3](https://developer.chrome.com/docs/extensions/mv3/)
- [Chrome Web Store Policies](https://developer.chrome.com/docs/webstore/program-policies/)
- [Best Practices](https://developer.chrome.com/docs/webstore/best_practices/)