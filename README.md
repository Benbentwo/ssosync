# ssosync 🌐

**Sync Google Workspace users and groups to AWS IAM Identity Center (SSO) via CLI or Lambda, now fully configurable via environment variables.**

This is intended to be used with CloudPosse's [aws-ssosync](https://github.com/cloudposse-terraform-components/aws-ssosync) component.

---

## 🚀 Features

- Fetches users and groups from Google Workspace using Admin SDK.
- Provisions, updates, and removes identities in AWS Identity Center using SCIM API.
- Configurable entirely via environment variables—ideal for Terraform, Kubernetes, or AWS Lambda.

---

## 🔧 Why This Fork?

The original `ssosync` required AWS Secrets Manager for sensitive parameters like SCIM tokens. Our fork removes that limitation. Now you can:

- Use any secret source supported by your Terraform provider.
- Inject values via environment variables: no more hardcoding or AWS Secrets dependency.
- Maintain portability across deployment platforms.

---

## 📌 Configuration

All settings are configurable via env vars (or CLI flags):

| Env Variable                                                                                     | Description                         |
| ------------------------------------------------------------------------------------------------ | ----------------------------------- |
| `GOOGLE_CREDENTIALS`                                                                             | Path to Google service‑account JSON |
| `GOOGLE_ADMIN`                                                                                   | Workspace admin email               |
| `SCIM_ENDPOINT`                                                                                  | AWS SSO SCIM endpoint URL           |
| `SCIM_ACCESS_TOKEN`                                                                              | AWS SSO SCIM access token           |
| Optional filters: `USER_MATCH`, `GROUP_MATCH`, `IGNORE_USERS`, `IGNORE_GROUPS`, `INCLUDE_GROUPS` |                                     |
| Sync mode: `SYNC_METHOD` (`groups` or `users_groups`)                                            |                                     |
| Logging options: `LOG_LEVEL`, `LOG_FORMAT`                                                       |                                     |

These map exactly to the fork’s CLI flags, e.g.:

```bash
export GOOGLE_CREDENTIALS="/secrets/google-creds.json"
export GOOGLE_ADMIN="admin@example.com"
export SCIM_ENDPOINT="https://portal.sso.us-west-2.amazonaws.com/scim/v2"
export SCIM_ACCESS_TOKEN="xxxx"
export SYNC_METHOD="groups"
export GROUP_MATCH="name:Dev*"
```

---

## ⚙️ Installation

1. Clone this repo:

   ```bash
   git clone https://github.com/Benbentwo/ssosync.git
   cd ssosync
   ```

2. Build the Go binary:

   ```bash
   make go-build
   ```

3. Or deploy the `./ssosync` binary directly in your environment or Lambda.

---

## 💻 Local Usage

```bash
./ssosync \
  --google-credentials "$GOOGLE_CREDENTIALS" \
  --google-admin "$GOOGLE_ADMIN" \
  --endpoint "$SCIM_ENDPOINT" \
  --access-token "$SCIM_ACCESS_TOKEN" \
  --sync-method "$SYNC_METHOD" \
  --group-match "$GROUP_MATCH" \
  --log-level info
```

Environment variables automatically provide defaults for flags.

---

## ☁️ AWS Lambda Deployment

Package the binary and deploy using your preferred IaC:

- **ZIP or containerize** `ssosync`.
- Set the above env vars in Lambda configuration (via Terraform, CloudFormation, or console).
- Schedule the function on a CRON trigger via EventBridge (CloudWatch Events) for periodic syncing.

---

## 📚 References

This fork is built on top of the original `awslabs/ssosync` project ([github.com][1], [github.com][2], [github.com][3])—all major capabilities remain intact, with the improved configuration layer front and center.

---

## 🛠️ Contributing

Please file issues or PRs if you encounter bugs or want new features. We welcome help with:

- More flexible secret backends (e.g., Kubernetes Secrets, HashiCorp Vault).
- Enhanced filtering options.
- Improved test coverage.

---

## ⚖️ License

Apache 2.0. See [LICENSE](LICENSE).
