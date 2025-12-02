# CI/CD Pipeline Overview

## Visual Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                   Developer Workflow                        │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  Developer makes changes and pushes to GitHub               │
│  - git commit                                                │
│  - git push origin feature-branch                           │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│               GitHub Actions Triggered                       │
└─────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
┌───────────────────────────┐  ┌───────────────────────────┐
│  Validation Workflow      │  │  Code Quality Workflow    │
│  ✓ Check .uproject file   │  │  ✓ Format checking        │
│  ✓ Validate JSON          │  │  ✓ Large file detection   │
│  ✓ Check source files     │  │  ✓ Secret scanning        │
│  ✓ Verify Build.cs        │  │  ✓ Code statistics        │
│  ⏱️  ~1-2 minutes          │  │  ⏱️  ~2-3 minutes          │
└───────────────────────────┘  └───────────────────────────┘
                │                       │
                └───────────┬───────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              All Checks Pass? ✓                              │
└─────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
        ┌───────────────┐       ┌──────────────────┐
        │ Pull Request  │       │ Merge to Main    │
        │ Ready for     │       │ or Develop       │
        │ Review        │       │                  │
        └───────────────┘       └──────────────────┘


┌─────────────────────────────────────────────────────────────┐
│                    Release Workflow                          │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  Developer creates version tag                               │
│  - git tag v1.0.0                                            │
│  - git push origin v1.0.0                                    │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              Release Workflow Triggered                      │
│  ✓ Generate changelog from commits                          │
│  ✓ Create source archive (zip)                              │
│  ✓ Create GitHub release                                     │
│  ✓ Attach files to release                                   │
│  ⏱️  ~3-5 minutes                                             │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         GitHub Release Published ✓                           │
│  - Changelog included                                        │
│  - Source code archive attached                              │
│  - Ready for distribution                                    │
└─────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────┐
│           CI Build Workflow (Optional/Advanced)              │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  Requires Unreal Engine 5.6 Installation                     │
│  ✓ Build UE project with UnrealBuildTool                    │
│  ✓ Run automated tests                                       │
│  ✓ Upload build logs                                         │
│  ⏱️  ~15-60 minutes (depends on project size)                │
│  ⚠️  Best used with self-hosted runners                      │
└─────────────────────────────────────────────────────────────┘
```

## Workflow Triggers

### Push to Main/Develop
```
Event: git push
    │
    ├─→ Validation Workflow (runs)
    ├─→ Code Quality Workflow (runs)
    └─→ CI Build Workflow (runs)
```

### Pull Request
```
Event: Pull Request opened/updated
    │
    ├─→ Validation Workflow (runs)
    ├─→ Code Quality Workflow (runs)
    └─→ CI Build Workflow (runs)
```

### Version Tag
```
Event: git push origin v*.*.*
    │
    └─→ Release Workflow (runs)
```

### Manual Trigger
```
GitHub UI: Actions → Release → Run workflow
    │
    └─→ Release Workflow (runs)
```

## Workflow Dependencies

```
Validation Workflow (no dependencies)
├─ Requires: jq, basic Linux tools
└─ Runtime: ~1-2 minutes

Code Quality Workflow (no dependencies)
├─ Requires: clang-format, basic Linux tools
└─ Runtime: ~2-3 minutes

CI Build Workflow (requires UE)
├─ Requires: Unreal Engine 5.6, Windows runner
└─ Runtime: ~15-60 minutes

Release Workflow (no dependencies)
├─ Requires: basic Linux tools, zip
├─ Permissions: write access to create releases
└─ Runtime: ~3-5 minutes
```

## Success Criteria

### For Pull Requests
- ✓ Validation workflow passes
- ✓ Code quality checks pass
- ✓ No security issues detected
- ✓ Code review approved

### For Releases
- ✓ Version tag pushed
- ✓ Release workflow completes
- ✓ GitHub release created
- ✓ Files attached to release

## Failure Handling

```
Workflow Fails
    │
    ├─→ Check Actions tab for logs
    ├─→ Review error messages
    ├─→ Fix issues locally
    ├─→ Push fixes
    └─→ Workflows re-run automatically
```

## Best Practices

1. **Before Pushing:**
   - Test locally
   - Follow code style
   - Write clear commit messages

2. **When Creating PR:**
   - Wait for all checks to pass
   - Address any failures
   - Respond to review comments

3. **Before Releasing:**
   - Update changelog if manual
   - Test the release build
   - Use semantic versioning (v1.2.3)

4. **After Release:**
   - Verify release on GitHub
   - Test downloaded archive
   - Update documentation if needed

## Monitoring & Alerts

### Where to Check Status
```
GitHub Repository
    │
    └─→ Actions Tab
        │
        ├─→ Recent workflow runs
        ├─→ Detailed logs
        ├─→ Artifacts (if any)
        └─→ Timing information
```

### Status Badges
Add to README.md to show real-time status:
```markdown
![Validation](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/validate.yml/badge.svg)
```

## Integration Points

```
GitHub Repository
    │
    ├─→ Commits → Triggers CI/CD
    ├─→ Pull Requests → Runs checks
    ├─→ Tags → Creates releases
    └─→ Actions → Shows workflow status
```

## Future Enhancements

Possible additions to the pipeline:

1. **Deployment Stage**
   - Deploy to cloud storage
   - Publish to distribution platform
   - Update live builds

2. **Additional Testing**
   - Performance tests
   - Integration tests
   - End-to-end tests

3. **Notifications**
   - Slack/Discord integration
   - Email notifications
   - Status updates

4. **Advanced Features**
   - Multi-platform builds
   - Parallel test execution
   - Caching strategies

---

**Last Updated:** December 2, 2024
**Pipeline Version:** 1.0
**Status:** ✅ Operational
