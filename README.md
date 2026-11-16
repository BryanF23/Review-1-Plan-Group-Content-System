# Review 1 — Plan: Shared Python Study Guides

**Team:** Angelo Bustamante and Bryan  
**Due:** November 17, 2026, 1 p.m. Eastern  
**Status:** Plan stage — not yet built. No application, local prototype, retrieval index, or demo exists in this repository.  
**Repository and shared record:** https://github.com/ab2737/Review-1-Plan-Group-Content-System

This is the single evolving group record. Proposed responsibilities below are assignments, not claims of completed individual work. Both members still need to review and adopt the plan.

## 1. Problem, reader, and bounded scope

Students learning introductory Python often save explanations in separate bookmarks, messages, and personal chats. Useful material becomes difficult for a study partner to find, verify, or improve. Our educational-content publishing project will turn selected learning sources into a shared collection of short, cited study guides.

**Primary reader:** an introductory programming student who wants a clear explanation and a small example. **Contributors/reviewers:** Angelo and Bryan, taking turns drafting and checking content.

**Research question:** How can a small team use approved Python documentation and a shared RAG workflow to create accurate, source-cited study guides that students can review, reuse, and improve?

The first version will cover Python lists, tuples, sets, and dictionaries using at most 10 approved sections from the official Python tutorial. It will produce five reviewed study guides. It will not collect social-media feeds, private course materials, student records, or graded assignment answers. It will not attempt to answer questions outside its approved collection.

A guide will contain a question/title, a short explanation, an original example, source-section links, author, reviewer, source version, and review status.

**Shared value:** sources, drafts, feedback, and approved guides persist in one collection. A second person must check a draft before publication. Both teammates can reuse and revise the same approved guide. Copying an individual chat does not provide this source register, review queue, or version history.

### Observations and assumptions

**Observations from the current project:**
- The proposed educational source is the official Python documentation, with its license and reuse conditions recorded in Section 2.
- The team has two members who can take turns creating and reviewing study guides.
- The project is currently at the Plan stage; no application, retrieval index, or publishing workflow has been built.
- G-001 provides one manually traced example, but it does not demonstrate a working ETL or RAG pipeline.

**Assumptions to investigate:**
- Introductory programming students would find short explanations and small examples useful.
- Retrieving up to three relevant passages will provide enough evidence for most questions in the four selected Python topics.
- A small vector-based RAG approach will be sufficient for the initial collection.
- A two-person review process will help catch incorrect explanations, missing citations, and unsupported claims.

These assumptions are not user-research findings. The team will evaluate them during Build using the planned user stories and success criteria.

## 2. Permitted sources and source records

| Source | Use | Permission/access rule |
| --- | --- | --- |
| [Python tutorial: Data Structures](https://docs.python.org/3/tutorial/datastructures.html) | Initial educational corpus; select sections on the four scoped topics | Public documentation under the [PSF license](https://docs.python.org/3/license.html); retain required license/copyright notices for redistributed source text and describe modifications. |
| Team-written examples and explanations | Original guide content | Include only material authored for this project; record its author. |
| Research references in Section 4 | Architecture decisions | Link and paraphrase for research; not automatically part of the indexed educational corpus. |

Public access alone will not qualify a new source for ingestion. The source register must record its permission basis first. Do not copy classmates' work, LMS content, private messages, or unlicensed third-party tutorials.

Each source record will include: source ID, title, exact URL and section anchor, publisher, documentation version, access date, permission/license URL, required notices, topic, and content hash. Each chunk will retain its source ID, heading, chunk ID, and source version.

The unversioned Python URL can change. At Build, pin a documentation version and keep the retrieved source hash so updates are detectable. Permission to reuse documentation does not imply permission to ignore its conditions.

## 3. Source-to-transform-to-reviewable-output workflow

| Step | Planned behavior | Reviewable evidence/output |
| --- | --- | --- |
| Extract | A contributor manually selects an allowed section and records source metadata. Fetch only approved pages. | Source register entry and source snapshot/hash. |
| Clean and split | Remove menus/navigation, preserve headings and code blocks, split by section into approximately 200–400-word chunks, and detect duplicates by normalized-text hash. | Chunk records linked to original sections; rejected-duplicate log. |
| Index and retrieve | Embed permitted chunks and the question; filter by topic/version; rank by cosine similarity and return up to three chunks. | Ranked chunk IDs, scores, source URLs, and model/version. |
| Draft | Supply the retrieved passages to a language model to draft a short guide with citations. Treat source text as data, not instructions. | Draft, prompt/model record, retrieved chunk IDs, and claim-to-source mapping. |
| Review | The other teammate checks claims, citations, example behavior, and source permissions; requests changes or approves a specific revision. | Reviewer identity, decision, feedback, and revision ID. |
| Publish and maintain | Add approved revisions to the shared guide list. Changes to a guide or its source return it to review. | Approved collection and retained history of revisions. |

Planned states: **draft → needs review → changes requested → needs review → approved**. A reviewer can also approve directly from needs review. The author cannot approve their own guide. Source changes mark affected guides stale until rechecked.

**Planned implementation:** Python for extraction and embeddings, JSON/SQLite for metadata and review records, a small local web interface for the shared collection, and GitHub for versioned project evidence. Exact cosine search is sufficient for the small initial corpus; a dedicated vector database is unnecessary for this version. Model selection, execution environment, and generation-service access remain Build decisions. If generation access is unavailable, manually authored drafts can test the review workflow, but will be labeled manual and will not count as an implemented RAG feature.

## 4. Research and decision record

References were reviewed during preparation on November 16, 2026. The following summaries are limited to the cited material; the implementation choice is a project judgment, not a measured performance claim.

| ID | Research | Finding and relevance |
| --- | --- | --- |
| R1 | [Lewis et al., Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | RAG combines retrieval from external information with generation. This motivates retrieving source passages before drafting rather than relying only on model memory. It does not guarantee correctness. |
| R2 | [Sentence Transformers: Semantic Search](https://www.sbert.net/examples/sentence_transformer/applications/semantic-search/README.html) | Semantic search embeds queries and documents and ranks their similarity. Query/document model suitability matters. This supports retrieving a relevant explanation even when a learner uses different wording. |
| R3 | [Microsoft GraphRAG documentation](https://microsoft.github.io/graphrag/) | GraphRAG extracts entities and relationships and uses graph structures and community summaries. It targets questions requiring connections across information or summaries of broader collections. This introduces extra extraction and review work for our small corpus. |

**Vector:** a numeric representation of text used to search for similar meaning. **Graph:** entities/concepts connected by explicit relationships. **RAG:** retrieve evidence and use it as context for generation; it is a workflow, not itself a database.

### Two approaches compared

| Dimension | A: Vector RAG with metadata and human review | B: Graph-assisted RAG with vector retrieval |
| --- | --- | --- |
| Retrieval | Similarity-ranked passages, filtered by topic/version | Passages plus concept relationships and graph traversal |
| Best fit | Focused questions such as how a list method works | Connected questions about concepts and prerequisites across sources |
| Additional stored data | Chunks, embeddings, provenance, reviews | All of A plus extracted entities, edges, and their evidence |
| Main weakness | Similar text can still be irrelevant; multi-step connections may be missed | Incorrect/missing relationships can mislead retrieval; graph maintenance adds work |
| Two-person feasibility | Smaller implementation and evaluation surface | More components to build and independently validate |
| Decision | Selected for the first version | Deferred; not implemented |

**D1 — Select vector-based RAG, without a graph database.** The scoped output is a short explanation grounded in a few sections, so passage retrieval is a reasonable starting point. Metadata links between a guide and its sources are provenance records, not a claim that GraphRAG has been implemented.

**D2 — Require independent human approval.** Retrieval scores and fluent writing cannot establish factual correctness. Review must verify each claim against its cited section and run each example.

**D3 — Reconsider graphs only if evidence justifies them.** If at least three of ten later evaluation questions require cross-source relationships that passage retrieval consistently misses, document those failures before proposing graph support. This threshold is a planning rule, not an observed result.

A simple keyword search will also be measured as a low-cost retrieval baseline during Build. If vectors do not help on the same questions, report that result instead of assuming the more complex option is better.

## 5. Four checkable stories and ownership

| ID | User story | Owner | Different reviewer | Acceptance check planned for Build |
| --- | --- | --- | --- | --- |
| S1 | As a contributor, I can add a permitted section so teammates can trace its content. | Angelo | Bryan | A valid entry retains all required source fields; an entry without permission evidence is rejected; importing identical content twice creates no duplicate chunks. |
| S2 | As a learner, I can request a cited draft so I can inspect its supporting evidence. | Bryan | Angelo | For a supported question, show up to three retrieved chunks and a draft with valid source links; for an unsupported question, report insufficient evidence instead of publishing an answer. |
| S3 | As a reviewer, I can request changes or approve a particular revision so only checked guides enter the collection. | Angelo | Bryan | Self-approval is rejected; requested changes prevent publication; another person's approval publishes that revision; editing it resets approval. |
| S4 | As a teammate, I can find approved guides and their history so the group can reuse and maintain them. | Bryan | Angelo | Both teammate accounts can find the same approved guide by topic, see its citations and review record, and distinguish a stale/revised version from the previously approved one. |

## 6. Success criteria and evaluation plan

These are future targets, not completed test results.

| Measure | Target | How to check |
| --- | --- | --- |
| Source traceability | 100% of indexed chunks have valid source ID, section, version, and permission evidence | Inspect all records in the small corpus; attempt one missing-permission import. |
| Retrieval relevance | At least 8 of 10 supported questions return a reviewer-designated supporting chunk in the top three | Freeze questions and expected sections before tuning; save question, expected IDs, returned IDs, and result for vector and keyword retrieval. |
| Unsupported questions | All 3 out-of-scope questions remain unpublished and receive an insufficient-evidence response | Test weather, personal grades, and an excluded programming topic. Similarity alone is not an evidence-sufficiency rule. |
| Citation support | Every factual claim in the 5 approved guides is supported by its cited passage | Reviewer records claim, supporting section, pass/fail, and correction. |
| Example accuracy | All code examples in the 5 approved guides run with documented expected results | Save runtime version, input, expected output, actual output, and result. |
| Shared workflow | All S3 approval cases and both-account visibility in S4 pass | Record actions, revision IDs, actor/reviewer identities, and outcomes. |

At Build, each teammate will author five supported evaluation questions and check the other's expected source labels before tuning. Keep failed cases. At Improve, record a real peer-requested change, its linked revision, and a repeat of the relevant check; no such change or retest has happened yet.

## 7. Actual Plan-stage checks and worked example

| Check | Actual result | Evidence/limit |
| --- | --- | --- |
| Inspect repository before drafting | GitHub contents API reported an empty repository. | No existing implementation or plan was available to review. |
| Open primary research references | R1, R2, and R3 returned readable content supporting the summaries above. | Direct links in Section 4; this is source reading, not a runtime benchmark. |
| Check candidate corpus access and permission basis | Python Data Structures and its license page returned readable content. | Section 2 links; documentation license identifies reuse conditions. No corpus ingestion has run. |
| Walk through one source-backed guide manually | The example below can be traced to the lists section, and its code output was checked during review. | This is a manual example/code check only; no retrieval or generation pipeline was tested. The required peer review by another group member is still pending. |
| Inspect ownership matrix | Four stories have named owners and different reviewers; each teammate owns two. | Section 5. Assignment does not establish completed effort. |

### Manual walkthrough: example guide G-001

- **Question:** How do I add one item to the end of a Python list?
- **Source:** [Python tutorial, More on Lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists).
- **Source record sketch:** PY-LISTS-001; publisher: Python Software Foundation; accessed: November 16, 2026; permission: PSF documentation license; version/hash: pending actual ingestion.
- **Draft explanation:** Use the list's append method to add one item at its end.
- **Original illustrative example:**

~~~python
topics = ["lists"]
topics.append("dictionaries")
print(topics)
# Expected: ['lists', 'dictionaries']
~~~

- **Manual evidence check:** The cited section describes append as adding an item to the end. This supports the draft's single factual claim.
- **Execution:** The example was run during review on November 16, 2026. Actual output: `['lists', 'dictionaries']`. This verifies the example's output in that review environment; it does not test the planned ETL, retrieval, or publishing system.
- **Source check:** The linked Python tutorial section supports the explanation that `append()` adds an item to the end of a list.
- **Code check:** During review, the example was executed and produced `['lists', 'dictionaries']`. This checks the example's output in that review environment; it does not test the planned ETL, retrieval, or publishing system.
- **Review status:** G-001 is still **pending approval** because its documentation version and content hash have not been recorded and the independent group approval step has not been completed.
- **Workflow lesson:** The source URL alone is insufficient for reproducibility; ingestion must also capture version and hash before the guide can be approved.

### Preliminary G-001 review — peer review still pending

- **Review completed:** The source claim and the example's output were checked.
- **Example reviewed:** G-001 — Adding one item to the end of a Python list.
- **Source check:** The linked Python tutorial section supports the explanation of `append()`.
- **Code check:** The example produced `['lists', 'dictionaries']` when executed during review.
- **Feedback/recommendation:** Record the documentation version and content hash before approval. Keep G-001 pending until the source metadata is complete and the independent review/approval step is recorded.
- **Reason:** A successful code check alone does not verify source reproducibility or the full ETL, retrieval, review, and publishing workflow.
- **Peer-review requirement:** Not yet fulfilled. Angelo or another classmate still needs to follow the G-001 source-to-output trace and question one assumption or tradeoff. Their actual feedback must be recorded here.
- **Group response and decision:** Pending the peer's feedback and the team's discussion.
- **Documentation changes:** This README now distinguishes the source/code check from the required human peer review. Further changes depend on the peer feedback.
- **Retest:** The code example was executed during review. No ETL, retrieval, or publishing workflow retest has been performed.

## 8. Balanced work allocation and schedule

Equal work means comparable responsibility, not a claim that equal effort has already occurred. Estimated planning/review effort is six hours per member; implementation estimates will be revised at Build.

| Member | Plan ownership and estimated effort | Build ownership | Cross-review |
| --- | --- | --- | --- |
| Angelo Bustamante | Problem/sources (1.5 h), ETL/review workflow (1.5 h), two stories and integration (1 h), review/evidence (2 h) | S1 source ingestion and S3 review controls | Review Bryan's research, evaluation, S2, and S4. |
| Bryan | Research/approach comparison (1.5 h), success criteria/evaluation (1.5 h), two stories and limitations (1 h), review/evidence (2 h) | S2 retrieval/drafting and S4 shared collection/history | Review Angelo's scope, permissions, ETL, S1, and S3. |

**Proposed milestones, Eastern time:**

- November 16: Read the draft and sources; confirm or revise the topic, ownership, and decisions.
- November 17 by 10 a.m.: Complete cross-review and record specific findings/corrections with links.
- November 17 by noon: Verify submission links, status labels, and individual contribution notes.
- November 17 before 1 p.m.: Each member submits the same shared record/repository plus their own truthful note.

## 9. Limitations, help received, and contribution record

Similarity is not truth. Source passages can be relevant yet incomplete, generation can add unsupported statements, and a small Python-only evaluation will not demonstrate quality on other subjects. Reviewers can also make mistakes. Version tracking, claim-level checks, and retained failures reduce these risks but do not eliminate them.

The topic and technical design are proposed in this draft; no instructor approval beyond the assignment's educational-content option is claimed. Hosting, model access, costs, account controls, and actual retrieval quality remain unresolved Build concerns.

**Work completed during preparation:** The project scope and plan were drafted, the repository and linked references were reviewed, the work split was organized, and G-001 received a manual source/code check. No student interviews or peer feedback from Angelo or another classmate have been completed. No working app has been built, and the ETL/retrieval/publishing workflow has not been tested. Both members must verify the claims and revise or adopt the work before submission.

| Person | Contribution verified in this preparation | Assigned work still requiring personal completion |
| --- | --- | --- |
| Angelo | Supplied the assignment and repository, identified Bryan as partner, requested an even split and creation of the plan on GitHub. | Read and revise owned sections, verify sources/workflow, review Bryan's sections, and record actual evidence. |
| Bryan | Participated in a review of G-001: checked the cited Python documentation against the explanation, reviewed the code output, and identified missing source-version/hash metadata and pending independent approval. Also reviewed Angelo's individual portfolio and RAG repository, recommending clearer project links, research visibility, and testing evidence. | Personally verify/adopt the group-plan research, architecture, evaluation criteria, S2/S4, and Angelo's source permissions and ETL workflow; ask Angelo or another classmate to complete the required peer trace review, then record the actual feedback and group decision. |

### Individual submission notes — current truthful status

**Angelo:** I provided the assignment requirements and repository and requested a shared plan with an even division of responsibilities. The initial draft is a working proposal that still needs my review and revision. My assigned areas are scope, permitted sources, the ETL/review workflow, and stories S1/S3. My personal review and revisions are still pending. Relevant evidence: [workflow](#3-source-to-transform-to-reviewable-output-workflow), [stories](#5-four-checkable-stories-and-ownership), and [research/decisions](#4-research-and-decision-record).

**Bryan:** I participated in a review of G-001. The linked Python tutorial supports the explanation, and the example produced `['lists', 'dictionaries']` when executed during that review. I identified that the documentation version and content hash are still missing, so G-001 should remain pending approval until its source metadata and independent approval are complete. This check does not replace the assignment's required peer review; I still need to ask Angelo or another classmate to follow the trace, question an assumption or tradeoff, and record their actual feedback and the group's response. I also reviewed Angelo's individual portfolio and RAG repository and gave feedback on project links, research visibility, and testing evidence. My remaining assigned areas are research/approach comparison, evaluation criteria, and stories S2/S4. Relevant links: [research/decisions](#4-research-and-decision-record), [evaluation](#6-success-criteria-and-evaluation-plan), [stories](#5-four-checkable-stories-and-ownership), and [preliminary G-001 review](#preliminary-g-001-review--peer-review-still-pending).

## 10. Changed / checked / next

**Changed:** Created a project-specific shared plan with a bounded Python corpus, source policy, ETL workflow, two-approach comparison, four stories, balanced ownership, evaluation targets, and an illustrative guide.

**Checked:** Preparation checks included inspecting the initially empty repository, reading three primary technical references and the candidate corpus/license, and tracing G-001's claim to its source. During the G-001 review, the code example was executed and produced the expected output. The source version/hash and required human peer review/independent approval remain pending. No application, ETL, retrieval, or publishing workflow tests are claimed. See Section 7.

**Next:** Angelo and Bryan review and adopt/revise the plan; each records actual contributions and cross-review findings. Then implement the scoped pipeline and run the planned evaluation. Add a demo link only after one exists.

**Submission:** Each member links this same README and repository, includes their own updated contribution and changed/checked/next note, and links relevant research/decisions. Reuse this record instead of creating duplicate reports.
