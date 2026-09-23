# llm-condense

A skill for concise, professional AI responses. Preserves accuracy, natural
grammar, and necessary detail. Inspired by
[Caveman](https://github.com/JuliusBrussee/caveman), without the persona.

## Install in Codex

Copy [SKILL.md](SKILL.md) to `~/.codex/skills/llm-condense/SKILL.md`, or
`$CODEX_HOME/skills/llm-condense/SKILL.md` if configured.

Windows default: `%USERPROFILE%\.codex\skills\llm-condense\SKILL.md`.

## Use

- Enable: "Use llm-condense for this conversation."
- One passage: "Use llm-condense to revise this text: ..."
- Disable: "Turn off llm-condense."

Requested depth and format take precedence over brevity. This skill guides output
style; it does not compress existing context or guarantee token savings.

## License

[AGPL-3.0](LICENSE).
