# CI/CD 工作流

本目录包含用于 Unreal Engine 项目的 GitHub Actions 自动化持续集成和持续部署（CI/CD）工作流。

## 工作流概述

### 1. 验证工作流 (`validate.yml`)

**触发条件：**
- 推送到 `main` 或 `develop` 分支
- 针对 `main` 或 `develop` 分支的 Pull Request

**目的：**
验证项目结构并确保所有必需文件都存在且格式正确。

**检查项：**
- ✓ 验证 `my_game.uproject` 文件存在且为有效 JSON
- ✓ 验证引擎版本兼容性
- ✓ 检查必需的源文件（`.cpp`、`.h`）
- ✓ 验证 Build.cs 和 Target.cs 文件
- ✓ 确认 Content 目录结构

**使用方法：**
此工作流在每次推送和拉取请求时自动运行，无需配置。

### 2. CI 构建工作流 (`ci.yml`)

**触发条件：**
- 推送到 `main` 或 `develop` 分支
- 针对 `main` 或 `develop` 分支的 Pull Request

**目的：**
构建 Unreal Engine 项目并运行自动化测试。

**要求：**
- Windows 运行器（Unreal Engine 通常在 Windows 上运行）
- Unreal Engine 5.6 安装（通过 game-ci 自动化）
- 足够的磁盘空间（UE 项目很大）

**功能：**
- 使用 Unreal Build Tool 构建项目
- 运行自动化测试
- 上传构建日志作为工件

**注意：** 此工作流需要完整的 Unreal Engine 安装，这可能耗时且资源密集。对于大多数检查，建议使用验证工作流以获得更快的反馈。

### 3. 代码质量工作流 (`code-quality.yml`)

**触发条件：**
- 推送到 `main` 或 `develop` 分支
- 针对 `main` 或 `develop` 分支的 Pull Request

**目的：**
分析代码质量并检查常见问题。

**检查项：**
- C++ 代码格式化（使用 clang-format）
- 大文件检测
- 潜在的硬编码密钥
- 代码行数统计

**使用方法：**
在每次推送和拉取请求时自动运行。提供警告但不会阻止构建。

### 4. 发布工作流 (`release.yml`)

**触发条件：**
- 推送版本标签（例如 `v1.0.0`、`v2.1.3`）
- 手动工作流调度

**目的：**
创建包含变更日志和项目归档的自动化发布。

**功能：**
- 从 git 提交生成变更日志
- 创建源代码归档（排除构建工件）
- 创建带有发布说明的 GitHub 发布
- 将项目归档附加到发布

**使用方法：**

#### 自动发布（通过标签）：
```bash
git tag v1.0.0
git push origin v1.0.0
```

#### 手动发布（通过 GitHub UI）：
1. 转到仓库的 "Actions" 标签
2. 选择 "Release" 工作流
3. 点击 "Run workflow"
4. 输入版本（例如 `v1.0.0`）
5. 点击 "Run workflow"

## 设置说明

### 前提条件

1. **GitHub 仓库设置：**
   - 确保在仓库设置中启用了 GitHub Actions
   - 检查工作流权限是否允许读写访问

2. **分支保护（可选但推荐）：**
   - 转到 Settings → Branches → Branch protection rules
   - 为 `main` 分支添加规则
   - 启用 "Require status checks to pass before merging"
   - 选择验证工作流作为必需项

### 配置

工作流已预先配置并准备使用。但是，您可以自定义它们：

1. **编辑工作流触发器：**
   - 修改每个工作流文件中的 `on:` 部分
   - 根据需要添加或删除分支

2. **调整 Unreal Engine 版本：**
   - 如果使用不同的 UE 版本，更新 `ci.yml` 中的 `unreal-version`
   - 更新 `my_game.uproject` 中的引擎关联

3. **自定义构建参数：**
   - 编辑 `ci.yml` 中的构建命令
   - 添加其他构建配置（Debug、Shipping 等）

## 监控工作流

### 查看工作流运行：
1. 转到 GitHub 仓库的 "Actions" 标签
2. 点击工作流查看其运行历史
3. 点击特定运行以查看详细日志

### 徽章（可选）：
将工作流状态徽章添加到您的 README.md：

```markdown
![Validation](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/validate.yml/badge.svg)
![CI](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/ci.yml/badge.svg)
![Code Quality](https://github.com/yuban3841/wangyi-proj-down/actions/workflows/code-quality.yml/badge.svg)
```

## 故障排除

### 工作流失败并显示 "command not found"
- 检查工作流中是否安装了所需工具
- 如需要，添加安装步骤

### 构建超时
- 增加工作流中的超时：`timeout-minutes: 60`
- 考虑为大型项目使用自托管运行器

### 工件上传失败
- 检查工件大小限制（免费层最大 2GB）
- 从工件中排除不必要的文件

### 权限被拒绝错误
- 确保工作流在仓库设置中具有适当的权限
- 检查工作流文件中的 GITHUB_TOKEN 权限

## 最佳实践

1. **保持工作流快速：**
   - 使用验证工作流进行快速检查
   - 仅对重要分支保留完整构建

2. **使用缓存：**
   - 缓存依赖项以加快构建速度
   - 缓存 Unreal Engine 中间文件

3. **监控资源使用：**
   - GitHub Actions 有使用限制
   - 考虑为重负载工作使用自托管运行器

4. **保护密钥：**
   - 切勿将密钥提交到工作流
   - 对敏感数据使用 GitHub Secrets

5. **定期维护：**
   - 保持 action 版本最新
   - 随着项目的发展审查和更新工作流

## 其他资源

- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [Unreal Engine 文档](https://docs.unrealengine.com/)
- [Game CI 文档](https://game.ci/)

## 支持

如有问题或疑问：
1. 检查工作流日志以获取详细的错误消息
2. 查阅本文档
3. 在仓库中打开 issue
