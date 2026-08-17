# Lead avatars for the demo chat

The phone in the hero rotates four scripted conversations. Each one shows the
lead's photo in the chat header. Drop the four files here:

```
naledi.jpg    james.jpg    greg.jpg    michelle.jpg
```

Until a file exists the header falls back to a neutral figure glyph, so a
missing or slow-loading photo degrades quietly rather than leaving an empty
circle. The image is preloaded and only swapped in once it has decoded.

## Specification

| | |
|---|---|
| Shape | square, 1:1 — it is masked to a circle at 36px |
| Size | 256x256 minimum (36px at 3x device pixel ratio, with headroom) |
| Format | JPG, or WebP if you also keep a JPG fallback |
| Weight | under 40KB each; they load on every page view |
| Crop | head and shoulders, face centred, eyes around the upper third |
| Background | plain and uncluttered — it is only 36px on screen |

Faces read as roughly 30px across in the header. Busy backgrounds, hats,
sunglasses and wide crops all turn to mush at that size. Tight, well-lit,
front-facing portraits are the only thing that survives.

## These must not be real people

They are AI-generated faces, deliberately. The conversations are written
scripts, so a real person's likeness beside a fabricated statement is both a
licensing problem (free stock licences do not grant likeness rights for
implied endorsement) and a credibility one.

Do not replace these with stock photography, scraped images, or photos of
staff or clients, unless the conversations become real transcripts published
with those people's consent.

## Keeping it honest

Because the faces read as real clients, the label above the phone should not
overclaim. If it ever says these are live or real conversations rather than a
demonstration, the wording needs revisiting.
