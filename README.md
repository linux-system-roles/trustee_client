# trustee_client

[![ansible-lint.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/ansible-lint.yml) [![ansible-test.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/ansible-test.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/ansible-test.yml) [![codespell.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/codespell.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/codespell.yml) [![markdownlint.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/markdownlint.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/markdownlint.yml) [![qemu-kvm-integration-tests.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/qemu-kvm-integration-tests.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/qemu-kvm-integration-tests.yml) [![shellcheck.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/shellcheck.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/shellcheck.yml) [![tft.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/tft.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/tft.yml) [![tft_citest_bad.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/tft_citest_bad.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/tft_citest_bad.yml) [![woke.yml](https://github.com/linux-system-roles/trustee_client/actions/workflows/woke.yml/badge.svg)](https://github.com/linux-system-roles/trustee_client/actions/workflows/woke.yml)

![trustee_client](https://github.com/linux-system-roles/trustee_client/workflows/tox/badge.svg)

Ansible role for deploying Trustee Guest Components using Podman Quadlets for
confidential virtual machine deployments. The role downloads quadlet files and
configuration files from a GitHub repository, installs them, and manages them as
systemd services. The role also supports an optional secret registration client
for disk key registration and optional disk encryption for securing additional
storage devices.

## Features

- **Trustee Client (Quadlet)**: Deploys Trustee guest components Attestation Agent(AA), Confidential Data Hub(CDH) and API Server REST(ASR) using Podman Quadlets from a Github repository
- **Secret Registration Client**: Utility script and service which registers to Secret Registration Server on Trustee Server. It acquires the encryption key from Trustee and decrypts the designated disk upon boot
- **Encrypt Disk**: Does LUKS2 encryption of the found empty data disk. The encryption key is provided by Secret Registration Client.

Example of setting the variables:

```yaml
trustee_client_quadlet_repo_url: "https://github.com/litian1992/trustee-gc-quadlet-rhel"
trustee_client_quadlet_repo_path: "quadlet"
trustee_client_quadlet_repo_branch: "main"
trustee_client_kbs_url: "https://kbs.example.com"
trustee_client_kbs_cert_content: "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----"  # or trustee_client_kbs_cert_src: "/path/to/server.crt"
trustee_client_secret_registration_enabled: true
trustee_client_encrypt_disk: true
```

## Example Playbook

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

```yaml
- name: Deploy Trustee Guest Components using Podman Quadlets
  hosts: all
  vars:
    trustee_client_quadlet_repo_url: "https://github.com/litian1992/trustee-gc-quadlet-rhel"
    trustee_client_quadlet_repo_path: "quadlet"
    trustee_client_quadlet_repo_branch: "main"
    trustee_client_kbs_url: "https://kbs.example.com"
    trustee_client_kbs_cert_content: "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----"
    trustee_client_secret_registration_enabled: true
    trustee_client_encrypt_disk: true
  roles:
    - linux-system-roles.trustee_client
```

## Trustee Client

The task:

1. Downloads the Podman Quadlets from designated repo
2. Configures the settings in /etc/trustee-gc/
3. Enables and starts trustee-gc.pod as a service

## Secret Registration Client

When enabled, this task:

1. Sends registration request to Secret Registration Server via HTTPS to acquire disk encryption keys
2. Requests above disk encryption key upon boot when Encrypt Disk is enabled to decrypt and mount disk

## Encrypt Disk

An unpartitioned empty disk must be attached to the target. When enabled, this task:

1. Finds the first unpartitioned and unmounted disk
2. Encrypts the disk using a key from either:
  a. secret key fetched using Secret Registration Client (when enabled), or
  b. `systemd-cryptenroll` which binds to PCR 7
3. Mounts it at the designated path
4. Sets up automatic unlock and mount either with Secret Registration Client service or /etc/crypttab with `systemd-cryptenroll`

## License

Whenever possible, please prefer MIT.

## Author Information

An optional section for the role authors to include contact information, or a
website (HTML is not allowed).
