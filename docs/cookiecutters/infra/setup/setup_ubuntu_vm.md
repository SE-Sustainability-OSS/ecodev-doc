# Setting up an Ubuntu Virtual Machine

In order to apply all the ecodev-infra docker compose stacks, you need a Cloud Virtual Machine (VM) with ssh 
access. 

There are countless blog posts to help you decide which cloud provider is the best for you, and we won't add 
to this litterature here 😊. 

Instead we just provide a simple <a href=https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/ class="external-link" target="_blank">bash script</a>
to setup a freshly bought VM with an <a href=https://ubuntu.com/ class="external-link" target="_blank">Ubuntu</a> Operating System.

## Launching the script

After having accessed with ssh your VM, just `git clone` ecodev-infra. Then in the main folder, launch 
```bash
make setup-vm <YOURUSER> 
```

<div class="termy">
```console
$ make setup-vm toto
---> 100%
VM successfully setup!
```
</div>


Where `<YOURUSER>` is the username you used for connecting to the VM. The bash script executed will

- Install docker and related components
- Add `<YOURUSER>` to the `sudo` and `docker` group of the VM (might need to disconnect and reconnect **at the end of the setup** for this to take effect) 
- Setup <a href=https://doc.ubuntu.org/ufw class="external-link" target="_blank">ufw</a> (uncomplicated firewall) to work with docker (more on that below) 
- block all but 22 (with a <a href=https://www.cyberciti.biz/faq/howto-limiting-ssh-connections-with-ufw-on-ubuntu-debian/ class="external-link" target="_blank">rate limiter</a>), 80 (tcp only) and 443 (tcp only) ports.
- Setup a decent `history` (<a href=https://www.digitalocean.com/community/tutorials/how-to-use-bash-history-commands-and-expansions-on-a-linux-vps class="external-link" target="_blank">english resource on the topic</a>) memory size.


!!! warning 
    🚨 **Technical topic incoming** 🚨 

    As explained <a href=https://www.baeldung.com/linux/docker-container-published-port-ignoring-ufw-rules class="external-link" target="_blank">here</a> way better than we could, 
    the standard **ufw setup won't forbid access to docker exposed ports on your VM, even ports explicitely blocked with an ufw rule!!**.
    
    This has to do with technicalities related to  <a href=https://help.ubuntu.com/community/IptablesHowTo class="external-link" target="_blank">iptables</a>  explained in the linked posts. 
    
    To make ufw work with docker, one **has** to use this <a href=https://github.com/chaifeng/ufw-docker class="external-link" target="_blank">solution</a>. This is the reason of 
    the `sudo cp after.rules /etc/ufw/after.rules` line in the `setup.sh` script.

    So in reality after the setup your VM will look like so
    <p align="center">
     <a href="/img/ecodev_infra/ecodev_infra.png"><img src="/img/ecodev_infra/ecodev_infra.png" alt="infra with ufw"></a>
    </p>
    <p align="center">
        <em>Ecodev infra with ufw correctly setup</em>
    </p>
    <p align="center">
    </p>

!!! note
    You might be as curious as we were and wondering why there is no Open Source docker container responsible for dealing with firewall issues. 
    To the best of our knowledge, this has to do with the fact that firewell related stuff is really low level and needs to live close to the VM kernel.
    <a href=https://www.reddit.com/r/docker/comments/cmuxcs/how_do_i_deploy_a_firewall_in_a_container/ class="external-link" target="_blank">One example</a> out of the numerous conversations on the topic. 

    

## Future connections

Edit your `vim ~/.ssh/config` like so

```
Host your-host
    HostName `<VM IP>`
    User `<YOURUSER>`

Host *
   ForwardAgent yes 
   AddKeysToAgent yes 
   ControlMaster auto
   ControlPath ~/.ssh/ctrl-socket-%r@%h:%p
   ControlPersist 900
```

The last section will apply to all `Host` defined above. It has the following benefits:

- <a href=https://www.howtogeek.com/devops/what-is-ssh-agent-forwarding-and-how-do-you-use-it/ class="external-link" target="_blank">ForwardAgent</a>
- <a href=https://man.openbsd.org/ssh_config#AddKeysToAgent class="external-link" target="_blank">AddKeysToAgent</a> 
- <a href=https://man.openbsd.org/ssh_config#ControlMaster class="external-link" target="_blank">ControlMaster</a>  
- <a href=https://ldpreload.com/blog/ssh-control class="external-link" target="_blank">ControlPath and ControlPersist</a>   

## Configuring ssh securely

Configuring ssh in a secure manner being obviously critical, we advocate the curious reader to go read <a href=https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html target="_blank">this article</a>.

In our opinion, you should at least:

- disable password connection: a <a href=https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/ target="_blank">good link</a> explaining how to do so
- remov sha-1 key algorithms. a <a href=https://www.ezeelogin.com/kb/article/kex-and-host-key-algorithms-in-ssh-565.html target="_blank">good link</a> explaining how to do so 
- put `PrintMode`, `X11Forwarding` and `UsePAM` to `no`

Hence a `/etc/ssh/sshd_config` that looks like so (all comments ommited):

```bash
Include /etc/ssh/sshd_config.d/*.conf
ChallengeResponseAuthentication no
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
UsePAM no
KexAlgorithms diffie-hellman-group14-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group-exchange-sha256,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,curve25519-sha256
MACs umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512
X11Forwarding no
PrintMotd no
AcceptEnv LANG LC_*
```

If you find any files in `/etc/ssh/sshd_config.d/`, be sure to propagate the changes there (for instance 
overriding `PasswordAuthentication` to `no` in `50-cloud-init.conf`).

 
## Configuring  `timesyncd` securely

Some attacks rely on the fact that VMs fetch time in a specific manner. To change that and fetch time in a secure manner:

- edit `/etc/systemd/timesyncd.conf`, adding or editing the `NTP` key

  ```bash
  NTP=*.pool.ntp.org
  ```
  
  where * stands for the closest country to where your VM sits. Go consult [the official site](https://www.ntppool.org/) to learn what to put there.


- restart the `systemd-timesyncd` service: `sudo systemctl restart systemd-timesyncd`

- Check that everything is working as intended: `systemctl status systemd-timesyncd`

  You should see something like

  ```bash
  systemd-timesyncd.service - Network Time Synchronization
  Loaded: loaded (/usr/lib/systemd/system/systemd-timesyncd.service; enabled; preset: enabled)
  Active: active (running) since Tue 2024-05-07 10:08:55 UTC; 5 days ago
  ```
  


[A Reference](https://askubuntu.com/questions/972799/how-do-i-set-ubuntu-to-use-the-primary-time-server-time-nist-gov)

## Unattended upgrades 

Why? To ensure that both security and regular updates are automatically installed regularly on your VM

### In a nutshell

Go read [this](https://dev.to/bearlike/why-and-how-to-use-unattended-upgrades-on-a-ubuntu-server-4hoc#:~:text=However%2C%20it%20can%20be%20time,automatic%20installation%20of%20security%20updates)

It really contains all the steps to follow. To be noted, most distros already have `unattended-upgrades` installed, such that ` sudo apt-get install` does not install anything new.

Be sure to also allow `Update` upgrades.
