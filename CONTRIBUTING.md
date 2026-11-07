# Contributing pets

Use [Mandala's Pets specification](https://github.com/MaktubCN/open-mana/blob/main/docs/market-ui-extensions.md#pets) as the source of truth. Do not introduce package fields outside that format.

## Create and validate

Create a UTF-8 JSON file with `format: "mandala-pet"`, `version: 1`, `name`, `tint`, and `states`. Required states are `idle`, `working`, `waiting`, and `error`. Optional states are `unknown`, `thinking`, `done`, and `sleep`, with the fallbacks defined in the specification.

Each image must be an inline SVG, PNG, APNG, GIF, or WebP data URI. Animation is contained in the image. Scripts, event handlers, `javascript:`, `foreignObject`, and external references in SVG are rejected. The complete JSON file must not exceed 4 MiB. Use a square canvas and verify legibility at 22 px and 84 px. Check theme tinting when `tint` is enabled.

From a local Mandala checkout with its dependencies installed, validate the file:

```sh
PET_FILE="/absolute/path/to/example.mandala-pet.json" node --import tsx --input-type=module <<'JS'
import { readFileSync } from 'node:fs';
import { parseExtensionData } from './shared/ui-extensions.ts';
parseExtensionData(readFileSync(process.env.PET_FILE, 'utf8'), 'pet');
console.log('Pet package format is valid.');
JS
```

Import the file in Mandala settings and preview every state, including fallbacks. Record the App build and parser result. Parser validation does not establish visual quality or redistribution rights.

## Submit

Add the package under `packages/` in a branch and open a PR to `main`. Include a license permitting redistribution, the package ID, display name, English description, publisher, release version, minimum App version, and validation results. Leave `catalog.json` unchanged for a package-file PR; its final commit does not exist before merge. Use English for documentation, PR titles and descriptions, commit messages, and catalog descriptions.

Only submit artwork you may redistribute. Do not include personal data, credentials, confidential material, or real-user screenshots. Synthetic previews are sufficient.

## Maintainer publication

1. Review the package format, size, artwork rights, required states, optional fallbacks, and preview evidence. Require Code Owner review and at least one approval when branch rules are available.
2. Merge the package PR and select the actual existing commit containing the reviewed bytes. Squash and rebase merges can change the commit ID.
3. Calculate SHA-256 from the committed bytes, for example `git show COMMIT:packages/example.mandala-pet.json | shasum -a 256`.
4. Add an item using the shared catalog fields: `id`, `name`, `description`, `publisher`, `version`, `minAppVersion`, and `package`. The package reference contains `repository`, a full 40-character `revision`, relative JSON `path`, and `sha256`. Follow the [catalog protocol](https://github.com/MaktubCN/open-mana/blob/main/docs/market-ui-extensions.md#catalog-and-package-protocol) for field constraints. Never use a branch, tag, or future commit as the revision.
5. Validate the catalog with `parseExtensionCatalog(value, 'pet')` from Mandala's `shared/ui-extensions.ts`. Check the pinned package bytes against the checksum and `parseExtensionData(raw, 'pet')`. Keep `format`, schema `version`, and `kind` unchanged.
6. After public launch, verify catalog discovery, installation, explicit application, restart persistence, and uninstallation in an isolated test profile. Private repositories are unavailable to Market's anonymous downloader.

For an update, keep the catalog ID, commit the package first, and increase its `x.y.z` release version before updating the pinned revision and checksum in a separate catalog PR. Externally hosted packages must be publicly downloadable at the pinned commit and provide a redistribution license. Each catalog is limited to 500 entries.
