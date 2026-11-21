# AI live-session probe — 21 Nov 2026

Final retry round for Q2–Q8 before the 22 Nov 2026 contest close.
Result: **no new live answers obtained.** Q2–Q8 remain NOT OBTAINED,
as labelled in `ai-live.csv`. Nothing in this folder is an engine answer.

## Attempt 1 — Gemini (gemini-3.1-flash-lite), Q2 "is Housebets legit"

- Command form: prompt on stdin, `gemini -p "" -m gemini-3.1-flash-lite`
  (the same form that produced the genuine Q1 answer on 15 Nov 2026).
- Outcome: the API answered **429 RESOURCE_EXHAUSTED** (see `q2.err`);
  the CLI kept retrying silently and was stopped at 150 s.
  `q2.txt` is 0 bytes — no answer text was produced.
- Reading: the free Gemini API quota is still exhausted, as on the
  15, 16, 17 and 18 Nov retries.

## Attempt 2 — Xingchi gpt-6.1-sol via OpenCode

- `opencode run -m xingchi/gpt-6.1-sol` with a trivial test prompt.
- Outcome: no output within 100 s. The relay was likewise silent on
  18 Nov (gpt-6-astra, 150 s) and 19 Nov. Third consecutive failure;
  the relay is treated as unavailable for this entry.

## Conclusion

The AI-search section stands exactly as labelled: one genuine live
session (Q1, Gemini, 15 Nov 2026, in `evidence/ai_live_2026-11-15/`),
seven questions resting on the labelled 13 Nov 2026 source-pool
samples. No session is claimed that did not happen.
