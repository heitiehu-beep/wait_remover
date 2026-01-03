# Wait Remover & Optimizer Plugin

一个针对 MaiBot 的核心逻辑优化插件。旨在解决机器人频繁触发 `wait` (等待) 动作导致对话卡顿的问题，提供“智能优化”和“强制移除”两种模式，让对话响应更加流畅自然。

## ✨ 核心功能

* **智能优化 Wait 动作 (推荐)**：重写了 `wait` 的触发逻辑，让机器人仅在真正需要“倾听”或“留白”时才等待，而非频繁挂起。
* **强制移除 Wait 动作**：在私聊等需要即时反馈的场景下，可以完全禁用等待，强制机器人进行回复。
* **优化对话结束判定**：调整了 `complete_talk` 的提示词，防止机器人在对方短暂沉默时过早结束话题。
* **热修补机制**：采用直接修改 `global_prompt_manager` 的方式，确保提示词修改能即时生效，无需修改核心源码。

## 📦 安装

1. 下载本插件文件夹。
2. 将文件夹放入 MaiBot 的 `plugins` 目录下（例如 `plugins/wait_remover`）。
3. 重启 MaiBot。

## ⚙️ 配置

插件首次运行后会自动生成 `config.toml`，你可以手动修改配置：

```toml
[plugin]
# 配置版本 (请勿修改)
config_version = "1.0.2"

# 【模式一：智能优化】(推荐)
# 是否改善 wait 动作的逻辑。
# 开启后，机器人会保留 wait 能力，但仅在需要对方把话说完或保持安静时触发，减少无意义的等待。
change_wait_action = true

# 【模式二：强制移除】
# 是否彻底移除 wait 动作。
# 开启后，机器人将失去 wait 能力，必须对每条消息进行回复或结束对话。
remove_wait_action = false
```

### 配置建议

* **日常使用**：建议保持默认 (`change_wait_action = true`, `remove_wait_action = false`)。
* **强迫症**：如果希望机器人永远秒回，可以开启 `remove_wait_action = true`(可能出现副作用)

## 🛠️ 技术原理

本插件通过 Hook 系统的 `global_prompt_manager`，动态替换了 `brain_planner` 中的 `Prompt` 模板。
它修改了以下 Action 的定义：

- `wait` (增强或移除)
- `complete_talk` (调整触发阈值)

*注意：本插件可能会与其他修改了 `Planner` 提示词的插件产生冲突。*

## 📝 更新日志

**v1.0.2**

- 修复了 Patch 无法生效的问题（改为直接操作 Prompt Manager）。
- 新增 `change_wait_action` 选项，支持优化而非暴力移除。
- 优化了提示词文本，解决缩进导致的潜在解析问题。

## 作者

elevenfd

