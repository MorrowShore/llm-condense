# llm-condense

An agent skill that removes conversational overhead from LLM responses.

The model still does the work. It simply stops opening with "Sure, I'd be happy to help,"
stops restating your question, stops closing with "let me know if you need anything else,"
and stops volunteering advice you did not ask for. Answers begin with the answer.

## What it changes

| Without llm-condense | With llm-condense |
|---|---|
| "Sure! I'd be happy to help with that. The issue you're experiencing is likely caused by..." | "Bug in auth middleware. Token expiry check uses `<` instead of `<=`." |
| "Great question — there are a few approaches you could consider, each with tradeoffs..." | "Three options. Ranked by effort:" |
| "I'll now check the configuration file to see what's going on." | *(tool call, no narration)* |
| "Let me know if you need anything else!" | *(nothing)* |

## What it does not change

Compression applies to prose only. The skill explicitly preserves:

- Code blocks, configuration, and file paths
- Errors and log lines, quoted exactly
- Version numbers, flags, identifiers, and units
- Technical terms, proper nouns, and standard acronyms
- Negations and quantifiers (`not`, `never`, `only`, `except`, `all`, `any`)

That last item matters. Dropping a single `not` inverts the meaning of a sentence, which costs
far more than the token it saves. Prose gets compressed; meaning does not.

## Levels

| Level | Command | Behavior |
|---|---|---|
| **standard** | `/llm-condense standard` | No filler or hedging. Structured bullets, tables, and blocks. Full grammatical sentences. **Default.** |
| **strict** | `/llm-condense strict` | Fragments permitted. Drops articles, transitions, and redundant clauses. Maximum density at full accuracy. |
| **audit** | `/llm-condense audit` | Strict, plus stated confidence and the exact evidence (file, line, command output) behind each material claim. |
| **off** | `/llm-condense off` | Revert to normal response style. |

Deactivate at any time with "stop llm-condense" or "normal mode". The mode persists for the
whole session until you change it.

## Safety override

Brevity is not worth an ambiguous warning. The skill automatically suspends compression and
switches to complete, connective prose for:

- Security warnings and credential exposure risks
- Confirmations of irreversible actions (`DROP`, `DELETE`, `rm -rf`, force-push, migrations)
- Multi-step procedures where fragment order could be misread
- Any case where the compressed form admits more than one reading

It resumes signal-only output once the risky passage is done.

## Scope

llm-condense governs chat responses only. Written artifacts stay in normal prose, because other
humans read them:

- Source code and comments
- Commit messages, pull requests, and issue text
- Documentation, READMEs, and changelogs
- Memory and configuration files
- Any message addressed to a third party

## Install

Copy the skill directory into your agent's skills folder:

```bash
# Claude Code
mkdir -p ~/.claude/skills/llm-condense
cp SKILL.md ~/.claude/skills/llm-condense/SKILL.md

# Generic agents that read a skills directory
mkdir -p ~/.agents/skills/llm-condense
cp SKILL.md ~/.agents/skills/llm-condense/SKILL.md
```

Or clone the repository directly:

```bash
git clone https://github.com/morrowshore/llm-condense.git
```

To activate on every session, add the skill name to your project instructions file
(`CLAUDE.md`, `AGENTS.md`, or equivalent).

## Why this exists

This skill is a rewrite of the `caveman` skill by
[JuliusBrussee](https://github.com/JuliusBrussee/caveman), which achieves a similar reduction by
having the model talk like a caveman — dropping articles and writing in broken syntax.

That gets the token savings, but the register is a joke, and it is one you have to look at all
day. It also degrades output in ways the original does not fully guard against: broken grammar
is harder to parse under time pressure, and joking register makes the model's confidence harder
to read.

llm-condense keeps the underlying discipline and discards the costume. The rules that actually
produce brevity — no filler, no narration, no restatement, no unsolicited scope — have nothing
to do with speaking like a caveman, so they survive on their own. Standard grammatical English,
objective tone, same reduction.

The full upstream skill is preserved verbatim in
[`reference/caveman-original.md`](llm-condense/reference/caveman-original.md) so the differences
can be diffed.

## Honest expectations

**No benchmark is claimed here.** The upstream project reports token reductions in the 65–75%
range across real sessions, and cites third-party research that brevity constraints improved
accuracy by 26 percentage points on some benchmarks. Neither figure is independently verified
for this skill, and the savings vary substantially with task type and with how verbose the
model's default style already is. Treat "fewer tokens than the default" as the claim, not a
specific number.

**What you should actually expect:** a meaningful reduction in response length for explanatory
and conversational turns, a smaller reduction for code generation (where output is mostly code,
and code is never compressed), and no change at all to reasoning quality. If a task needs
sustained prose — a design explanation, a careful walkthrough — `standard` exists for that.

## Differences from `caveman`

| | `caveman` | `llm-condense` |
|---|---|---|
| Register | Caveman roleplay, broken syntax | Standard grammatical English |
| Levels | lite / full / ultra / wenyan ×3 | standard / strict / audit |
| Classical Chinese modes | Yes | No |
| Grammar degradation | Permitted at `full` and above | Never permitted |
| Evidence citations | No | Yes (`audit`) |
| Guiding standard | ASD-STE100, informally | Plain-language directives |

## Content

```
SKILL.md                        the skill definition
reference/caveman-original.md   upstream caveman skill, verbatim
```
