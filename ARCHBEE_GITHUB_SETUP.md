# Archbee GitHub Integration - Step by Step Guide

This guide follows the official [Archbee GitHub Integration](https://www.archbee.com/docs/set-up-the-repository) documentation to integrate your Xandeum Pod documentation with Archbee via GitHub.

## Prerequisites

- ✅ GitHub repository with your documentation
- ✅ Archbee account ([sign up here](https://archbee.com))
- ✅ Markdown source files (not just built HTML)

## Step-by-Step Setup

### Step 1: Prepare Your Repository Structure

Your repository should have the following structure:

```
your-repo/
├── archbee.yaml          # Archbee configuration (created for you)
├── summary.md            # Navigation structure (created for you)
├── docs/                 # Your markdown documentation
│   ├── index.md
│   ├── rpc-api.md
│   └── cli.md
└── assets/              # Images and other assets
    └── images/
        └── XandeumLogoStandard.png
```

**Important Notes:**

1. **If you only have HTML files**: You need to convert them back to Markdown or locate your original Markdown source files. The built HTML site in this directory won't work directly with Archbee.

2. **Source Repository**: If your docs are built from a different repository (like using MkDocs), you need to integrate that source repository with Archbee, not this built site.

### Step 2: Add Configuration Files to Your Repository

Two configuration files have been created for you:

#### 1. `archbee.yaml` (main configuration)

This file tells Archbee:
- Where your docs are (`docspath: /docs`)
- Where the navigation structure is (`summary.md`)
- Where assets are stored (`assets: assets`)
- To auto-publish after commits (`publishspace: true`)

#### 2. `summary.md` (navigation structure)

This file defines how your documentation appears in the left sidebar navigation in Archbee.

**Action Required**: 
```bash
# Copy these files to your source repository root
cp archbee.yaml /path/to/your/source/repo/
cp summary.md /path/to/your/source/repo/
```

### Step 3: Connect GitHub to Archbee

1. **Create or log into your Archbee account**
   - Go to [https://app.archbee.com](https://app.archbee.com)
   - Sign up or log in

2. **Connect GitHub Integration**
   - In Archbee, click on your profile/settings
   - Go to **Integrations** → **GitHub**
   - Click **"Connect GitHub"**
   - Authorize Archbee to access your GitHub account

3. **Configure Repository Access**
   - In your GitHub account, go to **Settings** → **Applications**
   - Find **Archbee** in the list
   - Click **"Configure"**
   - Choose one of:
     - **All repositories** (Archbee can access all your repos)
     - **Only select repositories** (choose specific repos)
   - Select your documentation repository
   - Click **"Save"**

### Step 4: Create a New Space in Archbee

1. **Create Space**
   - In Archbee, click **"+ New Space"**
   - Choose **"Write in GitHub"**
   - Select your documentation repository from the dropdown

2. **Configure the Space**
   - **Space Name**: "Xandeum Pod Documentation"
   - **Repository**: `your-username/your-repo`
   - **Branch**: `main` (or `master`, depending on your setup)
   - **Path**: `/` (root, since `archbee.yaml` is in root)
     - If using multiple folders, specify the folder path

3. **Advanced Settings**
   - **Auto-sync**: Enable (syncs on every commit)
   - **Sync frequency**: On push (immediate)
   - **File format**: Markdown

4. **Click "Create Space"**

### Step 5: Initial Sync

1. Archbee will now sync your repository
2. It will:
   - Read `archbee.yaml` configuration
   - Parse `summary.md` for navigation structure
   - Import all markdown files from your `docs/` folder
   - Upload assets from your `assets/` folder
   - Build the documentation structure

3. Wait for sync to complete (usually 1-2 minutes)

### Step 6: Verify and Customize

1. **Check Your Space**
   - Navigate through your documentation in Archbee
   - Verify all pages are imported correctly
   - Check that navigation matches your `summary.md`

2. **Customize Appearance**
   - Go to **Space Settings** → **Branding**
   - Upload your logo: `assets/images/XandeumLogoStandard.png`
   - Set brand colors
   - Configure theme (light/dark mode)

3. **Configure Domain** (Optional)
   - Go to **Space Settings** → **Custom Domain**
   - Add your domain: `docs.xandeum.com`
   - Follow DNS setup instructions:
     ```
     Type: CNAME
     Name: docs
     Value: custom.archbee.com
     TTL: Auto
     ```
   - Wait for DNS propagation (5-60 minutes)
   - SSL certificate is automatic

### Step 7: Enable Auto-Publishing

Since you set `publishspace: true` in `archbee.yaml`, your space will automatically publish to production after each commit.

To verify:
1. Go to **Space Settings** → **General**
2. Check that **"Auto-publish on sync"** is enabled
3. Your production URL will be: `https://your-space.archbee.com`

## Using Multiple Folders/Repositories

If you want to sync multiple folders or repositories:

### Option 1: Multiple Folders in Same Repo

1. Create `archbee.yaml` in each folder:
   ```
   repo-root/
   ├── folder1/
   │   ├── archbee.yaml
   │   ├── summary.md
   │   └── docs/
   └── folder2/
       ├── archbee.yaml
       ├── summary.md
       └── docs/
   ```

2. When creating space in Archbee:
   - **Path**: `/folder1` or `/folder2`

### Option 2: Multiple Repositories

1. Repeat Steps 3-6 for each repository
2. Each repository gets its own space in Archbee
3. You can link spaces together using space links

## Editing and Syncing

### Automatic Sync (Recommended)

With auto-sync enabled:

1. **Edit in your repository**
   ```bash
   # Make changes to your markdown files
   vim docs/rpc-api.md
   
   # Commit and push
   git add .
   git commit -m "Update API documentation"
   git push origin main
   ```

2. **Archbee syncs automatically**
   - Webhook triggers on push
   - Changes appear in Archbee within seconds
   - If `publishspace: true`, changes go live immediately

### Manual Sync

If you need to trigger a manual sync:

1. Go to your space in Archbee
2. Click **Settings** → **GitHub Integration**
3. Click **"Sync Now"**

### Two-Way Sync

Archbee supports two-way sync (edit in Archbee, sync back to GitHub):

1. Enable in **Space Settings** → **GitHub Integration**
2. Toggle **"Two-way sync"** on
3. Choose sync behavior:
   - **Commit to same branch**: Direct commits
   - **Create pull requests**: For review workflow

## Summary.md Syntax

The `summary.md` file uses simple Markdown link syntax:

```markdown
# Documentation Title

[Page Title](path/to/file.md)

## Section Title

[Another Page](another-page.md)
[Page with Anchor](page.md#section)

### Subsection

[Nested Page](nested/page.md)
```

**Rules:**
- Use standard Markdown links: `[Title](path.md)`
- Paths are relative to `docspath` in `archbee.yaml`
- Headers (`#`, `##`, `###`) create hierarchy
- Order in `summary.md` = order in navigation

## Customization Options

### Custom CSS

Edit `archbee.yaml`:

```yaml
customcss: |
  :root {
    --primary-color: #4f46e5;
    --accent-color: #06b6d4;
  }
  
  .header {
    background-color: var(--primary-color);
  }
```

### Custom JavaScript

```yaml
customjs: |
  // Analytics
  (function() {
    // Your custom JS here
  })();
```

### Header Includes

For analytics, meta tags, etc.:

```yaml
headerincludes: |
  <script async src="https://www.googletagmanager.com/gtag/js?id=GA_TRACKING_ID"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'GA_TRACKING_ID');
  </script>
```

## Troubleshooting

### Issue: "archbee.yaml not found"

**Solution:**
- Ensure `archbee.yaml` is in the root of your repository
- If using multiple folders, copy to each folder
- Check file name is exactly `archbee.yaml` (not `archbee.yml`)

### Issue: "No documents imported"

**Solution:**
- Check `docspath` in `archbee.yaml` points to correct folder
- Ensure markdown files have `.md` extension
- Verify files are committed and pushed to GitHub
- Check sync logs in Archbee for errors

### Issue: "Navigation not showing correctly"

**Solution:**
- Verify `summary.md` syntax is correct
- Check that paths in `summary.md` match actual file paths
- Ensure paths are relative to `docspath`
- Look at [this example](https://github.com/archbee-io/docs-example/blob/main/summary.md)

### Issue: "Assets/images not loading"

**Solution:**
- Check `assets` path in `archbee.yaml`
- Ensure assets folder exists and contains files
- Verify image paths in markdown are correct
- Use relative paths: `![Logo](../assets/logo.png)`

### Issue: "Changes not syncing"

**Solution:**
1. Check webhook status: **Space Settings** → **GitHub**
2. Verify branch name matches your commits
3. Try manual sync: **Settings** → **Sync Now**
4. Check Archbee webhook in GitHub repo settings
5. View sync logs in Archbee for errors

## Important Notes

### About This Directory

⚠️ **Important**: This directory (`/home/dev-ice/xandeum/pod-docs`) appears to contain **built HTML files** from MkDocs, not the source Markdown files.

**To integrate with Archbee, you need:**

1. **Source Markdown Files**: The original `.md` files used to build this site
2. **Source Repository**: The repository containing these markdown files

**Next Steps:**

```bash
# Find your source repository
cd /path/to/your/source/repo

# Copy configuration files there
cp /home/dev-ice/xandeum/pod-docs/archbee.yaml .
cp /home/dev-ice/xandeum/pod-docs/summary.md .

# Adjust paths in archbee.yaml if needed
# Then commit and push
git add archbee.yaml summary.md
git commit -m "Add Archbee integration"
git push origin main
```

### Converting HTML to Markdown (If Needed)

If you only have HTML files:

```bash
# Install pandoc
sudo apt-get install pandoc

# Convert each HTML file
pandoc index.html -f html -t markdown -o docs/index.md
pandoc rpc-api/index.html -f html -t markdown -o docs/rpc-api.md
pandoc cli/index.html -f html -t markdown -o docs/cli.md
```

## Additional Resources

- [Archbee GitHub Setup](https://www.archbee.com/docs/set-up-the-repository)
- [Archbee GitHub 2-way Sync](https://www.archbee.com/docs/github-2-way-sync)
- [Example Repository](https://github.com/archbee-io/docs-example)
- [Markdown Syntax](https://www.archbee.com/docs/use-markdown-shortcuts)
- [Archbee Support](https://www.archbee.com/support)

## Quick Reference

### archbee.yaml Structure

```yaml
docspath: /docs                    # Where markdown files are
structure:
  summary: summary.md              # Navigation structure
  assets: assets                   # Assets folder
publishspace: true                 # Auto-publish on commit
customcss: ""                      # Custom CSS
customjs: ""                       # Custom JavaScript
footertemplate: ""                 # Footer HTML
headerincludes: ""                 # Header includes
```

### summary.md Example

```markdown
[Home](index.md)

## Getting Started
[Installation](install.md)
[Quick Start](quickstart.md)

## API Reference
[Overview](api/overview.md)
[Methods](api/methods.md)
```

---

## Support

If you have questions or need help:

- **Archbee Docs**: https://docs.archbee.com
- **Archbee Support**: support@archbee.com
- **GitHub Example**: https://github.com/archbee-io/docs-example

Good luck with your documentation! 🚀

