# learn-audio

Narration audio for the course site [spareilleux.github.io/learn](https://spareilleux.github.io/learn/), served by GitHub Pages at
`https://spareilleux.github.io/learn-audio/<locale>/<page slug>.mp3` (for example
[`en/rust-for-csharp-java/03-ownership-and-moves.mp3`](https://spareilleux.github.io/learn-audio/en/rust-for-csharp-java/03-ownership-and-moves.mp3)).
The site's page player reads `src/data/audio-manifest.json` in the [learn](https://github.com/spareilleux/learn) repository.

The audio is **synthetic speech**: every file is generated from the text of the matching page, not recorded by a person.
Code blocks, tables and exercise solutions are not read.

## How it is made

1. `node scripts/tts/extract.mjs` (in the learn repository) turns each page into narration chunks.
2. `scripts/tts/generate.py` synthesises the chunks locally on a GPU, caches them by (engine, voice, chunk hash),
   joins them with short pauses and encodes mono 48 kbps MP3 (24 kHz, loudness-normalised to −16 LUFS) with ffmpeg.
   Only pages whose text changed are regenerated.

## Engine, voices and licences

| | Used for | Licence |
|---|---|---|
| [Kyutai TTS 1.6B en_fr](https://huggingface.co/kyutai/tts-1.6b-en_fr) (`kyutai/tts-1.6b-en_fr`), by [Kyutai](https://kyutai.org/) | all files | model weights [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Voice `unmute-prod-website/developer-1.mp3` from [kyutai/tts-voices](https://huggingface.co/kyutai/tts-voices) | English (`en/`) | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) |
| Voice `unmute-prod-website/developpeuse-3.wav` from [kyutai/tts-voices](https://huggingface.co/kyutai/tts-voices) | French (`fr/`) | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) |

Attribution: *Narration generated with Kyutai TTS 1.6B (kyutai/tts-1.6b-en_fr) by Kyutai, licensed CC BY 4.0,
using CC0 voices from kyutai/tts-voices.* The text read aloud comes from the learn course pages; see that repository for its licence.

## Layout

```text
.nojekyll                               serve files as-is
en/<course>.mp3                         course home page
en/<course>/<page>.mp3                  lessons and journal
fr/…                                    French mirror, same slugs
```
