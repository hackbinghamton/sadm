# Installation

## Prerequisites

First, you will need the secret key file in order to decrypt sensitive files in this repo.

Then, you will want to find an Ubuntu box, ideally AMD64 or ARM64 on some recent Ubuntu server version. Keep the following in mind creating a new server instance:

- Minimal images are good.
- Prefer SSH keys over password authentication. If your provider doesn't let you automatically set this up, you need to [configure it yourself](https://wiki.archlinux.org/title/OpenSSH#Force_public_key_authentication).

## Setup

Note that when a command has a `#`, it means [you must run the command as root](https://wiki.archlinux.org/title/Help:Reading#Root,_regular_user_or_another_user), e.g. using [sudo](https://manpages.ubuntu.com/manpages/stable/en/man8/sudo.8.html).

1. Log into the Ubuntu machine.
2. Update the system:
   ```
   # apt update
   # apt upgrade
   ```
3. Install dependencies for this repo:
   ```
   # apt install git git-crypt
   ```
4. Reboot so that the changes take effect: (this may need to be ran as root)
   ```
   $ systemctl reboot
   ```
5. Clone this repo:
   ```
   # git clone https://github.com/hackbinghamton/sadm.git /opt/sadm
   ```
6. Transfer ownership of the repo:
   ```
   # chown -R $USER:$USER /opt/sadm
   ```
7. Transfer the git-crypt key to the server. For instance, if the key is a file named `key`, then we might run `scp key bunny:/opt/sadm/key` on our host.
8. Decrypt the repo (using the destination path from the prior SCP command):
   ```
   $ cd /opt/sadm
   $ git-crypt unlock key
   $ rm key
   ```
9. Run the setup script:
   ```
   # /opt/sadm/setup
   ```
10. Obtain an SSL certificate:
    ```
    # certbot certonly --dns-cloudflare --dns-cloudflare-credentials /opt/sadm/cloudflare.ini --domains '*.hackbu.org' \
    --agree-tos --email 'admin@hackbu.org' --no-eff-email
    ```
