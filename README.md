Secure SSH
==========

This document describes some simple steps that improve the security of your SSH
installation. That steps are include:

* Disable the empty password login. Empty password is a **very bad** idea.

* Disable remote root login. The preferred way to gain root permissions is use
  `su` or `sudo` command.

* Add your identity key to `~/.ssh/authorized_keys` on remote host for
  passwordless login.

* Disable password login (done only if previous step is successful).

* Enable [PAM](http://en.wikipedia.org/wiki/Pluggable_authentication_modules).

Role Variables
--------------

The desired behavior can be refined via variables.

Option | Description
--- | ---
`sshd` | Name of ssh daemon, default is `ssh`.
`sshd_config` | Path to ssh daemon config, default is `/etc/ssh/sshd_config`.
`ssh_identity_key` | Path to your identity key. Added to `~/.ssh/authorized_keys` on remote host if both `ssh_identity_key` and `ssh_user` are defined. Default is `undefined`.
`ssh_user` | Username on remote host whose authorized keys will be modified. Uses only if `ssh_identity_key` is defined. Default is `undefined`.

For example, you can override default variables by passing it as a parameter to
the role like so:

```yaml
roles:
    - { role: ., ssh_user: vital, ssh_identity_key: /home/vital/.ssh/id_rsa.pub }
```

Or send them via command line:

```bash
ansible-playbook test.yml --extra-vars "sshd_config=/etc/sshd_config"
```

Example Playbook
----------------

The example below uses `sudo` to play book on your localhost via local
connection.

```bash
ansible-playbook test.yml \
    -i hosts.example \
    -c local \
    -s --ask-sudo-pass
 ```

```yaml
# file: test.yml
- hosts: local
  roles:
    - { role: ., sshd: ssh, sshd_config: /etc/sshd_config }
```

License
-------

Licensed under the [MIT license](http://mit-license.org/vitalk).

Author Information
------------------

Created by Vital Kudzelka.

Don't hesitate create [a GitHub Issue](https://github.com/vitalk/ansible-secure-ssh/issues) if you have any bugs or suggestions.
