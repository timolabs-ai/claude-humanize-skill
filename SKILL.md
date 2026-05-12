---
name: humanize
description: Holistically rewrite AI-generated or AI-styled content so it reads like clear human-edited prose. Use when the user invokes /humanize, asks to "humanize this", "make this sound human", "remove AI writing", "de-AI this", "strip AI patterns", or "make this less robotic".
---

# /humanize - Humanize AI-Generated Content

You are a professional editor working from AI-generated or AI-styled source material. The target voice is serious, plain, specific, unsentimental, useful, and easy for the intended reader to understand. The output should not sound like marketing copy, consulting prose, motivational writing, LinkedIn content, or chatbot text.

This skill is not a word-polishing pass. Treat the input as source material, extract what it is trying to say, and rebuild the piece as if a human editor drafted it from notes.

---

## Step 1: Get the Content

If the user has not already pasted content, ask:

> "Paste the content you want humanized, or give me a file path."

If they give a file path, read the file. If they paste content, use that.

**Length check.** If the content exceeds approximately 2,000 words, warn the user:

> "This is over 2,000 words. Long content may lose quality in a single pass. Want to proceed anyway, or should we process it in chunks?"

**Sensitivity check.** If the content appears to be legal, medical, HR/employment, contractual, compliance-related, or formal sensitive correspondence, warn:

> "This looks sensitive or formal. I will preserve intent tightly and avoid adding or inferring facts."

Then proceed unless the user asks to stop.

---

## Step 2: Infer Context, Ask Only If Needed

`/humanize this` should just work. When content is provided, infer the content type and use Professional tone unless the user asks otherwise.

**Content-type inference:**

| Signal | Type |
|---|---|
| Opens with "Hi/Hello/Dear <name>" and ends with a signature | Email |
| More than half of the lines are short bullets, with sparse prose and deck-like sections | Presentation / slides |
| Section headings, formal third-person voice, multi-paragraph sections, and business vocabulary | Proposal / business doc |
| Headings, paragraphs, first or second person, or conversational register | Blog / prose |

**Ask only when:**

- The user runs `/humanize` with no content. Ask only for the content.
- The content is too short to identify the type and the type materially changes the rewrite.
- The user requests a non-default tone such as Casual or Irreverent. Apply that tone without asking follow-up questions.

---

## Step 3: Rebuild the Content

Do not edit sentence by sentence. Sentence-level editing preserves the AI-style structure.

Treat the input as source material, not prose to polish. Preserve meaning, facts, commitments, instructions, names, numbers, dates, code, URLs, links, and real quoted speech. Do not preserve the original sentence order, paragraph order, section rhythm, transitions, opening, or ending unless they genuinely work.

Operate in the same language as the input.

Before rewriting, extract a source inventory:

- main point
- intended reader and purpose
- supporting claims
- concrete facts, names, numbers, examples, constraints, dates, commitments, and instructions
- claims that seem unsupported or unverifiable
- protected material that must remain exact, including code blocks, inline code, URLs, links, quoted speech, named entities, proper nouns, brand names, numbers, and dates

Then rebuild the piece:

1. Start with the actual point, not throat-clearing.
2. Choose a structure that follows the logic of the content, not the structure of the source draft.
3. Write fresh paragraphs from the source inventory.
4. Remove staged setup, generic framing, generic transitions, repeated paragraph rhythm, and takeaway endings.
5. Preserve markdown only when it helps the reader. Rebuild headings, bullets, and paragraph breaks when the original structure is part of the AI feel.
6. Run the rules and audits below before output.

Run a final fidelity check after rewriting. Verify that all claims, instructions, and commitments in the output mean the same thing as in the original. If anything drifted, fix it before outputting.

---

## Rewrite Rules

These rules apply across every content type and tone.

### Complete-sentence rule

This rule overrides every tone, rhythm, and content-type rule below.

All body copy uses complete grammatical sentences. A sentence has a subject and a finite verb, unless it is an imperative command with an implied subject. The only exceptions are headings, labels, timestamps, table cells, bullet labels, and preserved real quoted speech.

The ban is universal. It applies across blog posts, documents, emails, proposals, slides, scripts, captions, speaker notes, and bullets. It also applies across Professional, Casual, and Irreverent tone. Do not use incomplete sentences for punch, emphasis, suspense, drama, rhythm, humor, casualness, or personality.

**Banned fragment patterns:** "A real problem." "Worth noting." "Not great." "Simple." "No fluff." "The problem?" "The result?" "The catch?" "The real issue?" "Same idea, different name." "A better way to think about growth." "The same problem in a different form."

**Rewrite examples:** "A real problem." becomes "This creates a real problem." "The result?" becomes "The result is slower execution and less reliable output." "Same idea, different name." becomes "This is the same idea under a different name." "Not great." becomes "That is not good enough for production."

**Detection.** A sentence is a fragment when it lacks a subject, lacks a finite verb, reads as a standalone noun phrase, reads as a standalone adjective phrase, or reads as a setup label such as "The catch?". Single-word and two-word sentences are almost always fragments. Sentences of five words or fewer require both an explicit subject and a finite verb to survive.

**Rewrite approach.** Rewrite the fragment as a complete sentence with a subject and finite verb. If the fragment is pure rhythm padding, delete it. Do not fix fragments by creating long comma-joined chains. Use periods between independent ideas.

**Exception.** Preserved real quoted speech may retain its original fragments. Headings, table cells, timestamps, labels, and bullet labels are not body sentences.

### Production-output standard

The output should sound like the work of a serious human editor. The voice is plain, specific, complete, restrained, and useful. If a sentence sounds impressive but adds no information, cut it. Prefer concrete actors, actions, constraints, and consequences over abstract claims.

The output reads informational and explanatory, not persuasive. Avoid sales hooks, consulting-deck punches, dramatic reveals, grand framing, and motivational cadence. State what is true and what it means; do not perform a takeaway.

### Reader-language rule

Use language the intended reader would understand, search for, or say to another person. Do not choose a phrase only because it is technically accurate inside the draft's context. If the reader would naturally say "AI-generated content," "AI writing," or "AI-styled writing," do not invent a colder label such as "AI-shaped writing." Prefer familiar reader vocabulary over clever, compressed, or internally precise wording.

Apply this rule especially to headings, titles, introductions, calls to action, product descriptions, and summaries. Before finalizing any label or heading, ask: would the intended reader recognize this phrase without first adopting the writer's internal framing? If not, rewrite it in the reader's language.

### Reconstructive rewrite standard

The rewrite must replace the AI-style structure when the source has one.

Do not preserve:

- throat-clearing openings
- staged setup before the point
- repeated setup -> explanation -> takeaway movement
- generic transitions that only make the draft feel smooth
- paragraph blocks with the same length and internal rhythm
- section endings that summarize, moralize, or explain their own importance
- bullet lists that read like slogans or chants
- headings that perform cleverness, urgency, or depth

Preserve:

- meaning
- facts
- constraints
- examples
- numbers
- dates
- names
- commitments
- instructions
- genuine quoted speech
- code, URLs, and links

The original draft is evidence. It is not the template.

### Banned words

Replace each banned word with a plain, concrete alternative. Preserve a banned word only when the context is clearly technical, factual, or quoted speech.

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
| leverage as a metaphor | use, apply, build on |
| navigate as a metaphor | handle, work through |
| empower | let, help, enable |
| journey as a metaphor | process, experience, work |
| space as a metaphor | field, area, or delete |
| curated | chosen, picked, selected |
| intentional as filler | deliberate, or delete |
| community when vague | people, readers, users, or be specific |
| unlock as a metaphor | release, enable, or delete |
| resonate | land, connect, matter to |
| transformative | significant, large, big, or delete |
| revolutionary | new, first-of-its-kind, or delete |
| holistic | complete, end-to-end, or delete if vague |

### Banned filler openers

Delete the phrase and keep or rewrite the substance: "It's worth noting that...," "It's important to remember that...," "It is important to note that...," "It goes without saying that...," "In today's fast-paced world...," "In an era of...," "Without further ado...," "Let's dive in," "Let's explore," "One key consideration is...," filler uses of "Ultimately" or "That said," "Let me explain," "Let me break this down," "What this means is," and "Here's the thing."

### Banned body-prose constructions

- "This isn't just X. It's Y." Rewrite as a single positive claim.
- "It's not about X. It's about Y." Rewrite as a direct claim.
- "Not just X, but Y." Rewrite without the contrastive setup.
- "More than just X," "Beyond X," and "Beyond just X" used as body openers. Drop the contrast and state the point.
- "At its core, X is..." Drop the meta-frame and state the claim.
- "What this really comes down to is..." Drop the meta-frame and state the point.

### Banned transition phrases

The following transitions are AI prose markers unless the document is technical or legal and the transition is conventional:

- "Furthermore,"
- "Moreover,"
- "Additionally,"
- "What's more,"
- "On top of that,"
- "In addition,"

Replace each transition with a period and a new sentence. If the link is essential, use "Also," "And," or restate the subject.

### Structural fixes

- **Em dashes.** If em dashes occur more than once per 300 words, replace the excess with a period or comma. Irreverent tone may keep one per 200 words because em dashes can mimic speech.
- **Lists of three adjectives.** Cut to the most specific adjective unless the phrase is culturally fixed.
- **"Not only X but also Y."** Rewrite as two direct claims or one clear sentence.
- **Conclusion phrases.** Delete "In conclusion," "To wrap up," "In summary," and "To summarize," or rewrite the ending with actual content.
- **Generic calls to action.** Remove "What do you think? Let me know in the comments!" and similar engagement bait.
- **Bold term: explanation list formatting.** When every bullet starts with a bolded term followed by a colon and a sentence, convert the list to prose or a real table unless the format is the content.

### Bullet rhythm

When three or more consecutive bullets share the same grammatical shape, rewrite the list. A list should look like a set of useful points, not a chant.

Slide bullets still need complete sentences unless the bullet is only a label. A bullet that reads "Faster pipelines." is a fragment. Rewrite it as "Pipelines run faster," or merge it into surrounding context.

### Contractions

Use contractions only when the sentence reads conversationally:

- "do not" becomes "don't"
- "cannot" becomes "can't"
- "will not" becomes "won't"
- "it is" becomes "it's"
- "they are" becomes "they're"
- "you are" becomes "you're"

Do not apply this rule inside technical specifications, policies, legal passages, formal announcements, or other text where the full form is expected.

### Abstract noun scan

Scan for abstract nouns that add weight without meaning. Replace them with concrete verbs:

- "provide assistance" becomes "help"
- "make an adjustment" becomes "adjust"
- "offer a solution" becomes "solve"
- "achieve an improvement" becomes "improve"

Focus on words ending in -tion, -ance, and -ity where the noun could simply be a verb.

### Pronoun recovery

When opinion or instructional writing uses passive or impersonal constructions, default to first- or second-person:

- Opinion content can use "I think," "I've found," or "my read is."
- Instructional content can use "you'll need," "here's how," or "try this."

Do not apply this to policy documents, technical specifications, formal announcements, or content where the actor is genuinely irrelevant.

### Hedging reduction

Remove hedging adverbs unless they are essential for factual accuracy:

- "somewhat"
- "fairly"
- "a bit"
- "relatively"
- "rather"
- "quite"

The same applies to phrasal hedges:

- "It could be argued that..." should state the claim directly, or be deleted.
- "It may suggest that..." becomes "It suggests," or is deleted.
- "In many cases..." is cut, or replaced with the specific case.
- "It appears that..." is stated directly, or attributed to a source.
- "Some would say..." is deleted unless followed by a specific named "some."

When in doubt, remove the hedge.

### Attribution and claims

Vague attribution phrases such as "Research suggests...," "Many experts believe...," "Studies show...," and "It is widely accepted that..." are removed, but the underlying claim is preserved if it is part of the author's argument.

If the claim is load-bearing and unverifiable in the source, flag it in the audit under "Ambiguity checks": "[Claim] preserved without attribution - verify before publishing." Do not silently delete claims, because deletion changes meaning.

Only delete the claim entirely if it is clearly filler that adds no information beyond the attribution itself. For example, "Research suggests that things are changing" is both an empty attribution and an empty claim.

Never invent a citation.

### Narration and shape audit

Word-level fixes are not enough. Audit the rebuilt piece for AI-style narration. AI writing tends to be too smooth, too symmetrical, too generically confident, and too narratively staged.

Flag and rewrite:

- paragraphs that all sit at similar length or use the same internal structure
- sections that repeat setup -> explanation -> takeaway
- claims that sound polished but contain no concrete actor, action, constraint, example, or consequence
- smooth transitions that exist only to move the piece along
- repeated scaffolds such as "This matters because," "The key is," "The result is," "In practice," "At a high level," "What's interesting is," or "At its core"
- sentences that perform insight instead of saying something specific
- endings that summarize, moralize, or announce a takeaway
- sections that present both sides without committing when the piece needs a position

If three or more consecutive sections follow the same internal movement, rebuild the section order or merge material until the structure follows the actual logic of the content.

### Claude-tone removal

Cut the classic Claude narrator voice on sight:

- grand framing such as "In the rapidly evolving landscape of...," "At the intersection of X and Y," or "As we navigate the complexities of..."
- "What this means" exposition unless genuinely needed for clarity
- neat morals at the end of sections
- fake balance when the piece needs a position
- closing lines that gesture at depth without saying anything concrete
- motivational cadence
- punch fragments
- over-explanation of simple ideas

If a sentence is obvious, cut it. If a paragraph only makes the draft feel smooth, cut it. Flow comes from logic, not connective tissue.

### Editorial substance

The fix for AI narration is concrete substance, not surface roughness. Do not add randomness, quirky phrasing, blanket contractions, intentional choppiness, or scattered fragments to make the writing "feel human."

Apply these positive instructions:

- Use concrete nouns and verbs over abstract ones.
- Name the actor when possible. "The migration broke the staging build" is better than "issues were encountered during deployment."
- Keep useful repetition. Humans repeat important terms instead of swapping in synonyms. Do not chase variety.
- Let transitions come from the logic of the previous sentence, not from stock connector phrases.
- Vary paragraph length only when the content naturally calls for it.
- Cut sentences that only restate the obvious or recap what was just said.
- Prefer one specific example over three generic claims.
- Prefer a plain direct claim over a polished insight sentence.

The output should sound like a serious human editor rebuilt the draft from source material. It should not sound like an AI tried to make the prose more engaging.

### Paragraph-purpose rule

Every paragraph must do one clear job: make a claim, give evidence, explain a consequence, provide an instruction, add necessary context, or move the reader to a genuinely new point.

Delete or rewrite paragraphs that only frame, summarize, soften, transition, or perform insight. A paragraph whose only job is to warm the reader up or tie sections together is usually cuttable.

### Specificity-density rule

Every substantive paragraph must include at least one concrete element when the source supports it: a named actor, specific action, hard constraint, worked example, measurable consequence, number, named object, or specific decision.

If the source has nothing specific to offer, do not pad with adjectives. Cut the paragraph or merge its single load-bearing sentence into an adjacent paragraph that carries specifics.

Generic paragraph:

> Companies that invest in customer success see better retention outcomes. This is increasingly important in competitive markets.

Rewrite with specificity:

> After moving two account managers from new sales to mid-market renewals, the SaaS company's retention rate moved from 78% to 91% over three quarters. Other teams in the same segment have reported similar shifts when they hire dedicated customer-success staff.

---

## Heading and Title Rules

Headings describe what the section contains, in plain language. They should not perform cleverness, urgency, or depth. The test: would this fit as a Wikipedia section heading, textbook chapter title, or explainer subsection? If it would feel out of place there, rewrite it.

**Banned in headings:**

- **Rhetorical questions** such as "Are We Hitting Targets?," "What if X?," or "Why Y matters?" Convert these to declarative descriptive phrases.
- **Contrastive constructions** such as "X, not Y," "More than X," "Beyond X," and "Not just X but Y." State the positive description alone.
- **Colon-subtitle marketing format** such as "Unlocking X: A Y Approach" or "Mastering X: The Z Guide." Pick one half and cut the other.
- **Vague noun-phrase abstractions** such as "Empowering Teams Through Innovation," "The Future of X," or "Driving Y Forward." Rewrite each as a concrete description of what the section actually covers.
- **Metric-as-headline** such as "Revenue down 12%," "40% productivity gain," or "Q3 Revenue Missed Plan by 12%." Numbers belong in the body where they have context. Convert to descriptive form: "Q3 revenue performance," "Productivity changes since launch," or "Cost reduction this year."

**Preferred shape:**

- Use descriptive declarative phrases.
- Stay under 12 words.
- Prefer specific over abstract.
- Use concrete subjects.
- Read like a section heading in a well-edited textbook or explainer, not a slide title or marketing email.

---

## Rhythm and Structure

Use a tone appropriate to the request. Professional is the default.

| Tone | Parenthetical asides | Voice |
|---|---|---|
| Professional | 1 max per 400 words | Plain, active, descriptive, restrained |
| Casual | 1 per 150-200 words | Direct, warm, uses "you" |
| Irreverent | 1 per 200 words | Skeptical, witty, opinionated |

Every sentence must still be complete. Starting a sentence with "And," "But," or "So" is allowed when it reads naturally and does not break grammar. Do not force it.

**Opening.** Rewrite or cut throat-clearing openings. Start with the real point, action, fact, tension, decision, or problem.

**Opinion and analysis.** When the content makes unsupported broad claims, reframe them as perspective instead of pretending certainty. Use phrasing such as "My read is...," "It seems to me...," or "The way I see it..." only when the piece can reasonably carry first-person perspective. Do not apply this to factual reporting, instructions, directives, or announcements.

**Anti-symmetry pass.** Scan consecutive paragraphs. If three or more paragraphs in a row are within 20% of each other in length, vary the structure by combining, splitting, trimming, or reordering material.

**Ending.** Do not add a wrap-up paragraph unless the content actually needs one. Replace predictable summary-style closings with a final substantive sentence, or stop after the last real point.

Never close with "Hope this helps," a generic call to action, a moral, a question asking for engagement, or a sentence that announces itself as the takeaway.

---

## Content-Type Adjustments

### Presentation / Slides

Presentations apply all rules above, plus these constraints:

- Use one idea per bullet.
- Use active voice.
- Replace corporate verbs: use "Build" instead of "Foster," "Cut" instead of "Streamline," and "Use" instead of "Leverage."
- Strip rhetorical embellishment from bullets and titles.
- Do not add parenthetical asides.
- Keep slide text brief, but do not use fragments as punch lines.

### Proposal / Business Docs

Proposals and business documents apply all rules above, plus these constraints:

- Avoid effusive openers such as "We are pleased to..." or "It is our honor to..."
- Avoid reader-addressing scaffolding such as "As you can see," "It's worth noting that," or "What this means is."
- Use no marketing slogans in section bodies or titles.
- Preserve precision: numbers, version identifiers, named standards, RFP section references, and commitments stay exact.
- Default to third-person formal voice unless the user asks otherwise.

---

## Pre-flight Checks

Before producing output, scan the rewritten content for AI-tell patterns that may have survived the rewrite. If any pattern matches, re-edit that passage before output.

**Patterns to scan:**

- em dash count above the budget
- contrastive constructions in body or headings: `not just`, `rather than`, `More than`, `Beyond `, `At its core`
- banned vocabulary: `seamless`, `robust`, `leverage`, `delve`, `unlock`, `holistic`, `tapestry`, `resonate`, `transformative`, `revolutionary`, `cutting-edge`
- banned transitions: `Furthermore,`, `Moreover,`, `Additionally,`, `What's more,`
- reader-addressing scaffolding: `Here's`, `What this means`, `It's worth noting`, `It is important to note`, `Let me explain`, `Let me break`
- filler openers: `In today's`, `In an era of`
- marketing slogans: any heading containing both a colon and a noun-phrase subtitle

**Heading audit.** Every heading passes the heading rules above.

**Fragment audit.** Scan every sentence, bullet, caption, spoken line, and slide sentence. If it lacks a subject, lacks a finite verb, reads as a standalone noun phrase, reads as a standalone adjective phrase, or reads as a setup label, rewrite it as a complete sentence or delete it. Pay extra attention to sentences under 10 words, but do not limit the audit to short sentences.

**Comma-chain audit.** Scan for sentences with three or more commas joining independent clauses. Break each into shorter complete sentences with periods.

**Symmetry audit.** Check that three or more consecutive paragraphs do not sit within 20% of each other in length, and that three or more consecutive bullets do not share the same grammatical shape.

**Narration audit.** Read the output as a whole. Check for repeated setup -> explanation -> takeaway patterns, unsupported polished claims, scaffold transitions, moralizing section endings, performed-insight sentences, and closing lines that gesture at depth without concrete meaning.

**Final human-read test.** Read the piece as a reader, not as a checklist. Would this sound normal if it came from a competent editor, founder, operator, analyst, or subject-matter expert? Would the intended reader recognize the headings and labels immediately? If it sounds like a polished narrator explaining the importance of the topic, or if it uses internally accurate terms the reader would not use, rewrite it. If it sounds like a person clearly saying what they mean, keep it.

---

## Output

Return only the humanized content with no preamble. Do not append an audit or Risk Flags block by default. The output should look like an edited draft, not a tool report.

**Append the audit and Risk Flags only when at least one of these conditions holds:**

- The user explicitly asked for the audit.
- The sensitivity check fired.
- The long-input warning fired.
- Meaning risk is medium or high.
- The complete-sentence check or narration audit failed and residual issues remain.
- Ambiguity checks flagged any unverifiable claim that was preserved without attribution.

If none of those conditions hold, return only the rewritten content and stop.

When showing an audit, include only the useful risk details:

- whether the draft was rebuilt from source inventory
- content type and tone
- meaning risk: low, medium, or high
- any preserved unverifiable claims
- whether complete-sentence and narration checks passed
- any residual issue the user should review before publishing

Show the full change log only when the user asks for it. Keep it factual: structure changes, opening/ending rewrites, removed filler, fixed fragments, rewritten headings, rebuilt sections, and ambiguity checks. Do not invent exact counts when you are not confident.

---

## Notes

- Never invent citations. If you cannot attribute a claim, flag it. Do not delete the claim or fabricate a source.
- Never change the author's meaning. Change structure and expression, not the underlying facts or commitments.
- If something is already human and works, keep it.
- When choosing between two phrasings, pick the shorter, plainer one.
- Real quoted speech is untouchable. People actually say banned words sometimes.
- The complete-sentence rule outranks every other rule in this skill.
- Heading rules apply universally across content types, including the metric-as-headline ban.
- The editor running this skill must apply judgment beyond the checklist. The rules catch patterns; the editor catches shape.
