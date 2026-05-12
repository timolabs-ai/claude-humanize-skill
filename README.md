# /humanize — Strip AI Writing Patterns from Content

A Claude Code skill that rewrites AI-generated text to sound human. Invoke it as `/humanize` and it walks the content through a five-layer editorial pass: vocabulary, sentence shape, paragraph rhythm, section structure, and narration patterns.

## What this skill does

`/humanize` cleans the writing tells that mark text as AI-produced. Banned vocabulary (robust, seamless, leverage, unlock, transformative, and so on), em-dashes, filler sentence openers, contrastive constructions (X, not Y), rhetorical questions in headings, metric-as-headline framing, setup-explanation-takeaway narration shapes, motivational closers, and the polished-but-empty Claude-tone that runs through a lot of generated text.

The output target is the voice of a serious editor. Plain, specific, unsentimental, useful. If a sentence sounds impressive but does not add information, the skill cuts it.

## Why this exists (the editorial-substance principle)

Most attempts to "humanize" AI writing add surface roughness. Random typos, fake contractions, intentionally broken grammar, eccentric phrasing meant to read as quirky. This skill rejects that approach.

The position is that AI writing reads as AI not because it is too clean, but because it is structurally performative. It hedges. It builds rhetorical scaffolding around weak claims. It uses contrastive emphasis (not just X, but Y) to make filler feel substantive. It opens with framing sentences and closes with quietly-profound takeaways. It pads with adjectives that signal importance without earning it.

Editorial substance is the cure, not surface roughness. The skill rewrites for clarity, concrete detail, and complete sentences. It does not add contractions, fragments, or deliberate informality.

## Installation

Drop the skill into your Claude Code skills directory:

```
mkdir -p ~/.claude/skills/humanize
curl -o ~/.claude/skills/humanize/SKILL.md https://raw.githubusercontent.com/timolabs-ai/claude-humanize-skill/main/SKILL.md
```

Or clone the repo and symlink:

```
git clone https://github.com/timolabs-ai/claude-humanize-skill.git ~/repos/claude-humanize-skill
ln -s ~/repos/claude-humanize-skill ~/.claude/skills/humanize
```

After installation, restart Claude Code and confirm the skill is registered. It should appear in your available skills list as `humanize`.

## How to invoke

The skill triggers on any of these patterns:

- The slash command `/humanize`
- Phrases such as "humanize this," "make this sound human," "remove AI writing," "de-AI this," "strip AI patterns," "make this less robotic"

You can also reference it directly: "Run this through the humanize skill."

## What the skill checks

The skill applies five editorial layers in sequence.

### Word layer (vocabulary)
Banned words and phrases that mark text as AI-produced. The list covers marketing filler (seamless, robust, leverage, holistic, transformative, revolutionary, cutting-edge, best-in-class, game-changing), consulting tells (orchestrate, mission-critical, at its core), and Claude-specific patterns (delve, tapestry, unlock, empower, resonate). Em-dashes are replaced with commas, colons, or periods. Filler sentence openers are removed.

### Sentence layer (grammar and shape)
Sentence-opening clauses such as "To keep it direct," "In short," and "Put simply" are deleted. Contrastive constructions ("not X but Y") are rewritten as positive statements. Sentence fragments are converted to complete sentences across all modes and tones, with no exceptions for casual or irreverent voice. Comma-joined sentence chains are split into separate sentences.

### Bullet layer (list rhythm)
Bullets that all share the same syntactic shape are varied. Bullet lists that read as parallel slogans are rewritten for varied length and structure. Banned bullet-list transitions: "Furthermore," "Moreover," "Additionally."

### Paragraph layer (rhythm and symmetry)
Anti-symmetry pass: paragraphs of identical length and shape are broken up. Polished-but-empty claims are cut. The setup-explanation-takeaway pattern (a sentence framing what comes next, a sentence explaining it, a sentence stating its significance) is rewritten because it reads as Claude-trained narration.

### Section layer (structure and narration)
Heading rules apply across all content types. Rhetorical-question headings ("Are We Hitting Targets?," "What if X?") become declarative descriptive phrases. Contrastive headings ("X, not Y," "More than X," "Beyond X," "Not just X but Y") become positive descriptions. Colon-subtitle marketing format ("Unlocking X: A Y Approach") is reduced to a single descriptive phrase. Vague noun-phrase abstractions ("Empowering Teams Through Innovation," "The Future of X") become concrete descriptions of what the section actually contains. Metric-as-headline ("Revenue down 12%," "40% productivity gain") is converted to descriptive form ("Q3 revenue performance," "Productivity changes since launch") because numbers belong in the body where they have context.

Performed-insight closer sentences ("This is how X becomes real," "What this all means," "The lesson here is") are cut. Motivational-cadence sentences are cut. Grand framing sentences ("In today's landscape," "At its core") are cut.

## Configuration

The skill operates in two intensity modes and supports four content types and three tone settings.

### Modes
- **Light:** Word and sentence-level fixes only. Preserves document structure. Use when the source content is mostly correct and you want surgical edits.
- **Full:** All five editorial layers run. Headings, paragraphs, narration, and section structure are rewritten. Use when the content needs editorial intervention beyond surface fixes.

The skill defaults to inferring the right mode from content type. You can override by saying "Light mode" or "Full mode" in your invocation.

### Content types
- **Technical documentation:** Code, API references, architecture writing. Preserves technical precision.
- **Op-ed / Long-form essay:** Opinion writing, analytical essays. Rewrites rhetorical-question headings to declarative form.
- **Marketing / Sales copy:** Aggressive removal of marketing filler and effusive openers.
- **Proposal / Business document:** Formal third-person voice. Removes effusive openers ("We are pleased to..."), maintains business register.

### Tones
- **Professional:** Default. The serious editor voice. Plain, specific, useful.
- **Casual:** Looser register. Complete sentences still required (no fragments, no Casual/Irreverent exceptions).
- **Irreverent:** Sharper voice with stronger opinions. Same complete-sentence rule applies.

## Known limitations

- The metric-as-headline ban is universal across all content types and modes. There is no override. News-style "Revenue down 12%" headings are always converted to descriptive form.
- Op-ed content with rhetorical-question headings will be rewritten. If the source genre genuinely requires rhetorical questions in headings (some essay forms do), the skill will still convert them.
- The complete-sentence rule overrides all tone settings. Casual and Irreverent modes still require complete sentences. Fragments are converted regardless of tone.
- The skill optimizes for readability and editorial substance. It does not attempt to mimic any specific author's voice or apply stylistic quirks.

## What the skill does not do

- Add typos, contractions, or surface roughness to make text "feel human"
- Introduce randomness or eccentric phrasing
- Apply intentionally broken grammar
- Mimic the voice of any specific writer

The position is that these tricks produce visibly-edited AI writing, which is worse than unedited AI writing. The skill rewrites for substance, not for the appearance of imperfection.

## Contributing

Issues and pull requests are welcome. Please describe the AI writing pattern you want addressed and provide a before/after example.

When proposing additions to the banned vocabulary list, include three or more independent examples of the word appearing in generic AI output. The skill is deliberately conservative about additions, since banning a word also bans its legitimate uses.

## License

MIT License. See `LICENSE` for the full text.

Copyright (c) 2026 TIMO Labs.
