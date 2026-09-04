# PR Intelligence Automation

This repository includes `.github/workflows/pr-intelligence.yml` to provide:

- PR complexity scoring and risk hints
- Mermaid impact flowchart auto-inserted into the PR description
- AI-based review suggestions (free-first model rotation)
- Dependency vulnerability checks (OSV Scanner + OWASP + GitHub dependency review)

## What the workflow does

1. Calculates a **complexity score** (0–100) based on changed files, LOC delta, affected modules, and build-file changes.
2. Writes/updates a PR description block between:
   - `<!-- pr-intel:start -->`
   - `<!-- pr-intel:end -->`
3. Generates a **Mermaid flowchart** showing which of the three modules (`shared-kernel`, `core-module`, `app`) are impacted — changed nodes are highlighted in orange.
4. Posts an **AI review comment** (updated in-place on subsequent pushes) between:
   - `<!-- pr-intel-ai:start -->`
   - `<!-- pr-intel-ai:end -->`
5. Runs dependency scans with:
   - `actions/dependency-review-action` (fails on HIGH severity)
   - `google/osv-scanner-action`
   - `./gradlew dependencyCheckAnalyze` (OWASP NVD)

## Free-first model rotation

By default, the AI step tries models in this order:

1. `gemini:gemini-2.0-flash-lite`
2. `openrouter:meta-llama/llama-3.1-8b-instruct:free`
3. `openrouter:google/gemma-2-9b-it:free`
4. `groq:llama-3.1-8b-instant`

Paid models are hard-blocked unless the repository variable `ALLOW_PAID_MODELS=true` is set.

When enabled, additional paid candidates are appended:

1. `anthropic:claude-3-5-haiku-latest`
2. `openai:gpt-4o-mini`

Override with the repository variable `AI_REVIEW_MODEL_ORDER` (comma-separated):

```text
openrouter:google/gemma-2-9b-it:free,groq:llama-3.1-8b-instant
```

## Secrets / variables

Set these only for the providers you want enabled:

| Name | Type | Purpose |
|------|------|---------|
| `GEMINI_API_KEY` | Secret | Google Gemini (free tier available) |
| `OPENROUTER_API_KEY` | Secret | OpenRouter free models |
| `GROQ_API_KEY` | Secret | Groq free tier |
| `ANTHROPIC_API_KEY` | Secret | Claude (paid; requires `ALLOW_PAID_MODELS=true`) |
| `OPENAI_API_KEY` | Secret | OpenAI (paid; requires `ALLOW_PAID_MODELS=true`) |
| `AI_REVIEW_MODEL_ORDER` | Variable | Optional override of the model rotation order |
| `ALLOW_PAID_MODELS` | Variable | Set to `true` to allow paid providers |

If no provider key is set, the workflow falls back to a **deterministic heuristic review** and still posts a comment on the PR.

## Complexity scoring

| Metric | Points |
|--------|--------|
| Each changed file | +2 |
| Each 10 LOC changed | +1 |
| Each affected module | +10 |
| Build files changed | +20 |
| More than 20 files changed | +15 |
| More than 500 LOC changed | +10 |

| Score | Risk |
|-------|------|
| 0–39 | 🟢 Low |
| 40–69 | 🟡 Medium |
| 70–100 | 🔴 High |

## Notes

- `dependency-review-action` fails on HIGH severity and above.
- The workflow updates a single AI review comment per PR using hidden markers to avoid comment spam.
- For private repositories, AI provider free-tier quotas still apply per provider account.
- The OWASP dependency check requires the NVD database to download on first run (may be slow); subsequent runs use the Gradle cache.
