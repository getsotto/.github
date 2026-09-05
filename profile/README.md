# Sotto

End-to-end encrypted secret sync for developer teams. Stop Slacking your `.env`.

[![CI](https://github.com/getsotto/sotto/actions/workflows/ci.yml/badge.svg)](https://github.com/getsotto/sotto/actions/workflows/ci.yml)
![Release](https://img.shields.io/github/v/release/getsotto/sotto)
![Licence](https://img.shields.io/github/license/getsotto/sotto)
[![Good first issues](https://img.shields.io/github/issues-search/getsotto/sotto?query=label%3A%22good%20first%20issue%22%20state%3Aopen&label=good%20first%20issues&color=7057ff)](https://github.com/getsotto/sotto/labels/good%20first%20issue)

> [!WARNING]
> Sotto is pre-1.0 and has not had a third-party cryptographic audit.
> See [SECURITY.md](https://github.com/getsotto/sotto/blob/main/SECURITY.md).

## How it fits together

```mermaid
flowchart LR
    subgraph devices["Devices - plaintext and keys stay here"]
        direction TB
        cli["sotto CLI - init, set, run, share"]
        web["Browser vault - the same core via WASM"]
        core["sotto-core - KDF, XChaCha20-Poly1305 envelopes, X25519 sealed-box grants, rotation"]
        cli --> core
        web --> core
    end
    subgraph server["Server and network - ciphertext it cannot read"]
        direction TB
        api["Sync API - versioned writes, per-member grants, rotation log"]
        db[("Postgres - envelopes and grants")]
        api <--> db
    end
    core -- "encrypted envelopes" --> api
    core -- "sealed grants" --> api
    core -- "one-time shares" --> api
    style server stroke-dasharray: 5 5
```

Everything left of the dashed boundary holds keys and plaintext. Everything
right of it only ever sees ciphertext - including the database if stolen.

## Pick your path

| I want to… | Start here |
| --- | --- |
| Use Sotto | `curl -fsSL https://raw.githubusercontent.com/getsotto/sotto/main/install.sh \| sh`, then the [quick start](https://github.com/getsotto/sotto#quick-start) |
| Contribute | [CONTRIBUTING.md](https://github.com/getsotto/sotto/blob/main/CONTRIBUTING.md), then a [good first issue](https://github.com/getsotto/sotto/labels/good%20first%20issue) |
| Self-host | One-command deploy in [deploy/README.md](https://github.com/getsotto/sotto/blob/main/deploy/README.md) |

## Repositories

| Repo | What it is |
| --- | --- |
| [sotto](https://github.com/getsotto/sotto) | CLI, sync server, web client, and the shared crypto core |
| [sotto-action](https://github.com/getsotto/sotto-action) | Install the CLI in GitHub Actions |

<details>
<summary>Security model in 30 seconds</summary>

- Zero knowledge sync: the server sees ciphertext plus minimal metadata, never plaintext or usable keys.
- Teams: organisations with roles, per-member environment grants, key rotation on member removal, machine tokens for CI, lost key recovery.
- Honest limits: metadata exposure, pre transparency key directory, and the weaker assurance of the served browser client are all documented in [THREAT-MODEL.md](https://github.com/getsotto/sotto/blob/main/THREAT-MODEL.md).
- Report vulnerabilities privately per [SECURITY.md](https://github.com/getsotto/sotto/blob/main/SECURITY.md), never as a public issue.

</details>

<details>
<summary>First contribution in 5 minutes</summary>

1. Fork and clone [sotto](https://github.com/getsotto/sotto).
2. Pick a [good first issue](https://github.com/getsotto/sotto/labels/good%20first%20issue) - docs and CLI help-text issues need no cryptography knowledge.
3. Sign off every commit with `git commit -s` (Developer Certificate of Origin).
4. Questions that are not bugs belong in [Discussions](https://github.com/getsotto/sotto/discussions).

</details>
