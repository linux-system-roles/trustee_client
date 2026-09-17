Changelog
=========

[1.2.0] - 2026-09-17
--------------------

### New Features

- feat: Argument spec implementation for trustee client role (#59)

### Other Changes

- ci: use commit hash for github action, add persist-credentials false [citest_skip] (#48)
- ci: refactor build_docs so that pandoc runs in isolated read-only job [citest_skip] (#49)
- ci: use exact match for systemroller instead of contains [citest_skip] (#50)
- ci(build_docs): fix pandoc container syntax [citest_skip] (#51)
- ci: [citest_skip] Bump myrotvorets/set-commit-status-action from 2774e1f040c82ed70a76b4b5cd53bb11ffaedd0a to c0f880c99d91381c6fdb97726f03feb8004409b4 (#52)
- ci: [citest_skip] Bump actions/download-artifact from 7.0.0 to 8.0.1 (#53)
- ci: [citest_skip] Bump codespell-project/actions-codespell from 2.1 to 2.2 (#54)
- refactor: Relax collection constraints, gate vendored modules by python version, update ci versions [citest_skip] (#55)
- ci: do not run ci tests by default, require citest comment or label [citest_skip] (#56)
- ci: replace weekly_ci with periodic_ci, stagger schedules [citest_skip] (#57)
- ci: update status when action triggered by issue comment (#58)
- docs: fix spelling error in CHANGELOG.md (#60)

[1.1.0] - 2026-08-06
--------------------

### New Features

- feat: Write roles fingerprints to /var/log/sysroles.jsonl [citest_skip] (#46)

### Other Changes

- ci: Bump actions/checkout from 6 to 7 (#39)
- ci: Use our own pr_title_lint.py instead of NPM commitlint [citest_skip] (#40)
- ci: bump tox-lsr version to 3.20.0 to fix tox 4.58 api breakage [citest_skip] (#41)
- ci: Add support for Fedora 44 and drop Fedora 42 - use ansible-core 2.21 [citest_skip] (#43)
- ci: Bump actions/setup-python from 6 to 7 (#44)
- ci: ensure dependabot updates do not invoke ci tests [citest_skip] (#45)

[1.0.1] - 2026-06-24
--------------------

### Other Changes

- refactor: use ansible.posix 2.1.X for EL7 compatibility [citest_skip] (#35)
- ci: Add config file for CodeRabbit with custom rules (#36)
- ci: Skip reviews for PRs with [citest_skip] in the title (#37)

[1.0.0] - 2026-05-07
--------------------

### New Features

- feat: Add trustee-gc quadlet and disk encryption option (#9)
- feat: fix role for AWS (#10)
- feat(secret_registration_client): add secret registration client service (#16)
- feat: change name to trustee_client, add systemd-cryptenroll and allow kbs_cert by file path (#18)
- feat: add role fingerprints to syslog (#29)
- feat: new variable `trustee_client_secure_logging` defaulting to `true` (#32)

### Other Changes

- ci: bump ansible/ansible-lint from 25 to 26 (#1)
- refactor: rename template to cvm_deploy (#2)
- ci: skip most CI checks if title contains citest skip [citest_skip] (#4)
- ci: ansible-lint - remove .collection directory from converted collection [citest_skip] (#5)
- ci: tox-lsr version 3.15.0 [citest_skip] (#6)
- ci: Add Fedora 43, remove Fedora 41 from Testing Farm CI (#7)
- ci: Ansible version must be string, not float [citest_skip] (#8)
- ci: Bump actions/upload-artifact from 6 to 7 (#11)
- ci: use two managed nodes [citest_skip] (#12)
- ci: tox-lsr 3.16.0 - fix qemu tox test failures - rename to qemu-ansible-core-X-Y [citest_skip] (#13)
- chore: Rename cvm_deploy to trustee_attestation_client (#14)
- ci: tox-lsr 3.17.0 - container test improvements, use ansible 2.20 for fedora 43 [citest_skip] (#15)
- ci: tox-lsr 3.17.1 - previous update broke container tests, this fixes them [citest_skip] (#17)
- test: ensure role gathers the facts it uses by having test clear_facts before include_role (#24)
- chore: Update CODEOWNERS (#26)
- refactor: copy files into role, copy containers into quay lsr (#27)
- test: add test cleanup, add flush_handlers (#28)
- ci: use tox-lsr 3.18.1 [citest_skip] (#30)
- ci: Bump actions/github-script from 8 to 9 (#31)
- docs: document role parameters in README.md [citest_skip] (#33)

