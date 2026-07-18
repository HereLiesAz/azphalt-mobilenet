# MobileNetV3

**MobileNetV3 (image classifier + embedder)** — A compact vision backbone usable both as a labeler and as a feature embedder.

An **azphalt** AI-model plugin, packaged as a `.azp` (the azphalt analogue of a VS Code `.vsix`). It is
named for the *model*, not a single feature — the same model powers many tools, and it is **host-neutral**:
any azphalt host that understands its role can use it, not just one app. Install it from any host's
**Azphalt Storefront**.

## What it can do

- image labeling / tagging
- footage search
- learned concepts (image embeddings)
- content-based similarity

## Roles (host-neutral routing)

This plugin contributes the role(s): `image-embedding`, `image-labeling`. A host routes the model by role — it carries no
`targetApps`, so it is not tied to any single application.

**Example host — [Guillotine](https://github.com/HereLiesAz/Guillotine):** Desktop `labelModelPath` AND `idEmbedModelPath` — describe frame, footage search, concept learning.

## Model file(s)

- **`mobilenetv3.onnx`** (role `image-labeling`) — [upstream](https://huggingface.co/onnx-community/mobilenetv3_small_100/resolve/main/onnx/model.onnx)
- **`mobilenetv3.onnx`** (role `image-embedding`) — [upstream](https://huggingface.co/onnx-community/mobilenetv3_small_100/resolve/main/onnx/model.onnx)

Model license: **Apache-2.0 (MobileNetV3, Google / timm)**. This plugin's manifest/packaging is `Apache-2.0`.

## How it works — the VSCode Header Pattern

The `.azp` does **not** bundle the weights. The manifest declares each model as a *remote asset*
(`"path": ""` + `remoteUrl` + `checksum` + `byteSize`); the host downloads the weights on install and
verifies them against the pinned SHA-256 — exactly how a large VS Code extension fetches its language
server instead of shipping it inside the `.vsix`. `remoteUrl` points at this repo's own GitHub **Release**
asset (named the exact filename the host expects); the `release` workflow fetches the upstream model,
renames it, checksums it, and publishes it beside the packed `.azp`.

## Build / release

```sh
npm install && npm run build     # packs com.hereliesaz.azphalt.mobilenet-1.0.0.azp
git tag v1.0.0 && git push --tags   # runs the release workflow: hosts the model + .azp
```
