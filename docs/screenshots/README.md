# 界面截图

Kotoba 词间 1.2.0 的界面截图，两个平台各六张。图片为应用运行时直接截取，未做界面重绘或生成式修改；仅关闭了过渡动画以保证静态画面清晰。

## Windows（发行包实机运行）

Windows 11 x64 发行包实际运行，窗口 1286 × 863，缩放 1.5，输出 1929 × 1295 PNG。

| 文件 | 画面 | 说明 |
| --- | --- | --- |
| `windows-01-home.png` | 发现词语 / 首页 | 英语、日语、中文三个语言胶囊、查词框、偶遇一个好词 |
| `windows-02-english.png` | 英语查词 | 查询 `serendipity`，右侧词条含音标、标签与中文释义，左侧 8 条匹配 |
| `windows-03-japanese.png` | 日语查词 | 查询 `勉強`，假名读音、中文释义优先，英文释义可展开 |
| `windows-04-chinese.png` | 中文拼音检索 | 带声调拼音 `xuéxí` 命中「学习」，支持中英双向反查 |
| `windows-05-favorites.png` | 生词本 | 已收藏 3 词，可导出 JSON |
| `windows-06-history.png` | 最近查词 | 最近 80 条查词记录，仅保存在本机 |

## Android（模拟器渲染）

Android 8 / API 26 模拟器，Chrome 69 WebView，Kotoba 1.2.0，输出 720 × 1280 PNG。**这些是模拟器截图，不是实体手机真机截图。**

| 文件 | 画面 | 说明 |
| --- | --- | --- |
| `android-01-home.png` | 首页与三语选择 | 移动布局下的三语胶囊与底部导航 |
| `android-02-english.png` | 英语查询与中文释义 | 查询 `hello`，中文释义优先显示 |
| `android-03-japanese.png` | 日语假名查询与中文释义 | 查询 `がっこう`，27 条匹配，「学校」含其他写法与中文释义 |
| `android-04-chinese.png` | 中文查询、拼音与繁体字形 | 拼音 `xuéxì` 命中「学习」，同屏可见繁体字形 |
| `android-05-wordbook.png` | 生词本 | 收藏列表与导出入口 |
| `android-06-history.png` | 最近查词 | 查词记录列表 |

演示用的收藏与历史由实际查询和收藏操作产生，不是预先写入的假数据。

## 清单

`manifest.json` 记录全部 12 张图片的平台、运行环境、尺寸与 SHA-256，可用于核对文件完整性。

```bash
# 校验单张图片
sha256sum docs/screenshots/windows-01-home.png
```
