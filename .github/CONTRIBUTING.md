# Contributing to my_game

Thank you for your interest in contributing to this Unreal Engine project! This document provides guidelines for contributing.

## Development Workflow

### 1. Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/wangyi-proj-down.git
   cd wangyi-proj-down
   ```

### 2. Create a Branch

Create a feature branch for your work:
```bash
git checkout -b feature/your-feature-name
```

Branch naming conventions:
- `feature/` - New features
- `bugfix/` - Bug fixes
- `hotfix/` - Critical fixes for production
- `docs/` - Documentation updates

### 3. Make Changes

1. Open `my_game.uproject` in Unreal Engine 5.6
2. Make your changes
3. Test your changes thoroughly
4. Ensure code follows project conventions

### 4. Commit Changes

Write clear, descriptive commit messages:
```bash
git add .
git commit -m "Add feature: description of what you did"
```

Commit message guidelines:
- Use present tense ("Add feature" not "Added feature")
- First line should be concise (< 72 characters)
- Add detailed description if needed in subsequent lines

### 5. Push and Create Pull Request

1. Push your branch:
   ```bash
   git push origin feature/your-feature-name
   ```

2. Go to GitHub and create a Pull Request
3. Fill out the PR template with:
   - Description of changes
   - Related issues (if any)
   - Testing performed
   - Screenshots (if UI changes)

### 6. Code Review

- CI workflows will automatically run on your PR
- Address any CI failures
- Respond to review comments
- Make requested changes if needed

## Code Standards

### C++ Guidelines

1. **Naming Conventions:**
   - Classes: `PascalCase`
   - Functions: `PascalCase`
   - Variables: `camelCase`
   - Constants: `UPPER_CASE`

2. **Code Style:**
   - Follow Unreal Engine coding standards
   - Use spaces, not tabs (4 spaces)
   - Maximum line length: 120 characters

3. **Comments:**
   - Document public APIs
   - Explain complex logic
   - Use `//` for single-line comments
   - Use `/* */` for multi-line comments

### Blueprint Guidelines

1. Keep blueprints organized and well-commented
2. Use meaningful node names
3. Avoid overly complex blueprint graphs
4. Consider converting complex logic to C++

## Testing

Before submitting a PR:

1. **Build the project:**
   - Ensure project compiles without errors
   - Test in both Development and Shipping builds

2. **Test functionality:**
   - Test your changes in the editor
   - Test in packaged builds if applicable
   - Test on target platforms

3. **Check for regressions:**
   - Ensure existing features still work
   - Run any existing automated tests

## Pull Request Checklist

Before submitting your PR, ensure:

- [ ] Code builds without errors
- [ ] Changes have been tested
- [ ] Code follows project conventions
- [ ] Commit messages are clear and descriptive
- [ ] PR description is complete
- [ ] All CI checks pass
- [ ] No merge conflicts with main branch

## CI/CD Checks

Your PR will automatically run through several checks:

1. **Validation** - Verifies project structure
2. **Code Quality** - Checks code formatting
3. **Build** - Attempts to build the project (if configured)

If any check fails:
1. Review the error logs in the Actions tab
2. Fix the issues locally
3. Push the fixes to your branch
4. CI will re-run automatically

## Getting Help

If you need help:

1. Check the project documentation
2. Review existing issues and PRs
3. Ask questions in issue comments
4. Contact maintainers if needed

## Code of Conduct

- Be respectful and constructive
- Welcome newcomers
- Focus on the code, not the person
- Assume good intentions

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## Thank You!

Your contributions help make this project better for everyone. Thank you for taking the time to contribute!
