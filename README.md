# PenSkills

**AI agent 技能安全扫描器。** 由 [Aevon-sec/PenSkills](https://github.com/Aevon-sec/PenSkills) 发布。在安装 Claude Code、Cursor 及其他 agent 的 skill 之前，检查漏洞、恶意模式、提示注入与供应链风险。

**Security scanner for AI agent skills.** Published at [Aevon-sec/PenSkills](https://github.com/Aevon-sec/PenSkills). Check a skill for vulnerabilities, malicious patterns, prompt injection, and supply-chain risk before you install it.

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

PenSkills 用来回答一个问题：**这个 skill 能不能装？**

PenSkills answers one question: **is this skill safe to install?**

## 做什么 / What it does

Agent skill（`SKILL.md` 及随附脚本）往往在很高的信任级别下执行。PenSkills 对 skill 目录、单个 `SKILL.md`、Git 仓库、URL 或 zip 做静态检查，并可选地做 LLM 语义分析，输出终端、JSON、Markdown 或 SARIF 报告和风险分。

检查范围包括：提示注入、数据外传、权限提升、供应链、过度授权、系统提示泄漏、记忆投毒、工具误用、恶意代码模式、YARA 签名，以及 MCP 最小权限与工具投毒。模式说明见 [docs/MANUAL.md](docs/MANUAL.md)。

Agent skills (`SKILL.md` plus bundled scripts) often run with broad trust. PenSkills statically scans a skill directory, a single `SKILL.md`, a Git repository, a URL, or a zip. Optional LLM semantic analysis can follow. Reports are terminal, JSON, Markdown, or SARIF, with a risk score.

Coverage includes prompt injection, data exfiltration, privilege escalation, supply chain, excessive agency, system-prompt leakage, memory poisoning, tool misuse, dangerous-code patterns, YARA signatures, and MCP least-privilege and tool-poisoning checks. See [docs/MANUAL.md](docs/MANUAL.md).

## 安装 / Install

从本仓库安装。需要 Python **3.12 至 3.15**。`make` 目标假定虚拟环境已激活；有 [uv](https://docs.astral.sh/uv/) 时用 uv，否则用 pip。

Install from this repository. Python **3.12 through 3.15** is required. `make` targets expect an active virtual environment and use uv when it is installed, otherwise pip.

安装时会下载第三方依赖。使用前请查看 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

**uv：**

```bash
uv tool install git+https://github.com/Aevon-sec/PenSkills.git
# 需要 MCP 时：
uv tool install 'skillspector[mcp] @ git+https://github.com/Aevon-sec/PenSkills.git'
# 升级：
uv tool update skillspector
```

**源码：**

```bash
git clone https://github.com/Aevon-sec/PenSkills.git
cd PenSkills

python3 -m venv .venv && source .venv/bin/activate
make install       # 使用
make install-dev   # 开发与测试
```

**Docker：**

```bash
make docker-build
docker run --rm -v "$PWD:/scan" skillspector scan ./my-skill/ --no-llm
```

装好之后有两个命令，做的是同一件事：`penskills` 和 `skillspector`。Python 包名是 `skillspector`。

After installation, `penskills` and `skillspector` run the same scanner. The Python package name is `skillspector`.

## 快速开始 / Quick start

```bash
penskills scan ./my-skill/ --no-llm
penskills scan ./SKILL.md --no-llm
penskills scan https://github.com/user/my-skill --no-llm
penskills scan ./my-skill.zip --no-llm

penskills scan ./my-skill/ --no-llm --format json --output report.json
penskills scan ./my-skill/ --no-llm --format markdown --output report.md
penskills scan ./my-skill/ --no-llm --format sarif --output report.sarif

penskills scan ./my-skills/ --recursive --max-skills 100 --no-llm --format json
penskills --version
```

没有配置模型时加上 `--no-llm`，只做静态检查。模型相关环境变量以 `SKILLSPECTOR_` 开头，例如 `SKILLSPECTOR_PROVIDER` 和 `SKILLSPECTOR_MODEL`。说明见 [docs/MANUAL.md](docs/MANUAL.md) 和 [docs/INFERENCE_USAGE.md](docs/INFERENCE_USAGE.md)。

Pass `--no-llm` for static analysis only. Model settings use `SKILLSPECTOR_*` variables.

```python
from skillspector import graph

result = graph.invoke({
    "input_path": "/path/to/skill",
    "output_format": "json",
    "use_llm": False,
})
print(result["risk_score"], result["risk_severity"], result["risk_recommendation"])
```

## 许可证与包含的软件 / License and included software

- 许可证是 [Apache License 2.0](LICENSE)。
- 请阅读 [NOTICE](NOTICE) 和 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
- 扫描器包含 [NVIDIA SkillSpector](https://github.com/NVIDIA/SkillSpector)（修订 `d501c0d5035c2ed6385740a384d942bb05651fda`）。源文件里的 NVIDIA 版权声明和 SPDX 头予以保留。
- PenSkills 不是 NVIDIA 官方产品，也未获 NVIDIA 背书。

The license is Apache-2.0. The scanner includes NVIDIA SkillSpector at revision `d501c0d5035c2ed6385740a384d942bb05651fda`. NVIDIA copyright and SPDX headers in the source stay in place. PenSkills is not an NVIDIA product and is not endorsed by NVIDIA.

## 文档 / Documentation

- [docs/MANUAL.md](docs/MANUAL.md) — 扫描器使用说明（模式、配置、示例、MCP）
- [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md) — 架构与如何扩展分析器
- [docs/SUPPRESSION.md](docs/SUPPRESSION.md) — 基线与误报抑制
- [docs/PI_EXTENSION.md](docs/PI_EXTENSION.md)、[docs/OPENCODE_EXTENSION.md](docs/OPENCODE_EXTENSION.md) — 在 agent 会话里调用扫描
- [CONTRIBUTING.md](CONTRIBUTING.md)、[SECURITY.md](SECURITY.md) — 贡献与安全报告

## 安全与贡献 / Security and contributing

漏洞请按 [SECURITY.md](SECURITY.md) 私下报告给本仓库维护者，不要把细节发到公开 issue。贡献见 [CONTRIBUTING.md](CONTRIBUTING.md)。开发命令是 `make test` 和 `make lint`。

Report vulnerabilities privately using [SECURITY.md](SECURITY.md). Contribution steps are in [CONTRIBUTING.md](CONTRIBUTING.md).
