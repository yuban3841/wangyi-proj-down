# CI/CD Workflows

This directory contains GitHub Actions workflows for automated continuous integration and continuous deployment (CI/CD) for the Unreal Engine project.

## Workflows Overview

### 1. Validation Workflow (`validate.yml`)

**Triggers:**
- Push to `main` or `develop` branches
- Pull requests to `main` or `develop` branches

**Purpose:**
Validates the project structure and ensures all required files are present and correctly formatted.

**Checks:**
- ✓ Validates `my_game.uproject` file exists and is valid JSON
- ✓ Verifies engine version compatibility
- ✓ Checks for required source files (`.cpp`, `.h`)
- ✓ Validates Build.cs and Target.cs files
- ✓ Confirms Content directory structure

**Usage:**
This workflow runs automatically on every push and pull request. No configuration needed.

### 2. CI Build Workflow (`ci.yml`)

**Triggers:**
- Push to `main` or `develop` branches
- Pull requests to `main` or `develop` branches

**Purpose:**
Builds the Unreal Engine project and runs automated tests.

**Requirements:**
- Windows runner (Unreal Engine typically runs on Windows)
- Unreal Engine 5.6 installation (automated via game-ci)
- Sufficient disk space (UE projects are large)

**Features:**
- Builds the project using Unreal Build Tool
- Runs automated tests
- Uploads build logs as artifacts

**Note:** This workflow requires a full Unreal Engine installation, which can be time-consuming and resource-intensive. For faster feedback, the validation workflow is recommended for most checks.

### 3. Code Quality Workflow (`code-quality.yml`)

**Triggers:**
- Push to `main` or `develop` branches
- Pull requests to `main` or `develop` branches

**Purpose:**
Analyzes code quality and checks for common issues.

**Checks:**
- C++ code formatting (using clang-format)
- Large file detection
- Potential hardcoded secrets
- Lines of code statistics

**Usage:**
Runs automatically on every push and pull request. Provides warnings but doesn't block the build.

### 4. Release Workflow (`release.yml`)

**Triggers:**
- Push of version tags (e.g., `v1.0.0`, `v2.1.3`)
- Manual workflow dispatch

**Purpose:**
Creates automated releases with changelogs and project archives.

**Features:**
- Generates changelog from git commits
- Creates source archive (excluding build artifacts)
- Creates GitHub release with release notes
- Attaches project archive to release

**Usage:**

#### Automatic Release (via tag):
```bash
git tag v1.0.0
git push origin v1.0.0
```

#### Manual Release (via GitHub UI):
1. Go to "Actions" tab in your repository
2. Select "Release" workflow
3. Click "Run workflow"
4. Enter version (e.g., `v1.0.0`)
5. Click "Run workflow"

## Setup Instructions

### Prerequisites

1. **GitHub Repository Settings:**
   - Ensure GitHub Actions are enabled in your repository settings
   - Check that workflow permissions allow read and write access

2. **Branch Protection (Optional but Recommended):**
   - Go to Settings → Branches → Branch protection rules
   - Add rule for `main` branch
   - Enable "Require status checks to pass before merging"
   - Select the validation workflow as required

### Configuration

The workflows are pre-configured and ready to use. However, you can customize them:

1. **Edit workflow triggers:**
   - Modify the `on:` section in each workflow file
   - Add or remove branches as needed

2. **Adjust Unreal Engine version:**
   - Update `unreal-version` in `ci.yml` if using a different UE version
   - Update engine association in `my_game.uproject`

3. **Customize build parameters:**
   - Edit build commands in `ci.yml`
   - Add additional build configurations (Debug, Shipping, etc.)

## Monitoring Workflows

### View Workflow Runs:
1. Go to the "Actions" tab in your GitHub repository
2. Click on a workflow to see its run history
3. Click on a specific run to see detailed logs

### Badges (Optional):
Add workflow status badges to your README.md:

```markdown
![Validation](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/validate.yml/badge.svg)
![CI](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/ci.yml/badge.svg)
![Code Quality](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/code-quality.yml/badge.svg)
```

## Troubleshooting

### Workflow fails with "command not found"
- Check that required tools are installed in the workflow
- Add installation steps if needed

### Build timeout
- Increase timeout in workflow: `timeout-minutes: 60`
- Consider using self-hosted runners for large projects

### Artifact upload fails
- Check artifact size limits (max 2GB for free tier)
- Exclude unnecessary files from artifacts

### Permission denied errors
- Ensure workflow has proper permissions in repository settings
- Check GITHUB_TOKEN permissions in workflow file

## Best Practices

1. **Keep workflows fast:**
   - Use validation workflow for quick checks
   - Reserve full builds for important branches only

2. **Use caching:**
   - Cache dependencies to speed up builds
   - Cache Unreal Engine intermediate files

3. **Monitor resource usage:**
   - GitHub Actions has usage limits
   - Consider self-hosted runners for heavy workloads

4. **Secure secrets:**
   - Never commit secrets to workflows
   - Use GitHub Secrets for sensitive data

5. **Regular maintenance:**
   - Keep action versions up to date
   - Review and update workflows as project evolves

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Unreal Engine Documentation](https://docs.unrealengine.com/)
- [Game CI Documentation](https://game.ci/)

## Support

For issues or questions:
1. Check the workflow logs for detailed error messages
2. Review this documentation
3. Open an issue in the repository
