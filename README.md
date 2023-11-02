ansible-debian-base
===================

Setup some basic security and automation on new debian based systems.


Requirements
------------

A debian/ubuntu install with users matching the ones in the host_vars for the target host.


Host Variables
--------------

- apt_no_recommends (bool): If true disable apt recommends package install by default.
- apt_no_suggests (bool): If true diable apt suggests package install by default.
- apt_unattended_upgrades (bool): Enable unattended upgrades.
- apt_unattended_upgrades_mail (str): Email address for unattended upgrades.
- extra_packages (list): A list of extra packages to install with apt.
- no_passwords (bool): Disable passwords for root and interactive users for login and ssh.
- required_groups (list): A list of groups to add to the system.
- sshd_allowed_ssh_users_group: The group to allow ssh access to.
- sudoers_files (list): A list of sudoers files to deploy.
- user.groups (list): A list of groups to add the user to.
- user.home_dir (str): The user's home directory. 
- user.name (str): The user's name.
- user.password (str): The user's password hash. This should be encrypted with ansible vault. Defaults to `*` (disabled password).
- user.pubkeys (list): A list of ssh pubkeys to add the user's authorize_keys file.
- user.pubkeys_from_github_user (str): A github username to pull pubkeys from.
- user.pubkeys_from_gitlab_user (str): A gitlab.com username to pull pubkeys from.
- user.pubkeys_from_url (list): A list of URLs to pull ssh pubkeys from.
- user.ssh_user (bool): If true add this user to the `sshd_allowed_ssh_users_group` group.
- user.sudo_no_password (bool): If true add a sudoers file allowing all commands with no password.


Dependencies
------------

- devsec.hardening.ssh_hardening
- jnv.unattended-upgrades


Examples
--------

`host_vars/alice.yml`:
- Passwordless system
- Pull ssh pubkeys from config, github, gitlab, and a random URL
- Allow the default behavior for apt recommends.
- Force apt to not install suggests by default.
- Force the docker group GID to be ``996``.

```
---

users:
  - name: "hax"
    home_dir: "/home/hax"
    append_groups: yes
	groups:
	  - docker
    pubkeys:
      - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINEF8vRlfjhumQQhySJOxBdYyHZT/w/3lg7DZ6EjibCp backup user"
    pubkeys_from_github_user: haxwithaxe
    pubkeys_from_gitlab_user: haxwithaxe
    pubkeys_from_urls:
      - "https://git.local/haxwithaxe/pubkeys/raw/branch/main/automation"
    ssh_user: yes
    no_password: yes
    sudo_no_password: yes
  - name: "root"
    home_dir: "/root"

required_groups:
  - name: ssh-users
  - name: docker
    gid: 996
    system: yes

no_passwords: yes
sshd_allowed_ssh_users_group: ssh-users
sshd_moduli_size: 2048
sshd_rsa_host_key_size: 8192

apt_no_recommends: no
apt_no_suggests: yes

```


License
-------

GPLv3


Author Information
------------------

Created by [haxwithaxe](https://github.com/haxwithaxe).
