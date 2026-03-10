# trustee_attestation_client

[![ansible-lint.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/ansible-lint.yml) [![ansible-test.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/ansible-test.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/ansible-test.yml) [![codespell.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/codespell.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/codespell.yml) [![markdownlint.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/markdownlint.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/markdownlint.yml) [![qemu-kvm-integration-tests.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/qemu-kvm-integration-tests.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/qemu-kvm-integration-tests.yml) [![shellcheck.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/shellcheck.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/shellcheck.yml) [![tft.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/tft.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/tft.yml) [![tft_citest_bad.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/tft_citest_bad.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/tft_citest_bad.yml) [![woke.yml](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/woke.yml/badge.svg)](https://github.com/linux-system-roles/trustee_attestation_client/actions/workflows/woke.yml)

![trustee_attestation_client](https://github.com/linux-system-roles/trustee_attestation_client/workflows/tox/badge.svg)

Ansible role for deploying Trustee Guest Components using Podman Quadlets for
confidential virtual machine deployments. The role downloads quadlet files and
configuration files from a GitHub repository, installs them, and manages them as
systemd services. The role also supports optional disk encryption functionality for
securing additional storage devices.

The role will:

1. Install Podman and Git if not already present
2. Download Trustee Guest Components quadlet files and config files from the
   specified GitHub repository
3. Copy quadlet files (`.container`, `.volume`, `.network`, `.kube`) to the
   install directory (`/etc/containers/systemd` by default)
4. Copy config files from the repository's `configs` directory to `/etc/trustee-gc/`
5. Replace `KBS_URL` and `KBS_CERT` placeholders in `/etc/trustee-gc/cdh/config.toml`
   with the values from `trustee_attestation_client_trustee_kbs_url` and `trustee_attestation_client_trustee_kbs_cert`
   variables (if provided)
6. Reload systemd daemon
7. Enable and start the Trustee Guest Components services
8. (Optional) If `trustee_attestation_client_encrypt_disk` is `true`:
   - Find an unpartitioned and unmounted disk
   - Create a GPT partition table and partition on the disk
   - Generate an encryption key and encrypt the partition using LUKS
   - Format the encrypted partition with ext4
   - Mount the encrypted disk at the specified mount point
   - Store the encryption key in the `encrypted_disk_key` fact

Example of setting the variables:

```yaml
trustee_attestation_client_quadlet_repo_url: "https://github.com/litian1992/trustee-gc-quadlet-rhel"
trustee_attestation_client_quadlet_repo_path: "quadlet"
trustee_attestation_client_quadlet_repo_branch: "main"
trustee_attestation_client_trustee_kbs_url: "https://kbs.example.com"
trustee_attestation_client_trustee_kbs_cert: "/path/to/cert.pem"
trustee_attestation_client_encrypt_disk: true
```

## Variables Exported by the Role

### encrypted_disk_key

If disk encryption is enabled (`trustee_attestation_client_encrypt_disk: true`), this fact
contains the base64-encoded encryption key for the encrypted disk. This key is
required to mount the encrypted disk after a reboot. The key is automatically
generated during disk encryption and should be securely stored for future use.

## Example Playbook

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

```yaml
- name: Deploy Trustee Guest Components using Podman Quadlets
  hosts: all
  vars:
    trustee_attestation_client_quadlet_repo_url: "https://github.com/litian1992/trustee-gc-quadlet-rhel"
    trustee_attestation_client_quadlet_repo_path: "quadlet"
    trustee_attestation_client_quadlet_repo_branch: "main"
    trustee_attestation_client_trustee_kbs_url: "https://kbs.example.com"
    trustee_attestation_client_trustee_kbs_cert: "/path/to/kbs-cert.pem"
    trustee_attestation_client_encrypt_disk: true
  roles:
    - linux-system-roles.trustee_attestation_client
```

## License

Whenever possible, please prefer MIT.

## Author Information

An optional section for the role authors to include contact information, or a
website (HTML is not allowed).
