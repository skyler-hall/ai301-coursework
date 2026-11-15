# Fixing a too-strict proof rule in my repro-check rubric

## Overview

repro-check is a grader for my reproduction package. Before I post on a real issue, I write two things: a claim comment saying I want to work on it, and a repro report showing I can make the bug happen. repro-check reads both, checks them against the actual issue, and tells me if they're ready to post or what's missing. I built the rubric for repro-check, the rules it uses to grade a claim and repro report, and on my first run it scored 19/20 because it rejected a good ripgrep report just for describing a control run in one sentence instead of pasting the output. So the fix was to make the rule smarter: the core bug claim must have pasted proof, but a simple, easy-to-check control can be described in words. The next full run hit 20/20.

## What happened

### Why I built it

For example, repro-check checks for clear steps, the expected vs. actual result, my environment, and proof that I really hit the bug. On a real open source project, maintainers are busy volunteers. If my first comment is vague or wrong, I waste their time and look like I didn't do the work. repro-check catches that before anyone sees it. It helps me show up with something solid on the first try, which builds trust and makes it more likely they'll let me take the issue.

### The first run missed a good report

My first full run (Sept 29, 2026) showed this:

```text
Full run: agreement 19/20 scored items (bar: 18/20: PASS).
pkg-03 disagreed: my rubric rejected it on claims-backed.
```

The first version of my claims-backed rule said:

> Pass if every claim is supported by an artifact in the package.

My first thought was, wait, is the answer key wrong? So I went back and read pkg-03 myself. The report was actually good. The main thing, the bug itself, had real pasted output proving it. The only thing not pasted was the control run, and that was one simple sentence: drop `-r '$1'` and you get the right result.

That's when I saw my rule was the problem, not the report. My rule treated every sentence the same, so a small side check needed the same proof as the bug itself. But those aren't equal. The bug is the main claim, so it needs pasted proof. The control run is a supporting detail that a maintainer can check in a few seconds by running the same command without the flag. Making people paste output for every little claim would punish good reports and push people to pad them with extra output nobody needs.

### The change

So the fix was to make the rule smarter: the core bug claim must have pasted proof, but a simple, easy-to-check control can be described in words. This is the sentence I added to the rule:

> A side observation that names its exact command (for example a control run described in one sentence, "dropping flag X gives the correct result") does not need its own pasted output, as long as the headline claim has one.

### Making sure the fix didn't go too far

When you loosen a rule, the risk is that bad reports start getting through. So before I spent a full run, I picked the packages most likely to break if my fix went too far.

pkg-13 and pkg-15 were the big ones. They made confident claims with no proof at all. pkg-15 even said "I verified this race condition" but showed nothing. If my new rule was too loose, those would pass, and that would be worse than the original problem. pkg-17 checked that garbled output still couldn't be passed off as a crash. pkg-20 was the only disclosure package, so I had to make sure that special case still worked. And calib-02 was a trap check, to make sure the grader wasn't just getting lucky.

Testing a few targeted ones first was faster and cheaper than a full run. If one failed, I'd know right away what broke, instead of digging through 20 results.

### The result

The confirming full run, saved in `eval-run.txt`:

```text
pkg-03  accept  accept   yes

categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

When it hit 20/20, I was relieved more than anything. It showed the fix did what I wanted: it let the good report through without letting the bad ones sneak in. That's when I trusted the change.

## Limitations and what's next

My fix lets a simple side check be described in one sentence instead of pasted. The problem is that the grader has to decide what counts as a side check. A sneaky or lazy report could describe its main bug in one sentence, like "running this command crashes it," and paste nothing. If the grader reads that as a side observation, the report passes with no real proof. So the gap is that I made the rule looser, but I didn't clearly define where the line is between the main claim and a side detail.

I'd close it in two ways. First, I'd make the rule stricter about what a headline claim is. The headline claim is whatever the issue is about: the bug, the crash, the wrong output. That always needs pasted proof, no matter how short it's written. Only a control or comparison run can be described in words, and only if there's already pasted proof of the bug itself.

Second, I'd build a new test package designed to sneak through that gap. For example, a report where the bug is only described in one sentence with zero pasted output. If repro-check passes it, I know the gap is real. If it fails it, I know the fix holds. Then I'd add it to the eval set so this never quietly breaks later.

One other lesson, not about the grader itself: on render-engine #609, I learned a good package doesn't matter if the issue isn't wanted anymore. My fix is still on my local branch. Next time, I'd check with the maintainer that the issue is still open and wanted before I build.
