# PenSkills

**AI agent 技能安全扫描器。** 在安装 Claude Code、Cursor 及其他 agent 的 skill 之前，检查漏洞、恶意模式、提示注入与供应链风险。

**Security scanner for AI agent skills.** Check a skill for vulnerabilities, malicious patterns, prompt injection, and supply-chain risk before you install it.

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)
[![Upstream](https://img.shields.io/badge/upstream-NVIDIA%2FSkillSpector-76b900)](https://github.com/NVIDIA/SkillSpector)

PenSkills 基于 [NVIDIA SkillSpector](https://github.com/NVIDIA/SkillSpector)，用来回答一个问题：**这个 skill 能不能装？**

PenSkills is based on [NVIDIA SkillSpector](https://github.com/NVIDIA/SkillSpector). It answers one question: **is this skill safe to install?**

## 项目目的 / Purpose

Agent skill（`SKILL.md` 及随附脚本）往往在很高的信任级别下执行，安装前却很少被审查。PenSkills 对 skill 目录、单个 `SKILL.md`、Git 仓库、URL 或 zip 做静态检查，并可选地做 LLM 语义分析，输出终端、JSON、Markdown 或 SARIF 报告和风险分。

检测覆盖包括：提示注入、数据外传、权限提升、供应链、过度授权、系统提示泄漏、记忆投毒、工具误用、恶意代码模式、YARA 签名，以及 MCP 最小权限与工具投毒等。完整模式表见上游说明。

Agent skills (`SKILL.md` plus bundled scripts) often run with broad trust and little review. PenSkills statically scans a skill directory, a single `SKILL.md`, a Git repository, a URL, or a zip. Optional LLM semantic analysis can follow. Reports are terminal, JSON, Markdown, or SARIF, with a risk score.

Coverage includes prompt injection, data exfiltration, privilege escalation, supply chain, excessive agency, system-prompt leakage, memory poisoning, tool misuse, dangerous-code patterns, YARA signatures, and MCP least-privilege and tool-poisoning checks. The full pattern catalog is in the upstream manual.

## 与上游的差异 / How PenSkills differs

| | PenSkills（本仓库） | NVIDIA SkillSpector（上游） |
| --- | --- | --- |
| 角色 | 面向 skill 安装前审查的发行版 | 上游扫描引擎与研究实现 |
| 代码 | 导入上游 `main` @ `d501c0d`，保留许可证、版权头与第三方声明 | 原始项目 |
| 包名 / 命令 | 仍为 `skillspector`（避免破坏导入、测试与集成） | `skillspector` |
| 项目名 | PenSkills | SkillSpector |
| 安装地址 | `https://github.com/Aevon-sec/PenSkills` | `https://github.com/NVIDIA/SkillSpector` |

本仓库相对上游的改动限于发行说明：NOTICE、双语 README、安装地址同时给出本 fork 与上游，以及少量用户可见的帮助文字。扫描引擎、分析器和测试夹具保持上游结构。报告正文里仍会出现 “SkillSpector”，因为那是引擎的输出。

Changes in this repository are distribution-level: NOTICE, this bilingual README, install instructions that name both this fork and upstream, and a small amount of user-visible help text. The scanner engine, analyzers, and test fixtures keep the upstream structure. Report bodies still say “SkillSpector”, because that string comes from the engine.

## 归因与许可证 / Attribution and license

- **许可证 / License:** [Apache License 2.0](LICENSE)。请同时阅读 [NOTICE](NOTICE) 与 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
- **上游 / Upstream:** NVIDIA SkillSpector — https://github.com/NVIDIA/SkillSpector
- **导入提交 / Imported commit:** `d501c0d5035c2ed6385740a384d942bb05651fda`
- **版权 / Copyright:** 导入的源码保留 `Copyright (c) 2026 NVIDIA CORPORATION & AFFILIATES` 与 SPDX 头。PenSkills 是衍生作品，不是 NVIDIA 官方产品，也未获 NVIDIA 背书。
- **命令名 / Command name:** 控制台命令和 Python 包名保持 `skillspector`。`--version` 打印 `PenSkills (skillspector) v<version>`。

Imported source keeps its NVIDIA copyright and SPDX headers. PenSkills is a derivative work. It is not an official NVIDIA product and is not endorsed by NVIDIA.

## 安装 / Install

需要 Python **3.12 至 3.15**（上游约束为 `>=3.12,<3.16`）。下面的 `make` 目标假定虚拟环境已激活；有 [uv](https://docs.astral.sh/uv/) 时 Makefile 用 uv，否则用 pip。

Python **3.12 through 3.15** is required (upstream constraint: `>=3.12,<3.16`). `make` targets expect an active virtual environment. The Makefile uses uv when it is installed, and pip otherwise.

第三方依赖会在安装时下载。使用前请查看它们的许可证（见 `THIRD_PARTY_NOTICES.md`）。

Installation downloads third-party open source. Review those licenses before use (`THIRD_PARTY_NOTICES.md`).

**用 uv 安装命令（推荐） / uv tool install:**

```bash
# 本仓库 PenSkills（安装后的命令仍是 skillspector）
uv tool install git+https://github.com/Aevon-sec/PenSkills.git

# 上游 NVIDIA SkillSpector
uv tool install git+https://github.com/NVIDIA/SkillSpector.git

# 以后升级已安装的命令：
uv tool update skillspector
```

需要 MCP 服务时带上 extra：

```bash
uv tool install 'skillspector[mcp] @ git+https://github.com/Aevon-sec/PenSkills.git'
# 上游：uv tool install 'skillspector[mcp] @ git+https://github.com/NVIDIA/SkillSpector.git'
```

**从源码安装 / From source:**

```bash
git clone https://github.com/Aevon-sec/PenSkills.git
cd PenSkills

python3 -m venv .venv && source .venv/bin/activate
# 或者：uv venv .venv && source .venv/bin/activate

make install       # 使用
make install-dev   # 开发与测试
```

上游源码同样可用：`git clone https://github.com/NVIDIA/SkillSpector.git`。

**Docker（本仓库源码构建） / Docker:**

```bash
make docker-build
# 或：docker build -t skillspector .
docker run --rm -v "$PWD:/scan" skillspector scan ./my-skill/ --no-llm
```

镜像入口点仍是 `skillspector`，与上游 Dockerfile 一致。

## 快速开始 / Quick start

```bash
# 扫描本地 skill 目录（仅静态分析，不调用 LLM）
skillspector scan ./my-skill/ --no-llm

# 只扫描一个 SKILL.md
skillspector scan ./SKILL.md --no-llm

# Git 仓库、zip
skillspector scan https://github.com/user/my-skill --no-llm
skillspector scan ./my-skill.zip --no-llm

# JSON / Markdown / SARIF
skillspector scan ./my-skill/ --no-llm --format json --output report.json
skillspector scan ./my-skill/ --no-llm --format markdown --output report.md
skillspector scan ./my-skill/ --no-llm --format sarif --output report.sarif

# 版本（包名仍是 skillspector）
skillspector --version
```

递归扫描某个目录下每个含 `SKILL.md` 的直接子目录：

```bash
skillspector scan ./my-skills/ --recursive --max-skills 100 --no-llm --format json
```

默认会尝试 LLM 语义分析。没有配置模型时，请加上 `--no-llm`，只跑静态检查。LLM 的提供商、密钥和模型名与上游相同，环境变量仍以 `SKILLSPECTOR_` 开头（例如 `SKILLSPECTOR_PROVIDER`、`SKILLSPECTOR_MODEL`）。细节见 [docs/UPSTREAM_README.md](docs/UPSTREAM_README.md) 与 [docs/INFERENCE_USAGE.md](docs/INFERENCE_USAGE.md)。

The default path may call an LLM. Pass `--no-llm` for static analysis only. Provider settings are unchanged from upstream and still use `SKILLSPECTOR_*` variables.

Python API 同样保持上游导入路径：

```python
from skillspector import graph

result = graph.invoke({
    "input_path": "/path/to/skill",
    "output_format": "json",
    "use_llm": False,
})
print(result["risk_score"], result["risk_severity"], result["risk_recommendation"])
```

## 文档 / Documentation

- [docs/UPSTREAM_README.md](docs/UPSTREAM_README.md) — 上游 README 快照（模式列表、配置、示例输出、MCP）。安装命令处同时写了本 fork 与上游。
- [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md) — 架构与如何扩展分析器。
- [docs/SUPPRESSION.md](docs/SUPPRESSION.md) — 基线与误报抑制。
- [docs/PI_EXTENSION.md](docs/PI_EXTENSION.md)、[docs/OPENCODE_EXTENSION.md](docs/OPENCODE_EXTENSION.md) — 在 agent 会话里调用同一个 CLI。
- [NOTICE](NOTICE)、[LICENSE](LICENSE)、[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) — 归因与许可证。
- 上游托管文档：<https://docs.nvidia.com/skills/scanning-agent-skills>

## 安全与贡献 / Security and contributing

- 扫描器引擎中的漏洞请按 [SECURITY.md](SECURITY.md) 处理：引擎问题同时遵循上游 NVIDIA PSIRT 流程；本 fork 特有的打包或文档问题请私下联系本仓库维护者，不要把漏洞细节发到公开 issue。
- 贡献流程、DCO 签署要求见 [CONTRIBUTING.md](CONTRIBUTING.md)。提交到本仓库的补丁如果也适用于上游，建议同时给 [NVIDIA/SkillSpector](https://github.com/NVIDIA/SkillSpector) 开 PR。
- 行为准则与开发命令（`make test`、`make lint`）仍以上游仓库中的说明为准。

Report engine vulnerabilities using [SECURITY.md](SECURITY.md), including NVIDIA PSIRT for the upstream scanner. Fork-specific packaging or documentation issues should go privately to this repository's maintainers. Patches that belong in the engine should also be offered upstream.
