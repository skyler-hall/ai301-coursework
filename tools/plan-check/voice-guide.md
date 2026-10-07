# Voice guide: how I talk upstream

## Who I am in threads

I'm Skyler, a CS senior at FIU and still new to open source. In Path
Review I'm here to reproduce one bug carefully and learn the codebase,
not to show off. Readers can expect short, plain comments that say
exactly what I ran and what I saw.

## Rules I write by

### Rule: Promise the work, not the fix

I only promise what I control: investigating and posting what I find.
No fix promises, no dates, no "guaranteed".

- Wrong: "I'll have this fixed by the weekend, guaranteed."
- Right: "Next I'll set up the repo from its README, try the steps in the issue, and post a repro report here."

### Rule: Name this issue, not any issue

Every comment names at least one detail that only this issue has, like
the error text, the file, or the function.

- Wrong: "Hi, I'd like to work on this. Please assign me."
- Right: "I'd like to work on the crash when a context chunk is None; I'll start by running the failing case from the issue."

### Rule: Say what I saw, not what I believe

If I didn't run it or show the output, I don't say it's confirmed. Guesses
get labeled as guesses.

- Wrong: "Confirmed, the root cause is the loop in the parser."
- Right: "I haven't reproduced it yet. My guess is the parser loop, but I'll check that after I run the steps."

### Rule: Tell the truth about AI help

When the repo's policy asks for it, I say plainly that I used an AI
assistant and for what. I never let AI words go out that I haven't
read, checked, and rewritten in my own voice.

- Wrong: (posting a polished comment an AI drafted without saying so on a repo that requires disclosure)
- Right: "I used Claude to help organize this report. I ran every step myself and checked every output."

### Rule: Plain words, no filler

No "Hello sir", no "amazing repository", no em dashes, no hype. Just the facts
in simple sentences.

- Wrong: "Hello sir! Great project, I love it, this looks amazing for me!"
- Right: "Hi, I'd like to take this one."

### Rule: Commit to the approach, label what I don't know

A plan comment says the one approach I chose and where in the code it
goes. Anything I haven't checked yet is called an open question, not
hidden and not stated as fact. If a maintainer already suggested a
direction, I say whether I'm following it.

- Wrong: "I'll look into a few ways to fix this and see what works."
- Right: "Plan: guard the empty case in `index()` and remove the xfail marker. Open question: whether `search()` should log the same warning after an empty index."

## Things I never post

- A fix promise, a deadline, or the word "guaranteed"
- "Same as above", "+1", or "can confirm" with no proof of my own
- "Confirmed" or "reproduced" before I have the output to show
- A root cause stated as fact without a trace or test behind it
- Words an AI wrote that I haven't read and rewritten myself
- Flattery or "kindly assign me" requests
- A plan that is just "same approach as above"
