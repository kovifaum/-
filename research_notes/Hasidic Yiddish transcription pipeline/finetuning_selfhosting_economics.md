# Economics and practicalities of fine-tuning and self-hosting a Whisper-class ASR model for Hasidic Yiddish lecture audio

*Method note (read first): research date 2026-11-27. Direct page fetching of arxiv.org and huggingface.co was blocked in this session (DNS/allow-list), so figures from papers and model cards on those hosts come from search-engine extracts of those pages, not from reading the full PDFs. They are cited to the original URLs but should be spot-checked before being quoted to the client. Prices are Oct–Nov 2026 snapshots, mostly via aggregators; GPU prices are unusually volatile in late 2026. Client volume used throughout: ~100 h/week ≈ 433 h/month ≈ 5,200 h/year.*

## 1. Data scaling: how much labeled audio buys a meaningful WER gain (incl. LoRA vs full fine-tuning, forgetting of English)

### Takeaway
Published curves consistently show the steepest gains in the first ~10–50 h of in-domain labeled audio, real but diminishing gains from ~100 to ~200–400 h (roughly a 10–25% relative WER cut per doubling in that range), and that dialects far from Whisper's pre-training (Swiss German) needed ~1,000 h (much of it weak/pseudo-labeled) for strong results; the two closest analogs (ivrit.ai Hebrew ~5,000 h; ivrit.ai Yiddish ~97 h of read speech) show the same pattern. Full fine-tuning usually beats LoRA on hard languages, but both forget English unless English (or code-switched) data is mixed in.

### Cited Findings
**Low-data regime (≤50 h)**
- Hugging Face's tutorial reports ~8 h of Hindi fine-tuning data took Whisper-small from 63.5% to 32.0% WER (tutorial, not a controlled study) — [HF blog: Fine-Tune Whisper](https://huggingface.co/blog/fine-tune-whisper)
- African-language benchmark (arXiv 2512.10968): "the steepest gains are concentrated below 50 hours"; scaling is strongly language-dependent (Afrikaans and Bambara reach strong performance quickly; Akan and Ewe stay at high error rates despite more data) — [Benchmarking ASR Models for African Languages](https://arxiv.org/html/2512.10968)
- Assamese study: "practical performance can be achieved with at least 50 hours of transcribed data, with performance continuing to improve up to 200 hours"; cites earlier work with significant WER gains from <15 h for African languages — [arXiv 2607.17164](https://arxiv.org/pdf/2607.17164)
- Literature summary in a 2026 Pashto paper: zero-shot Whisper WER ranges from 48% to >90% on seven low-resource languages; targeted fine-tuning brings them to 12–45% — [Fine-tuning Whisper for Pashto ASR: strategies and scale](https://arxiv.org/abs/2604.06507)
- An endangered-language paper fine-tuned with only 6 h of data (title) and cites prior work needing >900 h to take Swiss German WER from 45% to 18% — [WARDEN, arXiv 2605.13846](https://arxiv.org/pdf/2605.13846)

**Mid-scale curves (50–400 h)**
- Kinyarwanda (Whisper row in the African benchmark): 100% WER at 1 h → ~49% at 50 h → 36.43% at 100 h → 32.37% at 200 h. Swahili 51.33% at 1 h → 16.88% at 400 h with "diminishing gains after 100 hours" (the Swahili row could not be matched to a specific model in the extract — treat as unverified) — [arXiv 2512.10968](https://arxiv.org/html/2512.10968)
- Dedicated Kinyarwanda/Kikuyu data-scaling study: 50-h model 12.51% WER; crosses 10% WER at 200 h — [arXiv 2510.07221](https://arxiv.org/abs/2510.07221). CLEAR Global's summary: Whisper large-v3 Kinyarwanda 10.9 WER at 100 h, 9.82 at 200 h; W2v-BERT-2.0 Swahili 9.69 at 200 h → only 8.76 with another 200 h — [CLEAR Global](https://clearglobal.org/lessons-from-advancing-african-language-ai-and-insights-on-data-scaling-for-african-automatic-speech-recognition/). (The two Kinyarwanda curves disagree by ~3x in absolute WER — different test sets/setups.)
- Pashto, 113 h Common Voice: whisper-small 24.89% vs whisper-large-v3-turbo 23.37% WER (author: small is the practical optimum at this data size); online augmentation (speed perturbation + noise) improved WER by 7.25 points — [arXiv 2604.06507](https://arxiv.org/abs/2604.06507)
- Faroese: a 2026 LREC model fine-tuned on 100 h for 30k steps; an earlier paper found zero-shot Whisper very poor for Faroese — [LREC 2026 FPSC](https://aclanthology.org/2026.lrec-1.490.pdf); [ASR Language Resources for Faroese](https://openreview.net/pdf?id=Vzp2aRidnh)
- Luxembourgish: fine-tuning whisper-large-v3-turbo moved WER from 0.95 to 0.28 (hours not given in extract) — [WorldSpeech, arXiv 2605.09167](https://arxiv.org/pdf/2605.09167); a public LoRA recipe uses FLEURS' ~8.46 h — [lb-whisper GitHub](https://github.com/Sitraka17/lb-whisper)
- Scottish Gaelic: best Whisper-Turbo with LoRA rank 512 averaged 22.0% WER; adding manually and automatically transcribed data → 19.0%; a hybrid system reached 12.8% — [Practitioner's Guide, arXiv 2506.04915](https://arxiv.org/abs/2506.04915). A fully fine-tuned large-v3-turbo with semi-supervised data reports 13.0% average WER on four test sets — [eist-edinburgh model card](https://huggingface.co/eist-edinburgh/whisper-large-v3-turbo-gaelic-ct2-v2)

**Dialects far from pre-training (~1,000 h)**
- Swiss German (FHNW): ~1,033 h total (502 h of long-form data built from sentence-level corpora incl. STT4SG-350, + 406 h pseudo-labeled SRG broadcasts, + 125 h Common Voice German) → 12.1% WER on STT4SG-350 test; Large-v2 was used because it beat Large-v3 on their Swiss German sets — [arXiv 2412.15726](https://arxiv.org/html/2412.15726v3). STT4SG-350 = 343 h from 316 speakers, 34-h test set — [arXiv 2506.08836](https://arxiv.org/html/2506.08836v1)
- A 2026 preprint argues several Swiss German results are inflated by benchmark contamination and convention mismatch; its "honest baseline" is 25.6% WER (13.8% cWER) on the All Swiss German Dialects Test Set, and it notes the 12.1% figure is evaluated with the same conventions as training — [arXiv 2606.07608](https://arxiv.org/pdf/2606.07608)

**Closest analogs: ivrit.ai (Hebrew, Yiddish)**
- ivrit-ai/whisper-large-v3 (Hebrew, continued training of large-v3, April 2025): ~300 h crowd-transcribed public audio + ~50 h crowd-recorded Wikipedia snippets + ~4,700 h Knesset plenum ≈ 5,000 h; datasets interleaved 0.15:0.8:0.05 (knesset : crowd-transcribe : crowd-recital); card points to the ivrit.ai Hebrew leaderboard instead of giving a WER — [model card](https://huggingface.co/ivrit-ai/whisper-large-v3)
- Third-party FLEURS-Hebrew measurement: stock large-v3-turbo 28.59% WER vs ivrit.ai turbo fine-tune 14.37% (greedy decoding); another source measured 18.46% for the same model; a GitHub issue describes it as "37% fewer errors than stock turbo" — [Handy discussion #1975](https://github.com/cjpais/Handy/discussions/1975); [hex issue #752](https://github.com/anomalyco/hex/issues/752)
- ivrit-ai/yi-whisper-large-v3 (Yiddish): ~97 h of crowd-sourced *read/prompted* speech (≈78 h Wikipedia/Michlol snippet recordings + ≈19 h WhatsApp-style recordings of predefined prompts); two-phase training (both sets, then post-training on the WhatsApp set); LR 1e-5, batch 32, 8× NVIDIA A40, ~15 h total; no published WER (Yi eval set "not yet published", internal set used) — [yi-whisper-large-v3 card](https://huggingface.co/ivrit-ai/yi-whisper-large-v3); [yi turbo README commit](https://huggingface.co/ivrit-ai/yi-whisper-large-v3-turbo/commit/ee856dd79b6f7b1714da08750c2f7ff9bd0d9c07). (Base-model metadata conflicts between the ivrit-ai and yoad mirrors — large-v3 vs turbo — [yoad mirror](https://huggingface.co/yoad/yi-whisper-large-v3).)

**LoRA/PEFT vs full fine-tuning**
- Frisian thesis: LoRA with 1.4% of parameters achieved WER comparable to full fine-tuning; low-resource languages benefited more from LoRA than high-resource ones — [RUG thesis](https://campus-fryslan.studenttheses.ub.rug.nl/539/1/MA-5521904-X-Liu.pdf)
- Pashto (whisper-base): full fine-tuning best (21.22% WER); LoRA trailed by 33.36 points, frozen-encoder by 14.76 points; Optuna-tuned LoRA on Uyghur/Kazakh/Kyrgyz gave 6–21% relative WER reduction over untuned LoRA (cited work) — [arXiv 2604.06507](https://arxiv.org/abs/2604.06507)
- Swiss German LoRA (2026): model hallucinated frequent training phrases on silence; base Whisper assigns >90% probability to the no-speech token on silent input, the LoRA model only 8.8%; reducing α/r from 2.0 to 0.2 eliminated all hallucinations in their setup — [arXiv 2606.07608](https://arxiv.org/pdf/2606.07608)
- AWS: full fine-tuning required 24× more GPU hours than LoRA in their experiment — [AWS ML blog](https://aws.amazon.com/blogs/machine-learning/fine-tune-whisper-models-on-amazon-sagemaker-with-lora/)

**Catastrophic forgetting (English)**
- LoRA-Whisper: fine-tuning only on new-language data "results in serious catastrophic forgetting", base-language WER "nearly triples"; mixing base-language data alleviates it at some cost to the new language — [arXiv 2406.06619](https://arxiv.org/html/2406.06619v1)
- Learn and Don't Forget: standard full fine-tuning reached a forgetting-WER of 89.80% on Whisper medium; Elastic Weight Consolidation "halv[ed] the WER degradation" on out-of-domain English — [arXiv 2407.06800](https://arxiv.org/pdf/2407.06800)
- Rehearsal-free LoRA study: LoRA variants also forget in sequential training (Chinese WER >30% after Tibetan); orthogonal-gradient O-LoRA reduced forgetting — [arXiv 2408.10680](https://arxiv.org/pdf/2408.10680)
- Yoruba practitioner blog (anecdotal): English CV17 WER rose from 21.84% to 147.09% after Yoruba-only fine-tuning; adding English data "significantly" mitigated it — [Medium](https://medium.com/@ccibeekeoc42/advancing-multilingual-speech-recognition-fine-tuning-whisper-for-enhanced-low-resource-34529b525f90)
- Replay of as few as ~1,000 samples per language restored base languages to near-original WER (LLM-based ASR, 2026) — [MEUSLI, arXiv 2607.22100](https://arxiv.org/pdf/2607.22100); gradient projection + replay reported 0.04% forgetting-WER on Whisper-large-v3 — [arXiv 2607.11163](https://arxiv.org/pdf/2607.11163)
- A continual-learning extract reports Welsh WER jumping 27.5% → 96.8% after fine-tuning on Irish (source attribution in the extract uncertain; appears to be the Cambridge paper) — [arXiv 2407.06800v1](https://arxiv.org/html/2407.06800v1)

### Inferences
- **Starting point matters more than the curve.** The client would continue-train an existing Yiddish checkpoint (ivrit.ai yi, ~97 h read speech) or a Hebrew one, not raw Whisper. Their labeled data then mainly has to teach register (yeshiva lecture style), acoustics (phone-line/lecture-hall audio, spontaneous speech), Ashkenazi pronunciation of loshn koydesh, and English code-switching — all absent from 97 h of read Wikipedia/prompt speech.
- **Tiered extrapolation for Hasidic Yiddish lectures (not published evidence):**
  - 10–25 h: enough for a gold test set and a pilot fine-tune; expect visible gains on recurring speakers/terms but not robustness.
  - ~50 h: plausible minimum for a "meaningful" domain gain (cf. Assamese ≥50 h practical; African benchmark steepest below 50 h; Kinyarwanda 50 h → 12.5%).
  - 100–300 h: continued, diminishing gains. Arithmetic from the Kinyarwanda study: 50→100 h = 12.51→10.9 (−12.9% relative); 100→200 h = 10.9→9.82 (−9.9% relative). From the African benchmark: 50→100 h = 49→36.43 (−25.7% rel.); 100→200 h = 36.43→32.37 (−11.1% rel.). So each doubling past ~100 h buys roughly 10–25% relative WER reduction.
  - 1,000+ h (mostly weak/pseudo-labeled in-house audio): what dialect-level state-of-the-art projects used (Swiss German ~1,033 h; ivrit.ai Hebrew ~5,000 h). The client generates ~5,200 h/year of unlabeled in-domain audio, so this tier is reachable via pseudo-labeling (Section 2).
- **Model choice:** Pashto shows little benefit from model size at ~100 h; turbo is far cheaper to serve (Section 5) and ivrit.ai ships turbo variants, so fine-tune turbo first and only move to large-v3 if the gold-set eval shows a gap.
- **LoRA vs full:** for a register this far from pre-training, expect full fine-tuning (or high-rank LoRA, cf. Gaelic rank 512) to win; LoRA is fine for cheap pilots or 24 GB GPUs, with α/r tuned low to avoid silence hallucinations.
- **Forgetting/code-switching:** because the lectures themselves contain English, in-domain labels act as partial English "replay". Still mix in some English and Hebrew data (my rough suggestion: 5–20% of samples, based on the replay results above) and hold out English and code-switched eval slices.

### Gaps
- No published fine-tuning scaling curve exists for Hasidic Yiddish or any Yiddish register; no WER is published for the ivrit.ai Yiddish model.
- No study measured forgetting on *code-switched* utterances specifically.
- AfriSpeech (African-accented English), Arabic-dialect and Welsh fine-tuning scaling numbers were not collected; full scaling tables in 2512.10968/2510.07221 could not be opened (arxiv.org blocked), so intermediate points (10 h, 25 h) are missing.
- Also surfaced but not examined: "Whispering Wisely: Smart Subset Selection for ASR Fine-Tuning" (arXiv 2610.32997, title only), which may bear on picking which hours to label — [arXiv 2610.32997](https://arxiv.org/pdf/2610.32997)

## 2. Weak/pseudo-labeling, noisy-student self-training, and "found" transcripts

### Takeaway
Weak labels are how successful dialect projects scaled (ivrit.ai: ~4,700 h of non-verbatim Knesset protocols, force-aligned; FHNW: 406 h of pseudo-labeled broadcasts). But self-training a model on its own outputs gives mixed results, from no gain to worse. Gains show up when the teacher is stronger or already in-domain, when labels are filtered (Distil-Whisper used a ~10% WER threshold), and when weak data is used for a pre-training stage followed by clean human data.

### Cited Findings
- Distil-Whisper filters pseudo-labels whose normalized WER against ground truth exceeds a threshold; 10% was chosen as the trade-off (too high admits hallucinations, too low keeps only easy samples). In its table, lowering the threshold from 100 to 15 raises the share of data filtered from 0% to 32%, with gains flattening below ~20 — [Distil-Whisper, arXiv 2311.00430](https://arxiv.org/pdf/2311.00430). The project combined 12 open datasets totalling ~22k h (secondhand README) — [README copy](https://huggingface.co/supawichwac/training/blob/main/flax/README.md)
- uDistil-Whisper proposes label-free filtering because WER-based filtering needs ground truth that low-resource languages lack — [arXiv 2407.01257](https://arxiv.org/pdf/2407.01257)
- ivrit.ai Knesset pipeline: Whisper + stable-whisper word-level alignment to force-align protocol text; protocols are "non-verbatim… by design" (they preserve the speaker's "spirit" in proper Hebrew grammar); audio sliced to ≤30 s; ~4,700 h — [knesset-plenums dataset](https://huggingface.co/datasets/ivrit-ai/knesset-plenums); [Whisper-training version](https://huggingface.co/datasets/ivrit-ai/knesset-plenums-whisper-training). Training: Knesset-based pre-training over all ~4,700 h for 3 epochs, then mixed post-training on crowd-transcribed data plus the "highest-quality filtered" 150 h of Knesset data — [ivrit.ai model card](https://huggingface.co/ivrit-ai/whisper-large-v3/blob/main/README.md)
- FHNW Swiss German: 406 h of pseudo-labeled SRG broadcast audio was part of the ~1,033 h that reached 12.1% WER — [arXiv 2412.15726](https://arxiv.org/pdf/2412.15726)
- Swiss German 2026: training on Whisper's own predictions gave "no measurable improvement" over the original editorial subtitles; "Whisper is not effectively self-correcting" — [arXiv 2606.07608](https://arxiv.org/pdf/2606.07608)
- BEARD side experiment: Whisper-small fine-tuned on pseudo-labels from 3,500 h of untranscribed ATCO2 audio scored 25.97% WER, worse than fine-tuning on transcribed data alone (22.79%) — [arXiv 2510.24570](https://arxiv.org/pdf/2510.24570)
- Kazakh: fine-tuning on pseudo-labels with a frozen encoder (decoder-only) brought a model to roughly the WER of the original Whisper-large — [Interspeech 2024](https://www.isca-archive.org/interspeech_2024/li24ia_interspeech.pdf)
- Sudanese Arabic: self-training plus TTS augmentation took Whisper-Medium to 51.6% WER on an out-of-domain holdout, ~45% relative better than zero-shot — [arXiv 2601.06802](https://arxiv.org/pdf/2601.06802)
- Massively multilingual pseudo-labeling: fine-tuning on each language's labeled data before generating pseudo-labels mattered; pooling unlabeled data across languages produced poor pseudo-labels for low-resource languages (e.g., Greek with 2.75 h) — [arXiv 2111.00161](https://arxiv.org/pdf/2111.00161)
- Scottish Gaelic: adding manually and automatically transcribed data improved best Whisper from 22.0% to 19.0% WER — [arXiv 2506.04915](https://arxiv.org/abs/2506.04915)
- Post-editing machine drafts is only faster than typing from scratch when draft WER is below roughly 30% — [CMU caption latency study](https://www.cs.cmu.edu/~fmetze/interACT/Publications_files/publications/asr_threshold_w4a.pdf)

### Inferences
- **The client's ~5,200 h/year of in-domain audio is its biggest asset.** Suggested recipe, built from the cited patterns: (1) fine-tune on 50–250 h of human-verified data; (2) transcribe 1,000–5,000 h of archive audio; (3) keep segments that pass agreement and quality filters, e.g. pairwise WER ≤10–20% between two different systems (own model vs ivrit.ai checkpoint or a commercial tool, a Distil-Whisper-style threshold), plus no-speech-probability, compression-ratio and length-ratio checks; (4) pre-train on the filtered weak set, then fine-tune on clean human data (the ivrit.ai Knesset→crowd pattern); (5) A/B against a labeled-only baseline on a gold set.
- **Expected payoff is uncertain.** The evidence runs from negative (BEARD), to none (Swiss German self-labels), to moderate (Gaelic: 22.0→19.0 = −13.6% relative), to large (Sudanese, from a weak baseline). Self-labels from the same model risk reinforcing its own errors. A *different, stronger* teacher (a specialized commercial Hasidic-Yiddish tool) is the most promising source, provided its terms allow training on outputs (not verified, see Gaps).
- **Commercial-draft + human-correction labeling** is the cheapest way to make *clean* labels (Section 3): if the commercial draft is below ~25–30% WER, correction cuts labor roughly in half.
- **Found transcripts** (published lecture write-ups, sicha/shiur summaries) are likely edited, abridged or partly in Hebrew rather than verbatim Yiddish, like the Knesset protocols. Use them only after forced alignment and filtering, and only in a pre-training stage. Risk: the model learns to "edit" or normalize speech. That may be tolerable for summaries but corrupts timestamps where alignment is poor.
- **Compute for pseudo-labeling is negligible:** 2,000 h at 20–50× real time is 40–100 GPU-hours, i.e. ~$30–$300 at $0.75–$2.99/h serverless (prices in Section 4).

### Gaps
- No published pseudo-labeling or found-transcript result for Yiddish.
- Not verified: whether specialized commercial Hasidic-Yiddish vendors (or ElevenLabs/AssemblyAI etc.) permit using their outputs as training data.
- Numbers from HebDB (weakly supervised Hebrew) and the Greek podcast corpus were not collected (titles only) — [HebDB](https://arxiv.org/pdf/2407.07566); [Greek podcast corpus](https://arxiv.org/pdf/2406.15284)

## 3. Data creation cost: transcription effort, pay rates, tools, cost per labeled hour

### Takeaway
A realistic fully loaded cost is ~$145–$420 per labeled audio hour from scratch and ~$70–$210 when correcting a good machine draft (draft WER ≲25–30%). Yiddish agency rates of $1–$7 per audio minute ($60–$420 per audio hour) bracket this. An adjudicated gold test set costs ~1.5× the from-scratch rate. Label Studio Community (free, MIT-licensed) is sufficient tooling.

### Cited Findings
**Effort ratios (real-time factors)**
- Manual transcription by experienced transcribers took ~5× the audio duration; starting from ASR drafts can halve it — [Recorded Debating Dataset, arXiv 1709.06438](https://arxiv.org/pdf/1709.06438)
- A one-hour interview typically takes 5–10 h to transcribe manually; an AI-assisted workflow saved 53.8%–76.9% — [arXiv 2503.13031](https://arxiv.org/pdf/2503.13031)
- Post-editing was consistently ~2× as fast as from-scratch transcription, with a risk that post-editors miss ASR errors — [LREC 2016, L16-1314](https://aclanthology.org/L16-1314.pdf)
- Editing ASR output becomes no faster than typing from scratch at roughly 30% WER — [CMU study](https://www.cs.cmu.edu/~fmetze/interACT/Publications_files/publications/asr_threshold_w4a.pdf)
- Icelandic parliament: post-editing automatic transcripts was faster and more accurate than transcribing from scratch — [arXiv 1807.11893](https://arxiv.org/pdf/1807.11893); a long-term-care ASR at 22.6% WER still required manual correction — [PMC9933064](https://pmc.ncbi.nlm.nih.gov/articles/PMC9933064/)

**Yiddish transcription vendor prices (marketing pages, undated or partially dated)**
- Transcription Services US: general transcription from $1 per audio minute, legal $2 — [link](https://www.transcription-services-us.com/Yiddish-Transcription.php)
- Quick Transcription Service: same $1/$2, but the page also says pricing "begins at just $5 per minute" (internal conflict) — [link](https://www.quicktranscriptionservice.com/Yiddish-Transcription.php)
- Yiddish Translations: $7 per audio minute for transcription — [link](https://www.yiddish-translations.com/pricing)
- We Do Seforim: $2 per audio minute (page ~844 days old) — [link](https://wedoseforim.com/prices/)
- YiddishLabs (AI) bills 180 credits per minute, with no dollar figure on the page — [link](https://app.yiddishlabs.com/credits)

**Freelance/worker pay rates (US and Israel/Hebrew analogs)**
- Upwork Yiddish→English translators: featured rates of $15, $20 and $25/hr; one New York listing at $63/hr; Upwork estimates $200–$800 per project — [Upwork](https://www.upwork.com/hire/yiddish-to-english-translators/)
- Upwork general transcription: typically $17–$22/hr; specialized (legal/medical) $30–$90/hr — [Upwork resources](https://www.upwork.com/resources/best-transcription-jobs)
- Hebrew analogs: GoTranscript pays $0.50–$1.20 per audio minute for Hebrew work — [GoTranscript](https://gotranscript.com/hebrew-transcription-job); an Upwork Hebrew listing offers $40 per audio hour — [Upwork listing](https://www.upwork.com/freelance-jobs/apply/Hebrew-Audio-Transcriptionist-Long-Term-Remote-Freelance_~022060055639943047956/); Jobify360 posting at $3.25 per audio minute — [Jobify360](https://jobify360.co.il/jobs/4150558293-in); Braintrust Hebrew podcast role at $100 per completed transcription hour — [listing](https://remotestack.in/jobs/podcast-audio-transcription-specialist-hebrew-remote-freelance-braintrust-1783f3ac-6232-41ef-9945-984de150709a); Perle Hebrew transcriber $20.50–$26.50/hr (a Latin-America version $8–$10/hr) — [Perle](https://aitrainer.work/jobs/perle/expert-audio-transcriber-hebrew-latam); a Tel Aviv University project lists $5/hr for a native Hebrew transcriptionist and $25–$30/hr for another role — [TAU PDF](https://humanities.tau.ac.il/sites/humanities.tau.ac.il/files/media_server/all-units/Linguistic%20Editing/Hebrew%20Language%20Project%20%E2%80%93%20Collaboration%20Opportunity.pdf); an Israeli Fiverr seller charges $50/hr — [Fiverr](https://www.fiverr.com/moriahbe)

**Tools and crowdsourcing precedent**
- Label Studio Community Edition is free and open source; ASR output (e.g., Amazon Transcribe, NVIDIA NeMo as an ML backend) can be loaded as pre-annotations for reviewers to correct; the paid Enterprise tier has an "advanced audio transcription interface" — [Label Studio OSS](https://labelstud.io/label-studio-oss/); [NVIDIA blog](https://developer.nvidia.com/blog/generating-high-quality-labels-for-speech-recognition-with-label-studio-and-nemo/); [HumanSignal](https://humansignal.com/blog/building-a-better-ui-for-audio-transcription-at-scale/). Its MIT licence allows commercial use — [arXiv 2006.05236](https://arxiv.org/pdf/2006.05236.pdf)
- ivrit.ai built its data with crowdsourcing: ~300 h of crowd-transcribed public audio (Hebrew) — [card](https://huggingface.co/ivrit-ai/whisper-large-v3); crowd-recorded Wikipedia/Michlol snippets and WhatsApp-style prompt recordings (Yiddish) — [card](https://huggingface.co/ivrit-ai/yi-whisper-large-v3)

### Inferences
- **Effort assumptions for this audio** (extrapolation): from scratch 6–10 labor-hours per audio hour, above the ~5× norm because of dense loshn koydesh/Aramaic vocabulary, English code-switching, phone or lecture-hall acoustics, and no agreed orthography. Post-editing 3–5 h per audio hour (≈50% saving per L16-1314/1709.06438), valid only when the draft is ≲25–30% WER. Today's open models may not meet that bar on this register; a specialized commercial draft might.
- **Wage assumption:** $20–$35/h for Yiddish-literate transcribers (Brooklyn/Monsey/Lakewood or Israel freelancers), anchored on the Upwork $17–$22 general and $30–$90 specialized bands, Yiddish translators at $15–$25 (one at $63), and Perle Hebrew at $20.50–$26.50. QC overhead +20% (spot review plus adjudication).
- **Cost per labeled audio hour:**
  - From scratch: 6 × $20 × 1.2 = **$144** to 10 × $35 × 1.2 = **$420**
  - Post-edit: 3 × $20 × 1.2 = **$72** to 5 × $35 × 1.2 = **$210**
  - Gold eval set (full second pass/adjudication ≈1.5× from scratch): **$216–$630** per audio hour
  - Cross-check: agency $1–$7/min = $60–$420 per audio hour; per-audio-hour piece rates in the Hebrew analogs = $30–$195 ($0.50–$3.25/min) for a language with much better ASR drafts.
- **Dataset totals:**

  | Labeled set | Arithmetic | Cost |
  |---|---|---|
  | 50 h post-edited | 50 × $72–$210 | $3,600–$10,500 |
  | 50 h from scratch | 50 × $144–$420 | $7,200–$21,000 |
  | 200 h post-edited | 200 × $72–$210 | $14,400–$42,000 |
  | 300 h mixed (50 scratch + 250 post-edit) | $7,200–$21,000 + $18,000–$52,500 | $25,200–$73,500 |
  | 1,000 h post-edited | 1,000 × $72–$210 | $72,000–$210,000 |
  | 10 h gold test set | 10 × $216–$630 | $2,160–$6,300 |

- **Throughput/calendar:** 50 h post-edit = 150–250 labor-hours (+20% QC ≈ 180–300 h), about 1–2 part-timers for 2–3 months. 250 h mixed = 50×(6–10) + 200×(3–5) = 900–1,500 h (+QC ≈ 1,080–1,800 h), about 5 part-timers at 20 h/week for 11–18 weeks.
- **Guidelines matter for cost and WER.** Fix the orthography (Hasidic vs YIVO spellings, traditional spelling of loshn koydesh words, English in Latin vs Hebrew script), code-switch markup and normalization rules before labeling. Report normalized WER and CER: the Swiss German "convention mismatch" result (25.6% WER vs 13.8% cWER) shows conventions alone can nearly halve or double measured error.

### Gaps
- No wage survey or job postings for *Yiddish* transcribers in Brooklyn/Monsey/Lakewood, and no shekel-denominated Israeli rates, were found; the $20–$35/h band is an inference from adjacent markets.
- Audino, Praat and custom web tools were not researched; Label Studio Enterprise pricing is not public.
- No measured real-time factor for transcribing Hasidic Yiddish specifically.

## 4. Training compute (GPU-hours, GPU type, $), hosted fine-tuning, and commercial adaptation options

### Takeaway
Compute is cheap relative to data. A full fine-tune of large-v3 or turbo on 50–500 h of packed long-form data needs roughly 2–300 A100-hours per run (wide range from first principles and empirical anchors), so a whole experimental campaign costs ~$10–$5,000 at Nov-2026 cloud prices (A100 $1.39–$2.79/h; H100 $1.93–$4.39/h). Turbo saves only ~20% of training FLOPs (the encoder dominates) but much more at inference. No managed "upload data, get a custom Whisper" service was found, and no mainstream cloud API offering *acoustic* adaptation for Yiddish was found; only vocabulary biasing (keyterms) exists.

### Cited Findings
**Empirical training-compute anchors**
- AWS: 5–10 full fine-tuning runs of Whisper ≈ 100 A100 (40 GB SXM4) GPU-hours; full fine-tuning needed 24× the GPU-hours of LoRA in their experiment — [AWS ML blog](https://aws.amazon.com/blogs/machine-learning/fine-tune-whisper-models-on-amazon-sagemaker-with-lora/)
- HF Whisper fine-tuning event: a run takes ~8 h on an A100 40GB; ~100 A100-hours is enough for 5–10 runs — [HF community-events README](https://github.com/huggingface/community-events/blob/main/whisper-fine-tuning-event/README.md)
- FHNW Swiss German: Large-v2 on one A100 40GB with gradient checkpointing, gradient accumulation and an 8-bit optimizer (effective batch 256), a mixed-precision run of ~42 h (dataset size per run not confirmed in extract) — [arXiv 2412.15726](https://arxiv.org/pdf/2412.15726). Their talk: 8-bit optimizers cut AdamW VRAM by ~9 GB; activation checkpointing raised batch from 1 to 16 — [SwissText 2025 write-up](https://kenfus.github.io/asr/research/presentation/2025/06/15/whisper-fine-tuning-swisstext.html)
- Another Swiss German paper: 4–12 epochs on 2× A100 40GB, batch 4 × 32 accumulation, gradient checkpointing — [arXiv 2506.08836](https://arxiv.org/pdf/2506.08836)
- One paper reports a large-v3 epoch takes 12 min on one A100 80GB using up to 37 GB, best at 12 epochs, i.e. ~2.4 GPU-h on a small, unstated dataset (attribution within the extract uncertain) — [arXiv 2404.01737](https://arxiv.org/pdf/2404.01737)
- ivrit.ai Yiddish: 8× A40 for ~15 h over ~97 h of audio (two phases) — [card](https://huggingface.co/ivrit-ai/yi-whisper-large-v3)
- A (non-Whisper) paper measured an H100 at ~2× an A100's training speed — [arXiv 2503.12314](https://arxiv.org/pdf/2503.12314)

**Cloud GPU prices (per GPU-hour, Oct–Nov 2026 snapshots; aggregators, verify live)**
- RunPod pods, Community Cloud: A100 PCIe $1.19, A100 SXM $1.39, H100 PCIe $1.99, H100 NVL $2.59, H100 SXM $2.69, RTX 4090 $0.34, L4 from ~$0.39. Secure Cloud: A100 $1.79 (some list $2.19), H100 PCIe $2.89, H100 SXM $3.99–$4.39, RTX 4090 $0.89 — [Thunder Compute](https://www.thundercompute.com/blog/runpod-pricing-vs-thunder-compute); [gpufinder](https://gpufinder.dev/providers/runpod); [Hackceleration](https://hackceleration.com/labs/runpod-pricing)
- RunPod Serverless (per active second; per one review checked 2026-11-22): 24 GB class (L4/A5000/3090) $0.75/h, RTX 4090 $1.34/h, 48 GB class $1.89/h, A100 80GB $2.99/h, H100 $5.89/h (another source lists H100 at $4.55/h) — [PromptQuorum review](https://www.promptquorum.com/power-local-llm/runpod-review)
- Modal (per-second billing; GPU functions preemptible; CPU/RAM billed extra): H100 ~$3.95/h, A100 80GB ~$2.50/h, A100 40GB ~$2.10/h, L40S ~$1.95/h, A10 ~$1.10/h, L4 ~$0.80/h, T4 ~$0.59/h — [Modal pricing](https://modal.com/pricing)
- Vast.ai live snapshot (Nov 16, 2026), lowest/median: H100 SXM $1.73/$1.93, A100 SXM4 $0.35/$0.84, RTX 5090 $0.37/$0.63, RTX 4090 $0.27/$0.50; interruptible can be >50% cheaper — [markaicode](https://markaicode.com/pricing/vast-ai-pricing/)
- Lambda (Oct 2026 comparison): H100 $3.99/h, A100 80GB $2.79/h; RTX 4090 not offered — [neurohive](https://neurohive.io/en/guides/cloud-gpu-pricing/); [tech-insider](https://tech-insider.org/runpod-vs-lambda-vs-vast-ai-2026/)
- AWS g6.xlarge (1× L4): $0.8048/h on-demand (us-east-1); spot ~$0.35–$0.42 in one snapshot ($0.59–$0.99 in another); interruption rate >20% — [DoiT](https://www.doit.com/compute/spot/us-east-1/g6.xlarge)

**Hosted fine-tuning services**
- No managed upload-your-transcripts Whisper fine-tuning product from Baseten, Replicate, Modal or AutoTrain surfaced. Modal publishes a do-it-yourself Whisper fine-tune-and-deploy example — [Modal example](https://modal.com/docs/examples/fine_tune_asr); Phonexia offers Whisper adaptation on customer data, deployed via Docker (self-run) — [Phonexia docs](https://docs.phonexia.com/products/speech-platform-4/technologies/speech-to-text/enhanced-speech-to-text-built-on-whisper/whisper-adaptation); NVIDIA documents Riva/NIM ASR customization — [NVIDIA docs](https://docs.nvidia.com/nim/riva/asr/latest/customization.html)

**Commercial APIs: custom acoustic/language adaptation for Yiddish**
- AWS Transcribe custom language models accept only en-US, hi-IN, es-US, en-GB, en-AU, de-DE, ja-JP, so there is no Yiddish — [CreateLanguageModel API](https://docs.aws.amazon.com/transcribe/latest/dg/API_CreateLanguageModel.html); a CLM is silently not applied if the audio language does not match — [CLM docs](https://docs.aws.amazon.com/transcribe/latest/dg/custom-language-models.html)
- Azure: the speech-to-text and Custom Speech locale tables exist, but the extracts did not show a Yiddish locale (unconfirmed) — [Azure language support](https://learn.microsoft.com/azure/ai-services/speech-service/language-support)
- Google Chirp 3: Yiddish not found in the language lists seen; whether biasing/model adaptation is supported for chirp_3 conflicts across doc language versions — [Chirp 3 docs](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3); [V2 supported languages](https://docs.cloud.google.com/speech-to-text/docs/languages)
- ElevenLabs offers keyterm prompting (bias toward listed words/phrases): up to 1,000 keyterms of ≤5 words in batch mode; an older help page says it adds 20% to cost. Yiddish support was not confirmed in these searches — [keyterm docs](https://elevenlabs.io/docs/eleven-api/guides/how-to/speech-to-text/batch/keyterm-prompting); [help center](https://elevenlabs.io/docs/help-center/product/core-capabilities/speech-to-text/what-is-speech-to-text)
- AssemblyAI has a Yiddish speech-to-text page (lists Universal-3 Pro); keyterms prompting up to 1,000 words on Universal-3.5 Pro; batch ~$0.21/h, with keyterms possibly +$0.05/h (sources conflict) — [AssemblyAI Yiddish](https://www.assemblyai.com/languages/yiddish); [models doc](https://www.assemblyai.com/docs/getting-started/models); [pricing](https://assemblyai.com/pricing)

### Inferences
- **First-principles FLOPs (my derivation; Whisper large = 32 encoder + 32 decoder layers, d_model 1280, 1,500 encoder frames per 30 s, per the [Whisper paper](https://cdn.openai.com/papers/whisper.pdf); turbo = same encoder, 4 decoder layers, per OpenAI's release, not re-verified here):**
  - Encoder forward: linear 2 × (12 × 1280² × 32 = 629M params) × 1,500 = 1.89 TFLOP, plus attention 4 × 1,500² × 1,280 × 32 = 0.37 TFLOP, ≈ **2.26 TFLOP**.
  - Decoder forward (assume ~250 tokens per 30 s of Yiddish in Whisper's tokenizer): per layer 2 × (14 × 1280² × 250 + 2 × 1280² × 1,500 for cross-attention K/V over encoder frames) = 21.3 GFLOP; × 32 layers = 0.68 TFLOP, + cross-attention scores 0.06 + logits 0.03 ≈ **0.78 TFLOP** (turbo: 4 layers ≈ 0.13 TFLOP).
  - Training ≈ 3 × forward: large-v3 ≈ 3 × 3.04 = **9.1 TFLOP per 30-s sample**; turbo ≈ 3 × 2.39 = **7.2 TFLOP** (≈79% of large-v3, so turbo does not make training much cheaper). With activation checkpointing (+1 forward): ≈12.1 / 9.6 TFLOP.
  - Packed long-form data (~28-s segments) ≈ 130 samples per audio hour, so large-v3 with checkpointing ≈ 130 × 12.1 ≈ **1.58 PFLOP per audio-hour per epoch**. Short, unpacked utterances padded to 30 s cost 3–5× more per audio hour.
  - At ~100 TFLOPS effective on an A100 (≈32% of 312 TFLOPS bf16 peak; assumption): 15.8 s per audio-hour-epoch. With 5 epochs and 1.5–2× overhead (eval, I/O, packing losses): 50 h ≈ 1.6–2.2 A100-h, 200 h ≈ 6.6–8.8, 500 h ≈ 16–22 per run.
- **Cross-check against anchors:** FHNW's ~42 A100-h for a ~1,000-h-scale run ≈ 0.04 A100-h per audio hour, consistent with the low estimate (500 h ≈ 20 A100-h). ivrit.ai Yiddish: 8 A40 × 15 h = 120 A40-h; at A40 ≈ ½ A100 (assumption) ≈ 60 A100-h for ~97 h ≈ 0.6 A100-h per audio hour, ~15× higher (short utterances, many epochs, two phases). AWS/HF: 10–20 A100-h per run.
- **Per-run budget range** (low = packed long-form, 5 epochs; high = ivrit.ai-like efficiency):

  | Labeled hours | A100-h per run | Campaign (≈3–6 full-run equivalents) | $ at A100 $1.39–$2.79/h |
  |---|---|---|---|
  | ~50 h | 2–30 | 6–180 A100-h | ~$8–$500 |
  | ~200 h | 8–120 | 24–720 A100-h | ~$33–$2,000 |
  | ~500 h | 20–300 | 60–1,800 A100-h | ~$83–$5,000 |
  | +2,000 h weak-label pre-training (2–3 epochs, 1–2 runs) | 35–400 total | — | ~$50–$1,100 |

  An H100 at ~2× A100 speed and $1.93–$4.39/h costs $0.97–$2.20 per A100-equivalent hour, the same or cheaper.
- **GPU type:** A100 80GB or H100 for full fine-tuning of large-v3/turbo. A 24 GB RTX 4090/5090 suffices for LoRA, and probably for full turbo fine-tuning with an 8-bit optimizer and checkpointing (rough memory: bf16 weights 3.1 GB + fp32 master 6.2 GB + grads 3.1 GB + 8-bit Adam 3.1 GB ≈ 15.5 GB before activations for large-v3; turbo ~half). Cheapest per FLOP: Vast/RunPod community 4090 at $0.27–$0.50/h, at ~0.5× A100 peak.
- **LoRA's 24× saving (AWS) is probably configuration-specific:** LoRA still runs the full forward/backward pass, so its main saving is memory and optimizer state. Treat LoRA as a way to use cheaper GPUs, not as a 24× cut in compute.
- **Commercial adaptation:** for Yiddish the realistic option today is vocabulary biasing (ElevenLabs/AssemblyAI keyterms, if their Yiddish quality is adequate), not acoustic-model customization. A custom acoustic model means self-training.

### Gaps
- No measured samples/second or GPU-hours for fine-tuning Whisper large-v3/turbo on a stated number of hours; the FLOP model and anchors leave a ~10× uncertainty band (immaterial to TCO, because compute is <5% of costs).
- GCP spot (g2/L4), Together AI and Baseten Training prices not gathered; no confirmation whether Baseten/Replicate/Fireworks host *custom* fine-tuned Whisper checkpoints.
- Azure/Google/ElevenLabs Yiddish support remains unconfirmed; Speechmatics custom dictionary and Deepgram custom-model training were not researched successfully.

## 5. Inference throughput and hosting cost at ~433 h/month

### Takeaway
At ~433 h/month, inference compute is a rounding error: about $10–$60/month on serverless GPUs, $10–$30/month for a nightly batch pod, or ~$250–$600/month for an always-on cloud GPU. Batched faster-whisper/CTranslate2 runs ~12–50× real time even on modest GPUs, and hundreds of times real time for turbo on a 4090. Buying a workstation is hard to justify on inference alone in Nov 2026, when RTX 5090s sell at ~3.5× MSRP.

### Cited Findings
- faster-whisper README (RTX 3070 Ti 8 GB, CUDA 12.4), 13 min of audio with large-v2: openai/whisper fp16 beam 5 took 2m23s; faster-whisper fp16 beam 5 took 1m03s; batched faster-whisper (batch 8, int8) took 16 s — [faster-whisper README](https://github.com/SYSTRAN/faster-whisper)
- TheStageAI benchmark on RTX 4090, large-v3-turbo: unmodified baseline RTFx ≈157 at batch 1 and ≈702 at batch 24 (short-form open_asr evaluation); their optimized variants ≈790–925 — [TheStageAI benchmark](https://github.com/TheStageAI/TheWhisper/blob/main/benchmark/README.md)
- WhisperX claims 70× real time with large-v2, via batched inference plus word-level timestamps and diarization — [WhisperX](https://github.com/m-bain/whisperX)
- Databricks benchmark: large-v3-turbo on a single A10 ≈ $0.054 per audio hour; full large-v3 ≈ $0.123 per audio hour — [whisper-benchmarking](https://github.com/jtaylorisbell/whisper-benchmarking)
- Practitioner estimate: an L4 at $0.43/h running faster-whisper at ~46× real time ≈ $93 for 10,000 audio hours — [DEV](https://dev.to/viswaretas_kotra/how-do-you-transcribe-thousands-of-hours-of-audio-with-whisper-24pb); another knowledge base estimates ~$0.07 per audio hour for L4 + turbo INT8 — [Yobitel](https://yobitel.com/knowledge-base/whisper)
- Cost formula: $/audio-hour = hourly GPU rate ÷ RTFx — [Cerebrium](https://cerebrium.ai/resources/whisper-batch-transcription-cost-per-audio-hour-serverless-gpu)
- An RTX 3080 ran a large-v3 job in ~68% of the A10G's time and ~50% of the T4's — [EarningsCall](https://earningscall.biz/blog/benchmarking-whisper)
- Hosted *stock* Whisper (cannot serve a custom fine-tune) for reference: Groq large-v3 $0.111/h, turbo $0.04/h (other posts cite ~$0.02/h) — [Developers Digest](https://www.developersdigest.tech/models/speech/groq); Fireworks whisper-v3 $0.0015/min ≈ $0.09/h — [Fireworks](https://www.fireworks.ai/models/fireworks/whisper-v3)
- Hardware (US, Nov 2026): RTX 5090 MSRP $1,999; Tom's Hardware's Nov 3 tracker best price $6,919; other snapshots $2,873 to $9,400; EU street >€5,000 (blamed on GDDR7/DRAM shortages and AI-inference demand); no readily purchasable RTX 5090 desktops at Lenovo/HP in early November — [Tom's Hardware](https://www.tomshardware.com/news/lowest-gpu-prices); [tech-insider](https://tech-insider.org/nvidia-rtx-50-prices-rise-november-2026-rtx-5090-3340/); [Guru3D](https://www.guru3d.com/story/geforce-rtx-5090-street-prices-in-eu-now-exceed/); [WindowsForum](https://windowsforum.com/news/rtx-5090-us-availability-7-680-amazon-listing-and-prebuilt-pc-options.451441/). RTX 4090 (launched $1,599, out of production): a used listing at $1,499.99; an Aug-2026 sold-listing average of $2,541 — [tech-insider](https://tech-insider.org/rtx-4090-vs-rtx-5080-used-market-2026/)
- WhisperX's design: VAD segmentation + batched no-timestamp decoding + forced alignment for word timestamps — [WhisperX paper](https://arxiv.org/abs/2303.00747)

### Inferences
- **Real-time factors implied by the README:** 780 s / 143 s ≈ 5.5× (openai/whisper); 780/63 ≈ 12.4× (faster-whisper, beam 5); 780/16 ≈ 48.8× (batched int8) on an 8 GB consumer GPU. Turbo is several times faster again (Databricks: $0.123/$0.054 ≈ 2.3× cheaper per audio hour end-to-end).
- **Monthly GPU-hours = 433 ÷ RTFx:**

  | Setup (assumed RTFx) | GPU-h/month | $/month |
  |---|---|---|
  | Serverless L4, conservative sequential large-v3 (12×) | 36 | × $0.75–$0.80 = $27–$29 |
  | Serverless L4, batched turbo (50×) | 8.7 | × $0.75–$0.80 = ~$7 |
  | Serverless RTX 4090, batched turbo (100×+) | ≤4.3 | × $1.34 = ≤$6 |
  | Databricks-measured per-audio-hour cost | — | 433 × $0.054 = $23 (turbo); 433 × $0.123 = $53 (large-v3) |
  | + pipeline overhead (VAD, diarization, alignment, cold starts) ×1.5–2 | — | ≈ $10–$110, typically **$10–$60** |
  | Nightly batch pod (~1 h/day) | 30 | × $0.34–$0.89 = $10–$27 |
  | Always-on RunPod 4090 / L4 (community) | 730 | × $0.34 / $0.39 = $248 / $285 |
  | Always-on AWS g6.xlarge on-demand / spot | 730 | × $0.8048 = $588; × $0.35–$0.42 = $256–$307 |

- **Own workstation (assumptions flagged):** used RTX 4090 $1,500–$2,541 + ~$1,000–$1,500 for the rest of the PC (assumption) = $2,500–$4,000 capex. Electricity (assumption: 100–150 W average × 730 h = 73–110 kWh × $0.20–$0.30/kWh) ≈ $15–$33/month. Year-1 ≈ $2,700–$4,400, versus $120–$720 serverless. Buy only if it also does training/LoRA experiments, or for data-control reasons; against an always-on cloud GPU ($3,000–$7,000/yr) it breaks even in about a year. RTX 5090 at $2,873–$9,400 makes the case worse.
- **Load profile:** 100 h/week ≈ 14.3 h/day of audio; at 50× that is ~17 GPU-minutes/day. One GPU is enough with large headroom, and turnaround is set by queueing and pipeline steps, not compute.
- **Deployment pattern:** fine-tune in HF Transformers, convert to CTranslate2 for faster-whisper/WhisperX, and run on serverless (Modal/RunPod) triggered per uploaded file or as a nightly batch.

### Gaps
- No reliable long-form benchmark of faster-whisper turbo on L4/T4/A10G/4090/5090 (the TheStageAI numbers are short-form and HF-based); no vLLM-hosted Whisper or whisper.cpp throughput figures were collected.
- HF Inference Endpoints, Baseten and Replicate pricing for custom checkpoints not gathered (Replicate's per-minute stock-Whisper price was seen only in an aggregator and is not used).
- No Yiddish forced-alignment (wav2vec2/CTC) model was checked for WhisperX word-level timestamps.

## 6. Engineering effort, skills, ongoing maintenance and failure modes

### Takeaway
No published person-week benchmarks were found. My estimate is ~6–10 person-weeks for a minimal pilot-to-production build and ~12–20 for a serious one (weak-label pipeline, multiple training rounds), plus ~1–4 days/month upkeep. Most technical risk is in long-form behavior: models fine-tuned on short clips lose timestamp prediction and loop or hallucinate. The documented mitigations are long-form training data with timestamps, VAD, low LoRA scaling, and evaluating in the deployment runtime.

### Cited Findings
- Fine-tuning Large-v2 on sentence-level clips made it lose timestamp prediction and fail as segments got longer; the fix was building a long-form training corpus from the sentence data. Padded sentence-level training caused "a substantial decline in timestamp prediction accuracy, even with VAD" — [arXiv 2412.15726](https://arxiv.org/html/2412.15726v2)
- Sentence-only fine-tunes scored well on validation but failed on long recordings, "often getting stuck transcribing endless sequences of periods"; synthetic long-form data fixed it — [SwissText 2025 talk](https://kenfus.github.io/asr/research/presentation/2025/06/15/whisper-fine-tuning-swisstext.html)
- Catalan project: available fine-tuning scripts "do not generate timestamps tokens and the timestamp prediction degradates"; WER was much worse in the HF inference stack than in CTranslate2, so the authors recommend CTranslate2 — [softcatala TRAINING.md](https://huggingface.co/softcatala/whisper-small-ca/blob/main/TRAINING.md)
- LoRA silence hallucinations of training phrases; fixed by lowering α/r — [arXiv 2606.07608](https://arxiv.org/pdf/2606.07608)
- Whisper's long-form decoding depends on timestamp tokens to advance the 30-s window; beam search and an initial-timestamp constraint reduce looping — [Whisper paper](https://cdn.openai.com/papers/whisper.pdf)
- WhisperX notes that conditioning on previous text is more prone to hallucination and repetition, hence its no-timestamp, VAD-segmented batched decoding — [WhisperX paper](https://arxiv.org/pdf/2303.00747)
- whisper.cpp users report hallucination on silence; one user reports a 4× WER reduction in faster-whisper with without_timestamps=True (anecdotal) — [whisper.cpp #1724](https://github.com/ggml-org/whisper.cpp/issues/1724)
- Adalat AI: models trained mostly on studio speech degraded on spontaneous or noisy audio; they retrained from the base checkpoint with all quality tiers mixed — [Vividh-ASR blog](https://huggingface.co/blog/adalat-ai/vividh-benchmark)
- Timestamp drift after fine-tuning is a recognized problem with dedicated 2026 research (title: "Forgetting-Resistant Correction of Timestamp Drift in ASR via Replay-Based Distribution Editing") — [arXiv 2607.05364](https://arxiv.org/pdf/2607.05364)
- A fine-tuned model "still requires a production pipeline before it can handle… speech recognition reliably" — [F22 Labs](https://www.f22labs.com/blogs/how-to-fine-tune-whisper-small-for-better-speech-recognition/)
- Experiment budgets are planned as 5–10 runs (~100 A100-h), so iteration is the norm — [HF event README](https://github.com/huggingface/community-events/blob/main/whisper-fine-tuning-event/README.md)

### Inferences
- **Work breakdown (person-weeks; estimate):**

  | Work package | Minimal (50 h) | Serious (250 h + weak labels) |
  |---|---|---|
  | Eval harness, gold-set curation, baselining existing checkpoints/commercial tools | 1–2 | 1.5–2.5 |
  | Data pipeline: ingest, VAD segmentation, draft generation, Label Studio setup, transcriber guidelines, QC tooling, export with long-form packing + timestamp tokens | 2–4 | 4–6 |
  | Weak-label pipeline (bulk transcription, agreement filtering, alignment of found texts) | — | 2–3 |
  | Training experiments (full FT vs LoRA, replay ratio, turbo vs large-v3, 2–3 rounds) | 1–2 | 3–5 |
  | Deployment: CTranslate2 conversion, batched faster-whisper/WhisperX service, VAD + hallucination guards, serverless deploy, monitoring | 1.5–2 | 2–3 |
  | Docs/handover | 0.5 | 0.5–1 |
  | **Total** | **6–10.5** | **13–20.5** |

- **Ongoing:** 1–2 days/month (minimal) or 2–4 days/month (serious) for drift monitoring, quarterly retraining with newly corrected data, dependency/runtime updates, and handling new speakers or audio sources.
- **Skills:** an ML engineer comfortable with PyTorch/HF Transformers, audio (ffmpeg, VAD, sampling rates) and evaluation; a data/annotation lead fluent in Hasidic Yiddish and loshn koydesh to own guidelines and QC; light DevOps. A single strong contractor plus a part-time Yiddish data lead is the minimum team.
- **Failure-mode checklist for acceptance testing:** (1) silence/music hallucination: VAD, no-speech threshold, compression-ratio filters, conservative LoRA α/r; (2) repetition loops: beam search, condition_on_previous_text=False, temperature fallback; (3) long-form chunking: train on packed long-form samples *with* timestamp tokens, test on full-length lectures; (4) timestamp accuracy for headings/clips: measure segment-boundary error on the gold set (forced alignment needs a Yiddish-capable aligner, unverified); (5) English/Hebrew forgetting: replay data plus separate eval slices; (6) orthographic inconsistency: guidelines plus normalized WER/CER; (7) runtime mismatch: evaluate in the production runtime (CTranslate2), not only in HF Transformers.

### Gaps
- No published effort or timeline data for comparable Whisper fine-tuning projects (searches for CARS24/Gladia write-ups returned nothing usable).
- Contractor/engineer rates were not researched (assumption used in Section 7).
- Availability of a Yiddish phoneme/CTC alignment model for word-level timestamps was not checked.

## 7. 12-month total cost of ownership: self-built scenarios vs paying a commercial API per hour

### Takeaway
Year-1 fully loaded TCO is ≈$30k–$120k for a minimal 50-h fine-tune and ≈$75k–$280k for a serious 250-h + weak-label build, dominated by engineering and labeling, not GPUs (compute + inference <5%). Cash-only cost (if staff time is already paid) is ≈$5k–$16k and ≈$24k–$74k. At 5,200 h/year, self-building pays back within year 1 only if the commercial alternative costs more than ≈$6–$23/h (minimal) or ≈$14–$54/h (serious) fully loaded, or ≈$1–$3/h and ≈$5–$14/h cash-only. Year-2+ run-rate is ≈$9k–$45k (minimal) or ≈$16k–$78k (serious) fully loaded, ≈$2k–$11k cash-only.

### Cited Findings
- Unit-cost inputs (detailed above): Yiddish human transcription $1–$7 per audio minute — [Transcription Services US](https://www.transcription-services-us.com/Yiddish-Transcription.php), [Yiddish Translations](https://www.yiddish-translations.com/pricing); post-editing ≈2× faster than from scratch — [L16-1314](https://aclanthology.org/L16-1314.pdf); A100 $1.39–$2.79/h — [Thunder Compute](https://www.thundercompute.com/blog/runpod-pricing-vs-thunder-compute), [neurohive](https://neurohive.io/en/guides/cloud-gpu-pricing/); serverless L4-class $0.75–$0.80/h — [PromptQuorum](https://www.promptquorum.com/power-local-llm/runpod-review), [Modal](https://modal.com/pricing)
- Floor references for *commodity* ASR (stock models, not adapted to Hasidic Yiddish): Groq large-v3 $0.111/h — [Developers Digest](https://www.developersdigest.tech/models/speech/groq); Fireworks ≈$0.09/h — [Fireworks](https://www.fireworks.ai/models/fireworks/whisper-v3); AssemblyAI Universal-3.5 Pro batch ≈$0.21/h with a Yiddish landing page (quality on this register unknown) — [AssemblyAI](https://www.assemblyai.com/languages/yiddish)

### Inferences
**Assumptions (explicit):** transcriber $20–$35/h; from-scratch label $144–$420/audio-h; post-edit $72–$210/audio-h; gold set $216–$630/audio-h (Section 3). Engineering at **$75–$175/h, i.e. $3,000–$7,000 per person-week (unsourced assumption, US contractor range; adjust to actual staff cost)**. Training compute per Section 4. Inference serverless $10–$60/month.

**Scenario A: minimal (6-h gold set + 50 h post-edited, turbo full FT or LoRA, serverless)**

| Item | Arithmetic | Year-1 |
|---|---|---|
| Gold set 6 h | 6 × $216–$630 | $1,296–$3,780 |
| 50 h post-edited | 50 × $72–$210 | $3,600–$10,500 (from-scratch fallback if drafts >30% WER: $7,200–$21,000) |
| Training compute | Section 4 | $20–$500 |
| Engineering 6–10 pw | × $3,000–$7,000 | $18,000–$70,000 |
| Inference | 12 × $10–$60 | $120–$720 |
| Maintenance 1–2 days/mo | 96–192 h × $75–$175 | $7,200–$33,600 |
| **Total (fully loaded)** | | **≈$30,200–$119,100** |
| **Cash-only (excl. engineering/maintenance)** | | **≈$5,000–$15,500** |

**Scenario B: serious (10-h gold set + 50 h from scratch + 200 h post-edited + ~2,000 h filtered pseudo-labels, full FT with replay)**

| Item | Arithmetic | Year-1 |
|---|---|---|
| Gold set 10 h | 10 × $216–$630 | $2,160–$6,300 |
| 50 h from scratch | 50 × $144–$420 | $7,200–$21,000 |
| 200 h post-edited | 200 × $72–$210 | $14,400–$42,000 |
| Pseudo-labeling 2,000 h (inference) | 40–100 GPU-h × $0.75–$2.99 | $50–$300 |
| Training compute (incl. weak-label pre-training) | Section 4 | $150–$3,700 |
| Engineering 12–20 pw | × $3,000–$7,000 | $36,000–$140,000 |
| Inference | 12 × $10–$60 (always-on would be $3,000–$7,000) | $120–$720 |
| Maintenance 2–4 days/mo | 192–384 h × $75–$175 | $14,400–$67,200 |
| **Total (fully loaded)** | | **≈$74,500–$281,200** |
| **Cash-only** | | **≈$24,100–$74,000** |

**Scenario C: bootstrap from a commercial tool (optional, subject to vendor ToS).** Keep paying the specialized vendor for ~3 months (≈1,300 h × vendor price P/h). Post-edit 200 h of its drafts ($14,400–$42,000, the cheaper post-edit rate if its drafts are ≲25% WER), use the rest as filtered pseudo-labels, then switch to the own model. Year-1 ≈ 1,300 × P + Scenario-B costs minus the 50-h from-scratch block ($7,200–$21,000) and ~2 pw of pipeline work, plus the vendor's cost during the build.

**Year-2+ run-rate:** inference $120–$720 + maintenance + refresh labeling of 25–50 h post-edited ($1,800–$10,500). Scenario A (maintenance $7,200–$33,600): **≈$9,100–$44,800/yr** fully loaded. Scenario B (maintenance $14,400–$67,200): **≈$16,300–$78,400/yr**. Cash-only for either: **≈$1,900–$11,200/yr** (≈$0.37–$2.16 per audio hour).

**Commercial comparison at 5,200 h/yr:** annual spend = 5,200 × P. P = $0.25 → $1,300; $1 → $5,200; $3 → $15,600; $6 → $31,200; $12 → $62,400; $30 → $156,000 (human Yiddish transcription at $1–$7/min = $312,000–$2,184,000/yr).

| Scenario | Year-1 breakeven price P* = TCO ÷ 5,200 h | Two-year breakeven = (Y1 + Y2) ÷ 10,400 h |
|---|---|---|
| A fully loaded | $30.2k–$119.1k → **$5.8–$22.9/h** | ($39.3k–$163.9k) → **$3.8–$15.8/h** |
| A cash-only | $5.0k–$15.5k → **$1.0–$3.0/h** | ($6.9k–$26.7k) → **$0.7–$2.6/h** |
| B fully loaded | $74.5k–$281.2k → **$14.3–$54.1/h** | ($90.8k–$359.6k) → **$8.7–$34.6/h** |
| B cash-only | $24.1k–$74.0k → **$4.6–$14.2/h** | ($26.0k–$85.2k) → **$2.5–$8.2/h** |

**Interpretation and risk adjustments:**
- If specialized Hasidic-Yiddish tools cost on the order of $10–$30 per audio hour (to be filled from the commercial-pricing research), a minimal or serious self-build plausibly pays back in 1–2 years. If they cost ≲$3/h, building only makes sense for control/privacy or with nearly free in-house engineering.
- These TCOs assume the self-built model reaches "good enough for summaries". That is unproven: a 50-h fine-tune may not match a vendor trained on far more Hasidic audio. Budget for a fallback where the vendor handles hard content.
- Calendar time is about 3–6 months to production (labeling throughput in Section 3), and the client keeps paying the current provider during that period: 3–6 × 433 h × P.
- **Staged, low-regret path:** Stage 0: build a 6–10 h gold set ($1.3k–$6.3k) and benchmark the commercial tools and open checkpoints on it; this is useful whatever the decision. Stage 1: 50-h pilot (cash ≈$5k–$16k). Stage 2: scale to 200–300 h plus pseudo-labels only if the pilot shows a large relative WER/CER gain and acceptable LLM summaries on held-out lectures.

### Gaps
- Specialized commercial Hasidic-Yiddish ASR prices and their Hasidic-register accuracy are covered by other researchers and were not verified here; breakeven depends entirely on them.
- Engineering rates are an unsourced assumption; the TCO is very sensitive to them (engineering is 50–75% of fully loaded cost).
- No evidence was gathered on the WER/CER threshold at which LLM-generated summaries and headings remain accurate for this content, which determines how much data is "enough".
- Vendor terms on using outputs for training (Scenario C) not checked.
