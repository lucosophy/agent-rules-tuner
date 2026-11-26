# agent-rules-tuner

[English](README.md) | 简体中文

面向任意 AI 编码代理的通用全局与项目规则调优 Skill（适用于所有支持 `AGENTS.md` 或原生规则文件的代理，如 Claude Code、Codex、Antigravity、Cursor、Copilot 等），用于从日常开发对话中安全沉淀编码偏好，同时防止提示词越改越臃肿或产生过拟合副作用。

## 核心特性

- **双模环境适配**：自动检测本机是否使用 [Magpie](https://usemagpie.ai)（通过 `magpie library instructions` 统一同步至所有托管代理）；无 Magpie 时默认只维护当前正在使用的代理自身规则文件（按需通过标记块增量同步）。
- **内置上下文工程护栏**：通过“信号隔离 + 三次法则 + 30 行注意力预算 + 一进一出合并”，防止把一次性指令或偶发报错写成全局铁律。
- **纯异步非阻塞与会话内接力**：默认派发后台子代理（继承主模型保证准确率）静默写入，仅在规则冲突或超预算时唤醒主代理确认；当前会话直接按新偏好生效。
- **零外部脚本依赖**：纯 Markdown 构成，跨 Windows / macOS / Linux 通用。

## 安装方式

### 方式一：通过 `skills` CLI 安装（推荐）
```sh
npx skills add lucosophy/agent-rules-tuner
```

### 方式二：通过 Magpie 安装
在 Magpie 应用的 **Library › Skills** 中添加 GitHub 仓库 `lucosophy/agent-rules-tuner`（或完整仓库 URL），随后即可一键分发至已配置的所有 Agent，并通过 `magpie library skill update` 拉取更新。

### 方式三：手动安装
将本仓库克隆或复制到通用 Skills 目录（如 `~/.agents/skills/agent-rules-tuner`）或各工具对应的 `skills/` 目录即可。

## 遵循的行业标准与参考规范

本 Skill 的结构与规则治理逻辑遵循以下公开标准与最佳实践（维护时请定期对照上游更新）：

1. **Agent Skills 开放标准**：https://agentskills.io/
   - 规范了 `SKILL.md` 元数据格式、`references/` 目录结构及 **Progressive Disclosure（渐进式披露）** 机制。
2. **AGENTS.md 开放标准**：https://agents.md/ （GitHub：[agentsmd/agents.md](https://github.com/agentsmd/agents.md)）
   - 规范了跨工具项目级指令文件的通用约定。
3. **Context Engineering 最佳实践**：Anthropic *Effective Context Engineering for AI Agents*
   - 核心关注 **Attention Budget（注意力预算）**、**Context Rot（上下文腐烂）** 与 **Instruction Dilution（指令稀释）** 的治理方法。

## 目录结构

```text
agent-rules-tuner/
├── SKILL.md                             # 核心工作流与内建护栏（遵循 agentskills.io 规范）
├── README.md                            # 英文说明文档
├── README.zh-CN.md                      # 中文说明文档
├── LICENSE                              # MIT 开源协议
└── references/
    └── scope-and-formatting.md          # 作用域判定、防膨胀准则与检查清单
```

## 维护与定期更新指南

随着编码代理和底层模型能力升级，建议维护人员每隔 1–3 个月按以下清单检查并更新本 Skill：

1. **核对上游格式标准**：检查 [agentskills.io](https://agentskills.io/) 与 [agents.md](https://agents.md/) 是否有新增 frontmatter 字段、目录约定或新主流代理的默认全局规则路径。
2. **更新“内置能力剔除列表”**：新模型原生自带的约束越来越多（例如计划模式只读、验证结果如实汇报等），及时将已无需人工声明的条目加入 `references/scope-and-formatting.md` 的反膨胀剔除清单。
3. **同步分发**：更新本目录后，若使用 Magpie 管理，执行 `magpie library sync` 即可同步至所有已注册代理。
