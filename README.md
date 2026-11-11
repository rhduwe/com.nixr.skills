# Bespoke Agent Skills

This repository stores reusable instructions and supporting files for agent workflows.

## Add a skill

1. Create a directory under `skills/` with a short, lowercase, hyphenated name.
2. Add a `SKILL.md` file. Match the `name` to the directory name and use the `description` to explain when an agent should use the skill.
3. Write the workflow below the frontmatter. Keep any scripts, references, or assets the skill uses in the same directory and link to them with relative paths.
4. Try the skill on a representative request before committing it.

For example, `skills/check-release-notes/SKILL.md` could start with the following frontmatter.

```markdown
---
name: check-release-notes
description: Review draft release notes against the changes included in a release.
---

# Check release notes

Compare the draft with the commits in the release. Flag user-facing changes
that are missing, and cite the source for each proposed correction.
```

## Use a skill

Copy or link the entire skill directory into a location your agent scans for skills. Keeping the directory intact preserves relative links to its supporting files.
