# How this catalog was made

This catalog was compiled using **Claude Code**, a coding agent by the AI company **Anthropic**, between April 23 and May 19, 2026.  The bulk of the work was done with the model *Opus 4.7*, with 'xhigh' effort.  Towards the end of the period, a switch to Sonnet 4.6 with 'medium' effort was made.  Running out of tokens, the last 74 texts in KR4k and the overview of section KR4k was made using the same harness, but with the model *Deepseek V4 Pro* instead.  The Anthropic sourced interactions were covered by a Max 20x plan for 1 month, at the cost of `US$ 220`, while the Deepsek part run to roughly `US$ 10`, bringing the total cost to `US$ 230`.   To make it easier to talk about this, I nicknamed the agent [[Bunkankun]], affectionately abbreviated to **BKK**, which is how I will refer to it below.

The process was set up as an unsupervised autonomous process, where the agent was instructed to go over all texts in Kanripo one after another and carry out a certain number of steps to produce the desired result.  Setting up the system in a satisfying way was key to the successful completion. 

## Setting up the environment

When starting this project, I had no specific experience with AI agents, except a few days of playing around with it for light coding tasks.  The first few days were spent setting up the environment and establish a viable path, after which the process went ahead without much oversight.   I will try to describe the lessons learned during this startup phase.

Key to interacting with AI models is to be as explicit as possible about the expected response. In interactive chat sessions, this is usually evolving in a question-and-answer form of interaction, where the AI responds directly to a user request.  The main difference between chat sessions and agent use is the so-called *harness*: A local program that receives the input and decides what to do with it, for example calling tools or other agents, or sending it directly to the model for response.  The response will also be received by the harness, and acted upon, until at some point a final action brings this loop to an end: a message to the user, or  a newly written catalog entry in our case. 

For agentic use, written instructions also form part of the harness.  Some of these are very basic instructions that set up the agent for a specific task, others are more fleeting and depend on the context.  In the case of Claude Code, this fundamental file is called `CLAUDE.md`.  The first part of the bootstrapping process was therefore to set up this file.  I did this in an interactive manner, trying some requests and refining these according to the result, then updated the agent file with the result.   In addition to the task instruction, I drafted a  basic template for the catalog entry, called `template.md`   Eventually, I asked the agent to go over the draft and suggest improvements. 

The first few texts were cataloged interactively, where I could observe what was going on.  Claude Code is pretty chatty and explains what it is doing when used in interactive mode.  Also, by default it requires permission to use tools and, for example go searching on the web.  This permission can be given for a class of actions, for example a web search.  After I gave this permission for web searches, I could observe that BKK was working on a Daoist file and went looking for a copy of Schipper/Verellen's *The Daoist Canon*, found it, downloaded it (somewhere on the *Internet Archive*) and stored it for future reference.  Since the results looked quite promising, I then decided to actually make a full run for the catalog. 

## Sources

The first samples did look promising, but they have also shown that it is quite necessary to anchor the work in good existing data.  Fortunately, I had quite a few files that could be used for this:

1) The 'WWW Database of Chinese Buddhist texts', online since 1997, in a revised version available at [CANWWW](http://www.kanji.zinbun.kyoto-u.ac.jp/~wittern/can/can2/ind/canwww.htm).
2) A number of text lists and metadata collections, mostly compiled while working on the Daozang Jiyao project with the late Monica Esposito, and after that while compiling the Kanseki Repository.  Some of these lists are also public at [KR-Catalog](https://github.com/kanripo/KR-Catalog) .
3) More work went into the master text list of the Kanseki Repository, `krp-titles.txt` -- this is not public, but can be searched with the [Titles searchbox at Kanripo](https://www.kanripo.org)
4) In ca. 2018, the krp-titles and the text list of [HXWD](https://hxwd.org/search.html?search-type=12) diverged, with the latter having gained large numbers of new texts that did not make it into Kanripo. 

Other publicly available reference material were also provided to BKK for convenient local reference, e.g.
1) CBDB in sqlite format, downloaded from [China Biographical Database Project](https://huggingface.co/datasets/cbdb/cbdb-sqlite/tree/main/history/cbdb_202603), at Huggingface, the version used here was of 2026-03-28.
2) Authority files from DILA, published at [DILA-edu](https://github.com/DILA-edu/Authority-Databases), accessed  2026-04-27.

The most consequential of all resources was without doubt *Chinese History: A New Manual, Seventh Edition* by **Endymion Wilkinson**, (2025 ebook version). I instructed BKK to look here first and log any findings to make sure that the information was as accurate and up to date as possible.  Other sources used were found and collected by BKK, they are cited where appropriate.  

### Data format
Since I had used Obsidian in other projects and found it suitable for the purpose at hand, I designed a template that allowed me to collect some structured information in the `frontmatter` part, as well as a free-form essay in the body of each file.  Again, this went over a few iterations, but at the end it looked like this:
```yaml
textid: KR1a0002
title: 子夏易傳
titlePinyin: Zǐxià Yìzhuàn
titleEnglish: "Zǐxià's Commentary on the Changes"
extent: 11 卷
notBefore: 618
notAfter: 1279
dynasty: 周
persons:
  - "[[卜商]] (attributed)"
commentedTextid: KR1a0001
editions:
  - WYG
source: 四庫全書 文淵閣版, V7.1, p1
created: 2026-04-23
updated: 2026-05-16
reviewed: 2026-05-16
```

Not all fields applied everywhere, but this was the core -- a mixture between traditional cataloging practice, e.g. the `dynasty` field and modern conventions.  I was especially interested in the `notBefore` and `notAfter` bracket to position a text on a timeline. The bracket is especially wide for this text, the body should give the reasoning, which in this case reads: 

```quote
The frontmatter dating window (618–1279, Táng through Southern Sòng) reflects the assumed actual composition and recompilation history of the received recension, not the lifedates of the attributed author.
```
As for the body of the notes, a certain skeleton was given to BKK to fill in, which read in part like this:
```md
# {titlePinyin} {title}
-- Pinyin title (with tone marks) followed by the Chinese title on the same line. Both also in the prolog.
 *{titleEnglish}*
-- translate the title into English as good as possible
by {persons}
-- List all persons here. Each is linked via `[[漢字名]]` with the bare Kanji name (filename in kb/Persons/<漢字>.md). The per-work function (撰, 編, 註, 輯, 監修, 編纂, 奉敕撰, 記, 考補, attributed, etc.) is given both in the frontmatter `persons:` list (as `"[[name]] (function)"`) and in prose on first mention.
-- Per-person structured data (lifedates, dynasty, alternate names, cbdbId, dilaAuthorityId, biography stub) lives in the person note, not here.

## About the work
-- One paragraph framing the work: what it is (commentary / treatise / gazetteer / liturgy / zàn), how long, tradition, and the notable feature a reader must know. If it is a commentary, mention the parent title here and put the textid in the prolog `commentedTextid`; otherwise delete that field.

### Tiyao
-- (only for texts from KR1 to KR4) If the text is in WYG, translate the 提要 here (literary but clear English). Strip `<pb:...>`, `¶`, and `(臣/)` elisions; preserve substantive interlinear notes. Preserve typographical slips in the source (e.g. 紀均 for 紀昀, 托克托 / 脫脫) and flag them in a parenthetical note. If no tiyao is in the source file, check the Kyoto Zinbun digital 四庫提要 (see CLAUDE.md § Sources). If still not found, write `*No tiyao found in source.*`.

### Abstract
-- Research the text and write a concise English summary. Anchor claims in evidence: prefaces, postfaces, colophons within the text first; then bibliographic catalogs (《漢書·藝文志》, 《隋書·經籍志》, 《崇文總目》, 《經義考》…); then modern reference works. Mark uncertainty explicitly. Note catalog-vs-external date discrepancies when correcting them. Note pseudepigraphy, recompilation, fragmentary transmission where relevant.

## Translations and research
<span style="color: red;">Reminder: <b>AI generated list</b> - beware of hallucinations. Corrections and additions welcome</span>
-- Translations (usually none in English for most texts). Substantial secondary studies only — monographs, critical editions, peer-reviewed articles. Skip casual mentions. If nothing is located, write `*No substantial secondary literature located.*`.

## Other points of interest
-- Optional. Include only if there is something genuinely notable. Otherwise omit the section.

## Links
-- provide links to references used for compiling this entry. Wikipedia and Wikidata links if available.
```

This gives the basic skeleton; BKK received also instructions on how to proceed in a `command` `catalog-continue` . In addition to the text notes, there was also a template for persons related to the text, which in the end amounted to a list of more than 8000 persons with a bibliographically relevant relation to at least one of the texts.  The persons are explicitly linked here, so that the relation can be discovered and followed in Obsidian and on the Web interface (as `Backlinks`); this is also the reason the Chinese name is used directly here.  BKK has been instructed to add several records to the same file, if there are persons with the same name. 

There are 103 subsections (including sub-subsections of [[KR3e/|KR3e]] and [[KR3f/|KR3f]]), for these BKK was asked to provide overview notes once all texts where processed, again with a fixed skeleton of sections, including 
- Scope and scholarly tradition
- Important texts and text clusters
- Important persons
- Topics
- Timeline
This should provide a first overview and entry point to the text presented in a section. 

## Procedure

The first few sections have been cataloged in interactive mode, with the explicit instruction of going on to the end of a section.  However, no matter how strong-worded these instructions where, in the end the cataloging run came to an end, BKK claiming exhaustion or other expressions for running out of steam.  Since the startup of a cataloging session is quite expensive -- reading the instructions and basic sources, finding the place where the last session gave up etc. --  my intention was to minimize this startup cost.  As I learned more about the internal workings of Claude Code et.al., I realized that this was the wrong strategy:  As a session continues, the short-term memory (`context window`) fills up and in the end prevents the model from working, at which points it gives up.  There was also a visible degradation in quality, in one memorable case BKK followed my strong worded instructions to continue, but produced only stub entries without content, to fulfill the letter of its order.  

The solution then was to break down the effort in a sequence of sessions, in this case of about 50 texts, and then start a new session.  Once I had set up the harness in this way, the cataloging proceeded smoothly, until it ran out of tokens.  Even with the `Max 20x` plan, there is a weekly limit of token usage and also a moving 5 hours window.  With multiple sessions in parallel, BKK would quickly reach the limit in a 5 hour window, so I learned that I had to limit its work to just one session.  Depending on the length of a text, and the complexity of its history, the cataloging for one text will take up up to 15 minutes, and uses plenty of tokens.  In some cases, I burned through a weekly allowance in just 3 days and had to ask BKK to rest for a while. 

## Other lessons learned

Since one of the points of the whole project was to unearth problems in the text data, it was important to ask BKK to provide the necessary feedback.  In addition to writing into the text notes, BKK was also instructed to maintain a `TODO.md` file, where problems could be mentioned, for example missing or incomplete files that prevented the cataloging altogether.  The output in this file turned out to be very useful, here are some examples:

```md
- **KR1a0176** — Unidentified reconstruction. Content: fragments on 周易上經 (乾,
   etc.) and 周易繫辭下 (the 9-chapter division), all attributed to 孔頴達《正義》,
    with multiple "《正義》引褚氏莊氏" joint citations and a closing reference to
     "周氏莊氏並為九章". Best candidates from *Yùhán shānfáng jíyì shū* 易類: 周易褚
     氏講疏 (梁·褚仲都, 1 卷) — most likely given the joint 褚氏莊氏 citation
      pattern, since 褚氏 would not be cited as a separate source in his own
       work; OR 周易周氏義疏 (陳·周弘正). Catalog entry deferred pending firm
        attribution.

These are not in the official Kanripo catalog but exist in the local
 `/home/Shared/krp/KR1a/` snapshot. Future work: cross-reference against the
  actual 玉函山房輯佚書 text (e.g., the Waseda digital edition, ロ11_01236_0043,
   vols. 41–45 [上]) to confirm attribution for 0174 and 0176.

```


```md
- **KR3fa047** 幾何原本 (dynasty: 明, source: hxwd) — catalog meta lists this as
   a separate variant edition of 幾何原本 (cf. [[KR3f0047]], the WYG 6-juan Ricci-
   Xu Guangqi translation). Source marked "hxwd" (外部 database, likely HXWD or
    similar Chinese digital corpus). No `/home/Shared/krp/KR3f/KR3fa/KR3fa047/`
     directory exists (KR3fa subdirectory only runs to KR3fa039). No local 
     source files accessible; cannot catalog without text. Skipped.
```

Another lesson was that no matter how detailed the instructions, BKK would sometime deviate from them and do something different.  Since further cataloging usually took existing entries as an example to imitate, such mistakes could proliferate.  At one point, for example, BKK started to write Pinyin as *Yù-hán shān-fáng jí-yì shū* instead of the correct *Yùhán shānfáng jíyì shū*, e.g. there were unwanted extra hyphens in the Pinyin words.  A latter review process then was initiated to rectify this, but that was over eager and produced things like *SòngYuán* in cases where *Sòng-Yuán* was correct. 
