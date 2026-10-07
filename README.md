# AccuKnox Secret Scan (Gitleaks) GitHub Action

Scans a repository for hardcoded secrets with Gitleaks via the AccuKnox ASPM scanner and uploads findings to the AccuKnox Console.

## Usage

Add `ACCUKNOX_TOKEN`, `ACCUKNOX_ENDPOINT`, `ACCUKNOX_LABEL` as repository secrets, then:

```yaml
name: AccuKnox Secret Scan
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  secret-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0   # full history so gitleaks can scan past commits

      - uses: Vickydew1/secret-scan-action-new@latest
        with:
          accuknox_token: ${{ secrets.ACCUKNOX_TOKEN }}
          accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          accuknox_label: ${{ secrets.ACCUKNOX_LABEL }}
          soft_fail: true
```

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `accuknox_token` | yes | | AccuKnox API token |
| `accuknox_endpoint` | yes | | AccuKnox Console endpoint |
| `accuknox_label` | yes | | Label for results in the Console |
| `additional_arguments` | no | `""` | Extra args appended to the gitleaks command |
| `base_command` | no | `detect --source . --report-format sarif --report-path results.json --no-banner` | Replaces the gitleaks command (keep `--report-path results.json` for artifact upload) |
| `soft_fail` | no | `false` | Don't fail the job on findings |
| `scanner_version` | no | `v0.15.1` | AccuKnox ASPM scanner CLI release |
| `upload_results` | no | `true` | Upload `results.json` as an artifact |


## Security notes

- Pass credentials only via `secrets.*`; they are exposed to the scanner as environment variables, never on the command line.
- Pin the action to a release tag or commit SHA for reproducible builds.
- The scanner binary is downloaded from the official `accuknox/aspm-scanner-cli` GitHub release; pin `scanner_version` as needed.

## License

Apache License 2.0, see [LICENSE](LICENSE).
