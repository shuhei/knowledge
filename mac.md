# Mac

## VirutalBox

### SSH into a guest OS

On Mac:

1. VirtualBox -> Selected the guest OS
2. Click "Settings" -> "Network"
3. Attach "Adapter 1" to "Bridged Adapter" and "en0: Wi-Fi (AirPort)", etc.
  - "Host-only Adapter" works for SSH, but the guess OS cannot access to the internet
4. Restart the guest OS

On guest OS:

```sh
sudo apt-get install openssh-server
sudo service ssh status

# Note your username and IP address
ifconfig
whoami
```

On Mac:

```sh
ssh ${username}@${ip}
```

It's handy to name the guess OS:

```sh
sudo bash -c "echo ${ip} ${nickname} >> /etc/hosts"
```
