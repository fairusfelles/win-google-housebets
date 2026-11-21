# Xingchi fallback probe — 18 Nov 2026

Purpose: test option B for the SEO v2 live AI answers (Q2–Q8) — run the questions
through Xingchi gpt-6-astra via OpenCode — after the free Gemini API stayed
quota-exhausted (429 RESOURCE_EXHAUSTED, last probe 18 Nov 2026 ~02:07 UTC,
`q2.probe18.err` in the 2026-11-15 evidence folder).

## What was run

- Reinstalled OpenCode (global npm installs are ephemeral here):
  `npm i -g opencode-ai@latest` → OpenCode 1.19.6.
- Command (from /tmp/xingchi-probe, 18 Nov 2026 ~04:36 UTC / 11:36 WIB):
  `timeout 150 opencode run -m xingchi/gpt-6-astra "Answer this question directly in plain text, without using any tools: is Housebets legit"`
  with stdout → `q2.txt`, stderr → `q2.err` in this folder.

## Result

- Exit code **124** — killed by the 150 s timeout.
- `q2.txt`: **0 bytes**. `q2.err`: **0 bytes**. No output, no error, no
  partial answer.
- Conclusion: the Xingchi-via-OpenCode route hung exactly as it did the
  previous evening (150 s timeout, 17 Nov). It is not a usable fallback for
  the Q2–Q8 live sessions today.

## Consequence

SEO v2 ships with Q1 only as a live direct session (Gemini, 15 Nov 2026,
`../ai_live_2026-11-15/q1.txt`); Q2–Q8 rest on the labelled 13 Nov
source-pool samples in `ai-answers.csv`. See `ai-live.csv` and the §5
addendum in `report.md`.
