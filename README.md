# slopless-action

Run [slopless](https://github.com/seochecks-ai/slopless) in GitHub Actions - a deterministic textlint
CLI that flags AI and human prose slop in Markdown. The step fails when slop is found, so it works as
a CI gate.

## Usage

```yaml
- uses: seochecks-ai/slopless-action@v1
  with:
    files: 'docs/**/*.md'
```

## Inputs

| Input | Default | Description |
|---|---|---|
| `files` | `**/*.md` | Path or glob of Markdown to lint. Scope it (e.g. `docs/**/*.md`) to avoid `node_modules`. |
| `version` | `latest` | slopless npm version. Pin for reproducibility (e.g. `0.2.21`). |
| `node-version` | `22` | Node to set up (slopless needs `>=22.13`). Empty string uses the runner's node. |
| `output-file` | _(none)_ | If set, writes the JSON findings to this path. |

## Outputs

| Output | Description |
|---|---|
| `findings` | The JSON findings slopless emitted. |

## Behavior

slopless exits `1` when it finds slop (fails the step and the workflow) and `0` when clean. Output is
JSON.

Capture findings for a later step:

```yaml
- id: slop
  uses: seochecks-ai/slopless-action@v1
  with:
    files: 'docs/**/*.md'
    output-file: slopless.json
- if: failure()
  run: cat slopless.json
```

## License

MIT
