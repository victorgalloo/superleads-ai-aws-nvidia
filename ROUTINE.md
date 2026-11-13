# Weekly update routine

How the operations agent keeps this showcase current. Run once a week (suggested: Friday 17:00, Mexico City time).
Everything here is read-only on AWS: **never launch GPU, SageMaker or Bedrock jobs from this routine.**

Helper script on the server: `~/.hermes/scripts/showcase_aws_nvidia.py` (not part of this repo).

## Steps

1. **Collect** the week's raw facts (private report, never committed):
   ```bash
   python3 ~/.hermes/scripts/showcase_aws_nvidia.py collect --days 7
   ```
   It writes `~/.hermes/cache/scratch/showcase_semana.md` with:
   - commits per repository in the last 7 days (subjects are private: summarize by theme, never paste them);
   - scheduled routines defined/enabled, and which were added or removed since the last run;
   - Cost Explorer by service (if the role ever gets permission; today it is AccessDenied);
   - SageMaker total from per-job billable seconds;
   - Bedrock calls/tokens of the research harness and the operations agent for the period;
   - S3 object count and size of the project bucket.

2. **Write the changelog in both languages.** Add a section `## Week of D–D Mon YYYY` at the top of
   `docs/en/changelog.md` and the same content as `## Semana del D al D de mes de YYYY` in `docs/es/changelog.md`.
   - 3 to 8 bullets, grouped as *Admissions AI (NVIDIA + AWS)*, *Operations agent*, *Products*.
   - Only aggregated numbers. Mark estimates as estimates. If nothing happened on a track, say so in one line.
   - No client, school or person names (except the author's), no conversation text, no internal URLs, no IDs.

3. **Refresh numbers** that changed in `README.md`, `README.es.md`, `docs/*/aws-usage.md`,
   `docs/*/training-and-evaluation.md` (new exam rows) and `docs/*/roadmap-and-credits-ask.md` (experiments done move
   to the changelog with their real cost). Keep English and Spanish in sync.

4. **Sanitize:**
   ```bash
   python3 ~/.hermes/scripts/showcase_aws_nvidia.py check
   ```
   It flags access keys, `sk-` keys, GitHub/Slack tokens, credential assignments, any at-sign, numbers of 10+ digits,
   12-digit numbers, 32-hex IDs, phone numbers, URLs outside the public domains, the live AWS account ID and a private
   blocklist of names. Fix every hit; do not publish with hits.

5. **Publish:**
   ```bash
   python3 ~/.hermes/scripts/showcase_aws_nvidia.py publish "Weekly update YYYY-MM-DD"
   ```
   It re-runs the check, commits and pushes with the GitHub CLI credential helper. Never `--force`.

6. **Report** to the founder in one short message: what was added, the commit hash, and anything that could not be
   verified.

## Rules

- Honest numbers only; if a source is not readable, say so instead of estimating silently.
- Failed experiments are reported too.
- The private blocklist and the collection report live outside the repo.
