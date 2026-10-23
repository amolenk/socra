# Adolescent data and provider constraints for the study-aid pilot

Research resolution for [What adolescent data and provider constraints must the pilot decisions account for? (#4)](https://github.com/amolenk/socra/issues/4).

- Researched: 23 October 2026.
- Artifact context: `amolenk/socra`, isolated throwaway orphan branch `research/adolescent-data`, file `docs/research/adolescent-data.md`. Research-only; not an implementation decision.
- Working scenario: a private Dutch-language study aid for a 15-year-old, almost 16, at 4 VWO. Netherlands/EU is conditional on confirming the learner's location/residence, operator establishment, hosting and processing locations, and actual household-only scope.
- Method: official legislation/regulator publications and first-party contracts/documentation. Facts below are source statements; design implications are explicitly inferences. No provider is selected and no legal conclusion about this particular arrangement is established.

## Answer in brief

The pilot needs explicit boundaries for age/product eligibility, raw-chat visibility, every retained copy, third-party processing, and each material's upload rights. Turning 16 changes some Dutch consent/representation rules but does not end provider under-18 conditions. Household processing may be exempt from GDPR for the family while its cloud suppliers remain subject to it. An API's no-training default does not mean no retention, no human access, or immediate deletion. [S1–S12]

The most consequential provider-specific constraint found is OpenAI's under-18 API guidance: personal data below the applicable digital-consent age should not be processed without first implementing API zero data retention (ZDR). Under the conditional Dutch threshold of 16, that matters **now**, before the birthday, even with parental permission. ZDR requires provider approval and has feature/model exceptions. Claude consumer use is 18+; API-backed products serving minors have a separate route with safeguards, whose fit to a private parent-developed arrangement needs confirmation. [S2, S4, S5, S9, S10]

## 1. Jurisdiction, household scope and the birthday

### Sourced facts

1. GDPR Article 2(2)(c) excludes processing by a natural person in a purely personal or household activity. Recital 18 says that this activity has no connection with professional/commercial activity and expressly preserves GDPR applicability to controllers/processors providing its processing means. Online activity can fall within household activity; using a server is not by itself the test. [S1, Article 2; Recital 18]
2. GDPR territorial scope includes EU-established controllers/processors regardless of processing location, and certain non-EU operators offering services to people in the EU or monitoring their EU behaviour. Payment is not required. Hosting outside Europe does not itself remove EU applicability. [S1, Article 3; S2, Article 4]
3. GDPR Article 8 concerns **consent-based** processing in a directly offered information-society service: threshold 16 unless lowered nationally, with reasonable efforts to verify parental authorisation below it. It does not declare consent the only lawful basis or determine contractual capacity. The Dutch law consulted contains no lower Article 8 threshold. [S1, Articles 6–8; S2; S3]
4. The Dutch UAVG text in force from **1 September 2026**, Article 5, requires representative consent below 16 where Article 8 does not apply and consent is the basis. It now says a child aged 12+ can withdraw consent and a 12–15-year-old can exercise GDPR Chapter III rights, alongside the representative. From 16, the additional representation rule in Article 5(4)(b) no longer applies. The article has exceptions for directly and freely offered assistance/advice services and specific medical-contract rules; this research does not classify a study aid under those exceptions. [S2, Article 5(1), (3)–(7)]
5. Under-18 provider requirements are separate from this Dutch threshold: OpenAI consumer terms require parental permission below 18, and its Services Agreement prohibits allowing minors to use its services without parent/guardian consent. Anthropic consumer terms require 18. [S4, Registration; S6, Section 3.3(c); S9, Section 2]

### Implications and uncertainty

- **Inference:** record whether this is solely a family activity or an operator's professional/research/product activity. A private URL, tiny user count or free pilot does not establish the household exemption. Adding other households, school administration, external evaluation or commercial reuse requires reassessing scope and roles. [S1]
- **Inference:** the family may have an exempt activity while providers have regulated processing. Supplier contracts, age conditions and copyright do not disappear through the household exemption. [S1, S4–S6, S9–S12, S14]
- **Inference:** schedule an age-transition review of consent/rights handling and parental UI access at 16. Do not automatically remove all parental permissions or switch models on the birthday; under-18 provider rules persist. Avoid collecting a full identity document when a proportionate mechanism suffices. [S1, Articles 5, 8; S2; S4–S6]
- **Unresolved fact:** actual user/operator jurisdictions and intended expansion. The location of storage, inference, support access and subprocessors are separate facts to obtain for the eventual stack. [S1, Article 3; S5; S13]

## 2. Chat sensitivity and parent/developer access

### Sourced facts

- Identifiable chat, progress, errors, uploads and logs can be personal data; Article 4 includes indirect/online identification and defines pseudonymisation separately. Article 9 adds restrictions for information revealing health, religion, politics, ethnicity, sexual life/orientation and other specified categories. A grade is not automatically Article 9 data, and a biology question is not automatically a disclosure about the learner. [S1, Articles 4, 9]
- Where GDPR applies, purpose limitation, minimisation, storage limitation and security apply; child-facing explanations must be clear and accessible. Article 25 requires privacy by design/default, including limiting default accessibility. Consent for studying does not automatically authorise unrelated monitoring or model-development reuse. [S1, Articles 5–7, 12–13, 25; S15]
- Access rights are not unlimited disclosure rights: GDPR Article 15(4) protects others' rights/freedoms, and Article 17 erasure has exceptions. The current Dutch representation rule allows both child and representative rights exercise at 15; that is not a requirement for a permanent parent transcript dashboard. [S1, Articles 15, 17; S2, Article 5]
- ChatGPT parental controls link distinct accounts and do **not** give parents conversation/history or real-time access. Limited safety notifications may follow trained-reviewer assessment, do not include transcripts, and are not comprehensive real-time monitoring. This is a consumer-product feature, not an API promise. School/organisation-domain account transfer can instead give an administrator content access under the consumer terms. [S8; S4, Corporate domains]
- API supplier confidentiality still permits controlled staff/contractor access and legally required disclosure. OpenAI's Services Agreement allows need-to-know disclosure to bound affiliates/employees/contractors/agents. Anthropic's commercial terms similarly allow need-to-know representatives. No-training is not no-human-access. [S6, Section 7; S10, Section E]

### Design implications for the human decisions

- **Inference:** distinguish learner history, learner-selected sharing, developer troubleshooting, backups, supplier safety review, and statutory rights requests. Each has a different purpose and audience. A parent who operates the backend may technically retrieve plaintext even if the UI hides it; explain the actual limit rather than promising absolute secrecy. [S1, Articles 5, 12–13, 25; S6; S10]
- **Inference:** no raw transcripts in ordinary telemetry is a materially different architecture from logging everything and hiding the dashboard. Redaction, short-lived explicitly shared debug extracts, least-privilege access and recorded access can narrow developer exposure. These are options for #10 to decide, not research-imposed policy. [S1, Articles 5, 25, 32]
- **Inference:** stress, shame, self-harm or abuse disclosures can enter a study chat without an intended counselling feature. Decide the handling and disclosure policy in #9/#10, including limits of detection and any exceptional access. Neither a generic moderation recommendation nor ChatGPT's consumer notification feature establishes a duty or capability for the custom pilot to notify the parent. [S5, S8, S12]
- **Inference:** keep private data out of GitHub research assets, screenshots and coding-agent context. A pseudonym in a one-child pilot can remain identifiable. Synthetic examples avoid reproducing that exposure during planning. [S1, Articles 4–5]

## 3. Provider/product distinctions that affect feasible designs

These are illustrative constraint comparisons, not an exhaustive market survey. Revalidate the exact service, contract, account settings, model and endpoints at selection time. A branded model behind another cloud or gateway has additional terms and processing paths. [S5, S10, S11]

| Surface | Eligibility/age | Training | Retention/deletion/access that matters |
| --- | --- | --- | --- |
| OpenAI consumer ChatGPT | At least 13 or country minimum consent age; parent permission and joint terms reading under 18. Do not share a parent's credentials to bypass eligibility. [S4] | Personal Free/Plus/Pro sharing enabled by default; opt-out covers new conversations. Feedback may allow the entire associated conversation to be trained. Temporary chats are not used for improvement while temporary. [S7] | Saved/archived chats remain until deletion; deletion scheduled within 30 days with de-identification/security/legal exceptions. Library files can survive chat deletion; GPT/project files have separate lifetimes. Temporary chats can remain up to 30 days. Parent linking is not transcript access. [S7, S8] |
| OpenAI API/custom app | API agreement allows integration for end users, requires parental consent for minors. Under-18 guidance says not to process personal data under 13 **or the applicable digital-consent age** without implementing ZDR first. Under-18 safeguards also apply after 16. [S5, S6] | Inputs/outputs not used to train/improve by default, unless explicitly opted in. [S5, S6] | Default content-bearing abuse logs up to 30 days, with longer law/harm exceptions. Responses application state stored at least 30 days by default/`store=true`; `store=false` does not turn off abuse logs. Files/vector stores/conversations have persistent, endpoint-specific state. ZDR approval is required and does not make every feature stateless. [S5] |
| Anthropic consumer Claude | At least 18; consumer terms prohibit making one's account available to others. This route is unavailable to the 15/16-year-old under those terms. [S9] | Terms permit training unless opted out; feedback and safety-review exceptions remain. If improvement is enabled, de-identified data may remain in training pipelines up to 5 years; deletion does not undo already trained models/runs in progress. [S9, S11] | Chat deletion removes history immediately and backend within 30 days. Flagged content up to 2 years, safety classification scores up to 7; feedback associated data 5 years; other legal/dispute exceptions. [S11] |
| Anthropic API/custom app | Commercial terms permit powering end-user products; separate minors guidance lists age assurance, moderation, monitoring/reporting, educational guidance and AI disclosure. This does not authorise using a consumer subscription as a proxy. Commercial terms say services are not for consumer use; applicability to a purely private parent-developed app needs provider confirmation. [S10, S12] | Commercial terms prohibit training on Customer Content. Feedback is a distinct path; do not assume submitting real chats as bug reports preserves the ordinary-content regime. [S10, S11] | Standard API inputs/outputs deleted within 30 days, except persistent features such as Files, agreed retention, enforcement and law. Flagged inputs/outputs up to 2 years/scores 7; feedback 5 years. Certain designated Covered Models require 30-day retention/review despite ordinary ZDR, with specified eligibility exceptions. Exact model/contract matters. [S11] |

### Material consequences

1. **Before 16, OpenAI personal-data API feasibility is conditional.** Parental authorisation, a paid account or `store=false` is not a substitute for approved ZDR in the provider guidance. Simply stripping a name may not eliminate personal data from a recognisable family/student conversation. The guidance does not expressly resolve its application to an exempt household operator; obtain confirmation rather than asserting an exemption. [S1, Articles 4, 8; S5]
2. **After 16, fewer consent constraints does not mean an adult experience.** OpenAI minors consent/safeguards and Claude consumer 18+ persist. [S4–S6, S9, S12]
3. **Separate own storage from supplier storage.** A delete button should cover chats, summaries/memory, source images/PDFs, OCR text, embeddings/vector objects, caches, logs, exported debug samples and backups, plus provider-owned objects where deletion is supported. Distinguish removal from active use, pending expiry and lawful exceptions. This is a design inference from separate object lifetimes, not a promise that every supplier offers immediate erasure. [S1, Article 17; S5, S7, S11]
4. **Treat training switches and feedback separately.** A paid consumer plan is not equivalent to an API DPA/no-training regime. Record opt-in settings and avoid transmitting sensitive learner conversations through feedback/evaluation channels without separately deciding that purpose. [S6, S7, S9–S11]
5. **EU residency requires exact verification.** OpenAI regional storage and inference depend on eligible configurations/endpoints/models and approval; system data and third-party services may fall outside residency. Its DPA provides for processing/subprocessors and EEA transfers using SCCs or adequacy decisions; neither the DPA nor an EU hosting checkbox guarantees every byte stays in the EU. [S5, S13]
6. **Count OCR, embeddings and observability as providers too.** A local UI backed by remote OCR, hosted file search, error tracking, analytics or agent tools has multiple recipients. Review their age, data-use, retention, region and access terms independently. Supplier-safe settings for the main LLM cannot govern an external MCP service or other processor. [S1, Articles 13, 28, 44–49; S5, S13]

## 4. Uploaded textbook/PDF/slide rights

### Sourced facts

- The Dutch Auteurswet Article 1 reserves reproduction/publication to the rightsholder subject to statutory exceptions; Article 13 includes certain adaptations. Owning a book or school account does not itself transfer those rights. [S14, Articles 1–2, 13]
- Article 16b allows limited personal-study copies under conditions; for books it generally limits copying to a small portion with stated exceptions, restricts handing copies to third parties, and expressly excludes copying governed by Article 16c. Article 16c has its own non-commercial personal-use conditions and third-party delivery restrictions. It would be inaccurate to apply 16b's small-portion rule to all digital copying or declare all whole-book digital uploads lawful. [S14, Articles 16b–16c]
- Quotation under Article 15a requires lawful publication, a justified purpose/extent, and source/author attribution where reasonably possible. Education provisions have conditions including remuneration, with digital use under an educational institution's responsibility in a restricted environment. A privately built study aid is not automatically that institutional arrangement. [S14, Articles 15a, 16]
- Text/data mining Article 15o requires lawful access and no appropriately reserved rights, with retention limited to what is needed. That does not establish that every retrieval, cloud upload or reproduced response qualifies. [S14, Article 15o]
- Both providers require sufficient input rights/licenses/permissions. Assigning output rights between supplier and customer does not clear the textbook publisher's rights. [S4, Content; S6, Sections 4.3, 9.2; S9, Section 4; S10, Section L.1]

### Implications and missing evidence

- **Inference:** keep an asset provenance/rights record (title/edition, source, license/permission and allowed upload/use audience). Separate learner-owned notes, teacher slides, textbook excerpts and openly licensed resources. A teacher or pupil possessing slides may not control embedded textbook images. [S14, Articles 1–2; S4, S10]
- **Inference:** local original-source inspection with carefully scoped excerpts can reduce cloud reproduction/disclosure, but is not an automatic legal clearance. Sending extracted text or image crops still copies/transmits source content. Retrieval isolation and preventing cross-user corpus reuse matter if scope expands. [S14]
- **Unresolved:** no particular publishers, textbooks, slide licenses or school-platform agreements were supplied. This research cannot clear the eventual corpus. Confirm permission for the specific processing route, including OCR/embeddings/remote storage; resolve genuine uncertainty with the rights owner or competent advice before relying on an exception. [S14; S6, S10]
- **Inference:** attribution/quoting constraints and limiting lengthy reconstructed passages can affect #6's evidence contract. “No provider training” protects against one use, not every reproduction involved in ingestion and tutoring. [S14; S6]

## 5. Household pilot versus later public/school deployment

For the conditional household pilot, determine whether the household exemption actually fits, disclose real technical access to the learner, satisfy the chosen supplier's age/data conditions, and establish material rights. Those facts inform #9/#10/#12; they do not select the family's sharing or serious-disclosure policy. [S1, S4–S6, S9–S14]

For a service beyond household activity, identify controllers/processors by actual purpose and means, document a lawful basis for each purpose and an Article 9 condition if relevant, use suitable processor arrangements, provide child-readable recipient/retention information, and assess transfers, rights handling and breach responsibilities. Article 35 requires a DPIA for processing likely to pose high risk; it is an assessment of the proposed processing, not an automatic declaration that every chatbot needs one. [S1, Articles 4–9, 12–17, 24–35, 44–49; S15]

Educational admission, grading with significant effects, profiling and wellbeing inference change the risk/use classification. GDPR Article 22 addresses solely automated decisions with legal or similarly significant effects; a practice hint is not automatically such a decision. OpenAI consumer terms additionally prohibit using person-related outputs for materially impactful educational and other decisions. Reassess applicable AI/education rules if the use becomes institutional; this research does not clear a school deployment or exhaust the EU AI Act/DSA. [S1, Article 22; S4, Accuracy]

## 6. Sharp follow-ups and remaining fog

Existing tickets already cover the principal decisions; no new issues are proposed for matters they contain:

- [#10: Privacy/access boundaries](https://github.com/amolenk/socra/issues/10): confirm jurisdictions/household scope; choose parent-as-developer access, retention/deletion promises, rights-request handling and the 16-year transition.
- [#9: Study emotions and serious disclosures](https://github.com/amolenk/socra/issues/9): decide exceptional handling and honest confidentiality/detection boundaries.
- [#6: Supported guidance](https://github.com/amolenk/socra/issues/6) and [#12: System boundaries](https://github.com/amolenk/socra/issues/12): reflect material rights, quoted passage limits, actual recipients and deletable object lifecycles.

Genuinely sharp follow-up candidates **when a stack/corpus is proposed** (parent owns map/ticket updates):

1. **Can the proposed provider/model/endpoint path contractually serve this learner at the pilot start age with the required retention settings?** Evidence needed: household/developer eligibility confirmation, ZDR approval if required, persistent-feature and model exceptions, and account-level opt-ins. Changes feasible launch timing/data path. Currently provider selection is map fog, not a reason to invent a vendor decision now.
2. **For the named first textbook/slide set, what permission or exception supports its exact local/cloud processing and output use?** Evidence needed: actual license/publisher/school terms and embedded-content provenance. Generic study-purpose claims cannot answer this.

Remaining research limitations:

- Gemini API terms could not be retrieved from `https://ai.google.dev/gemini-api/terms` after several attempts; no Google age/training/retention conclusion is made. Review the official terms before it becomes a candidate, distinguishing Gemini consumer, developer API and cloud-hosted products.
- EUR-Lex GDPR URLs returned empty extraction. The substantive text was read on the Dutch regulator's official reproduction [S1], with its link to the legally authoritative EUR-Lex edition. This preserves a primary official basis without claiming the fetched EUR-Lex response contained readable text.
- Anthropic's current `/legal/aup` page displayed an effective date of **12 November 2026**, after this research date. Its forward-dated wording is not used as the pilot's current binding policy. The separately dated March 2026 minors guidance and June 2025 commercial terms support the API/consumer distinction; retrieve the applicable dated AUP at provider selection.
- Web help pages are mutable and often show relative update dates. Findings record access date and specific sections rather than treating them as stable contract versions. Recheck actual terms/settings at implementation. No real learner chat or study material was uploaded for this research.

## Sources

All sources accessed 23 October 2026; dates below are those displayed by the source, where available.

- **S1 — GDPR text, Dutch Autoriteit Persoonsgegevens reproduction:** https://www.autoriteitpersoonsgegevens.nl/algemene-verordening-gegevensbescherming-avg . Articles 2–9, 12–17, 22, 25, 28, 32–35, 44–49; Recital 18. Official EUR-Lex edition linked by AP: https://eur-lex.europa.eu/eli/reg/2016/679/oj . AP reproduction is regulator-published; legally authoritative text is the EU publication.
- **S2 — Dutch UAVG:** https://wetten.overheid.nl/BWBR0040940/2026-09-01/ . Articles 4–5; version in force 1 September 2026 onward.
- **S3 — European Commission child-data safeguards:** https://commission.europa.eu/law/law-topic/data-protection/information-business-and-organisations/legal-grounds-processing-data/are-there-any-specific-safeguards-data-about-children_en . Answer and references to Articles 8/12.
- **S4 — OpenAI Europe consumer terms:** https://openai.com/policies/eu-terms-of-use/ . Updated 16 January 2026; registration, corporate domains, content, accuracy.
- **S5 — OpenAI API documentation:** https://developers.openai.com/api/docs/guides/safety-checks/under-18-api-guidance ; https://developers.openai.com/api/docs/guides/your-data . Under-18 regulatory/safety standards; abuse logs, ZDR approval and exceptions, endpoint application state, model-specific retention, data residency.
- **S6 — OpenAI Services Agreement:** https://openai.com/policies/services-agreement/ . Effective 1 January 2026; Sections 2.2, 3.2–3.3, 4, 5.3, 7, 9.2, 11.3.
- **S7 — ChatGPT data controls and retention:** https://help.openai.com/en/articles/7730893-data-controls-faq ; https://help.openai.com/en/articles/8983778-chat-and-file-retention-policies-in-chatgpt . Training defaults/feedback, temporary chats, separate Library/GPT/project retention.
- **S8 — ChatGPT parental controls:** https://help.openai.com/en/articles/12315553-parental-controls-in-chatgpt . Overview, safety notifications, unlinking and 18-year transition.
- **S9 — Anthropic EEA consumer terms:** https://www.anthropic.com/legal/consumer-terms . Effective 8 October 2025; Sections 2, 4–5; distinct commercial-terms scope.
- **S10 — Anthropic commercial terms:** https://www.anthropic.com/legal/commercial-terms . Effective 17 June 2025; opening scope, A–E, L.1; API/end-user use, no training, DPA, confidentiality and input rights.
- **S11 — Anthropic retention:** https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-data (commercial, 1 July 2026); https://privacy.claude.com/en/articles/10023548-how-long-do-you-store-my-data (consumer, 1 July 2026); https://privacy.claude.com/en/articles/15425996-data-retention-practices-for-covered-models (effective 9 June 2026). Standard retention, persistent features, enforcement/feedback exceptions, consumer training pipeline, designated-model ZDR exceptions.
- **S12 — Anthropic minors guidance:** https://support.claude.com/en/articles/9307344-responsible-use-of-anthropic-s-models-guidelines-for-organizations-serving-minors . Dated 16 March 2026; additional technical measures, regulatory compliance, disclosure and audit/enforcement.
- **S13 — OpenAI DPA:** https://openai.com/policies/data-processing-addendum/ . Effective 1 January 2026; Sections 1–4 and Schedule 1; roles, confidentiality, rights assistance, subprocessors, retention configuration and transfers.
- **S14 — Dutch Auteurswet:** https://wetten.overheid.nl/BWBR0001886/2026-01-01/ . Articles 1–2, 13, 15a, 15o, 16, 16b–16c. Version in force 1 January 2026 onward.
- **S15 — Dutch AP GDPR overview:** https://www.autoriteitpersoonsgegevens.nl/themas/basis-avg/avg-algemeen/de-avg-in-het-kort . Principles, responsibilities, rights and conditional DPIA obligations.
