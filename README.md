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
trustee_client_kbs_url: "https://kbs.example.com"
trustee_client_kbs_cert_content: "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----"  # or trustee_client_kbs_cert_src: "/path/to/server.crt"
trustee_client_secret_registration_enabled: true
trustee_client_encrypt_disk: true
```

## Variables

### trustee_client_trustee_gc

Whether to deploy Trustee Guest Components using Podman Quadlets (packages,
quadlet files, `/etc/trustee-gc/` configuration, and the `trustee-gc` pod
service). When `false`, this part of the role is skipped.

The secret registration client is only deployed when this variable and
`trustee_client_secret_registration_enabled` are both `true`.

Default: `true`

Type: `bool`

### trustee_client_kbs_url

URL of the Key Broker Service (KBS) used by Trustee Guest Components. When
non-empty, the role replaces the `KBS_URL` placeholder in the Attestation Agent
and Confidential Data Hub `config.toml` files under `/etc/trustee-gc/` with
this value.

If unset (empty string), the bundled placeholder values in those files are left
unchanged.

Default: `""`

Type: `string`

### trustee_client_kbs_cert_content

PEM-encoded TLS certificate for the KBS server, as a string (for example from
Ansible Vault). When non-empty, the role replaces the `KBS_CERT` placeholder in
the AA and CDH `config.toml` files and writes the certificate to
`/etc/trustee-gc/server.crt`.

Use either this variable or `trustee_client_kbs_cert_src`, not both as the
primary source; if both are set, explicit content takes precedence when it is
non-empty.

Default: `""`

Type: `string`

### trustee_client_kbs_cert_src

Path on the control node to a PEM certificate file for the KBS server. Used
when `trustee_client_kbs_cert_content` is empty. The file is read with
`lookup('file', ...)` and applied like `trustee_client_kbs_cert_content`.

Default: `""`

Type: `string`

### trustee_client_secret_registration_enabled

Whether to install and configure the secret registration client (requires
Trustee Guest Components; see `trustee_client_trustee_gc`). When `true`, the
client can register with the Secret Registration Server and supply disk
encryption keys when encrypt-disk features use it.

Default: `false`

Type: `bool`

### trustee_client_encrypt_disk

Whether to locate an unpartitioned disk, create a LUKS2 encrypted volume, and
mount it. The encryption key comes from the secret registration client when
`trustee_client_secret_registration_enabled` is `true`, otherwise from a
generated key and TPM binding via `systemd-cryptenroll` (PCR 7).

Default: `false`

Type: `bool`

### trustee_client_encrypt_disk_mount_point

Filesystem path where the encrypted disk (mapper device
`trustee_client_encrypted_disk_0`) is mounted when
`trustee_client_encrypt_disk` is `true`. The directory is created if missing.

Default: `"/mnt/encrypted-disk"`

Type: `string`

### trustee_client_secure_logging

If `true`, suppress potentially sensitive output from tasks that handle
credentials, secrets, and other sensitive data by setting `no_log: true` on
those tasks. This prevents passwords, API tokens, private keys, and similar
sensitive information from appearing in Ansible logs and console output.

If you need to debug issues with credential handling or secret management, you
can temporarily set `trustee_client_secure_logging: false` to see the full output from
these tasks. However, be aware that this may expose sensitive information in
logs, so it should only be used in development or troubleshooting scenarios.

Default: `true`

Type: `bool`

## Example Playbook

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

```yaml
- name: Deploy Trustee Guest Components using Podman Quadlets
  hosts: all
  vars:
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
