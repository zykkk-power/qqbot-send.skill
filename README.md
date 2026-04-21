# qqbot-send

An OpenClaw skill for sending files through QQBot end to end.

It combines two responsibilities into one skill:

1. stage local files into the QQBot media relay directory
2. send the staged path with QQ rich-media tagging

## What this skill does

When a user asks to send a local file to QQ, this skill decides the safest path automatically.

### Local absolute-path file
For files like desktop files, downloads, and workspace-external files, the skill:

1. runs `python scripts/stage_media.py <source_path>`
2. copies the file into:
   - `~/.openclaw/media/qqbot/`
3. uses the returned path with:
   - `<qqmedia>...</qqmedia>`

### Already staged file
If the file is already inside the QQBot relay directory, it sends it directly.

### URL media
If the source is an HTTP(S) URL, it can be sent directly with `<qqmedia>...</qqmedia>`.

## Repository layout

```text
qqbot-send/
├─ SKILL.md
├─ README.md
├─ LICENSE
├─ .gitignore
└─ scripts/
   └─ stage_media.py
```

## Script behavior

The bundled script:

- accepts a single file path argument
- checks that the file exists
- rejects files larger than 10 MB
- preserves the file extension
- copies the file instead of changing the original
- writes to `~/.openclaw/media/qqbot/`
- prints the staged absolute path

## Example

```bash
python scripts/stage_media.py "C:\Users\zyk\Desktop\resume.pdf"
```

Example output:

```text
C:\Users\zyk\.openclaw\media\qqbot\9b0f3b80-19b2-410b-adfc-438d6028b93a.pdf
```

Then send:

```xml
<qqmedia>C:\Users\zyk\.openclaw\media\qqbot\9b0f3b80-19b2-410b-adfc-438d6028b93a.pdf</qqmedia>
```

## Notes

- This skill replaces the old split flow where staging and sending were handled by separate skills
- The final delivery format still uses QQBot rich-media tagging
- Staged media output is not part of this repository

# qqbot-send

一个用于通过QQBot端到端发送文件的OpenClaw技能。

它将两项职责合并为一项技能：

1. 将本地文件暂存到QQBot媒体中继目录
2. 发送带有QQ富媒体标签的已暂存路径

## 此技能的功能

当用户请求将本地文件发送到QQ时，这项技能会自动决定最安全的路径。

### 本地绝对路径文件
对于像桌面文件、下载文件和工作区外部文件这样的文件，技能：

1. 运行 `python scripts/stage_media.py <source_path>`
2. 将文件复制到： 
   - `~/.openclaw/media/qqbot/`
3. 使用返回的路径： 
   - `<qqmedia>...</qqmedia>`

### 已经暂存的文件
如果文件已经在QQBot中继目录内，它会直接发送。

### URL 媒体
如果源是一个HTTP(S) URL，可以直接用`<qqmedia>...</qqmedia>`发送。

## 仓库布局

qqbot-send/
├─ SKILL.md
├─ README.md
├─ LICENSE
├─ .gitignore
└─ scripts/
   └─ stage_media.py

## 脚本行为

捆绑的脚本：

- 接受一个文件路径参数
- 检查文件是否存在
- 拒绝大于10 MB的文件
- 保留文件扩展名
- 复制文件而不是更改原始文件
- 写入到 `~/. openclaw/media/qqbot/`
- 打印暂存的绝对路径

## 示例

```bash
python scripts/stage_media.py  "C:\Users\zyk\Desktop\resume.pdf"

示例输出：

C:\Users\zyk\.openclaw\media\qqbot\9b0f3b80-19b2-410b-adfc-438d6028b93a.pdf

然后发送：

```xml
<qqmedia>C:\Users\zyk\.openclaw\media\qqbot\9b0f3b80-19b2-410b-adfc-438d6028b93a.pdf</qqmedia>
```

## 注释

- 这个技能取代了旧的分流流程，其中分阶段和发送由不同的技能处理。
- 最终交付格式仍然使用  QQBot 富媒体标记 
- 已分阶段的媒体输出不属于此存储库
