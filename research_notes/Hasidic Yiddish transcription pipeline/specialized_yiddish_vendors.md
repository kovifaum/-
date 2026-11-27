# Specialized Yiddish / Hasidic-Yiddish / Shiur Transcription Vendors (AI and Human): inventory, accuracy, pricing at ~100 audio-hours/week

## Q1. Which products and services (2024–2026) transcribe Yiddish, specifically Hasidic/yeshivish Yiddish and shiur audio, and what is their current status?

### Takeaway
Since 2025 a small group of frum-built Yiddish speech-to-text products has appeared: YiddishLabs/SpeakInk, Yiddish Tools, Heimish Geredt, DerVort, YidKey, Devniks Voice, Red Flow, Yiddi Type and 3Yiddish. Alongside them are:
- **Platforms for mixed-language shiurim:** Sofer.ai in the US; Milai, EchoTora, Kol Kesav and HaKol Katuv in Israel.
- **Open models:** ivrit.ai's Yiddish Whisper (Oct 2025) and TorahAnytime's "yeshivish" Whisper.
- **Generic Whisper wrappers** that merely list Yiddish.

No major Jewish content platform has publicly launched transcripts or summaries of Yiddish shiurim. That includes TorahAnytime, Kol Halashon, OU/AllDaf, Dirshu, Kol Chai, Yeshiva.org.il and Chabad.org. The OU (OHRBIT) and JEM (JEMai) have launched AI layers over their own archives.

### Cited Findings

#### Conventions used throughout these notes
- **[PRIMARY]:** read on the vendor's own page or repo.
- **[VENDOR CLAIM]:** marketing copy, not independently checked.
- **[SECONDARY]:** a third-party directory, press or search-engine summary of a page.
- **[ANECDOTE]:** a forum or user report.
- **Prices:** all retrieved 2026-11-27 unless another date is given.
- **Directory snapshot:** the pinta-partners "Torah & Yiddish AI directory" file `data.json` was generated on 2026-10-21. I read it from a git clone of the public repo. It is a well-sourced secondary source; its author, Chezky Kohn, lists his own projects in it and flags them.
- **Network limits:** this session's network allowlist blocked direct fetches of www.sofer.ai and forums.jtechforums.org. Facts about those two come from search-engine extracts of the pages and should be re-checked by a human.

#### A. Yiddish ASR products built for Hasidic/"heimish" speech (commercial)
- **YiddishLabs** (yiddishlabs.com / app.yiddishlabs.com) is active.
  - Web editor plus Windows desktop app for grammar, nikud, rephrasing, "add headers" and translation. It also transcribes Yiddish audio into Yiddish or English text, and has a developer API that includes TTS. [SECONDARY] — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Operated by "Super I Technologies LLC" per the directory, which is consistent with the GitHub org "super-i-technologies" that hosts its desktop releases. — [GitHub](https://github.com/super-i-technologies/yiddish-labs-desktop-releases)
  - Sister product **SpeakInk** (speakink.co) is Yiddish/Hebrew/English voice dictation: a Windows app, an iOS beta and the Android "Speakink Keyboard". It was free during preview with a YiddishLabs account (mid-2026 thread). — [JTech SpeakInk thread](https://jtechforums.org/t/speakink-transcribe-yiddish-instantly/6503); [Speakink](https://speakink.co/)
  - Forum posters credit one founder with "SafetmMind" (sic), PointFlow AI, Yiddish Labs and SpeakInk, and mention an interview with him in the Yingerman magazine. The founder's name was not found. [ANECDOTE] — [JTech](https://jtechforums.org/t/speakink-transcribe-yiddish-instantly/6503)
- **Yiddish Tools** (yiddish.tools) is active. It is run by Hard on Soft Inc.; its Hugging Face org is "YiddishTools". It offers a web service (transcription, TTS, translation, nikud) and offline Windows programs. — [yiddish.tools](https://yiddish.tools/); [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Heimish Geredt** (heimishgeredt.com) is active per the directory.
  - Yiddish speech-to-text "built for Hasidic/heimish Yiddish": mic or upload, Windows/Android/Kyocera apps and a phone line. Also TTS (beta) and grammar correction. [SECONDARY] — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Web searches for the site returned nothing, so I could not confirm it independently.
- **DerVort** (dervort.com) is in beta. It offers hold-a-key dictation plus file transcription and claims "Heimish Yiddish"; the site banner says it is under construction. [SECONDARY] — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **YidKey** (paid website with a free tier), **Devniks Voice** (free website) and **Red Flow** (free website and app; live mic only, up to 30 s) were all tested in the Nov 2026 3Yiddish benchmark, so all three exist as of Nov 2026. — [3Yiddish BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/main/BENCHMARK.md)
- **Speechyou** (speechyou.com) is a general AI transcription site with a "Yiddish (Hebrew script)" page. It lists four dialects: Standard, Western, Eastern and Hasidic. [VENDOR CLAIM] — [Speechyou](https://speechyou.com/yiddish-hebrew-script-transcription)

#### B. Platforms built for shiurim and mixed-language Torah audio
- **Sofer.ai** (US) is active.
  - Built "for shiurim that mix English and Hebrew with Aramaic, Yiddish and Yeshivish terms". "Dedicated Yiddish transcription is also available in beta." [VENDOR CLAIM via search extract] — [sofer.ai](https://www.sofer.ai/)
  - Founder/CEO Zach Fish, a YU alum. — [YWN](https://www.theyeshivaworld.com/news/general/2378877/the-new-tech-startup-revolutionizing-how-torah-is-preserved.html); [LinkedIn](https://www.linkedin.com/in/zach-fish/); [Jewish Link](https://jewishlink.news/from-the-people-of-the-book-to-the-people-of-the-transcript-powered-by-sofer-ai/)
  - The beta was publicly praised in Aug 2024. — [Dovi Safier on X](https://x.com/safier/status/1828125920965582966); [David Bashevkin on X](https://x.com/DBashIdeas/status/1828226193448919472)
  - "Founded 2024 in Denver" per [Startup Seeker](https://startup-seeker.com/company/sofer~ai); about 4 employees per a [RocketReach](https://rocketreach.co/soferai-profile_b6edd993c6d8aba3) estimate. Both are unverified third-party profiles.
  - Conflict: the ChabadLabs AI Toolbox labels Sofer.ai "Freemium · Shluchim pick · Built by Chabad", which doesn't match the YU/Denver origin. — [ChabadLabs Toolbox](https://www.chabadlabs.ai/Toolbox)
- **Milai / מילאי** (milai.co.il, Israel) is active.
  - A Hebrew transcription service "built for Torah shiurim, podcasts and lectures". It says "one file can mix Hebrew, Yiddish, Aramaic and English", supports Ashkenazi, Sephardi and Litvish pronunciations, and recognizes names of sages and Gemara phrases.
  - The operator is not named on the site. [SECONDARY/VENDOR CLAIM] — [pinta directory, citing milai.co.il](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **EchoTora** (echotora.com, Israel) is active. It offers transcription of Torah shiurim, handwriting OCR and source identification. Yiddish support is unconfirmed. A tora-forum thread about an offline Torah transcription program has a user asking whether it handles Yiddish lectures, with no visible answer. A search summary linked that thread to EchoTora; the link is uncertain, so the thread may describe a different program. — [EchoTora](https://www.echotora.com/); [tora-forum thread](https://tora-forum.co.il/threads/%D7%91%D7%A9%D7%95%D7%A8%D7%94-%D7%90%D7%93%D7%99%D7%A8%D7%94-%D7%9C%D7%A2%D7%95%D7%A8%D7%9B%D7%99%D7%9D-%D7%AA%D7%95%D7%A8%D7%A0%D7%99%D7%99%D7%9D-%D7%AA%D7%95%D7%9B%D7%A0%D7%AA-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%9C%D7%94%D7%A8%D7%A6%D7%94-%D7%A2%D7%9C-%D7%9E%D7%97%D7%A9%D7%91-%D7%90%D7%95%D7%A4%D7%9C%D7%99%D7%99%D7%9F.69854/)
- **Kol Kesav / קול כתב** (kolkesav.com) is a "Hebrew & English Shiur Transcriber". A directory listing says you can record or upload in "English, Hebrew, Yiddish or a yeshivish mix" and that it is free to start. — [kolkesav.com](https://kolkesav.com/); [Luach listing](https://luach.com/online-businesses/kol-kesav-shiur-transcription-notes-free-to-start)
- **HaKol Katuv / הקול כתוב** (akol-catuv.web.app) offers AI transcription of Torah shiurim. It claims to be fast and free for yeshivas, publishers and content people. [VENDOR CLAIM] — [akol-catuv](https://akol-catuv.web.app/)
- **Shiur AI Assistant** is a Chrome extension that generates transcripts and notes from YUTorah and Kol Halashon shiurim. Its language handling is undocumented. — [Chrome Web Store](https://chromewebstore.google.com/detail/shiur-ai-assistant/nfkenagfgiiobeaadckifeljnndjpbdl)

#### C. Free, offline and community tools, and open models
- **Yiddi Type** ("Yiddi AI", GitHub account YankyAppel) is a free Windows program.
  - It transcribes "Yiddish recordings (shiurim, drashos, tishen) into Word and text, with sources (mareh mekomos)". Everything runs offline, and each computer gets its own free license.
  - Releases v2.5–2.6.5 came out in early Nov 2026. — [GitHub releases](https://github.com/YankyAppel/yiddi-type-releases); [v2.6.5](https://github.com/YankyAppel/yiddi-type-releases/releases/tag/v2.6.5)
- **3Yiddish** (GitHub TripleU613; fork MeilichM) is free and open, and runs offline on Linux, macOS and Windows.
  - It uses its "own [models], trained on heimish Yiddish": one Whisper-based and one built on Meta's Omnilingual.
  - v0.4.0 was committed 2026-11-18. — [3Yiddish](https://github.com/TripleU613/3Yiddish); git log of BENCHMARK.md (read from a local clone)
- **ivrit.ai Yiddish models:** `yi-whisper-large-v3` and `-turbo` were released Oct 2025 under Apache-2.0 by the Israeli nonprofit ivrit.ai. They are the de-facto open baseline most Yiddish ASR builds on. — [HF yi-whisper-large-v3](https://huggingface.co/ivrit-ai/yi-whisper-large-v3); [HF turbo](https://huggingface.co/ivrit-ai/yi-whisper-large-v3-turbo); [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **TorahAnytime's `whisper-large-v3-yeshivish`** is published on Hugging Face (org "torahanytimecom", Apache-2.0). It has a stock model card with no data or metrics. — [HF](https://huggingface.co/torahanytimecom/whisper-large-v3-yeshivish); [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Chabad community projects:**
  - *Rebbe Yiddish ASR* (Golan Benoni): Whisper fine-tuned on the Rebbe's farbrengens. — [GitHub](https://github.com/golanbenoni/rebbe-yiddish-asr)
  - *Sicha to Text*: an offline Mac app using whisper.cpp with a modified ivrit-ai yi-whisper-large-v3. — [GitHub](https://github.com/MMS-HM/sicha-to-text) (via [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory))
- **Small community fine-tunes:** `akiva-yiddish/yiddish-whisper-phone` (phone-size models) and es2814207's Whisper fine-tunes. — [HF](https://huggingface.co/akiva-yiddish/yiddish-whisper-phone); [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)

#### D. Jewish content platforms and institutional deployments
- **TorahAnytime:** I found no public transcript or summary feature. It has published the Yeshivish Whisper model above. A third-party English-shiur fine-tune built on that model reports a WER of 24.8% for the base model and 14.1% after fine-tuning. That fine-tune used 193 h of English shiurim from 77 speakers. — [HF shimonkolodny/whisper-torah](https://huggingface.co/shimonkolodny/whisper-torah)
- **OU:**
  - **OHRBIT** launched in Feb 2026 as a free AI-powered app. It personalizes learning over "hundreds of thousands of hours of OU shiurim" and is a recommendation/learning product, not a transcription service. — [OU press release](https://www.ou.org/news/the-orthodox-union-launches-revolutionary-ai-powered-torah-app/); [VIN News 2026-02-23](https://vinnews.com/2026/02/23/orthodox-union-launches-ai-powered-torah-learning-app/); [Jewish Action](https://jewishaction.com/cover-story/spotify-for-shiurim-the-ous-ai-powered-app-provides-customized-torah-learning/)
  - **AllDaf:** its Play listing describes "bite-sized clips". One user review mentions AI updates with transcription and topics. [ANECDOTE] — [Google Play](https://play.google.com/store/apps/details?id=org.ou.all.daf)
  - Sofer.ai's site lists All Daf among its users. [VENDOR CLAIM via search extract] — [sofer.ai](https://www.sofer.ai/)
- **YU:** the "Rav Legacy Project" (Apr 2026) runs AI audio enhancement, then Sofer.ai transcription, then manual editing. The content is English. — [YU Commentator](https://yucommentator.org/2026/04/new-rav-legacy-project-uses-ai-to-make-rabbi-soloveitchik-shiurim-accessible/)
- **Chabad / JEM:** JEMai searches the archive of the Rebbe's recorded talks, gives "a short summary and links to the exact moment". It opened as a beta for JEMClub members in Apr 2025 and was still members-only on 2026-10-21. — [pinta directory, citing meetjemai.jem.tv](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Kol Halashon:** I found no official AI transcription. Hebrew forum threads discuss transcribing Kol Halashon shiurim, including a proposed "pilot of 1,000 shiurim of about an hour each" and its cost on Gemini. These are proposals, not a launched product. The search summary did not say which of the two threads below holds the pilot quote. — [mitmachim thread](https://mitmachim.top/topic/64815/%D7%91%D7%A7%D7%A9%D7%94-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%A9%D7%99%D7%A2%D7%95%D7%A8%D7%99-%D7%AA%D7%95%D7%A8%D7%94-%D7%A2%D7%91%D7%95%D7%A8-ai?page=13); [tora-forum thread](https://tora-forum.co.il/threads/%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%A9%D7%99%D7%A2%D7%95%D7%A8%D7%99-%D7%AA%D7%95%D7%A8%D7%94-%D7%A2%D7%91%D7%95%D7%A8-ai.57232/); [NHLOCAL projects](https://nhlocal.github.io/)
- **Kol Chai:** search results describe only the Israeli Haredi radio station. I found no AI transcription or hotline product. — [Wikipedia](https://en.wikipedia.org/wiki/Kol_Chai)

#### E. Community channels (WhatsApp, SMS, kosher phones)
- **Sofer.ai WhatsApp bot** for transcribing voice notes, "especially for chevra". — [YWN](https://www.theyeshivaworld.com/news/general/2378877/the-new-tech-startup-revolutionizing-how-torah-is-preserved.html)
- **POM Phone** (a kosher phone) uses SpeakInk from Yiddish Labs, with payment going to Yiddish Labs. [ANECDOTE] — [JTech POM thread](https://jtechforums.org/t/pom-phone/5791)
- **Heimish Geredt** offers a phone line and a Kyocera app. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **TachlisAI:** "kosher" AI over plain SMS, with a Yiddish page. It is LLM chat, not transcription. — [TachlisAI](https://tachlisai.com/yi/kosher-ai-texting)
- **Transcriber for WhatsApp** (transcriber.to) lists Yiddish among 109 languages and returns a transcript plus a short summary. It is a generic engine. [VENDOR CLAIM] — [transcriber.to](https://transcriber.to/whatsapp/en/)
- **Shmooz AI:** a WhatsApp/Telegram multi-model assistant for Hebrew speakers. No Yiddish is documented. — [shmooz.ai](https://shmooz.ai/)

#### F. Generic apps and APIs that list Yiddish
- **TurboScribe** has Yiddish landing pages and is built on Whisper. — [TurboScribe Yiddish](https://turboscribe.ai/speech-to-text/Yiddish); [ChabadLabs wiki](https://chabadlabs.ai/wiki-tools-turboscribe-whisper)
- **HappyScribe**'s languages page lists Yiddish (`yi`) for AI transcription, AI subtitles and AI notetaker, but not for human transcription. — [HappyScribe languages](https://www.happyscribe.com/languages)
- **AssemblyAI** has a Yiddish STT API page claiming coverage of Western and Eastern Yiddish (Litvish, Poylish, Ukrainish) and "flat pricing across all Yiddish dialects". — [AssemblyAI](https://www.assemblyai.com/languages/yiddish)
- **Whisper-wrapper sites listing Yiddish:**
  - Rewind.ai is free, has a 100 MB limit and rates Whisper's Yiddish as "Moderate". — [rewind.ai](https://rewind.ai/transcribe/yiddish/)
  - Others: [whisperai.com](https://whisperai.com/ai-transcription/transcribe-yiddish), [PrismaScribe](https://prismascribe.ai/transcribe/yiddish), [Speech Scribe](https://speechscribe.ai/tools/transcribe-yiddish), [Lingvanex](https://lingvanex.com/services/yiddish-speech-to-text/), [1Transcribe](https://1transcribe.com/transcribe-yiddish/) and [Evidano](https://www.evidano.com/speech-to-text/yi) (claims to be "Yiddish-tuned", with speaker labels and 1 free hour).
  - [Vscoped](https://vscoped.com/audio-to-text-transcription/yiddish) claims 95%+ accuracy.
- **Not listing Yiddish:** Google Cloud STT (including Chirp) and Azure Speech. Meta MMS supports Yiddish for language ID only. Meta Omnilingual ASR supports `ydd_Hebr`. — [pinta directory benchmark table, citing the official language lists](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Transkriptor** advertises 100 languages, but I could not confirm Yiddish. — [Wikipedia: Transcription software](https://en.wikipedia.org/wiki/Transcription_software)

### Inferences
- The "specialized Hasidic-Yiddish ASR tools" the client has heard of are almost certainly YiddishLabs and Sofer.ai, the two named in early-2026 forum discussion, and possibly Yiddish Tools. The field is very young: most products launched or changed substantially in 2025–2026, several in Nov 2026 alone.
- Only Sofer.ai combines shiur-specific ASR with built-in timestamped outlines and summaries, which is the client's actual goal. The Hasidic-Yiddish specialists (YiddishLabs, Yiddish Tools, Heimish Geredt) are general Yiddish ASR or writing tools without shiur summarization.

### Gaps
- **Unconfirmed sites:** I could not confirm Heimish Geredt, DerVort, Milai and Speechyou independently; searches did not surface the sites and direct fetches were not attempted because of the network allowlist. Their status rests on the 2026-10-21 directory snapshot or search extracts.
- **Hebrew/Yiddish press:** no coverage turned up in my searches (Hamodia, Yated, Mishpacha, Ami, Der Blatt, Der Yid, BHOL, Kikar). My searches were not exhaustive in Yiddish.
- **Other platforms:** I found no official AI transcription or summary announcements from Kol Halashon, Dirshu, Yeshiva.org.il, Chabad.org or Kol Chai, and no public product from TorahAnytime.
- **Dicta:** no ASR product surfaced in any search. Dicta appears to remain a text NLP organization (nikud, LLMs), but I did not verify this directly.
- **Founders:** the YiddishLabs/SpeakInk founder's name and the Yingerman interview (said to be issue 90) were not retrieved.

## Q2. What does each tool actually do (dialect coverage, outputs, API/bulk, accuracy claims vs. independent evidence, privacy, who is behind it)?

### Takeaway
The only multi-tool comparison on Hasidic ("heimish") Yiddish is the 3Yiddish benchmark, run 15–23 Nov 2026. It ranks:
1. YiddishLabs: 2.9 wrong words per 100.
2. Yiddish Tools: 4.7.
3. Open and free options (3Yiddish, ivrit.ai, Devniks): about 12–18.
4. Gemini: about 13.
5. Heimish Geredt, YidKey and Yiddi Type's small model: about 32.
6. Plain Whisper and therefore Whisper wrappers such as TurboScribe: 93–110, which is unusable.

The test is small: one speaker, 1,191 words of free speech rather than shiurim. Sofer.ai was not tested; community reports put its Yiddish at about 6/10 and "sloppy". Sofer.ai is the only product with native timestamped topic outlines and summaries. YiddishLabs has an API, and Yiddish Tools offers word timestamps and self-hostable weights.

### Cited Findings

#### Independent and semi-independent accuracy evidence
- **3Yiddish benchmark, committed 2026-11-15 to 2026-11-23** — [BENCHMARK.md](https://github.com/TripleU613/3Yiddish/blob/main/BENCHMARK.md)
  - "21 Yiddish speech-to-text tools and models on 25 takes of free heimish speech (1,191 words)". The main metric is "Wrong words per 100".
  - Sub-scores: loshn-koydesh words wrong (of 13), English words wrong (of 14) and names wrong (of 3).

  | Tool | Wrong/100 | LK wrong /13 | English wrong /14 | Takes perfect /25 |
  |---|---|---|---|---|
  | YiddishLabs (paid website) | **2.9** | 1 | 0 | 14 |
  | Yiddish Tools (paid website) | 4.7 | 2 | 3 | 4 |
  | 3Yiddish Whisper-based (free, local) | 11.9 | 6 | 10 | 3 |
  | Gemini 4 Carbon (via OpenRouter) | 13.2 | 5 | 2 | 2 |
  | Devniks Voice (free site; its own result plus a Gemini pass) | 13.5 | 5 | 6 | 4 |
  | Gemini 4 Argon (via OpenRouter) | 13.6 | 4 | 2 | 1 |
  | ivrit-ai turbo | 15.6 | 10 | 13 | 2 |
  | 3Yiddish Omnilingual-based | 16.0 | 9 | 8 | 4 |
  | ivrit-ai large-v3 | 18.3 | 11 | 8 | 5 |
  | Red Flow (free) | 18.6 | 13 | 12 | 2 |
  | Yiddi Type (built-in small model) | 31.6 | 13 | 13 | 0 |
  | YidKey (paid site, free tier) | 32.5 | 9 | 8 | 0 |
  | Heimish Geredt (paid site) | 32.9 | 11 | 6 | 0 |
  | Whisper large-v3 (plain) | 92.9 | 13 | 14 | 0 |
  | Whisper turbo (plain) | 110.4 | 13 | 14 | 0 |

  - TurboScribe and RocketScribe were left out of the table because their free tiers allowed too few takes. Their output was "the same kind of output as the plain Whisper rows: rough sound-spelling from one, German in Latin letters from the other."
  - Heimish Geredt "stopped early or skipped a stretch in six takes".
  - YidKey "reads smoothly but in several takes whole stretches were replaced with words that were not said".
  - Two other Gemini 4 models (Neon, Hydrogen) failed to return transcripts on about half the takes.
  - **Caveats stated by the author:** one speaker; scoring was partly done by AI agents following the speaker's judgments; the answer key "grew out of 3Yiddish's own draft", which could favour ivrit-ai-based tools.
  - The benchmark author builds a competing tool (3Yiddish), yet ranks two paid competitors above it.
- **JTech Forums thread "Yiddish Transcription through AI"** (early 2026; the search tool reported it was about 282 days old) [ANECDOTE] — [JTech](https://forums.jtechforums.org/t/yiddish-transcription-through-ai/5937)
  - YiddishLabs "worked pretty well but is expensive".
  - Sofer.ai "is cheaper but did a sloppy job".
  - Gemini was "hit or miss" and Whisper fared worse.
- **ChabadLabs wiki:**
  - Sofer.ai's Yiddish quality is reported as about "6/10". — [pinta directory, citing chabadlabs.ai/wiki-topics-hebrew-yiddish](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - "For shiurim that mix Hebrew, Yiddish and Aramaic, Sofer.ai still does better" than Whisper tools. — [ChabadLabs wiki](https://chabadlabs.ai/wiki-tools-turboscribe-whisper)
- **Chabad domain results:**
  - Stock ivrit.ai Yiddish Whisper large-v3 scored "about 0.57" WER on whole farbrengen recordings, which are heavy in loshn koydesh.
  - Fine-tuning turbo on 153 h of the same speaker brought whole-file WER down to 0.080 (CER 0.045) on 20 held-out days. — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
- **Hard oral-history audio:** ivrit.ai models scored about 17% CER / 32% WER (large-v3) and about 25% CER / 32% WER (turbo q4) on 6 minutes of oral history with an elderly speaker and tape noise. — [HF YiddishTools ONNX](https://huggingface.co/YiddishTools/yi-whisper-large-v3-ONNX)
- **Phone-size models:**
  - Small and base Whisper fine-tunes score 62.8% and 59.3% of words right on the ivrit.ai Yiddish eval split. — [HF akiva-yiddish](https://huggingface.co/akiva-yiddish/yiddish-whisper-phone)
  - Meta Omnilingual 7B reports CER 15.8 for `ydd_Hebr`, trained on 12.8 h. — [Omnilingual results CSV](https://github.com/facebookresearch/omnilingual-asr/blob/main/per_language_results_table_7B_llm_asr.csv)
- **No public Yiddish ASR leaderboard exists**; "Today the open landscape is thin." — [pinta README](https://github.com/pinta-partners/torah-yiddish-ai-directory)

#### Vendor profiles

- **Sofer.ai**
  - **Dialect and languages:** mixed English, Hebrew, Aramaic, Yiddish and Yeshivish in one file; "Dedicated Yiddish transcription... in beta". The directory records "Hasidic support not documented". — [sofer.ai](https://www.sofer.ai/); [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - **Outputs:**
    - Click-to-play highlighting (the transcript is synced to the audio).
    - An automatically generated, timestamped topic outline for every transcript, plus summaries.
    - A choice of Hebrew letters or transliteration.
    - Exports include subtitles, audio and captioned clips.
    - "Maishiv" search returns quotes from your own transcripts.
    - Hebrew and Rashi-script OCR, and a WhatsApp bot.
    - Sources: [sofer.ai](https://www.sofer.ai/?dub_id=uWfVPGpbm7c7ZtA5); [Jewish Link](https://jewishlink.news/from-the-people-of-the-book-to-the-people-of-the-transcript-powered-by-sofer-ai/); [YWN](https://www.theyeshivaworld.com/news/general/2378877/the-new-tech-startup-revolutionizing-how-torah-is-preserved.html)
  - **Limits:** 90-minute maximum per file on the free plan and 3 hours per file on Pro. — [sofer.ai pricing (search extract)](https://www.sofer.ai/#pricing)
  - **API / bulk:** I found no public API.
  - **Terms:** free-plan transcripts are for personal use only. "Commercial use of transcripts is permitted only for paid plan subscribers who have received explicit written permission from us." — [app.sofer.ai/terms](https://app.sofer.ai/terms)
  - **Who:** Zach Fish (founder/CEO). — [YWN](https://www.theyeshivaworld.com/news/general/2378877/the-new-tech-startup-revolutionizing-how-torah-is-preserved.html)
  - **Institutional use:** used by "Torah institutions... often using other AI tools to transform the transcript into structured content" ([YWN](https://www.theyeshivaworld.com/news/general/2378877/the-new-tech-startup-revolutionizing-how-torah-is-preserved.html)) and by YU's Rav Legacy Project ([YU Commentator](https://yucommentator.org/2026/04/new-rav-legacy-project-uses-ai-to-make-rabbi-soloveitchik-shiurim-accessible/)).
- **YiddishLabs**
  - **Input/output:** MP3, WAV or M4A up to 100 MB, or recording in the browser, transcribed into Yiddish or English text. A developer API covers transcription and TTS (`/api/v1/speech`). The site says it uses ivrit.ai datasets. The directory gives the dialect as "not stated". — [pinta directory, citing app.yiddishlabs.com/developer/api-docs](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - **Billing:** a cost estimate is shown before you submit; billing is rounded up to the nearest second. — [YiddishLabs credits](https://app.yiddishlabs.com/credits)
  - **Not found:** timestamps, speaker labels and summarization features.
- **Yiddish Tools**
  - **Web:** upload or record and "get everything that was said as text", with timestamps for each word. — [yiddish.tools](https://yiddish.tools/)
  - **Desktop:** offline Windows programs with speaker detection, a read-along player and optional AI clean-up using your own AI key. A local connector lets AI assistants (Claude Desktop, ChatGPT/Codex) use the installed tools. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - **Dictionary:** a free community dictionary of 144,000+ Hasidic Yiddish words. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - **Open builds:** publishes ONNX/transformers.js builds of the ivrit.ai models "so audio never leaves the device". — [HF](https://huggingface.co/YiddishTools/yi-whisper-large-v3-ONNX)
  - **Dialect:** "Not stated".
  - **Hosted API: conflicting.** The directory (2026-10-21) says "there is no hosted public API". The homepage, as of Nov 2026, advertises an "unlimited commercial license: use our tools and API in your business or product". — [yiddish.tools](https://yiddish.tools/)
- **Heimish Geredt:** built for the Hasidic/heimish register; mic, upload, apps and phone line; TTS beta; grammar checking. No metrics are published. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Yiddi Type**
  - Word/text output with mareh mekomos, an editor and "talk and type".
  - Faster multi-core transcription (v2.5.2), a live display (v2.5.4) and an optional beta GPU mode "through the user's own Modal account" (v2.6.0).
  - Closed source. — [GitHub](https://github.com/YankyAppel/yiddi-type-releases)
  - It also offers a larger model that the program describes as ivrit-ai's. — [3Yiddish benchmark](https://github.com/TripleU613/3Yiddish/blob/main/BENCHMARK.md)
- **3Yiddish**
  - CLI file transcription (`3yiddish transcribe shiur.mp3`) and a local OpenAI-style transcription API at `http://127.0.0.1:8793`. No SRT or timestamp output is documented.
  - A GPU build is "about ten times faster". It "knows loshn koydesh. Mostly. It's learning."
  - Sharing of transcriptions is off by default. — [3Yiddish](https://github.com/TripleU613/3Yiddish)
- **Milai:**
  - Outputs: transcript, word-synced captions, and video with burned-in captions, in Word, PDF, text, SRT or VTT.
  - Its terms mention an API.
  - "Hasidic Yiddish not specifically documented". — [pinta directory, citing milai.co.il/terms](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **EchoTora:** transcription, source identification and PDF export. A tora-forum post about an offline transcription program says it works offline and accepts recordings of up to two hours. Attributing that post to EchoTora is uncertain because it came from a search summary. — [EchoTora](https://www.echotora.com/); [tora-forum](https://tora-forum.co.il/threads/%D7%91%D7%A9%D7%95%D7%A8%D7%94-%D7%90%D7%93%D7%99%D7%A8%D7%94-%D7%9C%D7%A2%D7%95%D7%A8%D7%9B%D7%99%D7%9D-%D7%AA%D7%95%D7%A8%D7%A0%D7%99%D7%99%D7%9D-%D7%AA%D7%95%D7%9B%D7%A0%D7%AA-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%9C%D7%94%D7%A8%D7%A6%D7%94-%D7%A2%D7%9C-%D7%9E%D7%97%D7%A9%D7%91-%D7%90%D7%95%D7%A4%D7%9C%D7%99%D7%99%D7%9F.69854/)
- **Speechyou:**
  - Claims "95%+ word accuracy" on standard Yiddish, with no Hasidic-specific figure. Another snippet says "around 90% for clear audio, higher for standard YIVO-based speech".
  - SRT/VTT output, unlimited transcriptions and 10 GB uploads (Solo plan). [VENDOR CLAIM] — [Speechyou](https://speechyou.com/yiddish-hebrew-script-transcription)
- **ivrit.ai yi-whisper:**
  - Trained on about 78 h of crowd-read speech plus about 19 h of WhatsApp-style prompts. The Yiddish language token must be forced, and translation is degraded. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory); [HF dataset](https://huggingface.co/datasets/ivrit-ai/crowd-whatsapp-yi-whisper-training)
  - The free web service transcribe.ivrit.ai is described as a Hebrew service. A GitHub architecture document lists Yiddish among its languages, which is unconfirmed. Its terms forbid automated access and API wrappers. — [ivrit.ai privacy/terms](https://www.ivrit.ai/en/privacy/); [ARCHITECTURE.md](https://github.com/ivrit-ai/transcribe-service/blob/master/ARCHITECTURE.md)
- **ElevenLabs Scribe v2:** the author of a WhatsApp-Web transcription extension built on it says its Yiddish is "materially wrong". — [GitHub](https://github.com/omd0/whatsapp-voice-note-transcriber)
- **Technical pitfalls for shiur audio:**
  - Sicha-to-Text warns that niggunim cause repetition loops. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Whisper's default token suppression blocks the ASCII double quote, which is used for gershayim in abbreviations like רש"י. The fix is `--suppress allow-quotes`.
  - About half of the remaining errors after fine-tuning are words never seen in training: names, sources and loshn-koydesh phrases. — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)

### Inferences
- **Accuracy ranking:** on Hasidic Yiddish, YiddishLabs and then Yiddish Tools are the accuracy leaders on current evidence. Both get almost all of the benchmark's loshn-koydesh and English words right, which matters most for accurate headings and summaries.
- **What the benchmark does not show:** it is dictation-style speech from one speaker. Shiur audio is long-form, fast and full of citations and sing-song, and is often recorded in noisy halls. All error rates should be expected to rise. A 5–10 hour pilot on the hotline's own sources is needed before buying.
- **Sofer.ai:** it has the best end-to-end workflow (timestamped outlines and summaries), but its Hasidic-Yiddish accuracy is unproven and reported as mediocre. It may suit the English/yeshivish portion of the content better than the Hasidic Yiddish portion.
- **Whisper wrappers:** TurboScribe, Rewind, whisperai.com and similar sites run plain Whisper and should be treated as unusable for Hasidic Yiddish. Plain Whisper large-v3 scored 92.9 wrong words per 100.
- **Free options:** 3Yiddish's Whisper-based model and the ivrit.ai models (about 12–18 wrong per 100) are close to Gemini. They may be adequate for producing summaries, where verbatim accuracy is not required, especially if loshn-koydesh terms are corrected downstream.

### Gaps
- **Missing from the benchmark:** Sofer.ai, Speechyou, Milai, EchoTora, Kol Kesav, AssemblyAI, HappyScribe and DerVort.
- **Undocumented features:** I found no documentation of word or segment timestamps for YiddishLabs, Heimish Geredt or Yiddi Type, and no speaker diarization except Yiddish Tools desktop.
- **Privacy policies:** not retrieved for Sofer.ai, YiddishLabs, Milai or Speechyou. Whether uploaded audio is used for training is unknown for all hosted vendors.
- **Sofer.ai's Yiddish beta:** I couldn't determine whether it is a separate model or mode, or how it handles Hasidic pronunciation.

## Q3. What would ~100 h/week (~433 h/month, ~5,200 h/year) cost on each at list price, and are volume or custom deals known?

### Takeaway
At list prices, the specialized AI options range widely:
- **Yiddish Tools:** about $20–40 per month on its plans, if their terms permit this business use; about $270 per month pay-as-you-go; or a $1,499 one-time self-host license as of Oct 2026.
- **Sofer.ai:** about $360–590 per month ($4.3–7.0k per year), with commercial use requiring written permission.
- **Milai:** about $1.4k per month.
- **YiddishLabs:** likely low thousands to many thousands per month. Its rate is 180 credits per audio minute (10,800 per hour), but the dollar price per credit is unpublished.
- **EchoTora:** about $8.8–12k per month.

Human transcription would cost about $16k–$180k per month. The only volume terms found are:
- Sofer.ai: a commercial license by written permission.
- YiddishLabs: subscriptions are 20–25% cheaper per credit.
- Yiddish Tools: a contract-based unlimited commercial license or dedicated server.
- Milai and EchoTora: prepaid minute banks or custom hour packs.
- TurboScribe: "High Volume Mode".

### Cited Findings
- **Sofer.ai** (search extracts of sofer.ai pricing, 2026-11-27; the extract was "somewhat jumbled")
  - **Free:** $0, up to 5 transcripts per month, a "3-hour" cap, and 90 minutes maximum per file. Another extract gives 18 h for the free plan, which conflicts.
  - **Pro (personal use):** $24/month with 18 transcription hours; usage beyond that is billed at $1.50 per hour; 3 h maximum per file. One extract places the "$1.50/hour additional transcription" line in a different column, so which plan it belongs to is unclear.
  - **Max (commercial use):** $72/month with 90 transcription hours, "custom AI transformations", a commercial license and 5,000 Document AI credits per month. It is marked "Max plan, in beta".
  - Sources: [sofer.ai/#pricing](https://www.sofer.ai/#pricing); [sofer.ai](https://www.sofer.ai/)
  - The directory (2026-10-21) confirms "Pro $24/month, Max $72/month (commercial)". — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Commercial use requires explicit written permission. — [terms](https://app.sofer.ai/terms)
  - No published overage rate for Max, and no enterprise tier.
- **YiddishLabs** — [YiddishLabs credits](https://app.yiddishlabs.com/credits)
  - Audio transcription costs 180 credits per minute, with a 1-minute minimum per request.
  - Pay-as-you-go credit bundles never expire.
  - Monthly subscriptions give a fixed credit amount and are "20-25% cheaper per credit than Pay-As-You-Go". Unused subscription credits do not roll over.
  - Text processing costs 0.5 credit per input word.
  - Plans: 5,000 free credits to start, then "$19.99, $29.99 or $149.99 a month, or pay-as-you-go" (directory snapshot 2026-10-21). — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Credits per plan and the dollar price per credit were not found.
- **Yiddish Tools** (yiddish.tools homepage via search extracts, 2026-11-27) — [yiddish.tools](https://yiddish.tools/)
  - **Pay-as-you-go:** "$1 per 1,000 credits", which never expire. New accounts get 5,000 credits, which the site says is enough for about 8 hours of transcription.
  - **Unlimited plan:** $19.99/month, or $16.99/month billed yearly. It includes "unlimited transcription, speech, translation and nikud", with limits of 24 h of audio per day and at most 4 h in progress at a time. Another extract says Unlimited "covers everyday tools without limits, with premium tools paid by credits".
  - **Pro plan:** $39.99/month, or $33.99/month billed yearly. It removes the 4-hour limit, adds an audio archive and audio enhancement, and has premium usage "counting toward a 24-hour daily cap".
  - Yearly billing saves 15%.
  - **Commercial:** an "unlimited commercial license" (tools and API, "without the limits of the regular plans") and "a dedicated server that runs only your work", both handled by contract. No price is published.
  - **One-time desktop prices** (directory snapshot 2026-10-21):
    - Dictation $99, Translation $89, Nikud $59 and Speak $199, each a one-time purchase.
    - The commercial license includes raw model weights and scripts to run on your own servers: Speak or large Dictation $1,499 each, Translation $599, Nikud $499.
    - Launch bundles ran through Nov 13, 2026, so these prices may have changed.
    - Source: [pinta directory, citing yiddish.tools/store/commercial](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Heimish Geredt:** "30 days free, then $4 a month" (directory snapshot 2026-10-21). Volume limits unknown. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Milai** (directory snapshot 2026-10-21) — [pinta directory, citing milai.co.il](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - "35 free minutes once; Basic ₪39/month (200 min), Creator ₪89/month (500 min); prepaid minute bank from ₪900."
- **EchoTora** (search extract, 2026-11-27; the snippet's formatting was garbled) — [EchoTora](https://www.echotora.com/)
  - Free tier: 30 minutes.
  - Paid tiers: ₪90 per transcription hour (basic), ₪80 per hour (standard, 5 h) and ₪70 per hour (professional, 10 h).
  - Custom monthly plan: 20 h for ₪1,500, 30 h for ₪2,100, and +10 h blocks for ₪650.
- **Speechyou:** $15/month billed monthly or $67/year. Solo includes unlimited transcriptions; there is a 3-day free trial. — [Speechyou](https://speechyou.com/yiddish-hebrew-script-transcription)
- **TurboScribe:** — [TurboScribe](https://turboscribe.ai/); [ParrotNotes review](https://parrotnotes.app/blog/turboscribe-review)
  - Unlimited costs $20/month billed monthly, or $10/month billed yearly ($120/year).
  - "No caps on overall usage". Its systems are designed to handle "at least 720 hours of audio or video per month". Unlimited is for one person.
  - Up to 10 h / 5 GB per file and 50 files per upload. "High Volume Mode" applies above an average of about 24 h per day.
- **AssemblyAI:** a search summary of its Yiddish page reported "$0.15 to $0.21 per hour". This was not verified on the page and is covered by the mainstream-API research. — [AssemblyAI](https://www.assemblyai.com/languages/yiddish)
- **Free tools:** Yiddi Type is free with a license per computer ([GitHub](https://github.com/YankyAppel/yiddi-type-releases)). 3Yiddish is free under CC BY-NC-SA 4.0 ([GitHub](https://github.com/TripleU613/3Yiddish)). The ivrit.ai models are free under Apache-2.0 ([HF](https://huggingface.co/ivrit-ai/yi-whisper-large-v3)).
- **Exchange rate:** about 3.20 ILS per USD on 2026-11-06 (OFX showed 3.2027). Free Currency Rates showed 3.19 on the same day. — [OFX](https://www.ofx.com/en-nz/exchange-rates/usd-to-ils/); [Free Currency Rates](https://freecurrencyrates.com/en/convert-USD-ILS/cbm)

### Inferences

#### Normalized cost table
All figures in this table are my arithmetic on the cited list prices.
- **Volume:** 100 h/week = 433.3 h/month = 5,200 h/year.
- **Exchange rate:** 1 USD = 3.20 ILS.
- **Caveat:** none of these include compute, LLM summarization or staff review.

| Option | Effective $/audio hour | ~Per week (100 h) | ~Per month (433 h) | ~Per year (5,200 h) | Notes and risks |
|---|---|---|---|---|---|
| Yiddish Tools Unlimited / Pro plan | ~$0.05–0.09 | ~$5–9 | $19.99–39.99 (or $16.99–33.99 billed yearly) | ~$204–480 | Fits the 24 h/day cap at ~14.3 h/day. Business use may require the separate commercial license (price not public). Unclear whether transcription counts as an "everyday" or a credit-billed "premium" tool. |
| Yiddish Tools pay-as-you-go | ~$0.63 (from "5,000 credits ≈ 8 h" at $1/1,000) | ~$63 | ~$271 | ~$3,250 | The credits-per-hour rate is derived from the vendor's "about 8 hours" claim. |
| Yiddish Tools self-host license (large Dictation) | — | — | — | $1,499 one-time plus own GPU/CPU | Oct 2026 launch price; may have changed after Nov 13, 2026. |
| Sofer.ai Max + $1.50/h overage | ~$1.35 | ~$135 | ~$587 ($72 + 343 h × $1.50) | ~$7,040 | Assumes the $1.50/h overage applies to Max. Commercial use needs written permission. |
| Sofer.ai, 5 Max seats (450 h) | ~$0.83 | ~$83 | $360 | $4,320 | Only if multiple seats are permitted. Likely needs a custom deal instead. |
| YiddishLabs (10,800 credits/h) | **Unknown.** Hypothetical: $5.40 / $10.80 / $21.60 if credits cost $0.50 / $1.00 / $2.00 per 1,000 | $540 / $1,080 / $2,160 | $2,340 / $4,680 / $9,360 | $28k / $56k / $112k | Hypothetical only. The $1/1,000 scale is borrowed from Yiddish Tools and is not YiddishLabs' price. Subscriptions are 20–25% cheaper per credit. Consistent with forum reports that it is "expensive". |
| Heimish Geredt | ~$0.01 | ~$1 | $4 | $48 | Volume limits unknown; weakest accuracy among the specialists (32.9 wrong/100). |
| Milai Creator (₪89 / 500 min) | ~$3.34 (₪10.68) | ~$334 | ~$1,447 | ~$17,400 | Would need the prepaid minute bank or a custom deal; Hasidic Yiddish unverified. |
| Milai Basic (₪39 / 200 min) | ~$3.66 | ~$366 | ~$1,585 | ~$19,000 | |
| EchoTora (₪65–90 per h) | ~$20–28 | ~$2,030–2,810 | ~$8,800–12,190 | ~$106k–146k | Yiddish support unconfirmed. |
| Speechyou | ~$0.03 | ~$3.5 | $15 (or $67/year) | $67–180 | Accuracy on Hasidic speech unverified. |
| TurboScribe Unlimited | ~$0.02–0.05 | ~$2–5 | $10–20 | $120–240 | Plain-Whisper quality, i.e. unusable for Hasidic Yiddish (see Q2). One-person license. |
| AssemblyAI (unverified price) | ~$0.15–0.21 | ~$15–21 | ~$65–91 | ~$780–1,090 | No Hasidic accuracy evidence. |
| 3Yiddish / Yiddi Type / ivrit.ai self-run | $0 license | compute only | compute only | compute only | 3Yiddish's license is non-commercial. Sicha-to-Text reports about 9× real-time on an Apple M5, which implies roughly 11 machine-hours per week for 100 h of audio on one machine (rough). |

#### What the table implies
- **Why it feels expensive:** the client's sense that specialized tools are expensive most likely comes from YiddishLabs' per-minute credit pricing, since its plan prices top out at $149.99/month. Sofer.ai's overages could contribute too.
- **Cheapest specialized option:** at list price, Yiddish Tools is dramatically cheaper and is second-best on accuracy. Whether a hotline can use the consumer plans commercially is the key question to put to the vendor.
- **Negotiation leverage:** 433 h/month is far above any published tier (Sofer.ai Max is 90 h; YiddishLabs' top plan is $149.99). Every vendor would need to quote a custom deal. YiddishLabs (accuracy leader, API available) and Yiddish Tools (self-host weights) are the two worth requesting quotes from.

### Gaps
- **YiddishLabs:** dollar price per credit; credit allotments of the $19.99, $29.99 and $149.99 plans; enterprise/API volume pricing.
- **Sofer.ai:** overage rate on Max, whether multiple seats or enterprise deals exist, and what the commercial-permission process costs.
- **Yiddish Tools:** price of the contract-based "unlimited commercial license" and dedicated server; whether consumer plans allow business use; current price of the self-host license after the Nov 13, 2026 launch period.
- **Milai:** how many minutes the ₪900 bank buys.
- **Others:** volume terms for Heimish Geredt, DerVort and Kol Kesav.

## Q4. What does human Yiddish transcription cost per audio minute or hour, and what is the turnaround (baseline)?

### Takeaway
US-facing Yiddish transcription agencies list about $1–7 per audio minute ($60–420 per audio hour). Israeli Haredi-market rates for Yiddish shiurim are lower, at about ₪120–300 per audio hour ($37–94 per hour). Even the cheapest human option costs about $16k–26k per month at 433 h/month, roughly 30–100× the specialized AI options. I found no published turnaround times.

### Cited Findings
- **US agencies:**
  - quicktranscriptionservice.com: "General transcription starts at $1 per audio minute and legal transcription at $2". It confirms Yiddish availability before quoting. — [Quick Transcription Service](https://www.quicktranscriptionservice.com/Yiddish-Transcription.php)
  - transcription-services-us.com shows the same $1 and $2 figures but also says "pricing begins at just $5 per minute", which is contradictory on the same page. — [Transcription Services US](https://www.transcription-services-us.com/Yiddish-Transcription.php)
  - yiddish-translations.com: "$7 / minute of audio" for audio transcription. — [yiddish-translations.com pricing](https://www.yiddish-translations.com/pricing)
  - Interpreters Unlimited offers Yiddish transcription; no price was retrieved. — [interpreters.com](https://www.interpreters.com/yiddish-transcription-services/)
  - HappyScribe offers no human transcription for Yiddish (AI only). — [HappyScribe languages](https://www.happyscribe.com/languages)
- **General freelance benchmarks** (not Yiddish-specific):
  - Fiverr transcribers charge about $0.4–1.0 per minute ($24–60 per hour) and Upwork about $0.2–0.4 per minute ($12–22 per hour). This page is about 3.5 years old, so the figures are dated. — [Designrr](https://designrr.io/what-should-you-pay-for-transcription/)
  - A 2026 freelance guide gives $0.75–2.50 per audio minute ($45–150 per audio hour) for general audio. — [FreelanceRates](https://www.freelancerates.net/transcriptionist)
- **Israeli/Haredi market** (Hebrew forums; anecdotal and of mixed age) — [tora-forum "What is the accepted price for transcription?"](https://tora-forum.co.il/threads/%D7%9E%D7%94-%D7%94%D7%9E%D7%97%D7%99%D7%A8-%D7%94%D7%9E%D7%A7%D7%95%D7%91%D7%9C-%D7%A2%D7%9C-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C.41433/)
  - A cheap price is about ₪5 per minute.
  - Precise Yiddish-to-Yiddish transcription costs about ₪50–100 per work hour; imprecise Yiddish-to-Yiddish costs ₪200–250 per recording hour.
  - One transcriber charged ₪120 per recording hour.
  - Yiddish-to-Hebrew transcription with light editing costs about ₪50 per work hour.
  - On another forum, ₪4 per minute was called reasonable. — [prog.co.il](https://www.prog.co.il/threads/%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%94%D7%A7%D7%9C%D7%98%D7%95%D7%AA-%D7%91%D7%AA%D7%A9%D7%9C%D7%95%D7%9D.969997/)
- **Israeli agency price pages** (general Hebrew, not Yiddish; figures come from a search summary that did not attribute each figure to a specific page):
  - About ₪2.5–7 per minute depending on recording quality and speech rate.
  - Another lists about ₪6 per minute for a clean single speaker, up to ₪12 per minute for noisy recordings.
  - Sources: [Friedman Transcript](https://www.friedmantranscript.org.il/%D7%A4%D7%AA%D7%A8%D7%95%D7%A0%D7%95%D7%AA/%D7%A9%D7%99%D7%A8%D7%95%D7%AA%D7%99-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C/%D7%9E%D7%97%D7%99%D7%A8-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%9C%D7%93%D7%A7%D7%94/); [Tal-Hayam 2026 price list](https://www.tal-hyam.co.il/%D7%9B%D7%99%D7%A6%D7%93-%D7%9E%D7%97%D7%95%D7%A9%D7%91-%D7%9E%D7%97%D7%99%D7%A8-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%94%D7%94%D7%A7%D7%9C%D7%98%D7%94/)
- **Edited vs. verbatim:** a Torah-shiur transcription service offers a full verbatim option ("every word the rabbi said") and an edited option with paragraphs and headings. — [Friedman Transcript, Torah shiurim](https://www.friedmantranscript.org.il/%D7%A4%D7%AA%D7%A8%D7%95%D7%A0%D7%95%D7%AA/%D7%A9%D7%99%D7%A8%D7%95%D7%AA%D7%99-%D7%AA%D7%9E%D7%9C%D7%95%D7%9C/%D7%AA%D7%9E%D7%9C%D7%95%D7%9C-%D7%A9%D7%99%D7%A2%D7%95%D7%A8%D7%99-%D7%AA%D7%95%D7%A8%D7%94/)

### Inferences
Human costs at 100 h/week (433 h/month), my arithmetic at 3.20 ILS/USD:

| Rate | Per week | Per month | Per year |
|---|---|---|---|
| $1/min ($60/h) | ~$6,000 | ~$26,000 | ~$312k |
| $2/min ($120/h) | ~$12,000 | ~$52,000 | ~$624k |
| $5/min ($300/h) | ~$30,000 | ~$130,000 | ~$1.56M |
| $7/min ($420/h) | ~$42,000 | ~$182,000 | ~$2.18M |
| Israeli ₪120–250 per recording hour (~$37.5–78/h) | ~$3,750–7,800 | ~$16,250–33,800 | ~$195k–406k |
| Israeli ₪4–5 per minute (~$75–94/h) | ~$7,500–9,400 | ~$32,500–40,600 | ~$390k–488k |

- Human transcription is not a realistic baseline for full coverage at this volume. At most it is useful for:
  - creating gold-standard test sets, a few hours, to evaluate AI vendors;
  - paid correction of AI drafts for fine-tuning data. "Work hour" rates of ₪50–100 apply to editing, not to audio hours.
- Even creating 100 h of fine-tuning data at about $40–80 per audio hour (Israeli rates) would cost about $4k–8k. This is a plausible one-off investment compared with years of SaaS fees; it connects to the fine-tuning evidence in Q5.

### Gaps
- No vendor published turnaround times for Yiddish. Typical human throughput (work hours per audio hour) for dense Yiddish/Aramaic shiur audio was not found.
- No current Upwork, Fiverr or ProZ Yiddish-specific transcription rates were retrieved.
- US agency prices are undated "starting at" figures and are probably not quotes for Hasidic shiur audio.

## Q5. Do any vendors license models, offer on-premise/self-host deals, or allow fine-tuning on customer data?

### Takeaway
- **Self-hosting:**
  - Yiddish Tools is the only commercial vendor found that sells self-hostable weights: a one-time commercial license with raw model weights and scripts, $1,499 for the large Dictation model as of Oct 2026. It also offers contract-based dedicated servers.
  - Open alternatives are ivrit.ai's Apache-2.0 Yiddish Whisper models (commercial use allowed) and TorahAnytime's Apache-2.0 yeshivish Whisper.
  - 3Yiddish's models are non-commercial (CC BY-NC-SA).
- **APIs, not self-hosting:** YiddishLabs offers a developer API and Milai's terms mention an API.
- **Sofer.ai:** no API or self-hosting found.
- **Fine-tuning:** no vendor advertises fine-tuning on customer audio. The Rebbe-ASR project shows that domain fine-tuning on about 150 h cut WER from about 0.57 to about 0.08 for a single speaker.

### Cited Findings
- **Yiddish Tools** — [pinta directory, citing yiddish.tools/store/commercial](https://github.com/pinta-partners/torah-yiddish-ai-directory); [yiddish.tools](https://yiddish.tools/)
  - "For businesses, a commercial license gives the raw model weights and scripts to run on your own servers": Speak or large Dictation $1,499 each, Translation $599, Nikud $499; launch bundles through Nov 13, 2026 (directory 2026-10-21).
  - The homepage (Nov 2026) offers an "unlimited commercial license" for tools and API plus "a dedicated server that runs only your work", handled "in a proper contract".
- **YiddishLabs:** a developer API for transcription and TTS. — [pinta directory, citing app.yiddishlabs.com/developer/api-docs](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - No self-host or fine-tuning offer was found.
  - The site says it uses ivrit.ai datasets. — same source
- **Sofer.ai:** commercial use of transcripts only with explicit written permission ([terms](https://app.sofer.ai/terms)). Max includes "custom AI transformations", a configurable post-processing feature, not model fine-tuning ([sofer.ai pricing](https://www.sofer.ai/#pricing)). No API or self-host was found.
- **Milai:** "Its terms mention an API." — [pinta directory, citing milai.co.il/terms](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **ivrit.ai:**
  - The `yi-whisper-large-v3` and `-turbo` weights are Apache-2.0, with CT2 and GGML conversions available. — [HF](https://huggingface.co/ivrit-ai/yi-whisper-large-v3); [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - The training datasets (`crowd-recital-yi`, `crowd-whatsapp-yi`) are gated under a custom license. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Third-party hosting exists, for example a FriendliAI inference endpoint for `yi-whisper-large-v3`. — [FriendliAI](https://friendli.ai/models/ivrit-ai/yi-whisper-large-v3)
  - There is also a RunPod serverless worker for the Yiddish model. — [ivrit-runpod-yi](https://github.com/smrtesy/ivrit-runpod-yi) (via [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory))
- **TorahAnytime:** `whisper-large-v3-yeshivish` is Apache-2.0. — [HF](https://huggingface.co/torahanytimecom/whisper-large-v3-yeshivish)
- **3Yiddish:** CC BY-NC-SA 4.0 — "Don't sell it. Ever." Derivatives must stay equally free. — [3Yiddish](https://github.com/TripleU613/3Yiddish)
- **Yiddi Type:** free per-computer license, closed source. Its GPU mode runs on the user's own Modal account. — [GitHub](https://github.com/YankyAppel/yiddi-type-releases)
- **Rebbe Yiddish ASR** (Golan Benoni) — [rebbe-yiddish-asr](https://github.com/golanbenoni/rebbe-yiddish-asr)
  - **Data:** 30,212 clips (168 h) were cut; after filtering, 27,833 clips (153 h) were used for training. A further 306 h was being aligned for v5.
  - **Method:** full fine-tuning of ivrit.ai Yiddish turbo at lr 3e-5 for 4 epochs.
  - **Results:** whole-file WER 0.081 / CER 0.045, best 0.080, versus about 0.57 for the untouched model.
  - **Hardware:** trained on Apple-silicon Mac Studios.
  - **Alignment:** for audio without timings, the hypothesis is anchored to the reference text, with anchor rates of 0.94–0.98.
  - **License:** the code is "all rights reserved" and the weights are not public.
- **Sicha to Text:** an Apache-2.0 offline Mac app. — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Community data:**
  - The pinta directory's author is seeking licensed or public-domain Hasidic audio with transcripts, "especially Hasidic (Satmar/Hungarian, Polish/Central) speakers". — [pinta README](https://github.com/pinta-partners/torah-yiddish-ai-directory)
  - Mozilla Common Voice Yiddish has only 2.1 validated hours from 129 speakers (as of 2026-10-20). — [pinta directory](https://github.com/pinta-partners/torah-yiddish-ai-directory)
- **Privacy:** offline and on-device options keep audio local. These are Yiddi Type, 3Yiddish (sharing off by default), Yiddish Tools desktop and its ONNX builds ("audio never leaves the device"), and Sicha to Text. — [Yiddi Type](https://github.com/YankyAppel/yiddi-type-releases); [3Yiddish](https://github.com/TripleU613/3Yiddish); [HF YiddishTools](https://huggingface.co/YiddishTools/yi-whisper-large-v3-ONNX)

### Inferences
- **Paths that avoid per-hour SaaS fees at 5,200 h/year:**
  1. License Yiddish Tools' weights and self-host. Check whether the license permits fine-tuning.
  2. Self-host ivrit.ai Apache-2.0 models and fine-tune them on the hotline's own corrected transcripts. Rebbe ASR shows large gains from about 150 h of in-domain data. A hotline with 10–20 recurring maggidei shiur is closer to that single-speaker case than to general Yiddish.
  3. Negotiate an API volume deal with YiddishLabs for the best current accuracy.
- **Privacy is a low-stakes concern here:** the audio is already public. The bigger constraints are license terms (Sofer.ai's written-permission clause, 3Yiddish's non-commercial license, ivrit.ai's ban on automating its free web service) and data ownership if the hotline invests in corrections that a vendor could then train on.

### Gaps
- **No vendor states fine-tuning or custom-vocabulary terms:** for example, whether speaker names, sefer names or a loshn-koydesh lexicon can be supplied to YiddishLabs, Sofer.ai or Yiddish Tools.
- **Yiddish Tools license:** whether the self-host license allows modification or fine-tuning, and its terms after the launch period.
- **Hosted vendors:** I could not retrieve data-retention or training-use policies for YiddishLabs, Sofer.ai, Milai or Speechyou.
- **Network allowlist:** these sites (and possibly others) must be allowed in the session's network settings to verify vendor pages directly: www.sofer.ai, forums.jtechforums.org, app.yiddishlabs.com, yiddish.tools, heimishgeredt.com and milai.co.il.
