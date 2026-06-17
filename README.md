# 期末复习智能助手 🎓

上传课件（PDF / DOCX / PPTX / TXT / 图片），AI 自动生成复习内容——核心考点、交互式思维导图、标准化习题，一站式备考工具。

## 功能一览

| 功能 | 说明 |
|------|------|
| 📄 **课件解析** | 支持 PDF、DOCX、PPTX、TXT、JPG/PNG/WEBP 图片（OCR 识别），拖拽即上传 |
| 🎯 **核心考点** | AI 提炼知识点，按「必考 / 高频 / 易错」分级，附带精准学习资源链接 |
| 🧠 **复习架构** | 交互式思维导图，支持折叠/展开/筛选，复习进度自动保存到浏览器 |
| ✏️ **习题练习** | 标准化组卷，支持顺序刷题 / 随机刷题 / 专题训练，自动评分 + 答案解析 |
| 📕 **错题本** | 错题自动收集，支持重做、收藏、导出 |
| 💬 **划词 AI 问答** | 选中页面任意文字，一键解释/举例/追问，支持连续对话 |
| 📚 **历史记录** | 每次导入的题目和复习方案自动存档，随时回溯 |
| 🔐 **账号系统** | 注册/登录，数据云端隔离（可选） |
| 🖥 **桌面应用** | Electron 打包，独立窗口运行，免开浏览器 |

## 技术栈

| 层 | 技术 |
|----|------|
| 框架 | Next.js 14 (App Router) + React 18 |
| 语言 | TypeScript 5.9 |
| 样式 | Tailwind CSS 3.4 |
| 数据库 | better-sqlite3（本地 SQLite，零配置） |
| AI 大模型 | DeepSeek Chat API（题目生成 / 问答） |
| AI 视觉 | 阿里云 DashScope Qwen-VL（OCR 图片识别） |
| 公式渲染 | KaTeX |
| 桌面端 | Electron |

## 环境要求

### 必需

| 依赖 | 最低版本 | 检查命令 |
|------|---------|---------|
| Node.js | >= 18 | `node --version` |
| npm | >= 9 | `npm --version` |

### 可选

| 依赖 | 说明 |
|------|------|
| Python 3.9+ | `py_module/` 目录预留的 AI 工具脚本（当前无需安装） |

---

### ⚠️ 各平台前置准备（必读）

项目使用 `better-sqlite3`（含 C++ 原生模块），**不同操作系统需要不同的编译工具链**。请根据你的系统先完成以下步骤，再执行 `npm install`。

#### 🍎 macOS

需要 **Xcode Command Line Tools**：

```bash
# 安装（如果没装过）
xcode-select --install

# 验证
xcrun --show-sdk-path
```

#### 🪟 Windows

`better-sqlite3` 在 Windows 上需要 **C++ 编译工具链**。以下两种方式任选其一：

**方式一：安装 Visual Studio Build Tools（推荐）**

1. 下载 [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)
2. 运行安装程序，勾选 **"使用 C++ 的桌面开发"** 工作负载
3. 安装完成后重启终端

**方式二：使用 windows-build-tools（轻量，需管理员终端）**

以**管理员身份**打开 PowerShell 或 CMD，运行：

```powershell
npm install --global windows-build-tools
```

> 该命令会自动安装 Python、Visual Studio Build Tools 等必要组件，耗时约 5-15 分钟。

**验证编译环境就绪：**

```powershell
# 确认 npm 可正常编译原生模块
npm install -g node-gyp
node-gyp --version
```

#### 🐧 Linux (Ubuntu/Debian)

```bash
sudo apt install build-essential python3 make gcc
```

#### 🔧 npmmirror 镜像（国内用户推荐）

国内用户建议使用 npmmirror 镜像加速依赖下载：

```bash
npm config set registry https://registry.npmmirror.com/
```

项目 `scripts/setup.sh`（macOS/Linux）已自动配置。

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/2774025475l-hue/test-tool.git
cd test-tool
```

### 2. 安装依赖

> **Windows 用户注意**：请先完成上方「各平台前置准备」中的 Windows 编译工具链安装，否则 `npm install` 会报错。

**macOS / Linux：**

```bash
# 一键安装（配置镜像 + 安装依赖 + 创建 venv）
bash scripts/setup.sh
```

**Windows（CMD / PowerShell）：**

```powershell
# 1. 配置国内镜像（可选但推荐）
npm config set registry https://registry.npmmirror.com/

# 2. 安装依赖（编译 better-sqlite3）
npm install

# 3. 创建 .env 文件
copy .env.example .env
```

### 3. 配置 API Key

编辑项目根目录下的 `.env` 文件，填入你的 API Key：

```env
# DeepSeek API Key — 用于 AI 题目生成和问答
# 获取地址: https://platform.deepseek.com
DEEPSEEK_API_KEY=你的DeepSeek_Key

# 阿里云 DashScope API Key — 用于图片 OCR 识别
# 获取地址: https://dashscope.console.aliyun.com
DASHSCOPE_API_KEY=你的DashScope_Key
```

> ⚠️ **安全提示**：`.env` 文件包含 API Key，已被 `.gitignore` 排除，**请勿将真实 Key 提交到 Git**。`.env.example` 是公开模板，只含占位符。

**API Key 获取步骤：**

| Key | 平台 | 注册地址 | 说明 |
|-----|------|---------|------|
| `DEEPSEEK_API_KEY` | DeepSeek | [platform.deepseek.com](https://platform.deepseek.com) | 注册后在「API Keys」页面创建，以 `sk-` 开头 |
| `DASHSCOPE_API_KEY` | 阿里云 DashScope | [dashscope.console.aliyun.com](https://dashscope.console.aliyun.com) | 开通「模型服务灵积」后在「API-KEY 管理」创建 |

### 4. 启动开发服务器

**所有平台通用：**

```bash
npm run dev
```

浏览器打开 **http://localhost:3000** 即可使用。

### 5. 桌面应用（可选）

**macOS / Linux：**
```bash
bash scripts/electron.sh
```

**Windows（CMD / PowerShell）：**
```powershell
npm run build
npx electron electron/main.js
```

会先构建生产版本，然后启动 Electron 桌面窗口。

## 项目结构

```
test-tool/
├── src/
│   ├── app/                        # Next.js 路由层
│   │   ├── api/                    # 17 个 API 路由
│   │   │   ├── auth/               #   注册/登录/登出/用户信息
│   │   │   ├── ai/                 #   AI 问答/生成/重新生成
│   │   │   ├── parse/              #   文件解析/图片 OCR
│   │   │   ├── batch-import/       #   批量导入
│   │   │   ├── history/            #   历史记录
│   │   │   └── resources/          #   资源链接管理
│   │   ├── layout.tsx              # 根布局
│   │   ├── page.tsx                # 主页面（组件组装）
│   │   └── globals.css             # 全局样式 + Tailwind
│   │
│   ├── components/                 # UI 组件
│   │   ├── ui/                     # 通用组件
│   │   │   ├── FileDrop.tsx        #   文件拖拽上传
│   │   │   ├── MathFormula.tsx     #   KaTeX 公式渲染
│   │   │   └── MarkdownView.tsx    #   Markdown → HTML
│   │   └── workbench/              # 业务组件
│   │       ├── SelectionPanel.tsx  #   划词 AI 问答面板
│   │       ├── KeyPointsView.tsx   #   考点列表视图
│   │       ├── FrameworkView.tsx   #   交互式思维导图
│   │       ├── ExercisesView.tsx   #   习题练习系统
│   │       └── WrongBookView.tsx   #   错题本
│   │
│   ├── lib/                        # 核心逻辑（零 UI 依赖）
│   │   ├── ai/                     # AI 模块
│   │   │   ├── client.ts           #   DeepSeek API 封装（超时/重试）
│   │   │   ├── ocr.ts              #   通义千问 OCR 封装
│   │   │   └── prompts.ts          #   所有 AI Prompt 模板
│   │   ├── db/                     # 数据库层
│   │   │   ├── connection.ts       #   单例 SQLite 连接 + 建表
│   │   │   ├── users.ts            #   用户 CRUD
│   │   │   ├── history.ts          #   历史记录
│   │   │   ├── cache.ts            #   AI 缓存
│   │   │   └── resources.ts        #   资源链接
│   │   ├── parser/                 # 文件解析器
│   │   │   ├── index.ts            #   统一入口
│   │   │   ├── txt.ts              #   TXT 解析
│   │   │   ├── docx.ts             #   DOCX 解析 (mammoth)
│   │   │   ├── pptx.ts             #   PPTX 解析 (adm-zip)
│   │   │   └── pdf.ts              #   PDF 解析 (pdfjs-dist)
│   │   ├── auth/                   # 认证模块
│   │   │   ├── password.ts         #   密码哈希
│   │   │   └── session.ts          #   Cookie 会话
│   │   ├── config.ts               # Zod 环境变量校验
│   │   └── linkHelper.ts           # 外部链接工具
│   │
│   └── middleware.ts               # 认证保护 + 限流（60次/分钟）
│
├── electron/                       # Electron 桌面端
│   ├── main.js                     #   主进程
│   └── preload.js                  #   预加载脚本
│
├── scripts/                        # 一键脚本
│   ├── setup.sh                    #   全新安装
│   ├── dev.sh                      #   开发启动
│   └── electron.sh                 #   构建 + 桌面预览
│
├── py_module/                      # Python 工具预留
│   ├── requirements.txt            #   Python 依赖声明
│   └── README.md                   #   使用说明
│
├── data/                           # 运行时 SQLite 数据库（不进 Git）
├── .env.example                    # 环境变量模板（公开）
├── .gitignore                      # Git 忽略规则
├── package.json
├── tsconfig.json
├── next.config.js
└── tailwind.config.ts
```

## 可用命令

| 命令 | 说明 | 平台 |
|------|------|------|
| `npm run dev` | 启动开发服务器（热更新） | 全平台 |
| `npm run build` | 构建生产版本 | 全平台 |
| `npm run start` | 启动生产服务器（需先 build） | 全平台 |
| `npm run lint` | ESLint 代码检查 | 全平台 |
| `bash scripts/setup.sh` | 一键安装环境 | macOS / Linux |
| `bash scripts/dev.sh` | 启动开发（含 Python venv 激活） | macOS / Linux |
| `bash scripts/electron.sh` | 构建 + 启动桌面应用 | macOS / Linux |
| `npx electron electron/main.js` | 直接启动桌面应用（需先 build） | 全平台 |

## 常见问题

### 🪟 Windows：`npm install` 报错 / better-sqlite3 编译失败

```
Error: Could not load better_sqlite3.node
npm ERR! gyp ERR! stack Error: Can't find Python executable
npm ERR! gyp ERR! find VS could not find a version of Visual Studio
```

**原因**：缺少 C++ 编译工具链。

**解决方案**：

1. **确认编译工具已安装**——参考上方「Windows 前置准备」步骤，安装 Visual Studio Build Tools 或 windows-build-tools。

2. **以管理员身份打开终端后重装：**
   ```powershell
   # 清理
   rmdir /s /q node_modules
   del package-lock.json

   # 设置 npm 使用 Windows 编译工具
   npm config set msvs_version 2022

   # 重新安装
   npm install
   ```

3. 如果仍然失败，尝试在 **PowerShell（管理员）** 中：
   ```powershell
   npm install --global windows-build-tools
   npm install
   ```

### 🍎 macOS：安装 better-sqlite3 报错

```
Error: Could not load better_sqlite3.node
```

**解决方案：**

```bash
# 1. 确认 Xcode Command Line Tools 已安装
xcode-select --install

# 2. 清理重装
rm -rf node_modules package-lock.json
npm install
```

### npm 安装慢 / 卡住

**国内用户**建议使用 npmmirror 镜像：

```bash
npm config set registry https://registry.npmmirror.com/
```

> 项目 `scripts/setup.sh`（macOS/Linux）已自动配置。

### Windows：`bash` 命令不可用

Windows 的 CMD 和 PowerShell 不支持 `bash` 命令。解决方案：

- **使用 `npm` 命令代替**：`npm run dev`、`npm run build` 等在所有平台通用
- **或安装 Git Bash**：安装 [Git for Windows](https://git-scm.com/downloads/win)，会附带 Git Bash 终端，支持 bash 脚本
- **或使用 WSL**：Windows 10/11 可通过 `wsl --install` 安装 Linux 子系统，获得完整 Linux 环境

### 端口被占用

**macOS / Linux：**
```bash
lsof -i :3000          # 查看占用进程
kill -9 <PID>          # 杀掉进程
```

**Windows（CMD / PowerShell）：**
```powershell
netstat -ano | findstr :3000       # 查看占用进程（最后一列是 PID）
taskkill /PID <PID> /F              # 杀掉进程
```

> Next.js 也会自动尝试下一个可用端口（3001、3002…），看终端输出即可。

### API Key 不生效

1. 确认 `.env` 文件在项目根目录（与 `package.json` 同级）
2. 确认变量名拼写正确：`DEEPSEEK_API_KEY` / `DASHSCOPE_API_KEY`
3. 重启开发服务器（`.env` 修改后需重启）
4. DeepSeek Key 格式以 `sk-` 开头；DashScope Key 以 `sk-` 开头

### OCR 识别失败 / 超时

- 图片大小建议 < 5MB，分辨率清晰
- 网络需要能访问阿里云 DashScope API
- 如持续失败，检查 DashScope 控制台余额和 API Key 权限

### Windows：Electron 白屏或无法启动

- 确保先执行 `npm run build`，再启动 Electron
- 检查防火墙是否拦截了 Node.js 网络访问
- Windows 上 Electron 启动较慢，首次启动可能需要 5-10 秒

## 许可证

仅供学习使用。
