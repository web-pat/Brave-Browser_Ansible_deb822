# Brave-Browser_Ansible_deb822

Ansible task installing Brave Browser using the new *ansible.builtin.deb822_repository* module as a replacement for deprecated methods handling signing keys.

## Why this task / playbook?
Most playbooks I found in the net used deprecated methods handling repository signing-keys. This task uses the new aforementioned Ansible module and best practices.

## What does it do?
### Security aspects:
The GPG key is directly referenced in the signed_by parameter of the repository configuration
This corresponds to the modern DEB822 format, which embeds keys in the source file
No deprecated methods like apt-key or separate key storage in /etc/apt/trusted.gpg.d/

### Process:
* Installation of required packages (apt-transport-https, curl)
* Download of the official Brave GPG key
* Conversion to the required format (dearmored)
* Configuration of the repository with deb822_repository
* Installation of Brave Browser
* Cleanup of temporary key files

The playbook is idempotent and can be executed multiple times without causing unwanted side effects.