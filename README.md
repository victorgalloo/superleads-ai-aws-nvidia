# SuperLeads AI on AWS + NVIDIA

**[Leer en español](README.es.md)**

SuperLeads builds CRM and admissions systems for private schools and universities in Mexico and Latin America.
This repository is a public showcase of what we are building with **AWS credits** and as a member of
**NVIDIA Inception**: architecture, flows, experiments, real usage numbers and what we want to do next.

It contains **documentation only**: no source code, no customer data, no credentials.

## In one paragraph

We trained and evaluated our own admissions assistant on **NVIDIA Nemotron 3.5 Lightning (30B-A3B)** using
**Amazon SageMaker** GPUs (L40S) and the **NVIDIA NeMo AutoModel** container, on an anonymized corpus of
**1.3 million chat messages from 86 thousand prospective students across 80 schools**. We built a blind evaluation
of 300 real, held-out prospect messages, judged by Claude Opus on **Amazon Bedrock**. Our third fine-tuning run
(teacher-student distillation) took Nemotron from **6.5 to 7.2 / 10** answering instantly (judge 1), with
**0% price quotes** and roughly **1/20 of the teacher's tokens**. The admissions AI that schools use today scores 4.2–4.8.
In parallel, a 24/7 operations agent (Hermes) on **Amazon EC2** runs 50 scheduled routines on **Claude Opus via Bedrock**.

## Headline results (blind exam, 300 real held-out turns, 6 schools never seen in training)

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
| [Architecture](docs/en/architecture.md) | System diagram: operations agent, data pipeline, training stack, products |
| [Training and evaluation](docs/en/training-and-evaluation.md) | **The showcase**: datasets, pipeline, five attempts, results, lessons |
| [AWS usage](docs/en/aws-usage.md) | Every AWS service we use, how, and the real numbers we could verify |
| [NVIDIA](docs/en/nvidia.md) | Nemotron, NeMo AutoModel, NVFP4, L40S/H100, and how we use Inception |
| [Impact](docs/en/impact.md) | Why this matters for education admissions in LATAM, with measured findings |
| [Roadmap and credits ask](docs/en/roadmap-and-credits-ask.md) | Concrete experiments, estimated cost, what more credits unlock |
| [Changelog](docs/en/changelog.md) | Weekly log of new work |

## Numbers at a glance (verified from our own logs, Nov 2026)

| Item | Value |
|---|---|
| SageMaker GPU jobs (us-east-1) | 29 jobs, 24.8 billable instance-hours, **$299.51** (estimated at list price; internal cap $300) |
| Bedrock calls for the evaluation harness | 11,932 calls, 52.9M input / 3.8M output tokens |
| Bedrock use by the operations agent (since Oct 2026) | ~14,400 model calls, ~$990 at list price (agent's own estimate) |
| Training corpus in S3 | 1 bucket, 1,432 objects, ~193 GB (anonymized data, checkpoints, outputs) |
| Always-on server | 1× EC2 m7i.2xlarge, mx-central-1 (Mexico) |
| Scheduled routines on the agent | 67 defined, 50 enabled |

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

Victor Jiménez Gallo, Enrollment Growth Executive, SuperLeads, [superleads.mx](https://superleads.mx).
