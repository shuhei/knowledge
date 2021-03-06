# Mac

## Command-line Tools

Use the command-line tools from XCode:

```sh
sudo xcode-select --switch /Applications/Xcode.app
```

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

## Network

### Get a local IP address

```sh
ipconfig getifaddr en0
```

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

## How-to

### Compress big PDF files

```sh
brew install ghostscript
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```

- https://gist.github.com/firstdoit/6390547
- https://www.ghostscript.com/doc/9.50/Use.htm

### Share a bluetooth mouse sequentially between two Macs

System Preferences -> Bluetooth -> Uncheck "Allow Bluetooth devices to wake this computer".

- https://apple.stackexchange.com/a/89707/66857

Make sure to connect the charger to the Macbook that you want to use closed because the bluetooth mouse can't wake it up.

### Take a screenshot without the shadow

To do it once, press `<Space>` holding `<Option>`.

To make it the default:

```sh
defaults write com.apple.screencapture disable-shadow -bool TRUE
killall SystemUIServer
```

- https://apple.stackexchange.com/questions/50860/how-do-i-take-a-screenshot-without-the-shadow-behind-it

### Portrait mode

To use displays in the portrait mode with bigger resolutions, click `Displays -> Display -> Resolution -> Scaled` with the Alt/Option key.

https://discussions.apple.com/thread/252102178

### Application shortcut

To add an application shortcut, go to `Keyboard -> Shortcuts -> App Shortcuts` and click "+". Write the exact menu item name that you can see in the application's menu.

https://support.apple.com/guide/mac-help/create-keyboard-shortcuts-for-apps-mchlp2271/mac

I'm not sure about the advice about `->`. With OS 11.1 and Chrome 88, I had to input `Developer Tools` instead of `Developer->Developer Tools`.
