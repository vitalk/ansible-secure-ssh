What is it?
-----------

This document describes some simple steps that improve the security of your SSH
installation. That steps are include:

* Disable the empty password login. Empty password is a **very bad** idea.

* Disable remote root login. The preferred way to gain root permissions is use
  `su` or `sudo` command.

* Disable password login. Before doing that step ensure that you add your
  public key into `~/.ssh/authorized_keys` on remote host.

  ```bash
  # Create a your public and private keys (if no exists)
  ssh-keygen -f identity

  # Copy your public key to remote host
  ssh-copy-id -i ~/.ssh/identity.pub remote-host

  # Note. Mac OSX does not provide ssh-copy-id program,
  # so your can use the command
  ssh remote-host 'mkdir -p .ssh'; cat >> ~/.ssh/authorized_keys' < ~/.ssh/identity.pub
  ```

* Enable [PAM](http://en.wikipedia.org/wiki/Pluggable_authentication_modules).


## Usage

The example below uses `sudo` to play book on your localhost via local
connection.

```bash
ansible-playbook playbook.yml \
    -i hosts.example \
    -l local -c local \
    -s --ask-sudo-pass
 ```
