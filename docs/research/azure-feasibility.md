# Azure feasibility for the Dutch study-aid pilot

Research date: **2026-10-24**. Documentation investigation only; no provisioning, uploads, inference, performance experiments, or paid API calls. All examples below are synthetic. This is a throwaway research artifact, never an implementation branch.

Question: [Which Azure model, document and speech options can meet the pilot’s requirements?](https://github.com/amolenk/socra/issues/24). Architecture authority: the latest resolution of [What source representation and system boundaries are sufficient for the pilot?](https://github.com/amolenk/socra/issues/12#issuecomment-6382278885). Human stack, configuration, disclosure and budget decisions remain in [Which processing stack and operating budget should the pilot use?](https://github.com/amolenk/socra/issues/26).

## Finding

**There is a small, documented candidate set worth testing later, but documentation does not establish that any complete stack meets the pilot’s Dutch explanation, source-support and continuous-voice requirements together.** The practical comparison is:

1. NAS-controlled retrieval and answer formation using **GPT-4.1-mini / GPT-4.1**, with **GPT-5.4-mini** as a reasoning-capable comparator; local document rendering/extraction or **Document Intelligence v4 Read/Layout**; continuous **Azure Speech STT → LLM → standard neural TTS**.
2. The same NAS source boundary with **Voice Live**, comparing a text-based `gpt-4.1-mini` configuration against a native `gpt-realtime` configuration. Managed audio orchestration helps, but Dutch automatic-turn behavior and release-before-support checking remain unresolved.
3. **Direct Azure OpenAI GPT Realtime**, initially `gpt-realtime`, with NAS-controlled tools/context. It supports audio, image input and interruption events, but has a different deployment/API boundary and lacks Voice Live’s documented server echo-cancellation enhancements. [M1, M2, S1–S5]

These are candidates, not selected vendors/configurations. A small pilot needs no catalog sweep. Newer GPT-6.1 and Realtime-2.1 models are present in current Microsoft documentation; their existence does not demonstrate pilot superiority. The latter is relevant where a documented EU Data Zone route is needed. No claim here ranks models by measured quality or speed. [M1, M2, S2]

## Scope and evidence method

- App modules, immutable originals, extraction, indexes, topic data and saved chats/summaries persist on the NAS. Relevant chat/source text and selected images may go to Azure Foundry; Azure speech may receive audio and speech-output text. The Mac Mini is only a possible speech fallback. The earlier all-local assumption is superseded by the linked architecture resolution.
- Continuous voice stays learner-enabled, with automatic turns, spoken interruption/correction, visible text and original-source access. Ordinary first-response targets remain about **2 seconds voice / 5 seconds text**, measured from the learner’s actual turn end or text submission. They are targets, not service or measured guarantees.
- Microsoft Learn EN-US pages and vendor-maintained API/license documentation were fetched on the research date. Sources at the end include canonical links and selected published revision identifiers. A page’s update date is not proof that every sample or table is consistent. Regional tables establish **documented offerings**, not deployable subscription capacity today.
- Dynamic pricing returned `$-` for relevant rates. No numeric rate is presented as a verified current quote. Failed old `/ai-foundry/` routes were retried through canonical `/foundry/` routes where available. Actual tenant quota, capacity, contractual program, Document Intelligence region inventory and some service-version specifics remain unverified.

## 1. Grounded multimodal explanation shortlist

| Candidate | Documented interface/capability | Practical reason to compare | Remaining uncertainty |
| --- | --- | --- | --- |
| `gpt-4.1-mini`, `2025-04-14` | Text + images in, text out; Chat Completions and Responses; streaming, functions and structured output. | Straightforward controlled answer pipeline; standard/regional and Data Zone options are documented. Also available as a Voice Live text-based model. | Dutch 4 VWO pedagogy, subject reasoning, visual fidelity, support checking and useful first-output latency. |
| `gpt-4.1`, `2025-04-14` | Same modality/API family. | Same-family comparator for difficult formulas/diagrams and explanation quality; useful without changing adapters. | Larger model is not automatically more faithful; price and latency difference must be measured/quoted. |
| `gpt-5.4-mini`, `2026-03-17` | Text/image processing, reasoning, functions, structured outputs, Chat Completions and Responses. | Reasoning-capable comparator using the same original-region evidence. | Reasoning effort/output budgeting may increase delay/cost; no pilot-specific quality evidence. Not listed in the current Voice Live predeployed model table. |

Capability source: [M1]. None of these rows is marked preview in that model table. Pin exact versions and check retirement/upgrade policy at selection time; avoid assuming a `latest` alias is a fixed comparison. GPT-4.1’s published context allowance depends on deployment type; the table documents a 300,000-token standard limit and lower provisioned/batch limits. Its enormous theoretical context is no reason to send full collections. [M1]

The explanation pipeline can send scoped passages, table structure and original crops together. Image requests support URL or inline base64 image content; inline selected crops avoid requiring Azure to fetch a NAS URL or making originals publicly accessible. Models understand images, but this API does not produce a reliable original-page coordinate extraction contract. Images also incur input usage, with model-specific token accounting. [M3]

**Grounding is an application responsibility.** Separate:

1. retrieval relevance (did we find the useful material?);
2. pointer integrity (does the ID/page/region resolve to the actual original?);
3. transcription fidelity (were symbols, text, units, headers and relationships extracted correctly?);
4. claim support (does that material justify each academic assertion, including derivations?);
5. coverage (are omitted/unreadable areas and supplementation explicit?).

Structured output can constrain the shape of source IDs; it cannot prove that the claims are supported. A second model pass can find defects but can share the generator’s mistakes. For example, a correct pointer to synthetic `v = s/t` does not support a generated claim that doubling `s` doubles `v` if `t` is also changing. The original diagram, surrounding qualifiers and derivation matter. This distinction follows the architecture evidence boundary, not an Azure accuracy guarantee.

No retrieved original, OCR text, web page, tool result or generated interpretation may authorize another chat/topic lookup or extra disclosure. NAS tool handlers must enforce scope independently of model instructions. Managed Agents, file stores and hosted retrieval are not necessary to use the shortlist; enabling them adds persistence and service boundaries. [M4, P1]

### Model API/container boundary

NAS backend → authenticated HTTPS Azure model endpoint → streamed text/tool proposals → NAS validation/release → client text/source view. Azure deployment **name**, model ID/version, deployment type and resource region are distinct values. Responses uses `/openai/v1/responses`; Chat Completions is a separate surface. SDK choices include Python, .NET, JavaScript/TypeScript and Java; keys or Entra token authentication are documented. Do not infer that an ordinary household NAS automatically has an Azure managed identity. The concrete credential/access decision is outside this research. [M4]

For NAS-owned conversation state, compare stateless Chat Completions against Responses with explicit `store=false` and client-carried context. Responses otherwise retains response data **30 days by default** and has a delete API. Microsoft documents stateless input-array chaining; `previous_response_id` is a service-state workflow, not the NAS history itself. Disabling response storage does **not** disable abuse monitoring. Background Responses mode has documented streaming-performance issues; background document ingestion on the NAS need not use that mode. [M4, P1]

## 2. Document extraction and inspectable originals

| Option | Documented output | Fit and limitation |
| --- | --- | --- |
| Local native PDF extraction/rendering + Tesseract 5 for scanned regions | PDF.js text content, page rendering/viewports; Tesseract `nld` data and hOCR/TSV word boxes/confidence. | No OCR cloud transmission; avoids paying for native text extraction. App must group tables/formulas and preserve image regions; ordinary OCR is not a semantic diagram or formula authority. Hardware/photograph performance unmeasured. |
| Document Intelligence **v4.0 `2024-11-30` GA `prebuilt-read`** | Text, paragraphs, lines/words, positions for rendered PDF/images; optional searchable PDF. | Narrow OCR comparator for simpler pages. Searchable PDF output has no additional output-generation charge, but analysis remains billable. It is a derivative, not a replacement original. |
| Document Intelligence **v4 Layout `prebuilt-layout`**, optionally `features=formulas` / `ocrHighResolution` | Text, roles/sections, tables/cells/merged spans, figures/captions, polygons; formula LaTeX and polygon with the formula add-on. | Best documented extraction contract here for mixed textbook layout. Add-ons cost extra; detection/LaTeX is fallible and is not explanation. Figure/table crops omit captions/footnotes from the core bounding region. |

Sources: [D1–D6, L1–L3]. Azure Read/Layout support printed Dutch (`nl`). **Dutch is absent from their documented handwritten-language list**; do not transfer printed-language support to handwritten Dutch. Formula confidence is explicitly **hard-coded**, so a value such as 0.99 is not calibrated evidence of fidelity. [D3, D4]

Layout accepts PDF, common images and Office formats, but **native PPTX/DOCX/XLSX/HTML ingestion does not render those files or return geometric regions, and embedded/linked images are unsupported**. For slide originals with diagrams, consider a NAS-generated PDF/page render with an explicit slide→render mapping. Keep the original slide file and immutable render; do not claim native Office extraction covers its visuals. Rendering fidelity, fonts, annotations and hidden content require later verification. [D1, D2]

Cloud Document Intelligence is a **separate processor/service choice**. Permission to send selected images to a Foundry model is not a blanket selection of whole-document OCR uploads. Researching it is within this ticket; choosing actual submitted pages, use of the service and its 24-hour processing storage belongs to the human stack/disclosure decision.

Its boundary is NAS background worker → HTTPS `POST /documentintelligence/documentModels/{modelId}:analyze?api-version=2024-11-30` → poll `Operation-Location` → persist scoped results on NAS. REST accepts `base64Source` or `urlSource`, subscription-key or OAuth authentication, selected `pages`, `features`, `outputContentFormat` and optional figure/PDF output. A NAS private file need not become a public URL. [D7]

### Coordinate handling that the representation must preserve

This is an integration implication, not implemented code:

- Retain immutable original identity/hash, physical page/slide index, displayed printed page label separately, extraction engine/model/API version, and submitted-page→original-page mapping. Split-file numbering must not become original numbering.
- Keep raw JSON geometry and text spans, page width/height/unit/orientation, element IDs, table/cell row/column spans, captions and footnotes. Markdown alone loses the geometric contract. DI pages/regions are **1-indexed**; coordinates are typically **pixels for images / inches for PDFs**, not normalized fractions. [D2]
- Spans index the top-level extraction `content`, not the original file bytes. Record the actual REST `stringIndexType`/SDK indexing choice and returned content. REST documents `textElements`, `unicodeCodePoint` and `utf16CodeUnit`; the explanatory page contains a `stringIndexIndex` typo. Derived text needs explicit mapping to raw spans. [D2, D7]
- Store transformations for image EXIF orientation, rotation/deskew, scaling, cropping and PDF visible boxes/viewport. Overlay polygons on the appropriate original render, accounting for coordinate origin and unit conversions. PDF.js provides page rotation, visible user-space box, user unit and viewport transforms. Test rotated/cropped pages; multiplying every value by 72 is not a complete mapping. [D2, L1]
- Keep figure/caption/footnote links and table headers together. DI figure IDs are documented as following an **undocumented convention**; treat them as returned opaque IDs rather than parsing their shape. Fetch required crops/results before retention expiry, or reproduce crops from the retained original geometry. [D1, D2, P4]
- Mark unusable regions excluded as reliable evidence, preserving the original for inspection; do not silently repair illegible text with a plausible model answer. Unprocessed pages are not available evidence. Coverage is separate from whether the API job succeeded.

S0 documented limits: **500 MB, 2,000 pages**, default **15 analyze requests/s and 50 get operations/s**. F0: **4 MB, first 2 pages**, 1 request/s; the pricing page lists 500 free pages/month but excludes premium features. These are limits, not promised throughput/latency. Poll asynchronous `Operation-Location`, honor retry guidance, and keep ingestion outside the ordinary response critical path. [D5, D6]

## 3. Continuous speech: three practical approaches

| Approach | Automatic turns / interruption | Source-grounded answer boundary | Main trade-off |
| --- | --- | --- | --- |
| **Speech SDK continuous STT → NAS-controlled LLM → streaming standard neural TTS** | Continuous STT emits partial/final results. App coordinates turn completion, microphone activity, cancellation and actual playback stopping while continuing to listen. | Most direct way to use the same answer/support pipeline for voice and text; only released supported text reaches TTS. | More engineering and serial work; ordinary two-second first useful speech remains unproven. STT segmentation is not a complete dialogue turn manager. |
| **Voice Live**, `gpt-4.1-mini` or `gpt-realtime` | WebSocket events; server VAD, semantic variants, interruption, noise suppression and server echo cancellation; cancellation/truncation controls. | Scoped source tools are documented (VoiceRAG). Must verify that retrieval/checks precede academic audio release and that interruption does not race with tool work. | Managed orchestration reduces audio plumbing, but predeployed model/region, VAD language, speech routing and API-version restrictions matter. |
| **Direct GPT Realtime** | Server/semantic VAD, start/stop events, cancel and truncate; WebRTC or server WebSocket. | Image input and function calling permit NAS evidence access. A streaming speech answer still needs the same source-support boundary. | Native speech may preserve conversational cues; separate transcript may be imperfect. Raw speed does not justify unsupported speech. App handles echo/playback/context synchronization. |

Sources: [S1–S9]. Text-based Voice Live is itself managed STT+LLM+TTS, not a guarantee that it removes the reasoning/retrieval steps. The documentation’s marketing comparison claiming DIY cannot achieve low latency is not evidence that a controlled pipeline cannot meet this pilot.

### Dutch support and turn-mechanics caveats

- Azure STT lists `nl-NL` and `nl-BE`; standard TTS includes **`nl-NL-FennaNeural`, `nl-NL-MaartenNeural`, `nl-NL-ColetteNeural`**. Select a voice later after pronunciation/tone checks. Spelling out formulas, units, decimal commas and ambiguous symbols is an answer/TTS formatting concern, not ordinary prose pronunciation. [S5]
- Voice Live with `azure-speech` supports these locales, but its **empty-language default multilingual model omits Dutch**. The documented implication is to specify a supported Dutch locale such as `nl-NL` when comparing that path. Native `gpt-realtime` / mini list Dutch; the page’s broad language criterion does not establish school-level recognition quality. It does not separately certify every newer Realtime-2.1 variant. [S4]
- Voice Live’s `azure_semantic_vad` primarily supports English; `azure_semantic_vad_multilingual` lists ten languages **without Dutch** and says other languages are ignored. General multilingual STT or native Dutch model support does not establish Dutch semantic VAD support. Compare automatic silence-based VAD and a separately verified semantic option; do not quietly change to push-to-talk or require a button for each turn. [S3]
- Standalone Speech SDK **semantic segmentation is not recommended for interactive scenarios**. Continuous recognized segments are not inherently a learner’s whole turn. Its silence timeout trades shorter latency against cutting off pauses; preserve longer Dutch hesitations and self-corrections in the later test. [S7]
- On interruption: stop local audible playback, discard buffered future audio, cancel in-flight generation, ignore stale tool/results using turn IDs, and synchronize server history to what was actually heard. GPT Realtime documents `response.cancel` and `conversation.item.truncate` with playback position; truncation removes unheard transcript text from server context. This transient synchronization does not introduce an original-source deletion/revision system. [S2]
- Voice Live documents `interrupt_response` and `auto_truncate` (default false) for Azure semantic VAD. Test their behavior with the Dutch candidate rather than assuming they apply identically to `server_vad`. Echo suppression and noise suppression are separate from robust barge-in. [S3]
- Voice Live’s default server echo reference assumes prompt playback; **more than two seconds of client playback delay can degrade echo cancellation**. Live-reference AEC takes actual playback-reference audio alongside microphone audio (interleaved two-channel PCM16), available from **API `2026-07-15`**. Its main endpoint example uses `2026-04-10`; those version boundaries must not be conflated. Support-check buffering may make this timing issue important. [S3]
- Direct GPT Realtime documents near-/far-field noise reduction, not Voice Live’s `server_echo_cancellation` extension. Browser/device AEC and playback tracking need verification. Disabling the mic while speaking would remove spoken interruption and therefore cannot be silently substituted.

### Transport and runtime boundaries

**NAS relay:** browser/device microphone → NAS → Speech/Voice Live/Realtime over authenticated encrypted transport; output → NAS → device. Credentials can remain server-side; capture/playback still happen on the actual learner device, not automatically on the NAS. SDK input streams make a relay possible. Exact NAS CPU architecture/native-library support must be checked later. [S3, S7, S10]

**WebRTC:** direct client↔service media can avoid a NAS audio relay while NAS controls tools/session through a side channel. It sends microphone audio directly to Azure and needs explicit session/credential scoping. Current **Voice Live WebRTC documentation uses a `2026-01-01-preview` calls endpoint and explicitly says global-standard routing**. Do not assume its transport honors a Voice Live Data Zone model option merely because the WebSocket model table lists that option. The article also has conflicting event-routing descriptions and a duplicated SDP sample property; verify protocol/version behavior before depending on it. [S6]

Direct Azure OpenAI Realtime documentation specifies the GA `/openai/v1/realtime` API (no date `api-version`) and recommends WebRTC for client audio, WebSocket for server integration. Its examples mix older flat session fields with the GA session model; use the matching reference/SDK contract. Max documented session length is **60 minutes**; renewal must preserve scoped context and learner enable/pause/stop state. No unsolicited activity on reconnect. [S2]

Streaming/preconnection can reduce setup/finish wait, but does not prove first useful-response latency. Speech TTS supports streamed audio, reusable connections and **text streaming in C#/C++/Python at its WebSocket v2 endpoint**, with no SSML support for that text-streaming feature. It is not safe to assume JavaScript feature parity. Chunked supported sentences can instead use ordinary synthesis, at the cost of extra coordination and prosody boundaries. [S8]

**The central feasibility risk:** full-answer checking before any speech adds delay; unchecked native audio may violate the evidence contract. Consider later measuring supported-sentence/chunk release with stable sources and proper cancellation, but do not select that design or declare its checks reliable here. A filler acknowledgment is not a useful first academic response for target measurement.

## 4. Regions, release status, capacity and API compatibility

These are **documentation observations on 2026-10-24**, not authenticated availability checks. [M1, M2, M5, S1–S6, S9]

| Candidate boundary | Specific documented regional observation | Caveat |
| --- | --- | --- |
| Direct GPT-4.1-mini | West Europe and Sweden Central listed for Global Standard, EU Data Zone Standard and regional Standard. | Resource region, chosen SKU/version, allocated quota and actual capacity all need later confirmation. |
| Direct GPT-4.1 | Both listed for Global/Data Zone; regional Standard listed in Sweden Central, not West Europe. | Do not treat availability of mini as availability of full under the same deployment type. |
| Direct GPT-5.4-mini | Both listed for Global/Data Zone Standard; absent from regional Standard table. | Quota overview omits some Data Zone tier rows that the region matrix contains. Reconcile with actual selected offer. |
| Direct GPT Realtime | `gpt-realtime`/mini Global Standard listed in France Central and Sweden Central; not West Europe. `gpt-realtime-2.1` also listed for EU Data Zone in France Central/Sweden Central. | Overview says realtime models are global, while the newer regional matrix includes Data Zone offerings. Route/API/SKU must be reconciled; not a universal global-only or EU-only claim. |
| Voice Live WebSocket | `gpt-4.1-mini`: West Europe Data Zone, Sweden Central Regional. `gpt-realtime`: Sweden Central Global; `gpt-realtime-datazone` also listed there. No native realtime row available in West Europe. | These managed names/scopes are not direct Azure OpenAI deployment names/SKUs. |
| Voice Live speech components | HD voices Regional in West Europe/Sweden Central; `mai-transcribe-2` preview Data Zone there; streaming MAI preview Global in Sweden Central, unavailable in West Europe. | LLM scope is independent of speech-component scope; one regional endpoint is not a whole-stack residency guarantee. |
| Standalone Speech | Core STT and neural TTS listed in West Europe and Sweden Central. | Voice availability/capacity and features differ. Documented 429s can be voice capacity, not account quota. |
| Document Intelligence v4 | Read/Layout documented GA; processing privacy page specifies same resource region. | Exact current West Europe/Sweden Central resource/API availability and household subscription access were not independently verified; check before selection. |

Global processing can leave the resource geography; EU Data Zone can process across EU countries; storage geography and processing scope are different. The generic Foundry privacy page says standard processing may move between regions within the geography for operations, while the regional availability page describes processing in the deployment region. Treat strict single-region interpretation as a fact to clarify, not an assurance extracted from the shorter table. [P1, M2]

Voice Live overview lists its normal service/models without a blanket preview label, explicitly marking Phi/MAI features preview. WebRTC examples still use a preview API. GA LLM capability does not make every speech transport, SDK feature or preview integration GA. Supplemental preview terms (June 2026) impose important live-use/early-access restrictions and potentially different abuse practices; evaluate the exact feature’s terms before a live pilot. [S1, S3, S6, T3]

Quotas are not a cost ceiling or latency SLA. Direct model quota tiers depend on offer/payment/usage; shared subscription/global/data-zone quota management is rolling out, not universally the old per-region pool. GPT-4.1 permits max 50 images per request in the quotas table. Voice Live documents S0 **100 new connections/min, 120,000 tokens/min, <=60-minute sessions**, no F0; its later NCPM×4,000 formula/example conflicts with those defaults. Verify the resource’s effective limits, not a computed entitlement. Standalone STT defaults to 100 concurrent S0 requests; TTS 30 TPS. Speech quota-increase application currently requires an organization/work email and rejects personal email, a potential household limitation. [M5, S9]

## 5. Service-specific data handling

**“Not used for training”, “stateless model”, “no routine app logs”, “not stored at rest”, and “no human access” are different claims.** NAS persistence does not prevent cloud processing storage, moderation or explicitly enabled diagnostics.

| Service / mode | Transmission and retention documented | Training / abuse / human access distinction |
| --- | --- | --- |
| **Foundry Models sold by Azure, including Azure OpenAI** | Selected prompt/chat text, images, embeddings inputs and generated output. Base model is stateless. Optional Responses/Files/vector stores/Agents create service state; Responses default 30 days, `store=false` available. | Inputs/outputs/embeddings not available to OpenAI/other model providers; no foundation-model training or Microsoft/provider product improvement without explicit permission/instruction. Automated harmful-content/abuse review is distinct; sampled flagged/pattern content may be stored for authorized Microsoft human review. Current fetched privacy/abuse pages do **not specify an exact human-review storage duration**. Do not substitute Responses’ 30 days. [P1, P2, M4] |
| **Modified Foundry abuse monitoring** | Approval removes described abuse storage/human-review process, not all automated review or optional feature storage. | Limited Access; only managed-account-team or eligible-program customers can apply, with additional requirements. Household eligibility and approval unverified. Setting `store=false` or ordinary filtering thresholds is not this approval. `ContentLogging=false` is documented verification after approval. [P1–P3] |
| **Document Intelligence Read/Layout analysis** | Submitted pages/files and analysis results temporarily encrypted in same-region Azure storage; **24 hours after completion**, then automatic deletion; v4 Delete Analyze Result removes input and result earlier. | Authentication/logical isolation from other customers. Its privacy page does not establish a service-specific blanket no-training guarantee or no-human-access guarantee; do not import Azure OpenAI abuse policy. Custom-training data/model storage is separate. Confirm applicable DPA/support/diagnostics handling for chosen prebuilt analysis. [P4] |
| **Standalone real-time Azure STT** | Microphone/audio stream; processed in Azure server memory, no storage at rest; real-time, fast transcription and translation described as no retention of customer data. | That mode’s no-content-retention statement is not the OpenAI abuse-review policy. Custom Speech training/storage, batch input/output and optional diagnostic/support pathways require separate treatment. Transcription is an inference and may be wrong. [P5] |
| **Standalone real-time standard neural Azure TTS** | Selected text in/audio out; no input/output content in Microsoft logs, neither retained with real-time synthesis. | No basis to promise zero Microsoft operational access; no content-training inference should be generalized to all speech features. Custom voice/avatar entails training, consent files and possible retained model copies; not required for this shortlist. Batch synthesis stores scripts/output. [P6] |
| **Voice Live** | Own layer says no customer-data storage/retention; it processes audio for VAD/AEC/noise suppression and model/speech processing plus tool outputs. Connected components have their own storage policies. | Do not advertise the complete chain as no-retention: consult underlying Azure OpenAI/Agents/Speech policies. **Opt-in support debugging** stores speech data in Microsoft-managed same-region storage for **30 days**, accessible to authorized debugging engineers through request-ID/JIT controlled access. Confirm scope/abuse behavior for the exact natively managed model and feature version. [P7] |
| **Direct Azure OpenAI native Realtime audio** | Audio, optional transcript, text/images/tool results and session context reach Azure model service. Stateful in-session context is not a NAS-only computation. | Azure model-service policy is the relevant starting point, not standalone STT/TTS no-retention. Exact raw-audio sampling/retention/human-review coverage for the selected version remains to clarify; the generic privacy page discusses text/images more explicitly. [P1, S2] |
| **Foundry partner/community endpoint or added Agent/Search/Bing tools** | Depends on the actual deployment and connected services. | A model catalog listing is not proof of the sold-by-Azure policy. Product Terms distinguish Azure-sold models, other Microsoft consumption services and non-Microsoft models; connected Bing has separate terms/data boundary. Not added to this shortlist. [T1, T2, P1, P7] |

The Document Intelligence and standalone Speech pages are narrower than the Foundry Models privacy promise. Their gaps are **unknowns**, not evidence that Microsoft trains on the content. DPA and selected service contracts must resolve remaining use/access questions. Relevant metadata/operational telemetry, app content logs, SDK diagnostics, support uploads and optional stored completions must be accounted for separately. No logging, moderation exception, retention period, residency policy or disclosure is selected by this research.

## 6. Pricing sources, units and licenses

Fetched official price sources: **Azure OpenAI**, **Speech (including Voice Live)** and **Document Intelligence**. Relevant fetched rates were `$-`, despite visible region/currency selectors. Therefore there is **no verifiable current EUR/USD price total or cost ranking in this artifact**. Selection must capture dated region/SKU/model-version/currency/contract prices and taxes from the calculator/offer, without using consumer OpenAI rates as Azure rates. [C1–C3]

| Component | Cost units to collect |
| --- | --- |
| GPT text/image explanation and optional check | Per million input, cached input and output tokens; reasoning output where applicable; image input accounting depends on model/detail. Every check/retry/summarization has its own usage. |
| Optional Foundry embeddings | Input tokens, plus NAS index/storage/compute; transmitted passages are a separate data disclosure. Embeddings rank relevance, not support. |
| Standalone STT | Processed audio hours; price docs describe per-second billing. Continuous listening/silence and segmentation accounting require confirmation for the actual connection/mode. |
| Standalone neural TTS | Per million billable characters; punctuation/formatting rules and requested versus actually heard output matter. Interrupted generated speech can still consume service usage. |
| Voice Live | Per-million text/audio input, cached input and output, image where offered; Pro/Standard/Lite tier follows chosen model. Native audio and standard Azure speech audio have different categories. BYO model/Agent introduces separate LLM/services billing. |
| Direct native GPT Realtime | Separate text/audio input/cached/output token meters; newer translation/transcription-only models use duration units and are not equivalent to conversation models. |
| Read / Layout / formula-high-resolution add-ons | Per 1,000 analyzed pages, distinct model/add-on/query-field meters. Office billable page units differ from printed pages. Reanalysis/reupload adds usage. No claim that multiple paid add-ons are one flat surcharge. |

Voice Live overview estimates Azure OpenAI audio at ~10 input tokens/s and ~20 output tokens/s. As arithmetic only, **10 minutes input + 10 minutes output** would suggest ~6,000 input / ~12,000 output audio tokens, plus text/context and cache meters. It is not a measured monthly estimate and does not establish how silence/replayed context is charged. A synthetic month of 100 analyzed pages plus 1,000 explanation/check calls needs `pages × chosen page rate + Σ(input/cached/output meters × chosen rates) + speech meters`, not one price per “chat”. [S1, C1–C3]

Pay-per-token/page/character/hour options are practical candidates; provisioned throughput or commitments add fixed/reserved costs that can dominate a single learner. Quota is not a budget, and a budget notification is not proof of a hard spending stop. Usage allowance, cancellation, retry limits, session duration and warning behavior belong to the human operating-budget choice.

**Licensing/terms:** Azure models are subscription services, not downloaded open-source model rights. Product-specific/model-specific terms, universal online terms, AI Code of Conduct and DPA apply. Paid-tier prebuilt neural TTS output-use rights (including commercial use) are expressly documented; do not automatically extend that grant to F0. Tesseract code and PDF.js are Apache-2.0; Tesseract dependencies have their own licenses, and exact model-data/dependency versions still require inspection. Speech SDK has its own Microsoft SDK license and third-party notices; its source repository does not establish that every binary feature is covered by the same license. No source-content rights are granted by any SDK license. [T1, T2, S10, L1–L3]

## 7. Age, education and input-content terms

The enterprise AI Code of Conduct requires AI/synthetic-voice disclosure, appropriate oversight/testing, access controls, required notices/consents and rights to input/output material; it forbids exploitation of age vulnerabilities and inference of emotional states from speech patterns. The latter is different from responding to a learner’s explicit statement of frustration. No emotional profiling or new policy is selected here. [T1, T2, T4]

**No numeric minimum end-user age for this Azure-backed custom tutoring application was established in the fetched service-specific terms.** This is not a finding that every minor use is permitted. Check the actual Azure subscription agreement, end-user access arrangement, applicable consent requirements and feature-specific terms before live use. Consumer ChatGPT/OpenAI account ages do not automatically govern an Azure OpenAI backend: Microsoft says it does not call OpenAI-operated services. The subscription operator and learner are different roles. [P1, T1, T2, P3]

The current Azure for Students offer page restricts its main credit offer to full-time university students, so a secondary-school pilot cannot simply assume student credits/eligibility. Exact operator/account eligibility is still a human deployment fact. [T5]

Voice Live explicitly lists education as an intended use while calling out high-stakes education considerations; that is not certification for Dutch 4 VWO content or decisions about school access/grading. Custom/personal voice cloning is unnecessary here and adds consent, Limited Access, training and retained-copy obligations. Standard Dutch voices are sufficient candidates to compare. [S1, P6, T1]

Cloud textbook/slide processing, external transcript/frame downloading and reproducing passages require actual content rights. An original-source link, a purchased textbook or permitted video viewing does not itself establish cloud-processing rights. This research used no learner documents, copyrighted study uploads or third-party video content.

## 8. External text, images and video interpretation

The shortlisted text/vision models can interpret **provided** relevant web passages, images, transcripts and sampled frames; the NAS retains URL, retrieval time, inspected scope and original/source timestamp pointers. They do not establish video inspection by receiving a title or URL. A native Realtime image capability likewise does not prove access to a linked video. [M1, M3, S2]

For a later synthetic video case, distinguish metadata-only recommendation, inspected transcript excerpts and inspected frames with time ranges. Transcript coverage does not cover silent diagrams; sampled frames do not cover uninspected time or spoken content. Show gaps and keep external supplementation distinct from supplied material. Topic-only external-search terms remain the existing boundary; a new cloud-hosted web-grounding tool could transmit additional context and is not authorized just because Foundry offers it. NAS interpretation can use permitted Azure model processing under the latest architecture, without selecting Bing/Video Indexer or a new download policy. [T1, T2]

## 9. Minimal later acceptance measurements and unresolved choices

Proposed operator verification, **not an app-managed learner evaluation system**, and not run in this research. Human acceptance thresholds and failure decisions remain in the stack/budget ticket.

1. **Availability/contract preflight:** record exact model version, resource/deployment/SKU, API/SDK version, relevant region/routing for every speech component, effective quota/capacity, actual price meters, privacy terms and logging settings. Clarify raw-audio abuse sampling, exact abuse-storage duration, DI data-use/access details, preview live-use terms and minor end-user/account conditions. Resolve contradictions above with current API/docs/vendor answers; do not provision solely to resolve this research.
2. **Source representation:** create ~12 original synthetic pages/slides covering Dutch prose, two columns, rotated/cropped/blurred photographs, superscripts/fractions/minus signs, decimal commas/units, merged/header tables and a labeled diagram. Compare local extraction versus Read/Layout only where applicable. Human transcription/geometry is the reference. Record exact symbol/cell/header/caption fidelity, missing regions, original overlay alignment, readiness gaps and derivative→original mapping. Include Dutch handwriting as explicitly unsupported/unverified, not presumed usable.
3. **Explanation/support:** ~20 synthetic source-bound questions across prose, maths/science and diagrams; include insufficient, conflicting and unreadable material, plausible-but-unsupported conclusions and embedded instructions requesting another chat. Human-review all academic claims for support and Dutch clarity/4 VWO fit, separately from retrieval hit rate, pointer integrity, extraction fidelity and coverage. Compare a small model and one comparator on the same sources. Count false claims accepted by any second pass; its own judgment is not ground truth.
4. **Continuous speech:** ~20 nonprivate Dutch scripted exchanges from consenting testers on intended devices, speakers and headphones. Include long pauses, corrections, Dutch subject terms, spoken formulas, soft interruption, overlapping speaker echo, background sound and network jitter. Measure false/late turns, missing words, self-triggered echo, interruption-onset→audible-stop time, discarded audio and consistency of heard text/server state. Verify enable/pause/stop and cancellation during retrieval/check/TTS, source inspection while listening, and session renewal. Avoid interpreting prosody as emotion.
5. **Useful latency:** ~30 ordinary turns per surviving configuration, annotate real speech-end time independently of server VAD. Record VAD wait, STT finalization, NAS retrieval, model first output, support-release wait, TTS first byte and actual first audible/visible useful supported response. Report individual samples, median, p90 and range with warm/cold/network/ingestion conditions; a small sample is diagnostic, not a performance guarantee. Compare to the existing ~2s voice/~5s text targets; no acknowledgment-padding or unchecked output to claim success.
6. **Cost/recovery:** collect meters only (no routine content logs) for one representative synthetic session, short/long answers, an interruption, summary, extraction and bounded retry. Verify interrupted/cancelled work accounting and stale-output rejection. Estimate a month using the human’s usage scenarios/prices. Test 429/disconnect/timeout/unreadable-source behavior and the explicit source-gap response. Do not quietly substitute push-to-talk, disable interruption, relax support or make Mac Mini the LLM host if targets fail.

Open facts capable of changing the choice: source/geometry quality; pedagogical fit; support-gated speech delay; Dutch automatic turns; echo under delayed playback; selected transport’s residency/preview status; service-version retention/human access; real household quota/eligibility/capacity; actual unit prices and usage budget. Documentation alone settles none of their runtime outcomes.

## Primary sources

All links below were fetched on **2026-10-24** unless explicitly described as a remaining check. Learn pages are EN-US. Revision IDs are the metadata returned by Microsoft Learn, useful for identifying the observed document; they are not a claim that the backing private documentation repository is publicly accessible.

### Models and interfaces

- **M1** — [Foundry Models sold by Azure: capabilities and model versions](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure), updated 2026-10-08, revision `9586c846951c6a1142057355d5b7d6f73c63d1b7`.
- **M2** — [Model region availability by deployment category](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability), updated 2026-10-19, revision `3f372af1290f0e9ba3d05d8c727aa6a62ee00412`.
- **M3** — [Vision-enabled chat inputs](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/gpt-with-vision), updated 2026-07-31.
- **M4** — [Responses API: storage, manual chaining, streaming, supported APIs/regions](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/responses), updated 2026-08-18, revision `50853cc49f4e8e7a42a484337fa2e0c41e972fd9`.
- **M5** — [Azure OpenAI quotas and limits](https://learn.microsoft.com/en-us/azure/foundry/openai/quotas-limits), updated 2026-08-20, revision `a7293ab57b2b33653cae505e60a79a37643d6c7f`.

### Document processing

- **D1** — [Document Intelligence v4 Layout](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/prebuilt/layout?view=doc-intel-4.0.0), updated 2026-10-19, revision `9660574e8bceefc42ccae14833949a30345f04ad`.
- **D2** — [Analyze response: spans, page units, geometry and Office limitations](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/concept/analyze-document-response?view=doc-intel-4.0.0), updated 2026-10-19, revision `5b6edd065314c3dc79ccea6fe6e514e1b915e97f`.
- **D3** — [Read/Layout language and handwriting support](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/language-support/ocr?view=doc-intel-4.0.0), updated 2026-10-19.
- **D4** — [Add-ons, formula geometry and hard-coded confidence](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/concept/add-on-capabilities?view=doc-intel-4.0.0), updated 2026-10-19.
- **D5** — [Document Intelligence limits](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/service-limits?view=doc-intel-4.0.0), updated 2026-10-19.
- **D6** — [Read OCR and searchable PDF](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/prebuilt/read?view=doc-intel-4.0.0), updated 2026-10-19.
- **D7** — [Analyze Document REST contract, API 2024-11-30](https://learn.microsoft.com/en-us/rest/api/aiservices/document-models/analyze-document?view=rest-aiservices-v4.0%20%282024-11-30%29), updated 2025-03-18, revision `0ca2c6cb5cbffdce70b13a6209caaecc0c352334`.

### Speech

- **S1** — [Voice Live overview, model tiers and token accounting](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/voice-live), updated 2026-10-18, revision `f493b6963ef9b2bf2d3efefd8fa62c48d7e87da3`.
- **S2** — [Direct GPT Realtime: GA API, VAD, cancellation/truncation, images and session length](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/realtime-audio), updated 2026-09-23, revision `1501895985b8c16cf61f000a6c703c0c37e82be7`.
- **S3** — [Voice Live session configuration, Dutch-relevant VAD limits and echo references](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/voice-live-how-to), updated 2026-10-08, revision `f57dcc3a647f9a8bcaf53b83b5a3550d2031ad76`.
- **S4** — [Voice Live language support and default recognizer limitations](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/voice-live-language-support), updated 2026-09-24, revision `65cfd687178840a3f944df7bb6d18defa3273f38`.
- **S5** — [Speech STT locales and Dutch standard voices](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support), updated 2026-10-22, revision `6a8e03f7d7986ba7f1500045065679511361c9b1`.
- **S6** — [Voice Live WebRTC, global routing and preview calls endpoint](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/voice-live-webrtc), updated 2026-10-01, revision `5ec2bae124b9cb140742fa4ea5111498def7c1d2`.
- **S7** — [Continuous recognition, silence and semantic segmentation limitations](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-recognize-speech), updated 2026-06-05.
- **S8** — [Speech synthesis latency instrumentation, streaming and text-stream limitations](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-lower-speech-synthesis-latency), updated 2026-06-05.
- **S9** — [Speech / Voice Live quotas, 429 capacity and quota request eligibility](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-services-quotas-and-limits), updated 2026-10-23, revision `f3bfa58967f5337f7a709ff21e1b87d9e1712ee0`; [Speech/Voice Live regions and separate speech/LLM scopes](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/regions?tabs=voice-live), updated 2026-10-18, revision `f493b6963ef9b2bf2d3efefd8fa62c48d7e87da3`.
- **S10** — [Speech SDK platforms, licenses and binary/source differences](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-sdk), updated 2026-08-17.

### Privacy and terms

- **P1** — [Foundry Models sold by Azure data/privacy/security](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy), updated 2026-06-05, revision `777f84fbedeb0995f4fdea6c65bee2f2a4af1657`.
- **P2** — [Foundry abuse monitoring](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/abuse-monitoring), updated 2026-06-05.
- **P3** — [Limited Access and modified-monitoring eligibility](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/limited-access), updated 2026-06-05.
- **P4** — [Document Intelligence input/result retention and early deletion](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/document-intelligence/data-privacy-security), updated 2026-10-19, revision `a03c5d55c424f5e960a39e51f066a86ff90d5e02`.
- **P5** — [Standalone speech-to-text data/privacy/security](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/speech-service/speech-to-text/data-privacy-security), updated 2026-08-26, revision `64238d2b5021e0c0c1dedabf6428d314dce5fb8f`.
- **P6** — [Text-to-speech data/privacy/security: prebuilt versus custom/batch](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/speech-service/text-to-speech/data-privacy-security), updated 2026-08-26, same revision as P5.
- **P7** — [Voice Live data/privacy/security and opt-in 30-day debugging logs](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/speech-service/voice-live/data-privacy-security), updated 2026-06-20, revision `b012657aa3ca35e8ad76d9c90cbfb961b58bbe7a`.
- **T1** — [Microsoft Azure Product Terms, MCA view](https://www.microsoft.com/licensing/terms/productoffering/MicrosoftAzure/MCA): AI services, sold-by-Azure/other models, abuse access, integrated services, TTS paid-tier output rights, containers. Actual subscription program remains to confirm.
- **T2** — [Universal Online Services terms, MCA view](https://www.microsoft.com/licensing/terms/product/ForOnlineServices/MCA): DPA, generative training/output rights, connected Bing and previews.
- **T3** — [Azure supplemental preview terms](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/), last updated June 2026.
- **T4** — [Microsoft Enterprise AI Services Code of Conduct](https://learn.microsoft.com/en-us/legal/ai-code-of-conduct), version 4.0, 2026-05-01.
- **T5** — [Azure for Students current offer](https://azure.microsoft.com/en-us/free/students/): university-student restriction; not a universal end-user age rule.

### Cost and local extraction vendors

- **C1** — [Azure OpenAI pricing](https://azure.microsoft.com/en-us/pricing/details/azure-openai/): input/cached/output text/audio meters; relevant fetched rates `$-`.
- **C2** — [Azure Speech pricing including Voice Live](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/): hour/character/token units and paid custom extras; relevant fetched rates `$-`.
- **C3** — [Document Intelligence pricing](https://azure.microsoft.com/en-us/pricing/details/ai-document-intelligence/): model/page/add-on units and F0 limitations; relevant fetched rates `$-`.
- **L1** — [PDF.js PDFPageProxy API](https://mozilla.github.io/pdf.js/api/draft/module-pdfjsLib-PDFPageProxy.html); [vendor license](https://raw.githubusercontent.com/mozilla/pdf.js/master/LICENSE).
- **L2** — [Tesseract vendor README and license/dependency description](https://raw.githubusercontent.com/tesseract-ocr/tesseract/main/README.md).
- **L3** — [Tesseract hOCR/TSV output contract](https://tesseract-ocr.github.io/tessdoc/Command-Line-Usage.html); [Dutch `nld` model-data support](https://tesseract-ocr.github.io/tessdoc/Data-Files-in-different-versions.html).
