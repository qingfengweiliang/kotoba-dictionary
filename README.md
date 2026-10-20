# Kotoba · 词间

一个面向 Windows 11 和 Android 的离线中英日词典。用简洁的绿色界面，把查词、收藏和复习放在一起。

当前版本：**1.2.0**。查词使用本地 SQLite 词库，不依赖在线翻译 API。

## 目录

- [功能](#功能)
- [词库规模](#词库规模)
- [下载](#下载)
- [安装与使用](#安装与使用)
- [仓库结构](#仓库结构)
- [词库引用、来源与处理方式](#词库引用来源与处理方式)
- [技术依赖与界面说明](#技术依赖与界面说明)
- [开发与构建](#开发与构建)
- [发布说明](#发布说明)
- [许可与致谢](#许可与致谢)

## 功能

- 首页提供英语、日语、中文三个圆角胶囊入口。
- 英语支持单词、短语、部分词形及中文释义反查。
- 日语支持汉字、平假名、片假名、部分常用罗马音，以及中英文释义反查。
- 中文支持简繁体、带声调／无声调／数字声调拼音，以及中英文释义检索。
- 中文词条提供“查英语释义”和“查日语释义”快捷入口。
- 收藏生词、保存最近 80 条查词历史、导出 JSON 生词本。
- 调用系统语音朗读；效果和可用语言取决于设备安装的语音包。
- 默认展示 60 条结果，可以继续加载，最多展示 300 条。
- 页面渐出渐入、查词淡入，并尊重系统“减少动态效果”设置。

查看原词条和作者时会打开外部浏览器，需要网络。除此之外应用本身不联网。

## 词库规模

| 方向 | 词目／短语条数 | 具有中文释义字段的条数 |
| --- | ---: | ---: |
| 英语 | 3,416,168 | 3,399,902 |
| 日语 | 316,014 | 120,503 |
| 中文 | 159,926 | 73,627 |

统计来自 1.2.0 数据构建结果。搜索别名、拼音和罗马音不重复计为词条；日语同形但不同读音的词条可能分别计数。词库包含词组、专业术语、古语和部分名称。

优先显示已有中文释义；缺少中文时会标注英文补充。释义来自下列数据源，没有使用机器翻译补写。开源词库仍可能有遗漏或错误，不代表收录了所有词语、词义。

## 下载

大型安装包放在 [Releases](../../releases) 中，不进入 Git 历史。

| 平台 | 文件 | 大小 | 适用环境 |
| --- | --- | ---: | --- |
| Windows | `Kotoba-Windows-1.2.0.zip` | 约 853 MiB | Windows 11 x64 |
| Android | `Kotoba-Android-1.2.0.zip`（内含 APK） | 约 640 MiB | Android 8.0 及以上 |
| 文档 | `Kotoba-GitHub-Docs-1.2.0.zip` | 约 29 KiB | README、许可证与数据元信息 |

最新发行版：[`v1.2.0`](../../releases/latest)

- Windows：<https://github.com/qingfengweiliang/kotoba-dictionary/releases/download/v1.2.0/Kotoba-Windows-1.2.0.zip>
- Android：<https://github.com/qingfengweiliang/kotoba-dictionary/releases/download/v1.2.0/Kotoba-Android-1.2.0.zip>
- 文档包：<https://github.com/qingfengweiliang/kotoba-dictionary/releases/download/v1.2.0/Kotoba-GitHub-Docs-1.2.0.zip>

下载后可用仓库根目录的 [`SHA256SUMS.txt`](SHA256SUMS.txt) 校验完整性：

```powershell
# Windows PowerShell
Get-FileHash .\Kotoba-Windows-1.2.0.zip -Algorithm SHA256
```

```bash
# Linux / macOS
sha256sum -c SHA256SUMS.txt
```

## 安装与使用

### Windows

适用 **Windows 11 x64**。

1. 下载 Windows ZIP 并完整解压（不要只解压出 `Kotoba.exe`）。
2. 打开 `Kotoba.exe`。
3. 保留同目录下的 `resources` 等文件，移动程序时请移动整个文件夹。
4. 若双击后没有反应或立即退出，把整个文件夹移动到当前用户目录（例如 `%LOCALAPPDATA%\Kotoba\1.2.0`）后再启动：在受限的临时目录下运行时，程序创建 Chromium 沙箱窗口站可能被系统拒绝。

使用发行版不需要安装 Node.js。`Ctrl+K` 聚焦查询框，`Esc` 返回发现页。收藏和历史存放于 `%APPDATA%\kotoba-dictionary\learning.json`，可另行备份。

平台说明：[`windows/使用说明.md`](windows/使用说明.md)

### Android

适用 **Android 8.0 及以上**，需要可用的系统 WebView，支持 ARM / ARM64 / x86 / x86_64。

1. 下载安卓版 ZIP，解压出 APK。
2. 允许所用浏览器或文件管理器安装应用，然后安装 APK。
3. 首次打开等待离线词库解压完成，界面会显示进度。

`versionName 1.2.0`、`versionCode 3`，使用相同签名的新版本直接覆盖安装，可以保留收藏和历史。卸载应用会删除其本地数据，卸载前请导出生词本；目前不支持 JSON 导入。

平台说明：[`android/使用说明.md`](android/使用说明.md)

### 存储空间

完整词库数据库为 **1,695,350,784 字节，约 1.58 GiB**。下载包、安装文件与解压后的数据库会额外占用空间，建议至少预留 **4 GB**。

SQLite 按需查询，应用不会在启动时把全部词条加载到内存。安卓首次解压较慢，后续打开直接使用已准备的数据库。

## 仓库结构

```text
kotoba-dictionary/
├─ README.md                    本文件
├─ LICENSE                      应用代码 MIT 许可
├─ SHA256SUMS.txt               发行包 SHA-256 校验值
├─ data/                        数据来源、版本与统计元信息
│  ├─ sources.json
│  ├─ expansion-meta.json
│  ├─ ja-zh-meta.json
│  └─ jmdict-meta.json
├─ licenses/                    各词库的署名与许可原文
│  ├─ ECDICT-MIT.txt
│  ├─ JMdict-CC-BY-SA-4.0.txt
│  ├─ jmdict-simplified-LICENSE.txt
│  ├─ Wiktionary-Attribution.txt
│  ├─ Wiktionary-Expansion-Attribution.txt
│  └─ CC-CEDICT-Attribution.txt
├─ windows/                     Windows 版平台说明
│  └─ 使用说明.md
└─ android/
   └─ 使用说明.md
```

安装包、APK 与 `dictionary.db`（1.6 GB）体积超过 GitHub 普通 Git 的单文件限制，因此通过 Releases 分发；本仓库保存说明文档、许可证与数据元信息。

## 词库引用、来源与处理方式

以下是实际采用的数据源；数据作者保留其权利。

| 来源 | 在本项目中的用途 | 链接 | 许可 |
| --- | --- | --- | --- |
| ECDICT / skywind3000 及贡献者 | 英语词目、音标、释义、词形和标签；使用 `stardict.7z` 完整数据，并以基础数据补漏 | [GitHub](https://github.com/skywind3000/ECDICT) | MIT |
| JMdict / EDRDG 及贡献者 | 日语词目、假名读音、词性和英文释义 | [JMdict 项目](https://www.edrdg.org/wiki/index.php/JMdict-EDICT_Dictionary_Project) | CC BY-SA 4.0 |
| jmdict-simplified / scriptin 及贡献者 | JMdict 的 JSON 数据分发，作为日语基础数据的输入 | [GitHub](https://github.com/scriptin/jmdict-simplified) | 所用数据按 CC BY-SA 4.0 分发 |
| 中文维基词典各词条贡献者 | 中、英、日相关词目及中文解释 | [中文维基词典](https://zh.wiktionary.org/) | 本项目选用 CC BY-SA 4.0 分发相关文本衍生数据 |
| 英语维基词典各词条贡献者 | 补充日语词目及英文解释 | [英语维基词典](https://en.wiktionary.org/) | 本项目选用 CC BY-SA 4.0 分发相关文本衍生数据 |
| Kaikki.org / Wiktextract，Tatu Ylonen 及贡献者 | 提供维基词典的机器可读抽取数据；词条内容归原词条贡献者 | [中文抽取](https://kaikki.org/zhwiktionary/) · [日语英文抽取](https://kaikki.org/dictionary/Japanese/) · [Wiktextract](https://github.com/tatuylonen/wiktextract) | 所用词条文本遵循上述 CC BY-SA 4.0 |
| CC-CEDICT 社区贡献者 / MDBG | 中文词目、繁简字形、拼音和英文释义 | [项目与说明](https://www.mdbg.net/chinese/dictionary?page=cc-cedict) · [贡献者与编辑](https://cc-cedict.org/editor/editor.php) | CC BY-SA 4.0 |

### 数据版本及加工

- JMdict JSON：`3.6.2+20261019180022`，词典日期为 2026-10-19。
- 用于日语中文释义的中文维基词典抽取：转储日期 2026-10-01，抽取日期 2026-10-18。
- CC-CEDICT 快照：2026-10-19T05:01:26Z；该上游快照有 125,301 条记录，合并后不直接以此作为本项目中文词条总数。
- ECDICT 和新增维基词典抽取文件的具体输入由 SHA-256 标识，见 [`data/expansion-meta.json`](data/expansion-meta.json)；不要把浮动下载地址当作固定版本。
- 处理包括筛选释义、按语言与词目合并、添加搜索别名、构建 SQLite 索引。
- 中文词目和解释使用 OpenCC 转为简体；日语词目保持原字形。CC-CEDICT 拼音的数字声调转换为声调符号，同时建立检索别名。
- 每条相关维基词典记录保留原页面标题，界面“原词条与作者”可打开来源；来源页面的历史记录列出作者。可在原页面地址后添加 `?action=history` 查看。

详细记录见 [`data/sources.json`](data/sources.json)、[`data/expansion-meta.json`](data/expansion-meta.json)、[`data/ja-zh-meta.json`](data/ja-zh-meta.json) 和 [`data/jmdict-meta.json`](data/jmdict-meta.json)。`sources.json` 中早期 `counts` 为历史基础数据，1.2.0 统计以 `expandedDictionary.counts` 及 `expansion-meta.json` 为准。

## 技术依赖与界面说明

界面以 HTML、CSS 和 JavaScript 编写，采用浅色背景、绿色点缀、圆角胶囊与轻量过渡。字体调用设备已有的系统字体，图标主要使用文本符号及 CSS。现有实现未记录直接复用的第三方 UI 模板。

| 项目 | 用途 | 项目地址 |
| --- | --- | --- |
| Electron | Windows 桌面容器 | https://github.com/electron/electron |
| Node.js | 桌面运行环境及内置 `node:sqlite` | https://github.com/nodejs/node |
| SQLite | 本地词库、索引和 FTS4 全文检索 | https://www.sqlite.org/ |
| electron-builder | Windows 应用打包 | https://github.com/electron-userland/electron-builder |
| Android SDK / WebView / TextToSpeech | 安卓应用、网页界面承载和系统语音 | https://developer.android.com/ |
| esbuild | 安卓前端 JavaScript 构建与兼容性处理 | https://github.com/evanw/esbuild |
| Playwright | 桌面及移动尺寸界面自动化验证 | https://github.com/microsoft/playwright |
| OpenCC | 数据准备时的中文繁简转换 | https://github.com/BYVoid/OpenCC |
| opencc-python-reimplemented | 实际使用的 Python 繁简转换实现；仅用于准备数据 | https://github.com/yichen0831/opencc-python |

上述表格列出主要直接依赖和工具，完整版本及传递依赖以各项目的 `package-lock.json`、实际构建环境和发行版自带许可文件为准。Electron 发行包内的 Chromium 等第三方许可文件也应保留。

## 开发与构建

以下命令按两个现有工程分别说明。合并到同一 GitHub 仓库时，可保留 `kotoba/` 和 `kotoba-android/` 两个子目录，在仓库根目录放本 README。

需要 Node.js 24+、Python 3。安卓构建另外需要 JDK 21、Android SDK Platform 35 和 Build Tools 35。

数据库体积超过 GitHub 普通 Git 的单文件限制。源码克隆后，需要从发行版获取配套 `dictionary.db`，或使用数据脚本重建。数据库应放到：

- 桌面：`kotoba/data/dictionary.db`
- 安卓：`kotoba-android/app/src/main/assets/dictionary.db`

Windows ZIP 内的配套数据库位于 `resources/app.asar.unpacked/data/dictionary.db`，APK 内位于 `assets/dictionary.db`。提取时保留随附的数据来源、署名与许可文件。

### 桌面工程

在 `kotoba/` 中执行：

```bash
npm ci
npm test
npm start
```

构建 Windows x64 ZIP：

```bash
npm run build:win
```

### 安卓工程

在 `kotoba-android/` 中执行：

```bash
npm ci
npm run build:web
python scripts/build_apk.py --sdk /path/to/android-sdk --jdk /path/to/jdk-21
```

请将路径替换为自己的 SDK 和 JDK 路径。脚本使用本地 SDK 工具编译、对齐和签名。

首次独立构建会生成自己的签名密钥；该密钥与已有发行包不同，不能直接覆盖原签名安装。维护同一个应用时应安全保留原签名密钥及密码，保证后续更新使用同一签名。

### 数据重建

桌面工程的 `scripts/build_full_dictionary.py` 负责生成三语数据库，数据准备需要 `opencc-python-reimplemented`。输入除 ECDICT、Kaikki 和 CC-CEDICT 原始文件外，还包括已有的基础语言数据；需要按脚本要求准备输入，不能仅凭源码中的统计 JSON 重建数据库。

### 验证范围

1.2.0 已验证词库查询、三语切换、移动尺寸布局、收藏与历史、旧收藏刷新、动画和底部选中框。安卓运行验证采用 Android 8 / API 26 和 Chrome 69 WebView 环境，并修复旧版 SQLite 的全文检索语法兼容问题。桌面打包界面在 Linux 下验证；Windows x64 发行包已构建，尚未在 Windows 系统中执行验证。

## 发布说明

- Git 仓库保存 README、许可证、来源元数据和发行包内的说明文档。
- APK、Windows ZIP 及大型词库放到 Releases，便于下载与版本管理。
- 忽略 `node_modules/`、构建输出、临时原始数据和大型生成数据库。
- 安卓 `signing/` 目录包含私钥和密码，必须排除在公开源码和上传压缩包之外，并自行安全备份。
- 上传源码 ZIP 后，GitHub 不会自动展开为源文件，需要解压后提交目录中的文件。

## 许可与致谢

应用自有代码使用 **MIT License**，见 [`LICENSE`](LICENSE)。**应用代码的 MIT 许可不替代词库和第三方组件各自的许可。**

ECDICT 数据保留 MIT 声明；JMdict、维基词典及 CC-CEDICT 相关衍生材料按 **CC BY-SA 4.0** 分发。再分发这些材料时，请保留署名、来源、许可链接和修改说明，并遵守相同方式共享要求。

随附 [`licenses/`](licenses) 内包含：

- `ECDICT-MIT.txt`
- `JMdict-CC-BY-SA-4.0.txt`
- `jmdict-simplified-LICENSE.txt`
- `Wiktionary-Attribution.txt`
- `Wiktionary-Expansion-Attribution.txt`
- `CC-CEDICT-Attribution.txt`

CC BY-SA 4.0 完整许可文本：[Creative Commons](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

感谢各词典的作者、维护者及所有贡献者。本项目与 EDRDG、Wikimedia、Kaikki、MDBG 等机构无隶属关系，不表示获得其背书。
