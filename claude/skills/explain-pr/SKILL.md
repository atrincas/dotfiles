---
name: explain-pr
description: Explain a pull request in plain English for a reader who does not know the project. Gives background first, then the core change, then what a reviewer must check, in a fixed six-section template written in ASD-STE100 Simplified Technical English. Use when the user asks what a PR does, asks to explain or simplify a PR description, says a description is too dense or too AI-generated, or gives a GitHub PR URL or number and wants simpler wording.
---

# Explain a PR in plain English

Explain a pull request to a reader who does not know the project.

Only explain it. Do not rewrite the description. Do not change code. Do not add a comment to the PR.

## Gather

Find the target PR. Use the number or the URL from the user. If the user gives neither, use the PR for the current branch.

Get the metadata and the file statistics:

```bash
gh pr view <n> --repo <owner/repo> --json title,body,author,state,additions,deletions,changedFiles
gh api "repos/<owner>/<repo>/pulls/<n>/files?per_page=100" \
  --jq '.[] | "\(.additions)\t\(.deletions)\t\(.status)\t\(.filename)"' | sort -rn
```

The second command gives the added lines, the deleted lines, and the status for each file. Sort the files by size, because the largest file usually explains an unusual total. Do not use `gh pr diff --stat`, because that flag does not exist.

If the PR contains more than 100 files, add `&page=2` to the second command, and continue until the result is empty.

Read the statistics and the file names. Do not read the full diff. If the user wants a check of the code itself, tell them to use `/code-review`.

If `gh` shows an `x509` error or a TLS certificate error, the sandbox is the cause. Do the same command again with the sandbox off.

## Compare the description with the files

Compare the text of the description with the file statistics. Report these problems in the last section. Do not ignore them:

- The line count is much larger than the work in the description. Generated files or vendored files are the usual cause. Name the files, and give the line count for the code alone.
- A shared file changes, but the description says that other code is safe. A tag or an ID keeps data rows apart. It does not keep shared code apart.
- The PR changes files that the description does not mention. Look for a second feature that the title does not name.
- The description names files that the PR does not change.

If you find no problem, say so in one sentence. Do not invent a problem.

## Use these six sections

Keep these headings and this sequence. Write 400 to 600 words in total.

1. **What this is about** — the background. Give the reader the domain knowledge that the description assumes. Keep it under 60 words.
2. **What problem this solves** — the situation before the PR, and the reason for the work.
3. **The main idea** — the one central change. Draw the before and after shapes as two lines of ASCII art if the PR changes a pipeline, a data flow, or a call path.
4. **What is in it** — the other parts, in order of importance. Omit small details.
5. **What it does not change** — the parts of the system that stay the same, and the author's reason why the change is safe. Name a shared file here if the PR changes one.
6. **What to check in review** — the files a reviewer must open, and each problem from the comparison above.

## Write in Simplified Technical English

Obey these rules from ASD-STE100:

- Write a maximum of 25 words in a sentence.
- Write a maximum of 6 sentences in a paragraph. Write about one topic in each paragraph.
- Use the simple present, the simple past, or the simple future tense. Do not use the perfect tenses. Write "the team removed it", not "the team has removed it".
- Use the active voice. Use the passive voice only if the actor is not known or is not important.
- Use an `-ing` word only as part of a technical name. Write "the step reads the file", not "reading the file, the step ...".
- Use a maximum of three nouns together. Write "the mapping of survey questions to indicators", not "the survey question indicator mapping".
- Use the same word for the same thing every time. Do not use a synonym for variety.
- Keep the articles "a", "an", and "the". Do not remove words to make a sentence short.
- Give each word one meaning.

The approved dictionary is part of the paid standard, and you do not have it. Apply the writing rules, and select the most usual and most simple word. Do not claim full dictionary compliance.

## Do not use these

- Filler words: harmonise, leverage, robust, seamless, comprehensive, streamline, facilitate, utilise, holistic, delve.
- Filler phrases: "it's worth noting", "in essence", "at its core".
- Idioms and phrasal verbs: "throw away", "roll out", "kick off". Write "delete", "release", "start".
- Emoji.
- A heading that is only a file name.
- The `**bold label**: text` bullet list. This is the format that you translate away from.
- The sentences of the author. Use your own words.
- Praise for the PR, the author, or the approach.

## Always do these

- Explain each technical term the first time that you use it: "the `CF*` columns (the personal details from the consent form)".
- Give the reader the result of a change, not only the change. Write "no personal data goes into the database", not "the step removes PII".
- Show the decisions that the team did not make. Commented-out code, TODO notes, and open questions are important.
- Give each safety claim to the author. Write "the author says that Wave 1 is safe", not "Wave 1 is safe".

## Worked example

See [EXAMPLES.md](EXAMPLES.md) for a dense description and the explanation that it must produce.
