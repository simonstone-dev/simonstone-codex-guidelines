# Simon Stone Codex 指南

一组可直接给 Codex 使用的行为规则，用于减少 LLM 编码代理常见失误。本项目由 Simon Stone（`@simonstone-dev`）维护，灵感来自公开讨论中的 coding-agent 常见问题观察。

[English](./README.md) | 简体中文

## 问题所在

LLM 编码代理经常出现这些问题：

- 默默替用户做假设，然后直接执行。
- 不管理自己的困惑，也不暴露歧义。
- 把简单需求过度工程化，提前堆抽象和功能。
- 顺手修改无关代码、注释、格式或死代码。
- 用“应该可以了”结束任务，而不是用明确标准验证。

## 解决方案

用四个原则约束 Codex 的行为：

| 原则 | 解决什么问题 |
| --- | --- |
| **Think Before Coding / 编码前思考** | 错误假设、隐藏困惑、缺少权衡 |
| **Simplicity First / 简洁优先** | 过度工程、臃肿抽象、推测式功能 |
| **Surgical Changes / 精准修改** | 无关 diff、风格漂移、不安全清理 |
| **Goal-Driven Execution / 目标驱动执行** | 模糊任务、弱成功标准、不可验证修复 |

本仓库包含：

- `AGENTS.md`：项目级 Codex 指令。
- `skills/semionstone-codex-guidelines/SKILL.md`：可复用 Codex skill。
- `.codex-plugin/plugin.json`：Codex 插件打包元数据。
- `EXAMPLES.md`：常见错误和正确行为示例。

## 四个原则

### 1. Think Before Coding / 编码前思考

不要默默假设。不要隐藏困惑。把权衡说出来。

- 实现前说明关键假设。
- 如果存在多种合理解释，列出来，不要静默选择。
- 如果有更简单的方案，主动指出。
- 如果上下文不足以安全推进，停下来说明缺口并提问。

### 2. Simplicity First / 简洁优先

只写解决当前问题所需的最少代码。不要提前为未来需求付费。

- 不添加用户没有要求的功能。
- 不为一次性逻辑创建抽象。
- 不添加未被要求的配置项或扩展点。
- 不为当前系统不可能发生的路径堆叠错误处理。
- 如果实现可以明显更简单，先简化。

检验标准：资深工程师会觉得它解决了今天的问题，还是在提前解决想象中的问题？

### 3. Surgical Changes / 精准修改

只碰必须碰的代码。只清理自己制造的混乱。

- 每一行改动都应该能直接追溯到用户请求。
- 不顺手重构、格式化、改名或“改善”相邻代码。
- 匹配项目现有风格。
- 发现无关死代码或技术债，可以提到，但不要擅自删除。
- 清理由本次改动造成的未使用导入、变量、函数或文件。

### 4. Goal-Driven Execution / 目标驱动执行

把任务转成可验证目标，并循环到检查完成。

- “修 bug”变成：复现问题，修复问题，验证问题不再出现。
- “添加验证”变成：覆盖无效输入，确认有效输入仍通过。
- “重构 X”变成：重构前后行为一致。
- 多步骤任务应包含简短计划，并为每一步写明检查方式。

## 安装

### 选项 A：项目级 `AGENTS.md`

把 `AGENTS.md` 复制到使用 Codex 的项目根目录：

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/simonstone-dev/semionstone-codex-guidelines/main/AGENTS.md
```

如果项目已经有 `AGENTS.md`，请合并规则，不要覆盖项目专属指令。

### 选项 B：可复用 Codex skill

把 skill 目录复制到 Codex skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R skills/semionstone-codex-guidelines ~/.codex/skills/
```

之后在写代码、改代码、review、重构或调试时，让 Codex 使用 `semionstone-codex-guidelines`。

### 选项 C：Codex 插件包

本仓库包含 `.codex-plugin/plugin.json` 和 `skills/` 目录，可用于支持本地插件安装的 Codex 环境。

## 如何判断它在起作用

如果你看到以下现象，说明规则正在生效：

- diff 中无关改动更少。
- 第一次实现就更简单。
- 澄清问题发生在实现之前，而不是出错之后。
- PR 更干净，没有顺手重构。
- 最终回复明确说明验证了什么。

## 定制

这些规则适合和项目专属指令合并，例如：

```markdown
## Project-Specific Guidelines

- 使用 TypeScript strict mode。
- 遵循 `src/errors` 里的错误处理模式。
- 所有 API 行为变化都需要测试。
```

## 权衡说明

这些规则偏向谨慎而不是速度。对于简单拼写修复或明显的一行改动，可以自行判断。目标是减少非琐碎任务中的高成本错误，而不是拖慢简单任务。

## 致谢

本项目基于 [`forrestchang/andrej-karpathy-skills`](https://github.com/forrestchang/andrej-karpathy-skills) 的思路和结构改写；原项目源自 Andrej Karpathy 对 LLM 编码代理常见问题的观察。

本仓库将这些思路重新整理为适用于 Codex 的 `AGENTS.md`、可复用 Codex skill 和 Codex 插件元数据。本仓库是独立适配版本，不隶属于原作者。

## 许可

MIT
