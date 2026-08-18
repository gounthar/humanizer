# Humanizer

[![skills.sh installs](https://skills.sh/b/blader/humanizer)](https://skills.sh/blader/humanizer)

A portable agent skill that removes signs of AI-generated writing from text, making it sound more natural and human. It is plain Markdown, so it can run in any harness that supports skill-style instructions.

## Installation

### Skills CLI

Install globally with the cross-agent skills CLI so Humanizer is available in every project:

```bash
npx skills add blader/humanizer --global
```

Update an existing install:

```bash
npx skills update humanizer --global
```

To install globally into every supported agent harness:

```bash
npx skills add blader/humanizer --global --agent '*'
```

To target one configured harness, pass its agent name:

```bash
npx skills add blader/humanizer --global --agent <agent-name>
```

Omit `--global` for a project-local install that can be committed and shared with collaborators. Start a new agent session or reload skills after installation.

### Claude Code plugin

Claude Code users can also install Humanizer as a plugin:

```
/plugin marketplace add blader/humanizer
/plugin install humanizer@humanizer
```

The skill is then invoked as `/humanizer:humanizer`.

### Manual

Any agent harness can use the skill directly. The runtime artifact is `SKILL.md` plus the two files in `references/`. `detection-guidance.md` holds the false-positive carve-outs, the signs of human writing, and the read-aloud pass; `SKILL.md` tells the agent to read it before rewriting, so copy it or the carve-outs go missing and the skill will flatten prose a person wrote on purpose. `decision-ledger.md` is how a project banks the lines its author has accepted or rejected, and is only consulted where such a ledger exists.

For example:

```bash
git clone https://github.com/blader/humanizer.git /path/to/your/skills/humanizer
```

Or, if you already have this repo cloned:

```bash
mkdir -p /path/to/your/skills/humanizer/references
cp SKILL.md /path/to/your/skills/humanizer/
cp references/*.md /path/to/your/skills/humanizer/references/
```

## Usage

Invoke the skill however your agent harness exposes installed skills. Common forms include a slash command or a direct request:

```
/humanizer

[paste your text here]
```

```
Please humanize this text: [your text]
```

Point it at a file and the skill rewrites it in place:

```
Humanize the prose in docs/launch-post.md
```

### Voice Calibration

To match your personal writing style, provide a sample of your own writing:

```
/humanizer

Here's a sample of my writing for voice matching:
[paste 2-3 paragraphs of your own writing]

Now humanize this text:
[paste AI text to humanize]
```

The skill will analyze your sentence rhythm, word choices, and quirks, then apply them to the rewrite instead of producing generic "clean" output.

## Overview

Based on [Wikipedia's "Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) guide, maintained by WikiProject AI Cleanup. This comprehensive guide comes from observations of thousands of instances of AI-generated text.

The skill also includes a final "obviously AI generated" audit pass and a second rewrite, to catch lingering AI-isms in the first draft.

Rewrites follow a no-fabrication rule: they never add facts, names, dates, or citations that aren't in the source text. Specificity has to come from the source or the author, not from the rewrite.

One step sits between the draft and the audit: the read-aloud pass. Say the draft as if talking to somebody, and mark wherever you stumble, pause somewhere odd, run out of breath, or reword on the fly. A mark shows where to look rather than what is wrong, so each one routes back to a section, and that section decides whether anything is wrong at all. Smoothing the sentence is not a fix; it reintroduces signposting and tends to cost the specific detail that made the prose sound like a person.

Two rules sit alongside the pattern list. Detector evasion is banned outright: no homoglyphs, invisible characters, planted typos, or paraphrase-spinning, because they break copy-paste, search, and screen readers without making the writing any better. And three checks are countable rather than judgment calls, so the skill verifies them against the final rewrite: hedging density (at most one per 300 words, counting only qualifiers stacked on a single claim), sentence rhythm (at least one sentence of six words or fewer per 120 words), and list length (two or four items, not three or five).

### Key Insight from Wikipedia

> "LLMs use statistical algorithms to guess what should come next. The result tends toward the most statistically likely result that applies to the widest variety of cases."

## 42 Patterns Detected (with Before/After Examples)

`SKILL.md` carries the same 42 with their false-positive carve-outs.

### Content Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Significance inflation** | "marking a pivotal moment in the evolution of..." | "was established in 1989 as part of a wider decentralization" |
| 2 | **Notability name-dropping** | "cited in NYT, BBC, FT, and The Hindu" | Trim the list; keep only sourced context |
| 3 | **Superficial -ing analyses** | "symbolizing... reflecting... showcasing..." | Remove, or keep only what the source supports |
| 4 | **Promotional language** | "nestled within the breathtaking region" | "is a town in the Gonder region" |
| 5 | **Vague attributions** | "Experts believe it plays a crucial role" | Name a real source or cut the claim |
| 6 | **Formulaic challenges** | "Despite challenges... continues to thrive" | Keep the sourced facts; cut the boosterism |

### Language Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 7 | **AI vocabulary** | "Actually... additionally... testament... landscape... showcasing" | "also... remain common" |
| 8 | **Copula avoidance** | "serves as... features... boasts" | "is... has" |
| 9 | **Negative parallelisms / tailing negations** | "It's not just X, it's Y", "..., no guessing" | State the point directly |
| 10 | **Rule of three** | "innovation, inspiration, and insights" | Use natural number of items |
| 11 | **Synonym cycling** | "protagonist... main character... central figure... hero" | "protagonist" (repeat when clearest) |
| 12 | **False ranges** | "from the Big Bang to dark matter" | List topics directly |
| 13 | **Passive voice / subjectless fragments** | "No configuration file needed" | Name the actor when it helps clarity |

### Style Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 14 | **Em/en dashes** | "institutions—not the people—yet this continues—" | Cut them: periods, commas, colons, or parentheses |
| 15 | **Boldface overuse** | "**OKRs**, **KPIs**, **BMC**" | "OKRs, KPIs, BMC" |
| 16 | **Inline-header lists** | "**Performance:** Performance improved" | Convert to prose |
| 17 | **Title Case Headings** | "Strategic Negotiations And Partnerships" | "Strategic negotiations and partnerships" |
| 18 | **Emojis** | "🚀 Launch Phase: 💡 Key Insight:" | Remove emojis |
| 19 | **Curly quotes** | `said “the project”` | `said "the project"` |
| 26 | **Hyphenated word pairs** | “cross-functional, data-driven, client-facing” | Drop hyphens on common word pairs |
| 27 | **Persuasive authority tropes** | "At its core, what matters is..." | State the point directly |
| 28 | **Signposting announcements** | "Let's dive in", "Here's what you need to know" | Start with the content |
| 29 | **Fragmented headers** | "## Performance" + "Speed matters." | Let the heading do the work |
| 30 | **Diff-anchored writing** | "This function was added to replace..." | Describe what it does, not what changed |
| 31 | **Manufactured punchlines / staccato drama** | "It had no preference. No prior. No nostalgia." | Use varied sentence lengths and concrete claims |
| 32 | **Aphorism formulas** | "Symmetry is the language of trust" | Replace the formula with the actual claim |
| 33 | **Conversational rhetorical openers** | "Honestly? It depends..." | Remove the fake-candid setup |
| 34 | **Tables where prose belongs** | One-row table of "aspect / description" pairs | Write it as a sentence |
| 35 | **Skipped heading levels** | `## Installation` then `#### Prerequisites` | Step down one level at a time |
| 36 | **Thematic breaks before headings** | `---` sitting just above a heading | Let the heading start the section |
| 37 | **Colon reveals** | "The best part: it retries on its own." | Rewrite as a plain sentence |
| 38 | **Faux-insight setups** | "Here's what nobody tells you: ..." | Let the claim stand by itself |
| 39 | **Parallel sentence frames** | "The parser handles quotes correctly. The parser handles escapes correctly." | Merge them, or subordinate one into the other |
| 40 | **Nominalization** | "performed an analysis and made a determination" | "read the logs and rolled back" |
| 41 | **Stacked noun phrases** | "customer engagement optimization workflow" | Unstack it with prepositions |
| 42 | **Unsubordinated clause chains** | "The cache was cold. The build took nine minutes." | "Because the cache was cold, the build took nine minutes" |

### Communication Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 20 | **Chatbot artifacts** | "I hope this helps! Let me know if..." | Remove entirely |
| 21 | **Cutoff disclaimers** | "While details are limited in available sources..." | Find sources or remove |
| 22 | **Sycophantic tone** | "Great question! You're absolutely right!" | Respond directly |

### Filler and Hedging

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 23 | **Filler phrases** | "In order to", "Due to the fact that" | "To", "Because" |
| 24 | **Excessive hedging** | "could potentially possibly" | "may" |
| 25 | **Generic conclusions** | "The future looks bright" | Specific plans or facts |

## Full Example

*(Illustration note: the rewrite below adds specifics, like the month and the neighborhoods, that stand in for details the author would supply. In a real session those come from the user; the skill asks rather than invents.)*

**Before (AI-sounding):**
> I recently spent five unforgettable days in Lisbon, and let me tell you — this city completely stole my heart. From the moment I arrived, I knew I was somewhere truly special.
>
> Nestled along the banks of the Tagus River, Lisbon stands as a vibrant testament to Portugal's enduring spirit, where rich history and modern energy intertwine at every turn. Yes, the famous hills are challenging — my legs certainly felt it! — but every climb rewards you with breathtaking, panoramic views that make it all worthwhile.
>
> No trip would be complete without riding the iconic Tram 28, winding through the city's most historic neighborhoods. And the food? Simply divine. The original pastéis de nata at Pastéis de Belém are a beloved national treasure, and savoring one still warm was a moment I will never forget.
>
> But what truly makes Lisbon special isn't just the sights — it's the feeling. Wander a few steps off the main squares and you'll discover a quieter, more authentic side: sun-drenched alleys, charming tiled facades, and friendly locals going about their daily lives, inviting you to slow down and savor every moment.
>
> Of course, the must-see São Jorge Castle offers stunning views over the rooftops below. The queues can be long, but the experience is absolutely worth it for any history buff or curious traveler.
>
> Would I go back? Absolutely. Lisbon isn't just a place to visit — it's a place to fall in love with, again and again. If you're dreaming of your next getaway, this is one destination that promises memories to last a lifetime. ✨

**After (Humanized):**
> I spent five days in Lisbon last October and still have mixed feelings about it. Beautiful, yes. Also harder on the knees than anyone warned me.
>
> The hills are the whole story and somehow never make the brochures. My hotel was up in Alfama, which photographs beautifully and translates, in practice, to climbing what felt like a six-story staircase every time I wanted coffee. By the second day my calves had opinions. I started planning each walk around which way was downhill, which is a strange way to see a city but probably an honest one.
>
> Everyone says to ride Tram 28, so I did, wedged against a stranger's backpack for forty minutes while three tour groups filmed the same corner. I would walk the route next time, or go before breakfast. The custard tarts, though, earn the fuss. I had one at a plain little place in Graça, still warm, and for about thirty seconds I understood why people build trips around pastry.
>
> What I did not expect was how quiet the city gets away from the main squares. Two blocks uphill from any plaza it turns into laundry lines, chipped tile, open windows, and old men watching football with the sound turned up. That is the Lisbon I keep thinking about, not the castle.
>
> The castle is fine. The view is great, the queue is long, and I spent more time shuffling toward the entrance than looking at anything once I got inside. If I had only two days, I would trade it for an afternoon of getting lost.
>
> I would go back, but in spring and with better shoes. Lisbon does not bend over backward to make things easy for you. I think I liked that, even when my legs disagreed.

## References

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) - Primary source
- [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) - Maintaining organization
- [no-ai-slop](https://github.com/petergyang/no-ai-slop) (MIT) - Source of the pattern names for #37 and #38
- [humanize-writing](https://github.com/marian-kamenistak/humanize-writing) (MIT) - Source of the numeric thresholds and the no-evasion stance in 2.12.0
- [respeak](https://github.com/quietmill/respeak) (MIT) - Source of the confidence-calibration rule in §24 and the decision ledger in 2.14.0

## Version History

- **2.14.0** - Split §24 so that removing a hedge is treated as a factual change. Stacked
  qualifiers are still the defect; the last qualifier on a claim the source does not settle is
  not, and the rewrite may not turn *possible* into *likely* to save a word. The hedging figure
  in Numeric Checks now counts only those stacks, because a threshold that counts honest
  uncertainty as filler pushes toward inventing confidence, which is the fabrication the
  no-fabrication rule went in for in 2.9.0 arriving from the other direction. The matching
  carve-out is in `references/detection-guidance.md`. Also added `references/decision-ledger.md`:
  a format for banking the lines an author accepted or rejected, with the reason classified so a
  ruling about accuracy travels further than one about taste. Both come from
  [respeak](https://github.com/quietmill/respeak) (MIT), read at commit `ac10b2c` on 2026-08-18;
  the wording, the reach table, and the cross-references to §14, §24, Voice Calibration and Your
  Task are written for this skill. Deliberately left there: respeak's proposition ledger and
  four-pass method, which restate work this skill already does in Your Task and Process and
  Output, and its per-surface registers for UI copy, narration, and localisation, which are a
  different job from removing AI tells. Still 42 patterns.
- **2.15.0** - Extended §12 to cover the numeric form of a false range: a span such as *5 to 10
  minutes* or *a 20-30% improvement*, where the width is the absence of a measurement rather
  than an error bar. §12 previously covered only rhetorical *from X to Y* sweeps across
  non-scales, which is a different construction. The repair is deliberately framed as an
  evidence question and cross-referenced to §24 and step 3 of Your Task, because narrowing the
  span without measuring invents a fact, and that is the failure mode a wording fix walks into.
  Carries two carve-outs: an interval that reports its basis survives, and so does a range that
  is genuinely the answer. Prompted by a pattern list circulated by
  [@rubenhassid](https://x.com/rubenhassid/status/2087856703773508025) on 2026-08-13, read on
  2026-08-18. Eight of its nine items were already covered here, two of them verbatim
  (*the part everyone misses* in §38, *X is the Y of Z* in §32), and the numeric range was the
  only gap. Deliberately left out: its ASD-STE100 recommendation, which is a controlled language
  for aircraft maintenance manuals and conflicts with PERSONALITY AND SOUL and Voice Calibration
  outside reference documentation; and its premise that AI no longer uses em dashes, which is an
  assertion this project measured and rejected. Still 42 patterns.
- **2.13.0** - Added four patterns (§39-42) covering structure rather than vocabulary: parallel
  sentence frames within a paragraph, nominalization, stacked noun phrases, and unsubordinated
  clause chains. Each carries a false-positive carve-out, since all four describe constructions
  that competent human writers use on purpose. 42 patterns. Also added a read-aloud pass to the
  process: say the draft, mark where you stumble or run out of breath, then look up what the
  stumble means. It finds no new defects of its own. It routes to the sections that already own
  them, which is why it earns its place next to four structural patterns that are hard to see
  on the page. It is detect-only by design: fixing a stumble by smoothing the sentence would
  reintroduce §28 signposting and cost the specific detail that made the prose sound human.
  Four patterns and a new process step did not fit the 500-line portability budget, so the
  detection guidance moved to `references/detection-guidance.md`: the false-positive list, the
  signs of human writing, and the read-aloud pass table. `SKILL.md` now tells the agent to read
  that file before rewriting, and the package validator fails if a `references/` path named in
  `SKILL.md` is not shipped alongside it.
- **2.12.0** - Added a no-detector-evasion rule (no homoglyphs, invisible characters, planted typos, or paraphrase-spinning) and a Numeric Checks section with three countable thresholds: hedging density, sentence rhythm, and list length. Both come from [humanize-writing](https://github.com/marian-kamenistak/humanize-writing) (MIT); the wording, the carve-outs for voice samples and short text, and the cross-references to §10, §24, and §31 are written for this skill. Still 38 patterns.
- **2.11.0** - Added patterns #37 (colon reveals) and #38 (faux-insight setups). Both pattern names come from [no-ai-slop](https://github.com/petergyang/no-ai-slop) (MIT); the rule text and examples here are written for this skill. 38 patterns total.
- **2.10.0** - Added structural/formatting patterns #34-36: tables where prose belongs, skipped heading levels, and thematic breaks before headings. 36 patterns total.
- **2.9.1** - Improved distribution and portability: removed nonportable frontmatter and tool preapprovals, made global installation the documented default, added package validation, and removed the duplicated long-form example from the runtime prompt. No change to the 33 patterns.
- **2.9.0** - Added a no-fabrication rule: rewrites may not invent facts, names, dates, or citations not present in the source, and every example that modeled invented specifics was re-cut to use only source information (fixes #187). Replaced paragraph-count parity with an information-over-shape rule, made a user's voice sample outrank the em dash ban, and added invocation modes (pasted text / file / embedded). No change to the 33 patterns.
- **2.8.3** - Moved the skill version from the unsupported top-level frontmatter key to `metadata.version` for Agent Skills and Claude compatibility. No change to the 33 patterns.
- **2.8.2** - Replaced the full before/after example with a first-person Lisbon trip recap. The after now keeps the same topic, perspective, and rough length as the before while removing the AI tells without becoming clipped or slogan-like. No change to the 33 patterns.
- **2.8.1** - Added cross-agent installation docs, optional Claude Code plugin packaging, and a compact secondhand-text false-positive guard. No change to the 33 patterns.
- **2.8.0** - Added style/cadence patterns #31-33 for manufactured punchlines, aphorism formulas, and conversational rhetorical openers; expanded #20 to catch offer-to-continue chatbot closers. 33 patterns total.
- **2.7.0** - Added pattern #30 (diff-anchored writing); made em/en dashes a hard cut rather than "overuse"; expanded #21 to cover speculative gap-filling ("maintains a low profile"). 30 patterns total.
- **2.6.0** - Cleanup pass: consolidated the duplicated workflow sections, gated the personality guidance to content where voice is wanted, removed the model-fingerprinting subsection, and condensed the worked example. No change to the 29 patterns.
- **2.5.1** - Added a passive-voice / subjectless-fragment rule, raising the total to 29 patterns
- **2.5.0** - Added patterns for persuasive framing, signposting, and fragmented headers; expanded negative parallelisms to cover tailing negations; tightened wording around em dash overuse; fixed frontmatter wording to use "filler phrases"
- **2.4.0** - Added voice calibration: match the user's personal writing style from samples
- **2.3.0** - Added pattern #25: hyphenated word pair overuse
- **2.2.0** - Added a final "obviously AI generated" audit + second-pass rewrite prompts
- **2.1.1** - Fixed pattern #18 example (curly quotes vs straight quotes)
- **2.1.0** - Added before/after examples for all 24 patterns
- **2.0.0** - Complete rewrite based on raw Wikipedia article content
- **1.0.0** - Initial release

## License

MIT
