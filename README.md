# learn-audio

**English** narration for the course site [spareilleux.github.io/learn](https://spareilleux.github.io/learn/), served by GitHub Pages at
`https://spareilleux.github.io/learn-audio/en/<page slug>.mp3` (for example
[`en/rust-for-csharp-java/03-ownership-and-moves.mp3`](https://spareilleux.github.io/learn-audio/en/rust-for-csharp-java/03-ownership-and-moves.mp3)).
The site's page player reads `src/data/audio-manifest.json` in the [learn](https://github.com/spareilleux/learn) repository.

There is one repository per locale — this one for English, [learn-audio-fr](https://github.com/spareilleux/learn-audio-fr) for
French and [learn-audio-es](https://github.com/spareilleux/learn-audio-es) for Spanish — because a published GitHub Pages site
may be no larger than 1 GB and the whole site narrated is about 2.5 GB.

The audio is **synthetic speech**: every file is generated from the text of the matching page, not recorded by a person.
Code blocks, tables and exercise solutions are not read.

## How it is made

1. `node scripts/tts/extract.mjs` (in the learn repository) turns each page into narration chunks.
2. `scripts/tts/generate.py` synthesises the chunks locally on a GPU, caches them by (engine, voice, lexicon, chunk hash),
   joins them with short pauses and encodes mono 24 kHz MP3 (loudness-normalised to −16 LUFS) with ffmpeg.
   Only pages whose text changed are regenerated.

The first courses were encoded at 48 kbps and the later ones at 32 kbps: at 48 kbps the whole site did not fit under the
1 GB a repository's Pages site may publish.

## Engine, voices and licences

| | Used for | Licence |
|---|---|---|
| [Kyutai TTS 1.6B en_fr](https://huggingface.co/kyutai/tts-1.6b-en_fr) (`kyutai/tts-1.6b-en_fr`), by [Kyutai](https://kyutai.org/) | English (`en/`) | model weights [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Voice `alba-mackenna/a-moment-by.wav` from [kyutai/tts-voices](https://huggingface.co/kyutai/tts-voices), voice-acted by Alba MacKenna | English (`en/`) | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |

Attribution: *English narration generated with Kyutai TTS 1.6B (kyutai/tts-1.6b-en_fr) by Kyutai, licensed CC BY 4.0;
English voice by Alba MacKenna (kyutai/tts-voices, CC BY 4.0).*
The text read aloud comes from the learn course pages; see that repository for its licence.
The other two locales use the same pipeline with their own engine and voice; see their repositories.

## Layout

```text
.nojekyll                               serve files as-is
en/<course>.mp3                         course home page
en/<course>/<page>.mp3                  lessons and journal
```
