# Neon Games Website Deployment

This document explains how the website in the `/www` folder is automatically deployed to a public repository for hosting on GitHub Pages.

## Overview

The website is automatically synced from this private repository to a public repository whenever changes are pushed to the `/www` folder on the `main` branch.

## Setup Instructions

### 1. Create the Public Repository

1. Go to GitHub and create a new **public** repository named `neon-games-website` (or your preferred name)
2. Initialize it with a README or leave it empty
3. Note the repository URL: `https://github.com/rogerfoxcroft/neon-games-website`

### 2. Create a Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a name like "Website Sync Token"
4. Set expiration (recommend: 1 year or no expiration)
5. Select the following scopes:
   - `repo` (Full control of private repositories)
   - `workflow` (Update GitHub Action workflows)
6. Click "Generate token"
7. **Copy the token immediately** (you won't be able to see it again)

### 3. Add the Token as a Repository Secret

1. Go to your **private** repository settings (this repo: `neon-games`)
2. Navigate to: Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `PUBLIC_REPO_TOKEN`
5. Value: Paste the personal access token you created
6. Click "Add secret"

### 4. Update the Workflow (if needed)

If your public repository has a different name or owner, edit `.github/workflows/sync-website.yml`:

```yaml
# Change this line to match your public repo:
git clone https://x-access-token:${PUBLIC_REPO_TOKEN}@github.com/YOUR_USERNAME/YOUR_REPO_NAME.git public-repo
```

### 5. Enable GitHub Pages on Public Repository

1. Go to the public repository settings
2. Navigate to: Settings → Pages
3. Under "Source", select "Deploy from a branch"
4. Choose branch: `main`
5. Choose folder: `/ (root)`
6. Click "Save"

Your website will be available at: `https://rogerfoxcroft.github.io/neon-games-website`

## How It Works

1. You make changes to files in the `/www` folder
2. You commit and push to the `main` branch
3. GitHub Action automatically triggers
4. The action:
   - Checks out this private repo
   - Clones the public repo
   - Copies all files from `/www` to the public repo root
   - Commits and pushes to the public repo
5. GitHub Pages automatically deploys the updated website

## Manual Trigger

You can also manually trigger the sync:
1. Go to Actions tab in this repository
2. Select "Sync Website to Public Repo"
3. Click "Run workflow"
4. Select branch: `main`
5. Click "Run workflow"

## Testing

After setting up:
1. Make a small change to `/www/index.html`
2. Commit and push to `main`
3. Check the Actions tab to see the workflow run
4. Check the public repository to see the synced files
5. Visit your GitHub Pages URL to see the live website

## Troubleshooting

### Workflow fails with authentication error
- Check that `PUBLIC_REPO_TOKEN` secret is set correctly
- Verify the token has the right permissions (`repo` scope)
- Make sure the token hasn't expired

### Changes not appearing on website
- Check that GitHub Pages is enabled in the public repo
- Wait a few minutes for GitHub Pages to rebuild
- Check the Pages section in public repo settings for deployment status

### Workflow doesn't trigger
- Ensure changes are in the `/www` folder
- Check that you're pushing to the `main` branch
- Verify the workflow file is in `.github/workflows/` and properly formatted

## Notes

- Only changes to files in the `/www/**` path will trigger the workflow
- The workflow preserves all files in `/www` including subdirectories
- The public repo will be completely overwritten on each sync
- Don't manually edit files in the public repo (they'll be overwritten)
