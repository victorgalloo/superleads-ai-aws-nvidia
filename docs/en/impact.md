# Impact: why this matters

[Español](../es/impact.md) · [Back to README](../../README.md)

## Who we are

SuperLeads is the CRM and admissions system for private schools and universities in Mexico and Latin America.

- **About 100 institutions served** (schools and universities). According to public press coverage we grew from
  5 schools (Nov 2024) to 54 (Nov 2025) to 100 (Sep 2026).
- **Our most important asset is our people and our know-how in education admissions.** The rules our AI follows
  (invite to visit, never quote prices by message, never invent facts) come from that know-how, and our data confirms
  them (see below).
- **100% bootstrapped**, with no outside funding. Cloud spend is measured job by job.
- **Mission:** every interested student or family gets a fast, warm answer and an invitation to visit, at any hour.

## The problem

Private schools and universities fill seats through admissions teams that answer WhatsApp, web chat, calls and email.
Most prospects are lost not because the school is wrong for them, but because nobody answered in time, nobody invited
them to visit, or nobody followed up.

## What the data says (1.3M anonymized messages, 86k prospects, 80 schools)

All comparisons are made **within each school** and then averaged; mixing schools reversed one result. These are
associations, not causal effects; a pilot is needed for causality.

| Finding | Number | What our AI does about it |
|---|---|---|
| Prospects who first write outside office hours (Mon–Fri 9–18, Mexico City time) | **48 of 100** | Answers at any hour |
| Conversations where nobody invited the prospect to visit | **52 of 100** | Invites in ~90% of replies (today's AI: ~30%) |
| Booking rate when someone invited vs nobody invited | **17 vs 7 per 100** (30 of 35 schools) | Moves every reply toward the visit |
| Prospects who ask the price | 26 of 100 | Never quotes; invites to see prices at the visit |
| Early price question: invite without price vs give price by message | 15 vs 9 per 100 book (7 of 9 schools; 1,999 cases) | Code-level guard: 0% price quotes |
| Reply the same day vs after more than a day | 10 vs 5 per 100 book | Replies instantly |
| Bookers who book within 24 h / within a week | 54 / 73 of 100 | The invitation must come in the first chat |
| Prospect goes silent and nobody writes again | 43 of 100 | Multi-day follow-up is on the roadmap |
| Leads that book a visit (101 schools, 354,602 leads, 12 months) | **2.8%** | The metric we want to move |
| Schools where today's AI books in chat vs staff only | 8.5% vs 2.8% of leads book | Different schools; suggestive, not proof |

## Constanza: one AI, from operations to admissions

**Constanza** is SuperLeads' AI. Today she runs our operations 24/7 on Amazon EC2 (mx-central-1) with Claude on Amazon
Bedrock, built on the open-source Hermes Agent framework.

- **A team of specialized AI "employees"** under her coordination: one per social network, one for SEO and websites,
  one for clients and partners. All are visible on our supervisor dashboard.
- **50 enabled scheduled routines** (67 defined) on one EC2 server: daily content per social network, technical SEO
  audits, an SEO autopilot for two sites, a weekly check of how AI assistants answer questions about our market,
  public-data sales signals (new admissions directors, new programs, job posts), partnership research and supervision.
- **Self-supervision**: a live dashboard, routine reports every 30 minutes, a daily review of failing routines and a
  daily work log, so a small team can run what would otherwise need several people.
- **Instructions by WhatsApp voice notes**; she answers by text or voice.

**Constanza will be our harness.** The harness measured below (per-school fact sheets, rules, style examples, code
guard for prices and scholarships) is what will run admissions conversations with prospective students. With Claude
Fable 5.2 on Bedrock it scored 8.2 / 10; with our own fine-tuned Nemotron, 6.7. Offline exam only, not production yet.

## Measured gains of our admissions AI (offline, blind exam)

| Metric | Today's AI | Our fine-tuned Nemotron (v5) | Best reference (Fable 5.2) |
|---|---|---|---|
| Overall quality (judge 2) | 4.2 | 6.7 | 8.2 |
| Offers the visit | 32% | 92% | 94% |
| Quotes a price | 8% | **0%** | 0% |
| Invents a fact | 34% | 5% | 2% |
| Preferred over today's AI | — | 77% | 96% |

This is an offline exam, not production. The next step is a human-agreement check and a one-school pilot.

## Calls: the next growth line

Phone calls are where many admissions are decided, and today they are a blind spot.

| Item | Status |
|---|---|
| Call records in the CRM (metadata only) | **208,751 calls; 621 hours longer than 30 s** (measured) |
| Transcribed so far | None (measured) |
| Transcription cost for the 621 h | ~US$894 with Amazon Transcribe at list, or ~US$60 with NVIDIA Parakeet / Canary on one L40S (~20 GPU-h) (**estimates**) |
| Call analytics for admissions coaching | **Plan**: what the best advisors say, where calls lose the visit, scored with our blind-exam method |
| Voice agents that book campus visits | We build them with Dapta today (one goal per call: a visit with date and time). **Plan**: Constanza's harness and fact sheets behind them, with the same price and scholarship guard |

## Measurable outcomes in our own commercial process

- **95% attendance in a full day of meetings.** On a day with back-to-back meetings with school and university
  directors (10:00–16:00), 95% of booked meetings took place. It is a single-day milestone, publicly posted by the
  sales lead, not a sustained average. He attributed it to AI, automations and the team; the tools involved include
  automated pre-meeting research briefs, a follow-up engine that proposes real calendar slots, and Constanza.
- **Every booked diagnostic meeting gets a research brief** the same morning, generated automatically.
- **Follow-up engine**: each salesperson opens one card with whom to write, why now, and a drafted message; the CRM is
  updated automatically.
- **Video at near-zero cost**: brand videos in 2–3 minutes, about MXN $1 per promo video.

## Why AWS and NVIDIA should care

1. **A growing customer, not an experiment.** ~100 institutions, bootstrapped, already running its AI on AWS in
   mx-central-1. Every new institution adds Bedrock, SageMaker and (soon) speech workloads.
2. **A real, under-served market.** Spanish-language admissions in LATAM; few vendors build models for it.
3. **A reproducible method.** Anonymize → fact sheets → harness with code guards → blind exam → distill. It already
   produced a measurable lift at a total GPU cost of $299.51.
4. **Efficient by design.** The distilled Nemotron answers at ~1/20 of the teacher's tokens, which makes an always-on
   endpoint affordable for schools.
5. **Privacy and policy by construction.** PII never leaves the anonymizer; price and scholarship policies are
   enforced in code; data stays in AWS.
6. **Honest measurement.** We report failures (two fine-tunes that made the model worse) and judge bias next to wins.
