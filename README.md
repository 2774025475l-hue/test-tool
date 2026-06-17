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

### macOS 特别说明

项目使用 `better-sqlite3`（含 C++ 原生模块），macOS 上需要 **Xcode Command Line Tools**：

```bash
# 如果没有安装过，运行：
xcode-select --install
```

macOS 上建议使用 **npmmirror 镜像** 加速依赖安装（项目 `setup.sh` 已自动配置）。

## 快速开始

### 1. 克隆项目

```bash
git clone <仓库地址>
cd test-tool
```

### 2. 一键安装

```bash
bash scripts/setup.sh
```

这个脚本会自动完成：
- 配置 npm 淘宝镜像源
- 安装所有 Node.js 依赖（包括编译 better-sqlite3 原生模块）
- 创建 Python 虚拟环境（可选）
- 从 `.env.example` 复制 `.env` 模板

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

### 4. 启动开发服务器

```bash
npm run dev
```

浏览器打开 **http://localhost:3000** 即可使用。

### 5. 桌面应用（可选）

```bash
bash scripts/electron.sh
```

会先 `npm run build` 构建生产版本，然后启动 Electron 桌面窗口。

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

| 命令 | 说明 |
|------|------|
| `npm run dev` | 启动开发服务器（热更新） |
| `npm run build` | 构建生产版本 |
| `npm run start` | 启动生产服务器（需先 build） |
| `npm run lint` | ESLint 代码检查 |
| `bash scripts/setup.sh` | 一键安装环境 |
| `bash scripts/dev.sh` | 启动开发（含 Python venv 激活） |
| `bash scripts/electron.sh` | 构建 + 启动桌面应用 |

## 常见问题

### macOS 安装 better-sqlite3 报错

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

项目 `setup.sh` 已自动配置淘宝镜像。如果需要手动切换：

```bash
npm config set registry https://registry.npmmirror.com/
```

### 端口被占用

```bash
# 查看占用 3000 端口的进程
lsof -i :3000
# 杀掉进程
kill -9 <PID>
```

Next.js 也会自动尝试下一个可用端口（3001、3002…），看终端输出即可。

### API Key 不生效

1. 确认 `.env` 文件在项目根目录（与 `package.json` 同级）
2. 确认变量名拼写正确：`DEEPSEEK_API_KEY` / `DASHSCOPE_API_KEY`
3. 重启开发服务器（`.env` 修改后需重启）
4. DeepSeek Key 格式以 `sk-` 开头；DashScope Key 以 `sk-` 开头

### OCR 识别失败 / 超时

- 图片大小建议 < 5MB，分辨率清晰
- 网络需要能访问阿里云 DashScope API
- 如持续失败，检查 DashScope 控制台余额和 API Key 权限

## 许可证

仅供学习使用。
