# Mandala Pets

The community pet catalog for [Mandala](https://github.com/MaktubCN/open-mana), a local-first AI workbench. Pet packages replace the figure in Mandala's side panel and desktop pet window with images for execution states.

The package specification is [Mandala's Pets documentation](https://github.com/MaktubCN/open-mana/blob/main/docs/market-ui-extensions.md#pets). This catalog uses `format: "mandala-catalog"`, `version: 1`, `kind: "pet"`, and an `items` array. It starts empty.

## Submit a pet

1. Create a `.mandala-pet.json` file following the specification.
2. Validate it with Mandala's parser and import it in Mandala settings to preview every state.
3. Open a pull request adding the package under `packages/`, with its license and validation results. See [CONTRIBUTING.md](CONTRIBUTING.md).
4. Maintainers review the package before merging. A separate catalog change pins the merged package to its existing 40-character commit and SHA-256 checksum. A package-file PR alone does not list it in Market.

## Review rules

Packages are data only, limited to 4 MiB, and must provide `idle`, `working`, `waiting`, and `error` images. Review covers the actual parser, all states, legibility at 22 px and 84 px, redistribution rights, and the absence of executable content or external resources. Do not include personal data, credentials, confidential material, or real-user screenshots.

Documentation, PR descriptions, commit messages, and catalog descriptions use English. Validation runs locally; this repository does not require automatic cloud CI/CD.

Before launch, this repository is private. Market's anonymous downloads require both the catalog and pinned packages to be public. The linked Mandala source documentation also requires access until Mandala is open-sourced.
