# Releasing a promptctl plugin

Every Claude Code plugin repo in this org releases the same way. The implementation
is `.github/workflows/release-plugin.yml` here; each repo's `.github/workflows/release.yml`
only calls it.

## The procedure

1. In a PR, bump `version` in `<plugin>/.claude-plugin/plugin.json` and add a section
   to `<plugin>/CHANGELOG.md`:

   ```markdown
   ## v0.26.0 - 2026-09-10

   - what changed, one bullet per change
   ```

2. Merge the PR.

3. On an up-to-date master, tag and push:

   ```sh
   claude plugin tag <plugin-dir> --push
   ```

   This creates `<name>--v<version>` after checking that the manifest and the
   repo's `.claude-plugin/marketplace.json` entry agree. Use `--dry-run` to see the tag first.

4. The pushed tag runs the workflow, which checks that the tag, the manifest, and
   the changelog agree, then publishes the changelog section as the release notes.

## What the workflow refuses

- A tag that is not `<name>--v<version>`.
- A name that is not in `.claude-plugin/marketplace.json`.
- A version that does not match the plugin manifest.
- A changelog with no `## v<version> - <date>` section.

Each refusal names the disagreement in the run log. Nothing is written to any branch,
and no tag is ever moved.
