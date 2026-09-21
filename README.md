# Salvage Rights — Campaign Wiki

A self-updating campaign wiki for our SWRPG game, generated from the session
recordings.

## How it works

```
Craig recording  ->  faster-whisper  ->  transcript.txt  ->  content/*.md  ->  docs/index.html
```

`content/` is an [Obsidian](https://obsidian.md) vault. Every entity —
character, place, item, faction — is one markdown file with `[[wikilinks]]`
to the others. Open the folder in Obsidian and you get local editing,
backlinks and the graph view for free.

`build.py` turns that vault into a single self-contained HTML page in
`docs/`. Standard library only, no dependencies:

```bash
python build.py
```

It fails loudly on any `[[wikilink]]` that points at a page which doesn't
exist, and renders those links in red on the page, so a typo can't silently
become a missing page.

**The generated site is committed to `docs/`.** If you edit anything in
`content/`, run `build.py` before committing or the published page won't
change.

## Publishing

The site is one static file with no build step, so any static host works.
GitHub Pages serves it directly from this repo:

> Settings → Pages → Source: **Deploy from a branch** → Branch **main**,
> folder **/docs** → Save

That publishes to `https://ausrabbit.github.io/salvage_rights_wiki/`.
The first build takes a minute or two; a 404 immediately afterwards is normal.

## Adding a session

1. Record with Craig, download the **multi-track** version.
2. Transcribe each speaker track and merge by timestamp.
3. Write the recap and any new entity pages into `content/`.
4. `python build.py`, then commit and push.

## Frontmatter

```yaml
---
title: "Commander Ilyra Dane"
group: "Adversaries"        # sidebar section
type: character             # character | place | thing | faction | doc
conf: hi                    # hi | mid | lo  — how confident we are
short: "Ilyra Dane"         # optional: the form people actually say
aliases: ["Elira Dane", "Ilya Dayne", "Lyra"]
dek: "One line under the title."
---
```

`aliases` is the important one. Whisper mangles invented names differently
every time it hears them, so each alias is a spelling a transcript actually
produced. Listing them means a `[[Lyra]]` written anywhere still resolves to
the right page.

## The two name files

`build.py` regenerates both on every run. They do opposite jobs and must not
be mixed up:

- **`names.txt`** — canonical spellings only. Prompted *into* the
  transcriber so it biases towards the right words. Never put a misspelling
  here; that teaches the model the misspelling is real.
- **`corrections.json`** — alias → spoken form. Applied to the transcript
  *after* recognition, to repair what the prompt didn't prevent. Corrections
  target the spoken name (`Brack` → `Brak`), not the formal title, because a
  transcript should read the way people talk.

## What's deliberately not here

GM notes. Plot the players haven't discovered, mechanical admin, and anything
said at the table that wasn't in character. This repository is public, so that
material lives locally in `gm/`, which `.gitignore` excludes.
