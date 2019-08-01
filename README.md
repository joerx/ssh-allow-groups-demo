# SSH User Group Restriction Demo

Use `AllowGroups` in `sshd` config to limit access to a specific user group.

## Usage

```sh
# start & provision  VM
$ vagrant up
...

# default login
$ vagrant ssh

# successful login
$ ssh -p 2222 -i .vagrant/machines/default/virtualbox/private_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null john@localhost
john@ubuntu-bionic:~$

# should fail
$ ssh -p 2222 -i .vagrant/machines/default/virtualbox/private_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null notjohn@localhost
Connection closed by 127.0.0.1 port 2222
```

## Reference

- https://www.ostechnix.com/allow-deny-ssh-access-particular-user-group-linux/
