# Security and Privacy

## Do not commit

- API keys, access tokens, cookies, passwords or browser profiles
- student names, scores, contact information or other personal data
- private absolute filesystem paths
- proprietary local corpora or copyrighted scans without permission
- `config/local-paths.yaml`

Use `config/local-paths.example.yaml` for public documentation and keep the real configuration local. Before publishing a change, inspect the staged diff and run a secret/path scan.

## Reporting

For a security or privacy issue, open a GitHub issue containing only a minimal, non-sensitive reproduction. Do not paste credentials or student data into an issue.
