# RarePath Evidence Atlas

**Hossam Elshahaby · Hack Nation Hackathon 7**

RarePath is an evidence-first research atlas for rare childhood diseases. It helps families, patient groups, clinicians, and researchers move from a case profile through sourced disease and mechanism evidence toward either a justified collaboration and next research step, or an honest evidence gap with a plan to investigate it. **Research hypotheses only; not a diagnosis, prescription, or claim of treatment success. No evidence → no claim.**

## Watch the walkthroughs

- [Research journey and technical walkthrough](https://youtu.be/xQQPJHInshE) — supported connections, rejected connections, and the evidence frontier.
- [Product demo](https://youtu.be/ksX-CVb7DUA) — entering a case, tracing evidence, and separating research evidence from case relevance.
- [Team introduction](https://youtu.be/r1tJXj0CasM) — Hossam and the motivation behind RarePath.

## What it does

- Capture symptoms, genomics, history, medications, family observations, and biological evidence in Quick Case or Detailed Case. Suggested terminology and uploaded report findings require explicit confirmation.
- Retrieve public records from PubMed, ClinVar, ClinicalTrials.gov, and MONDO; show source status, citations, retrieval provenance, and gaps. Source availability and results vary between runs.
- Use OpenAI to **extract** sourced claims, **reconcile** entities, **plan** research questions, and **explain** paths. The records, not the model, are the evidence; unsourced graph edges are not accepted.
- Explore mechanism clusters, reusable assets, people and partners found in retrieved records, and therapeutic **research** leads. Disease/mechanism evidence, therapeutic evidence, and applicability to this specific case remain separate.
- Continue through a research frontier to a shared action when the evidence supports one, or to an honest gap and specific next question when it does not. Symptom overlap alone never proves a shared mechanism.

The guided demos include a GLUT1 example, a Rett/MECP2 versus GLUT1/SLC2A1 connection-rejected example, and an evidence-frontier example. The sample cases are illustrative, not real patients. A fixed demo evidence snapshot is not yet available; live searches may return different results.

## Privacy and limitations

Use a Case ID rather than a child's real name. Do not enter directly identifying information unless necessary and permitted; remove names and dates of birth from reports before uploading. Case profiles and analyses are stored in this browser on this device, without accounts or cross-device sync. Clearing browser data may remove them. This prototype is for research navigation, not clinical decision-making. Trial status alone does not establish efficacy, and a research lead is not a treatment recommendation.

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
