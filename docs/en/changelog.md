# Changelog

[Español](../es/changelog.md) · [Back to README](../../README.md)

Weekly log of new work done on AWS and NVIDIA. Newest first. Numbers come from our own logs; estimates are marked.
Updated by a weekly routine of the operations agent (see `ROUTINE.md`).

## Week of 9–13 Nov 2026

**Credits and programs**
- Documented the US$10k AWS Activate Portfolio credits obtained through NVIDIA Inception (Oct 2026) and the next tier we are asking for (AWS Credits for AI Startups, via our AWS Account Manager).
- Requested the NVIDIA Brev credits pilot for Inception members.
- Meeting with AWS Startups booked for 20 Nov 2026.

**Admissions AI on NVIDIA Nemotron (SageMaker + Bedrock)**
- Built the anonymized research corpus in S3: 1.3M chat messages, 86k prospects, 80 schools (~193 GB incl. checkpoints).
- Built per-school fact sheets (prices and scholarships stripped in code); training sheets written by Nemotron on GPU.
- Built the harness (rules + fact sheet + code guard + one retry) and a 300-turn blind exam on 6 held-out schools.
- Ran five iterations: LoRA v3 and v4 (worse than untrained, kept as lessons), harness v0 and v1, and **LoRA v5
  distillation: 7.2 answering instantly vs 6.5 untrained (judge 1), 7.4 with reasoning, 0% price quotes**.
- Added a second, stricter judge (Opus 5.5) and Fable 5.2 as reference: Fable 8.2, Sonnet 7.8, Nemotron v5 6.7,
  today's CRM AI 4.2.
- Analyzed every closed conversation (counts only): 48% of prospects write after hours; inviting to visit goes with
  2.4× more bookings within the same school.
- Appointment ground truth: 354,602 leads in 101 schools, 2.8% book a visit.
- Live progress page for the team with an idea box answered by the agent.
- SageMaker: 29 jobs, 24.8 billable hours, $299.51 (estimate at list price). Bedrock research: 11,932 calls.

**Narrative (13 Nov)**
- **13 Nov:** the showcase now leads with SuperLeads' mission and growth story (~100 institutions, 100% bootstrapped,
  people and admissions know-how), a "Growing together with AWS" section, Constanza (our AI and future admissions
  harness) and calls as the next growth line (transcription, call analytics, voice agents; plan with estimates).

**Operations agent**
- Training watcher (every 15 min) with WhatsApp notices; Bedrock model-availability probe (every 3 h).
- Agent dashboard renamed and moved behind single sign-on; instructions by text or voice note.
- Weekly showcase routine created (this repository).

## Week of 2–6 Nov 2026

- **3 Nov:** the operations agent moved from a laptop to an EC2 m7i.2xlarge in mx-central-1, running 24/7 with
  Claude Opus 5.5 on Bedrock.
- Scheduled routines for content, SEO, sales signals and supervision migrated to the server.

## October 2026

- **1 Oct:** applied to NVIDIA Inception. **8 Oct:** accepted.
- Bedrock became the model provider of the operations agent.
