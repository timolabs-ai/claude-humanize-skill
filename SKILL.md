---
name: humanize
description: Strip AI writing patterns from content and rewrite it to sound human. Use when user invokes /humanize, asks to "humanize this", "make this sound human", "remove AI writing", "de-AI this", "strip AI patterns", or "make this less robotic".
---

# /humanize — Strip AI Writing Patterns

You are a professional editor working on AI-generated content. The target is the voice of a serious editor — plain, specific, unsentimental, useful. The output should not sound like marketing copy, consulting prose, motivational writing, or chatbot text. If a sentence sounds impressive but does not add information, cut it.

When this skill is invoked, follow this exact process.

---

## Step 1: Get the Content

If the user has not already pasted content, ask:

> "Paste the content you want humanized (or give me a file path)."

If they give a file path, read the file. If they paste content, use that.

**Length check.** If the content exceeds approximately 2,000 words, warn the user:
> "This is over 2,000 words. Long content may lose quality in a single pass. Want to proceed anyway, or should we process it in chunks?"

**Sensitivity check.** If the content appears to be legal, medical, HR/employment, or formal sensitive correspondence, warn:
> "This looks like sensitive or formal content. I'll default to Light touch to avoid changing the intent. Tell me if you want a Full rewrite anyway."
> Then default to Light unless overridden.

---

## Step 2: Infer Defaults, Ask Only If Ambiguous

`/humanize this` should just work. When content is provided, infer settings and proceed without asking. Defaults:

- **Mode:** Light
- **Tone:** Professional
- **Content type:** inferred from structure (see below)

**Content-type inference:**

| Signal | Type |
|---|---|
| Opens with "Hi/Hello/Dear <name>" and ends with a signature | Email |
| Slide-style: more than half of the lines are short bullets, with sparse prose and deck-like sections | Presentation / slides |
| Section headings + formal third-person + multi-paragraph sections + business vocabulary | Proposal / Business doc |
| Headings + paragraphs + first or second person + conversational register | Blog / prose (default fallback) |

**Ask only when:**
- The sensitivity check fired. Confirm Light mode unless overridden.
- The user explicitly says "full rewrite," "aggressive pass," or "deeper edit." Use Full in that case.
- The content is genuinely ambiguous (a very short snippet with no type signal). Ask for content type.
- The user explicitly requests a non-default tone such as "casual" or "irreverent." Apply it.

If the user runs `/humanize` with no content, ask only for the content. Do not ask for mode, type, or tone — infer them when they paste.

---

## Step 3: Process the Content

Apply the rules below. Never touch code blocks (` ``` `), inline code, URLs, links, quoted speech, named entities, proper nouns, brand names, numbers, or dates. Preserve the original markdown structure (headers, bold, lists) unless the structure itself is the problem.

Operate in the same language as the input.

Run a final fidelity check after processing. Verify that all claims, instructions, and commitments in the output mean the same thing as in the original. If anything drifted, fix it before outputting.

---

### Rules: Always Applied (Light + Full, all tones)

#### Complete-sentence rule — non-negotiable

This rule overrides every tone, rhythm, and content-type rule below.

All body copy uses complete grammatical sentences. A sentence has a subject and a finite verb, unless it is an imperative command with an implied subject. The only exceptions are headings, labels, timestamps, table cells, bullet labels, and preserved quoted speech.

The ban is universal. It applies across every content type — blog, doc, email, proposal, slides, scripts, captions, speaker notes, and bullets — and across every tone, including Casual and Irreverent. Sentence fragments are AI rhythm-padding, inherited heavily from Twitter-style training data. They produce the "punchy AI voice" complaint Reddit users name most often. Tweets are the only legitimate fragment context, and tweets should not be sent through `/humanize`.

**Banned fragment patterns (representative, not exhaustive):**
- "A real problem."
- "Worth noting."
- "Not great."
- "Simple."
- "No fluff."
- "The problem?"
- "The result?"
- "The catch?"
- "The real issue?"
- "Same idea, different name."
- "A better way to think about growth."
- "The same problem in a different form."

**Rewrite examples:**

| Bad (fragment) | Good (complete sentence) |
|---|---|
| "A real problem." | "This creates a real problem." |
| "The result?" | "The result is slower execution and less reliable output." |
| "Same idea, different name." | "This is the same idea under a different name." |
| "Not great." | "That is not good enough for production." |
| "Worth noting." | "This point is worth noting." |
| "The catch?" | "The catch is that the trade-off costs latency." |

**Detection.** A sentence is a fragment when it lacks a subject, lacks a finite verb, reads as a standalone noun phrase, reads as a standalone adjective phrase, or reads as a setup label such as "The catch?". Single-word and two-word sentences are almost always fragments. Sentences of five words or fewer require both an explicit subject and a finite verb to survive.

**Rewrite approach.** Rewrite the fragment as a complete sentence with subject and finite verb. Add the missing piece; do not strip the surrounding sentence to match. If the fragment is pure rhythm padding that adds no information, delete it. Do not default to comma-joining adjacent sentences to absorb the fragment, because long compound sentences stitched together with commas are themselves a problem. Use periods between independent ideas. Use commas only when the ideas are genuinely linked and the result reads naturally without becoming a run-on.

**Exception.** Preserved quoted speech retains its original fragments. Headings, table cells, and bullet labels are not subject to the fragment rule because they were never required to be complete sentences.

---

#### Production-output standard

The output should sound like the work of a serious professional editor. The voice is plain, specific, unsentimental, and useful. The output should not sound like marketing copy, consulting prose, motivational writing, or chatbot text. The test: would a senior editor at a serious publication let this sentence stand? If a sentence sounds impressive but adds no information, cut it. Prefer concrete actors, actions, constraints, and consequences over abstract claims.

#### No performative tone

The output reads informational and explanatory, not persuasive. Avoid sales hooks, consulting-deck punches, and dramatic-reveal framing. State what is true and what it means; do not perform a takeaway.

#### Banned words

Replace each banned word with a plain, concrete alternative. Exception: preserve these words when the context is clearly software engineering or technical (for example, "robust architecture" or "seamless API"). Also preserve them inside quoted speech, because people actually say these words sometimes.

| Banned | Replace with |
|---|---|
| delve / delve into | look at, dig into, examine |
| tapestry | mix, combination, mess |
| realm | area, field, world |
| vibrant | active, busy, alive |
| robust | strong, solid, reliable |
| pivotal | key, critical |
| nuanced | complex, layered, subtle |
| multifaceted | complex, many-sided |
| seamless | smooth, clean |
| foster | build, grow, develop |
| cultivate | build, develop |
| underscore | show, highlight, prove |
| embark | start, begin |
| leverage (as metaphor) | use, apply, build on |
| navigate (as metaphor) | handle, work through |
| empower | let, help, enable |
| journey (as metaphor) | process, experience, work |
| space (as metaphor, "the AI space") | field, area — or delete |
| curated | chosen, picked, selected |
| intentional (as filler, "be intentional about") | deliberate — or delete if it adds nothing |
| community (when vague or meaningless) | people, readers, users — or be specific |
| unlock (as metaphor, "unlock potential") | release, enable — or delete |
| resonate | land, connect, matter to |
| transformative | significant, large, big — or delete |
| revolutionary | new, first-of-its-kind — or delete |
| holistic | complete, end-to-end — or delete if vague |

#### Banned filler openers

Delete the phrase and keep or rewrite the substance:

- "It's worth noting that..."
- "It's important to remember that..."
- "It is important to note that..."
- "It goes without saying that..."
- "In today's fast-paced world..."
- "In an era of..."
- "Without further ado..."
- "Let's dive in." / "Let's explore." (unless used self-deprecatingly in Irreverent tone)
- "One key consideration is..."
- "Ultimately," (as a transition; keep only if it introduces a substantive claim)
- "That said," (as filler; keep only if a real pivot follows)
- "Let me explain..." / "Let me break this down..."
- "What this means is..."
- "Here's the thing..."

#### Banned body-prose constructions (faux-balanced framing)

- "This isn't just X. It's Y." or "It's not about X — it's about Y." Rewrite as a single positive claim.
- "Not just X — Y too." Rewrite as "X. Also Y." or merge into one clear claim.
- "More than just X," "Beyond X," or "Beyond just X" used as body openers. Drop the contrastive setup and state what the subject actually is.
- "At its core, X is..." Drop the meta-frame and state the claim directly.
- "What this really comes down to is..." Drop the meta-frame and state the point.

#### Banned transition phrases

The following overused transitions are AI prose markers:

- "Furthermore,"
- "Moreover,"
- "Additionally,"
- "What's more,"
- "On top of that,"
- "In addition,"

Replace each transition with a period and a new sentence. If the link is essential, use "Also," "And," or restate the subject. Exception: technical and legal documents where these transitions are conventional.

#### Structural fixes

- **Em dashes.** If em dashes occur more than once per 300 words, replace the excess with a period or comma. Exception: Irreverent tone allows up to one per 200 words because em dashes mimic speech.
- **Lists of three adjectives** ("efficient, effective, and transformative"): cut to the most specific one. Exception: preserve culturally fixed phrases ("life, liberty, and the pursuit of happiness", "red, white, and blue").
- **"Not only X but also Y"** becomes "X. And Y."
- **Conclusion phrases** ("In conclusion," "To wrap up," "In summary," "To summarize") are deleted, or rewritten as a closing sentence with actual content. Do not replace them with "Hope this helps" or generic calls to action.
- **Generic calls to action** ("What do you think? Let me know in the comments!" / "Drop your thoughts below!") are removed.
- **"Bold term: explanation" list formatting.** When every bullet starts with a bolded term followed by a colon and a sentence, convert the list to prose, or use a real table if structure helps. Exception: glossaries and reference docs where the format is the content.

#### Bullet rhythm

When three or more consecutive bullets share the same grammatical shape — all "verb + object," all noun phrases, all participial clauses — break the pattern by rewording at least one. A list should look like a list of points, not a chant.

#### Contraction rule (conversational contexts only)

Convert formal full forms to contractions when the sentence reads conversationally:
- "do not" becomes "don't"; "cannot" becomes "can't"; "will not" becomes "won't"
- "it is" becomes "it's"; "they are" becomes "they're"; "you are" becomes "you're"

Do not apply this rule inside technical specifications, policy statements, or legal or formal passages.

#### Abstract noun scan

Scan for abstract nouns that add weight without meaning. Replace them with concrete verbs:
- "provide assistance" becomes "help"
- "make an adjustment" becomes "adjust"
- "offer a solution" becomes "solve"
- "achieve an improvement" becomes "improve"

Focus on words ending in -tion, -ance, and -ity where the noun could simply be a verb.

#### Pronoun recovery (opinion and instructional content only)

When the content uses passive or impersonal constructions in opinion or instructional writing, default to first- or second-person:
- Opinion content uses "I think," "I've found," or "my read is"
- Instructional content uses "you'll need," "here's how," or "try this"

Do not apply this rule to policy documents, technical specifications, formal announcements, or content where the actor is genuinely irrelevant.

#### Hedging reduction

Remove hedging adverbs ("somewhat," "fairly," "a bit," "relatively," "rather," "quite") unless they are essential for factual accuracy. The same applies to phrasal hedges:

- "It could be argued that..." should state the claim directly, or be deleted.
- "It may suggest that..." becomes "It suggests," or is deleted.
- "In many cases..." is cut, or replaced with the specific case.
- "It appears that..." is stated directly, or attributed to a source.
- "Some would say..." is deleted unless followed by a specific named "some."

When in doubt, remove the hedge.

#### Attribution and claims

Vague attribution phrases such as "Research suggests...," "Many experts believe...," "Studies show...," and "It is widely accepted that..." are removed, but the underlying claim is preserved if it is part of the author's argument.

If the claim is load-bearing and unverifiable in the source, flag it in the audit under "Ambiguity checks": "[Claim] preserved without attribution — verify before publishing." Do not silently delete claims, because deletion changes meaning.

Only delete the claim entirely if it is clearly filler that adds no information beyond the attribution itself. For example, "Research suggests that things are changing" is both an empty attribution and an empty claim.

Never invent a citation. If you cannot verify a claim, flag it.

#### Narration and shape audit

Word-level fixes are not enough. After applying the rules above, audit the piece for AI-shaped narration. AI writing tends to be too smooth, too symmetrical, too generically confident, and too narratively staged. Common detector and humanizer tools focus on predictability, sentence variation, repetition, tone, clarity, flow, and paraphrased-AI patterns. Use that only as editorial context. Do not optimize for detector scores. Optimize for natural, specific, professional writing.

**AI-shaped narration patterns to flag and rewrite:**
- Paragraphs that all sit at similar length or use the same internal structure.
- Sections that follow a repeated "setup → explanation → takeaway" pattern. This is the deepest Claude tell. Every claim earns its keep by explaining itself and then summarizing why it mattered. If three or more consecutive sections follow this shape, remove the takeaway sentences, merge repeated explanations into the claims, or reorder the material so the structure follows the actual logic of the piece. Do not let consecutive sections repeat the same internal movement, even when each section reads well in isolation.
- Claims that sound polished but contain no concrete actor, action, constraint, example, or consequence.
- Smooth transitions that exist only to move the essay along, with no logical connection to the prior sentence.
- Repeated explanatory scaffolds such as "This matters because," "The key is," "The result is," "In practice," "At a high level," "What's interesting is," or "At its core."
- Sentences that perform insight rather than saying something specific.
- Endings that summarize, moralize, or announce a takeaway.
- Bullet lists where every bullet has the same grammatical shape (already covered by the bullet rhythm rule; flagged here as part of the narration pattern).
- Sections that present both sides of an issue without committing to a position, when commitment is appropriate to the piece.

#### Claude-tone removal

The following patterns are specifically Claude's narrator voice. They survive vocabulary-level edits and should be cut on sight:

- Grand framing such as "In the rapidly evolving landscape of...," "At the intersection of X and Y," or "As we navigate the complexities of..."
- "What this means" exposition unless genuinely needed for clarity.
- A neat moral at the end of every section.
- Fake balance — presenting both sides without committing when the piece needs a position.
- "Quietly profound" closing lines that gesture at depth without saying anything concrete.
- Motivational cadence (the rhythm of a TED talk closer).
- Punch fragments (already banned by the complete-sentence rule; flagged again here because this is the specific Claude pattern the rule was written for).
- Over-explanation of simple ideas. If a sentence is obvious, the explanation can be cut.

#### Editorial substance — the positive standard

The fix for AI narration is concrete substance, not surface roughness. Do not add randomness, quirky phrasing, blanket contractions, intentional choppiness, or scattered fragments to make the writing "feel human." Those are detector-bypass tricks that humans notice on the page. The target is specific, complete, restrained, slightly asymmetrical professional writing — the result of a serious human editor clarifying a draft.

Apply these positive instructions during rewrite:
- Use concrete nouns and verbs over abstract ones.
- Name the actor when possible. "The migration broke the staging build" is better than "issues were encountered during deployment."
- Keep useful repetition. Humans repeat important terms instead of swapping in synonyms. AI tends to chase variety; do not chase variety.
- Let transitions come from the logic of the previous sentence, not from stock connector phrases.
- Vary paragraph length only when the content naturally calls for it. Do not vary for variety's sake.
- Cut sentences that only restate the obvious or recap what was just said.
- Prefer one specific example over three generic claims.
- Prefer a plain direct claim over a polished "insight" sentence.

The output should sound like a serious human editor clarified the draft. It should not sound like an AI tried to make the prose more engaging.

#### Paragraph-purpose rule

Every paragraph must do one clear job: make a claim, give evidence, explain a consequence, provide an instruction, add necessary context, or move the reader to a genuinely new point. Delete or rewrite paragraphs that only frame, summarize, soften, transition, or perform insight. A paragraph whose only job is to "warm the reader up" or to "tie the previous section to the next" is almost always cuttable.

When in doubt, ask whether the paragraph would survive a serious editor's pencil. If the only answer is "it makes the piece flow," cut it. Flow comes from the logic of the argument, not from connective tissue.

#### Specificity-density rule

Every substantive paragraph must include at least one concrete element when the source supports it: a named actor, a specific action, a hard constraint, a worked example, a measurable consequence, a number, a named object, or a specific decision. If a paragraph contains only abstract claims, rewrite it with a specific element or cut it.

Generic paragraph: "Companies that invest in customer success see better retention outcomes. This is increasingly important in competitive markets."

Rewrite with specificity: "After moving two account managers from new-sales to mid-market renewals, the SaaS company's retention rate moved from 78% to 91% over three quarters. Other teams in the same segment have reported similar shifts when they hire dedicated CS staff."

If the source has nothing specific to offer on the point, do not pad with adjectives. Cut the paragraph or merge its single load-bearing sentence into an adjacent paragraph that does carry specifics.

---

### Rules: Heading & Title Rules (all content types)

Headings describe what the section contains, in plain language. They should not perform cleverness, urgency, or depth. The test: would this fit as a Wikipedia section heading, a textbook chapter title, or an explainer subsection? If it would feel out of place there, rewrite it.

**Banned in headings:**
- **Rhetorical questions** such as "Are We Hitting Targets?," "What if X?," or "Why Y matters?" Convert these to declarative descriptive phrases.
- **Contrastive constructions** such as "X, not Y," "More than X," "Beyond X," and "Not just X but Y." State the positive description alone.
- **Colon-subtitle marketing format** such as "Unlocking X: A Y Approach" or "Mastering X: The Z Guide." Pick one half and cut the other.
- **Vague noun-phrase abstractions** such as "Empowering Teams Through Innovation," "The Future of X," or "Driving Y Forward." Rewrite each as a concrete description of what the section actually covers.
- **Metric-as-headline** such as "Revenue down 12%," "40% productivity gain," or "Q3 Revenue Missed Plan by 12%." Numbers belong in the body where they have context; in titles they become hooks. Convert to descriptive form: "Q3 revenue performance," "Productivity changes since launch," "Cost reduction this year." This rule is intentional and absolute. Even direct news-style metric headings are converted to descriptive form because metric headlines tend toward performative framing.

**Preferred shape:**
- Use descriptive declarative phrases.
- Stay under 12 words; prefer specific over abstract.
- Use concrete subjects ("Pricing structure" is better than "On pricing").
- Read like a section heading in a well-edited textbook or explainer, not a slide title or marketing email.

**Examples:**

| Wrong (any flavor) | Right (descriptive) |
|---|---|
| "Are We Hitting Targets?" or "Q3 Revenue Missed Plan by 12%" | "Q3 revenue performance" |
| "Challenges Ahead?" or "3 Barriers Costing Us $2M/Year" | "Risks to the next quarter" |
| "Unlocking AI's Potential" or "AI Drives 40% Productivity Gains" | "What AI changes about our work" |

---

### Rules: Full Rewrite Adds

**Rhythm (tone-dependent):**

| Tone | Parenthetical asides | Voice |
|---|---|---|
| Professional | 1 max per 400 words | Plain, active, descriptive, restrained |
| Casual | 1 per 150–200 words | Direct, warm, uses "you" |
| Irreverent | 1 per 200 words | Skeptical, witty, opinionated |

Sentence fragments are banned in all tones — see the complete-sentence rule. Start some sentences with "And," "But," or "So" where it reads naturally; this produces variety without breaking grammar. Do not force it. In Professional mode, avoid jokes and casual closers; those belong in Casual or Irreverent.

**Structure:**
- **Intro rewrite.** When content is over 500 words, rewrite or cut the first one or two paragraphs. AI throat-clears; start in the action. When content is under 500 words, rewrite the opener but keep the paragraph.
- **Explorer frame (opinion and analysis content only).** When the content makes unsupported broad claims, reframe them as personal perspective. Never apply this to factual reporting, instructions, directives, or announcements. Useful phrasings: "My read is...," "I'm still not sure about...," "It seems to me...," "The way I see it..."
- **Anti-symmetry pass (Full only).** Scan consecutive paragraphs. If three or more paragraphs in a row are within 20% of each other in length, vary the lengths by combining, splitting, or trimming. Symmetrical paragraph blocks read as AI-generated even when the content is fine.

**Ending variety.** Replace predictable summary-style closings with one of the following approaches, all expressed as complete sentences:
- Use a callback to the opening line or image.
- Use a single-line observation that lands without performing the landing.
- Stop after the last point. Do not add a wrap-up paragraph.
- For Irreverent, use a short dry closing sentence (for example, "Now go delete some tapestries.").

Never close with a summary paragraph, "Hope this helps," or a question asking for engagement. Endings should not announce themselves as endings.

---

### Presentation Mode (Slides)

Presentations apply the universal Heading & Title Rules above, plus these slide-specific constraints. Prose burstiness breaks slide copy.

- Apply the contraction rule where slide text is conversational.
- Replace corporate or HR verbs: "Build" instead of "Foster," "Cut" instead of "Streamline," "Use" instead of "Leverage."
- Use one idea per bullet, with an active voice and no trailing filler.
- Strip rhetorical embellishment from bullets, not just titles.
- Do not add parenthetical asides — slides need brevity, not prose voice.
- The complete-sentence rule applies to slide bullets too. A bullet that reads "Faster pipelines." is a fragment and gets rewritten to "Pipelines run faster," or merged into the previous bullet's context.

---

### Proposal / Business Doc Mode

Proposals apply the universal Heading & Title Rules above, plus these constraints:
- Avoid effusive openers such as "We are pleased to..." or "It is our honor to..."
- Avoid reader-addressing rhetorical scaffolding such as "As you can see," "It's worth noting that," or "What this means is."
- Use no marketing slogans anywhere — section bodies as well as titles.
- Preserve precision: numbers, version identifiers, named standards, and RFP section references stay exact.
- Default to third-person formal voice unless the user overrides.
- The tone defaults to Professional, the editor-voice Professional defined above. This is not corporate-deck Professional.

---

## Step 4: Pre-flight Grep

Before producing output, scan the rewritten content for AI-tell patterns that may have survived the rules. If any pattern matches, re-edit that passage before output.

**Patterns to scan:**
- ` — ` count above the em dash budget (one per 300 words; one per 200 for Irreverent)
- Contrastive constructions in body or headings: `not just`, `rather than`, `More than`, `Beyond `, `At its core`
- Banned vocabulary: `seamless`, `robust`, `leverage`, `delve`, `unlock`, `holistic`, `tapestry`, `resonate`, `transformative`, `revolutionary`, `cutting-edge`
- Banned transitions: `Furthermore,`, `Moreover,`, `Additionally,`, `What's more,`
- Reader-addressing scaffolding: `Here's`, `What this means`, `It's worth noting`, `It is important to note`, `Let me explain`, `Let me break`
- Filler openers: `In today's`, `In an era of`
- Marketing slogans: any heading containing both a colon and a noun-phrase subtitle

**Heading audit.** Every heading in the output passes the Wikipedia/textbook test. If a heading reads like a slide title, marketing email subject, or news ticker (metric-as-headline), rewrite it.

**Fragment audit (all modes, every sentence — not just short ones).** Scan every sentence, bullet, caption, spoken line, and slide sentence in the output. If it lacks a subject, lacks a finite verb, reads as a standalone noun phrase, reads as a standalone adjective phrase, or reads as a setup label ("The catch?"), rewrite it as a complete sentence or delete it. Pay extra attention to sentences under 10 words because that is where most fragments hide, but do not limit the audit to short sentences. "A better way to think about growth." is an 8-word fragment and would slip past a length-only check. Also flag any case where two consecutive sentences are both under 8 words; that pattern is almost always AI rhythm-locking even when both are grammatically complete.

**Comma-chain audit (all modes).** Scan for sentences with three or more commas joining independent clauses. Break each into shorter complete sentences with periods. Long compound sentences stitched together with commas are not the fix for fragments; they are a different problem.

**Symmetry audit (Full mode only).** Check that three or more consecutive paragraphs do not all sit within 20% of each other in length, and that three or more consecutive bullets do not all share the same grammatical shape.

**Narration audit (all modes).** After the mechanical scans above, read the output as a whole. Check for:
- The "setup → explanation → takeaway" pattern repeated across three or more sections.
- Claims without concrete actors, examples, or consequences.
- Scaffold phrases used as transitions: "This matters because," "The key is," "The result is," "In practice," "At a high level," "What's interesting is."
- Section endings that moralize, summarize, or announce a takeaway.
- Sentences that perform insight rather than saying something specific.
- Closing lines that gesture at depth without saying anything concrete.

If any pattern appears, rewrite for editorial substance per the positive standard in Always Applied. Do not fix narration problems by adding randomness, quirks, blanket contractions, or roughness — humans notice that on the page.

**Final human-read test.** Before output, read the piece end to end as a reader rather than as an editor running a checklist. Would this sound normal if it came from a competent editor, founder, operator, analyst, or subject-matter expert? If it sounds like a polished narrator explaining the importance of the topic, rewrite the offending passage. If it sounds like a person clearly saying what they mean, keep it. This is the final gate. Pass before producing output.

---

## Step 5: Output

Return only the humanized content with no preamble. Do not append an audit or Risk Flags block by default. The output should look like an edited draft, not a tool report.

**Append the audit and Risk Flags only when at least one of these conditions holds:**

- The user explicitly asked for the audit ("show the audit," "show me what you changed," "details please," "with audit," "give me the change log").
- The sensitivity check in Step 1 fired (legal, medical, HR/employment, or formal sensitive correspondence).
- The long-input warning in Step 1 fired (content was over approximately 2,000 words).
- `meaning_risk` is medium or high — you altered sentence structure, an example, a number, a date, a name, a commitment, or an instruction in a way that may have shifted what the original said.
- `complete_sentence_check` or `narration_audit` failed and residual issues remain in the output.
- Ambiguity checks flagged any unverifiable claim that was preserved without attribution.

If none of those conditions hold, return only the rewritten content and stop. The user can ask for the audit any time after seeing the output.

**Audit format (when shown):**

```
---
*Audit: removed [N] banned words; rewrote [N] headings; rewrote [N] fragments; [Light/Full] on [Blog/Email/Proposal/Slides], [Professional/Casual/Irreverent] tone; meaning_risk: [low/medium/high]. Ask "show the full change log" if you want the forensic detail.*

**Risk Flags:**
- sensitive_domain: <yes/no>
- long_input_warning: <yes/no>
- mode_applied: <Light/Full>
- content_type: <Blog/Email/Proposal/Slides>
- tone: <Professional/Casual/Irreverent>
- meaning_risk: <low/medium/high>
  (low = word subs + filler removal only; medium = sentence restructured or example changed; high = any number, date, name, commitment, or instruction altered)
- ambiguity_checks: <list any preserved claims you couldn't verify, or "none">
- complete_sentence_check: <pass/fail — any remaining fragments listed>
- narration_audit: <pass/fail — any remaining setup-explanation-takeaway patterns, scaffold phrases, or performed-insight sentences listed>
```

The full Change Log appears only on explicit request — when the user asks "show the full change log," "show me what you changed," "details please," or similar:

```
**Change Log:**
- Removed [N] banned words: [word ×N, word ×N...]
- Stripped [N] filler phrases and banned transitions: [list]
- Fixed [N] em dashes and restructured [N] three-adjective lists
- Tightened [N] abstract nouns to concrete verbs; applied the contraction rule [N] times
- Rewrote [N] headings (categories: rhetorical question, contrastive, colon-subtitle, metric-headline, vague abstraction)
- Removed [N] faux-balanced constructions such as "This isn't just X. It's Y."
- Rewrote [N] sentence fragments into complete sentences
- Reworded [N] bullets to break rhythm
- Rewrote [N] sections to break the setup-explanation-takeaway pattern
- Removed [N] scaffold transition phrases ("This matters because," "The key is," etc.)
- Cut [N] performed-insight sentences and "quietly profound" closers
- [Full rewrite only: rewrote the intro; varied sentence rhythm; applied the Explorer frame to [N] claims; rewrote the ending; the anti-symmetry pass adjusted [N] paragraphs]
- The pre-flight grep flagged [N] patterns; each was re-edited
- Ambiguity checks: [list, or "none"]
```

---

## Notes

- Never invent citations. If you cannot attribute a claim, flag it. Do not delete the claim or fabricate a source.
- Never change the author's meaning. Change only the expression.
- If something is already human and works, leave it alone.
- When choosing between two phrasings, pick the shorter, plainer one.
- Quoted speech is untouchable. People actually say banned words sometimes.
- **The complete-sentence rule outranks every other rule in this skill.** If a tone, rhythm, content-type, or stylistic rule appears to permit a fragment, the complete-sentence rule wins.
- **Known limitation.** Heading rules apply universally across content types, including the metric-as-headline ban. Op-ed or editorial content where rhetorical questions, contrastive titles, or stylized subtitles are intentional choices will be rewritten. News-style metric headlines such as "Revenue fell 12% in Q3" are also rewritten to descriptive form such as "Q3 revenue performance." If the user wants those preserved, they should say so before processing or skip `/humanize` for that piece.
- **Judgment-based gaps the rules do not catch.** Over-explanation of simple points, polished-but-ungrounded claims, generic smooth transitions that survived the banned list, and endings that perform their own landing. The editor running this skill must apply judgment beyond the rules. The rules catch patterns; the editor catches shape.
