# my_game

使用 Unreal Engine 5 开发

![Validation](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/validate.yml/badge.svg)
![CI](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/ci.yml/badge.svg)
![Code Quality](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/code-quality.yml/badge.svg)

## 概述

一个具有自动化 CI/CD 流水线的 Unreal Engine 5.6 游戏项目。

## 系统要求

- Unreal Engine 5.6
- Windows 10/11（用于构建）
- Visual Studio 2022（推荐）

## 快速开始

1. 克隆仓库：
   ```bash
   git clone https://github.com/yuban3841/wangyi-proj-down.git
   cd wangyi-proj-down
   ```

2. 使用 Unreal Engine 5.6 打开 `my_game.uproject`

3. 通过 Unreal Engine 构建项目

## CI/CD 自动化

本项目使用 GitHub Actions 进行自动化持续集成和部署：

- **验证**：在每次推送时自动验证项目结构
- **构建**：构建项目并运行测试
- **代码质量**：检查代码格式和质量
- **发布**：在推送版本标签时创建自动化发布

有关 CI/CD 工作流的详细信息，请参阅 [.github/workflows/README_CN.md](.github/workflows/README_CN.md)

## 创建发布

要创建新版本：

```bash
git tag v1.0.0
git push origin v1.0.0
```

这将触发自动发布工作流，创建包含变更日志和项目归档的 GitHub 发布。

## 贡献

1. 创建功能分支
2. 进行更改
3. 推送分支并创建拉取请求
4. CI 工作流将自动运行以验证您的更改

## 许可证

[在此添加您的许可证信息]

---

[English Version](README.md)
