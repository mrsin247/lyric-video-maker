# Lyric Video Maker

Drop in a song file (MP3, WAV, or M4A) and it pulls the lyrics out with timestamps, so you can preview them as a vertical lyric video. No mic, no upload — it reads the audio file itself. You can fix any misheard lines, style the preview, and compare two songs' lyrics side by side.

Live site: https://mrsin247.github.io/lyric-video-maker/

## v2.1 — the phone-crash fix (2026-11-05)

v2.0 packed the whole Whisper model into ONE 356 MB HTML file. Opening it put the entire file in the DOM (~1.6 GB of Chrome memory just to hold the parsed page on desktop) and Chrome on a Pixel 8a ran out of memory and crashed ("something went wrong on our end") before the tool could run.

v2.1 is the same tool and the same Whisper small.en model (90.6% / 93.4% of words right on real songs, vs 32% / 18% for the old Vosk build) — but the page itself is small again (~65 KB) and the model loads as files from this site, only when you tap the Extract button. Voice-activity detection stays OFF, same 30-second windows, same steps and compare flow.

**First run:** tapping Extract downloads the model files once — about 253 MB total, so use WiFi. After that your browser caches them and lyric pulls work offline.

## Files on this site

- `index.html` — the tool (~65 KB)
- `assets/models/Xenova/whisper-small.en/onnx/encoder_model_quantized.onnx` — 92,324,819 bytes
- `assets/models/Xenova/whisper-small.en/onnx/decoder_model_merged_quantized.onnx.part0` — 78,390,090 bytes
- `assets/models/Xenova/whisper-small.en/onnx/decoder_model_merged_quantized.onnx.part1` — 78,390,091 bytes
- The two decoder halves join back into the 156,780,181-byte decoder in the browser (see the "v2.1 decoder loader" script in `index.html`). The decoder ships split because GitHub refuses any single file over 100 MB in a push.
- `assets/models/Xenova/whisper-small.en/` — tokenizer + config files, 3,645,745 bytes total
- `assets/ort/` — ONNX Runtime Web wasm files; `assets/transformers.min.js` — transformers.js 2.17.2; `assets/jsmediatags.min.js`

## History

- v1.0 — Vosk small model, single 62.8 MB file. Worked on desktop, garbled sung lyrics (18% / 32% right), stalled on phones.
- v2.0 — Whisper small.en, single 356 MB file. Accurate, but the giant page crashed Chrome on phones. Archived, not deleted: the file is `lyric-video-maker-v2.0.html` (356,241,305 bytes) in git history.
- v2.1 — Whisper small.en, small page + model files from this site. The fix for the v2.0 phone crash.
