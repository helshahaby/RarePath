# RarePath Evidence Atlas

**Hossam Elshahaby · Hack Nation Hackathon 7**

RarePath is an evidence-first research atlas for rare childhood diseases. It helps families, patient groups, clinicians, and researchers move from a case profile through sourced disease and mechanism evidence toward either a justified collaboration and next research step, or an honest evidence gap with a plan to investigate it. **Research hypotheses only; not a diagnosis, prescription, or claim of treatment success. No evidence → no claim.**

## Watch the walkthroughs

- [Research journey and technical walkthrough](https://youtu.be/xQQPJHInshE) — supported connections, rejected connections, and the evidence frontier.
- [Product demo](https://youtu.be/ksX-CVb7DUA) — entering a case, tracing evidence, and separating research evidence from case relevance.
- [Team introduction](https://youtu.be/r1tJXj0CasM) — Hossam and the motivation behind RarePath.

## Features in detail

### 1. Case capture — Quick Case and Detailed Case

- **Quick Case** is a single screen for the essentials: a research focus (for example a candidate gene or disease), key symptoms, and genomic findings. A completeness meter shows how much of the profile is addressed.
- **Detailed Case** walks through eight sections: **Symptoms, Genomics, History, Conditions, Medications, Lifestyle, Family, and Biology**. Every section has an explicit status — anything you do not fill in is sent as *NOT PROVIDED* and is never guessed or filled in by the AI.
- **Suggested HPO mapping**: when you type a symptom, RarePath suggests matching Human Phenotype Ontology terms. Every suggestion must be explicitly **Accepted**, **Rejected**, or edited before it is used — nothing enters the analysis without your confirmation.
- **Pseudonymous Case ID**: use a Case ID instead of a child's real name. No account is required and no real names should be entered.
- **Genomics fields** capture gene, variant notation (HGVS c. and p.), transcript, zygosity, classification, and laboratory interpretation, plus whether each finding came from a lab report or other source.
- **Demo mode**: "Fill missing data with default values" fills only the fields marked *not provided* — it never overwrites anything you entered.

### 2. Evidence retrieval from public sources

- Queries **PubMed, ClinVar, ClinicalTrials.gov, and MONDO** for public records relevant to the case.
- NCBI sources (PubMed, ClinVar) are queried **sequentially with pauses** to respect their rate limits, and retry automatically on temporary errors.
- **Exact-variant query expansion**: if a variant is given in exact notation, several rule-based forms of the notation are searched in stages.
- **Retrieval provenance table** shows every query run: the query text, source, status (found / searched with no match / search incomplete / source unavailable), record count, retries, and time. "Search incomplete" is never presented as "no evidence".
- **Autopilot follow-ups**: after the first round, up to three additional search rounds can run automatically, each driven by the open question from the previous round. You can turn autopilot off to run a single round, or press **Stop** at any time — previous results stay on screen.

### 3. AI analysis — the model is not the evidence

OpenAI is used to **extract** sourced claims from retrieved records, **reconcile** entities (genes, diseases, variants), **plan** the next research question, and **explain** the evidence path. Three rules are enforced in code:

- The records, not the model, are the evidence. **An unsourced claim never becomes a graph edge.**
- Model-inferred elements are labeled **AI_INFERRED** and drawn as dashed nodes so they can never be mistaken for retrieved evidence.
- Each analysis round usually takes **1–3 minutes**; a live timer, round counter, and Stop button are shown while it runs.

### 4. Evidence atlas and mechanism clusters

- An interactive graph organizes evidence nodes by type — disease, gene/variant, phenotype, molecular effect, pathway, and more. Dashed = AI-inferred; solid = retrieved and cited.
- **Mechanism clusters** group mechanistically related findings. Each cluster carries two separate judgments: whether the **disease/mechanism evidence** supports it, and whether it **applies to this specific case**. A supported cluster is never automatically supported for this case.
- **Evidence confidence** is computed in code by a transparent scoring function, not by the AI. Any component without a citation counts as *Insufficient*.
- **Contradiction search** looks for records that argue against the emerging picture, so disagreement is surfaced rather than hidden.

### 5. Therapeutic research leads — three separate assessments

The app keeps three questions apart at all times:

1. **Disease/mechanism evidence** — what is known about the disease and its mechanisms.
2. **Therapeutic evidence** — what research exists about drugs, trials, and interventions for related mechanisms. Low case applicability never removes this research.
3. **Case applicability** — whether any of it plausibly applies to *this* case.

Trials are shown as **research context only**: trial status alone never establishes efficacy or failure, and a research lead is never a treatment recommendation.

### 6. Research conversation

- After each analysis, RarePath asks the **single most informative open question** about the case.
- You can **type an answer** — each answer is appended to the case as user-provided information and the analysis re-runs.
- You can **upload a lab report** (PDF, image, text, CSV, VCF, or JSON, up to 8 MB). RarePath reads the file once, proposes the fields it found, and requires you to **Confirm or Reject each field** before anything is written to the case. The file itself is never stored.
- The conversation view shows the full question-and-answer trail with the provenance of every turn.

### 7. The journey endpoint — collaboration or honest gap

The guided journey runs Disease → Gene/Variant → Phenotype → Molecular mechanism → Pathway → Mechanistically related diseases → Existing research → Experimental models → Interventions being studied → Clinical trials → Researchers → Patient organizations → Reusable research assets → Evidence gaps → Recommended next research action. It ends in exactly one of two ways:

- **A justified collaboration**: a supported cross-disease connection, a reusable asset with explicit transfer limitations, a real partner found in retrieved records, a concrete shared action, and the next validation question — every item checked against the evidence.
- **An honest gap**: the exact statement *"No supported connection found with the currently available evidence."* plus what was searched, what is unknown, what evidence is missing, and a concrete research question or experiment that could reduce that uncertainty.

A **case–research focus mismatch** (for example, exploring a disease unrelated to the case's own evidence) is reported as *"Keep clusters separate"* with a staged plan: check for patient-specific evidence first, reassess the mechanism connection only then, and only after that consider a validation experiment. Symptom overlap alone never proves a shared mechanism.

### 8. Pipeline transparency

A live pipeline panel shows every stage — normalization, HPO mapping, each source query, each AI step, clustering, graph building, contradiction search, and evidence scoring — with its real execution status: completed, partial, not run, or source unavailable. Status reflects what actually ran, not what was hoped for.

### Guided demos

- **Demo A · Supported connection** — a GLUT1 deficiency example.
- **Demo B · Connection rejected** — Rett/MECP2 versus GLUT1/SLC2A1, where the mechanistic bridge fails and the connection is rejected.
- **Demo C · Evidence frontier** — a sparse case that lands on an honest gap with a plan.

The sample cases are illustrative, not real patients. A fixed demo evidence snapshot is not yet available; live searches may return different results between runs.

## How to use RarePath

1. **Open the app** — [live version](https://gene-whisperer-child-care.lovable.app) or run locally (below). A pseudonymous Case ID is created for you.
2. **Enter the case.**
   - Fast path: open **Quick case**, enter a research focus (e.g. a candidate gene), key symptoms, and any genomic findings, then press **Run evidence analysis**.
   - Full path: choose **Detailed case →** and walk through Symptoms, Genomics, History, Conditions, Medications, Lifestyle, Family, and Biology. Leave unknown sections empty — they are recorded as *not provided*, never guessed.
   - For each suggested HPO term, press **Accept**, **Reject**, or pick a different term. Only accepted terms are used.
3. **Or start from a demo.** Press **Demo A**, **B**, or **C** to load an illustrative case and run it immediately.
4. **Choose how deep the search goes.** Turn **Autopilot** on for up to three follow-up search rounds, or off for a single round. Each round usually takes 1–3 minutes; a timer and round counter are shown while it runs.
5. **Watch the pipeline** fill in as sources are queried and the AI steps run. If anything takes too long, press **Stop** — the results gathered so far stay on screen.
6. **Read the results.**
   - Check the **retrieval provenance** table first: what was searched, in what sources, and what came back.
   - Explore the **evidence graph** and **mechanism clusters**. Solid nodes and edges are cited to retrieved records; dashed ones are AI-inferred and never load-bearing.
   - Read the **therapeutic research leads** with all three assessments in mind: what the evidence shows, what therapeutic research exists, and whether it applies to this case.
7. **Continue the conversation.** Answer the open question by typing what you know, or upload a lab report and confirm each extracted field. The analysis re-runs with your new information.
8. **Follow the journey to its endpoint.** You will reach either a justified collaboration with a shared next action, or the honest-gap statement with what was searched, what is unknown, what is missing, and the next question to investigate. If a connection was rejected, follow the staged mismatch plan rather than testing the unsupported bridge directly.
9. **Manage cases.** Case profiles and analysis runs are stored **in this browser on this device**, append-only. Clearing browser data removes them; there is no account or cross-device sync.

### Reading the results honestly

- **No evidence ≠ no effect.** "No supported connection found" means the current retrieval did not support a claim — it is never a statement that a connection is scientifically impossible.
- **Not established in current retrieval** is the standard phrase for missing knowledge — never "scientifically unknown".
- **Source unavailable** or **search incomplete** means that source could not be fully queried; those runs are marked as not evaluated.
- **Confidence is computed, not asserted.** Uncited components score as *Insufficient* by design.
- Trial status alone does not establish efficacy, and a research lead is not a treatment recommendation.

## Privacy and limitations

Use a Case ID rather than a child's real name. Do not enter directly identifying information unless necessary and permitted; remove names and dates of birth from reports before uploading. Case profiles and analyses are stored in this browser on this device, without accounts or cross-device sync. Clearing browser data may remove them. Uploaded reports are read once to extract fields and are never stored. This prototype is for research navigation, not clinical decision-making. Never fabricate treatments, trials, drugs, genes, papers, researchers, or organizations — no evidence, no claim.

## Run locally

This is a TanStack Start, React, TypeScript, and Tailwind CSS application.

```sh
bun install
bun run dev
```

Open the local address printed by the development server. Live evidence and AI analysis depend on the configured external services and credentials in the hosting environment; never commit private API keys.

## Project links

- [Live RarePath application](https://gene-whisperer-child-care.lovable.app)
- [Research video](https://youtu.be/xQQPJHInshE)
- [Product video](https://youtu.be/ksX-CVb7DUA)
- [Team video](https://youtu.be/r1tJXj0CasM)
