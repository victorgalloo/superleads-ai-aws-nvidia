# SuperLeads: growing together with AWS and NVIDIA

**[Leer en español](README.es.md)**

SuperLeads is the CRM and admissions system for private schools and universities in Mexico and Latin America.
This repository shows what we have built with **AWS credits** and as a member of **NVIDIA Inception**, what it already
does for our business, and where we want to grow with AWS next.

It contains **documentation only**: no source code, no customer data, no credentials.

## Why SuperLeads

- **Mission.** Help private schools and universities fill their seats by making sure every interested student or
  family gets a fast, warm answer and an invitation to visit the campus, at any hour.
- **About 100 institutions served** (schools and universities) in Mexico and LATAM.
- **Our most important asset is our people and our know-how in education admissions.** The AI is built on that
  know-how and is measured against the rules our admissions experts use.
- **100% bootstrapped.** We have grown with no outside funding. Every dollar of cloud spend has to earn its place,
  which is why we measure everything below.
- **A real, under-served market.** Spanish-language admissions in LATAM, where 48 of 100 prospects first write
  outside office hours and 52 of 100 conversations never get an invitation to visit.

## Growing together with AWS

**What we built on credits so far (Oct–Nov 2026):**

- **Constanza, our AI, live 24/7** on Amazon EC2 in **mx-central-1 (Mexico)** with Claude on **Amazon Bedrock**.
- **Our own admissions model**: NVIDIA Nemotron fine-tuned on **Amazon SageMaker** GPUs (29 jobs, $299.51 at list
  price), from an anonymized corpus of 1.3M messages in **Amazon S3**.
- **A blind exam** of 300 real prospect messages, judged on Bedrock, that tells us which system to trust with a school.

**What grows with us.** Every new institution adds conversations, calls and routines. The stack is built so that
going from ~100 institutions to several times that adds cloud workload, not headcount in the same proportion.

| Workload | AWS service | Today (measured) | As we grow (plan; costs are estimates) |
|---|---|---|---|
| Constanza, operations | Bedrock (Claude) + EC2 mx-central-1 | ~14,400 model calls since Oct; 50 active routines | More routines and channels; ~US$9.3k for the next 6 months |
| Constanza, admissions conversations | Bedrock (harness) + SageMaker endpoint | Offline exam only: 8.2 / 10 vs 4.2 for today's AI | One-school pilot ~US$7k for 3 months, then institution by institution |
| Our own model | SageMaker GPUs (L40S, H100) + S3 | 29 jobs, 24.8 h, ~193 GB in S3 | Next research track ~US$0.9k–2.1k; larger Nemotron on H100 |
| Calls | Amazon Transcribe or NVIDIA ASR on GPU | 621 h of calls in the CRM, none transcribed yet | ~US$60 (GPU) to ~US$900 (Transcribe) for the backlog; grows with each institution |

Region matters: Constanza already runs in **mx-central-1**, and we want to be a reference case for it.
Plan and asks: [Roadmap and credits ask](docs/en/roadmap-and-credits-ask.md).

## Constanza, SuperLeads' AI

Constanza is the name of our AI. Today she is our **operations agent**, built on the open-source Hermes Agent
framework and running 24/7 on EC2 (mx-central-1) with Claude on Bedrock.

- **Coordinates a team of specialized AI "employees"**: one per social network, one for SEO and websites, one for
  clients and partners. Each one is visible on our supervisor dashboard, with its routines, reports and failures.
- **Runs ~50 active routines** (67 defined): daily content, technical SEO, AI-answer visibility, public sales signals,
  partnership research, training watchers and self-supervision.
- **Takes instructions by WhatsApp voice notes** and answers by text or voice.

**Constanza will be our harness.** The same harness (per-school fact sheets, rules, style examples and a code guard
that blocks prices and scholarships) scored **8.2 / 10** with Claude Fable 5.2 on Bedrock in our blind exam, against
4.2 for the admissions AI schools use today, with **0% price quotes** and the visit offered in 94% of replies. That
harness is what will run admissions conversations with prospective students: chat first, then the phone, under one
name. Our own fine-tuned Nemotron scores 6.7 inside the same harness at ~1/20 of its teacher's tokens; the plan is a
router (Nemotron for simple turns, Claude on Bedrock for hard ones). Nothing has run with live prospects yet.

## Calls: our next growth line

Admissions are still won on the phone. Our CRM holds **208,751 call records; 621 hours are calls longer than
30 seconds**, and none are transcribed yet. Calls open three lines of work (all **plan**, costs estimated):

1. **Transcription** with Amazon Transcribe (~US$894 at list for the 621 h) or NVIDIA Parakeet / Canary on one
   L40S (~20 GPU-hours, ~US$60), after a 50-call test of Mexican Spanish phone audio.
2. **Call analytics for admissions coaching**: what the best advisors say, where calls lose the visit, measured with
   the same blind-exam method we use for chat.
3. **Voice agents that book campus visits.** We already build admissions voice agents with Dapta (one goal per call:
   a visit with date and time). Next: put Constanza's harness and fact sheets behind them.

Details: [Impact](docs/en/impact.md) and [Roadmap](docs/en/roadmap-and-credits-ask.md).

## The proof: our own admissions AI on NVIDIA + AWS

We trained our own admissions assistant on **NVIDIA Nemotron 3.5 Lightning (30B-A3B)** using **Amazon SageMaker**
GPUs (L40S) and the **NVIDIA NeMo AutoModel** container, on an anonymized corpus of **1.3 million chat messages from
86 thousand prospective students across 80 schools**. Our third fine-tuning run (teacher-student distillation) took
Nemotron from **6.5 to 7.2 / 10** answering instantly (judge 1), with **0% price quotes** and roughly **1/20 of the
teacher's tokens**.

**Headline results** (blind exam, 300 real held-out turns, 6 schools never seen in training)

| System | Score (judge: Opus 5.5) | Offers the campus visit | Quotes a price | Invents a fact | Preferred over today's AI |
|---|---|---|---|---|---|
| Admissions AI schools use today (CRM built-in, GPT-4.1) | 4.2 | 32% | 8% | 34% | — |
| Nemotron, untrained + our harness (no reasoning) | 5.8 | 70% | 0% | 22% | 69% |
| **Nemotron fine-tuned by SuperLeads (v5) + harness, reasoning** | **6.7** | **92%** | **0%** | **5%** | **77%** |
| Claude Sonnet 5.5 + harness (Bedrock) | 7.8 | 94% | 0% | 2% | 93% |
| Claude Fable 5.2 + harness (Bedrock) | **8.2** | 94% | 0% | 2% | **96%** |

Judges are Anthropic models and may favour Anthropic candidates; no human has rated these yet. Full methodology and
caveats: [Training and evaluation](docs/en/training-and-evaluation.md).

## Contents

| Document | What it covers |
|---|---|
| [Impact](docs/en/impact.md) | Who we are, what the data says about admissions in LATAM, Constanza, calls |
| [Roadmap and credits ask](docs/en/roadmap-and-credits-ask.md) | How we grow with AWS: experiments, pilot, calls, estimated cost |
| [Architecture](docs/en/architecture.md) | System diagram: Constanza, data pipeline, training stack, products |
| [Training and evaluation](docs/en/training-and-evaluation.md) | Datasets, pipeline, five attempts, results, lessons |
| [AWS usage](docs/en/aws-usage.md) | Every AWS service we use, how, and the real numbers we could verify |
| [NVIDIA](docs/en/nvidia.md) | Nemotron, NeMo AutoModel, NVFP4, L40S/H100, and how we use Inception |
| [Changelog](docs/en/changelog.md) | Weekly log of new work |

## Numbers at a glance (verified from our own logs, Nov 2026)

| Item | Value |
|---|---|
| SageMaker GPU jobs (us-east-1) | 29 jobs, 24.8 billable instance-hours, **$299.51** (estimated at list price; internal cap $300) |
| Bedrock calls for the evaluation harness | 11,932 calls, 52.9M input / 3.8M output tokens |
| Bedrock use by Constanza (since Oct 2026) | ~14,400 model calls, ~$990 at list price (agent's own estimate) |
| Training corpus in S3 | 1 bucket, 1,432 objects, ~193 GB (anonymized data, checkpoints, outputs) |
| Always-on server | 1× EC2 m7i.2xlarge, mx-central-1 (Mexico) |
| Scheduled routines | 67 defined, 50 enabled |

How each number was obtained, and what we could **not** verify (Cost Explorer is not readable from the server role),
is in [AWS usage](docs/en/aws-usage.md).

## Principles

- **Privacy first.** Every record leaving the CRM is anonymized inside a Cloudflare Worker before it reaches AWS.
  Names, phones, emails, IDs and addresses are replaced; IDs become salted HMAC pseudonyms.
- **Rules in code, not in hope.** Our harness blocks price and scholarship quotes with a code-level guard; no version
  of our model ever sends one.
- **Honest numbers.** Estimates are marked as estimates. Failed experiments are reported next to the successful one.
- **License-aware training.** No output of an Anthropic model is used as training data for Nemotron.

## Contact

SuperLeads commercial team, [superleads.mx](https://superleads.mx).
