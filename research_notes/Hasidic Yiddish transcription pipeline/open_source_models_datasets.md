# Open-source / open-weight Yiddish ASR: models, checkpoints, datasets, precedents and orthography (status as of 2026-11-27)

Evidence labels used in every bullet: **[fetched]** = I read the primary page or file directly in this session. **[snippet]** = taken from a search-engine summary of the page, because the host was blocked from this session (huggingface.co, arxiv.org, aclanthology.org, lrec-conf.org, isca-archive.org, cdn.openai.com, commonvoice.mozilla.org, docs.mistral.ai and chezky-blog.pages.dev could not be opened). Treat snippets as lower confidence. **[aggregator]** = taken from the "Torah & Yiddish AI directory" data file (pinta-partners, generated 2026-10-21, https://github.com/pinta-partners/torah-yiddish-ai-directory), which I fetched and which cites its own primary sources (the primary URL it cites is given). That directory is maintained by Chezky Kohn, who also builds some of the listed tools (Phonikud-yi, yiddish-audio-alignment), so it is not fully independent. Hugging Face download counts and likes could not be collected because huggingface.co was blocked.

## 1. Base open models: Yiddish ability, published error rates, timestamps

### Takeaway
Only two open model families really support Yiddish: OpenAI Whisper (it has a Yiddish language token, but is close to useless zero-shot on Hasidic speech: 92.9–110.4 "wrong words per 100" in the only Hasidic benchmark found) and Meta Omnilingual ASR (`ydd_Hebr`, the only officially published open Yiddish number: CER 15.8 for the 7B LLM-ASR model, trained on 12.8 hours of Yiddish). MMS (ASR), SeamlessM4T v2, NVIDIA Canary/Parakeet, IBM Granite Speech, Qwen3-ASR, Kyutai STT and Cohere Transcribe do not list Yiddish. Whisper is the most practical base because of its timestamp and long-form tooling. Omnilingual needs clips under 40 s (except its "Unlimited" variants) and does not document timestamps.

### Cited Findings
**OpenAI Whisper (large-v2 / large-v3 / large-v3-turbo)**
- Whisper's tokenizer includes `"yi": "yiddish"` (and `"he": "hebrew"`), so Yiddish can be forced as the decoding language. [fetched] — [openai/whisper tokenizer.py](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)
- Whisper's original training data: 680,000 h of audio. About 117,000 h of it is non-English audio with same-language transcripts, covering 98 languages per the model card. [fetched] — [Whisper model card](https://github.com/openai/whisper/blob/main/model-card.md). (The paper says 96 other languages, per a [snippet] of the [Whisper paper](https://cdn.openai.com/papers/whisper.pdf).)
- OpenAI's published per-language FLEURS WER chart has Hebrew but no Yiddish entry. I searched the SVG for "Yiddish" (0 matches) and "Hebrew" (2 matches). [fetched] — [language-breakdown.svg](https://github.com/openai/whisper/blob/main/language-breakdown.svg)
- The only Hasidic/heimish zero-shot numbers found come from the 3Yiddish benchmark. It covers 21 tools on 25 takes of free heimish speech (1,191 words, one speaker). The metric is "wrong words per 100" (WER-like, and it can exceed 100). Results: Whisper large-v3 (plain) 92.9; Whisper medium 95.9; Whisper small 97.1; Whisper turbo 110.4. Stock Whisper got all 13 loshn-koydesh words and all 14 English words wrong. The benchmark describes the plain-Whisper output as "Garble in Yiddish letters; one take came out in English" (medium) and notes that plain Whisper models "spell by rough sound, write other languages, or repeat themselves". [fetched] — [3Yiddish BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
- On Chabad farbrengen audio, the starting point was ivrit.ai's Yiddish fine-tune of large-v3, not stock Whisper. The "untouched Yiddish Whisper large-v3" scored about 0.57 whole-recording WER on the Rebbe's farbrengens. [fetched] — [golanbenoni/rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)

**Meta Omnilingual ASR (released Nov 2025; v2 checkpoints Dec 2025)**
- Code and models are Apache 2.0, and the dataset is CC-BY-4.0. Families: CTC 300M/1B/3B/7B, LLM-ASR 300M–7B, a 7B zero-shot model, and in the Dec 2025 update `_v2` and `omniASR_LLM_Unlimited_*_v2` variants. "Only audio files shorter than 40 seconds are accepted for inference on CTC and LLM model suites"; the Unlimited variants "support decoding on unlimited audio length". The README does not mention timestamps. [fetched] — [facebookresearch/omnilingual-asr](https://github.com/facebookresearch/omnilingual-asr)
- `ydd_Hebr` is in the model's language-ID list. [fetched] — [lang_ids.py](https://github.com/facebookresearch/omnilingual-asr/blob/main/src/omnilingual_asr/models/wav2vec2_llama/lang_ids.py)
- Meta's per-language table for the 7B LLM-ASR lists `ydd_Hebr`: Training Hours 12.8, CER 15.8 (Meta's held-out test). For comparison, `heb_Hebr` has 155.5 h and CER 6.7. [fetched] — [per_language_results_table_7B_llm_asr.csv](https://github.com/facebookresearch/omnilingual-asr/blob/main/per_language_results_table_7B_llm_asr.csv)
- Omnilingual ASR CTC 300M zero-shot on heimish speech scored 61.3 wrong words per 100: "Recognisable Yiddish, spelled by rough sound and run together". [fetched] — [3Yiddish BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
- Paper: "Omnilingual ASR: Open-Source Multilingual Speech Recognition for 1600+ Languages", arXiv 2511.09690. It says the 7B LLM-ASR has CER below 10 for 78% of languages. [snippet] — [arXiv 2511.09690](https://arxiv.org/pdf/2511.09690). One blog gives the code as "yid_Hebr", which conflicts with the official `ydd_Hebr`. [snippet]
- A third-party Hebrew model card (VoicenterTeam/whisper-heb-v7-multi) reportedly lists a Yiddish score of 0.347 on an "Omnilingual ydd read test set", against 1.196 for whisper-large-v3-turbo. The metric is unclear. A second search could not find it. [snippet, unverified] — [VoicenterTeam/whisper-heb-v7-multi](https://huggingface.co/VoicenterTeam/whisper-heb-v7-multi)

**Meta MMS**
- MMS-1B-all covers 1,162 languages, trained on MMS-lab + FLEURS + CV + VoxPopuli + MLS. MMS also ships GPU forced-alignment tooling for long audio and "a multilingual alignment model trained on 31K hours of data in 1,130 languages". [fetched] — [fairseq MMS README](https://github.com/facebookresearch/fairseq/blob/main/examples/mms/README.md)
- The directory's benchmark row says "Meta MMS (ASR): No (Yiddish = language ID only)", citing MMS's language-coverage page. [aggregator] — [MMS coverage page](https://dl.fbaipublicfiles.com/mms/misc/language_coverage_mms.html). The MMS LID model mms-lid-126 includes `yid`. [snippet] — [facebook/mms-lid-126](https://huggingface.co/facebook/mms-lid-126/blob/main/README.md)

**Other open multilingual models (none list Yiddish)**
- SeamlessM4T v2: the language table lists `heb` (Hebrew; speech and text input). Searching for "yiddish", "ydd" and "yid" found nothing. [fetched] — [seamless_communication docs/m4t/README.md](https://github.com/facebookresearch/seamless_communication/blob/main/docs/m4t/README.md)
- NVIDIA Canary-1B-v2 and Parakeet-TDT-0.6B-v3 both support "25 European languages" (EU official languages plus Russian and Ukrainian). [snippet] — [arXiv 2509.14128](https://arxiv.org/abs/2509.14128v2); [NVIDIA blog](https://blogs.nvidia.com/blog/speech-ai-dataset-models)
- Mistral Voxtral: Voxtral Mini Transcribe V2 (Feb 2026) supports 13 languages: en, zh, hi, es, ar, fr, pt, ru, de, ja, ko, it, nl. The Voxtral 3 file-transcription models (`voxtral-mini/small-transcribe-3-0`) cover 53 languages per the English docs and 50 per the French docs and announcement. I could not retrieve the full list, so Hebrew and Yiddish are unverified. [snippet] — [Mistral docs: languages](https://docs.mistral.ai/studio/audio/specs-and-limits/languages); [Mistral speech_to_text docs](https://docs.mistral.ai/studio/audio/speech_to_text); [Voxtral 3 announcement](https://mistral.ai/news/voxtral-3/)
- IBM Granite Speech 3.3.2: English, French, German, Spanish, Portuguese. [fetched] — [ibm-granite/granite-speech-models](https://github.com/ibm-granite/granite-speech-models)
- Qwen3-ASR (0.6B/1.7B, released 2026-01-29): 30 languages plus 22 Chinese dialects. Neither Yiddish nor Hebrew is listed. Qwen3-ForcedAligner-0.6B predicts timestamps "within up to 5 minutes of speech in 11 languages" (no Yiddish). [fetched] — [QwenLM/Qwen3-ASR](https://github.com/QwenLM/Qwen3-ASR)
- Kyutai STT: `stt-1b-en_fr` (English/French) and `stt-2.6b-en`. Both "return word-level timestamps". [fetched] — [kyutai-labs/delayed-streams-modeling](https://github.com/kyutai-labs/delayed-streams-modeling)
- Cohere Transcribe (`cohere-transcribe-03-2026`, March 2026, 2B parameters, Apache 2.0): 14 languages (en, fr, de, it, es, pt, el, nl, pl, zh, ja, ko, vi, ar), no Yiddish or Hebrew. An Arabic fine-tune followed on 2026-07-07. [snippet] — [HF blog](https://huggingface.co/blog/CohereLabs/cohere-transcribe-03-2026-release); [Cohere docs](https://docs.cohere.com/docs/transcribe)

**Timestamps, as documented**
- Whisper-based Yiddish pipelines produce `.txt`, `.srt` and `.json` per recording. The Rebbe project uses word-level times from a Yiddish Whisper to align unaligned days, but "production decoding uses no timestamp tokens". [fetched] — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
- ivrit.ai's Yiddish WhatsApp training set "encoded timestamps along with previous text". [snippet] — [crowd-whatsapp-yi-whisper-training](https://huggingface.co/datasets/ivrit-ai/crowd-whatsapp-yi-whisper-training)
- Kyutai: word-level timestamps [fetched]. Qwen3: timestamps via a separate forced aligner, 11 languages [fetched]. Omnilingual: none documented [fetched]. MMS: a separate forced-alignment model [fetched]. Sources are above.

### Inferences
- For Hasidic Yiddish, only Whisper-family and Omnilingual checkpoints are realistic open starting points. Every other major 2025–2026 open ASR release leaves Yiddish out, and most leave out Hebrew too.
- Meta's CER 15.8 for `ydd_Hebr` comes from Meta's own held-out test, built on only 12.8 h of training data of unspecified dialect. It should not be read as Hasidic-shiur performance. The zero-shot CTC-300M result on heimish speech (61.3 wrong words per 100) is the more relevant signal.
- Whisper's ecosystem (faster-whisper/CTranslate2, whisper.cpp, stable-ts, segment and word timestamps) makes it the lowest-friction base for timestamped shiur transcripts. Omnilingual needs chunking under 40 s and a separate alignment step.
- MMS's language-agnostic forced-alignment model, which works on romanized text, could probably time-align Yiddish transcripts from a CTC or Omnilingual system. I did not verify this for Yiddish.

### Gaps
- **Whisper's Yiddish training hours could not be verified.** A garbled search extraction of the Whisper paper's training-statistics table put "Yiddish" next to "0.4", but the search summary itself said the label and number may belong to different rows. I could not open the paper (cdn.openai.com and arxiv.org were blocked), so treat "~0.4 h" as unconfirmed. I found no per-language breakdown for large-v3 or turbo training data, and no WER that OpenAI itself reports for Yiddish.
- I did not confirm FLEURS's language list directly. The absence of Yiddish from OpenAI's FLEURS chart suggests FLEURS has no Yiddish (inference).
- I could not confirm the absence of an MMS-1B-all Yiddish ASR adapter from Meta's own page; it rests on the aggregator.
- I did not verify Canary/Parakeet/Voxtral timestamp features, the Voxtral 3 language list, or SeamlessM4T timestamping.

## 2. Existing Yiddish fine-tuned checkpoints (Hugging Face and GitHub)

### Takeaway
About fifteen Yiddish ASR checkpoints, conversions and projects exist. Almost all descend from ivrit.ai's `yi-whisper-large-v3` / `-turbo` (Apache-2.0, about 97 h of crowd-sourced, mixed-dialect read speech, data cut-off Oct 2025). Only two projects publish numbers on Hasidic/heimish or Chabad speech after training on it. 3Yiddish's heimish models score 11.9 and 16.0 wrong words per 100, but are licensed CC BY-NC-SA 4.0 (non-commercial). The Rebbe-farbrengen fine-tune reaches whole-file WER 0.080 / CER 0.045 for a single speaker, but its weights are unreleased and it has no license. No permissively licensed, benchmarked Hasidic checkpoint exists today.

### Cited Findings
1. **ivrit-ai/yi-whisper-large-v3** (ivrit.ai, nonprofit). Base: openai/whisper-large-v3, described as "continued training". Data: ~78 h crowd-recital-yi (Wikipedia/Michlol snippets) plus ~19 h crowd-whatsapp-yi-whisper-training. License: Apache-2.0. Per the card, "Yi eval set is not yet published - an internal eval set was used". Language detection is degraded, so the Yiddish token must be set explicitly. [snippet] — [ivrit-ai/yi-whisper-large-v3](https://huggingface.co/ivrit-ai/yi-whisper-large-v3). Variants: `yi-whisper-large-v3-turbo`, `-ct2`, GGML. [snippet/aggregator] — [yi-whisper-large-v3-turbo](https://huggingface.co/ivrit-ai/yi-whisper-large-v3-turbo); [Yiddish Whisper Training collection](https://huggingface.co/collections/ivrit-ai/yiddish-whisper-training). The directory dates the training data to Oct 2025. [aggregator]. A personal copy also exists at [yoad/yi-whisper-large-v3](https://huggingface.co/yoad/yi-whisper-large-v3). [snippet]
   - Third-party scores:
     - 3Yiddish heimish benchmark: turbo 15.6 and large-v3 18.3 wrong words per 100. Loshn-koydesh words wrong (of 13): 10 and 11. English words wrong (of 14): 13 and 8. [fetched] — [BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
     - YiddishTools ONNX builds, greedy decoding, on 6 min of "hard oral-history audio (elderly speaker, tape noise)": large-v3 about 17% CER / 32% WER; turbo q4 about 25% CER / 32% WER. [aggregator] — [YiddishTools/yi-whisper-large-v3-ONNX](https://huggingface.co/YiddishTools/yi-whisper-large-v3-ONNX)
     - Rebbe farbrengens: about 0.57 whole-recording WER (large-v3). [fetched] — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
2. **3Yiddish** (GitHub TripleU613; 12★; updated 2026-11-24). Two transcription models "trained on heimish Yiddish":
   - The default is a fine-tune of OpenAI whisper-large-v3-turbo (about a 575 MB download; GPU or CPU). It scores 11.9 wrong words per 100, with 6/13 loshn-koydesh and 10/14 English words wrong.
   - The second is a fine-tune of Meta Omnilingual ASR CTC 1B v2 (about 1 GB; CPU via ONNX Runtime). It scores 16.0, with 9/13 and 8/14 wrong.
   - The earlier model, used up to version 2026.11.11, was built on ivrit-ai/yi-whisper-large-v3-turbo and scored 12.6.
   - It offers a local OpenAI-compatible transcription API (`3yiddish serve`, port 8793) and `3yiddish transcribe shiur.mp3`.
   - License: CC BY-NC-SA 4.0 ("Don't sell it. Ever.").
   - The README does not disclose training data or hours.
   [fetched] — [README](https://github.com/TripleU613/3Yiddish), [NOTICE](https://github.com/TripleU613/3Yiddish/blob/HEAD/NOTICE), [BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
   - Benchmark caveats stated by the authors: one speaker; "the answer key grew out of 3Yiddish's own draft", which may favour 3Yiddish and ivrit-ai-derived tools; and the latest entries were judged mostly "by AI agents, not the speaker". [fetched] — same BENCHMARK.md
3. **golanbenoni/rebbe-yiddish-asr** (Golan Benoni, using data from The Daily Sicha; 2★; README refreshed 2026-09-24).
   - Base and method: full fine-tunes of ivrit-ai's Yiddish Whisper large-v3-turbo and large-v3.
   - Training data: 27,833 clips (153 h, at most 28 s each) cut from farbrengens aligned to published hanachos.
   - Test sets: 20 fixed held-out test days (488 clips; 20 whole recordings).
   - Best whole-file results: WER 0.080 / CER 0.045 (turbo-full-lr3e5-4ep-seed7); large-v3 full fine-tune 0.081 / 0.047.
   - Other variants: LoRA about 0.084; frozen encoder about 0.20.
   - Transcripts delivered against hanachos, by year: 5778 WER 0.0716 / CER 0.0407 (247 days); 5779 0.0698 / 0.0404; 5780 0.0728 / 0.0419; 5781 0.0898 / 0.0497.
   - Release status: weights not released; "all rights reserved until [a license] is chosen".
   - Training ran in fp32 on 8 Apple-silicon Mac Studios.
   [fetched] — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
4. **akiva-yiddish/yiddish-whisper-phone**: small and base Whisper models for whisper.cpp, described as "Phone-sized Yiddish (Hasidic) speech recognition". They start from Hebrew fine-tunes, then get a full fine-tune on about 97 h of ivrit.ai Yiddish (crowd-recital-yi plus crowd-whatsapp-yi). No metrics found. [snippet] — [akiva-yiddish/yiddish-whisper-phone](https://huggingface.co/akiva-yiddish/yiddish-whisper-phone). The 3Yiddish benchmark, which did not test this model, says "There is no good small heimish model to download today". [fetched]
5. **alphaedge-ai/whisper-{tiny,base,small,medium}-ydd-{16384,32768}**: Whisper models with the vocabulary trimmed to Yiddish. 3Yiddish says they were "never trained on heimish speech" and scored base 226 and tiny 302 wrong words per 100 ("Nonsense with long repeat loops" / "English babble"). [snippet + fetched] — [alphaedge-ai/whisper-small-ydd-16384](https://huggingface.co/alphaedge-ai/whisper-small-ydd-16384); [BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
6. **Format conversions of the ivrit.ai models (no retraining)**: YiddishTools ONNX/transformers.js builds (Apache-2.0) [aggregator] — [YiddishTools/yi-whisper-large-v3-ONNX](https://huggingface.co/YiddishTools/yi-whisper-large-v3-ONNX); [mattscottml/yi-whisper-large-v3-ONNX](https://huggingface.co/mattscottml/yi-whisper-large-v3-ONNX) [snippet].
7. **torahanytimecom/whisper-large-v3-yeshivish** (TorahAnytime, apache-2.0): a "Whisper large-v3 variant… named for Yeshivish speech". The card is the stock Whisper card, so data and metrics are undocumented. [aggregator] — [model](https://huggingface.co/torahanytimecom/whisper-large-v3-yeshivish)
8. **es2814207/whisper-large-v2-yiddish-ivrit-ai**, plus a Yiddish data-collection Space; the cards contain little detail. [aggregator, flagged unverified by the directory] — [model](https://huggingface.co/es2814207/whisper-large-v2-yiddish-ivrit-ai)
9. **w2v-BERT 2.0 Yiddish models**: `ibleaman/w2v-bert-2.0-yiddish-northeastern` (Isaac Bleaman; Northeastern/Litvish) and `jacobjwebber/w2v-bert-2.0-yiddish_reyd` (trained on REYD). Both are tagged automatic-speech-recognition and have empty cards. [aggregator, unverified] — [ibleaman model](https://huggingface.co/ibleaman/w2v-bert-2.0-yiddish-northeastern); [jacobjwebber model](https://huggingface.co/jacobjwebber/w2v-bert-2.0-yiddish_reyd)
10. **MMS-HM/sicha-to-text**: offline Apple-Silicon transcription of Chabad sichos using whisper.cpp with "a modified ivrit-ai/yi-whisper-large-v3 (q5_0)". Apache-2.0. It runs at about 9x real time on an M5 and warns that niggunim cause repetition loops. [aggregator] — [MMS-HM/sicha-to-text](https://github.com/MMS-HM/sicha-to-text)
11. **smrtesy/ivrit-runpod-yi**: ivrit.ai's RunPod serverless worker with `ivrit-ai/yi-whisper-large-v3-ct2` baked in; "the upstream image bakes only the turbo Yiddish model". Called with `language="yi"`. Used to transcribe the Rebbe's "dollars" archive. Updated 2026-11-12. [fetched] — [smrtesy/ivrit-runpod-yi](https://github.com/smrtesy/ivrit-runpod-yi); upstream [ivrit-ai/runpod-serverless](https://github.com/ivrit-ai/runpod-serverless) (49★, MIT) [fetched via GitHub search]
12. **Hasidic phonetics and TTS side projects**:
    - `maxmelichov/xeus-yi-ipa`: "Hasidic Yiddish IPA ear with stress: PhoneticXEUS + CTC lattice over per-word reading menus". [snippet] — [repo](https://github.com/maxmelichov/xeus-yi-ipa)
    - `maxmelichov/phonikud-yi-blue-tts`: "Hasidic Yiddish pointing, G2P and speech". [fetched via GitHub search] — [repo](https://github.com/maxmelichov/phonikud-yi-blue-tts)
    - Phonikud-yi (Kohn-AI): self-reported nikud word accuracy of 99.94% in-distribution and 78.31% out-of-distribution (409 words). It was "built to label a ~197 h podcast corpus for TTS". [aggregator] — [Abe1018776/Phonikud-yi](https://github.com/Abe1018776/Phonikud-yi)
13. **"JEM Yiddish ASR"**: `Abe1018776/jem-asr-hub` is a hub repo ("documentation, architecture, and project links"; files include `50-HOURS-DASHBOARD-PLAN.md` and `weekly-update-2026-02-27.html`). It releases no models or metrics, and what "JEM" stands for is not stated. [fetched] — [jem-asr-hub](https://github.com/Abe1018776/jem-asr-hub). `khushi0433/jem-asr-app` is described as a "data preparation tool for Yiddish ASR training". [fetched via GitHub search] — [repo](https://github.com/khushi0433/jem-asr-app)
14. **Data-prep tools**:
    - `Abe1018776/yiddish-audio-alignment`: "Yiddish audio-text alignment using stable-ts with confusion-aware algorithm, adapted from ivrit-ai" (repo description, updated 2026-04-29). Its README is actually the transcript-cleaner README. [fetched] — [repo](https://github.com/Abe1018776/yiddish-audio-alignment)
    - `Shloimy15e/clean-yiddish-transcripts`: removes seif markers, titles, inline brackets and, optionally, editorial Hebrew citations, to extract spoken content for training. [fetched/aggregator] — [repo](https://github.com/Shloimy15e/clean-yiddish-transcripts)
15. **Closed or semi-closed Yiddish STT apps that surfaced on GitHub** (for the commercial-tools researcher):
    - Yiddi Type: its small built-in model is "the only small model trained on heimish speech" and scored 31.6. It also offers a larger model the program describes as ivrit-ai's. [fetched] — [BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
    - hafutza-typing-offline, yiddish-ear-gpu, zugit-android. [fetched via GitHub search] — [yiddishtype/hafutza-typing-offline](https://github.com/yiddishtype/hafutza-typing-offline), [retailjacob-del/yiddish-ear-gpu](https://github.com/retailjacob-del/yiddish-ear-gpu), [rivi-zel/zugit-android](https://github.com/rivi-zel/zugit-android)

### Inferences
- If the hotline's use counts as commercial, the permissively licensed starting points are ivrit-ai yi-whisper-large-v3(-turbo) (Apache-2.0; base Whisper MIT) and Omnilingual (Apache-2.0). 3Yiddish's better-scoring models are CC BY-NC-SA, so using them would need the author's permission unless the use is clearly non-commercial. Whether a nonprofit hotline counts as "non-commercial" is a legal question.
- The Rebbe project is the strongest evidence of what in-domain aligned data buys: WER fell from about 0.57 to 0.080 with 153 h of single-speaker aligned audio. Because it is one speaker and one register, the gain on multi-speaker Hasidic shiurim would probably be smaller per hour of data.
- Across every open model, loshn-koydesh words and embedded English words are where errors concentrate. That is exactly the register mix in shiurim.

### Gaps
- Not collected because huggingface.co was blocked: download counts, likes, exact last-modified dates of HF models, the full crowd-recital-yi card, and any akiva-yiddish metrics.
- 3Yiddish's training data, hours and test set are not published beyond the 25-take benchmark, and it is not documented whether its weights can be downloaded separately from the app.
- I found no Yiddish checkpoint trained specifically on Satmar/Hungarian-Hasidic shiur audio with published results.

## 3. Yiddish speech and text datasets usable for training or evaluation

### Takeaway
Public, openly licensed Yiddish speech with transcripts is tiny: ivrit.ai has about 97–99 h of crowd read speech (commercial-friendly custom license) and Common Voice has about 2 h (CC0). The largest research corpus is the Corpus of Spoken Yiddish in Europe (about 326 h transcribed; prewar Eastern European dialects; Hebrew-script and Latin transcripts; terms of use apply). The Yiddish Book Center holds large audio collections (oral histories, Montreal lectures, audiobooks), but I found no ML license for them. Large aligned Hasidic or Chabad material exists only privately, for example the Daily Sicha's 253 h plus 306 h of farbrengens aligned to hanachos. Kol Halashon's shiur archive (more than 180,000 shiurim) has no known transcripts.

### Cited Findings
**Crowd-sourced (ivrit.ai)**
- `ivrit-ai/crowd-whatsapp-yi`: volunteers read predefined messages as WhatsApp voice notes, and the captions were aligned with Stable Whisper. It is ongoing: cut-off Oct 2025, total recorded duration 20.55 h. It is released under the ivrit.ai License ("broad research and commercial use"), and access requires accepting it. [snippet] — [crowd-whatsapp-yi](https://huggingface.co/datasets/ivrit-ai/crowd-whatsapp-yi). The Whisper-ready derivative excludes low-quality segments, resamples to 16 kHz mono and totals about 19 h. [snippet] — [crowd-whatsapp-yi-whisper-training](https://huggingface.co/datasets/ivrit-ai/crowd-whatsapp-yi-whisper-training)
- `ivrit-ai/crowd-recital-yi`: about 78 h of crowd read speech, gated, under a custom ('other') license. [aggregator] — [crowd-recital-yi](https://huggingface.co/datasets/ivrit-ai/crowd-recital-yi). ivrit.ai's org page says its datasets use the "ivrit.ai License (v2)". [snippet] — [ivrit-ai org](https://huggingface.co/ivrit-ai)
- ivrit.ai's open-source recital tooling downloads sessions filtered by recording language and aligns them with `ivrit-ai/whisper-large-v3-turbo-ct2`. [fetched] — [ivrit-ai/ivrit.ai](https://github.com/ivrit-ai/ivrit.ai)

**Mozilla Common Voice (Yiddish, locale `yi`; CC0; read speech; dialect not specified)**
- Scripted Speech 23.0: 1,849 clips, 1.7 h (1.7 h validated), 95 speakers. [snippet] — [Mozilla Data Collective CV 23.0 yi](https://datacollective.mozillafoundation.org/datasets/cmflnuzw7mguohrskxmzwypsx)
- Scripted Speech 27.0: 2,024 clips, 2.23 h, 128 speakers, CC0. [snippet] — [Mozilla Data Collective CV 27.0 yi](https://mozilladatacollective.com/datasets/cmu614e1300k5nq07yunt0ea4)
- Stats API on 2026-10-20: 2.1 validated hours, 129 speakers. [aggregator] — [commonvoice.mozilla.org/yi](https://commonvoice.mozilla.org/yi)

**FLEURS and Omnilingual**
- FLEURS: no Yiddish appears in OpenAI's FLEURS per-language chart [fetched — [language-breakdown.svg](https://github.com/openai/whisper/blob/main/language-breakdown.svg)]. ivrit.ai's Hebrew eval suite uses FLEURS `he_il` [fetched — [ivrit-ai/asr-training](https://github.com/ivrit-ai/asr-training)].
- Omnilingual ASR used 12.8 h of `ydd_Hebr` training data [fetched — [CSV](https://github.com/facebookresearch/omnilingual-asr/blob/main/per_language_results_table_7B_llm_asr.csv)]. The released Omnilingual corpus is CC-BY-4.0 [fetched — [README](https://github.com/facebookresearch/omnilingual-asr)]. I could not verify whether it contains Yiddish.

**Corpus of Spoken Yiddish in Europe (CSYE; Isaac Bleaman, UC Berkeley; NSF CAREER grant)**
- Size: it passed the 300-hour mark (Berkeley news, 2026-05-10), and a Berkeley page gives "326.3 hours of manually transcribed speech from 171 survivors". It is drawn from USC Shoah Foundation testimony interviews. It is described in Bleaman & Nove (2025), *Language Documentation & Conservation* 19:142–157. [snippet] — [Berkeley: Yiddish corpus milestone](https://lx.berkeley.edu/news/yiddish-corpus-milestone); [LD&C article](https://scholarspace.manoa.hawaii.edu/items/6342c9d8-773c-4a38-9ad7-73314a38bce6); [Forward](https://forward.com/yiddish-world/826372/interviews-holocaust-survivors-authentic-yiddish-corpus/)
- Transcripts are public on GitHub in several formats:
  - txt: speaker, start time, end time, utterance; this is time-aligned
  - ELAN `.eaf` and Praat `TextGrid`
  - srt/vtt
  - "Reviewed" transcripts are available in both Latin transliteration and Hebrew-script Yiddish; "unreviewed" ones only in Latin. `UNK` and `<...>` mark unclear speech.
  - Use is "governed by our Terms of Use and requirements for proper attribution".
  [fetched] — [YiddishCorpus/CSYE-Transcripts](https://github.com/YiddishCorpus/CSYE-Transcripts); [CSYE user guide](https://www.yiddishcorpus.org/csye/user-guide/)
- Dialect: many Eastern European dialects [aggregator]. The LREC 2026 ASR paper used 42 h from 60 survivors, Northeastern ("Lithuanian") Yiddish. [snippet] — [ACL Anthology 2026.htres-2.3](https://aclanthology.org/2026.htres-2.3/)

**Yiddish Book Center (YBC)**
- Wexler Oral History Project: "more than 1,300 in-depth video interviews recorded since 2010". An older page says more than 100 of 600+ interviews were in Yiddish. A 2017 NEH grant produced transcripts of 400 interviews, and the 2019 review reports "first draft transcripts of 400 interviews" from an outside firm. Interviews are "freely available to view or download" on the site. [snippet] — [Yiddish Oral Histories](https://www.yiddishbookcenter.org/language-literature-culture/wexler-oral-history-project-presents/yiddish-oral-histories); [NEH grant news](https://yiddishbookcenter.org/about/news/neh-grant-provides-enhanced-access-hundreds-oral-history-interviews-about-yiddish-and); [2019 review](https://yiddishbookcenter.org/2019-year-review)
- Frances Brandt Online Yiddish Audio Library: lectures and interviews with writers at the Jewish Public Library of Montreal, 1953–2005. The source is "1,500 reel-to-reel tapes, totaling 3,000 hours" (an estimate), with a plan for 1,100 recordings online. [snippet] — [YBC collection page](https://www.yiddishbookcenter.org/collections/frances-brandt-online-yiddish-audio-library); [Jewish Ledger](https://www.jewishledger.com/?p=16975)
- YBC audiobooks: the REYD TTS work (Interspeech 2022) used about 8 h / 4,892 hand-corrected utterances from YBC audiobooks (3 speakers) and released MFA TextGrids. [aggregator] — [REYD-TTS](https://github.com/REYD-TTS); [Interspeech 2022 paper](https://www.isca-archive.org/interspeech_2022/webber22_interspeech.html). The LREC 2026 ASR paper tested cross-domain on "Yiddish audiobooks": 27.07% WER, 6.56% CER (phonemic representation). [snippet] — [ACL Anthology](https://aclanthology.org/2026.htres-2.3/)
- Text: full-text OCR search over the Steven Spielberg Digital Yiddish Library using Jochre 3 (AGPL-3.0). Jochre 3 scores CER 1.5%, against Jochre 2 17.0%, Transkribus DiJeST 19.9% and Tesseract Yiddish 9.0% on the corpus test set, and comes with a 658-page / 186K-token OCR corpus. [aggregator] — [ocr.yiddishbookcenter.org](https://ocr.yiddishbookcenter.org/); [arXiv 2501.08442](https://arxiv.org/abs/2501.08442). YBC "granted us permission to train on the Yiddish Book Center's Digital Yiddish Library" (MameLoshnLM), which shows that permission is negotiated rather than open. [fetched] — [katzurik/MameLoshnLM](https://github.com/katzurik/MameLoshnLM)

**Hasidic or Chabad speech with text**
- The Daily Sicha (via rebbe-yiddish-asr), owned by The Daily Sicha and not published:
  - Delivery 1 (site, 5783–5787): 1,468 days, 253 h audio, 1.86M words.
  - Delivery 2 (PDF hanachos, 5775–5781, received 2026-09-23): 1,758 days, 306 h, 2.31M words.
  - Alignment anchor rates were 0.94–0.98.
  - Transcript style: unpointed Hebrew script, loshn koydesh verbatim, abbreviations with gershayim (רש"י, הקב"ה).
  [fetched] — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
- `Kohn-AI/yiddish-tts-aligned-v0`: Yiddish TTS pairs with audio clips, transcripts and "American-Hasidic-Yiddish IPA phonemes". Size not retrieved. [snippet] — [dataset](https://huggingface.co/datasets/Kohn-AI/yiddish-tts-aligned-v0)
- Kol Halashon: "the full archive now exceeds 180,000 shiurim", in English, Yiddish, Hebrew, French, Spanish, Russian and Farsi, growing by more than 75 shiurim a day. The article is undated, and no language breakdown or transcripts were found. [snippet] — [Yeshiva World News](https://www.theyeshivaworld.com/news/general/31946/kol-haloshon-celebrates-four-years-of-providing-free-torah-shiurim-worldwide.html)

**Text corpora (language modeling, normalization)**
- Williamsburg Hasidic Yiddish Corpus: contemporary Williamsburg Hasidic prose, CC-BY-4.0. [aggregator] — [men-seg-1/williamsburg-hasidic-yiddish](https://huggingface.co/datasets/men-seg-1/williamsburg-hasidic-yiddish)
- The Yiddish Wikipedia "is written largely in the Chasidic orthography, along with a small proportion of YIVO orthography mixed in" (Sälevä, LREC 2020). [snippet] — [LREC 2020 paper](https://aclweb.org/anthology/2020.lrec-1.119.pdf)
- Kave Shtiebel forum: Bleaman (2020) extracted "nearly 38,000 tokens of non-finite particle verbs" from it, and his hasidifier exception list is based on its lexical frequencies. [snippet + fetched] — [Frontiers in AI 2020](https://www.frontiersin.org/articles/10.3389/frai.2020.00035/pdf); [hasidifier](https://github.com/ibleaman/hasidifier)
- MameLoshnLM's "Oytser" pretraining corpus combines contemporary web-native sources with literary materials. Its authors found that "much of [mC4's] putative Yiddish content is either machine-translated spam or not Yiddish at all". [snippet] — [arXiv 2608.05850](https://arxiv.org/pdf/2608.05850)
- Other text resources:
  - UD Yiddish-YiTB treebank. [fetched via GitHub search] — [repo](https://github.com/UniversalDependencies/UD_Yiddish-YiTB)
  - Penn Parsed Corpus of Historical Yiddish (about 200,000 words). [fetched via GitHub search] — [repo](https://github.com/beatrice57/penn-parsed-corpus-of-historical-yiddish)
  - YiDraCor (Yiddish drama). [fetched via GitHub search] — [repo](https://github.com/dracor-org/yidracor)
  - InTSaYT treebanking project (GPL-3.0). [fetched via GitHub search] — [repo](https://github.com/joelcw/intsayt)
  - alpaca-yiddish (machine-translated instructions). [aggregator] — [dataset](https://huggingface.co/datasets/saillab/alpaca-yiddish-cleaned)
  - yiddish-extractor (separates Yiddish from Hebrew and Aramaic in mixed documents). [aggregator] — [Mordy-S/yiddish-extractor](https://github.com/Mordy-S/yiddish-extractor)
- Historic: the AHEYM project (Indiana University) built a Yiddish speech corpus, forced aligner and basic ASR system (LREC 2016). Size not retrieved. [snippet] — [ACL Anthology L16-1744](https://aclanthology.org/L16-1744/)

### Inferences
- The client's own archive is likely the most valuable Hasidic-register training resource available: 100 h/week of in-domain shiurim. Even a few hundred hours with corrected transcripts would exceed all public Hasidic speech data found, and public data totals about 100 h of crowd read speech.
- CSYE and YBC audio are large, but they are prewar or literary dialects in YIVO-style orthography. They would mainly add acoustic and dialect robustness, and their transcripts would need conversion to Hasidic spelling (see section 6).
- The biggest aligned sources in the Hasidic/Chabad register (Daily Sicha, JEM, Kol Halashon, community podcasts) are privately held. Partnerships or licenses, not technology, are the bottleneck.

### Gaps
- Not researched or found: LibriVox Yiddish recordings; YIVO Sound Archive holdings and terms; Yiddish radio archives (only links to the Yiddish Radio Project, e.g. https://henrysapoznik.com/yiddish-radio, with no transcripts or sizes); JEM's archive size and transcript availability; Hasidic newspapers (Der Yid, Der Blatt, Di Tzeitung) as text corpora.
- License for ML use of YBC oral histories and audiobooks: not found.
- Dialect and orthography metadata for crowd-recital-yi: not found.
- Size and availability of the about 197 h podcast corpus mentioned for Phonikud-yi: unknown.
- Whether Kol Halashon permits bulk use, and what share of it is Yiddish: unknown.

## 4. Hebrew precedent (ivrit.ai) and other Jewish-language ASR projects

### Takeaway
ivrit.ai is the working template. It combined donated podcast and lecture audio, crowd transcription, crowd recital, WhatsApp-prompt recording and Knesset audio under a commercial-friendly license, then published Whisper fine-tunes and a public leaderboard. Its Hebrew turbo fine-tune has about 37–38% fewer word errors than stock turbo on FLEURS Hebrew (18.46% vs 29.51% WER). The same crowdsourcing machinery seeded the only open Yiddish models. I found no evidence that ivrit.ai specifically covers yeshivish or Talmudic Hebrew/Aramaic, and no measurement of whether Hebrew fine-tunes help Yiddish.

### Cited Findings
- ivrit.ai (2023): "over 3,300 hours of Hebrew speech together with their transcription" from podcasts and lectures, available "for research and development… encompassing both non-commercial and commercial uses". [snippet] — [arXiv 2307.08720](https://arxiv.org/abs/2307.08720)
- Marmor et al., "Building an Accurate Open-Source Hebrew ASR System through Crowdsourcing", Interspeech 2025, pp. 723–727 (DOI 10.21437/interspeech.2025-460). It describes training data of more than 15,000 hours of Hebrew and a crowd-transcribed subset. Its WER table against OpenAI Whisper large-v2/v3 came through garbled, so the column-to-test-set mapping is unverified and I am not reporting those numbers. [snippet] — [ISCA PDF](https://www.isca-archive.org/interspeech_2025/marmor25_interspeech.pdf); [Bar-Ilan record](https://cris.biu.ac.il/en/publications/building-an-accurate-open-source-hebrew-asr-system-through-crowds/)
- A secondary navigator (2026) says ivrit.ai hosts "22,000+ hours" of Hebrew audio "under permissive licenses that explicitly allow commercial training". [fetched, secondary] — [hebrew-ml-datasets-navigator SKILL.md](https://github.com/peleg-jpg/hebrew-ml-datasets-navigator). The hour counts differ by source and date: 3,300 (2023), more than 15,000 (2025 paper), 22,000+ (2026 secondary).
- Models:
  - `ivrit-ai/whisper-large-v3`: fine-tuned from openai/whisper-large-v3, training date April 2025, language detection degraded. [snippet] — [model](https://huggingface.co/ivrit-ai/whisper-large-v3)
  - `ivrit-ai/whisper-large-v3-turbo`: trained on crowd-transcribe-v5, crowd-recital-whisper-training and knesset-plenums-whisper-training; Apache-2.0. [snippet] — [README](https://huggingface.co/ivrit-ai/whisper-large-v3-turbo/blob/main/README.md)
  - An older checkpoint, `ivrit-ai/whisper-v2-d3-e3`, exists. [snippet]
- Measured improvement on FLEURS Hebrew (`he_il`, full test split of 792 utterances; greedy decoding; Q8_0 GGUF via transcribe.cpp; issue opened 2026-11-01): ivrit.ai large-v3-turbo 18.46% WER (95% CI 16.94–20.02) vs stock large-v3-turbo 29.51% (27.85–31.24). A maintainer rerun gave 18.45% vs 29.69% ("38% fewer errors"). [fetched] — [anomalyco/hex issue #752](https://github.com/anomalyco/hex/issues/752). Another community run reported 14.37% vs 28.59%, probably with a different harness. [snippet] — same search results
- License details: the `ivrit-ai/audio-vad` card uses CC BY 4.0 as a base but restricts use to "the express purpose of training AI models", and that clause overrides CC BY 4.0. [snippet] — [audio-vad](https://HuggingFace.co/datasets/ivrit-ai/audio-vad)
- Evaluation infrastructure:
  - The eval suite covers ivrit_ai_eval_d1, saspeech, FLEURS he_il, Common Voice 17 he and hebrew_speech_kan.
  - A public "Hebrew transcription leaderboard" mixes open models and commercial APIs.
  - The engines include faster-whisper, HF Transformers, OpenAI, Amazon and Google.
  - There is a converter to CT2, ONNX and GGML.
  [fetched] — [ivrit-ai/asr-training](https://github.com/ivrit-ai/asr-training); [leaderboard](https://huggingface.co/spaces/ivrit-ai/hebrew-transcription-leaderboard)
- Ecosystem: dozens of community apps build on ivrit.ai models (dictation, call transcription, subtitles), and ivrit.ai's RunPod serverless worker has 49★. [fetched via GitHub search] — [ivrit-ai/runpod-serverless](https://github.com/ivrit-ai/runpod-serverless)
- Does Hebrew help Yiddish? The akiva-yiddish phone models start from Hebrew fine-tunes before Yiddish training. [snippet] — [akiva-yiddish/yiddish-whisper-phone](https://huggingface.co/akiva-yiddish/yiddish-whisper-phone). The ivrit.ai Yiddish card copies the Hebrew card's wording ("intended for mostly-hebrew audio transcription"). [snippet] — [yi-whisper-large-v3](https://huggingface.co/ivrit-ai/yi-whisper-large-v3)
- Other Jewish-language and Torah ASR:
  - TorahAnytime `whisper-large-v3-yeshivish`: undocumented. [aggregator] — [model](https://huggingface.co/torahanytimecom/whisper-large-v3-yeshivish)
  - `danielrosehill/Whisper-Hebrish` (Hebrew-English mix). [snippet; exists] — [model](https://huggingface.co/danielrosehill/Whisper-Hebrish)
  - `renikud/whisper-he-ipa` (Hebrew to IPA). [snippet; exists] — [model](https://huggingface.co/renikud/whisper-he-ipa)
  - HebDB, a weakly supervised Hebrew speech dataset. [snippet] — [arXiv 2407.07566](https://arxiv.org/pdf/2407.07566)
  - `powerjews/torah-transcript-scribe`: a "Claude plugin that proofreads AI and YouTube transcripts of Torah classes, restores Hebrew, Aramaic and Yiddish, and checks citations on Sefaria" (MIT). [fetched via GitHub search] — [repo](https://github.com/powerjews/torah-transcript-scribe)
  - Commercial, with no metrics published: Sofer.ai (shiurim mixing English, Hebrew, Yiddish, Aramaic and "Yeshivish") and Milai (Torah shiurim; Ashkenazi pronunciation). [aggregator] — [sofer.ai](https://www.sofer.ai/); [milai.co.il](https://milai.co.il/)

### Inferences
- ivrit.ai's recipe can be reused directly for Hasidic Yiddish: donated audio, crowd correction in a web tool, read-prompt recording over WhatsApp, a permissive license, and a public leaderboard. Its Yiddish track already shows the WhatsApp/recital machinery works for Yiddish speakers. The missing piece is in-domain shiur audio with corrected transcripts.
- Hebrew fine-tunes may help with loshn-koydesh vocabulary. However, Hasidic Ashkenazi pronunciation of Hebrew and Aramaic words differs from Israeli Hebrew, so the benefit is uncertain. No ablation was found.

### Gaps
- The Interspeech 2025 per-test-set WER numbers (PDF host blocked).
- Whether any ivrit.ai Hebrew data covers yeshiva or Talmudic register or Aramaic: not documented in anything I could access.
- No published comparison of Hebrew-initialized vs OpenAI-initialized Yiddish fine-tunes.
- No Ladino, Judeo-Arabic or other Jewish-language ASR projects surfaced in my searches. This was not searched exhaustively.

## 5. Academic work on Yiddish ASR, Hasidic Yiddish NLP, orthography and corpora (2016–2026)

### Takeaway
Academic Yiddish ASR is very recent and centered on Isaac Bleaman (UC Berkeley). His LREC 2026 workshop paper builds the first ASR for European (Northeastern) Yiddish on CSYE. Its key finding is that the transcription orthography and Unicode form dominate results: a Hebrew-based phonemic script with precomposed characters reached 37.96% mean WER, while standard YIVO spelling in decomposed Unicode failed at 99.67%. Hasidic-specific NLP consists mainly of Bleaman's sociolinguistic and orthography work, the `yiddish` Python library and the hasidifier, plus 2026 LLM work (MameLoshnLM).

### Cited Findings
- **Bleaman, "Automatic Transcription of Holocaust Testimonies in Yiddish: Orthographic Comparison and Cross-Domain Validation"** (HTRes-2026 workshop at LREC 2026):
  - Presented as "the first ASR system for European Yiddish", focused on Northeastern ("Lithuanian") Yiddish.
  - Data: 42 h of speech segments from 60 survivors in CSYE.
  - Results: a Hebrew-based phonemic system with precomposed Unicode was best at a mean WER of 37.96%; romanized Yiddish 59.40%; standard Yiddish in decomposed Unicode 99.67%.
  - Cross-domain test on Yiddish audiobooks: 27.07% WER, 6.56% CER.
  - Snippets mention following "the fine-tuning procedure of Lacombe (2024)" with a CTC vocabulary and "Fine-tune w2v2-BERT", so the base is probably wav2vec2-BERT rather than Whisper. This is unconfirmed.
  [snippet] — [ACL Anthology 2026.htres-2.3](https://aclanthology.org/2026.htres-2.3/); [LREC PDF](http://www.lrec-conf.org/proceedings/lrec2026/workshops/htres/pdf/2026.htres-1.3.pdf); [HTRes 2026](https://www.clarin.eu/HTRes2026)
- **Ćavar, Ćavar, Kerler & Quilitzsch (LREC 2016)**: "Generating a Yiddish Speech Corpus, Forced Aligner and Basic ASR System for the AHEYM Project", described as the first attempt at Yiddish speech corpora and speech technology. [snippet] — [ACL Anthology L16-1744](https://aclanthology.org/L16-1744/)
- **Webber, Lo & Bleaman (Interspeech 2022)**: REYD, the first Yiddish TTS dataset and system. It is FastSpeech2 trained on about 8 h of YBC audiobooks, with pretrained checkpoints for three input orthographies including "hasidic" spelling. [aggregator] — [REYD-TTS](https://github.com/REYD-TTS)
- **Bleaman (2020)**, "Implicit Standardization in a Minority Language Community: Real-Time Syntactic Change among Hasidic Yiddish Writers", Frontiers in AI. It uses about 38,000 tokens from the Kave Shtiebel forum and relies on morphological criteria because taggers and parsers "have yet to be developed for Hasidic Yiddish". [snippet] — [Frontiers in AI](https://www.frontiersin.org/articles/10.3389/frai.2020.00035/pdf)
- **Sälevä (LREC 2020)**: the first multi-orthography parallel corpus of Yiddish nouns, scraped from Wiktionary. It notes the Yiddish Wikipedia is mostly in Chasidic orthography. [snippet] — [LREC 2020](https://aclweb.org/anthology/2020.lrec-1.119.pdf)
- **"A Part-of-Speech Tagger for Yiddish"** (arXiv 2204.01175, 2022): works on the YBC corpus and notes "many spelling inconsistencies". [snippet] — [arXiv 2204.01175](https://arxiv.org/abs/2204.01175)
- **Urieli, Clooney, Sigiel & Leyfer (2025), Jochre 3 OCR**: CER 1.5% and a 658-page OCR corpus. [aggregator] — [arXiv 2501.08442](https://arxiv.org/abs/2501.08442)
- **Bleaman & Nove (2025)**, CSYE description, LD&C vol. 19. [snippet] — [ScholarSpace](https://scholarspace.manoa.hawaii.edu/items/6342c9d8-773c-4a38-9ad7-73314a38bce6)
- **Katz, Goldman, Limisiewicz, Tsarfaty & Smith (COLM 2026; arXiv 2608.05850, submitted 2026-08-06)**: MameLoshnLM is an 8B Yiddish LM made by continued pretraining of Llama 3.1 8B on the Oytser corpus, with the Kashes multi-task benchmark. Weights are CC BY-NC 4.0 on HF (Yiddish-NLP org). [fetched + snippet] — [katzurik/MameLoshnLM](https://github.com/katzurik/MameLoshnLM); [arXiv 2608.05850](https://arxiv.org/pdf/2608.05850)
- **Kakugo 3B Eastern Yiddish** (Devine et al.; arXiv 2601.14051): Granite-4.0-micro distilled on synthetic Eastern-Yiddish conversations, Apache-2.0. [aggregator] — [ptrdvn/kakugo-3B-ydd](https://huggingface.co/ptrdvn/kakugo-3B-ydd); [arXiv 2601.14051](https://arxiv.org/abs/2601.14051)
- **Other HTRes-2026 work**: a large-scale evaluation of Whisper on 1,847 testimonies from Yale's Fortunoff archive. It is not Yiddish-specific. [snippet] — [ACL Anthology 2026.htres-2.10](https://aclanthology.org/2026.htres-2.10/)
- **Orthography and normalization tools** (details in section 6): [ibleaman/yiddish](https://github.com/ibleaman/yiddish), [ibleaman/hasidifier](https://github.com/ibleaman/hasidifier), and Raphael Finkel's Hasidic-to-standard normalizer [cs.uky.edu](https://www.cs.uky.edu/~raphael/yiddish/checkSpellUTF.cgi). [fetched]

### Inferences
- The academic ASR results so far are on prewar Lithuanian/literary Yiddish, not Hasidic speech. The orthography and Unicode lesson transfers directly, but the WER levels do not.
- No peer-reviewed paper on Hasidic-Yiddish ASR was found. Hasidic-specific ASR evidence today comes from GitHub projects (3Yiddish, rebbe-yiddish-asr) and community benchmarks.

### Gaps
- I could not read the full LREC 2026 paper: the exact base model, the exact phonemic scheme, and whether the decomposed-YIVO failure was a vocabulary or tokenization artifact.
- No Interspeech or ICASSP 2024–2026 Yiddish ASR papers surfaced in searches. arXiv and ISCA could not be browsed directly.
- I did not verify the MameLoshnLM/Kashes benchmark numbers.

## 6. Practical: Hebrew-script output vs romanization, orthography, code-switching, downstream LLM use

### Takeaway
All the open Yiddish fine-tunes output Hebrew-script Yiddish. Use the language token "yi", forced. The training-target orthography has to be fixed deliberately:
- Hasidic vs YIVO spelling: converting Hasidic to YIVO is ambiguous, so pick one.
- Pointed vs unpointed text.
- Precomposed vs decomposed Unicode: standard Unicode normalization silently decomposes YIVO letters, and Whisper's default text normalizer turns the nikud into spaces.
- Gershayim abbreviations: blocked by Whisper's default token suppression.

Code-switching is the weak point. Forcing "yi" garbles embedded English (ivrit-ai turbo got 13 of 14 English words wrong), and loshn koydesh is the next-worst category.

### Cited Findings
**Two orthographies**
- Standard (YIVO) Yiddish is used in college courses and the *Forverts*. The Hasidic community, "where the majority of today's Yiddish speakers (and writers) come from", uses different conventions.
- The hasidifier converts YIVO to Hasidic only. The reverse "would additionally require a large dictionary, and ideally some grammatical information due to… ambiguous string tokens (e.g., 'few' and 'for' are both written פאר; 'cry' and 'wine' are both written וויין)". For that direction it points to Raphael Finkel's normalizer.
[fetched] — [ibleaman/hasidifier](https://github.com/ibleaman/hasidifier)

**Tooling**
- The `yiddish` Python library (MIT; `pip install yiddish`) offers:
  - precomposed/decomposed conversion (e.g. `אַ` U+FB2E vs U+05D0+U+05B7)
  - `strip_diacritics`
  - YIVO romanization (`transliterate`, with an optional loshn-koydesh lexicon)
  - `detransliterate`
  - `respell_loshn_koydesh` (phonetic spelling)
  - `hasidify`
  For example, `משפּחה` → `mishpokhe` with the loshn-koydesh option, and `וווינט` → `וואוינט` when hasidified.
  [fetched] — [ibleaman/yiddish](https://github.com/ibleaman/yiddish)
- The Avremi TTS pipeline "normalizes Hasidic/Soviet spelling to YIVO, romanizes with the `yiddish` lib", which shows the normalization chain in practice. [aggregator] — [loqal.digital](https://loqal.digital/yiddish-tts)

**Unicode and normalization pitfalls**
- Orthography and Unicode choice changed results from 37.96% (phonemic, precomposed) to 99.67% (YIVO, decomposed) mean WER in the LREC 2026 study. [snippet] — [ACL Anthology](https://aclanthology.org/2026.htres-2.3/)
- Checked locally in this session (Python `unicodedata`):
  - NFC, NFKC and NFD all turn U+FB2E (alef with pasekh) into U+05D0 + U+05B7 (a combining mark, category Mn). Standard Unicode normalization therefore never yields the "precomposed" form.
  - Whisper's default `BasicTextNormalizer` (`remove_diacritics=False`) applies NFKC and then replaces every character whose Unicode category starts with M, S or P with a space. Pointed YIVO words get split at every nikud mark during WER scoring.
  - With `remove_diacritics=True`, the normalizer uses NFKD and drops the marks instead.
  [fetched] — [whisper/normalizers/basic.py](https://github.com/openai/whisper/blob/main/whisper/normalizers/basic.py)

**Abbreviations and Hebrew/Aramaic words**
- Whisper's default suppression of the ASCII double quote "blocks gershayim" (רש"י, הקב"ה). The fix is `--suppress allow-quotes`. The Rebbe corpus keeps unpointed Hebrew script with vowel points removed, keeps punctuation, and embeds loshn koydesh verbatim. [fetched] — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)

**Code-switching and loshn koydesh, measured on heimish speech**
- English words wrong (of 14): ivrit-ai turbo 13; ivrit-ai large-v3 8; 3Yiddish Whisper 10; 3Yiddish Omnilingual 8; Gemini 4 Carbon 2; Yiddish Labs 0.
- Loshn-koydesh words wrong (of 13): ivrit-ai turbo 10; large-v3 11; 3Yiddish Whisper 6; Omnilingual 9; Yiddish Labs 1.
- The benchmark counts "English words written in English letters, and numbers in digits" as correct when they are the words that were said, so mixed-script output is the expected target.
- One Whisper-based service (TurboScribe or RocketScribe; not specified) returned "German in Latin letters".
[fetched] — [3Yiddish BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/HEAD/BENCHMARK.md)
- ivrit.ai's Yiddish models have degraded language detection, and the Yiddish token "should be explicitly set". They cannot switch languages mid-file through language ID. [snippet] — [ivrit-ai/yi-whisper-large-v3](https://huggingface.co/ivrit-ai/yi-whisper-large-v3)

**Romanization as output**
- The CSYE transcripts exist in Latin transliteration (all) and Hebrew script (reviewed only). [fetched] — [CSYE-Transcripts](https://github.com/YiddishCorpus/CSYE-Transcripts)
- Romanized targets scored worse than Hebrew-based phonemic targets (59.40% vs 37.96% WER) in the LREC 2026 study. [snippet] — [ACL Anthology](https://aclanthology.org/2026.htres-2.3/)

**Long-form failure mode**
- Niggunim (singing) cause Whisper repetition loops in sicha transcription. [aggregator] — [MMS-HM/sicha-to-text](https://github.com/MMS-HM/sicha-to-text)

**Downstream LLM use**
- mC4's Yiddish is largely spam or not Yiddish. [snippet] — [arXiv 2608.05850](https://arxiv.org/pdf/2608.05850)
- The only Yiddish-adapted open LLMs found are MameLoshnLM (8B, CC BY-NC 4.0) [fetched — [repo](https://github.com/katzurik/MameLoshnLM)] and Kakugo 3B (Apache-2.0) [aggregator — [model](https://huggingface.co/ptrdvn/kakugo-3B-ydd)].
- 3Yiddish's chat feature uses Gemma 4 E4B plus "our own translator, trained on heimish Yiddish" (a fine-tune of NLLB-200 distilled 1.3B, CC BY-NC 4.0) to "turn it into the Yiddish people actually write". [fetched] — [3Yiddish README](https://github.com/TripleU613/3Yiddish), [NOTICE](https://github.com/TripleU613/3Yiddish/blob/HEAD/NOTICE)

### Inferences
- Training-target choices:
  - Unpointed Hebrew-script Hasidic orthography matches what Hasidic readers and writers use.
  - Loshn-koydesh words should stay in their etymological Hebrew/Aramaic spelling.
  - Gershayim abbreviations should be kept, with quote suppression disabled.
  - English should be left in Latin script.
  - Every target should go through one fixed normalization function. Use the `yiddish` library and make an explicit choice about pointed letters; do not rely on NFC or NFKC.
- Mixing YIVO-orthography data (CSYE, YBC) with Hasidic targets without first running hasidify would teach the model two spellings for the same word. This hurts both WER and later LLM summarization and search.
- For evaluation, normalize both reference and hypothesis with the same Yiddish-aware normalizer. Do not use Whisper's `BasicTextNormalizer` on pointed text. Report WER and CER separately, because spelling variants inflate WER far more than CER.
- English-heavy shiurim may need segment-level language handling, for example language ID per VAD chunk followed by forced "yi" or "en" decoding. Alternatively, train on code-switched targets so English stays in Latin script. No open Yiddish model handles this well today.

### Gaps
- No published study of Hasidic-orthography consistency in ASR output, or of how well frontier LLMs summarize Hasidic-orthography ASR text. This is outside my scope; the commercial-API researcher may cover it.
- Which orthography ivrit.ai's crowd-recital-yi targets use: not documented in sources I could access. It likely follows Yiddish Wikipedia/Michlol spelling, which is mostly Hasidic (inference).
