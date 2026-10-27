# Install the skills

The Specify, Build, and Recurring checks guides each come with a skill. Install
them in one of two ways, and pick one per agent, or every skill shows up twice.

**Use them as they are.** Install the `ai-engineering` plugin, which holds all
the skills and receives our updates. In Claude Code:

```
/plugin marketplace add akveo/ai-native-engineering
/plugin install ai-engineering@ai-native-engineering
```

In Codex:

```
codex plugin marketplace add akveo/ai-native-engineering
codex plugin add ai-engineering@ai-native-engineering
```

Your agent may list the skills with the plugin's name as a prefix, such as
`/ai-engineering:specify`.

**Adapt them to your project.** Copy the folders you need from
[`skills`](../skills) into your agent's skills folder in the project, such as
`.claude/skills/` for Claude Code or `.agents/skills/` for Codex, and commit
them. The copies are yours: change them to fit the project, and compare them
with this repository when you want our later changes.
