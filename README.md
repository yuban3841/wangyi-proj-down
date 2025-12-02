# my_game

Developed with Unreal Engine 5

![Validation](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/validate.yml/badge.svg)
![CI](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/ci.yml/badge.svg)
![Code Quality](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/code-quality.yml/badge.svg)

## Overview

An Unreal Engine 5.6 game project with automated CI/CD pipelines.

## Requirements

- Unreal Engine 5.6
- Windows 10/11 (for building)
- Visual Studio 2022 (recommended)

## Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/yuban3841/wangyi-proj-down.git
   cd wangyi-proj-down
   ```

2. Open `my_game.uproject` with Unreal Engine 5.6

3. Build the project through Unreal Engine

## CI/CD

This project uses GitHub Actions for automated continuous integration and deployment:

- **Validation**: Automatically validates project structure on every push
- **Build**: Builds the project and runs tests
- **Code Quality**: Checks code formatting and quality
- **Release**: Creates automated releases when version tags are pushed

For detailed information about the CI/CD workflows, see [.github/workflows/README.md](.github/workflows/README.md)

## Creating a Release

To create a new release:

```bash
git tag v1.0.0
git push origin v1.0.0
```

This will trigger the automated release workflow that creates a GitHub release with changelog and project archive.

## Contributing

1. Create a feature branch
2. Make your changes
3. Push your branch and create a pull request
4. CI workflows will automatically run to validate your changes

## License

[Add your license information here]

---

[中文版本](README_CN.md)
