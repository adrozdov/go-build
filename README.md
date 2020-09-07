Access LXD container with public SSH key.

1. Generate private and public keys on the host:

```
ssh-keygen -t rsa
```

2. Enter location of private key and its name

```
Enter file in which to save the key (~/.ssh/[key_name]_rsa):
```

3. Enter passphrase for the key:

```
Enter passphrase (empty for no passphrase):
```

You'll need to copy [name]_rsa.pub into container. We'll be accessing container as a `ubuntu` user. Location of public key is `/home/ubuntu/.ssh/authorized_keys`. In this project it's code in the `copy` section of the Bravefile.

It could be cone using `lxc` command:

```
cat [key_name]_rsa.pub | lxc exec <container> -- sh -c "cat >> /home/ubuntu/.ssh/authorized_keys"
```

Note, that the public key should be appended to `authorized_keys` file

4. Configure host shell to use generated private key. In the directory `~/.ssh` edit (or create a new if not exists) file `config`. Add to it:

```
Host [IP address of the LXD host]
  IdentityFile ~/.ssh/[key_name]_rsa
```

5. Modify `/etc/ssh/sshd_config` file inside container. Uncomment/modify `PubkeyAuthentication yes`. Restart ssh `systemctl restart sshd`