# Scaffold Repository Verification

[Docs](../README.md) > [Architecture](README.md) > Scaffold Repository Verification

This record contains verification evidence for `austinperrin/scaffolding` from
the initial review on 2026-11-09 and follow-up observations through 2026-11-17.
The initial review was performed with the repository owner's authorization. The
[settings checklist](repository-settings.md) defines the controls to verify;
the [roadmap](../roadmap/milestone-0-repository-scaffolding.md) owns phase status.

## Target and Review Policy

The target is the public `austinperrin/scaffolding` repository, with default branch
`main` and maintainer `austinperrin`. The active ruleset is
[Main branch review and checks](https://github.com/austinperrin/scaffolding/settings/rules/26913767).
Its verified configuration requires:

- Pull requests and resolution of review conversations.
- The four checks listed in the settings checklist, from the GitHub Actions
  integration, with the branch up to date before merging.
- Protection against branch deletion and force pushes, with no bypass actors.
- Squash merging as the only allowed merge method.

Required external approvals are zero because the project has one maintainer.
This does not represent an independent approval or remove the maintainer's
responsibility to review the diff, results, and merge contents. CODEOWNERS remains
an inactive scaffold; owner reviews are not required without eligible reviewers.

Repository settings enable automatic deletion of merged branches and select
`PR_TITLE` for squash commit titles. Default workflow permissions are read-only,
and workflows cannot approve pull requests. Actions are enabled; repository
checks enforce the reviewed action pins rather than an Actions allowlist.

## Verification Results

| Area | Result and evidence |
| :--- | :--- |
| Repository suite and external checks | [Main-branch run](https://github.com/austinperrin/scaffolding/actions/runs/42743937082) passed Linux, macOS, and external checks at `6242896`, including checkout-cleanliness checks. [PR #8's final run](https://github.com/austinperrin/scaffolding/actions/runs/42743480316) also passed. |
| Credential workflow dispatch | [Manual run on main](https://github.com/austinperrin/scaffolding/actions/runs/42748211138) passed full-history checkout, verified scanner installation, redacted scanning, and checkout-cleanliness verification. |
| Scheduled activation | The [2026-11-16 scheduled run](https://github.com/austinperrin/scaffolding/actions/runs/43870399784) passed on `main` at `b10560f`. Full-history checkout, verified scanner installation, redacted scanning, and checkout-cleanliness steps passed; the scan found no leaks. |
| Dependabot activation | The [initial update job](https://github.com/austinperrin/scaffolding/actions/runs/42743945842) and [2026-11-16 weekly job](https://github.com/austinperrin/scaffolding/actions/runs/43847316883) succeeded. The weekly log reports that no update is needed for `actions/checkout` 7.0.1. No update PR is available yet to inspect its title or checks. |
| PR-title events | [PR #9](https://github.com/austinperrin/scaffolding/pull/9) passed on opening with a [valid title](https://github.com/austinperrin/scaffolding/actions/runs/42748559879). An edit to an [invalid title failed](https://github.com/austinperrin/scaffolding/actions/runs/42748616572); restoring the valid title [passed again](https://github.com/austinperrin/scaffolding/actions/runs/42748669851). All runs used the expected `pull_request_target` event. |
| Merge blocking | PR #9 was `BLOCKED` while a required repository check was pending. With all three repository jobs successful, the invalid title kept it `BLOCKED`. Restoring the valid title changed the merge state to `CLEAN`. No merge was attempted. The [repository-check run](https://github.com/austinperrin/scaffolding/actions/runs/42748559565) passed all three jobs. |
| Fork contributions | [PR #26](https://github.com/austinperrin/scaffolding/pull/26) came from the separate `ECISDaustin/scaffolding` fork, whose account has read access but no write or maintain access to the base repository. GitHub held the `pull_request` workflow for maintainer approval before starting its jobs. After approval, the [repository-check run](https://github.com/austinperrin/scaffolding/actions/runs/44245767058) passed Linux, macOS, and external checks; the [title check](https://github.com/austinperrin/scaffolding/actions/runs/44245923792) passed through `pull_request_target`. The test PR was closed without merging. |
| Reporting channels | Private vulnerability reporting is disabled. On 2026-11-09, the maintainer confirmed that `perrinaustin@gmail.com` is monitored and receives private security reports. This is a maintainer confirmation, not an automated delivery test. |
| Additional dependency features | Advisory alerts and automatic security updates are disabled. The current configuration covers GitHub Actions version updates; these features have not been enabled as part of this verification. |

## Accepted Follow-ups

On 2026-11-09, the maintainer accepted the following cases as follow-ups owned by
`austinperrin`. Their status below reflects the evidence available on 2026-11-17.

| Follow-up | Status | Evidence or next trigger |
| :--- | :--- | :--- |
| First scheduled credential scan | Verified | The [2026-11-16 `schedule` run](https://github.com/austinperrin/scaffolding/actions/runs/43870399784) passed on the expected default-branch commit with redacted scanning and an unchanged checkout. |
| Dependabot update PR | Pending | The weekly check found no applicable update. Inspect the expected title and required checks when an upstream update produces a PR. |
| External fork contribution | Verified | [PR #26](https://github.com/austinperrin/scaffolding/pull/26) required maintainer workflow approval; all four required checks passed afterward. The `pull_request` job logs reported `contents: read`, and the title workflow used event metadata without checking out fork code. |

The implemented controls and available live cases are verified, and the
maintainer has accepted the remaining limitations. The roadmap records Phase 6
completion on that basis. The first scheduled scan and external fork case are
verified. The Dependabot update PR case remains pending until an applicable update is
available. The fork PR did not access or test a repository secret; its read-only
token scope is visible in the job logs, and the workflows do not pass project
secrets. The test PR allowed maintainer edits to its fork branch; that setting
did not grant the fork account write access to the base repository.

## Related Documents

- [Repository settings and verification](repository-settings.md) - Canonical setup checklist.
- [CI workflows and enforcement](ci-cd.md) - Hosted workflow behavior and enforcement status.
- [Git workflow](../standards/git-workflow.md) - Review and merge requirements.
- [Security policy](../../SECURITY.md) - Reporting channels.
