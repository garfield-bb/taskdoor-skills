# TaskDoor skills

Agent skills for TaskDoor. Each skill is an independent directory under `skills/`,
with its own `SKILL.md` and the references it needs.

## Install

    npx skills add nocoly/taskdoor-skills -g -y

The `taskdoor` CLI skill expects the TaskDoor CLI ([`@taskdoor/cli`](https://www.npmjs.com/package/@taskdoor/cli), source at
[nocoly/taskdoor-cli](https://github.com/nocoly/taskdoor-cli)):

    npm install -g @taskdoor/cli
    taskdoor login

For a private or sandbox deployment, add `--server <origin>` to `taskdoor login`.

## Skills

- [`taskdoor`](skills/taskdoor/SKILL.md): set up the CLI, find commands with `taskdoor --help`, read exit
  codes, and follow up on unresolved discussion items with lightweight subtasks when appropriate.
- [`taskdoor-mcp`](skills/taskdoor-mcp/SKILL.md): connect and use remote TaskDoor MCP, sign in through
  supported browser authorization or an access token, and follow up on discussion items without
  requiring the CLI. Preserve existing servers and verify the connection with a read-only operation.
- [`taskdoor-create-task`](skills/taskdoor-create-task/SKILL.md): turn a user's request into tasks and
  subtasks as an editable planning proposal, recommending owners from the supplied team context. It
  outputs a proposal only; the application handles confirmation and creation.

The creation skill includes task goals, completion criteria, due dates, execution tips and effort
estimates. It asks at most three multiple-choice questions when a key detail is missing, points out
existing tasks that look like duplicates or could be the parent, and writes user-facing text in the
requested language (Chinese or English). It preserves explicit user choices and never returns a
fabricated creation receipt. It contains no credentials, fixed workspace or team, or evaluation fixtures.

The CLI and MCP skills both teach the agent to notice concrete work that deserves follow-up during
ordinary reading, discussion and file handling. They check existing tasks and the latest discussion,
suggest a title and an evidenced owner, and create only when authorized. A small follow-up needs no
mandatory goal, criteria, date, estimate or planning flow. Existing work, resolved questions and ignored
suggestions do not prompt repeated creation; connecting a client does not start background monitoring
or synchronize page suggestions.

## Adding skills

Add `skills/<skill-name>/SKILL.md` and only the supporting files needed for that skill. Keep references
inside its directory so it can be installed on its own. Add the skill to the list above; no router or
shared runtime registry is required. Do not place evaluation reports or historical design protocols in
the installable directory.

Run the dependency-free packaging check before publishing:

    node scripts/check-skills.mjs
