# dashboard-migration

Power BI reports: switch the data source from Databricks to Tencent DLC by editing PBIP / TMDL files.

- **Read [HANDOFF.md](HANDOFF.md) first, update it last** (same commit as every change) + one line in [LOG.md](LOG.md).
- Several Claude accounts share this repo and nothing else: follow [docs/ACCOUNT_SWITCH_PROTOCOL.md](docs/ACCOUNT_SWITCH_PROTOCOL.md) (lock, take over, hand over). Commands: `/pbi`, `/handover`.
- Branch `main` only; never force-push. Reports in `reports/<report name>/`.
- Owner preferences: reply in Thai, short, verified facts only (say when something is inferred). Before building something big, give a token estimate and ask whether an atomic-level (step by step) explanation is wanted; skip questions already answered in HANDOFF.
- Never store credentials in files or chat. Never edit measures to hide a renamed column; fix it in Power Query.
