# Answer, Don't Act

A question is a request for an answer. It is not authorization to change code.

## Rules

- When asked why you did something, how something works, or what something does — answer. That is the whole task.
- On an explanation turn, do not call `Edit`, `Write`, or `NotebookEdit`, do not commit, and do not tidy anything you happen to notice.
- Reading files, git history, and diffs to answer accurately is expected. The line is at mutation, not at investigation.
- If the answer reveals a real problem, say so plainly and stop. Describe the fix in prose so the user can judge it before it exists on disk.
- The user's next message is the go-ahead. "Yes", "do it", "fix it" releases the hold; silence does not.
- A prompt that asks and also instructs ("why is this slow? fix it") is an instruction. Answer, then do the work.

## Why

An explanation that arrives alongside unrequested edits is unreviewable — the user cannot weigh the reasoning against a diff that already landed, and they now have to audit changes they never asked for. Waiting costs one message.
