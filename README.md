# Garuda demo: a Salesforce repo with a review agent that remembers

This is a small Salesforce DX project with [Garuda](https://github.com/dlrvrun-cpu/garuda) watching its pull requests.
Open the closed PRs to watch Garuda work:

1. **It sees.** Garuda builds its context from the diff, the full class, the tests, the callers and the repo rules, then posts inline comments.
2. **It learns.** Reply `/garuda dismiss <why>` on a comment. Garuda commits the lesson to `.garuda/memory/dismissed-findings.md` and adds a line to `.garuda/memory/episodes.jsonl`.
3. **It remembers.** The next PR gets the same rules from memory, and the dismissed finding stays quiet.
4. **It checks intent.** Each PR states its goal and the files it may touch in an `intent` block. Touch anything else and the `permit` check goes red.

## Garuda's memory

| File | Kind | What's in it |
|---|---|---|
| `.garuda/memory/repo-conventions.md` | semantic | Facts about this repo. Selectors own the queries, batch scope is 200. |
| `.garuda/memory/dismissed-findings.md` | episodic | Findings people said no to, with who and why |
| `.garuda/memory/episodes.jsonl` | episodic | Past failures, fixes and dismissals, one per line |
| `REVIEW_RULES.md` | rules | The team's review rules |

## Try it on your fork

1. Fork this repo.
2. Add an `ANTHROPIC_API_KEY` secret under Settings, Secrets and variables, Actions.
3. Open a PR. Start the description with an intent block:

````
```intent
schema: soi/v0
goal: Count lost opportunities on each Account
must_hold:
  - closeLostOpportunities stays bulk safe at 200 records
permit:
  paths:
    - force-app/main/default/classes/OpportunityService*.cls
```
````
