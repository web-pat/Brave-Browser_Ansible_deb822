# Brave-Browser_Ansible_deb822

An Ansible playbook that installs Brave Browser from Brave's APT repository using the `ansible.builtin.deb822_repository` module.

## Why this task / playbook?
The playbook uses a DEB822 source and scopes repository verification to Brave's signing key. It requires `ansible-core` 2.15 or newer, where `ansible.builtin.deb822_repository` was introduced.

## What does it do?
### Security aspects:
The Brave signing key is stored persistently at `/usr/share/keyrings/brave-browser-archive-keyring-dearmored.gpg` and referenced by path in the repository's `Signed-By` field. It is not embedded in the source file and is not installed in the global trusted keyring.

### Process:
* Installation of `gnupg` and `python3-debian`, required by the playbook
* Download of the official Brave GPG key
* Conversion to a dearmored keyring at the persistent path above
* Configuration of the repository with deb822_repository
* Installation of Brave Browser
* Cleanup of the temporary downloaded key (the persistent keyring is retained for future APT updates)

The playbook is designed to be idempotent. If the downloaded key changes, the persistent keyring is regenerated so subsequent APT updates continue to use the current signing key.
