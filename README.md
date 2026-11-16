# Encyclopedia of the Great Quran (plugin)

This plugin connects Claude to the **Encyclopedia of the Great Quran** («موسوعة القرآن العظيم», https://www.kitab-allah.org), from the works of Dr. Ratib Al-Samman, prepared and edited by Ghiath AL SAMMANE. It bundles two things:

- the remote connector `quran-encyclopedia` (`https://mcp.kitab-allah.org/mcp`, read-only, no sign-in): seven tools to search the encyclopedia, read an entry, a verse, a root with three classical lexicons, a chapter of the author's works, and browse what exists;
- the skill `quran-encyclopedia`, which teaches the assistant which tool to use for which question, to quote verses only from the verified Tanzil text the tools return, to keep the author's analysis, the classical lexicons, the Quranic Arabic Corpus data and its own words apart, to answer in the user's language with Arabic headwords, and to say plainly when the encyclopedia has nothing.

What it does not do: it gives no religious rulings (fatwa) and holds no tafsir. The long study text of each entry stays on the website (free subscription there); the connector returns the author's short meaning, the comparison, the group, the root, the cited verses, the volume and page, and the page address.

## What the connector sends

Only the arguments of each tool call (words, verse numbers, a root) and the usual web request data such as the IP address. No account, no cookies, nothing stored. See https://mcp.kitab-allah.org/privacy.

## Credits

Quran text: Tanzil (CC BY 3.0), unchanged. English meaning: M. M. Pickthall (1930). Roots and lemmas: Quranic Arabic Corpus (corpus.quran.com, GNU GPL). Lexicons: al-Raghib and Ibn Faris (OpenITI, CC BY-NC-SA 4.0), Lane (Perseus Digital Library). English and French versions of entry fields are AI-assisted and unreviewed.

## Install

In Claude Code: `/plugin install kitab-allah-encyclopedia --marketplace Kitab-allah/kitab-allah-encyclopedia` (older versions: `/plugin marketplace add Kitab-allah/kitab-allah-encyclopedia`, then `/plugin install kitab-allah-encyclopedia`). In claude.ai or Claude Desktop you can also add the connector alone: Settings, Connectors, Add custom connector, `https://mcp.kitab-allah.org/mcp`.

## Licence

Copyright (C) 2026 Ghiath AL SAMMANE.

The files of this plugin (the manifest, the connector configuration, the skill text and the logo) are free software under the GNU Affero General Public License, version 3 only (AGPL-3.0-only); see [LICENSE](LICENSE).

That licence does not cover the encyclopedia itself. The content the connector returns (the works of Dr. Ratib Al-Samman, © Ghiath AL SAMMANE, all rights reserved) is governed by the terms at https://mcp.kitab-allah.org/terms, and each outside source keeps its own licence (see Credits).

This is an independent project. It is not made, approved or endorsed by Anthropic or OpenAI.
