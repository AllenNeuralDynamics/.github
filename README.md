# Github Workflow Templates

## Versioning

Reusable workflows in `.github/workflows/` are versioned with git tags following [semantic versioning](https://semver.org/).

- `vX.Y.Z` – immutable release tag.
- `vX` – floating major tag, always points at the latest `vX.Y.Z`.

### Consuming

```yml
jobs:
  test:
    # Recommended: floating major tag, receives non-breaking updates automatically
    uses: AllenNeuralDynamics/.github/.github/workflows/test.yml@v1

    # Fully pinned: never changes
    # uses: AllenNeuralDynamics/.github/.github/workflows/test.yml@v1.2.3
```

Do not reference `@main`; it may contain unreleased or breaking changes.

### Releasing

1. Merge changes to `main`.
2. Run the **Release reusable workflows** action (`.github/workflows/release-reusable-workflows.yml`) from the Actions tab and choose a bump type:
   - `patch` – bug fixes, no interface changes
   - `minor` – new optional inputs/outputs/secrets, new workflows, backward-compatible behavior changes
   - `major` – renamed/removed inputs, new required inputs or secrets, removed workflows, any behavior change consumers must adapt to
3. The workflow creates the `vX.Y.Z` tag, moves the `vX` tag, and publishes a GitHub Release with generated notes.

Use the `dry-run` input to preview the next version without tagging.

