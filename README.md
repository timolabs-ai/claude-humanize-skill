# /humanize - Humanize AI-Generated Content

The /humanize skill rewrites AI-generated drafts and writing that still sounds like AI. It reads the draft for meaning, keeps the facts intact, and writes a cleaner version around the actual point. It does not edit sentence by sentence, because that approach often leaves the same AI-style structure in place.

The target voice is plain, specific, complete, restrained, and useful. Meaning, facts, numbers, names, dates, and commitments are preserved. The skill improves structure and wording without adding fake roughness, slang, typos, or forced contractions.

## Quick example

A typical AI-generated paragraph:

> In today's fast-paced digital landscape, robust customer engagement strategies are no longer a luxury — they are mission-critical. By leveraging our cutting-edge platform, organizations can seamlessly unlock transformative growth and empower their teams to deliver best-in-class experiences. The key is not just adopting technology, but reimagining how we connect with customers.

The same paragraph after `/humanize`:

> Customer engagement has become a core operational concern for most B2B organizations. The platform consolidates onboarding, retention workflows, and reporting into a single system. Adoption alone does not produce results. Teams that succeed redesign their customer-contact processes around the tool.

The rewrite says more with less filler.

## Install

Quick install (copy the skill file directly):

```
mkdir -p ~/.claude/skills/humanize
curl -o ~/.claude/skills/humanize/SKILL.md https://raw.githubusercontent.com/timolabs-ai/claude-humanize-skill/main/SKILL.md
```

Developer install (clone and symlink, easier to pull updates):

```
git clone https://github.com/timolabs-ai/claude-humanize-skill.git ~/repos/claude-humanize-skill
ln -s ~/repos/claude-humanize-skill ~/.claude/skills/humanize
```

Restart Claude Code. The skill registers as `humanize`.

## Use

Paste content and invoke:

```
/humanize <paste content here>
```

Trigger phrases also work: "humanize this," "make this sound human," "strip AI writing," "de-AI this."

The skill infers the content type from the structure of your input. Professional is the default tone. To request a different tone, add "Casual" or "Irreverent" to your request.

## How the skill works

The skill makes one rewriting pass with three steps.

### 1. Read the draft for substance

The skill reads the draft for meaning before rewriting. It identifies:

- the main point and the intended reader
- supporting claims
- concrete facts: names, numbers, dates, examples, constraints, commitments, instructions
- claims that seem unsupported or unverifiable
- protected material that must remain exact (code, URLs, quoted speech, named entities, numbers, dates)

The original draft is evidence. It is not the template.

### 2. Rebuild the piece

The skill picks a structure that fits the actual content. It writes new paragraphs around the main point and the supporting facts. Throat-clearing openings, staged setup, repeated setup-explanation-takeaway movement, generic transitions, and takeaway endings are removed.

Markdown structure is preserved only when it helps the reader. Headings, bullets, and paragraph breaks are rebuilt when the original structure is itself part of the AI feel.

### 3. Apply the rule set

After rebuilding, the rules check the output. The rules group into six categories.

**Vocabulary.** Words and phrases that mark text as marketing or consulting prose get cut or replaced. Examples (not exhaustive):

| Banned | Why it gets flagged | Replacement strategy |
|---|---|---|
| robust, seamless, holistic | Vague intensifier with no information | Cut, or replace with a specific quality |
| leverage (verb), unlock, empower | Marketing register | Use the plain verb ("use," "enable") |
| transformative, revolutionary, game-changing | Hype without evidence | Cut, or quantify the change |
| at its core, in today's landscape, mission-critical | Filler framing | Cut |
| delve, tapestry, resonate | Frequent in AI output | Substitute a concrete verb or cut |

Em-dashes above the budget (one per 300 words; one per 200 in Irreverent tone) get replaced. Filler sentence openers ("To keep it direct," "In short," "Put simply") get cut.

**Sentence shape.** Sentence fragments are converted to complete sentences across every tone, including Casual and Irreverent. Contrastive constructions ("not just X, but Y," "X rather than Y") become positive statements. Sentence-opening filler gets stripped. Long comma-joined chains split into separate sentences with periods.

Before:
> To keep it direct, our solution isn't just powerful, it's intuitive.

After:
> The solution handles complex use cases and stays easy to learn.

**Bullet rhythm.** Bullets that share the same grammatical shape get rewritten so the list looks like a set of useful points and reads naturally aloud. Three transitions are banned at the start of bullets: "Furthermore," "Moreover," "Additionally."

**Paragraph rhythm.** Paragraphs that all sit at similar length or repeat the same internal movement get broken up. Polished-but-empty sentences get cut. Every paragraph must do one clear job (claim, evidence, consequence, instruction, context, or genuinely new point) and must include at least one concrete element when the source supports it: a named actor, specific action, hard constraint, worked example, measurable consequence, number, named object, or specific decision.

**Section structure and headings.** Headings describe what the section contains, in plain language. Rhetorical-question, contrastive, colon-subtitle, vague noun-phrase, and metric-as-headline heading styles are converted to descriptive form. Section endings that summarize, moralize, or announce a takeaway are cut.

| Banned heading style | Example | Rewrite to |
|---|---|---|
| Rhetorical question | "Are We Hitting Targets?" | "Performance against targets" |
| Contrastive | "More than a CRM" | "What this CRM does" |
| Marketing colon-subtitle | "Unlocking Growth: A Strategic Approach" | "Strategies for growth" |
| Vague noun-phrase | "Empowering Teams Through Innovation" | "How teams use this platform" |
| Metric-as-headline | "Revenue Down 12% in Q3" | "Q3 revenue performance" |

**Reader language.** Headings, titles, and labels use words the intended reader would say themselves or search for. Internal jargon that is technically accurate but cold gets replaced with familiar reader vocabulary. If the reader would naturally say "AI-generated content" or "AI writing," the skill avoids inventing a colder label like "AI-shaped writing."

After the rules apply, the output runs through a final check for leftover AI-style wording, unclear labels, and structure that still feels staged.

## Configuration

Content type is inferred from structural signals in the input. Tone defaults to Professional. The skill recognizes four content types and three tones.

### Content types

| Type | Inferred from | What changes beyond the universal rules |
|---|---|---|
| Email | Greeting opener ("Hi/Hello/Dear &lt;name&gt;") and a signature | Universal rules apply |
| Presentation / slides | Mostly short bullets, sparse prose, deck-like sections | One idea per bullet, plain verbs replace corporate ones ("Build" for "Foster"), parenthetical asides are dropped |
| Proposal / Business doc | Section headings, formal third-person, business vocabulary | Effusive openers and reader-addressing scaffolding removed; formal third-person voice retained; numbers and named standards preserved exactly |
| Blog / prose (default fallback) | Headings, paragraphs, first or second person, conversational register | Universal rules apply |

### Tones

| Tone | Voice |
|---|---|
| Professional (default) | Plain, specific, useful. The serious-editor voice. |
| Casual | Looser register. Complete sentences enforced. |
| Irreverent | Sharper voice. Fragments still get converted. |

## Known limitations

- The skill rewrites the piece, which can change structure when the original structure is itself part of the AI feel. If you need the exact paragraph order, headings, or sentence count preserved, /humanize is not the right tool.
- Heading rules apply universally. The metric-as-headline, rhetorical-question, contrastive, colon-subtitle, and vague-abstraction bans are absolute with no override. News-style "Revenue Down 12% in Q3" headings always become descriptive form.
- The complete-sentence rule overrides tone settings. Sentence fragments are converted in Casual and Irreverent registers too. There is no "punchy fragment" exception.
- The skill does not mimic any specific author's voice or apply stylistic quirks.

## Why this design

Most attempts to make AI writing sound human add surface roughness: random typos, fake contractions, deliberate grammar errors, and eccentric phrasing meant to read as quirky. This skill rejects that approach.

AI writing reads as AI because of specific structural habits: hedging, rhetorical scaffolding around weak claims, contrastive emphasis that makes filler feel substantive, framing-and-takeaway narration, and adjectives that signal importance without earning it. Surface cleanliness has nothing to do with it.

The skill treats the AI input as evidence of what the writer meant. It rewrites the piece around that substance, in language the intended reader would actually use. The structure follows from the content itself.

## Contributing

Issues and pull requests are welcome. When proposing a new banned word, include three or more examples of that word appearing in generic AI output, drawn from at least two different writing contexts. The skill is conservative about additions, since a banned word also bans its legitimate uses.

## License

Released under the MIT License. See `LICENSE` for the full text. Copyright (c) 2026 TIMO Labs.
