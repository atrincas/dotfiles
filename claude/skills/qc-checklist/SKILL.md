---
name: qc-checklist
description: Write a QC checklist for the person who tests a change — usually a PM — from the current branch, a PR, or a Jira ticket. A short list in plain words: each point is one line — what to do in the app and what they should see. Use when the user asks for QC points, QC instructions, a QC list, a test plan for the tester or PM, "what should QA check", "points to check for the person doing the qc", or gives a ticket or PR and asks what to test by hand — even if the word "checklist" is not used.
---

# QC checklist

Write a list of manual checks for the person who does QC on a change. That person is usually a PM. They use the app in a browser and the CMS. They do not read code.

The output goes in chat only. Do not post it to Jira, to the PR, or anywhere else. Do not change code.

## Gather

Work out the target from the user's message: a PR number or URL, a Jira key, or nothing (then use the current branch).

**The change.** Find the base and read the diff, including uncommitted work:

```bash
gh pr view [<n>] --json number,title,body,state,baseRefName,headRefName,url
base=$(git merge-base origin/<baseRefName or default branch> HEAD)
git diff --stat "$base"
git diff "$base"
git log --oneline "$base"..HEAD
```

If there is no PR, the base is the default branch. If the change is already merged, use the commit or the merge commit the user names. Read the full diff, not only the stats: the checklist quotes the real button labels, headings, error messages, and URLs, and those are only in the source.

If `gh` fails with an `x509` or TLS error, run it again with the sandbox off.

**The ticket.** Look for a Jira key (`ABC-123`) in the user's message, the branch name, the PR title, and the commit messages. If you find one, read it with `getJiraIssue`, including its comments. The description and acceptance criteria say what the change is supposed to do; the diff says what it does. The checklist covers both, and the gap between them is worth a line.

**The decisions.** Read any ADR the diff adds or changes, and the decisions or trade-offs in the PR body. These become "Known limits".

## What to check

Go through the diff and ask, for each change: what can the tester do in the app that shows whether this works? Consider these, and keep only the points that matter most:

- **Happy path first.** The main flow from start to end, in the order a user meets it.
- **Edge and failure cases.** Empty fields, invalid input, used or expired links, a direct visit to a page out of flow, a signed-out user, a user without permission.
- **Existing data.** If a change can break records that existed before, ask the tester to try an old account or an old record.
- **Other pages the change reaches.** If the diff changes a shared component, a shared hook, or a shared style, name the other pages that use it and ask for a quick look.
- **Things that must not happen.** A leak (the page says which emails have an account), a second email, a crash, a sideways scroll on mobile.
- **Environment limits.** If something works only on staging or only on production, say so in the point. If a check sends real email or touches client data, tell them to ask the team first.

A change with no visible effect (a refactor, a type fix) gets no point of its own. Cover it with "still works" on the pages it reaches.

If the tester cannot check something without breaking the environment, leave it out.

## Write it

Use this shape:

```
QC for <ticket key>

**<Group>**
1. <Do this> → <you see this>.
2. ...

**<Next group>**
3. ...

**Known limits**
- <limit the team chose>
```

Rules for the points:

- **One line each.** One action, an arrow, one result. Aim for under 20 words. If a point needs two sentences, it is two points or it is too detailed.
- **The tester's words, not the developer's.** Name pages and buttons the way the tester sees them: "the Contact page", not `/contact`. No function names, file paths, env vars, status codes, or PR numbers. Use a URL only when the page has no link to it.
- **Quote the real UI text** from the source, in quotes: "Send message", "Check your inbox". The tester matches it against the screen.
- **Mention the old behaviour only when it is the point of the change:** "the link works (it used to go nowhere)".
- **Number across all groups** (1 to N), so the tester can say "7 fails".
- **Short group names** that fit the change: "Main flow", "Errors", "Signed out", "Mobile", "Other pages". Two to four groups. Leave out a group with nothing in it.
- **5 to 12 points.** Merge points that check the same thing. Drop a point the tester would check anyway while doing another one.

**Known limits** lists, in plain words, what the team chose not to do and what differs from the design or the ticket. Three bullets at most, one line each. Leave it out if it is empty.

After the checklist, outside it, add one line for the user (not the tester) if something is needed before release — an env var, a migration, a manual CMS step — or if a ticket criterion has no change in the diff. Leave it out if there is nothing.

## Example

A made-up change: a "Download map" button that exports the current map view as a PNG.

```
QC for MAP-214

**Main flow**
1. On the Explore page, turn on two layers and click "Download map" → a PNG downloads.
2. Open the PNG → same area, same layers, legend in the corner.
3. While it downloads → the button says "Preparing…" and cannot be clicked again.

**Edge cases**
4. Turn off all layers and download → only the base map, no legend.
5. Zoom out to the whole world and download → the image is not cut off or blank.
6. Sign out and try again → it works the same.

**Other pages**
7. Open any project page → the map zoom still works, and there is no download button.
8. On a phone → the button does not cover the zoom buttons.

**Known limits**
- The image is always the same size, whatever the screen.
- Satellite maps export as the street map instead.
```

Before release: set `MAP_EXPORT_ENABLED=true` on production, or the button does not show.
