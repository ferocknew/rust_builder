# 如何使用 GitHub Actions 构建 Windows exe

## 工作流说明

已为您设置好 GitHub Actions 工作流，当您推送 tag 时会自动编译 Windows 版本的可执行文件。

## 使用步骤

### 1. 打 tag 并推送

```bash
# 创建 tag（版本号）
git tag v1.0.0

# 推送 tag 到远程仓库
git push origin v1.0.0
```

### 2. 查看 GitHub Actions 构建状态

1. 访问您的 GitHub 仓库：https://github.com/ferocknew/rust_builder
2. 点击 "Actions" 标签
3. 查看 "Build Windows Exe" 工作流的运行状态

### 3. 下载构建好的 Windows exe

构建完成后（大约需要 5-10 分钟），您可以通过以下两种方式下载：

#### 方式 1：从 Artifacts 下载
1. 在 Actions 页面找到完成的工作流运行
2. 滚动到页面底部的 "Artifacts" 部分
3. 下载 `windows-exe` 文件

#### 方式 2：从 Releases 下载（推荐）
1. 访问仓库的 "Releases" 页面
2. 找到对应的版本（如 v1.0.0）
3. 下载 `课表管理系统_1.0.0_x64-setup.exe` 文件

## 工作流配置说明

当前工作流配置为：
- **触发条件**：推送以 `v` 开头的 tag（如 v1.0.0、v1.0.1 等）
- **构建环境**：Windows latest
- **构建产物**：NSIS 安装程序（.exe）
- **保留时间**：30 天
- **自动创建 Release**：是的

## 手动触发构建

如果您不想打 tag，也可以手动触发构建：

1. 访问仓库的 Actions 页面
2. 选择 "Build Windows Exe" 工作流
3. 点击 "Run workflow" 按钮
4. 选择 main 分支并点击运行

## 示例

### 发布新版本
```bash
# 1. 修改代码并提交
git add .
git commit -m "Update something"

# 2. 推送到 main 分支
git push

# 3. 创建版本 tag
git tag v1.0.1

# 4. 推送 tag（会触发 GitHub Actions 构建）
git push origin v1.0.1
```

### 查看所有 tags
```bash
git tag
```

### 删除错误的 tag
```bash
# 删除本地 tag
git tag -d v1.0.0

# 删除远程 tag
git push origin :refs/tags/v1.0.0
```

## 注意事项

1. **首次构建**：第一次运行可能需要更长时间（10-15 分钟），因为需要下载 Rust 和 Node.js 依赖
2. **构建时间**：后续构建通常需要 5-10 分钟
3. **存储空间**：Artifacts 会保留 30 天，Releases 会永久保存
4. **权限**：确保您的 GitHub 账号有 Actions 权限

## 下一步

现在您可以尝试推送第一个 tag 来测试构建流程：

```bash
git tag v1.0.0
git push origin v1.0.0
```

构建完成后，您就可以在 macOS 上获得一个 Windows 可执行文件了！
