**⭐PRs or any form of contribution will be appreciated⭐**

# SpoofDPI

Read in other Languages: [🇬🇧English](https://github.com/xvzc/SpoofDPI), [🇰🇷한국어](https://github.com/xvzc/SpoofDPI/blob/main/_docs/readme_ko.md), [🇨🇳简体中文](https://github.com/xvzc/SpoofDPI/blob/main/_docs/readme_zh-cn.md), [🇷🇺Русский](https://github.com/xvzc/SpoofDPI/blob/main/_docs/readme_ru.md), [🇯🇵日本語](https://github.com/xvzc/SpoofDPI/blob/main/_docs/readme_ja.md)

A simple and fast software designed to bypass **Deep Packet Inspection**

![image](https://user-images.githubusercontent.com/45588457/148035986-8b0076cc-fefb-48a1-9939-a8d9ab1d6322.png)

# Installation
## Binary
SpoofDPI will be installed in `~/.spoof-dpi/bin`.
To run SpoofDPI in any directory, add the line below to your `~/.bashrc || ~/.zshrc || ...`
```
export PATH=$PATH:~/.spoof-dpi/bin
```
---
```bash
# macOS Intel
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s darwin-amd64

# macOS Apple Silicon
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s darwin-arm64

# linux-amd64
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-amd64

# linux-arm
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-arm

# linux-arm64
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-arm64

# linux-mips
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-mips

# linux-mipsle
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-mipsle
```


## Go
You can also install SpoofDPI with `go install`
```bash
$ go install github.com/xvzc/SpoofDPI/cmd/spoof-dpi@latest
```

## Git
You can also build your own
```bash
$ git clone https://github.com/xvzc/SpoofDPI.git
$ cd SpoofDPI
$ go build ./cmd/...
```

# Usage
```
Usage: spoof-dpi [options...]
  -addr string
        listen address (default "127.0.0.1")
  -debug
        enable debug output
  -dns-addr string
        dns address (default "8.8.8.8")
  -dns-port int
        port number for dns (default 53)
  -enable-doh
        enable 'dns-over-https'
  -no-banner
        disable banner
  -pattern value
        bypass DPI only on packets matching this regex pattern; can be given multiple times
  -port int
        port (default 8080)
  -system-proxy
        enable system-wide proxy (default true)
  -timeout int
        timeout in milliseconds; no timeout when not given
  -v    print spoof-dpi's version; this may contain some other relevant information
  -window-size int
        chunk size, in number of bytes, for fragmented client hello,
        try lower values if the default value doesn't bypass the DPI;
        when not given, the client hello packet will be sent in two parts:
        fragmentation for the first data packet and the rest
```
> If you are using any vpn extensions such as Hotspot Shield in Chrome browser,
  go to Settings > Extensions, and disable them.

### OSX
Run `spoof-dpi` and it will automatically set your proxy

### Linux
Run `spoof-dpi` and open your favorite browser with proxy option
```bash
google-chrome --proxy-server="http://127.0.0.1:8080"
```

# How it works
### HTTP
 Since most of websites in the world now support HTTPS, SpoofDPI doesn't bypass Deep Packet Inspections for HTTP requets, However It still serves proxy connection for all HTTP requests.

### HTTPS
 Although TLS encrypts every handshake process, the domain names are still shown as plaintext in the Client hello packet.
 In other words, when someone else looks on the packet, they can easily guess where the packet is headed to.
 The domain name can offer a significant information while DPI is being processed, and we can actually see that the connection is blocked right after sending Client hello packet.
 I had tried some ways to bypass this, and found out that it seemed like only the first chunk gets inspected when we send the Client hello packet splited in chunks.
 What SpoofDPI does to bypass this is to send the first 1 byte of a request to the server,
 and then send the rest.

# Inspirations
[Green Tunnel](https://github.com/SadeghHayeri/GreenTunnel) by @SadeghHayeri
[GoodbyeDPI](https://github.com/ValdikSS/GoodbyeDPI) by @ValdikSS
