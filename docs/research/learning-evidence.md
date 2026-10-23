# Learning evidence for assistance and independent practice

Research resolution for [What learning evidence should guide assistance and independent practice?](https://github.com/amolenk/socra/issues/2), part of [Find the way to an independent-learning study aid](https://github.com/amolenk/socra/issues/1).

Investigated 2026-10-23. Artifact branch: `research/learning-evidence`. This is an isolated, throwaway research branch, with no PR and never to be merged. This document reports evidence and candidate evaluation methods; it does not settle the learner's or parent's teaching policy.

## Evidence gist

- **Assisted success is insufficient evidence of learning.** A classroom-randomized high-school mathematics trial found GPT-4 increased assisted practice performance but unrestricted assistance reduced immediately subsequent unaided examination performance. Its teacher-informed hint variant largely removed that harm without showing a significant unaided benefit [1].
- **There is evidence for structured AI tutoring, with important qualifications.** A university physics RCT found better immediate post-test scores with a carefully authored tutor than with an active-learning classroom. The intervention included question-specific solutions, structured sequencing, and instructional videos. It did not establish delayed retention, broad transfer, or efficacy for Dutch secondary-school study across subjects [2].
- **Retrieval and spacing have directly measured delayed benefits.** Grade-8 science quizzing with feedback improved later examinations [3]. Adult experiments distinguish immediate fluency from recall after days or a week, and show benefits on inference questions as well as recall [4,5,6]. These are evidence for study activities, not evidence that an LLM is necessary.
- **Questioning, explanation, and examples can each help in particular circumstances.** Human tutoring research supports constructive learner contributions [7]; worked-example and fading studies support teaching a method and progressively returning responsibility [8,9]. Neither establishes an always-Socratic policy. Deliberately designed problem generation followed by teacher consolidation can also outperform instruction-first in some secondary mathematics settings [10].
- **Feedback and confidence must be checked against independent responses.** Correct answers made with low confidence can benefit from corrective/confirmatory feedback [11]. More questions, fewer hints, or a more fluent conversation do not independently establish understanding [1,12,13,14].

## Method, confidence, and access boundaries

This is a targeted primary-study investigation, not a preregistered systematic review or a pooled effect-size estimate. The search followed the ticket's mechanisms (human/Socratic tutoring; explicit instruction; worked examples and fading; retrieval and spacing; feedback and confidence; independent AI learning outcomes). Primary discovery routes were publisher pages, author laboratory publication lists, ERIC, PubMed/PMC, and DOI metadata. OpenAlex was used to locate originals and abstract records, not as an independent empirical authority. Studies were selected for interpretable comparators, independent or delayed outcomes, age relevance, and contrary/null findings. This is not an exhaustive search of all work published through October 2026; later studies and replications may change the balance.

Full text was inspected for [1,2,4,5,6,11,13,13a,14]. For [3,10,12], only author-provided abstracts reproduced in ERIC were accessible; accordingly this report does not assert sample counts or fine-grained methods absent from those abstracts. For [7] the publisher-deposited abstract in Crossref was inspected; for [8] an indexed original abstract was located, but publisher full text was blocked; [9] was accessible as an ERIC synopsis and indexed abstract, not full text. [15] was inspected through the publisher-deposited abstract. These restricted-access sources supply bounded context; the central conclusions have independently inspected full-text support. PNAS returned HTTP 403, but the original [1] was available in PMC. Wiley/SAGE/Taylor & Francis returned 403 or browser challenges on several older papers; some author links returned 404 and two eScholarship PDFs returned empty bodies. No inaccessible paper is represented as fully read.

The research skill was loaded. It requests background delegation, but this harness exposed no delegation tool and MCP agent discovery returned no server; research was carried out directly in this isolated research session.

**Applicability:** No inspected study directly tests this proposed product with a Dutch-speaking 4 VWO learner. Grade-8 science and grades 9–11 mathematics are closer age/context evidence than university experiments. Vocabulary, short prose, algebra, geometry, physics, and scientific experimental design are different learning tasks. Historical rule-based intelligent tutoring systems (ITS) are not generative language models. Author-selected terms such as “deep understanding” or “far transfer” mean the outcomes operationalized in that study, not comprehensive mastery.

## 1. Generative AI: two controlled studies with different outcomes

### High-school mathematics: strong evidence of a practice–independence gap [1]

Bastani and colleagues conducted four 90-minute sessions in a Turkish high school in 2023–24, covering grades 9–11 and nearly 1,000 students. Classrooms were assigned to textbook/notes practice, GPT Base, or GPT Tutor. All groups received teacher review and examples before practice and a brief teacher answer review before a closed-book, closed-laptop exam. Exam problems were conceptually very similar to practice problems. Independent graders used teacher-designed rubrics; standard errors were clustered by classroom. Main analyses excluded honors classrooms and used intention-to-treat analysis despite several noncompliant class sessions. The primary unaided outcome was preregistered.

Table 1 reports these estimates on a 0–1 score scale:

| Outcome | Control mean | GPT Base effect (SE) | GPT Tutor effect (SE) |
| --- | ---: | ---: | ---: |
| Assisted practice | .284 | +.137 (.031) | +.361 (.032) |
| Subsequent unaided exam | .321 | −.054 (.022) | −.004 (.013) |

The abstract's **48%, 127%, and −17% are relative changes**, not percentage-point changes. The unaided GPT Tutor result was not significantly different from control: neither proof of improvement nor a formal equivalence/non-inferiority result. The large practice gain therefore cannot justify a mastery claim.

GPT Tutor combined instructions to give hints rather than full answers **and** teacher-authored correct solutions, common mistakes, and feedback guidance. This bundle does not isolate “no answers” as the causal ingredient. Chat analyses suggest copying/outsourcing as a mechanism; the mechanism was not itself randomized. Students' perceived learning was overly optimistic. More substantive messages in GPT Tutor accompanied no demonstrated unaided advantage.

The exams occurred within sessions. This is a short-term independence result, not observed week-long retention or broad transfer. Single-school mathematics, 2023 GPT-4, graded activities, supervised laptops, and teacher preparation limit generalization. The authors note labor-intensive problem-specific preparation. The linked August 2025 correction concerns an author affiliation, not the outcomes.

### University physics: positive immediate evidence for an authored intervention [2]

Kestin and colleagues used a two-lesson crossover in Harvard introductory physics (194 eligible students); assignment respected existing peer groups. Students learned either in an active-learning class or asynchronously at home with a GPT-4 tutor, then completed post-tests. Aggregate median post-test scores were 4.5 versus 3.5, with an aggregate pre-test median of 2.75. Median tutor time was 49 minutes; classroom learning time was assumed to be 60 minutes after excluding testing. The adjusted linear-regression effect was .63 SD; larger quantile-regression estimates addressed ceiling effects. These alternative estimates are not separate replications.

Question-specific expert solutions supported accuracy; software sequenced problem parts because a system prompt alone did not reliably do so. Content and worksheets were matched, and a separate team member constructed tests from learning goals. However, self-pacing, setting, feedback availability, videos, peer interaction, and software differed as a package. The study does not identify which component explains the benefit or compare against equal-time paper examples plus retrieval practice. Immediate post-tests do not demonstrate durable retention. “Over double the gain” is baseline-relative and should not be carried into a promised product effect.

**Reconciliation:** These trials study different learners, content, initial teaching versus review, comparators, and tutor behavior. Their outcomes support neither “AI inevitably harms learning” nor “AI tutoring generally doubles learning.” They justify inspecting tool design and testing independent outcomes in the actual intended setting.

## 2. Questioning, explicit teaching, examples, and fading

### Human tutoring and constructive responses [7]

Chi and colleagues compared explanations of tutoring effectiveness and experimentally restricted tutors' explanations and feedback in a second study, encouraging prompting instead. Their abstract reports similarly effective learning under the interactive condition, linked to deeper scaffolding episodes and more learner reading, with reading ability limiting what was learned. This is a human tutoring study; “similarly effective” is not evidence of universal superiority of questions or an equivalence trial. Exact samples, delays, and effect estimates were not verified here.

The useful distinction for the ticket is between **learner-generated reasoning** and conversational participation. A diagnostic question can expose a premise or misconception; a sequence of answer-leading questions can instead perform the reasoning for the learner. The latter is a mechanism hypothesis to inspect locally, not a proven classification of any individual learner from a chat transcript [1,7]. No inspected head-to-head study establishes a Dutch LLM Socratic policy as better than adaptive explanation/examples on delayed independent outcomes.

### Explicit instruction and examples have a place [8,15]

Sweller and Cooper's five algebra experiments included Year 9, Year 11, and university students in the first experiment. The accessible abstract reports quicker processing and subsequently faster, less error-prone solutions after worked-example study than conventional problems, **specific to problems of identical structure** [8]. These findings support a narrow novice-method-learning claim; they do not establish broad transfer or long-term recall across subjects.

Klahr and Nigam studied 112 third- and fourth-grade children learning how to design unconfounded experiments. More children acquired the procedure with direct instruction than discovery learning; those who learned through instruction performed as well on broader science-fair judgments as the smaller group who discovered it [15]. This challenges the claim that explicit instruction necessarily prevents transfer, but is younger-child evidence on one skill. Comparing only successful learners is also narrower than an overall population treatment comparison.

Examples and explanation need not remove all learner work: explaining why a step is legitimate, completing a missing step, selecting a method for a new problem, and correcting a misconception are different tasks. Whether to use any of these in the pilot remains a human decision.

### Fading can bridge examples and problem solving; transfer is conditional [9,14]

Atkinson, Renkl, and Merrill studied progressively removing worked solution steps while prompting learners to identify underlying principles. Their abstract reports near- and far-transfer advantages across two experiments, and specifically contrasts this with less reliable far transfer from fading alone [9]. Population details and retention delays were not verified; this is evidence for the combined experimental technique, not an optimal number/order of AI hints.

A more age-relevant 2022 conference experiment had 167 analyzed middle-school students (mean age 12.81). Four randomly assigned variants with examples and menu-based conceptual self-explanations were pooled (134 students), against ITS problem-solving alone (33). Total instructional time was matched at 20 minutes; the example group had less problem-solving time [14]. Conceptual and procedural post-test advantages were significant **only with number of attempted problems included as a covariate**; without it, condition effects were nonsignificant (procedural p=.148, conceptual p=.459). Attempts are affected by the intervention and allotted practice time: conditioning on this post-treatment variable complicates the causal interpretation. Transfer was not significant (p=.067 in the adjusted analysis), and no delayed test was reported. This is promising, qualified evidence, not an unqualified confirmation of durable understanding.

### Contrary evidence: struggle followed by consolidation can help [10]

Kapur and Bielaczyc report studies in three Singapore schools. Grade-7 students generated alternative representations/methods for average-speed problems without scaffolding, then received teacher-led consolidation. Although initially unsuccessful, they outperformed instruction-first groups on post-tests and demonstrated representation flexibility. The studies used intact classes; the accessible abstract does not establish individual randomization or long-delay outcomes [10].

This comparison includes **a designed task and subsequent teaching**. It is not a test of endless guessing or withholding explanations from a stuck learner. Alongside [8,15], it shows why prior knowledge, task structure, the purpose of initial attempts, and later consolidation matter. The evidence does not supply a universal instruction-first or attempt-first rule.

## 3. Retrieval, delayed recall, and transfer

### Secondary-school evidence [3]

McDaniel and colleagues compared quizzed versus unquizzed content within Grade-8 science classes. Multiple-choice quizzes included feedback. The author-provided abstract reports 13–25% gains on unit examinations and benefits persisting on cumulative semester and end-of-year exams. Review quizzes contributed the largest increases; previous quizzes added relatively little beyond review in the reported placement experiments [3]. Full-text effect denominators and counts were not inspected, so the abstract's percent figures should not be interpreted as precise percentage-point gains.

This is closer to the intended age and authentic classroom material than vocabulary experiments. It also bundles retrieval with feedback and additional encounters; it does not isolate retrieval alone, prove transfer to new subjects, or prescribe how many quizzes this learner needs.

### Immediate performance can reverse at a delay [4]

Roediger and Karpicke tested undergraduates studying short prose. In Experiment 1 (120 participants), restudy beat recall practice on a five-minute test (81% versus 75%), but recall practice beat restudy after two days (68% versus 54%) and one week (56% versus 42%). No feedback was given during retrieval. In Experiment 2 (180 participants), repeated study led to higher predicted recall, despite worse actual week-delayed recall than repeated testing [4].

These cleanly manipulated study/test conditions show why immediate ease and confidence are inadequate success criteria. They do not directly measure adolescent frustration, essay quality, or curriculum-wide independence.

### Retrieval can support inference, without making all transfer automatic [5,13]

Karpicke and Blunt found that retrieval-based reconstruction of science texts outperformed elaborative concept mapping on week-delayed verbatim and inference questions; a second experiment also favored retrieval when the final task was concept mapping [5]. This does not show concept maps are intrinsically ineffective: maps constructed from memory can themselves be retrieval practice. The studied maps were made with the source available.

Smith and colleagues compared free recall, prompted describe/explain recall, their combinations, and reading controls. The undergraduate experiment had 150 participants; the online adult experiment analyzed 314. All retrieval variants beat reading on one-week verbatim and higher-order questions, but prompts and prompt-fading showed no discernible advantage over free recall. Sequential prompts sometimes restricted overt retrieval output [13]. The reading controls played a game during the recall periods, so learning engagement/exposure was not matched throughout.

Evidence for inference over short texts does not establish transfer to unfamiliar multi-step problems, argument evaluation, a different subject, or classroom source navigation. A novel item needs to change something relevant (such as representation or applicability), not merely names and numbers. That last criterion is an evaluation proposal derived from the transfer limitations, not a validated universal rubric.

### Spacing: measured benefits, no universal schedule [6]

Karpicke and Bauernschmidt studied 96 undergraduates learning Swahili–English pairs. After first successful retrieval, three further tests were spaced or massed. One-week recall was approximately 25–26% for recall-once/massed practice, versus 49%, 64%, and 75% for increasing total spacing, averaged across relative schedules. Expanding, equal, and contracting schedules did not significantly differ when total spacing was matched [6]. The study's spacing units were intervening trials, not a validated calendar schedule for school concepts.

This supports separating repeated recall encounters and retesting beyond first success. It does not justify a specific app scheduling algorithm or a claim that increasingly difficult retrieval is always better.

## 4. Feedback, calibration, and misleading hint success

### Corrective and confirmatory feedback [11,13a]

Butler, Karpicke, and Roediger required undergraduate participants to answer multiple-choice knowledge questions and rate confidence before feedback. Both experiments had 30 participants. Feedback benefited errors and low-confidence correct answers; the second experiment measured delayed recall after two days and found improved calibration and resolution [11]. This is evidence that “correct” and “confident” are distinct observations; uncertainty can be a lucky guess, partial knowledge, or competing alternatives. A correct answer does not justify global praise or a mastery label.

Their 2007 experiments found delayed feedback better than immediate feedback on a delayed recall test, with no advantage of answering-until-correct over receiving the correct answer [13a]. This challenges “feedback must always be immediate” and “more guided attempts necessarily teach more.” It does not justify delaying needed correction during a difficult multi-step task; content, subsequent exposure, and spacing differ. Source-accurate feedback matters because erroneous feedback can teach the wrong content [1].

Confidence **calibration** is correspondence between reported probability and observed accuracy; **resolution** is whether confidence distinguishes more- from less-likely correct answers. Average confidence matching average accuracy can conceal high-confidence misconceptions. These definitions are used in [11], not proposed diagnoses of this learner.

### Help-seeking improvement is not the same as subject learning [12]

Roll and colleagues' geometry ITS studies (58 and 67 students) improved help seeking, with behavioral transfer to new content after help-seeking support was removed for a month [12]. The authors' later retrospective reports better help-seeking behavior but no improved domain learning outcome. The primary abstract is not itself evidence of a domain-learning advantage. We use the retrospective only to qualify this older result, not as a primary experiment or a pooled estimate.

### When hints can create apparent understanding

| Observable pattern | Evidence/interpretation | Independent check that could disambiguate it |
| --- | --- | --- |
| Correct work after copying a complete solution | Directly observed practice–exam dissociation and copying-associated interaction patterns [1]; exact mechanism remains partly inferential | A new, unaided similar problem, before corrective feedback |
| Correct step after the tutor selected method, decomposed the problem, and named the next operation | Plausible cue dependence; evidence for changing support and output differs by task [9,13]. No single cue-dependence effect size claimed | Choose and justify a method without the same sequence of cues |
| Selecting the “why” from offered options | Recognition differs from generating an explanation; [14] explicitly leaves mechanisms unresolved and found nonsignificant transfer | Explain the principle without options and apply it to a changed representation |
| “I understand” after reading a fluent explanation | Perception–performance mismatch in [1,4,5] | Delayed unaided recall/inference plus confidence before feedback |
| Fewer hints or more substantive chat turns | Process can change without demonstrated independent learning [1,12]; lower help use may also mean avoidance | Independent performance alongside voluntary report of stuckness and frustration |
| Three immediate repetitions succeed | Massed success did not produce the delayed benefits of spaced retrieval in a vocabulary experiment [6] | Retrieve again after a delay, with assistance recorded separately |
| Corrected error immediately disappears | [11] does not guarantee durable repair of high-confidence errors; its hypercorrection effect was not significant after two days | Revisit the misconception in a new item after a delay |

These are candidate interpretations and observations, not permission to infer motivation, dishonesty, a disability, or mastery from logs. Asking for help is not itself evidence of failure; understanding after help can be real, but needs a different observation from completing the helped item.

## 5. Simpler comparators and feasible evaluation approaches

The following are **options for the existing evaluation decision**, not a chosen pilot protocol. No pilot, learner interview, or copyrighted-material upload was performed.

### Candidate non-generative comparators

1. **Usual study with the same school material.** Most ecologically interpretable; study time, teacher contact, and existing strategies would need recording. This tests benefit over her current practice, not which AI component causes it [1].
2. **Read–close source–recall–check**, on paper or a simple document, repeated on later occasions. This operationalizes the retrieval/restudy contrasts without generative AI [3,4,5,6,13].
3. **Worked examples, completion problems, then independent problems**, using an existing book/answer key and optional “why this step?” prompts. This offers a low-tech comparison for explanation/scaffolding [8,9].
4. **Fixed teacher-/human-checked questions with answer feedback**, optionally paper flashcards for vocabulary/facts. This can match retrieval, feedback, and time more closely than a rereading-only control [3,11].
5. **A pre-authored hint sheet or source index.** A candidate mechanism comparator for adaptable help or source finding. Its effectiveness for this pilot is untested; [1,2] show why holding good authored content constant would be informative.

Using only rereading as control could make an AI retrieval tool look impressive while answering the wrong practical question. The learner's usual strategy might already contain effective retrieval. Conversely, authoring a “simple” comparator takes time: include preparation and verification work in cost accounting.

### Candidate small-pilot design

- Start with a short unaided baseline on prerequisite knowledge; identify objectives from school material before constructing tutor sessions or evaluation items. This reduces circular evaluation of the AI's own interpretation [1,2]. It is not a diagnosis from one quiz.
- For one learner, compare multiple matched topic/item sets with study approach and order counterbalanced or randomized where acceptable. Do not teach exactly the same concept twice under different approaches and pretend the second has a fresh baseline: learning carries over. Different sets also vary in difficulty, so baseline and matching do not remove all confounding.
- Distinguish **assisted completion**, **immediate unaided performance**, **delayed unaided performance**, and **transfer**. A plausible low-burden observation might be a few items after two days and about a week, because those delays expose reversals in [4,5,11,13]; those are examples, not chosen deadlines or mastery thresholds. Record intervening school lessons/revision, test delay, time spent, help access, and missing sessions.
- Recall items can ask for a concept/process from memory; understanding items can require explaining a causal step or distinguishing an incorrect explanation; transfer items can require choosing a method in a changed representation or recognizing when a rule does not apply. Score each separately with a checked rubric, including partial reasoning, rather than a single average grade. These are candidate operations informed by [1,2,5,9,14].
- Capture confidence **before** feedback on unaided items. Where probabilities are acceptable, examine confidence–accuracy gaps and high-confidence errors; with few items, raw item pairs are more honest than a precise calibration model. An optional “don't know” response can change guessing behavior; [11]'s forced-report results cannot automatically transfer to it.
- Measure source navigation in a **separate source-available task**: find the relevant original region, explain what it supports, and recognize an unsupported statement. Do not count this as unaided memory. None of the inspected learning trials validates the proposed citation/source-checking interface.
- Observe dependence as change in required help across fresh comparable tasks, together with independence tests. Hint counts alone conflate difficulty, legitimate help, and avoidance [1,12]. Ask the learner briefly about effort, frustration, usefulness, autonomy, and time; do not treat satisfaction as evidence of efficacy [1,4].
- Have items, solutions, distractors, and scoring checked against school expectations. An evaluation generated and graded solely by the same model risks circularity and correlated mistakes; the checked authoring and independent grading in [1,2] illustrate feasible separation, not a requirement to recruit their research teams.
- Report individual item outcomes and repeated trends, including null/worse outcomes, uncertainty, missingness, and time/verification burden. A household pilot can reveal usability and local failure signals; it cannot establish population efficacy, a precise effect size, or guaranteed grade improvement. Stop/revise thresholds and acceptable burden remain decisions for the parent and learner.

## 6. Decision handoff and fog patches for the parent

Existing map coverage was checked against all current ticket bodies. **No genuinely separate follow-up ticket is required by these findings.** The sharp questions below belong in existing decisions; creating duplicates would fragment the map.

| Existing decision | Evidence-informed question to take into the live decision |
| --- | --- |
| [When should the aid question, hint, explain, or demonstrate?](https://github.com/amolenk/socra/issues/7) | What observable prerequisite gap or repeated-confusion pattern triggers explanation/example rather than another question, and what fresh unaided attempt will show that help can be faded? A fixed number of hints is not supplied by the studies. |
| [How will we judge whether the study aid improves independent learning?](https://github.com/amolenk/socra/issues/13) | Which credible usual-study or matched non-AI comparison, delay, and changed-representation tasks will distinguish improved learning from easier completion? How will test/feedback contamination and intervening study be recorded? |
| [How should the teacher communicate progress, disagreement, and uncertainty?](https://github.com/amolenk/socra/issues/8) | Which statements may follow a correct assisted answer versus independent delayed evidence, and how will a low-confidence correct answer or high-confidence misconception be handled? |
| [What counts as supported guidance, and when may outside knowledge be used?](https://github.com/amolenk/socra/issues/6) and [What source representation and system boundaries are sufficient for the pilot?](https://github.com/amolenk/socra/issues/12) | Who checks generated practice items, worked solutions, distractors, and rubrics against source material/school expectations; what happens when there is insufficient support to grade? The positive AI studies relied on substantial question-specific authoring. |
| [What are topics and study conversations, and which activities belong in the pilot?](https://github.com/amolenk/socra/issues/11) | What does each activity actually exercise: locating evidence, learning a prerequisite/method, retrieving from memory, or transferring a method? What is retained when a session ends? |
| [What does she need from the study aid, and what feels helpful or controlling?](https://github.com/amolenk/socra/issues/5) and [Does a concrete Dutch study interaction feel useful and support independence?](https://github.com/amolenk/socra/issues/14) | What assessment effort, questioning pace, uncertainty wording, and return-to-source behavior are acceptable to her? These preferences cannot be inferred from the trials or the parent. |

**Proposed fog refinements, not map edits:**

- Expand the existing subject-specific/prerequisite/school-expectation fog to include differences among factual retrieval, diagram interpretation, method selection, and open-ended argument. A single scaffold/mastery rule has not been established across them.
- Expand evaluation-logistics fog to include item/rubric validation ownership, parallel-form difficulty, source-access versus unaided task boundaries, assessment-as-learning contamination, and intervening school instruction. Exact workload and procedures depend on learner preferences and the chosen activities.
- Expand cost/latency fog to include expert preparation and checking of problem-specific solutions/hints. The evidence base for flexible upload-and-chat does not remove that burden.
- Connect the existing persistent-learning-state fog to uncertainty: a model's inferred state needs provenance distinguishing assisted, immediate unaided, and delayed evidence. Whether memory should exist remains unresolved.

**Scope-changing caveats:** Flexible multi-subject product scope is not empirically validated by one-topic AI trials. Promising authored-tutor evidence introduces a potential preparation/verification cost that may change feasible pilot scope. This does not choose a restriction to one subject or a provider/architecture. Durable learning and source-navigation benefits require local evaluation; they cannot be inherited from immediate physics or same-session mathematics outcomes.

## Original studies and access trail

1. Bastani, H., et al. (2025). *Generative AI without guardrails can harm learning: Evidence from high school mathematics.* PNAS 122, e2422633122. [DOI](https://doi.org/10.1073/pnas.2422633122), [original full text in PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12232635/), [affiliation-only correction](https://pmc.ncbi.nlm.nih.gov/articles/PMC12403119/), [preregistration](https://aspredicted.org/4DL_Q3J). Claims above use article Experimental Design, Table 1, Student Perception, mechanisms, and limitations; supplementary appendices were not independently analyzed.
2. Kestin, G., et al. (2025). *AI tutoring outperforms in-class active learning: an RCT introducing a novel research-based design in an authentic educational setting.* Scientific Reports 15, 17458. [Original full text](https://www.nature.com/articles/s41598-025-97652-6), [study data](https://github.com/HarvardAItutor/Study-Data-v4). Results, design, and limitations inspected; data were not reanalyzed.
3. McDaniel, M. A., Agarwal, P. K., Huelser, B. J., McDermott, K. B., & Roediger, H. L. (2011). *Test-enhanced learning in a middle school science classroom: The effects of quiz frequency and placement.* JEP 103, 399–414. [DOI](https://doi.org/10.1037/a0021782), [author-provided abstract in ERIC](https://eric.ed.gov/?id=EJ934340). Abstract-only access.
4. Roediger, H. L., & Karpicke, J. D. (2006). *Test-enhanced learning: Taking memory tests improves long-term retention.* Psychological Science 17, 249–255. [DOI](https://doi.org/10.1111/j.1467-9280.2006.01693.x), [author-hosted original PDF](https://learninglab.psych.purdue.edu/downloads/2006/2006_Roediger_Karpicke_PsychSci.pdf). Full text, Experiments 1–2.
5. Karpicke, J. D., & Blunt, J. R. (2011). *Retrieval practice produces more learning than elaborative studying with concept mapping.* Science 331, 772–775. [DOI](https://doi.org/10.1126/science.1199327), [author-hosted original PDF](https://learninglab.psych.purdue.edu/downloads/2011/2011_Karpicke_Blunt_Science.pdf). Main article full text; supporting materials not independently analyzed.
6. Karpicke, J. D., & Bauernschmidt, A. (2011). *Spaced retrieval: Absolute spacing enhances learning regardless of relative spacing.* JEPLMC 37, 1250–1257. [DOI](https://doi.org/10.1037/a0023436), [author-hosted original PDF](https://learninglab.psych.purdue.edu/downloads/2011/2011_Karpicke_Bauernschmidt_JEPLMC.pdf). Full text, Methods and Final Recall.
7. Chi, M. T. H., Siler, S. A., Jeong, H., Yamauchi, T., & Hausmann, R. G. (2001). *Learning from human tutoring.* Cognitive Science 25, 471–533. [DOI](https://doi.org/10.1207/s15516709cog2504_1), [publisher-deposited abstract](https://api.crossref.org/works/10.1207/s15516709cog2504_1). Abstract-only; publisher and author-hosted full-text routes inaccessible.
8. Sweller, J., & Cooper, G. A. (1985). *The use of worked examples as a substitute for problem solving in learning algebra.* Cognition and Instruction 2, 59–89. [DOI/original](https://doi.org/10.1207/s1532690xci0201_3), [indexed original abstract/access record](https://api.openalex.org/works/https://doi.org/10.1207/s1532690xci0201_3?select=title,doi,locations,abstract_inverted_index). Abstract-only; publisher blocked.
9. Atkinson, R. K., Renkl, A., & Merrill, M. M. (2003). *Transitioning from studying examples to solving problems: Effects of self-explanation prompts and fading worked-out steps.* JEP 95, 774–783. [DOI](https://doi.org/10.1037/0022-0663.95.4.774), [ERIC synopsis](https://eric.ed.gov/?id=EJ678596), [repository context](https://freidok.uni-freiburg.de/data/47428). Full text inaccessible; original abstract also located in DOI-indexed discovery record.
10. Kapur, M., & Bielaczyc, K. (2012). *Designing for productive failure.* Journal of the Learning Sciences 21, 45–83. [DOI](https://doi.org/10.1080/10508406.2011.591717), [author-provided abstract in ERIC](https://eric.ed.gov/?id=EJ955907). Abstract-only; not a claim that the related 2009 conference paper was read.
11. Butler, A. C., Karpicke, J. D., & Roediger, H. L. (2008). *Correcting a metacognitive error: Feedback increases retention of low-confidence correct responses.* JEPLMC 34, 918–928. [DOI](https://doi.org/10.1037/0278-7393.34.4.918), [author-hosted original PDF](https://learninglab.psych.purdue.edu/downloads/2008/2008_Butler_Karpicke_Roediger_JEPLMC.pdf). Full text, Experiments 1–2 and General Discussion.
12. Roll, I., Aleven, V., McLaren, B. M., & Koedinger, K. R. (2011). *Improving students' help-seeking skills using metacognitive feedback in an intelligent tutoring system.* Learning and Instruction 21, 267–280. [DOI](https://doi.org/10.1016/j.learninstruc.2010.07.004), [author-provided abstract in ERIC](https://eric.ed.gov/?id=EJ908875). Abstract-only. Qualification: Aleven et al. (2016), *Help helps, but only so much*, [author-hosted retrospective abstract](https://www.cs.cmu.edu/~aleven/publications.html), [DOI](https://doi.org/10.1007/s40593-015-0089-1); retrospective is not counted as a primary trial.
13. Smith, M. A., Blunt, J. R., Whiffen, J. W., & Karpicke, J. D. (2016). *Does providing prompts during retrieval practice improve learning?* Applied Cognitive Psychology 30, 544–553. [author-hosted original PDF](https://learninglab.psych.purdue.edu/downloads/2016/2016_Smith_Blunt_Whiffen_Karpicke_ACP.pdf). Full text, both experiments and General Discussion.
13a. Butler, A. C., Karpicke, J. D., & Roediger, H. L. (2007). *The effect of type and timing of feedback on learning from multiple-choice tests.* JEP: Applied 13, 273–281. [DOI](https://doi.org/10.1037/1076-898X.13.4.273), [author-hosted original PDF](https://learninglab.psych.purdue.edu/downloads/2007/2007_Butler_Karpicke_Roediger_JEPA.pdf). Full text. Separate study from [13].
14. Vest, N. A., et al. (2022). *Self-explanation of worked examples integrated in an intelligent tutoring system enhances problem solving and efficiency in algebra.* Proceedings of the 44th Annual Conference of the Cognitive Science Society, 3466–3472. [Original full-text author submission in ERIC](https://files.eric.ed.gov/fulltext/ED623402.pdf). Conference study; Methods and Results inspected rather than relying on the positive title/abstract.
15. Klahr, D., & Nigam, M. (2004). *The equivalence of learning paths in early science instruction: Effects of direct instruction and discovery learning.* Psychological Science 15, 661–667. [DOI](https://doi.org/10.1111/j.0956-7976.2004.00737.x), [publisher-deposited abstract](https://api.crossref.org/works/10.1111/j.0956-7976.2004.00737.x). Abstract-only; publisher blocked.

All study claims above are bounded by the access level reported. Proposed pilot measures, decision questions, and fog patches are research synthesis, not findings that the cited authors tested this product or recommendations already accepted by its users.
