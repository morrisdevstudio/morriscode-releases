# Repository instructions

## GitHub Actions artifact policy (morrisdevstudio)

Applies to every existing and future repository owned by `morrisdevstudio`.
Minimize Actions storage without compromising tests, debugging, or official releases.

| Artifact purpose | Retention |
| --- | --- |
| Useful development builds (commits and pull requests) | 2 days |
| Test reports, exported diagnostic logs, screenshots | 7 days |
| Intermediate files needed by other jobs | 1 day fallback; delete after their last successful use |
| Official installers and distribution archives | GitHub Releases |
| Temporary files after successful publication | Delete when no consumer or recovery step needs them |

- Do not upload files without an identified download, diagnostic, or cross-job transfer need. Keep files local when all consumers run in the same job.
- Every `actions/upload-artifact` step must set an explicit `retention-days`. Upload only the required files, not an entire output directory by default.
- Preserve useful failure diagnostics for 7 days. Use `if: failure()` for failure-only diagnostics; retain successful test reports too when required for analysis or coverage.
- Publish official `.exe`, `.msi`, `.zip`, and other release assets directly to GitHub Releases. Preserve existing approved distribution/update channels; this storage policy does not authorize a production deployment.
- Do not retain redundant installer copies in Actions after successful publication. Never alter or delete assets already published in GitHub Releases as part of storage cleanup.
- Before changing a workflow, inspect producer/consumer jobs, reusable workflows, publication scripts, environment approvals, and retry/recovery behavior. If a necessary approval or recovery window exceeds 1 day, document and use the shortest sufficient intermediate retention.
- Delete intermediate artifacts only after successful publication and their last consumer. Restrict cleanup to exact artifact IDs or known names in the current run; never delete files still needed by queued, running, or failed jobs awaiting diagnosis/retry.
- On adoption, review existing artifacts separately: retention changes do not shorten existing expiry dates. Preserve necessary diagnostics and development downloads still within their useful window.
- Before finishing, validate workflow syntax, dependencies, explicit retention, and cleanup ordering. Check official assets remain available and report any execution checks that could not be run. Do not trigger a release merely to test retention.
- Review unnecessary repeated builds, but preserve required checks and deployment behavior. Actions caches have a separate allowance; do not purge useful dependency caches to fix the artifact quota.
- For every newly created repository, add this policy to its root `AGENTS.md`, configure the repository retention fallback to 7 days, and apply explicit 2/7-day artifact retention from the first workflow. The repository fallback also governs logs and run/check history, so explain this effect when setting it.

Actions builds and tests; GitHub Releases distributes and preserves official versions.
