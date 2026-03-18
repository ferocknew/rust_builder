# 课表管理系统

一个基于Tauri的课表管理系统桌面应用程序。

## 功能特性

- 📋 课程设置
- 📝 生成新课表文件
- 🔍 课表检验
- 👥 生成班级课表
- 👨‍🏫 生成教师课表
- 🎓 生成学生课表
- ⚙️ 设置课表页面
- ℹ️ 关于

## 技术栈

- **前端**: HTML, CSS, JavaScript
- **桌面框架**: Tauri 1.5
- **后端**: Rust

## 开发环境要求

- Node.js 18+
- Rust 1.70+
- 系统依赖：
  - macOS: Xcode Command Line Tools
  - Windows: Microsoft C++ Build Tools
  - Linux: webkit2gtk

## 本地开发

### 安装依赖

```bash
npm install
```

### 开发模式运行

```bash
npm run tauri dev
```

## 构建可执行文件

### 构建当前平台版本

```bash
npm run build
```

构建完成后，可执行文件位于：

- **macOS**: `src-tauri/target/release/bundle/macos/课表管理系统.app`
- **Windows**: `src-tauri/target/release/bundle/nsis/课表管理系统_1.0.0_x64-setup.exe`
- **Linux**: `src-tauri/target/release/bundle/deb/` 或 `src-tauri/target/release/bundle/appimage/`

### 构建Windows版本

由于您目前在macOS系统上，有以下几种方式构建Windows exe文件：

#### 方法1: 使用GitHub Actions（推荐）

1. 将代码推送到GitHub仓库
2. 在GitHub仓库页面点击 "Actions" 标签
3. 选择 "Build Windows Exe" 工作流
4. 点击 "Run workflow" 按钮
5. 构建完成后，在Artifacts中下载Windows exe文件

#### 方法2: 在Windows机器上构建

1. 将整个项目文件夹复制到Windows机器
2. 在Windows上安装Node.js和Rust
3. 运行 \`npm install\` 安装依赖
4. 运行 \`npm run build\` 构建Windows exe

#### 方法3: 使用虚拟机

1. 在Mac上安装Windows虚拟机（如Parallels Desktop、VMware Fusion）
2. 在虚拟机中按照方法2的步骤构建

## 项目结构

```
.
├── dist/                    # 前端资源文件
│   └── index.html          # 主页面
├── src-tauri/              # Tauri后端代码
│   ├── src/
│   │   └── main.rs        # Rust主文件
│   ├── icons/             # 应用图标
│   ├── Cargo.toml         # Rust依赖配置
│   └── tauri.conf.json    # Tauri配置文件
├── .github/
│   └── workflows/
│       └── build-windows.yml  # GitHub Actions工作流
├── package.json           # Node.js依赖配置
└── README.md             # 项目说明
```

## 许可证

MIT License
