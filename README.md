# Agent skills

A small, portable set of agent skills that work with both Claude Code and
GitHub Copilot Chat. See `skills/README.md` for how the layout works and how
to add a skill.

```
skills/                          canonical skill content (edit here)
.claude/skills/<name>            symlink -> skills/<name>            (Claude Code)
.github/prompts/<name>.prompt.md symlink -> skills/<name>/SKILL.md   (Copilot Chat, /<name>)
```

## Using in a new project

### Option A: copy in (simplest)

Copy all three directories into the root of the target project, merging with
anything already there:

```sh
cp -r skills .claude .github <path-to-new-project>/
```

- Claude Code picks up skills automatically from `.claude/skills/`.
- Copilot Chat (in VS Code) picks up prompt files from `.github/prompts/`
  and lets you invoke them with `/<name>`.

Because the symlinks are relative (`../../skills/<name>`), they still resolve
correctly as long as `skills/`, `.claude/skills/`, and `.github/prompts/`
stay siblings at the project root.

If the target project already has files under `.claude/skills/` or
`.github/prompts/`, copy `skills/` first, then add the two symlinks by hand
per `skills/README.md` instead of overwriting existing content.

### Option B: git submodule (multiple projects, one source of truth)

If you want changes to propagate to every project instead of copy-pasting:

```sh
git submodule add <this-repo-url> vendor/agent-skills
ln -s vendor/agent-skills/skills skills
ln -s vendor/agent-skills/.claude/skills .claude/skills
ln -s vendor/agent-skills/.github/prompts .github/prompts
```

Update all projects at once by bumping the submodule and running
`git submodule update --remote`.
