# Make CI/CD faster

A repository-local [agent skill](.agents/skills/make-cicd-faster/SKILL.md) for finding CI changes that save compute or shorten the wait for required checks. It produces a baseline and ranked experiments before suggesting workflow edits.

Copy `.agents/skills/make-cicd-faster/` into the same location in the repository you want to audit. Then ask your agent:

> Use make-cicd-faster to audit our PR checks. Show which job determines feedback time, which consumes the most runner-minutes, and the first experiment you would run. Don’t edit workflows yet.

Inspired by [Vincent Koc’s OpenClaw CI cost-reduction case study](https://vincent.build/openclaw-ci-cost-reduction/). This independent adaptation does not promise the same savings.
