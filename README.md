# Secure SSH

## Disable root login

1. Create new user `useradd -m  username`.
2. Set password `passwd username`.
3. **Optional**: Add user to sudoers `usermod -aG sudo username`.
4. Edit `/etc/ssh/ssh_config` or `/etc/ssh/sshd_config` and add:
```bash
# Authentication:
PermitRootLogin no
AllowUsers username
```
Might need to look for other config files being included that might override this setting (`grep -r "PermitRootLogin" /etc/ssh/`).

## Harden SSH
5. Disable empty password:
```bash
PermitEmptyPasswords no
```
6. Limit the number of authentication tries per connection:
```bash
MaxAuthTries 3
```
7. Changed to `ssh` version 2:
```bash
Include /etc/ssh/sshd_config.d/*.conf
Protocol 2
```
## Disable plain text authentication
8. Connecting with SSH key:
```bash
UsePAM no
PasswordAuthentication no
```

```bash
ssh-keygen 
```
## Restart SSH service
9. Restart `ssh` service `sudo systemctl restart ssh` or `sudo systemctl restart sshd`.

## Prevent brute force attacks
10. Install [fail2ban](https://github.com/fail2ban/fail2ban) or [sshguard](https://www.sshguard.net/) to ban IPs that fail to authenticate after a certain number of attempts.

# References
* [13 Ways to secure SSH server](https://www.makeuseof.com/improve-your-linux-server-security-with-these-hardening-steps/).
* [How To Set up SSH Keys on a Linux / Unix System](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/)
* [sshd_config](https://linux.die.net/man/5/sshd_config).
* [ssh_config](https://linux.die.net/man/5/ssh_config).
* [ssh-keygen](https://linux.die.net/man/1/ssh-keygen).
* [useradd](https://linux.die.net/man/8/useradd).
* [passwd](https://linux.die.net/man/1/passwd).
* [usermod](https://linux.die.net/man/8/usermod).
* [fail2ban](https://github.com/fail2ban/fail2ban).
* [sshguard](https://www.sshguard.net/).
