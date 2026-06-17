<div align="center">
  <h1>dev-daily-skills</h1>
  <p>
    <strong>装这一个包，解决日常 80% 的编码烦恼。</strong><br>
    <em>One package, 80% of your daily coding friction — gone.</em>
  </p>
</div>

---

<table align="center">
  <tr>
    <td align="center">
      <details>
        <summary><strong>🇨🇳 中文</strong></summary>

## 为什么选这个？ 

GitHub 上已有 1400+ 个 Skills，但 **70% 不合格**——要么臃肿不堪、要么描述模糊、要么一个 Skill 塞了五件事。真正能留下来日常使用的不到 10 个。

`dev-daily-skills` 只收录**每天都在用**的技能：

- **单职责** — 一个 Skill 只解决一个问题
- **精确触发** — description 写的是用户真正会说的话
- **硬性检查点** — 关键步骤有 `<HARD-GATE>`，AI 无法跳过
- **有范例** — 每个 Skill 自带正确/错误对比

## 安装

```bash
# 克隆到项目级 skills 目录
git clone https://github.com/yinboliu-git/dev-daily-skills.git .claude/skills/dev-daily-skills

# 或者克隆到个人 skills 目录（所有项目可用）
git clone https://github.com/yinboliu-git/dev-daily-skills.git ~/.claude/skills/dev-daily-skills
```

## 包含的 Skills

| Skill | 说这句话触发 | 解决的问题 |
|-------|------------|----------|
| **git-commit** | "commit my changes" / "写个 commit" | 告别想 commit message 的痛苦 |
| **systematic-debug** | "debug this" / "帮我调试" | 四阶段结构化调试：观察→假设→测试→修复 |
| **safe-refactor** | "refactor this" / "重构一下" | 先确认有测试保护，再逐步改动，每步验证 |
| **code-review** | "review my code" / "审查代码" | 按严重级别审查：正确性→安全→性能→可维护性 |
| **doc-generator** | "document this function" / "生成文档" | 聚焦 WHAT/WHY 的文档，不是代码的翻译 |
| **test-first** | "write tests" / "写测试" | 强制 TDD 红灯→绿灯→重构，不许先写实现 |
| **pr-description** | "create a PR" / "生成 PR" | 从分支 diff 自动生成结构化 PR 描述 |

## 使用场景

**场景 1：日常提交**
```
你：commit my changes
Claude：生成 Conventional Commits 格式的 message，询问是否提交
```

**场景 2：调试 Bug**
```
你：debug this — the login page doesn't redirect after submit
Claude：Phase 1 观察 → Phase 2 假设 → Phase 3 测试 → Phase 4 修复
```

**场景 3：重构前先加测试**
```
你：write tests for the payment module, then refactor it
Claude：test-first → safe-refactor，两个 Skill 自动串联
```

**场景 4：提交 PR 前**
```
你：review my changes, then create a PR
Claude：code-review → pr-description，一站式完成
```

## 设计原则

每个 Skill 遵循社区验证的最佳实践：

1. **先输出再澄清** — 不做问答式交互，直接输出并标注假设
2. **HARD-GATE 关键节点** — 该强制的地方绝不含糊（如无测试不重构）
3. **Anti-patterns 对比** — 用 DON'T/DO 表格替代长段描述
4. **Quality Checklist** — 每个 Skill 交付前必须自检
5. **SKILL.md 控制在 200 行以内** — 简洁是刻意设计的

## 文件结构

```
dev-daily-skills/
├── README.md
└── skills/
    ├── git-commit/SKILL.md
    ├── systematic-debug/SKILL.md
    ├── safe-refactor/SKILL.md
    ├── code-review/SKILL.md
    ├── doc-generator/SKILL.md
    ├── test-first/SKILL.md
    └── pr-description/SKILL.md
```

## 互补项目

`dev-daily-skills` 聚焦**日常高频操作**，与以下项目互补：

| 项目 | Star | 定位 | 与本项目的关系 |
|------|------|------|-------------|
| [superpowers](https://github.com/obra/superpowers) | ~204K | 完整软件工程流程 | superpowers 管完整流程，我们管零散日常 |
| [agent-skills](https://github.com/addyosmani/agent-skills) | ~50K | Google 工程师的生产级规范 | 他们是工程规范，我们是日常操作 |
| [karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) | ~133K | LLM 编码教训蒸馏 | 他们是 AI 哲学，我们是具体流程 |

## 贡献

欢迎贡献新 Skill 或改进现有的。提交前请确认：

- [ ] Skill 是单职责的（一个 Skill 只做一件事）
- [ ] description 包含用户真正会说的触发语
- [ ] 有 HARD-GATE 标注不可跳过的步骤
- [ ] 有完整的 Quality Checklist
- [ ] 有至少一个 Example 展示正确用法
- [ ] SKILL.md 不超过 200 行

      </details>
    </td>
    <td align="center">
      <details>
        <summary><strong>🇬🇧 English</strong></summary>

## Why This?

There are 1400+ Skills on GitHub, but **70% are subpar** — bloated, vaguely described, or one Skill doing five things. Fewer than 10 survive daily use.

`dev-daily-skills` only includes skills you'll **actually use every day**:

- **Single-purpose** — One Skill, one problem
- **Precise triggers** — Descriptions use phrases real people say
- **Hard gates** — Critical steps are enforced via `<HARD-GATE>`, not suggested
- **Examples included** — Do-this-not-that examples in every skill

## Installation

```bash
# Clone to project-level skills directory
git clone https://github.com/yinboliu-git/dev-daily-skills.git .claude/skills/dev-daily-skills

# Or clone to personal skills directory (available across all projects)
git clone https://github.com/yinboliu-git/dev-daily-skills.git ~/.claude/skills/dev-daily-skills
```

## What's Inside

| Skill | Trigger | Problem Solved |
|-------|---------|---------------|
| **git-commit** | "commit my changes" | Never stare at a blank commit message again |
| **systematic-debug** | "debug this" | Observe → Hypothesize → Test → Fix. No more guessing. |
| **safe-refactor** | "refactor this" | No tests? No refactor. Change, verify, repeat. |
| **code-review** | "review my code" | Severity-ranked review: correctness first, style last. |
| **doc-generator** | "document this function" | Documents purpose and contract, not syntax. |
| **test-first** | "write tests" | RED → GREEN → REFACTOR. No implementation before a failing test. |
| **pr-description** | "create a PR" | Structured PR description from your branch diff. |

## Usage Examples

**Scenario 1: Daily Commit**
```
You: commit my changes
Claude: Generates Conventional Commits message, asks whether to commit
```

**Scenario 2: Debugging**
```
You: debug this — the login page doesn't redirect after submit
Claude: Observe → Hypothesize → Test → Fix
```

**Scenario 3: Test Before Refactor**
```
You: write tests for the payment module, then refactor it
Claude: test-first → safe-refactor, two skills chained automatically
```

**Scenario 4: Pre-PR Checklist**
```
You: review my changes, then create a PR
Claude: code-review → pr-description, one-stop PR readiness
```

## Design Principles

Every skill follows community-validated best practices:

1. **Generate first, clarify second** — Output immediately, flag assumptions after
2. **Hard gates at critical checkpoints** — Non-negotiable enforcement (e.g., no refactor without tests)
3. **Anti-pattern side-by-side** — DON'T/DO tables, not paragraphs
4. **Quality Checklist** — Self-audit before delivering
5. **Under 200 lines** — Brevity is a feature, not laziness

## File Structure

```
dev-daily-skills/
├── README.md
└── skills/
    ├── git-commit/SKILL.md
    ├── systematic-debug/SKILL.md
    ├── safe-refactor/SKILL.md
    ├── code-review/SKILL.md
    ├── doc-generator/SKILL.md
    ├── test-first/SKILL.md
    └── pr-description/SKILL.md
```

## Complementary Projects

`dev-daily-skills` focuses on **high-frequency daily tasks**. It complements rather than competes with:

| Project | Star | Positioning | Relationship |
|---------|------|------------|-------------|
| [superpowers](https://github.com/obra/superpowers) | ~204K | Full SDLC workflow | They handle epics; we handle micro-tasks |
| [agent-skills](https://github.com/addyosmani/agent-skills) | ~50K | Production-grade practices | They set standards; we boost efficiency |
| [karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) | ~133K | LLM coding wisdom distilled | They teach philosophy; we ship workflows |

## Contributing

PRs welcome. Before submitting:

- [ ] Single-purpose: one Skill, one job
- [ ] Description includes real phrases users actually say
- [ ] Hard gates on unskippable steps
- [ ] Complete quality checklist present
- [ ] At least one example showing correct usage
- [ ] Under 200 lines

      </details>
    </td>
  </tr>
</table>

---

<div align="center">
  <sub>License: MIT</sub>
</div>
