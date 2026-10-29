# trilio-agent-marketplace

The one place customers add to get every Trilio agent plugin. Owned by the
**platform group**. Part of **Option A (three-repo model)** in the
*Agent Plugins: Repository Structure Proposal*.

## Install

**Claude Code**

```sh
claude plugin marketplace add somesh22123/trilio-agent-marketplace
claude plugin install trilio-tsr-guide@trilio
```

**Codex**

```sh
codex plugin marketplace add somesh22123/trilio-agent-marketplace
```

Then install `trilio-tsr-guide` from the `/plugins` browser.

## How it works

| File | What it is |
| --- | --- |
| `catalog.yaml` | **The only file you edit.** One entry per SKU plugin: repo, path, release tag, commit sha |
| `.claude-plugin/marketplace.json` | Generated for Claude Code |
| `.agents/plugins/marketplace.json` | Generated for Codex |

Each entry uses a `git-subdir` source pinned to a release tag and its commit
sha, so customers only ever get a tested, released version, and a moved tag
can't change what they install.

1. A SKU team tags a release in its own repo (e.g. `trilio-tsr-guide/v0.2.0`).
2. The platform group bumps the entry:
   ```sh
   git clone https://github.com/somesh22123/trilio-agent-plugin-kit .pluginkit -b v1.0.0
   python .pluginkit/pluginkit.py pin . trilio-tsr-guide trilio-tsr-guide/v0.2.0
   ```
3. CI checks the generated files match `catalog.yaml` and that every entry's
   tag still exists, still points at the pinned sha, and ships manifests whose
   name and version match.
4. A nightly job runs the same check.

## Catalog

| Plugin | SKU | Source repo |
| --- | --- | --- |
| `trilio-tsr-guide` | Trilio Site Recovery | [tsr-k8s-skill](https://github.com/somesh22123/tsr-k8s-skill) |
