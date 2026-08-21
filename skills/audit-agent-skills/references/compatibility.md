# Compatibility evidence

The scanner implements a conservative shared contract plus separate Claude and Codex profiles. It is intentionally an auditor, not a standards authority.

## Shared Agent Skills contract

- A skill is a directory containing `SKILL.md`.
- YAML frontmatter requires `name` and `description`.
- `name` must match the parent directory, use lowercase letters, digits, and hyphens, and avoid leading, trailing, or consecutive hyphens.
- The body should stay concise and progressively disclose detail through `scripts/`, `references/`, and `assets/`.
- Relative file references must resolve inside the skill folder.

## Claude profile

- User skills commonly live in `~/.claude/skills`.
- Names are at most 64 characters and must not contain the reserved words `anthropic` or `claude`.
- Descriptions are at most 1024 characters and should say both what the skill does and when it should trigger.
- `allowed-tools` is recognized as model-specific and therefore receives a portability review note.

## Codex profile

- Current standard discovery roots include `~/.agents/skills` and repository `.agents/skills`; some local installations also mirror personal skills under `~/.codex/skills`.
- `agents/openai.yaml` is optional interface and policy metadata, not a shared requirement.
- Product-specific tool names or MCP dependencies can be valid while still reducing portability.

## Static-analysis boundary

The scanner reads bounded text and metadata. It does not execute scripts, import skill code, call network services, evaluate prompts, or prove that an agent will choose or follow a skill correctly. Symlinks are never followed during the bounded walk; escaping links are refused, and links that cannot be resolved are surfaced as safety findings with only a safe operating-system error code such as `ENOENT`. Heuristic findings require human judgment.
