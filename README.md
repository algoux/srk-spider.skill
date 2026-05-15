# srk-spider

`srk-spider` is an Agent Skill for crawling external competitive-programming ranklists and converting them into Standard Ranklist (srk) JSON.

The skill is written for agents that support the `SKILL.md` convention. It guides the agent to prefer official algoUX crawler scripts, write reproducible converters when needed, preserve rich source data, handle media assets, and validate the generated srk output.

## What It Covers

- Explore and reuse [`algoux/rank-spider`](https://github.com/algoux/rank-spider) before writing a custom crawler.
- Prefer `rank_spider/` Python scripts for xcpcio boards.
- Prefer `spidercraft/` Node/JS scripts for DOMjudge, Codeforces Gym, PTA, Nowcoder, Hydro, and similar supported sources.
- Write a custom reproducible crawler/converter when the source is unsupported.
- Follow the srk spec, asset layout, marker conventions, ICPC series defaults, contributor handling, and `remarks` guidance.

## Repository Layout

```text
srk-spider.skill/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── crawler-strategy.md
    ├── icpc-series.md
    └── srk-output-checklist.md
```

`SKILL.md` is the entrypoint. The `references/` files are loaded by the agent only when the matching detail is needed.

## Install

Replace `algoux` with the GitHub owner after this repository is published.

### Codex

Recommended for local/user install:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/algoux/srk-spider.skill ~/.agents/skills/srk-spider
```

Codex also supports installing skills from other repositories through `$skill-installer`; if your Codex version supports GitHub directory URLs, you can ask:

```text
$skill-installer install https://github.com/algoux/srk-spider.skill
```

Restart Codex if the new skill does not appear.

### Claude Code

Personal install:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/algoux/srk-spider.skill ~/.claude/skills/srk-spider
```

Project install:

```bash
mkdir -p .claude/skills
git clone https://github.com/algoux/srk-spider.skill .claude/skills/srk-spider
```

Invoke explicitly with:

```text
/srk-spider
```

or ask for an srk crawling/conversion task and let the agent decide from the skill description.

### GitHub Copilot

For repository-scoped use, place the skill directory under one of GitHub Copilot's supported project skill locations:

```bash
mkdir -p .github/skills
git clone https://github.com/algoux/srk-spider.skill .github/skills/srk-spider
```

For personal use:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/algoux/srk-spider.skill ~/.agents/skills/srk-spider
```

If your GitHub CLI includes `gh skill`, you can preview and install skills from GitHub repositories:

```bash
gh skill preview algoux/srk-spider.skill srk-spider
gh skill install algoux/srk-spider.skill srk-spider
```

If `gh skill` does not detect a root-level single-skill repository, use the manual clone approach above.

### Cursor

Cursor supports Agent Skills and can import skills from GitHub through **Cursor Settings -> Rules -> Add Rule -> Remote Rule (Github)**.

Manual project install:

```bash
mkdir -p .cursor/skills
git clone https://github.com/algoux/srk-spider.skill .cursor/skills/srk-spider
```

For a shared project convention across multiple compatible tools, prefer checking the skill into:

```text
.agents/skills/srk-spider/
```

## Can Users Just Paste the GitHub Link in Chat?

Sometimes, but it is not the best distribution path.

If the agent has web or GitHub access, a user can say:

```text
Use the skill at https://github.com/algoux/srk-spider.skill to convert this contest ranklist to srk.
```

That may work for a one-off task, but it usually will not give automatic discovery, implicit invocation, update tracking, or access to referenced files unless the tool fetches the repository. For reliable use, install or copy the skill directory into the tool's skills directory.

## Validation

For Codex-style validation, run:

```bash
python3 /path/to/quick_validate.py /path/to/srk-spider.skill
```

The skill should validate with `name: srk-spider` and a trigger-focused `description`.

## Security

Review third-party skills before installing them. This skill is instruction-only and does not bundle executable scripts, but it instructs agents to run crawler tooling while solving user tasks. Agents should still ask for credentials, browser session access, or risky commands when needed.

## References

- [srk docs](https://srk.algoux.org/)
- [srk spec](https://github.com/algoux/standard-ranklist/blob/master/specs/README.md)
- [Agent Skills open standard](https://agentskills.io/)
- [OpenAI Codex skills](https://developers.openai.com/codex/skills)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [GitHub Copilot agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Cursor Agent Skills](https://cursor.com/docs/skills)
