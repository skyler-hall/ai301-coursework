# Rubric: is this a good first issue? yes, yes it is

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo not archived | Repo facts: "archived:" flag on the repo line | archived: no | required |
| Repo actively maintained | Repo facts: "last push to any branch" date | last push is within 90 days of the capture date (live mode: within 90 days of today) | required |
| Issue not already claimed | Repo facts: "this issue: assignees" and "linked PRs"; the Comments section | assignees is none, no linked PR is open, and no comment says "I'll take this" / "working on this" / "can I work on this" that was not later shown as abandoned or reopened by a maintainer | required |
| Scope fits a newcomer | Issue body and comment thread | the issue describes one bounded piece of work: it is not explicitly described as a tracking or umbrella issue meant to be split into separate issues or PRs, it is not a pure usage question, and no maintainer comment says the fix needs core internal changes with the design still unresolved. A single issue with a checklist of related edits within one feature area still counts as bounded. | required |
| Maintainer responds to issues | Repo facts: "maintainer first-response sample" | at least one issue in the sample shows an owner/member/collaborator comment within 30 days | preferred |
| Recent release | Repo facts: "latest release" date | latest release is within 12 months of the capture date | preferred |
| Beginner-friendly label | Issue labels | issue carries a "good first issue" label or clear equivalent | preferred |

## Verdict rule

Accept only if every required check passes: repo not archived, repo actively maintained, 
issue not already claimed, scope fits a newcomer, and the AI contribution policy allows this workflow. 
A required check graded `unclear` counts as fail. Preferred checks never change the verdict; they only 
rank issues the rubric already accepted, ranking passes ahead of fails.
