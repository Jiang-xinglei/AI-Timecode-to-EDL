# AI-Timecode-to-EDL
这是一段AI系统提示词，让AI变成edl转化工具，减轻流水线剪辑的工作量。安全，稳定，免费！！！

```markdown
# Mark1.0 – AI 剪辑协作工具（EDL 生成器）
# Mark1.0 – AI Editing Assistant (EDL Generator)

> 让 AI 成为你的剪辑助理：根据自然语言时间点需求，自动生成符合 Premiere Pro 标准的 EDL 文件，专为流水线剪辑设计。
>
> Turn your AI into an editing assistant: automatically generate Premiere Pro‑compatible EDL files from natural language timecode requests, designed for pipeline editing.

---

## 📖 简介 | Introduction

**Mark1.0** 是一段系统提示词，将其发送给 AI（推荐 DeepSeek、ChatGPT 等）后，AI 会按照固定规则将用户提供的时间点需求转换为 **EDL（Edit Decision List）** 文件。该文件可直接导入 Premiere Pro，用于快速标记、剪切素材，大幅减少手动查看时间码的时间。

本工具针对中等及以上复杂度的剪辑项目设计，基于 **30 帧/秒（30.00 fps 非丢帧）** 工作。请始终将 AI 输出作为辅助，最终剪辑效果仍需人工判断。

> Mark1.0 is a system prompt. When given to an AI (DeepSeek, ChatGPT, etc.), it instructs the AI to convert user‑provided timecode requests into an EDL file. The EDL can be imported into Premiere Pro for fast marking and cutting, significantly reducing manual timecode checking.
>
> Designed for mid‑ to high‑complexity editing projects, based on **30 fps (30.00 fps non‑drop)** . Always use AI output as an assistant – final editing decisions require human review.

---

## ✨ 特性 | Features

- 🧠 **智能时间码解析** – 支持中文数字、全角/半角、多种分隔符，自动识别“从…到…”时间段。
- 🎬 **生成标准 EDL** – 输出 CMX3600 格式 EDL，兼容 Premiere Pro（NTSC 编码）。
- ⏱️ **气口与默认时长处理** – 可自动添加 15 帧气口（0.5 秒）或按指定秒数调整。
- 🔄 **避免首尾相连** – 自动检测并微调相邻事件，避免时间码重叠或衔接帧错误。
- 🧩 **纯提示词驱动** – 无需 API 密钥、无需后台程序，完全通过人工复制粘贴操作，安全合规。

> - 🧠 **Smart timecode parsing** – Supports Chinese numerals, full/half‑width characters, various delimiters, and automatically recognises “from … to …” ranges.
> - 🎬 **Standard EDL output** – CMX3600 format, compatible with Premiere Pro (NTSC encoding).
> - ⏱️ **Breath / pause handling** – Automatically adds 15‑frame breath (0.5 sec) or user‑defined seconds.
> - 🔄 **Avoids back‑to‑back events** – Detects and slightly adjusts consecutive events to prevent overlapping frames.
> - 🧩 **Prompt‑only workflow** – No API keys, no background scripts; fully manual copy‑paste – safe and compliant.

---

## 🧩 系统提示词（核心）
## 🧩 System Prompt (Core)

将以下代码块中的内容完整复制，作为 **系统提示词（System Prompt）** 发送给 AI（推荐 DeepSeek）。

> Copy the content inside the code block below and send it as the **system prompt** to your AI (DeepSeek recommended).

```text
#角色
你是一个剪辑师助理。请等待用户提供时间点需求，再根据规则生成EDL文件代码块。不要在用户提供需求之前自行输出任何EDL示例。

#规则（收到需求后执行）
##时间码智能转换
1. 全角转半角，中文数字/单位转标准时间码（如“八分二十秒” → 00:08:20:00）。
2. 非标准分隔符（- 、-- 、中文描述）转标准冒号分隔。
3. 仅分钟数（如“8分钟”） → 00:08:00:00。
4. 时间段支持“从…到…”，自动识别起止。
5. 气口处理：
   一般气口（未注明时长）：在说话结束点之后加 **15 帧**（0.5秒）。例如00:03:58 → 00:03:58:00至00:03:58:15。
   特殊气口（注明“加X秒”）：按X秒 × 30帧计算。
6. 其他单个时间点（非气口且无时长）：默认顺延 **1 秒**（秒位+1，帧位不变）。
7. 无时间码的需求（如“处理声音”） → 忽略，不生成任何事件。

##输出规则
1. 帧率：30.00 fps 非丢帧，时间码格式 时:分:秒:帧（冒号，帧范围00-29）。
2. 时间段：直接使用起止时间。
3. **TITLE 行必须使用英文**，例如 `TITLE: EDL_Markers_30fps`（避免中文乱码）。
4. 其余格式严格遵循下方【范例 EDL】。
5. 只输出 EDL 代码块，无解释、无转换过程。
6. 每次生成全新 EDL，事件编号从 001 开始。
7. **避免首尾相连**：生成事件前，先将所有事件按起始时间升序排序。然后遍历，若当前事件的起始时间 **等于** 上一个事件的结束时间（即紧紧相连），则将当前事件的起始时间和结束时间 **均向后移动 1 帧**（保持原时长不变）。注意帧进位（例如 00:00:05:29 加1帧 → 00:00:06:00）。此规则在每个拆分的 EDL 文件内部独立执行，跨文件不调整。

# 范例 EDL（仅展示格式，不要输出此段）
TITLE: EDL_Markers_30fps
FROM CLIP NAME: Source
001 001 V C 00:00:03:58:00 00:00:03:58:15 00:00:03:58:00 00:00:03:58:15
002 001 V C 00:00:03:58:16 00:00:05:04:00 00:00:03:58:16 00:00:05:04:00

# 首次回复专属内容（只输出一次，不写入代码块）
【使用说明】
请提供您的时间点需求（支持中文、半角/全角、分隔符等）。我将根据上述规则生成 EDL 代码块。
导入方法：将代码块保存为 .edl 文件（ANSI 编码），在 Premiere Pro 中导入（文件 → 导入），选择 NTSC 编码，得到嵌套序列后对齐主序列时间码，即可用快捷键标记（↑ 切换片段，M 标记，shift+M 下一个，ctrl+shift+M 上一个）。

注意：我不会在您提供需求前输出任何 EDL 代码。请直接发送您的时间点列表。

# 等待用户输入
完成首次回复（只输出【使用说明】）后，立即停止，等待用户提供需求。用户提供需求后，再根据规则生成 EDL。
```

---

## 🚀 如何使用 | How to Use

### 1️⃣ 将系统提示词发送给 AI
- 打开 AI 对话界面（推荐 DeepSeek 网页版，免费）。
- 将上面的整个代码块内容作为 **系统提示词** 粘贴并发送。
- AI 会回复一段固定的「使用说明」，此时 **不会** 输出任何 EDL。

### 2️⃣ 提供你的时间点需求
- 直接发送你的时间码列表（支持中文自然语言）。例如：
  ```
  1. 采访镜头 00:05:12:10 到 00:07:30:05
  2. B-roll 从 8分20秒开始，持续 1分10秒
  3. 主持人在 01:02:03:04 说完话后加0.5秒气口
  ```
- AI 会根据规则生成 EDL 代码块（纯文本）。

### 3️⃣ 保存为 EDL 文件
- 复制 AI 输出的 EDL 代码块（从 `TITLE:` 开始到最后一个事件）。
- 粘贴到文本编辑器（如记事本），保存文件，**文件名以 `.edl` 结尾**，编码选择 **ANSI**。
  - Windows 记事本默认 ANSI 即可。
  - macOS 用户可使用 VS Code 或 CotEditor 手动选择编码。

### 4️⃣ 导入 Premiere Pro
- 在 Premiere Pro 中：`文件` → `导入` → 选择刚才保存的 `.edl` 文件。
- 导入设置选择 **NTSC 编码**。
- 导入后得到一个嵌套序列，将其拖到主序列上，**对齐主序列的起始时间码**（通常为 01:00:00:00）。
- 使用快捷键快速标记：
  - `↑` / `↓` 切换片段
  - `M` 添加标记
  - `Shift+M` 下一个标记
  - `Ctrl+Shift+M` 上一个标记

> 详细使用方法可参考项目中的 [完整工作流文档](WORKFLOW.md)（稍后提供）。

---

### 1️⃣ Send the system prompt to the AI
- Open an AI chat interface (DeepSeek web version is free and recommended).
- Copy the entire system prompt (the large code block above) and send it as the first message.
- The AI will reply with a fixed “usage guide” – it will **not** output any EDL yet.

### 2️⃣ Provide your timecode requests
- Send your list of timecode requirements (natural language, Chinese or English). Example:
  ```
  1. Interview clip 00:05:12:10 to 00:07:30:05
  2. B-roll from 8 minutes 20 seconds, duration 1 minute 10 seconds
  3. Host finishes speaking at 01:02:03:04, add 0.5 sec breath
  ```
- The AI will generate an EDL code block following the rules.

### 3️⃣ Save as an EDL file
- Copy the EDL code block (from `TITLE:` to the last event).
- Paste into a text editor (Notepad, VS Code, etc.) and save with **`.edl`** extension, encoding **ANSI**.
  - Windows Notepad uses ANSI by default.
  - On macOS, use CotEditor or VS Code and manually select ANSI encoding.

### 4️⃣ Import into Premiere Pro
- In Premiere Pro: `File` → `Import` → choose the `.edl` file.
- In the import dialog, select **NTSC** encoding.
- A nested sequence will appear. Drag it onto your master sequence and **align its start timecode** with the master sequence (usually 01:00:00:00).
- Use keyboard shortcuts for fast marking:
  - `↑` / `↓` navigate clips
  - `M` add marker
  - `Shift+M` next marker
  - `Ctrl+Shift+M` previous marker

---

## 📋 规则速查 | Rule Quick Reference

| 场景 | 处理方式 |
|------|----------|
| 中文数字 / 全角符号 | 自动转成标准时间码（如 `八分二十秒` → `00:08:20:00`） |
| 只有分钟数 | 转换为 `00:分钟:00:00` |
| 普通时间点（无时长） | 顺延 1 秒（秒位+1，帧位不变） |
| 未注明时长的气口 | 添加 **15 帧**（0.5 秒） |
| 注明“加 X 秒”的气口 | 按 X×30 帧添加 |
| 无时间码的需求 | 忽略，不生成 EDL 事件 |
| 相邻事件首尾相连 | 将后一个事件整体向后移 1 帧 |

> 完整规则请查看上方的系统提示词。

| Scenario | Action |
|----------|--------|
| Chinese numerals / full‑width chars | Convert to standard timecode (`八分二十秒` → `00:08:20:00`) |
| Minutes only | Convert to `00:minutes:00:00` |
| Single time point (no duration) | Extend by 1 second (seconds+1, frames unchanged) |
| Breath without specified duration | Add **15 frames** (0.5 sec) |
| Breath with “add X seconds” | Add X × 30 frames |
| Request without any timecode | Ignore, generate no EDL event |
| Back‑to‑back events | Shift the later event forward by 1 frame |

> See the full system prompt for all rules.

---

## ⚠️ 注意事项 & 已知限制 | Notes & Known Limitations

- **避免事件交叠**：如果需求中存在时间范围互相交叉或重叠的情况，生成的 EDL 可能不符合预期。建议将交叠的需求分批次生成不同的 EDL 文件。
- **帧率一致性**：请确保你的 Premiere Pro 序列也是 **30 fps 非丢帧**，否则导入后标记位置可能出现最多 3 秒左右的偏移。长时间序列可能累积 10–30 帧的误差。
- **编码问题**：保存 `.edl` 文件时务必使用 **ANSI 编码**，否则 Premiere Pro 可能无法正确识别中文字段（TITLE 行已强制英文，但文件整体仍建议 ANSI）。
- **纯辅助工具**：始终人工检查 AI 生成的时间码是否准确，特别是在复杂剪辑中。

> - **Avoid overlapping events** – If your timecode ranges overlap, the EDL may be invalid. Split overlapping requests into separate EDL files.
> - **Frame rate consistency** – Make sure your Premiere Pro sequence is also **30 fps non‑drop**. Otherwise markers may drift (up to ~3 seconds, or 10–30 frames for long sequences).
> - **Encoding** – Always save the `.edl` file with **ANSI** encoding, otherwise Premiere Pro may fail to read it.
> - **Assistant‑only** – Always double‑check AI‑generated timecodes, especially in complex edits.

---

## 🐞 常见问题 | FAQ

**Q: AI 没有输出 EDL，只输出了「使用说明」？**  
A: 这是正常行为。你需要 **先发送系统提示词**，待 AI 回复使用说明后，**再发送你的时间点列表**。AI 不会主动询问。

**Q: Premiere Pro 导入 EDL 后素材离线 / 找不到源素材？**  
A: 确保你的源素材文件名与 EDL 中 `FROM CLIP NAME` 一致（默认为 `Source`）。你可以在导入后手动重新链接素材。也可以在生成 EDL 前向 AI 指定素材名称。

**Q: 生成的标记位置不准确，偏移了几秒？**  
A: 最常见原因是 **序列帧率不是 30 fps**。请检查你的序列设置，确保为 `30.00 fps` 非丢帧。另外避免首尾相连规则已内置，如果仍有小误差，可手动微调标记。

**Q: 可以在命令行或其他自动化工具中使用吗？**  
A: 当前版本 **仅设计为人工复制粘贴流程**，不包含任何 API 调用或自动化脚本。如需自动化集成，请自行评估风险并遵守公司安全政策。

> **Q: AI only outputs the “usage guide”, not EDL.**  
> A: That’s expected. First send the **system prompt**, then after the AI replies with the guide, **send your timecode requests**. The AI will then generate the EDL.
>
> **Q: Media offline / wrong clip after EDL import?**  
> A: Make sure your source clip name matches the `FROM CLIP NAME` line (default is `Source`). You can relink manually, or ask the AI to use a specific name before generation.
>
> **Q: Markers are slightly off (a few seconds).**  
> A: Most likely your sequence frame rate is not exactly 30 fps non‑drop. Double‑check sequence settings. Small drift may still occur – adjust markers manually.
>
> **Q: Can I use this in an automated script / CI pipeline?**  
> A: This version is designed for **manual copy‑paste only**. No API keys, no background processes. For automation, please review your company’s security policies.

---

## 📌 版本记录 | Version History

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| 1.0 | 2025-05-09 | 初始版本（半自动，支持自动拆分时间码） |

> | Version | Date | Changes |
> |---------|------|---------|
> | 1.0 | 2025-05-09 | Initial release (semi‑automatic, timecode splitting) |

---

## 📄 许可证 | License

本项目采用 **MIT 许可证**。你可以自由使用、修改、分发，但请保留原始版权声明。  
This project is licensed under the **MIT License**. Feel free to use, modify, and distribute, but please retain the original copyright notice.

---

## 🙋 反馈与贡献 | Feedback & Contributions

如果你在使用中遇到 Bug 或有改进建议，欢迎提交 Issue 或 Pull Request。  
If you encounter bugs or have suggestions, feel free to open an Issue or Pull Request.

**Happy editing! 🎬**
```
