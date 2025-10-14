# Intercom Fork - Branch Strategy

This is an Intercom fork of [open-telemetry/opentelemetry-ruby-contrib](https://github.com/open-telemetry/opentelemetry-ruby-contrib).

## Branch Model

We maintain two main branches:

### `main` - Upstream Mirror
- **Purpose**: Clean mirror of the upstream `open-telemetry/opentelemetry-ruby-contrib` main branch
- **Updates**: Automatically synced weekly via GitHub Actions ([workflow](https://github.com/intercom/intercom/blob/master/.github/workflows/sync-org-forks.yml))
- **Policy**: Never commit directly to this branch. It should always fast-forward to match upstream.

### `fork-main` - Active Development Branch
- **Purpose**: Contains Intercom-specific changes and contributions pending upstream acceptance
- **Usage**: This is the branch referenced in the Intercom monolith's Gemfile
- **Updates**: Periodically merge `main` into `fork-main` to pull in upstream changes

## Workflow

### Adding New Features or Fixes

1. **Create PR to upstream**: Submit your changes to `open-telemetry/opentelemetry-ruby-contrib`
2. **Fast-track in fork**: Add the same changes to `fork-main` so Intercom can use them immediately
3. **When upstream merges**:
   - The weekly sync automatically pulls changes into `main`
   - Merge `main` into `fork-main` to reconcile
   - Your commit from step 2 stays in history, but the upstream version becomes canonical

### Syncing with Upstream

```bash
# Fetch latest from both branches
git fetch origin main fork-main

# Merge upstream changes into fork-main
git checkout fork-main
git merge origin/main

# Push the merged changes
git push origin fork-main
```

### When Upstream Accepts Your PR

Once your PR is merged upstream and synced to `main`, the changes exist in both branches. You can optionally:
- Keep the merge commit history as-is (simplest)
- Rebase `fork-main` to remove redundant commits (advanced, requires force-push)

## Why This Model?

This "fast-track" pattern allows us to:
- ✅ Use features immediately without waiting for upstream review cycles
- ✅ Maintain automated weekly syncs (no divergence issues)
- ✅ Contribute back to open source (all changes are intended for upstream)
- ✅ Keep clear separation between upstream mirror and active development

## Questions?

For questions about this fork or the branch strategy, ask in #builder-tools or consult the [sync workflow documentation](https://github.com/intercom/intercom/blob/master/.github/workflows/sync-org-forks.md).
