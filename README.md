# Shared Cursor Rules

Centralized cursor rules repository that automatically syncs to all dependent projects via GitHub Actions.

## 🚀 Setup

### 1. Create Private GitHub Repository

1. Go to [GitHub](https://github.com/new)
2. Repository name: `shared-cursor-rules` (or your preferred name)
3. Set visibility to **Private**
4. **Don't** initialize with README, .gitignore, or license (we already have these)
5. Click "Create repository"

### 2. Push This Repository

```bash
cd ~/Desktop/shared-cursor-rules

# Add your GitHub repository as remote
git remote add origin https://github.com/YOUR-USERNAME/shared-cursor-rules.git

# Push to GitHub
git push -u origin main
```

### 3. Create GitHub Personal Access Token (PAT)

1. Go to GitHub Settings → [Developer settings](https://github.com/settings/developers) → [Personal access tokens](https://github.com/settings/tokens?type=classic)
2. Click "Generate new token (classic)"
3. Name: `cursor-rules-sync`
4. Expiration: Choose your preference (or no expiration)
5. Select scopes:
   - ✅ `repo` (Full control of private repositories)
6. Click "Generate token"
7. **Copy the token immediately** (you won't see it again!)

### 4. Add PAT to Repository Secrets

1. Go to your `shared-cursor-rules` repository on GitHub
2. Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `PAT_TOKEN`
5. Value: Paste your Personal Access Token
6. Click "Add secret"

### 5. Configure Dependent Repositories

Edit `.github/workflows/sync-rules.yml` and add your repositories to the `repos` array:

```yaml
repos=(
  "your-org/project-1"
  "your-org/project-2"
  "your-org/project-3"
)
```

**Important**: The PAT token must have access to all repositories listed here.

### 6. Commit and Push

```bash
git add .github/workflows/sync-rules.yml
git commit -m "Add sync workflow"
git push
```

## 📦 Using in Dependent Projects

### Option 1: Copy Rules (Recommended)

Copy the rules to each project's `.cursor/rules/` directory:

```bash
# In your project
cp -r ~/Desktop/shared-cursor-rules/.cursor/rules .cursor/
```

Or use a script to sync:

```bash
#!/bin/bash
# sync-rules.sh in your project
SHARED_RULES="$HOME/Desktop/shared-cursor-rules"
cp -r "$SHARED_RULES/.cursor/rules" .cursor/
```

### Option 2: Git Submodule

```bash
# In your project
git submodule add https://github.com/YOUR-USERNAME/shared-cursor-rules.git .cursor/rules-shared
ln -s .cursor/rules-shared/.cursor/rules .cursor/rules
```

### Option 3: Symlink (Local Development)

```bash
# In your project
rm -rf .cursor/rules
ln -s ~/Desktop/shared-cursor-rules/.cursor/rules .cursor/rules
```

## 🔄 How It Works

1. **You update rules** in this repository
2. **You commit and push** to `main` branch
3. **GitHub Actions triggers** automatically
4. **Workflow clones** each dependent repository
5. **Rules are copied** to each repo's `.cursor/rules/`
6. **Pull requests are created** in each repository
7. **You review and merge** the PRs

## 📝 Adding New Dependent Projects

1. Edit `.github/workflows/sync-rules.yml`
2. Add the new repository to the `repos` array:
   ```yaml
   repos=(
     "your-org/existing-project"
     "your-org/new-project"  # Add here
   )
   ```
3. Commit and push
4. The next rule update will include the new project

## 🔒 Security Notes

- The PAT token has full repository access - keep it secure
- Only add repositories you trust to the workflow
- Consider using a GitHub App instead of PAT for better security (advanced)

## 🛠️ Troubleshooting

### PRs not being created

- Check GitHub Actions logs for errors
- Verify PAT token has `repo` scope
- Ensure PAT has access to all repositories
- Check that repositories exist and are accessible

### "No changes" message

- Rules are already up to date in that repository
- This is normal - the workflow skips repos with no changes

### Authentication errors

- Verify PAT token is still valid
- Check that PAT has access to all repositories
- Regenerate PAT if needed

## 📚 Documentation

Additional documentation can be added to the `docs/` folder.

