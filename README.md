# /humanize — Strip AI Writing Patterns from Content

The /humanize skill rewrites AI-generated text into prose that reads like a human edited it. The target is the structural patterns that mark text as AI-produced: hedging, rhetorical scaffolding, contrastive emphasis, polished filler, and performative narration. Surface features like contractions and typos stay untouched.

## Quick example

A typical AI-generated paragraph:

> In today's fast-paced digital landscape, robust customer engagement strategies are no longer a luxury — they are mission-critical. By leveraging our cutting-edge platform, organizations can seamlessly unlock transformative growth and empower their teams to deliver best-in-class experiences. The key is not just adopting technology, but reimagining how we connect with customers.

The same paragraph after `/humanize`:

> Customer engagement has become a core operational concern for most B2B organizations. The platform consolidates onboarding, retention workflows, and reporting into a single system. Adoption alone does not produce results. Teams that succeed redesign their customer-contact processes around the tool.

The lengths are comparable. The information density is much higher.

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

The skill defaults to Light mode. To run a deeper rewrite, add "Full mode," "aggressive pass," or "deeper edit" to your request. Content type (Email, Slides, Proposal, or Blog/prose) is inferred from the structure.

## What the skill changes

The editorial rules group into five categories: vocabulary, sentence shape, bullet rhythm, paragraph rhythm, and section structure. After the rules apply, the output runs through a pre-flight grep, a narration audit, and a final human-read test before delivery.

### 1. Vocabulary

The vocabulary pass removes words and phrases that mark text as marketing or consulting prose. Examples:

| Banned | Why it gets flagged | Replacement strategy |
|---|---|---|
| robust, seamless, holistic | Vague intensifier with no information | Cut, or replace with a specific quality ("reliable under load," "low-friction handoff") |
| leverage (verb), unlock, empower | Marketing register | Use the plain verb ("use," "enable") |
| transformative, revolutionary, game-changing | Hype without evidence | Cut, or quantify the change |
| at its core, in today's landscape, mission-critical | Filler framing | Cut |
| delve, tapestry, resonate | Frequent in AI output | Substitute a concrete verb or cut |

Em-dashes above the budget (one per 300 words; one per 200 words in Irreverent tone) get replaced with commas, colons, or periods. Filler sentence openers ("To keep it direct," "In short," "Put simply") get cut.

### 2. Sentence shape

The sentence pass handles grammatical shape, fragments, and contrastive constructions.

Before:
> To keep it direct, our solution isn't just powerful, it's intuitive.

After:
> The solution handles complex use cases and stays easy to learn.

Specific rules apply to four sentence patterns:

- Sentence-opening filler ("To keep it direct," "In short," "Put simply") gets cut.
- Contrastive constructions ("not just X, but Y," "X rather than Y") become positive statements.
- Every sentence fragment converts to a complete sentence. The rule applies in casual and irreverent registers too.
- Long comma-joined chains split into separate sentences with periods.

### 3. Bullet rhythm

The bullet pass varies grammatical shape across list items. Bullet lists that read as parallel slogans get rewritten for varied length and structure. Three transitions are banned at the start of bullets: "Furthermore," "Moreover," "Additionally."

Before:
- Drives revenue
- Empowers teams
- Unlocks growth

After:
- Sales pipeline coverage doubled in the first two quarters at the design partners.
- Operations teams now run the system without engineering support.
- Three customer cohorts saw cycle-time reductions of 15-22%.

### 4. Paragraph rhythm

The paragraph pass breaks up symmetric structures and cuts polished-but-empty sentences.

Before:
> Let's talk about onboarding. Onboarding is the process by which new users learn to use your product. Done well, it sets the tone for the entire customer relationship.

After:
> Onboarding sets the tone for the customer relationship. Most B2B products lose 20-40% of signups in the first session, usually because the first-screen experience is unclear about what to do next.

The framing-then-explanation-then-takeaway shape gets rewritten because it reads as AI-trained narration. Paragraphs of identical length and sentence-shape are broken up.

### 5. Section structure and headings

Headings should describe what the section contains. Rules apply across all content types:

| Banned heading style | Example | Rewrite to |
|---|---|---|
| Rhetorical question | "Are We Hitting Targets?" | "Performance against targets" |
| Contrastive | "More than a CRM" | "What this CRM does" |
| Marketing colon-subtitle | "Unlocking Growth: A Strategic Approach" | "Strategies for growth" |
| Vague noun-phrase | "Empowering Teams Through Innovation" | "How teams use this platform" |
| Metric-as-headline | "Revenue Down 12% in Q3" | "Q3 revenue performance" |

Performative closer sentences also get cut at this stage: "This is how it all comes together," "What this all means is...," "The lesson here is..." Grand framing and motivational cadences ("In today's landscape," "At the heart of...") are removed.

## Configuration

The skill exposes two intensity modes, four content types, and three tone settings.

### Modes

**Light** (default) applies every editorial rule in the skill: vocabulary, sentence shape, bullet rhythm, paragraph-purpose, specificity-density, narration audit, and heading rules. The document's opening, ending, paragraph-length symmetry, and overall structure stay close to the original.

**Full** adds structural rewrites on top of Light. The opening paragraphs of long documents get reworked because AI tends to throat-clear, and the ending gets rewritten when it summarizes. The anti-symmetry pass varies paragraph lengths so the structure looks less uniform. Tone-dependent rhythm rules also apply on top of the universal ones.

The skill stays in Light by default. To switch to Full, add "Full mode," "aggressive pass," or "deeper edit" to your request.

### Content types

Content type is inferred from the structure of the input. The skill recognizes four types:

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

- The complete-sentence rule overrides tone settings. Fragments are converted in casual and irreverent registers too.
- Heading rewrites apply in both Light and Full modes. The metric-as-headline, rhetorical-question, contrastive, colon-subtitle, and vague-abstraction bans are universal with no override. News-style "Revenue Down 12% in Q3" headings always become descriptive form. If you have a piece that intentionally uses one of those heading styles, consider whether /humanize is the right tool for it.
- The skill does not mimic any specific author's voice or apply stylistic quirks.

## Why this design

Most attempts to make AI writing sound human add surface roughness: random typos, fake contractions, deliberate grammar errors, and eccentric phrasing meant to read as quirky. This skill rejects that approach.

AI writing reads as AI because of specific structural habits: hedging, rhetorical scaffolding around weak claims, contrastive emphasis that makes filler feel substantive, framing-and-takeaway narration, and adjectives that signal importance without earning it. Surface cleanliness has nothing to do with it.

The fix is editorial substance: cutting sentences that add no information, replacing generic words with specific ones, and varying rhythm. The skill rewrites for clarity and information density. It does not add randomness or roughness to fake authenticity.

## Contributing

Issues and pull requests are welcome. When proposing a new banned word, include three or more examples of that word appearing in generic AI output, drawn from at least two different writing contexts. The skill is conservative about additions, since a banned word also bans its legitimate uses.

## License

Released under the MIT License. See `LICENSE` for the full text. Copyright (c) 2026 TIMO Labs.
