Changelog
=========

[1.0.1] - 2026-06-24
--------------------

### Other Changes

- refactor: use ansible.posix 2.1.X for EL7 compatibility [citest_skip] (#35)
- ci: Add config file for CodeRabbit with custom rules (#36)
- ci: Skip reviews for PRs with [citest_skip] in the title (#37)

[1.0.0] - 2026-05-07
--------------------

### New Features

- feat: Add trustee-gc quadlet and disk encyption option (#9)
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

