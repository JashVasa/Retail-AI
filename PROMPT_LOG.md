# Prompt log (appendix)

## P1: Order extraction (api/parse.js, SYSTEM)
Purpose: messy Hinglish to JSON lines. Temperature 0. Message treated as data (prompt-injection guard).
Design choices: LLM extracts only; it must not map to a catalog (prevents made-up products); null quantity instead of guessing (prevents misread quantities); output validated and sanitised server-side.
Known failure modes to record here after running the in-app evaluation: (add examples of wrong units, merged items, wrong splitting).

## P2: Development prompts (used to build with Claude)
1. Project brief and demo requirements pasted as given.
2. "I need a good UI/UX, a working demo of what the shopkeeper sees" produced the chat interface.
3. Bug report with screenshot (old items appeared) led to merging of duplicate items, "naya" tags and "Naya order shuru karein".
4. "Give me a folder I can push to git" produced this repo.
Add dates and your own iterations before submission.
