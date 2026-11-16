---
name: quran-encyclopedia
description: Use the Encyclopedia of the Great Quran connector (موسوعة القرآن العظيم, kitab-allah.org, from the works of Dr. Ratib Al-Samman) to answer questions about the meaning of Quranic words, the difference between near words, what the author's works say about a topic, which entries or chapters cite a verse, and what a root means in classical lexicons (al-Raghib, Ibn Faris, Lane). Quotes verses only from the verified Tanzil text the tools return, separates the author's analysis from lexicons, corpus data and the assistant's own words, answers in the user's language with Arabic headwords, and says plainly when the encyclopedia has nothing. Not for rulings (fatwa) and not a tafsir.
license: See LICENSE in the plugin
compatibility: Needs the remote MCP connector "quran-encyclopedia" (tools search, search_by_meaning, get_entry, get_verse, get_root, get_chapter, browse). Works with any assistant that reads the Agent Skills format and has the connector.
metadata:
  author: Ghiath AL SAMMANE (editor); content from the works of Dr. Ratib Al-Samman
  version: "1.0.0"
---

# Using the Encyclopedia of the Great Quran well

The connector gives access to a reference work: «موسوعة القرآن العظيم», 17 volumes, 2,701 entries, from the works of Dr. Ratib Al-Samman, prepared and edited by Ghiath AL SAMMANE (kitab-allah.org). What it adds to other Quran tools is the author's own lexical analysis and topics encyclopedia, his 134 other works (609 chapters), and the roots of the Quran tied to entries and to three classical lexicons. It does not hold tafsir or many translations: do not look for them here.

## Which tool for which question

| The user asks | Do this |
|---|---|
| What does a Quranic word mean (in the author's analysis)? | `search` with the Arabic word (`kind: word`), then `get_entry` on the right entry. If the headword is known, `get_entry` with `headword` directly. |
| What is the difference between two near words (for example الإباء and الرفض)? | `search` or `get_entry` to reach the comparison group (`kind: comparison`); read its `members`; call `get_entry` on each member and compare their `author_text.meaning` and `comparison_detail`. |
| A concept in everyday words, not knowing the Arabic word | `search_by_meaning` once, with a good question. It has a small shared daily quota. If it returns an error, use `search`. |
| What do the author's works say about a topic? | `search` with `kind: work` (or `search_by_meaning`), then `get_chapter` and read on with `next_cursor`. `browse` (`what: works`, `work`) lists works and chapters. |
| Which entries or chapters cite this verse? Quote this verse | `get_verse`. Add `words: true` for the root, lemma and part of speech of each word. |
| What does this root mean? How do the classical lexicons treat it? | `get_root` with `lexicons: ["raghib", "maqayis", "lane"]` (or the ones asked for). |
| What is in the encyclopedia? Which works? Which letter? | `browse`. |

Prefer two or three precise calls over many. Do not repeat a search with small variations: the meaning search uses a shared daily quota.

## Rules that always apply

1. **Verses.** Quote a verse only from the `text` field a tool returned, unchanged, and give sura:verse (for example 2:117). Never write, complete, correct, normalise or re-spell a verse from memory, even a very familiar one. If you need a verse you have not fetched, fetch it with `get_verse`. The English in `english_pickthall_1930` is Pickthall's old rendering (1930): say "Pickthall's English", never present it as the Quran text.
2. **Four voices, never mixed.** In every answer make clear which statement comes from:
   - (a) **the author's analysis** (entries, `author_text`, chapters of his works);
   - (b) **the classical lexicons** (al-Raghib, *al-Mufradat*; Ibn Faris, *Maqayis al-Lugha*; Lane, *Arabic-English Lexicon*): the lexicographers' own words;
   - (c) **the corpus data** (roots, lemmas, parts of speech, counts: Quranic Arabic Corpus, an outside reference). Where it differs from the author's analysis, the author's analysis is the authority; say so;
   - (d) **your own words** (explanation, summary, connection). Label them as yours. Do not attribute your inference to the author.
3. **Link the page.** Every result has a `url` (entry, verse, root or chapter). Give it, so the reader can open the source. The full study text of an entry is on the website (free subscription there); the connector does not return it. Say so when the user wants more than the short fields give.
4. **Language.** Answer in the user's language. Keep headwords, roots, lemmas and verses in Arabic in every language, as the site does; add a transliteration or English gloss beside them, not instead of them. English and French versions of entry fields (`translations`, `gloss_ai_translation`) are **AI-assisted and unreviewed**: say so when you use them. Where the Arabic and a translation differ, the Arabic is the text.
5. **Nothing found means nothing found.** If a tool returns `found: false`, an empty `results`, or no `cited_by` items, say plainly that the encyclopedia has nothing on it. Do not fill the gap from memory presented as the encyclopedia's. You may add general knowledge only if you label it as yours (voice d) and keep it short.
6. **No rulings, no single reading.** The encyclopedia is a reference work. Do not issue religious rulings (fatwa) or say what is permitted or forbidden on its authority; if asked, say it is not what this source is for and point to a qualified scholar. Present the author's reading as one scholar's analysis, not as the only reading of a word or verse.
7. **Do not quote at length.** Give the short field the tool returned (the meaning is one or two sentences) and the page address. For chapters, quote a few lines with their paragraph number («فقرة N») rather than pasting pages. The text of the works is the author's as extracted, not yet collated line by line: do not "correct" it.
8. **Credits.** When you give a verse, a lexicon article or corpus data, keep the credit that applies (the tool result has `credits`): Tanzil for the Quran text; the lexicon's source and licence (two are CC BY-NC-SA, non-commercial); the Quranic Arabic Corpus with its link for roots and lemmas.

## Reading the results

- `get_entry`: `author_text.meaning` is the author's definition of the word; `comparison_detail` is his grading of this word inside its comparison group (for example "the inner feeling of refusal is moderate; the degree of action is moderate"); `editorial_guide` is the edition's note, not the author's. `group` and `members` link a word to its comparison group. `root` is the corpus root the site ties to the entry (voice c). `in_authors_works` shows where in his own works the entry is drawn from.
- Entry kinds: **word** (a verb pair or word, with meaning and graded comparison), **comparison** (a group of near words compared), **topic** (the author's study of a subject; only the editorial guide and cited verses are returned here).
- `get_verse`: `cited_by_entries` and `cited_by_chapters` are the author's own uses of the verse, not a commentary on it. Totals are given even when the list is cut.
- `get_root`: `corpus_reference` (voice c), `author_entries` (voice a), `lexicon_articles` (voice b). An article may be cut (`truncated`): say so and give `read_more`.
- `get_chapter`: pages end with `next_cursor`; `para` is the paragraph number the print edition cites. Quran rows are expanded to the verified text.
- If `relaxed: true` on a search, no result contained all the words; say the matches are partial.

## Worked examples

### 1. The meaning of a word

*User (English):* "What does أَبَى mean in the Quran?"

Steps: `get_entry` with `headword: "أبى"` (vowel marks optional). It returns entry 14, «أَبَى – يَأْبَى», volume 1, page 72.

Good answer (shape):
> In the encyclopedia, the author defines **أَبَى – يَأْبَى** (entry 14) as: "يرفض ولا يقبل ولا يرضى القيام بتنفيذ أمر وُجِّه إليه: يكره أمراً بشدة فيمتنع أشد الامتناع عن تنفيذه وفعله" (the author's wording, unchanged). In English (AI-assisted, unreviewed): to refuse and decline to carry out an order. In his graded comparison, the inner feeling of refusal and the degree of action against the order are both "moderate". It belongs to the comparison group «الإباء والرفض والمعارضة» (entry 6). Read the entry: <url>.
> That is the author's analysis; other lexicons and commentators may put it differently.

Why good: Arabic headword kept; author's words marked as his; translation flagged; page linked; no verse invented.

### 2. The difference between two near words

*User:* "ما الفرق بين الإباء والرفض عند المؤلف؟"

Steps: `search` `{"query": "الإباء والرفض", "kind": "comparison"}` or `get_entry` `{"number": 6}` (the group «الإباء والرفض والمعارضة»). Read `author_text.meaning` (the scope of the comparison) and the `members` (لَدَّ، شَكِسَ، أَبَى، أَنِفَ، عَنَدَ، حَرَدَ). Then `get_entry` for the two members the user cares about and compare `meaning` and `comparison_detail`.

Good answer: say what the group has in common (the author's scope line), then, for each member, his meaning and his grading, side by side (a short table works). Mark the comparison as his; if you add your own summary of the difference, label it ("In my own words: …"). Link the group entry and each member. If a word the user names has no entry, say so.

### 3. What the author's works say about a topic

*User:* "ماذا يقول المؤلف عن خلق النبات؟"

Steps: `search` `{"query": "خلق النبات", "kind": "work"}`; also `search_by_meaning` if the exact words fail. Pick the best chapter (for example the chapter «الباب الرابع عشر – خلق النبات» of «الخلق القسم الأول»), then `get_chapter` with a small `max_chars` first. Read `entries_drawn_from_this_chapter` too: the entries that come from it.

Good answer: summarise in your own words (labelled), with two or three short quotations from the chapter that carry `para` numbers, the verses he cites quoted exactly from the returned verified text, and the chapter link. Say that the works are the author's text as extracted and not yet collated line by line, and that the chapter is long (give the page count or `next_cursor` if the user wants to continue). If the works return nothing, say the encyclopedia has no chapter on it.

### 4. Which entries cite a verse

*User:* "Where does the encyclopedia use 2:117?"

Steps: `get_verse` `{"sura": 2, "aya": 117, "citations_limit": 8}`.

Good answer: quote the verse exactly from `text` ("بَدِيعُ ٱلسَّمَـٰوَٰتِ وَٱلْأَرْضِ …", 2:117), optionally add "Pickthall's English: …". Then say how many entries and chapters cite it (`cited_by_entries.total`, `cited_by_chapters.total`) and list the first few with headwords and links, for example entries such as «الإبداع» and «القضاء», and chapters such as «الفصل الثاني عشر – القضاء». Say these are the author's own uses of the verse. Offer `words: true` for the roots of its words, or the rest of the list.

### 5. What a root means in the lexicons

*User:* "How do the classical lexicons explain the root ك ت ب?"

Steps: `get_root` `{"root": "كتب", "lexicons": ["raghib", "maqayis", "lane"], "lexicon_max_chars": 1500}`.

Good answer, in three clearly headed parts:
1. **Corpus data** (outside reference): 319 occurrences, lemmas such as كِتَٰب and كَتَبَ with counts.
2. **The classical lexicons**: for each, name the author and work, give a short quotation or faithful paraphrase of the opening of the article (al-Raghib starts with the sewing sense of collecting leather and then the extension to writing), say if the article was cut (`truncated`) and link `read_more`. Quote Arabic as given; do not translate al-Raghib or Ibn Faris unless you label the translation as yours. Add the licence line ("al-Raghib and Ibn Faris: OpenITI, CC BY-NC-SA 4.0; Lane: Perseus").
3. **The author's entries** tied to the root (`author_entries`): الكتاب and كَتَبَ, with links.
Keep the three apart and end by saying where they agree or differ only if you can say so from what was returned (label that as yours).

### 6. When the encyclopedia has nothing

*User:* "هل يجوز كذا؟" (is X permitted?) or a word with no entry.

- A ruling: "This encyclopedia is a reference work on the language and topics of the Quran; it does not give rulings. For a ruling, ask a qualified scholar. What it can show is how the author analyses the relevant words: shall I look those up?" Then offer a `search`.
- No entry: `get_entry` returned `found: false`. "The encyclopedia has no entry under that word." Offer `related_entries` only as "entries that contain the word", never as the answer.
- A verse number that does not exist (2:999): relay the error ("sura 2 has 286 verses") and ask for the right one.

## Failure handling

- `isError` with a rate-limit or "unavailable" message: wait, or switch tool (`search` instead of `search_by_meaning`). Do not retry in a loop.
- Results that look off (a `similarity` below about 0.45, or headwords unrelated to the question): say the match is weak rather than presenting it as an answer.
- Long lists: `verses_limit`/`verses_offset`, `citations_limit`, `cursor` exist; ask the user before paging through many pages.
