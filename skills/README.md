# Skills

Canonical source for agent skills, shared across tools via symlinks so each
skill is written once.

```
skills/<name>/SKILL.md          canonical content (frontmatter: name, description)
.claude/skills/<name>           -> symlink to skills/<name>          (Claude Code)
.github/prompts/<name>.prompt.md -> symlink to skills/<name>/SKILL.md (Copilot Chat, invoked as /<name>)
```

## Adding a new skill

```sh
mkdir skills/<name>
$EDITOR skills/<name>/SKILL.md   # needs `name` and `description` frontmatter

ln -s ../../skills/<name> .claude/skills/<name>
ln -s ../../skills/<name>/SKILL.md .github/prompts/<name>.prompt.md
```

Edit `skills/<name>/SKILL.md` only — both tools read through the symlink.
