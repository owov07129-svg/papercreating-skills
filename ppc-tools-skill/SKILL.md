---
name: ppc-tools-skill
description: "检查论文创作本地工具链是否完整且完好（TeX Live / VS Code / Zotero / chktex / textidote / Java / Pandoc / LibreOffice / Acrobat / Python 包 / Tesseract / Marp），自动探测每个工具的安装路径和 PATH 状态，缺失/损坏工具列出并询问用户后按指定目录安装（有特殊路径要求的工具提前告知）。触发器：检查工具链、检查环境、装工具、环境检查、ppc-tools、工具检查。"
version: "2.0.0"
last_updated: "2026-06-20"
status: active
data_access_level: redacted
task_type: open-ended
related_skills:
  - ppc-template-skill
  - ppc-edit-skill
  - ppc-modify-skill
---

# PPC Tools Skill — 论文创作工具链检查

检查本地论文创作工具链是否完整且完好。

**核心原则**：只检查不自动安装——发现缺失先列出，经用户同意后再装。安装时询问用户希望装到哪个目录。有特殊路径要求的工具（如只能装 C 盘）提前告知。

## 检查三要素

每个工具检查分三层：

1. **存在性** — 文件/命令是否存在于系统中
2. **完好性** — 是否能正常运行（执行 `--version` 或等效操作，确认不报错）
3. **调用地址** — 记录实际的绝对路径，检查是否在系统 PATH 中

三层中任一失败，该工具标记为异常。

## 工具路径探测策略

**不依赖硬编码路径**。对每个工具依次尝试以下方式定位：

1. `which <cmd>` 或 `where <cmd>` — 检查是否在 PATH
2. 按操作系统常见安装位置搜索（Windows: `Program Files`、`%APPDATA%`；macOS: `/usr/local`、`~/Library`；Linux: `/usr/bin`、`/opt`）
3. 环境变量（如 `JAVA_HOME`、`TEXMFHOME`）
4. 如果都找不到，标记为缺失，在报告中注明已搜索的路径范围

---

## 工具清单（17 项）

### 核心引擎

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 1 | **TeX Live** | `xelatex`、`biber`、`latexmk`、`tlmgr` 任一可执行 | `xelatex --version` / `biber --version` / `latexmk --version` 均正常执行 |
| 2 | **Pandoc** | `pandoc` 命令可用 | `pandoc --version` 正常且版本 ≥ 3.0 |
| 3 | **Java JDK** | `java` 命令可用 | `java -version` 正常且版本 ≥ 17；`JAVA_HOME` 环境变量存在且路径有效 |

### 编辑器与插件

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 4 | **VS Code** | `code` 命令可用 | `code --version` 正常执行 |
| 5 | **LaTeX Workshop** | VS Code 扩展 `james-yu.latex-workshop` 在列表中 | `settings.json` 中 LaTeX Workshop 配置段存在且 recipe 非空 |

### 文献管理

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 6 | **Zotero** | `zotero` 命令或可执行文件存在 | 版本 ≥ 7（读取 `application.ini` 或执行 `--version`） |
| 7 | **Better BibTeX** | Zotero 插件 `.xpi` 文件存在或已在 Zotero 中加载 | `unzip -t` 校验 XPI 文件完整性 |

### 检查与校对

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 8 | **chktex** | `chktex` 命令可用 | `chktex --version` 正常执行 |
| 9 | **textidote** | `textidote.jar` 文件存在 | 若 Java 就绪则 `java -jar <path> --version` 正常 |

### 格式转换

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 10 | **LibreOffice** | `soffice` 命令可用 | `soffice --version` 正常执行 |
| 11 | **Acrobat** | `Acrobat.exe` 或 `Acrobat.app` 存在 | Python COM `Dispatch('AcroExch.App')` 能连通（Windows），或应用能启动（macOS） |

### Python 生态

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 12 | **Python + 核心包** | `python3` 或 `python` 命令可用 | 逐个 `import`：`pylatex`、`pygments`、`fitz`、`pdfplumber`、`pytesseract`、`pptx`、`docx`、`win32com`（Windows） |

### OCR

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 13 | **Tesseract OCR** | `tesseract` 命令可用 | `tesseract --version` 正常且版本 ≥ 5；`tesseract --list-langs` 包含常用语言 |

### 幻灯片

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 14 | **Marp CLI** | `marp` 命令可用 | `marp --version` 正常且版本 ≥ 4 |

### 其他

| # | 工具 | 存在性检查 | 完好性检查 |
|---|------|-----------|-----------|
| 15 | **Git** | `git` 命令可用 | `git --version` 正常执行 |
| 16 | **reference.docx** | 文件存在（生成方式：`pandoc --print-default-data-file reference.docx > reference.docx`） | Python `docx.Document()` 能打开 |
| 17 | **reference.pptx** | 文件存在（生成方式：`pandoc --print-default-data-file reference.pptx > reference.pptx`） | Python `pptx.Presentation()` 能打开 |

---

## 执行流程

### Step 1：自动探测

按"工具路径探测策略"逐项定位每个工具，记录实际路径。

### Step 2：逐项检查（三要素）

每个工具按"存在性 → 完好性 → 调用地址"顺序检查：

| 状态 | 含义 |
|:---:|------|
| ✅ | 三项全部通过 |
| ⚠️ | 存在但完好性有问题（版本过低/文件损坏/配置缺失），或不在 PATH 中只能绝对路径调用 |
| ❌ | 文件/命令不存在 |

**调用地址记录**：为每个工具记录：

| 字段 | 说明 | 示例 |
|------|------|------|
| 主程序路径 | 可执行文件的绝对路径 | `/usr/local/texlive/2024/bin/x86_64-linux/xelatex` |
| 是否在 PATH | 命令行能否直接调用 | ✅ 是 / ❌ 否 |
| 命令行调用方式 | 用户可用的调用命令 | `xelatex` 或 `/path/to/xelatex` |

### Step 3：缺失/异常工具处理

对所有 ❌ 和 ⚠️ 项，汇总列表：

```
缺失/异常工具：
1. [工具名] — [状态] — [影响] — [当前调用地址（如有）]
2. ...

是否需要安装/修复这些工具？
```

**关键规则：不要自己装。** 必须等用户确认后再执行。

#### 安装前询问下载位置

对于每个待安装的工具，询问用户：

> **"你希望将 [工具名] 安装到哪个目录？"**

给出推荐路径（根据操作系统推荐常见位置），让用户确认或修改。

#### 特殊路径要求告知

某些工具有硬性安装要求，必须在询问时明确告知：

| 工具 | 路径要求 | 原因 |
|------|----------|------|
| **Acrobat** | 必须系统盘（Windows: C 盘；macOS: /Applications） | 写入注册表/系统配置 + COM/AppleScript 注册，挪动即失效 |
| **VS Code** | 推荐默认位置 | 用户级安装可自选，系统级安装需管理员权限 |
| **TeX Live** | 可任意盘/分区 | 安装时可自选路径 |
| **Java JDK** | 可任意位置（解压即用） | 需同步设置 `JAVA_HOME` 和更新 PATH |
| **Tesseract** | 可任意位置 | Windows 安装程序需要管理员权限；需同步添加 PATH |
| **Zotero** | 可任意位置 | 便携版解压即用；插件需在 Zotero 内手动加载 |
| **Python 包** | 跟随 Python 环境 | pip 自动安装到当前 Python 的 site-packages |
| **Marp CLI** | 跟随 Node.js | npm 全局安装到 npm 全局目录 |

#### 安装参数说明

安装时：
- Python 包优先使用当前网络最快的 PyPI 镜像
- 需要管理员权限的工具提前告知用户
- 从 GitHub 下载大文件时提示用户可能的网络延迟

### Step 4：安装后重新验证

安装完成后逐项重新检查，更新状态报告。

---

## 输出格式

```markdown
## PPC 工具链检查报告

### 概览
- 检查时间：YYYY-MM-DD HH:MM
- 操作系统：Windows / macOS / Linux
- 检查项：17
- 正常（✅ 三项通过）：X 项
- 异常（⚠️ 完好性/路径问题）：Y 项
- 缺失（❌ 未安装）：Z 项

### 详情

| # | 工具 | 状态 | 主程序路径 | 是否在 PATH | 命令行调用 | 完好性 | 备注 |
|---|------|:---:|------|:---:|------|:---:|------|
| 1 | TeX Live | ✅ | /path/to/xelatex | ✅ | `xelatex` | ✅ | — |
| 2 | chktex | ⚠️ | /path/to/chktex | ✅ | `chktex` | ❌ 执行报错 | 需重新安装 |
| ... | ... | ... | ... | ... | ... | ... | ... |

### 不在 PATH 中的工具

| 工具 | 调用命令（需完整路径） |
|------|----------------------|
| textidote | `java -jar /path/to/textidote.jar` |

### 特殊路径说明
（列出必须安装到特定位置的工具及其原因）

### 需要处理
（如果有缺失/异常，逐项询问用户：是否安装、安装到哪个目录）
```
