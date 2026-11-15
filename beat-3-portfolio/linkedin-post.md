I built a checklist tool that reviews my bug reports before I post them to public software projects, so I don't waste anyone's time with something sloppy. At first it was too picky and rejected a good report over a small detail, so I fixed it to focus on what matters without letting weak reports slip through.

On a real open source project, the people in charge are busy volunteers. If my first comment is vague or wrong, I waste their time and look like I didn't do the work. My tool catches that before anyone sees it.

When my tool got something wrong, my first thought was that the answer key must be wrong. But when I looked closer, I saw that my tool's rule was the problem, not the report. When it passed all 20 test reports, I was relieved more than anything. It showed the fix did what I wanted: it let the good report through without letting the bad ones sneak in.

That taught me not to trust my own work just because I built it. Now when something doesn't add up, I go check the details first, and I test my fix carefully so I don't create a new problem while fixing the old one.

https://github.com/skyler-hall/ai301-coursework/blob/main/beat-3-portfolio/technical-writeup.md
