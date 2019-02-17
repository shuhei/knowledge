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

## Mount Linux file system

To use a visual editor, etc.

```sh
brew cask install osxfuse
brew install sshfs
mkdir -p ~/rdev/remote
sshfs ${linux_host}:/ ~/rdev/remote
```

https://www.quora.com/What-is-the-best-way-to-use-Atom-io-over-SSH/answer/Ryan-Cheu

## Netowork

### Test multiple IP addresses on local:

```sh
sudo ifconfig lo0 alias 127.0.0.2
```

https://serverfault.com/a/403097/225384

### Replacement for `telnet`

OS X Sierra dropped `telnet`. Use `nc (netcat)` instead:

```sh
nc localhost 3000
```

### Monitor Network Changes

- https://github.com/slozo/Network-listener/blob/master/Network%20Listener/AppDelegate.m
- https://gist.github.com/ftiff/f482fd1280cb6a5ca77a55b08b332396

You can use `scutil` (System Configuration Utililty) to investigate the dynamic store keys, which are used with [SCDynamicStore](https://developer.apple.com/documentation/systemconfiguration/scdynamicstore-gb2) and managed by `configd(8)`, like `State:/Network/Global/IPv4` or `State:/Network/Interface/en0/AirPort`.

```sh
$ scutil
> help
...
> list
...(list of keys)...
> show State:/Network/Interface
<dictionary> {
  Interfaces : <array> {
    0 : lo0
    1 : gif0
    2 : stf0
    3 : XHC20
    4 : en0
    5 : p2p0
    6 : awdl0
    7 : en1
    8 : bridge0
    9 : utun0
  }
}
> show State:/Network/Interface/en0/AirPort
<dictionary> {
  ...
}
```
