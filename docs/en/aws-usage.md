# AWS usage

[Español](../es/aws-usage.md) · [Back to README](../../README.md)

## How these numbers were obtained

| Source | Status |
|---|---|
| Cost Explorer (`ce:GetCostAndUsage`) | **Not readable** from the server's instance role (AccessDenied). Credit consumption by service is therefore not shown here; it can be added from the billing console. |
| `ec2:DescribeInstances`, `s3:ListAllMyBuckets`, `sagemaker:ListTrainingJobs`, CloudWatch metrics | Not permitted for the server role (least-privilege policy). |
| `sagemaker:DescribeTrainingJob` per job | Permitted. Used for per-job instance type, status and **billable seconds**. |
| S3 object listing of the project bucket | Permitted. Used for object count and size. |
| Instance metadata (IMDS) | Used for the server's instance type and Region. |
| Per-call token logs written by our own code and by the agent | Used for Bedrock calls and tokens. |

Dollar amounts below are **estimates**: billable seconds or tokens × public list price. Actual charges (and credits
applied) come from AWS billing.

## Services and how we use them

| Service | Region | How we use it | Verified usage |
|---|---|---|---|
| **Amazon EC2** | mx-central-1 | One m7i.2xlarge (8 vCPU, 30 GiB, 193 GB root volume) runs the Hermes operations agent 24/7: WhatsApp gateway, scheduler, orchestrators for GPU jobs, extraction and analysis scripts | 1 instance, always on. ~US$294/month at us-east-1 list price (estimate; Mexico Region price may differ) |
| **Amazon Bedrock** | us-east-1 (cross-region and global inference profiles) | (a) Model behind the operations agent: Claude Opus 5.5. (b) Judges and reference candidates for the admissions-AI exam: Opus 4.7, Opus 5.5, Sonnet 5.5, Fable 5.1, Fable 5.2. (c) Research and message drafting in the prospect-research product | See tables below |
| **Amazon SageMaker** (training jobs) | us-east-1 | LoRA fine-tuning of Nemotron, merge of adapters, and batch inference with vLLM for exams, teacher generation and fact sheets. Managed spot where possible | 29 jobs, 24.8 billable h, $299.51 |
| **Amazon S3** | us-east-1 | Anonymized corpus (partitioned by pseudonymous account and outcome), fact sheets, SFT sets, LoRA checkpoints, merged models, job outputs | 1 bucket, 1,432 objects, ~193 GB |
| **Amazon ECR** | us-east-1 | Two images mirrored for SageMaker: vLLM 0.27.1 and NVIDIA NeMo AutoModel 26.08 (from NGC) | 2 repositories/tags |
| **IAM** | global | Least-privilege instance role for the server; separate role for SageMaker jobs | — |

## SageMaker detail (all jobs, us-east-1)

| Purpose | Jobs | Billable hours | Estimated cost |
|---|---|---|---|
| LoRA training (v3, v4, v5 and two pipeline tests) | 5 billed | 19.75 | $259.17 |
| Batch inference: exams | 11 billed | 3.79 | $31.22 |
| Batch inference: teacher answers (v5) | 1 billed | 0.46 | $6.05 |
| Batch inference: fact sheets by Nemotron | 1 billed | 0.11 | $1.44 |
| Merge LoRA into base (CPU) | 3 | 0.68 | $1.64 |
| Capacity-race twins and jobs stopped before starting | 8 | 0 | $0 |
| **Total** | **29** | **24.8** | **$299.51** |

By instance type: ml.g6e.12xlarge (4× NVIDIA L40S) 22.3 h / $292.85; ml.g6e.2xlarge (1× L40S) 1.8 h / $5.03;
ml.r5.8xlarge 0.68 h / $1.64; ml.p5.4xlarge (1× H100) requested 4 times, never received capacity, $0.

Spot was used on the large jobs. Every orchestrator checks the running total before launching and stops any job that
waits too long for capacity, so it cannot bill later.

## Bedrock detail: admissions-AI research (11–13 Nov 2026)

| Model | Calls | Input tokens | Output tokens | Role |
|---|---|---|---|---|
| Claude Opus 4.7 | 5,702 | 24.3M | 0.69M | Judge 1 |
| Claude Opus 5.5 | 3,955 | 16.3M | 1.84M | Judge 2 |
| Claude Sonnet 5.5 | 1,497 | 4.8M | 0.65M | Reference candidate; labelled 296 conversations |
| Claude Fable 5.1 | 449 | 5.3M | 0.34M | Fact sheets for analysis; reference candidate |
| Claude Fable 5.2 | 329 | 2.1M | 0.26M | Strongest reference candidate |
| **Total** | **11,932** | **52.9M** | **3.79M** | |

Estimated at list price, the Opus and Sonnet share is about US$265 (Fable excluded for lack of a confirmed price); the
judge-2 pass alone is about US$100.

## Bedrock detail: the operations agent

Sessions recorded in the agent's database since October 2026 (the agent moved from a laptop to EC2 on 3 Nov 2026 and
its history was merged).

| Month | Bedrock-billed sessions | Model calls | Estimated cost (agent's own accounting, list price) |
|---|---|---|---|
| Oct 2026 | 37 | 4,077 | $451 |
| Nov 2026 (to the 13th) | 491 | 10,293 | $539 |

Prompt caching does most of the work: ~2.0 billion cached input tokens read vs ~2.4 million uncached. In the same
period the scheduler logged ~7,300 runs that made no model call (script-only routines).

## What we could not verify

- Credits consumed and remaining, by service (needs Cost Explorer or the billing console).
- EC2 hours and data transfer since launch (needs `ec2:Describe*` or billing).
- Agent usage before October 2026 (not in the agent's database).
- Bedrock calls made by the edge products (they use their own keys and logs).
