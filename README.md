# The Wayfarer's Debt — Campaign Archive

A self-updating campaign wiki for our Star Wars FFG game, generated from the
session recordings.

**The site:** https://ausrabbit.github.io/wayfarers-debt-archive/

## How it works

```
Craig recording  ->  faster-whisper  ->  transcript.txt  ->  content/*.md  ->  docs/index.html
```

`content/` is an [Obsidian](https://obsidian.md) vault. Every entity —
character, place, item, faction — is one markdown file with `[[wikilinks]]`
to the others. Open the folder in Obsidian and you get local editing,
backlinks and the graph view for free.

`build.py` turns that vault into a single self-contained HTML page in
`docs/`, which GitHub Pages serves. Standard library only, no dependencies:

```bash
python3 build.py
```

It fails loudly on any `[[wikilink]]` that points at a page which doesn't
exist, and renders those links in red on the site, so a typo can't silently
become a missing page.

## Adding a session

1. Record with Craig, download the **multi-track** version.
2. Transcribe each speaker track and merge by timestamp.
3. Write the recap and any new entity pages into `content/`.
4. `python3 build.py`, then commit and push. The site rebuilds itself.

## Frontmatter

```yaml
---
title: "Commander Elira Dane"
group: "Adversaries"        # sidebar section
type: character             # character | place | thing | faction | doc
conf: lo                    # hi | mid | lo  — how sure we are of the name
aliases: ["Ilya Dayne", "Elenia Dane", "Lyra"]
dek: "One line under the title."
---
```

`aliases` is the important one. Whisper mangles invented names differently
every time it hears them, so each alias is a spelling the transcript actually
produced. Listing them here means a `[[Lyra]]` written anywhere still resolves
to the right page — and the same list, dumped to a `names.txt`, makes the next
session's transcription more accurate at source.

## What's deliberately not here

GM notes. Plot that the players haven't discovered, mechanical admin, and
anything said at the table that wasn't in character. This repository is
public; that material lives locally and `.gitignore` keeps `gm/` out.
