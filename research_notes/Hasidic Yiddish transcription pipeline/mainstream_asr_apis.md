# Mainstream commercial speech-to-text and multimodal-LLM APIs for Yiddish transcription (status as of 2026-11-27)

*Method caveat for the report writer: in this research session WebFetch could not reach vendor domains (DNS blocked for docs.cloud.google.com, elevenlabs.io, assemblyai.com, deepgram.com, learn.microsoft.com, docs.aws.amazon.com, docs.speechmatics.com), and reddit.com was blocked for search. Every finding below comes from search-engine excerpts of the cited pages. Many of those pages are official docs or pricing pages, and some are third-party price trackers. Where a full table was truncated, "Yiddish not found" means not seen in the excerpt; it does not prove absence. Spot-check every price and language claim on the live vendor page before the final report treats it as definitive. "Official" = the excerpt came from the vendor's own domain. Volume used throughout: 100 h/week, which is about 433 h/month or 5,200 h/year.*

## 1. Which APIs officially support Yiddish ("yi" / "yi-*") as of late 2026?

### Takeaway
Very few do. **Gladia Solaria-1** is the only dedicated speech-to-text vendor whose official language table lists Yiddish (yi). **Google's general Gemini models** list Yiddish in Google's Gemini language-support list, but that list is model-general (text) and not an audio-specific guarantee. **Whisper** (OpenAI whisper-1 and every Whisper host: Groq, Fireworks, Together, Deepgram Whisper Cloud) accepts the `yi` language token. However, OpenAI publishes only languages with WER under 50%, and Yiddish was not confirmed on that list.

Two vendors are borderline:
- **ElevenLabs Scribe** lets you select "Yiddish (YID)", but a 2026-11-24 bug report says the output comes back in Latin transliteration rather than Hebrew script.
- **AssemblyAI**'s legacy 99-language "Universal" list included Yiddish, but its newer docs conflict.

No Yiddish support was found for: Google Chirp 2 and Chirp 3, Gemini 3.5 Transcribe, Azure (standard, fast, LLM Speech, MAI-Transcribe), AWS Transcribe, Speechmatics, Deepgram Nova-3, Soniox, Rev.ai, Mistral Voxtral, Alibaba Qwen3-ASR, and AssemblyAI Universal-3.x.

### Cited Findings

**Google Cloud Speech-to-Text v2 (Chirp 2 / Chirp 3)**
- Filtered by model, the V2 supported-languages table lists chirp_3 entries alphabetically from Afrikaans through Zulu with no Yiddish row. No Yiddish row was found for chirp_2 either. Hebrew is listed for both models as `iw-IL`. — [Cloud STT V2 supported languages](https://docs.cloud.google.com/speech-to-text/docs/speech-to-text-supported-languages) (the excerpt was truncated)
- Chirp 3 was "built using self-supervised training on millions of hours of audio and 28 billion sentences of text spanning 100+ languages", but transcription is offered in only about 85 languages and variants. — [Chirp 3 model page](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3)
- An October 2025 release note says Chirp 3 transcription is expanding to "85+ languages and locales". — [STT release notes](https://docs.cloud.google.com/speech-to-text/docs/release-notes)
- Google says the authoritative per-region, per-model list of languages and features comes from the locations API. — [Chirp 3 model page](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3)

**Gemini general multimodal models (Gemini API / Vertex)**
- Google's Gemini language-support list on the Vertex "Google models" page includes Yiddish (yi) and Hebrew (iw). Google cautions that the languages supported vary by model. — [Vertex AI Google models](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models)
- The Live API (voice) language list includes Hebrew but not Yiddish. — [Live API: configure language and voice](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/live-api/configure-language-voice)
- The Gemini embeddings language list includes both Hebrew (iw) and Yiddish (yi). — [Text embeddings API](https://docs.cloud.google.com/gemini-enterprise-agent-platform/reference/models/text-embeddings-api)

**Gemini 3.5 Transcribe (Google's dedicated Gemini ASR model, 2026)**
- The model "automatically detects 85+ languages" and handles code-switching. — [Gemini 3.5 Transcribe model page](https://ai.google.dev/gemini-api/docs/models/gemini-3.5-transcribe)
- Hebrew (he-IL) appears in the Gemini API transcription page's language table. The Google Cloud page marks Hebrew "Experimental". Yiddish appeared in neither table excerpt. — [Gemini API audio transcription](https://ai.google.dev/gemini-api/docs/transcribe); [Cloud: Gemini 3.5 Transcribe](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-transcribe)
- Third-party launch coverage repeats "85+ languages" without listing them, and none mentions Yiddish. — [XiaoHu.AI](https://xiaohu.ai/en/article/gemini-3-5-transcribe/); [Gnoppix forum](https://forum.gnoppix.org/t/googles-gemini-3-5-transcribe-turns-speech-to-text-in-85-languages-while-auto-correcting-your-verbal-stumbles/7188)

**OpenAI**
- whisper-1 "covers 98 languages" with accuracy that varies by language. OpenAI's published list includes only "the languages that exceeded <50% word error rate (WER)". — [OpenAI speech-to-text guide](https://developers.openai.com/api/docs/guides/speech-to-text); the quote is reproduced in [HackMD notes on the OpenAI guide](https://hackmd.io/Ikddsjn9T2-dermdWP4BsQ)
- Whisper's tokenizer `LANGUAGES` dictionary contains `"yi": "yiddish"`, so Whisper-family models accept a Yiddish language token. — [openai/whisper tokenizer.py](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py); the same entry appears in the [Hugging Face Transformers Whisper tokenizer](https://github.com/huggingface/transformers/blob/main/src/transformers/models/whisper/tokenization_whisper.py)
- One third-party blog counts 57 languages on OpenAI's published list; another claims "99+". The two conflict, and neither confirms Yiddish. — [opentranscription.io on gpt-4o-transcribe](https://opentranscription.io/blog/gpt-4o-transcribe)
- New model gpt-transcribe (reported launch 2026-07-28). No official language count was found for it. — [llms.ninja](https://llms.ninja/openai/models/gpt-transcribe); [AlphaSignal](https://alphasignal.ai/news/openai-replaces-whisper-with-gpt-transcribe-slashing-errors-by-52)
- The transcription API's `language` input is ISO-639-1 and is "Supported by gpt-transcribe". — [OpenAI API reference: create transcription](https://developers.openai.com/api/reference/resources/audio/subresources/transcriptions/methods/create)
- OpenAI's guide lists whisper-1, gpt-4o-transcribe, gpt-4o-mini-transcribe, gpt-4o-transcribe-diarize and the `/v1/audio/transcriptions` endpoint as deprecated, shutting down **February 26, 2027**, and recommends gpt-transcribe for new work. — [OpenAI speech-to-text guide](https://developers.openai.com/api/docs/guides/speech-to-text)

**ElevenLabs Scribe (v1 / v2 / v3)**
- The docs claim 90+ languages and place Hebrew (heb) in the "Good" tier (>10% to ≤20% WER). Yiddish was not found in the tier excerpts. — [ElevenLabs STT docs](https://elevenlabs.io/docs/overview/capabilities/speech-to-text)
- GitHub issue #508 (opened 2026-11-24) reports that with the language set to "Yiddish (YID)" in Scribe Live in the dashboard, transcripts come back in Latin transliteration instead of Hebrew-script Yiddish. The reporter says romanized output is unusable for native readers, search, subtitles and NLP. No maintainer response or fix was visible. — [elevenlabs-js issue #508](https://github.com/elevenlabs/elevenlabs-js/issues/508)
- A separate open issue asks for native Yiddish TTS and voice cloning. — [elevenlabs-js issue #464](https://github.com/elevenlabs/elevenlabs-js/issues/464)
- Scribe v3 launched 2026-11-10. Sources disagree on coverage: "102 languages" (ElevenLabs' FLEURS-based accuracy benchmark) versus "90+" (docs and the v3 Live page). — [i-scoop](https://www.i-scoop.eu/elevenlabs-scribe-v3/); [ai-tldr](https://ai-tldr.dev/models/scribe-v3/); [intheworldofai](https://intheworldofai.com/p/elevenlabs-scribe-v3); [ElevenLabs realtime page](https://elevenlabs.io/realtime-speech-to-text)

**AssemblyAI**
- An older support article lists Yiddish among the 99 languages of the "Universal" model. — [AssemblyAI support: languages](https://support.assemblyai.com/articles/9106299104-what-languages-do-you-support-)
- This is contradicted by the code-switching doc, whose universal-2 language table ends at Welsh without Yiddish. — [AssemblyAI code switching](https://www.assemblyai.com/docs/pre-recorded-audio/code-switching)
- Universal-3.5 Pro (async) covers 18 languages (19 in the Dictation API section). Universal-3.6 Pro Realtime covers 32. Yiddish is in neither. Languages outside the Universal-3.5 Pro set fall back to Universal-2 (99 languages). — [AssemblyAI pricing reference](https://www.assemblyai.com/pricing.md); [Universal-3.6 Pro Realtime languages](https://www.assemblyai.com/blog/universal-3-6-pro-realtime-supported-languages)
- A marketing page titled "Yiddish Speech-to-Text API" exists. It points to Universal-3.5 Pro, which does not list Yiddish, so it looks like a programmatic SEO page. — [AssemblyAI Yiddish page](https://www.assemblyai.com/languages/yiddish)

**Gladia**
- The official supported-languages table lists Yiddish (`yi`) as supported on Solaria-1, not on Solaria-3. Auto language discovery, code-switching and translation are marked available for Yiddish. — [Gladia supported languages](https://docs.gladia.io/chapters/language/supported-languages)
- Solaria-1 covers 100+ languages in real-time and async. Solaria-3 is async-only and built for core European languages. — [Gladia supported languages](https://docs.gladia.io/chapters/language/supported-languages)

**Deepgram**
- In February 2026 Deepgram released Nova-3 monolingual models for Hebrew (`he`), Farsi and Urdu. They support streaming and batch, and "all support Keyterm Prompting". — [Deepgram changelog 2026-02-03](https://developers.deepgram.com/changelog/2026/2/3); [Deepgram blog](https://deepgram.com/blog/speech-to-text-for-hebrew-persian-urdu-on-nova-3)
- Hebrew support was added to self-hosted Nova-3 in March 2026. — [Deepgram changelog 2026-03-05](https://developers.deepgram.com/changelog/2026/3/5)
- No Yiddish was found. A secondary Nova-3 language list includes Hebrew but not Yiddish. — [aiwiki: Deepgram Nova-3](https://aiwiki.ai/wiki/deepgram_nova_3)
- Deepgram also sells "Whisper Cloud", a hosted Whisper Large. — [Developers Digest: Deepgram pricing](https://www.developersdigest.tech/models/speech/deepgram)

**Speechmatics**
- Hebrew is among 55+/56+ supported languages. Yiddish was not found in any list. — [Speechmatics AI info](https://www.speechmatics.com/ai-info); [Speechmatics languages docs](https://docs.speechmatics.com/speech-to-text/languages)

**Microsoft Azure AI Speech**
- No Yiddish speech-to-text locale was found, and he-IL appears in the fast-transcription table. Azure's separate Language (text-analytics) service does list Yiddish, but that is not speech recognition. — [Azure Speech language support](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support); [Azure Language service support](https://learn.microsoft.com/en-us/azure/ai-services/language-service/concepts/language-support)
- Azure LLM Speech supports 50 languages; the list was not seen. — [Azure LLM Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/llm-speech)
- MAI-Transcribe-2 supports 60 languages; its table was not seen. MAI-Transcribe-1.5 covers 43 languages. MAI-Transcribe-1 covers 25 languages (Hebrew and Yiddish not among them) and is deprecated as of 2026-08-20. — [Azure MAI-Transcribe docs](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/mai-transcribe); [MAI-Transcribe-1.5 model card](https://microsoft.ai/pdf/MAI-Transcribe-1.5-Model-Card.PDF)

**AWS Transcribe**
- Hebrew (he-IL) is supported for batch and streaming, but not custom language models, redaction or Call Analytics. No Yiddish entry was found. — [Amazon Transcribe supported languages](https://docs.aws.amazon.com/transcribe/latest/dg/supported-languages.html) (excerpt seen via [China-region mirror](https://docs.amazonaws.cn/en_us/transcribe/latest/dg/supported-languages.html))
- Hebrew streaming was added in October 2024. — [AWS What's New, Oct 2024](https://aws.amazon.com/about-aws/whats-new/2024/10/amazon-transcribe-streaming-transcription-additional-languages)

**Rev.ai**
- Third-party sources give 31, 36+ or 58+ languages. None names Hebrew or Yiddish. Confidence is low because no official page was seen. — [Brass Transcripts on Rev.ai](https://brasstranscripts.com/blog/rev-ai-pricing-per-minute-2025-better-alternative); [hokai.io](https://hokai.io/hub/tools/revai)

**Soniox**
- 60+ languages, with Hebrew (`he`) listed. Yiddish was not found. The full list is retrievable through the Get models endpoint. — [Soniox supported languages](https://soniox.com/docs/stt/concepts/supported-languages)

**Mistral Voxtral (API)**
- Voxtral Mini Transcribe V2 covers 13 languages: English, Chinese, Hindi, Spanish, Arabic, French, Portuguese, Russian, German, Japanese, Korean, Italian and Dutch. Neither Hebrew nor Yiddish is included. — [Mistral: Voxtral Transcribe 2](https://mistral.ai/news/voxtral-transcribe-2/)
- Voxtral 3 claims 50+ languages, but the full list was not seen. — [Mistral: Voxtral 3](https://mistral.ai/news/voxtral-3/)

**Alibaba Qwen3-ASR**
- The Model Studio cloud models (qwen3-asr-flash, -filetrans, -flash-realtime) cover Chinese dialects, English, Japanese, German, Korean, Russian, French, Portuguese, Arabic, Italian, Spanish, Hindi, Indonesian, Thai, Turkish, Ukrainian, Vietnamese, Czech, Danish, Filipino, Finnish, Icelandic, Malay, Norwegian, Polish and Swedish. There is no Hebrew or Yiddish. — [Model Studio ASR models](https://www.alibabacloud.com/help/en/model-studio/asr-model)
- The open-source Qwen3-ASR claims 52 languages and dialects. — [QwenLM/Qwen3-ASR](https://github.com/QwenLM/Qwen3-ASR)

**Whisper hosts and a hosted Yiddish-tuned Whisper**
- Groq hosts whisper-large-v3 and whisper-large-v3-turbo. — [Developers Digest: Groq](https://www.developersdigest.tech/models/speech/groq)
- Together hosts Whisper Large v3. — [Together: Whisper Large v3](https://www.together.ai/models/openai-whisper-large-v3)
- Fireworks hosts whisper-v3 and whisper-v3-turbo. — [Perkstack on Fireworks Whisper](https://perkstack.co/blog/cheapest-fireworks-whisper-v3-api)
- ivrit.ai's `yi-whisper-large-v3` is a Yiddish continued-training fine-tune of large-v3, released around October 2025. Its "language token should be explicitly set to Yiddish". — [HF: ivrit-ai/yi-whisper-large-v3](https://huggingface.co/ivrit-ai/yi-whisper-large-v3)
- FriendliAI lists an API inference endpoint for that model; pricing was not checked. This overlaps with the open-source research scope. — [FriendliAI: yi-whisper-large-v3](https://friendli.ai/models/ivrit-ai/yi-whisper-large-v3)

### Inferences
- **Strict "officially listed in a speech-to-text language table"**: only Gladia Solaria-1, plus the Whisper family by virtue of its `yi` token. Gemini's listing is for the model family generally, so it signals that the LLM "knows" Yiddish, not that audio transcription has been validated.
- **The `yi` token is not a quality signal.** OpenAI's own <50% WER filter appears to exclude Yiddish (unconfirmed). The same Whisper weights back Groq, Fireworks, Together and Deepgram Whisper Cloud, so all of them inherit whatever Whisper's Yiddish quality is.
- **ElevenLabs**: a Yiddish path exists ("YID" is selectable), but Latin-letter output is a blocker for a Hebrew-script website until fixed. It might still feed an LLM summarizer, but that is speculative and worth a quick test.
- **Google's dedicated ASR products** (Chirp 3 and Gemini 3.5 Transcribe) have similar "85+ language" sets that both appear to lack Yiddish. For Google, the realistic Yiddish route is prompting a general Gemini model with audio.
- Hebrew is broadly supported (Deepgram Nova-3, Soniox, ElevenLabs, Google, AWS, Azure, Speechmatics). A Hebrew model run on Yiddish audio would still be a mismatch: same script, different language.

### Gaps
- Several full language tables could not be opened: Google V2, ElevenLabs, AssemblyAI Universal-2, Azure (incl. MAI-Transcribe-2 and LLM Speech), Voxtral 3 and the full Soniox list. For these, "not found" ≠ "not supported".
- Not confirmed whether Yiddish is on OpenAI's published supported-language list, or whether gpt-transcribe accepts `language=yi`.
- Not confirmed whether AssemblyAI Universal-2 currently accepts `language_code="yi"`.
- Not confirmed whether ElevenLabs batch Scribe v2/v3 accepts `yid` through the API, or which script it outputs. The bug report concerns the dashboard's Scribe Live.
- Not confirmed whether Gemini 3.5 Transcribe returns usable Yiddish when no language code is set.

## 2. Published Yiddish quality numbers, Hebrew handling, and anecdotal reports

### Takeaway
No mainstream vendor publishes a Yiddish word-error rate (WER). There is no standard Yiddish ASR benchmark: Common Voice has only about 1.7 h of Yiddish, and Yiddish membership in FLEURS was not confirmed. The hard numbers come from research and fine-tuning projects:
- A **Yiddish-adapted Whisper large-v3 scored ~57% WER** on the Lubavitcher Rebbe's farbrengen recordings (Hasidic register) before fine-tuning, and 8.1% WER after.
- A dedicated CTC system on Holocaust-testimony Yiddish reached about 38% WER.

Off-the-shelf mainstream ASR should therefore be expected to perform poorly on Hasidic yeshivish Yiddish. No public test of Gemini or GPT on Yiddish audio was found. Hebrew, by contrast, is well supported by many vendors.

### Cited Findings

**Benchmarks and datasets**
- Common Voice Scripted Speech 23.0, Yiddish (`yi`): 1,849 clips, about 1.7 h, all validated, 95 speakers, CC-0. — [Mozilla Data Collective listing](https://datacollective.mozillafoundation.org/datasets/cmflnuzw7mguohrskxmzwypsx)
- FLEURS is an n-way parallel speech dataset in 102 languages built on FLoRes-101, with about 12 h of supervision per language. The full list is in Table 9 of the paper; whether Yiddish is included was not confirmed from the excerpts. — [FLEURS paper, arXiv:2205.12446](https://arxiv.org/abs/2205.12446)
- The Whisper paper notes that some FLEURS languages had no Whisper training data. Yiddish's training-hour count was not found in the excerpts. — [Whisper paper](https://cdn.openai.com/papers/whisper.pdf)
- ElevenLabs' Scribe v3 claim of about 27% fewer word errors than v2 "across 102 languages" uses ElevenLabs' own version of FLEURS (a vendor figure). — [DataNorth](https://datanorth.ai/news/elevenlabs-scribe-v3-claims-27-fewer-transcription-errors-than-v2)

**Yiddish-specific numbers (all self-reported or academic, none on commercial APIs)**
- **Rebbe's farbrengens (Chabad Hasidic Yiddish):** the README says "off-the-shelf speech recognition, including Whisper models adapted to Yiddish, transcribes this speaker and this register badly". The baseline was ivrit.ai's Yiddish-adapted large-v3 (not stock OpenAI) at about 0.57 whole-recording WER. The best recipe, a full fine-tune of large-v3-turbo (learning rate 3e-5, 4 epochs, Daily Sicha archive aligned to published hanachos), reached 0.081 WER and 0.045 CER on the first dataset. — [golanbenoni/rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
- **Whisper-small Yiddish fine-tune:** 83.5% of words and 92.6% of letters correct on a tiny private set (about 10 clips, about 2 minutes, one speaker; the author says ±3 points is noise). On a broader public eval split, 62.8% of words were correct. — [HF: akiva-yiddish/yiddish-whisper-phone](https://huggingface.co/akiva-yiddish/yiddish-whisper-phone)
- **Bleaman, LREC 2026 HTRes workshop:** presented as the first ASR for European (Northeastern-dialect) Yiddish testimony. Best mean WER was 37.96%, using a Hebrew-based phonemic transcript with precomposed Unicode. The same audio scored 59.40% with romanized Yiddish and 99.67% with standard spelling in decomposed Unicode. Cross-domain testing on Yiddish audiobooks gave 27.07% WER and 6.56% CER. The proceedings say this WER is too high for production without significant correction. — [ACL Anthology 2026.htres-2.3](https://aclanthology.org/2026.htres-2.3/); [LREC 2026 HTRes PDF](http://www.lrec-conf.org/proceedings/lrec2026/workshops/htres/pdf/2026.htres-1.3.pdf)
- **Holocaust testimony, not Yiddish (for contrast):** Whisper averaged 15.28% WER across 1,847 Fortunoff testimonies, which are mostly not in Yiddish. — [ACL Anthology 2026.htres-2.10](https://aclanthology.org/2026.htres-2.10/)
- **Vendor marketing claim, methodology not published:** a Yiddish transcription vendor advertises 95%+ word accuracy on standard Yiddish and about 90% on clear Hasidic speech. — [speechyou.com](https://speechyou.com/yiddish-hebrew-script-transcription)
- **Gemini and Yiddish handwriting, not audio:** a blogger found Gemini 3 Pro can read handwritten mid-20th-century Yiddish, "though not yet with human level accuracy". — [Generative Lives (Substack)](https://generativelives.substack.com/p/mid-c20th-yiddish-htr)
- A Yiddish language-model benchmark (MameLoshnLM) exists. It was not reviewed here, but it may matter for judging summarization quality. — [arXiv 2608.05850](https://arxiv.org/html/2608.05850)

**Hebrew handling (relevant to loshon-kodesh and Aramaic vocabulary)**
- **ElevenLabs:** the docs put Hebrew in the "Good" tier (>10% to ≤20% WER); an older English page used a >10% to ≤25% band. ElevenLabs' Hebrew landing page claims 3.1% WER on FLEURS and 5.5% on Common Voice, which conflicts with the tier. — [ElevenLabs STT docs](https://elevenlabs.io/docs/overview/capabilities/speech-to-text); [ElevenLabs Hebrew page](https://elevenlabs.io/speech-to-text/hebrew)
- **Deepgram:** Nova-3 Hebrew monolingual model (February 2026) with keyterm prompting. — [Deepgram blog](https://deepgram.com/blog/speech-to-text-for-hebrew-persian-urdu-on-nova-3)
- **Soniox:** Hebrew (`he`). — [Soniox languages](https://soniox.com/docs/stt/concepts/supported-languages)
- **Google:** Hebrew `iw-IL` on Chirp 2 and Chirp 3; Hebrew `he-IL` on Gemini 3.5 Transcribe, marked "Experimental" on Cloud. — [Google STT languages](https://docs.cloud.google.com/speech-to-text/docs/speech-to-text-supported-languages); [Cloud: Gemini 3.5 Transcribe](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-transcribe)
- **AWS:** he-IL, batch and streaming, no custom language models. — [AWS supported languages](https://docs.aws.amazon.com/transcribe/latest/dg/supported-languages.html)
- **Azure:** he-IL is listed for fast transcription. — [Azure language support](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support)
- **Speechmatics:** Hebrew is listed. — [Speechmatics AI info](https://www.speechmatics.com/ai-info)
- ivrit.ai runs a Hebrew transcription leaderboard on Hugging Face; its commercial-API rows were not retrieved. — [ivrit.ai spaces](https://huggingface.co/ivrit-ai/spaces)
- Open-model reference points on Hebrew: ivrit-ai whisper-large-v3-turbo scores 5.3 (eval-d1) and 7.1 (eval-whatsapp); Hebrish Zipformer scores 5.1 and 8.9. — [HF: Hebrish Zipformer2-66M](https://huggingface.co/Hebrish/Zipformer2-66M-Streaming-He)

### Inferences
- **Stock Whisper is likely worse than ~57% WER here.** Even a Yiddish-adapted Whisper scored ~57% on Hasidic farbrengen audio, so stock Whisper and every Whisper host are likely worse on Hasidic yeshivish Yiddish. Treat them as "noisy draft" quality that a summarizer might or might not tolerate. This is speculative and needs a pilot.
- **Hebrew-strong ASR engines are not a substitute.** They would probably render loshon-kodesh terms well but mangle Yiddish morphology and grammar.
- **Gemini may handle Talmudic vocabulary better, but this is unproven.** General LLMs (Gemini) bring world knowledge of Talmudic terms, sefarim names and rabbinic names, and can take a glossary in the prompt. That plausibly helps yeshivish content, but no public evidence exists.
- **Evaluate with normalized text.** The Bleaman results (99.67% vs 37.96% WER from Unicode composition and orthography alone) show that spelling conventions and Unicode normalization can dominate measured error. Any pilot should apply NFC normalization, tolerate Hasidic versus YIVO spelling, and score CER plus "heading and timestamp correctness" rather than raw WER.

### Gaps
- No WER for any commercial API (Gemini, ElevenLabs, Gladia, AssemblyAI, OpenAI, Whisper hosts) on Yiddish of any register.
- No independent reports found on mainstream APIs with Hasidic or yeshivish audio. Reddit was blocked; the searches found none on other sites.
- Whisper's Yiddish training hours and Yiddish's FLEURS membership were not confirmed.
- Artificial Analysis and Hugging Face Open ASR leaderboards were not retrieved, though they are unlikely to include Yiddish.
- The ivrit.ai Hebrew leaderboard's commercial-API rows were not retrieved.

## 3. Features that matter: timestamps, long files, diarization, vocabulary/context hints, code-switching, batch vs realtime, data use

### Takeaway
For this use case, segment-level or minute-level timestamps are enough; word-level timestamps are not required. The feature options split into four groups:
- **General Gemini models** take hours of audio per request and any free-form glossary in the prompt, but timestamps are just generated text and are known to drift.
- **Gemini 3.5 Transcribe** gives real word timestamps, but limits requests to 1 h (30 min with word timestamps), cannot combine custom vocabulary with word timestamps, and does not list Yiddish.
- **Whisper-family APIs** return segment timestamps (confirmed here for whisper-1). OpenAI's new gpt-transcribe launched without timestamps.
- **Keyterm features** (ElevenLabs, AssemblyAI, Deepgram) are capped at 200–1,000 terms and cost about $0.05/h extra.

### Cited Findings

**Gemini general models (prompted transcription)**
- Audio can be referenced with MM:SS timestamps in the prompt, and the docs include a transcription-with-timestamps example. — [Gemini audio understanding](https://ai.google.dev/gemini-api/docs/audio)
- Up to 9.5 h of audio per prompt. — [Gemini audio understanding](https://ai.google.dev/gemini-api/docs/audio)
- Inline requests are capped at 20 MB in total; larger files go through the Files API. — [Gemini audio understanding](https://ai.google.dev/gemini-api/docs/audio)

**Gemini 3.5 Transcribe**
- Recorded mode has word-level timestamps. Live streaming has only utterance-level timestamps. — [Cloud: Gemini 3.5 Transcribe](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-transcribe)
- Up to 1 h per request, or 30 minutes when speaker diarization or word-level timestamps are enabled. Google warns that enabling word-level timestamps "may degrade overall transcription accuracy". — [Gemini API audio transcription](https://ai.google.dev/gemini-api/docs/transcribe); [The Rundown summary](https://www.therundown.ai/tools/gemini-3-5-transcribe)
- Custom vocabulary of up to 1,000 phrases biases recognition toward domain terms and names. It is incompatible with word timestamps and with speaker diarization. — [Gemini API audio transcription](https://ai.google.dev/gemini-api/docs/transcribe)
- "Smart mode" cleanup cannot be combined with diarization or word timestamps. — [Cloud: Gemini 3.5 Transcribe](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-transcribe)
- Live sessions are limited to 10 minutes. — [The Rundown](https://www.therundown.ai/tools/gemini-3-5-transcribe)
- Status conflicts: the Gemini API docs say public preview, while a listing claims general availability since 2026-08-26. — [Gemini 3.5 Transcribe model page](https://ai.google.dev/gemini-api/docs/models/gemini-3.5-transcribe); [ProductBuilder](https://www.productbuilder.net/models/gemini-3-5-transcribe)
- A Japanese guide on note.com reports a 15-minute limit on the enterprise platform, which conflicts with the 1 h figure. The excerpt did not show which of two note.com guides made the claim. — [note.com guide A](https://note.com/ai_driven/n/n4e09c9f54040) / [note.com guide B](https://note.com/ai__worker/n/nf60bcbdb3ef4)
- Multi-speaker attribution is supported, but support for 3+ speakers is experimental. — [Google blog: Gemini 3.5 Transcribe](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-5-transcribe/)
- The API syntax differs between docs: the newer docs use `transcription_config`, while the legacy generate-content page uses `audioTranscriptionConfig`. — [Gemini API transcribe (legacy)](https://ai.google.dev/gemini-api/docs/generate-content/transcribe)

**Google Chirp 3**
- BatchRecognize suits long audio, generally 1 minute to 1 hour, but only up to 20 minutes with word-level timestamps enabled. — [Chirp 3 model page](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3)
- Diarization works only in BatchRecognize and Recognize, for a limited set of languages. — [Chirp 3 model page](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3)

**OpenAI**
- gpt-transcribe launched without word-level timestamps, SRT/VTT output, speaker diarization or English translation. Those still require whisper-1 (word and segment timestamps) or gpt-4o-transcribe-diarize. — [Digital Applied comparison](https://www.digitalapplied.com/blog/openai-gpt-transcribe-stt-model-comparison-2026); [Cekura](https://www.cekura.ai/blogs/whisper-api-pricing)
- The model detects the spoken language and accepts language hints. — [gpt-transcribe.org explainer](https://gpt-transcribe.org/model/gpt-transcribe)

**ElevenLabs**
- Keyterm prompting adds about $0.05/h; transcript editing adds about $0.07/h. — [i-scoop](https://www.i-scoop.eu/elevenlabs-scribe-v3/)
- Billing is per audio minute, so silence is billed too. — [weneedai](https://weneedai.net/elevenlabs-scribe-v3-pricing/)

**AssemblyAI**
- Keyterms on Universal-3.5 Pro: up to 1,000 terms, at most six words per phrase, +$0.05/h. — [AssemblyAI pricing reference](https://www.assemblyai.com/pricing.md); [U-3.5 Pro prompting guide](https://www.assemblyai.com/blog/speech-to-text-prompting-assemblyai-universal-3-pro)
- Keyterms on Universal-2: up to 200 terms. The pricing page lists them as included, but the pricing reference says +$0.05/h. — [AssemblyAI pricing](https://assemblyai.com/pricing); [AssemblyAI pricing reference](https://www.assemblyai.com/pricing.md)
- General prompting is available only on Universal-3.5 Pro, at +$0.05/h. — [AssemblyAI pricing reference](https://www.assemblyai.com/pricing.md)

**Deepgram**
- Keyterm prompting is supported on the Nova-3 Hebrew model, for both batch and streaming. — [Deepgram blog](https://deepgram.com/blog/speech-to-text-for-hebrew-persian-urdu-on-nova-3)
- Billing is per second. — [Cartesia: Deepgram pricing](https://www.cartesia.ai/learn/deepgram-pricing)

**Gladia**
- For Yiddish on Solaria-1, auto language discovery, code-switching and translation are marked available. — [Gladia supported languages](https://docs.gladia.io/chapters/language/supported-languages)

**Soniox**
- Language identification is included in the base rate. — [Soniox pricing](https://soniox.com/pricing)
- Automatic language detection and code-switching are offered. — [Soniox language detection](https://soniox.com/speech-to-text/features/language-detection)

**Groq**
- Each request is billed for at least 10 seconds. — [costperoutput](https://costperoutput.com/groq-whisper-pricing)
- The free tier allows up to 7,200 audio seconds per hour and 28,800 per day for each Whisper model. — [costperoutput](https://costperoutput.com/groq-whisper-pricing)
- The Batch API is reportedly 50% cheaper. — [Developers Digest: Groq](https://www.developersdigest.tech/models/speech/groq)

**Speechmatics**
- Billed per second. — [Speechmatics pricing.md](https://www.speechmatics.com/pricing.md)
- Add-ons: translation $0.65/h, summaries $0.12/h. — [usagepricing: Speechmatics](https://usagepricing.com/blueprint/speechmatics)

**AWS Transcribe**
- Minimum billing per request conflicts: 15 s on the AWS China page versus 60 s according to CostGoat. — [AWS China pricing](https://amazonaws.cn/en/transcribe/pricing); [CostGoat](https://costgoat.com/pricing/amazon-transcribe)

**Data use and retention**
- **Gemini API:** the official pricing table marks free-tier usage "Used to improve our products: Yes" and paid-tier usage "No". The exception is the paid "Contributor tier", which trades discounted tokens for data use on select models such as 3.1 Pro Preview. — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- A forum post quoting the terms says that once Cloud Billing is activated, all use counts as a "Paid Service". — [Google AI forum](https://discuss.ai.google.dev/t/clarification-on-gemini-api-free-tier-vs-paid-tier-after-billing-activation/96995)
- Logging is available only on billing-enabled projects, and logged prompts and responses are not used for product improvement by default. — [Gemini API logs policy](https://ai.google.dev/gemini-api/docs/logs-policy)
- **Google Cloud STT:** customer audio and transcripts are not logged by default; an opt-in data-logging program gives a discount, and opted-in data is used to train Google products. Async transcripts are stored about 5 days; input audio is not stored. — [Cloud STT data usage FAQ](https://docs.cloud.google.com/speech-to-text/docs/data-usage-faq)
- **Azure Speech:** real-time data is not retained. Batch mode uses customer-specified storage, and transcript retention is set with `timeToLive`. — [Azure STT data privacy](https://learn.microsoft.com/legal/cognitive-services/speech-service/speech-to-text/data-privacy-security)

### Inferences
- **Timestamp needs are coarse.** The team cuts clips a few minutes long and needs headings accurate to the moment of a topic change, so roughly ±10–30 s is probably enough; word-level timestamps are not needed. Two approaches meet this cheaply. The first is any API with segment timestamps (Whisper family). The second is a general LLM fed short chunks whose offsets are computed in code (see Q5).
- **Prompts are the most flexible way to inject vocabulary.** A Gemini prompt glossary of yeshivish terms, sefarim, rabbinic names and the speaker's name has no hard term limit and costs only text-input tokens. ASR keyterm features cap at 200–1,000 phrases and cost about $0.05/h extra.
- **Data-use policy is low-stakes here** because the source lectures are already public on websites. Still, avoid the Gemini free tier unless the team accepts Google using the data.
- **Realtime is not needed** (batch workflow). Use batch or flex tiers where they exist for about 50% savings (Gemini, Groq).

### Gaps
- ElevenLabs Scribe v3 file-size and duration limits, word timestamps and diarization were not re-verified this session.
- OpenAI's upload limit (25 MB, which is widely known) and gpt-transcribe's maximum duration were not re-verified.
- Gladia's custom vocabulary, timestamp granularity and maximum file length were not verified.
- Groq, Fireworks and Together file-size limits and timestamp output formats were not verified.
- Data-retention and training policies for ElevenLabs, AssemblyAI, Gladia, OpenAI, Deepgram, Soniox and Speechmatics were not verified.

## 4. Pricing per audio hour and cost at ~433 h/month

### Takeaway
Every mainstream option is cheap at this volume, a fraction of specialized-vendor pricing.

Yiddish-capable or plausible routes, monthly cost at 433 h:
- **Whisper on Groq, Fireworks or Together:** about **$17–48/month**. Cheapest, but quality is likely poor.
- **Gemini Flash-Lite class:** roughly **$10–45/month**, depending on model and output assumptions.
- **Gemini 3.5 Flash:** about **$86/month** (batch/flex) to **$172/month** (standard).
- **ElevenLabs Scribe v3:** about **$87–108/month**.
- **OpenAI gpt-transcribe:** about **$117/month**.
- **Gladia Solaria-1:** about **$134/month**.
- **OpenAI whisper-1:** about **$156/month**; deprecated, shuts down 2027-02-26.

The vendors without Yiddish span $43/month (Soniox) to about $624/month (AWS tier-1 list price). The price spread therefore matters much less than Yiddish quality and timestamp reliability.

### Master comparison table A: Yiddish support and features
| API / model | Yiddish status | Hebrew | Timestamps and length limits | Vocabulary / context hints | Key sources |
|---|---|---|---|---|---|
| Gladia Solaria-1 (async) | **Yes, `yi` in official table**; code-switching and translation marked available | not checked (100+ languages) | not verified | not verified | [Gladia languages](https://docs.gladia.io/chapters/language/supported-languages) |
| Gemini general models (3.x / 3.5 Flash / Flash-Lite, prompted) | **Yiddish (yi) in Gemini language list** (model-general, not audio-validated) | Yes (iw) | Text-generated timestamps only; drift reported; 9.5 h audio per prompt; 20 MB inline, Files API above that | Free-form prompt glossary, no hard cap | [Vertex models](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models); [Gemini audio](https://ai.google.dev/gemini-api/docs/audio) |
| Gemini 3.5 Transcribe | Not found in 85+ list | he-IL ("Experimental" on Cloud) | Word-level timestamps; 1 h per request (30 min with word timestamps or diarization) | Custom vocabulary ≤1,000 phrases (not usable with word timestamps or diarization) | [Gemini transcribe](https://ai.google.dev/gemini-api/docs/transcribe) |
| OpenAI whisper-1 | `yi` token accepted; probably not on published <50%-WER list (unconfirmed) | not verified | Word and segment timestamps; deprecated, shutdown 2027-02-26 | not verified | [OpenAI guide](https://developers.openai.com/api/docs/guides/speech-to-text); [tokenizer](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py) |
| OpenAI gpt-transcribe | Unknown (no official count) | unknown | No word timestamps, SRT/VTT or diarization at launch | language hints | [Digital Applied](https://www.digitalapplied.com/blog/openai-gpt-transcribe-stt-model-comparison-2026) |
| ElevenLabs Scribe v3 | "YID" selectable in dashboard, but **romanized output bug** (2026-11-24); not in tier list | "Good" tier (10–20% WER) | not re-verified | Keyterm prompting (+~$0.05/h) | [issue #508](https://github.com/elevenlabs/elevenlabs-js/issues/508); [docs](https://elevenlabs.io/docs/overview/capabilities/speech-to-text) |
| AssemblyAI Universal-2 | **Conflicting**: old 99-language list yes, universal-2 table no | not verified | not verified | Keyterms ≤200 | [support list](https://support.assemblyai.com/articles/9106299104-what-languages-do-you-support-); [code-switching doc](https://www.assemblyai.com/docs/pre-recorded-audio/code-switching) |
| AssemblyAI Universal-3.5 Pro | No (18 languages) | not verified | not verified | Keyterms ≤1,000; prompting | [pricing.md](https://www.assemblyai.com/pricing.md) |
| Whisper on Groq / Fireworks / Together / Deepgram Whisper Cloud | Via Whisper `yi` token (inferred) | via Whisper | Groq bills at least 10 s per request | not verified | [Groq](https://costperoutput.com/groq-whisper-pricing); [Together](https://www.together.ai/models/openai-whisper-large-v3) |
| Deepgram Nova-3 | Not found | **Yes, Hebrew monolingual (Feb 2026)** | batch and streaming | Keyterm prompting | [Deepgram blog](https://deepgram.com/blog/speech-to-text-for-hebrew-persian-urdu-on-nova-3) |
| Google Chirp 3 / Chirp 2 | Not found | iw-IL | Chirp 3 batch ≤1 h (≤20 min with word timestamps) | not verified | [Google languages](https://docs.cloud.google.com/speech-to-text/docs/speech-to-text-supported-languages) |
| Soniox | Not found (60+ languages) | Yes (`he`) | not verified | language ID and code-switching | [Soniox languages](https://soniox.com/docs/stt/concepts/supported-languages) |
| Speechmatics | Not found | Yes | not verified | not verified | [Speechmatics](https://www.speechmatics.com/ai-info) |
| Azure Speech (batch / fast / LLM Speech / MAI-Transcribe) | Not found | he-IL | not verified | Custom Speech | [Azure language support](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support) |
| AWS Transcribe | Not found | he-IL (no custom language model) | not verified | not verified | [AWS languages](https://docs.aws.amazon.com/transcribe/latest/dg/supported-languages.html) |
| Rev.ai | Not found | Not found | — | — | [Brass](https://brasstranscripts.com/blog/rev-ai-pricing-per-minute-2025-better-alternative) |
| Mistral Voxtral V2 / 3 | Not found (V2 has 13 languages) | Not found | — | — | [Mistral](https://mistral.ai/news/voxtral-transcribe-2/) |
| Qwen3-ASR (Model Studio) | No | No | — | — | [Model Studio](https://www.alibabacloud.com/help/en/model-studio/asr-model) |

### Master comparison table B: price per audio hour and per month
Per-hour figures come from the cited list prices. The monthly (×433) and yearly (×5,200) columns are my own arithmetic. Gemini and gpt-audio rows are estimates: 115,200 audio tokens/h plus an **assumed 25,000 output tokens/h**, with no thinking tokens (see Q5).

| API / model | List price as reported (date, quality) | $/audio h | $/month @433 h | $/year @5,200 h | Yiddish? |
|---|---|---|---|---|---|
| Groq whisper-large-v3-turbo | $0.04/h (2026-10-27; trackers) | 0.040 | ~$17 | ~$208 | via `yi` |
| Fireworks whisper-v3-turbo | $0.0009/min (Fireworks launch blog; possibly dated) | 0.054 | ~$23 | ~$281 | via `yi` |
| Groq whisper-large-v3 | $0.111/h (2026-10-27); Batch reportedly −50% | 0.111 (batch ~0.056) | ~$48 (batch ~$24) | ~$577 (~$289) | via `yi` |
| Fireworks / Together whisper-large-v3 | $0.0015/min (vendor pages) | 0.090 | ~$39 | ~$468 | via `yi` |
| Deepgram Whisper Cloud | $0.0048/min (trackers 2026); LiteLLM says $0.006 | 0.288 (or 0.36) | ~$125 (or $156) | ~$1,498 | via `yi` (inferred) |
| OpenAI gpt-transcribe | $0.0045/min (launched 2026-07-28; trackers Nov 2026) | 0.270 | ~$117 | ~$1,404 | unknown |
| OpenAI whisper-1 / gpt-4o-transcribe | $0.006/min (both deprecated, shutdown 2027-02-26) | 0.360 | ~$156 | n/a after Feb 2027 | whisper `yi` |
| ElevenLabs Scribe v3 | $0.20/h batch (launched 2026-11-10); +~$0.05/h keyterms | 0.20 (0.25 with keyterms) | ~$87 (~$108) | ~$1,040 (~$1,300) | YID selectable; romanized bug |
| Gladia Solaria-1 async | €0.27/h (Gladia blog 2026) | ≈0.31 at an assumed 1.15 USD/EUR | ≈$134 (€117) | ≈$1,612 | **Yes (official)** |
| AssemblyAI Universal-2 | $0.15/h (pricing ref 2026-09-28; page ~2026-11-03) | 0.15 (0.20 with keyterms if charged) | ~$65 (~$87) | ~$780 | conflicting |
| Gemini 2.5 Flash-Lite (being retired) | audio in $0.30/M std, $0.15/M batch; output $0.40/M **(output price not re-verified)** | ~0.045 (batch ~0.022) | ~$19 (batch ~$10) | ~$232 | Gemini list |
| Gemini 3.5 Flash-Lite | $0.30/M input incl. audio (official page excerpt); output price not found | 0.035 input only, plus output | ~$15 input only (plus ~$11–32 if output costs $1–3/M) | — | Gemini list |
| Gemini 3.1 Flash-Lite | audio in $0.50/M; output $1.50/M **(assumed, unverified)** | ~0.095 | ~$41 | ~$494 | Gemini list |
| Gemini 3 Flash (Preview) | audio in $1.00/M std, $0.50/M batch; output $3.00/M **(assumed, unverified)** | ~0.19 (batch ~0.095) | ~$82 (batch ~$41) | ~$989 | Gemini list |
| Gemini 2.5 Flash (being retired) | audio in $1.00/M std, $0.50/M batch; output $2.50/M **(not re-verified)** | ~0.18 (batch ~0.089) | ~$77 (batch ~$38) | ~$924 | Gemini list |
| Gemini 3.8 Flash | $0.75/M in, $3.75/M out until 2026-12-31, then reportedly doubles (third-party; audio rate unverified) | ~0.18 → ~0.36 from 2027 | ~$78 → ~$156 | — | Gemini list (assumed) |
| Gemini 3.5 Flash | $1.50/M in incl. audio, $9.00/M out; batch/flex $0.75 / $4.50 (Cloud pricing; LiteLLM Nov 2026); OpenRouter shows $3.00/M audio | ~0.40 (batch ~0.20) | ~$172 (batch ~$86) | ~$2,069 (~$1,034) | Gemini list |
| Gemini 3.5 Transcribe | ~$0.005/min blended (Google estimate); token-billed | ~0.30 | ~$130 | ~$1,560 | not listed |
| OpenAI gpt-audio (chat completions) | $32/M audio in, $10/M text out; about 600 audio tokens/min (realtime convention, assumption) | ~1.40 | ~$607 | ~$7,290 | unknown |
| *Reference: Soniox async* | $0.10/h | 0.10 | ~$43 | ~$520 | not found |
| *Reference: Qwen3-ASR-Flash* | $0.000035/s (international/US) | 0.126 | ~$55 | ~$655 | no |
| *Reference: Google Chirp 3 (V2 standard / dynamic batch)* | $0.016/min / $0.003/min | 0.96 / 0.18 | ~$416 / ~$78 | — | not found |
| *Reference: Azure batch / fast / MAI-Transcribe-2* | $0.18/h / $0.36/h / $0.54/h (or $0.10/h promo to 2026-12-31) | 0.18 / 0.36 / 0.54 | ~$78 / ~$156 / ~$234 | — | not found |
| *Reference: AWS Transcribe batch* | $0.024/min tier 1 vs $0.006/min flat (conflict) | 1.44 or 0.36 | ~$624 or ~$156 | — | not found |
| *Reference: Deepgram Nova-3 (Hebrew/mono) PAYG / Growth / multilingual* | $0.0043 / $0.0036 / $0.0052 per min | 0.258 / 0.216 / 0.312 | ~$112 / ~$94 / ~$135 | — | not found |
| *Reference: Speechmatics standard / enhanced* | $0.24/h / $0.40/h (trackers; unconfirmed) | 0.24 / 0.40 | ~$104 / ~$173 | — | not found |
| *Reference: AssemblyAI Universal-3.5 Pro* | $0.21/h | 0.21 | ~$91 | — | no |
| *Reference: Mistral Voxtral Mini Transcribe 3 / V2* | $0.0035/min / $0.003/min | 0.21 / 0.18 | ~$91 / ~$78 | — | not found |
| *Reference: Rev.ai foreign language* | $0.005/min (third-party) | 0.30 | ~$130 | — | not found |

### Cited Findings (prices, with dates)
- **Gladia:** Solaria-1 €0.27/h async and €0.68/h real-time; Solaria-3 €0.16/h async. Prices are quoted in euros. — [Gladia blog](https://www.gladia.io/blog/multilingual-transcription-accuracy-language-coverage-code-switching); [Dograh: Gladia pricing](https://www.dograh.com/hub/blogs/gladia-pricing)
- **Soniox:** $0.10/h async and $0.12/h real-time, the same for every language. — [Soniox pricing](https://soniox.com/pricing)
- **AssemblyAI:** Universal-3.5 Pro $0.21/h and Universal-2 $0.15/h. Keyterms and prompting add $0.05/h each. Universal-3.6 Pro Realtime starts at $0.45/h. The pricing reference is dated 2026-09-28; the pricing page and blog date from about 2026-11-03. — [AssemblyAI pricing.md](https://www.assemblyai.com/pricing.md); [AssemblyAI pricing](https://assemblyai.com/pricing); [AssemblyAI pricing guide](https://www.assemblyai.com/blog/speech-to-text-api-pricing)
- **ElevenLabs Scribe v3:** $0.20/h batch and $0.22/h for v3 Live, per a summary of the API pricing page. — [ai-tldr](https://ai-tldr.dev/models/scribe-v3/)
- **ElevenLabs add-ons:** keyterms about +$0.05/h and editing about +$0.07/h. — [i-scoop](https://www.i-scoop.eu/elevenlabs-scribe-v3/)
- **OpenAI, transcription models:** gpt-transcribe $0.0045/min ($0.27/h), 25% below whisper-1's $0.006/min; gpt-4o-transcribe is also $0.006/min; gpt-live-transcribe $0.017/min. — [Cartesia](https://www.cartesia.ai/learn/openai-whisper-api-pricing); [Cekura](https://www.cekura.ai/blogs/whisper-api-pricing); [TokenMix](https://tokenmix.ai/blog/gpt-4o-transcribe-speech-to-text-api-guide-2026)
- **OpenAI, outlier claim:** $0.006/min plus a $0.001/min timestamp add-on. The source is internally inconsistent, so discount it. — [transcribeall.io](https://transcribeall.io/knowledge/gpt-transcribe_vs_whisper_cost_which_openai_transcription_model_is_cheaper_in_2026.php)
- **OpenAI gpt-audio:** audio input $32/M tokens, audio output $64/M, text $2.50/M in and $10/M out. — [OpenAI gpt-audio model page](https://developers.openai.com/api/docs/models/gpt-audio)
- **OpenAI audio token rate:** about 1 audio token per 100 ms of user speech in the Realtime API, which is about $0.02/min at $32/M. — [Micdrop](https://micdrop.dev/blog/openai-realtime-api-pricing)
- **Groq:** whisper-large-v3 $0.111/h and turbo $0.04/h (as of 2026-10-27); the Batch API is reportedly 50% cheaper. — [costperoutput](https://costperoutput.com/groq-whisper-pricing); [Developers Digest: Groq](https://www.developersdigest.tech/models/speech/groq)
- **Fireworks:** whisper-v3-large $0.0015/min and turbo $0.0009/min (Fireworks' launch-blog table); streaming $0.0035/min. — [Fireworks audio launch blog](https://fireworks.ai/blog/audio-transcription-launch); [Perkstack](https://perkstack.co/blog/cheapest-fireworks-whisper-v3-api)
- **Together:** Whisper Large v3 $0.0015/min; streaming $0.0035/min. — [Together model page](https://www.together.ai/models/openai-whisper-large-v3); [Together pricing](https://together.ai/pricing)
- **Deepgram:**
  - Nova-3 pre-recorded $0.0043/min (pay-as-you-go). — [Cartesia: Deepgram](https://www.cartesia.ai/learn/deepgram-pricing)
  - Growth plan $0.0036/min. — [Toolchase](https://toolchase.com/tool/deepgram/)
  - Multilingual $0.0052/min. — [DIYAI](https://diyai.io/ai-tools/speech-to-text/deepgram-pricing-2026/)
  - Whisper Cloud $0.0048/min. — [Developers Digest: Deepgram](https://www.developersdigest.tech/models/speech/deepgram)
  - LiteLLM lists Whisper Cloud at $0.006/min instead. — [LiteLLM: Whisper](https://models.litellm.ai/models/whisper)
- **Google Cloud STT V2:**
  - Standard $0.016/min; Chirp 3 is billed at the V2 standard rate. Dynamic Batch $0.003/min, with discounted tiers of $0.0027 and $0.0024. — [Google STT pricing](https://cloud.google.com/speech-to-text/pricing)
  - The launch blog described Dynamic Batch as 75% cheaper than standard. — [Google Cloud blog: STT V2](https://cloud.google.com/blog/products/ai-machine-learning/google-cloud-speech-to-text-v2-api)
- **Azure:**
  - Batch $0.18/h (REST v3.2+), fast transcription $0.36/h, real-time $1.00/h; Custom Speech batch $0.225/h. These come from third-party trackers, not the official page. — [Developers Digest: Azure](https://www.developersdigest.tech/models/speech/azure); [TranscribeBee](https://transcribebee.com/blog/azure-speech-to-text-pricing)
  - MAI-Transcribe-2 is listed at $0.54/h. — [tokenprice.fyi](https://tokenprice.fyi/models/azure-ai-speech)
  - Another site reports a $0.10/h preview promo through 2026-12-31. — [speechischeap](https://speechischeap.com/compare/speech-to-text-api-pricing-comparison/)
- **AWS:**
  - The official page shows tiered pricing. — [AWS Transcribe pricing](https://aws.amazon.com/transcribe/pricing/)
  - A developer resource that read the page on 2026-10-18 reports a flat $0.006/min batch rate in US East. — [Developers Digest: Amazon](https://www.developersdigest.tech/models/speech/amazon)
  - Third parties cite $0.024/min for tier 1, falling to about $0.0078/min above 5M minutes. — [CostGoat](https://costgoat.com/pricing/amazon-transcribe); [Brass](https://brasstranscripts.com/blog/amazon-transcribe-pricing-2026-cost-calculator-guide)
- **Speechmatics:**
  - $0.24/h standard and $0.40/h enhanced (unconfirmed). — [usagepricing](https://usagepricing.com/blueprint/speechmatics)
  - A June 2026 batch-only "Melia 1" multilingual model is listed at $0.129/h or $0.19/h. — [usagepricing: 2026-07-06 change](https://usagepricing.com/blueprint/activity/speechmatics-2026-07-06-price-change)
- **Rev.ai:** Reverb $0.003/min and foreign-language $0.005/min, with other figures in the same source. — [Brass](https://brasstranscripts.com/blog/rev-ai-pricing-per-minute-2025-better-alternative)
- **Mistral:**
  - Voxtral Mini Transcribe 3 $0.0035/min, Small Transcribe 3 $0.0040/min, Realtime 3 $0.008/min (public preview). — [Mistral: Voxtral 3](https://mistral.ai/news/voxtral-3/); [FourWeekMBA](https://fourweekmba.com/ai-mistrals-voxtral-3-transcribes-speech-from-0-0035-a-minute/)
  - Voxtral Mini Transcribe V2 $0.003/min. — [Mistral: Voxtral Transcribe 2](https://mistral.ai/news/voxtral-transcribe-2/)
- **Qwen3-ASR-Flash:** $0.000035/s (Singapore/US) and $0.000032/s (Beijing); output is free. A CNY price page conflicts. — [Model Studio pricing](https://www.alibabacloud.com/help/en/model-studio/model-pricing)
- **Gemini 3.5 Transcribe:**
  - Google estimates about $0.005/min blended for recorded audio and about $0.009/min live. One breakdown gives about $0.003/min for input plus about $0.002/min for output; billing is token-based. — [MLQ](https://mlq.ai/news/google-launches-gemini-35-transcribe-at-an-estimated-0005-per-minute/); [The Rundown](https://www.therundown.ai/tools/gemini-3-5-transcribe)
  - A conflicting listing claims $2/M audio-in and $12/M out. — [ProductBuilder](https://www.productbuilder.net/models/gemini-3-5-transcribe)
- **Gemini general-model prices:** see Q5.

### Inferences
- **No Yiddish-capable route is expensive at 433 h/month.** The plausible ones range from about $10 to $175/month, roughly $120–2,100/year. Even the priciest LLM route (gpt-audio, roughly $600/month, a rough estimate) is modest. The decision should rest on Yiddish quality and timestamp reliability, not price.
- **The cost advantage of a "cheap ASR then LLM summary" pipeline is small.** A Gemini summarization pass over the text transcript costs little extra (a text-input cost much smaller than audio). In practice, the transcription step dominates only for the LLM-as-transcriber route.
- **The Gladia USD figure is my estimate.** The ≈$0.31/h figure converts €0.27 at an assumed 1.15 USD/EUR, which I did not source.
- **Avoid the retiring and deprecated models.** The Gemini 2.5 models (being retired, and access restricted to existing users) and OpenAI whisper-1 / gpt-4o-* (shutdown 2027-02-26) are poor long-term choices even where they price well.

### Gaps
- None of the official pricing pages could be opened directly. Several figures come from third-party trackers, notably Azure, AWS, Speechmatics, Deepgram, Rev.ai and Gemini 3.8 Flash.
- Some third-party sites quote Gladia in USD ($0.61/h async self-serve); the source was not pinned down, and the figure looks outdated.
- gpt-4o-mini-transcribe's price ($0.003/min, widely reported) was not re-verified.
- Output-token prices for Gemini 3 Flash, 3.1 Flash-Lite and 3.5 Flash-Lite were not verified; neither were Gemini Pro-tier prices (2.5 Pro, 3.1 Pro).
- Whether Google Dynamic Batch applies to Chirp 3 is unverified, but moot without Yiddish.
- Committed-use and enterprise discounts were not researched; all vendors appear to offer them at higher volumes.

## 5. Multimodal LLMs as transcribers: audio token rates, cost, request limits, and long-audio failure modes

### Takeaway
Gemini is the only major multimodal LLM whose language list includes Yiddish. At 32 audio tokens per second, one hour of audio is 115,200 input tokens, which costs about $0.035–0.17 per hour of input depending on the model, plus output.

Long-audio use has well-documented problems:
- **Timestamp drift.** In a March 2026 test, Gemini 3 Flash ended about 157 s early on an 11:49 lecture. Later reports show drift on 3.1 Flash-Lite, 3.1 Pro and 3.5 Flash.
- **Hallucinated or incorrect timestamps** on multi-hour files.
- **Repetition loops** that exhaust the output budget and truncate the transcript.

The consistent workaround is to chunk audio (roughly 1–10 minutes per request) and compute timestamps from chunk offsets in code. The Gemini 2.5 family is being retired; the current models are 3.5 Flash, 3.5 Flash-Lite and 3.8 Flash. OpenAI's audio-in chat models cost roughly 5–10× more per hour and have no Yiddish evidence.

### Cited Findings

**Token rates and limits**
- Gemini tokenizes audio at **32 tokens per second** (1,920 per minute), with a **9.5 h** maximum per prompt. Inline requests are capped at 20 MB; use the Files API above that. — [Gemini audio understanding](https://ai.google.dev/gemini-api/docs/audio); [Gemini token counting](https://ai.google.dev/gemini-api/docs/tokens)
- For TTS output, Google's pricing page says "audio tokens correspond to 25 tokens per second". This concerns output; input remains 32/s. — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)

**Gemini prices (official pricing-page excerpts, snapshot about early November 2026, unless noted)**
- **Gemini 3 Flash Preview:** audio input $1.00/M standard, $0.50/M batch; text $0.25 in batch. — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- **Gemini 2.5 Flash:** audio batch $0.50/M, half the standard audio rate, so $1.00/M standard. — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- **Gemini 2.5 Flash-Lite:** audio batch $0.15/M (text $0.05). — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- **Gemini 3.1 Flash-Lite:** audio input $0.50/M, text $0.25/M. — [Gemini 3 developer guide](https://ai.google.dev/gemini-api/docs/gemini-3)
- **Gemini 3.5 Flash-Lite:** $0.30/M input for text, image, video and audio (standard paid tier). — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- **Gemini 3.5 Flash:**
  - $1.50/M input (text/image/video/audio), $9.00/M output, cached input $0.15/M; batch/flex input $0.75/M and output $4.50/M. — [Google Cloud agent-platform pricing](https://cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing); [LiteLLM: Gemini 3.5 Flash](https://models.litellm.ai/models/gemini-3.5-flash)
  - OpenRouter lists $3.00/M for audio input, which conflicts. — [OpenRouter: Gemini 3.5 Flash](https://openrouter.ai/google/gemini-3.5-flash)
  - The OpenRouter model slug is dated 2026-05-19. — [OpenRouter: gemini-3.5-flash-20260519](https://openrouter.ai/google/gemini-3.5-flash-20260519)
- **Gemini 3.8 Flash:**
  - Third-party trackers report $0.75/M input and $3.75/M output through 2026-12-31, doubling to $1.50 and $7.50 from 2027-01-01. — [MorphLLM: Gemini API pricing](https://www.morphllm.com/gemini-api-pricing)
  - Google's official page shows the same kind of doubling for 3.8 Flash TTS: $4.50/M audio output through 2026-12-31, then $9.00/M. — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
  - The audio-input rate for 3.8 Flash was not confirmed.
- **Gemini 4 family (unverified):** third-party trackers also list Gemini 4-family models ("Argon", "Carbon", "Neon", "Hydrogen") with launch prices that change on December 1. — [Maximem](https://www.maximem.ai/blog/gemini-api-pricing-2026)
- **Gemini 4 Argon in the wild:** a forum thread references `gemini-4-argon` and `gemini-3.8-flash` in audio use. — [Google AI forum: functionResponse audio](https://discuss.ai.google.dev/t/gemini-4-argon-audio-inside-functionresponse-has-much-worse-temporal-accuracy-than-audio-in-a-user-turn/198425)

**Gemini model availability**
- On 2026-09-18 the Gemini API changelog said access to the 2.5 models was being limited to users who had actively used them. The models are "not deprecated and will continue to be served until further notice". New projects should use 3.5 Flash-Lite or 3.8 Flash. — [Gemini API changelog](https://ai.google.dev/gemini-api/docs/changelog)
- The Google Cloud model page gives a 2.5 Flash retirement date of 2026-10-20. — [Cloud: Gemini 2.5 Flash](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/2-5-flash)
- Cloud release notes say gemini-2.5-flash and -flash-lite are "deprecated by Dec 23, 2026". — [Cloud agent-platform release notes](https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes)
- These three sources conflict.

**Data policy**
- The Gemini free tier is "used to improve our products"; the paid tier is not, except the Contributor tier. — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)

**Known problem: timestamp drift and inaccuracy (anecdotal)**
- **March 2026 bug report:** on an 11:49 Arabic lecture, Gemini 3 Flash compressed the timestamps into 0:00–9:14, ending about 157 s early. Gemini 3.1 Pro drifted backward by about 16 s. Gemini 3.1 Flash-Lite stayed within a few seconds. — [Google AI forum: progressive timestamp drift](https://discuss.ai.google.dev/t/bug-gemini-3-flash-and-3-1-pro-progressive-timestamp-drift-in-audio-transcription/129501)
- **Later replies in the same thread (May–August 2026):** the same problem reported on 3.1 Flash-Lite, 3.1 Pro Preview and 3.5 Flash. One user says drift begins around the 60-second mark. — [same thread](https://discuss.ai.google.dev/t/bug-gemini-3-flash-and-3-1-pro-progressive-timestamp-drift-in-audio-transcription/129501)
- **Gemini 3 timing complaints:** one forum post says Gemini 3 "produces nonsensical results" for subtitles and timestamps. — [Google AI forum](https://discuss.ai.google.dev/t/it-seems-gemini-3-has-lost-its-audio-data-interpretation-capabilities-especially-regarding-time/110751)
- **Sudden regression report:** SRT timestamps became "wildly inaccurate since yesterday, despite no prompt/model change". — [Google AI forum](https://discuss.ai.google.dev/t/gemini-api-srt-transcription-suddenly-broken-timestamps-are-wildly-inaccurate-since-yesterday-despite-no-prompt-model-change/111846)
- **Older models:** Gemini 2.0 and 2.5 Flash-Lite timestamps were off by several seconds. A cookbook issue about the "refer to timestamps" feature being "wildly inaccurate" on 2.0 was closed as "not planned". — [Google AI forum: forced alignment broken](https://discuss.ai.google.dev/t/timestamp-generation-forced-alignment-on-2-0-production-models-is-still-broken/79553); [google-gemini/cookbook #733](https://github.com/google-gemini/cookbook/issues/733)
- **Hallucinated timestamps:** reported on Gemini 2.0 Flash, 2.0 Flash-Lite and 1.5 Flash 002. — [deprecated-generative-ai-js #426](https://github.com/google-gemini/deprecated-generative-ai-js/issues/426); [#269](https://github.com/google-gemini/deprecated-generative-ai-js/issues/269)
- **Audio passed inside a function response** has much worse temporal accuracy than audio in a user turn (gemini-4-argon, gemini-3.8-flash). — [Google AI forum](https://discuss.ai.google.dev/t/gemini-4-argon-audio-inside-functionresponse-has-much-worse-temporal-accuracy-than-audio-in-a-user-turn/198425)
- **Simon Willison, 3h33m council meeting on Gemini 3 Pro:** the transcript "isn't close to an exact match to what was said - and the timestamps are incorrect". A Hacker News commenter suggested one-minute segments. — [Simon Willison](https://simonw.substack.com/p/trying-out-gemini-3-pro-with-audio); [Hacker News thread](https://news.ycombinator.com/item?id=45970519)
- **Workaround reported by developers:** split the audio into short clips, transcribe each, and compute timestamps in code. — [pyvideotrans #624](https://github.com/jianchang512/pyvideotrans/issues/624); see also [pyvideotrans #618](https://github.com/jianchang512/pyvideotrans/issues/618)
- **Positive counterpoint:** a reviewer found Gemini 3 Pro's transcription of a 20-minute recording "truly impressive". — [TechRadar](https://www.techradar.com/ai-platforms-assistants/gemini/i-had-a-big-audio-transcription-problem-gemini-solved-it-and-chatgpt-didnt)

**Known problem: loops, hallucination and truncation (anecdotal)**
- **Gemini 2.5 Flash on 1.5-hour files:** it "gets stuck on transcribing a single piece repeating it dozens of times", while Pro seemed to work correctly. Filler words such as "Yeah" and "Right" were repeated many times in a row. — Google AI forum threads on 2.5 Flash transcription: [inconsistent transcriptions](https://discuss.ai.google.dev/t/inconsistent-transcriptions-with-gemini-2-5-flash/105328); [Flash 2.5 transcription](https://discuss.ai.google.dev/t/gemini-flash-2-5-transcription-gemini-2-5-flash-preview-04-17/82489)
- **Google could not reproduce it:** a Google forum response said it tried 30, 60, 90 and 120-minute files on gemini-2.5-flash without reproducing the problem. The date of that reply is unclear. — same forum threads
- **Gemini 2.5 Pro:** shortening the audio "greatly reduced" hallucination, with better results from 5–10 minute parts. — [Google AI forum: multi-speaker 2.5 Pro](https://discuss.ai.google.dev/t/how-to-get-consistent-multi-speaker-transcription-output-from-gemini-2-5-pro/87391)
- **Vertex 2.5 Flash regression:** the model repeatedly emitted the literal token `[unclear]` on noisy stretches until it consumed the full `max_output_tokens` budget, leaving the transcript incomplete. — [Google AI forum: repeated unclear](https://discuss.ai.google.dev/t/flash-2-5-vertex-ai-transcription-degrades-with-repeated-unclear-exhausting-output-tokens-recent-regression/115606)
- **An open-source tool's workaround:** re-ask Gemini for half of a piece it looped on, recursively (14 → 7 → 3.5 → 1.75 minutes), and re-ask for stretches where it "went quiet" while sound was present. The commit notes one case of 1,310 repetitions at a single timestamp. — [DenisKlimenko/amanu PR #22](https://github.com/DenisKlimenko/amanu/pull/22)
- **Audio transcription "getting stuck" mid-conversation** was also reported. — [Google AI forum](https://discuss.ai.google.dev/t/audio-input-transcription-getting-stuck-in-middle-of-the-comversation/114685)

**OpenAI audio-in chat models**
- gpt-audio is priced at $32/M audio input and $64/M audio output, and is used for audio input and output in Chat Completions. — [OpenAI gpt-audio](https://developers.openai.com/api/docs/models/gpt-audio)
- The Realtime API counts about one token per 100 ms of user audio. — [Micdrop](https://micdrop.dev/blog/openai-realtime-api-pricing)

### Inferences
- **Cost formula (my arithmetic):** $/audio-hour = 0.1152 × (audio-input $/M) + (output tokens/h ÷ 10⁶) × (output $/M), plus any thinking tokens and glossary-prompt tokens.
  - **Output assumption:** about 25k output tokens per hour of speech. That rests on roughly 8–9k spoken words/h × ~2.5–3 tokens per Hebrew-script word, plus timestamp markup. It is an unverified assumption. At 50k tokens/h, Gemini 3.5 Flash standard would be about $0.62/h, about $270/month.
  - **Headings-only output** (about 3k tokens/h) drops 3.5 Flash to about $0.20/h standard and $0.10/h batch.
- **Timestamp-safe chunking pattern for headings within ±30 s:**
  - Cut audio into fixed chunks, e.g. 2–5 minutes with about 5–10 s overlap, and send each with its absolute start offset.
  - Ask for chunk-relative times, or none at all, and add the offset in code.
  - This bounds any drift to within a chunk, avoids the 1.5-hour looping failure mode, and keeps each response far below output-token limits.
  - Overhead is small: a 1–2k-token glossary × 12–30 chunks/h ≈ 12k–60k text tokens/h, about $0.02–0.09/h on 3.5 Flash.
- **Thinking settings:** for transcription, set thinking to off or minimal where the model allows it, since thinking tokens are billed as output (an assumption from general Gemini billing practice, not re-verified here). Use batch or flex for about 50% savings, since turnaround of hours is acceptable.
- **Model choice:** given the 2.5 retirement, the forward-looking candidates are 3.5 Flash-Lite (cheapest), 3.1 Flash-Lite (best timestamp fidelity in the one benchmark, though later drift reports exist), 3.8 Flash (reportedly cheap until 2026-12-31, then doubling) and 3.5 Flash (pricier, likely better Yiddish). Run a pilot on the client's own shiurim before choosing.
- **Gemini 3.5 Transcribe is worth a one-hour pilot** despite not listing Yiddish. It returns real word-level timestamps, which would solve drift if its Yiddish (or Hebrew-biased) output is intelligible. Its 30-minute limit with word timestamps means chunking anyway, and custom vocabulary cannot be combined with word timestamps.
- **A hybrid can separate timing from content.** A cheap Whisper-family pass (Groq turbo about $17/month) supplies segment-level timing anchors, even if its Yiddish text is poor. Gemini supplies the content and headings, and the two are reconciled by time. Whether this beats chunked Gemini alone is a pipeline-design question for the pipeline researcher.

### Gaps
- No public test of Gemini, or any LLM, on Yiddish speech audio, Hasidic or otherwise, was found.
- Gemini maximum output tokens per response (believed to be 65,536 for recent Flash and Pro models) were not verified. Output prices for 3 Flash, 3.1 Flash-Lite and 3.5 Flash-Lite were not verified, nor were Pro-tier prices.
- Whether Gemini 3.x and newer models still use 32 tokens/s for audio input: the docs state 32/s generally, not per model.
- Whether Google ever acknowledged or fixed the timestamp-drift bug (no official response was found).
- gpt-audio-mini and any newer OpenAI audio-in models were not priced or tested.
- Yiddish tokenization efficiency (tokens per word) in Gemini's tokenizer was not measured; it drives output cost.
