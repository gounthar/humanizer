# Detection Guidance

Read this before rewriting anything. `SKILL.md` lists what a tell looks like; this file lists
what only looks like one, and what to leave alone. Every pattern in the skill can fire on prose
a person wrote on purpose, and these carve-outs are what stop a clean rewrite coming out worse
than the draft.

Section numbers below refer to the patterns in `SKILL.md`.

## What NOT to flag (false positives)

A clean human writer can hit several of the patterns without any AI involvement. Before rewriting, sanity-check that you are not gutting legitimate prose. The following are *not* reliable indicators on their own:

- **Perfect grammar and consistent style.** Many writers are professionals or have been edited. Polish does not equal AI.
- **Mixed casual and formal registers.** This often signals a person in a technical field, a young writer, or someone with neurodivergent prose habits — not a chatbot.
- **"Bland" or "robotic" prose.** AI prose has *specific* tells. Generic dryness without those tells is just dry writing.
- **Formal or academic vocabulary.** AI overuses *specific* fancy words (see §7), not all fancy words. Don't flatten "ostensibly" or "constituent" just because they sound brainy.
- **Letter-style opening or closing on a comment.** Salutations and sign-offs predate ChatGPT by centuries.
- **Common transition words in isolation.** *Additionally*, *moreover*, *consequently* are AI-coded only when piled up. One *however* is not a tell.
- **Curly quotes alone.** macOS, Word, Google Docs, and most CMSes auto-curl by default. Curly quotes only count when stacked with other tells.
- **Em dashes alone.** Many editors and journalists use them often. Em dashes are evidence only when paired with formulaic sales-y rhythm.
- **One short emphatic sentence.** Humans use clipped sentences to land a point. Flag staccato drama only when several short fragments appear in a row and inflate the tone.
- **"Honestly" or "look" mid-sentence.** These are ordinary in casual writing. The tell is the standalone theatrical opener, not the word itself.
- **Unsourced claims.** Most of the web is unsourced. Lack of citations doesn't prove anything.
- **Correct, complex formatting.** Visual editors and templates produce clean output without any AI.
- **Secondhand text.** Do not rewrite watched phrases inside quotations, titles, proper names, or examples where the phrase is being discussed rather than used.
- **Deliberate parallelism.** Repeating a frame on purpose is one of the oldest tools in rhetoric, and speeches, aphorisms, and closing lines earn it. §39 is about frames the writer did not choose — sentence after sentence in the same mould with no reason for the echo. If removing the repetition weakens the point, keep it.
- **Established compound nouns.** *Continuous integration pipeline*, *garbage collection pause*, *pull request template*. §41 targets stacks assembled on the spot, not fixed terms of art the field already reads as single units.
- **Short unsubordinated sequences.** Two flat sentences in a row are normal, and instructions, recipes, and procedures are flat by design because the steps really are equal. §42 needs a sustained run before it means anything.
- **Nominalizations with no verb behind them.** *Information*, *quality*, *evidence*, and *policy* are not disguised verbs. §40 is about actions turned into nouns, not every abstract noun in the draft.

When in doubt, look for **clusters** of tells, not isolated ones. A single em dash means nothing; em dashes plus rule-of-three plus *vibrant tapestry* plus a "Conclusion" section is a confession.

## Signs of human writing (preserve these)

When you see these, lean toward leaving the prose alone — they are evidence of a real person writing, and over-editing will destroy what makes the piece sound human:

- **Specific, unusual, hard-to-fabricate detail.** A real address. A weird quote. The phrase "the lawyer who used to work upstairs from my dentist." LLMs round off specifics; humans hoard them.
- **Mixed feelings and unresolved tension.** "I think this is mostly good, but it bothers me, and I can't fully explain why." LLMs default to clean takes.
- **Dated, era-bound references.** Slang, memes, or in-jokes that map to a specific year and subculture. Models lag by a year or more.
- **First-person editorial choices the writer can defend.** If the writer can explain *why* they made a particular cut or used a particular word, that's a strong human signal.
- **Variety in sentence length.** Real writing alternates short and long. AI writing tends toward an even, mid-length cadence.
- **Genuine asides, parentheticals, or self-corrections.** "(I keep wanting to say 'almost' here, but it really was certain.)" Models rarely interrupt themselves like this.
- **Edits made before November 30, 2022.** ChatGPT's public launch. Anything older than that is, with very rare exceptions, not AI-written.

## The read-aloud pass

Step 3 of the process in `SKILL.md`. Read the draft as if saying it to somebody, and mark every place where you would stumble, pause somewhere odd, run out of breath, or catch yourself rewording on the fly.

The marks locate a defect, they do not name one. Each points back at a pattern:

| What happens when you say it | Where to look |
|---|---|
| Breath runs out before the point lands | §42 clause chains, §41 stacked nouns |
| Several sentences in the same even beat | §39 parallel frames, sentence rhythm in NUMERIC CHECKS |
| A phrase that names no action trips you | §40 nominalization |
| A pause that feels staged rather than needed | §31 staccato drama, §37 colon reveals |
| Facts arrive with nothing saying how they relate | §42 |

Fix by that section's rule. Do **not** fix by smoothing, and note that the two can look identical. What separates them is whether the connective carries a relation. §42 wants a subordinator that states how two facts actually stand to each other (*because*, *although*, *since*), and supplying one is the fix. A connective dropped in to ease a transition while carrying no relation (*Additionally*, *Moreover*, *That said*) is §28 signposting, and supplying one is a new defect. Rounding off a sentence that is awkward but accurate is the other way to fail here, because it usually costs the specific detail that made the prose sound like a person (see Signs of human writing above).

Two limits on this pass, and they outrank the table. Most prose is written to be read rather than said, so a stumble is a reason to look, never a verdict on its own: where the sentence is clear on the page and the section it routes to does not actually apply, leave it alone. And where the text concedes an error to somebody who has just caught it, expect the concession to come out awkward and leave it that way. That limit is narrow on purpose. A calm, clear concession is fine in most writing, and this is only about the apology polished into a shape that reads as performed.
