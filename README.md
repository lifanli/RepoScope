# RepoScope

RepoScope is a Hermes/OpenClaw skill that turns a GitHub or open-source repository into a local, source-backed HTML study site.

It is designed for agents that need to learn a codebase by reading official documentation, source files, and tests, then produce a multi-page study module that can be opened directly in a browser.

## What It Does

- Confirms the target repository identity before analysis.
- Reads official docs, repo docs, source files, and tests.
- Builds a subsystem-oriented source map.
- Produces a local multi-page HTML study site.
- Embeds code explanations inside the relevant topic pages.
- Adds sibling-page navigation across every study page.
- Preserves `file://` compatibility for direct local opening.
- Includes a source/reference page so technical claims remain traceable.

## When To Use It

Use this skill when a user gives an agent a GitHub repository, documentation link, or open-source project and asks for:

- a study website,
- a local HTML learning package,
- a source-backed technical walkthrough,
- a reusable codebase study module,
- or a deeper explanation than a README-level summary.

## Output Shape

The default output is a foldered study module:

```text
TARGET_DIR/
├── index.html
├── manifest.json
└── <study-slug>/
    ├── index.html
    ├── overview.html
    ├── architecture.html
    ├── <topic>.html
    ├── optimization-summary.html
    ├── sources.html
    └── assets/
        ├── style.css
        ├── app.js
        └── manifest.json
```

If the target folder already has a multi-study launcher, RepoScope extends that structure instead of replacing it.

## Quality Bar

RepoScope is intentionally stricter than a generic summarization prompt. A good output should include:

- project identity confirmation,
- subsystem-level structure,
- source-backed technical claims,
- relevant code snippets near their explanations,
- engineering trade-offs and failure modes,
- explicit sources,
- working local navigation,
- and direct-open browser compatibility.

Tests are especially important when analyzing optimization, memory limits, persistence, fallback behavior, replay/cursor behavior, migrations, or correctness boundaries.

## Installation

Copy this repository into your agent's skill directory, or copy `SKILL.md` and `references/checklist.md` into an existing skill bundle.

Example:

```bash
skills/
└── RepoScope/
    ├── SKILL.md
    ├── README.md
    └── references/
        └── checklist.md
```

Then restart or reload the agent runtime so the skill can be discovered.

## Usage

Ask the agent to use RepoScope on a repository:

```text
Use RepoScope to analyze https://github.com/owner/project and create a local HTML study site.
```

Or describe the desired output naturally:

```text
Read this GitHub repo and generate a detailed offline HTML learning site with architecture, subsystem pages, embedded code explanations, and sources.
```

## Verification Checklist

Before finishing a generated study site, the agent should verify:

- the repository identity is correct,
- docs, source files, and tests were used where relevant,
- each substantive page is detailed enough to study from,
- code explanations are embedded in the matching topic pages,
- every page links to sibling pages,
- the launcher works when present,
- direct `file://` opening does not depend on blocked local JSON fetches,
- and the browser console is clean.

---

# RepoScope 中文说明

RepoScope 是一个给 Hermes/OpenClaw 这类 agent 使用的 skill。它的目标是：让 agent 自己读取一个 GitHub 或开源代码仓库，然后生成一个本地可打开、带源码依据的多页面 HTML 学习站点。

它不是简单总结 README，而是要求 agent 读取官方文档、仓库文档、源码和测试，把当前代码仓库整理成适合长期学习和复习的本地网页模块。

## 它能做什么

- 先确认目标仓库身份，避免分析错项目。
- 优先读取官方文档、仓库文档、源码和测试。
- 按子系统梳理项目结构。
- 输出本地多页面 HTML 学习站点。
- 在对应主题页面里嵌入代码解释，而不是把代码统一堆到一个页面。
- 每个页面都有同级页面导航。
- 支持直接双击 `index.html` 通过 `file://` 打开。
- 生成 sources/reference 页面，让技术结论可以追溯。

## 什么时候使用

当用户给 agent 一个 GitHub 仓库、文档链接或开源项目，并希望得到下面这些产物时，可以使用 RepoScope：

- 本地 HTML 学习网站；
- 源码级技术导读；
- 可复用的代码仓库学习模块；
- 比 README 摘要更深入的架构和子系统分析；
- 带代码片段、设计取舍、限制和失败模式的学习资料。

## 默认产物结构

默认会生成类似下面的目录：

```text
TARGET_DIR/
├── index.html
├── manifest.json
└── <study-slug>/
    ├── index.html
    ├── overview.html
    ├── architecture.html
    ├── <topic>.html
    ├── optimization-summary.html
    ├── sources.html
    └── assets/
        ├── style.css
        ├── app.js
        └── manifest.json
```

如果目标目录已经是一个多 study 的学习工作区，RepoScope 会扩展原有 launcher/manifest 结构，而不是覆盖它。

## 质量要求

RepoScope 对输出质量有明确要求。一个合格的学习站点应该包含：

- 项目身份确认；
- 按子系统组织的页面结构；
- 有源码、文档或测试支撑的技术结论；
- 放在对应主题附近的代码片段和解释；
- 工程取舍、边界、失败模式和可复用经验；
- 明确的 sources 页面；
- 每页可切换到其他页面的导航；
- 可直接本地打开的 `file://` 兼容性。

如果用户关注优化、内存、持久化、预算、边界、fallback、replay、迁移或正确性，agent 必须读取相关测试，因为测试经常暴露真实边界和维护者意图。

## 安装方式

把这个仓库复制到 agent 的 skill 目录中，或者把 `SKILL.md` 和 `references/checklist.md` 放进现有 skill bundle。

示例：

```bash
skills/
└── RepoScope/
    ├── SKILL.md
    ├── README.md
    └── references/
        └── checklist.md
```

然后重启或刷新 agent runtime，让它重新发现 skill。

## 使用方式

可以直接要求 agent 使用 RepoScope：

```text
Use RepoScope to analyze https://github.com/owner/project and create a local HTML study site.
```

也可以用自然语言描述目标：

```text
读取这个 GitHub 仓库，生成一个离线可打开的 HTML 学习站点，包含架构、子系统页面、源码解释和 sources。
```

## 交付前检查

生成学习站点后，agent 应该确认：

- 仓库身份正确；
- 相关文档、源码和测试已经被读取；
- 主要页面足够深入，能用来学习，而不是浅层摘要；
- 代码解释位于对应主题页面；
- 每个页面都能跳转到其他同级页面；
- 如果有 launcher，它可以正常工作；
- 直接通过 `file://` 打开不会因为本地 JSON fetch 被浏览器拦截而失败；
- 浏览器控制台没有明显错误。
