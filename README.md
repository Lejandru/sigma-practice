# sigma-practice

Three Sigma rules I wrote while learning the format. They all catch Windows post-exploitation behavior that shows up in pretty much every intrusion writeup I've read.

## Rules

| File | What it catches | ATT&CK |
|---|---|---|
| `rules/whoami_by_service_account.yml` | `whoami.exe` running under an account named `svc_*` | T1033 |
| `rules/powershell_encoded_command.yml` | PowerShell with `-EncodedCommand` or `-enc` | T1059.001, T1027 |
| `rules/net_group_domain_admins.yml` | `net.exe` enumerating Domain Admins membership | T1069.002 |

## Validating locally

```
pip install sigma-cli
sigma check rules/
```

## Converting to Splunk SPL

Splunk needs its own plugin. Install it once:

```
sigma plugin install splunk
```

Then convert any rule:

```
sigma convert -t splunk -p splunk_windows rules/whoami_by_service_account.yml
```

Sample output:

```
Image="*\whoami.exe" User="*svc_*"
```

## CI

Every push runs `sigma check rules/` through GitHub Actions. The workflow lives at `.github/workflows/validate.yml`.
