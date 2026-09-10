1.Workspace Locked: Caused by overlapping builds using the same folder simultaneously; fix by enabling "Do not allow concurrent builds" in job settings.

2.Permission Denied (build.log): Caused by previous commands running as root; fix by resetting ownership (sudo chown -R jenkins:jenkins <path>) and avoiding sudo in build steps.

3.Stale Repo / Missing Commits: Caused by leftover files in a dirty workspace blocking Git updates; fix by enabling "Clean before checkout" under Git Additional Behaviours.
