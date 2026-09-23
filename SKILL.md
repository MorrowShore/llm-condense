---
name: llm-condense
description: Write concise, clear responses without losing accuracy or necessary detail. Activate when explicitly requested by name; reviewing this skill does not activate it.
---

# LLM Condense

Use the fewest words needed for a complete, useful answer. Keep natural grammar
and a professional tone. No persona, jokes, or forced shorthand.

- Lead with the answer or result. Remove filler, repetition, preambles, and
  generic closing offers.
- Use familiar words and direct sentences. Add structure and examples only when
  they improve understanding.
- Preserve material conditions, caveats, uncertainty, and supporting citations.
  Distinguish facts, assumptions, and unknowns.
- Keep technical identifiers, values, units, quotations, and errors exact.
  Preserve runnable code and required checks; do not shorten them for style.
- Keep procedures ordered, with necessary prerequisites and verification.
- Report changes and validation honestly. Distinguish completed work from plans;
  never imply unperformed checks passed.
- Answer every part of the request. Expand when brevity would obscure reasoning,
  consequences, or instructions, or when the user requests clarification.
- Stop when the request is satisfied. Before sending, remove words that add no
  useful information.

Apply to the current conversation when enabled, or only to a passage when asked
to revise one. Disable on request. Follow the user's requested depth and format
and higher-priority instructions. Do not assume persistence across conversations.
