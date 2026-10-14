# CTF Bash Scripts 

<a id="overview"></a>
## Overview

Small Bash wrappers and helper tools for CTFs, pentesting, labs and security research.

### Why?

Because shell aliases are not easily chainable.

The main advantage of these tools is the ability to combine commands, especially when pivoting through one or more proxies. They avoid the need to repeatedly enter common commandline options.

Example:

```bash
PROXY 9050 CURL http://butzel.info/
```

or with chained proxies:

```bash
PROXY socks5 127.0.0.1 9050 \
    PROXY socks4 10.10.10.10 1080 \
    MIRROR https://web.butzel.lan/
```

These tools are designed around the KISS philosophy of simplicity.

---

<a id="index"></a>
## Whats in the box

### Proxying / Pivoting / Tor

- [PROXY](#proxy) - proxychains
- [PROXYDNS](#proxydns)
- [TOR2IP](#tor2ip) - redirect TOR service to 127.0.0.1 
- [TOR](#tor)

### SSH / Remote Access

- [SSH](#ssh)  - SSH
- [SCP](#scp)  - SCP
- [SSHSRV](#sshsrv)  - run a convenient SSH-SeRVer as a non-root user

### Networking

- [TCP](#tcp) - redirect a local port 
- [SSLCAT](#sslcat)  - netcat over SSL
- [SSLSRV](#sslsrv)  - netcat --listen over SSL
- [WIFI](#wifi) - scan for networks

### Sessions / Workflow / Automation

- [TMUX](#tmux) - TMUX with [predefined sessions](https://github.com/BearlyStable/tmux-sessions)
- [LOOP](#loop) - run a command for every line of a file



### Web / HTTP / TLS

- [CURL](#curl)  - ignore certs and set $BROWSER as useragent
- [WGET](#wget)  - ignore certs and set $BROWSER as useragent
- [MIRROR](#mirror)   
- [HTTPS-GET](#https-get)
- [GIT](#git)  - insecure usage of GIT
- [URLENCODE](#urlencode) - encode URL parameters

### Encoding / Passwords 

- [BASE64](#base64) - encode/decode base64: BASE64 YmFzZTY0 
- [BIN2SH](#bin2sh) - transfer a binary into shellscript
- [TOHEX](#tohex)  - transfer from string/file a hex-string 
- [FROMHEX](#fromhex) - convert from Hex a String: FROMHEX 63706531373034746b730a
- [JOHN](#john)
- [URLENCODE](#urlencode)

### Git

- [GIT](#git)  - insecure usage of GIT

### Serial / Hardware

- [XCOM](#xcom) - use screen for serial connections

### Configuration

- [Environment variables](#environment-variables)
- [Security notice](#security-notice)

---

<a id="tools"></a>
# Tools

<a id="base64"></a>
## BASE64

Display a value and try both Base64 encoding and decoding.

Usage:

```bash
BASE64 <value>
```

Example:

```bash
BASE64 "YnV0emVsLmluZm8K"
```

Back to [Index](#index).

---
<a id="bin2sh"></a>
## BIN2SH 

converts a binary into a ready-to-paste shell script.

Usage:

```bash
BIN2SH <bin-file> <blocksize> <targetfile>
```

Example:

```bash
BIN2SH /bin/nc 14 /tmp/nc 
```

Back to [Index](#index).

---

<a id="curl"></a>
## CURL

`curl` wrapper for CTF/lab environments.

- ignores TLS certificate validation errors
- if the env variable $BROWSER is set, its value is used as the User-Agent
- follows redirects

Usage:

```bash
CURL [curl-options] URL
```

Example:

```bash
CURL ip.butzel.net
```

Back to [Index](#index).

---

<a id="fromhex"></a>
## FROMHEX

an `xxd` wrapper converts a HEX-String back into binary (string/file)


Usage:

```bash
FROMHEX HEX_str_or_file 
```

Example:

```bash
FROMHEX 6275747a656c2e696e666f0a
```

```bash
FROMHEX /tmp/my/hex.file 
```


Back to [Index](#index).

---

<a id="git"></a>
## GIT

Git wrapper without TLS certificate verification.

Usage:

```bash
GIT <git-command> [arguments]
```

Example:

```bash
GIT clone  https://github.com/BearlyStable/tmux-sessions.git
```

Back to [Index](#index).

---

<a id="https-get"></a>
## HTTPS-GET

HTTPS GET request using `socat` (OpenSSL).

Usage:

```bash
HTTPS-GET <IP/HOSTNAME> [PORT] [URL]
```

Example:

```bash
HTTPS-GET target.local 443 robots.txt
```

Defaults:

```text
PORT = 443
URL  = robots.txt
```

Back to [Index](#index).

---

<a id="https-options"></a>
## HTTPS-OPTIONS

Send a simple HTTPS OPTIONS request using `socat` / OpenSSL.

Usage:

```bash
HTTPS-OPTIONS <IP/HOSTNAME> [PORT]
```

Example:

```bash
HTTPS-OPTIONS target.local 443
```

Default port:

```text
443
```

Back to [Index](#index).

---

<a id="john"></a>
## JOHN

Small John the Ripper helper for testing a single hash.

Usage:

```bash
JOHN <HASH> [WORDLIST] [john-options]
```

Examples:

```bash
JOHN '$6$...'
```

```bash
JOHN '$6$...' rockyou.txt
```

additional John options can be set after the hash or wordlist.

Back to [Index](#index).

---

<a id="loop"></a>
## LOOP
Execute a shell command for every line in a file.
  The current input line is available through `$DATA`.

Usage:

```bash
LOOP '<command>' <data-file>
```

Example:

```bash
LOOP 'nc -vvzwn1 $DATA 22 80 443 445 3389' listof.ips
```

The supplied command is evaluated by the shell for each line.

Back to [Index](#index).

---

<a id="mirror"></a>
## MIRROR

Mirror a (static) website.

Usage:

```bash
MIRROR <URL> [DEPTH]
```

Example:

```bash
MIRROR https://butzel.info/ 7
```

If `$BROWSER` is set, its value is used as the User-Agent.

TLS certificate validation errors are ignored and `robots.txt` restrictions are disabled.

Back to [Index](#index).

---

<a id="proxy"></a>
## PROXY

Run a command through `proxychains`.

Proxy definitions are chainable.

Usage:

```bash
PROXY <protocol> <proxyIP> <proxyPORT> <command> [arguments]
```

Example:

```bash
PROXY http localhost 3128 CURL https://ipinfo.io/
```

Example using SOCKS4:

```bash
PROXY socks4 23.95.95.102 3139 CURL https://ipapi.co/ip
```

### Local SOCKS5 shorthand

```bash
PROXY 9050 CURL https://target/
```

is equivalent to:

```bash
PROXY socks5 127.0.0.1 9050 CURL https://target/
```

### Chaining proxies

```bash
PROXY socks5 127.0.0.1 9050 \
    PROXY socks4 10.10.10.10 1080 \
    CURL https://target/
```

The generated `proxychains` configuration uses `strict_chain`.

Back to [Index](#index).

---

<a id="proxydns"></a>
## PROXYDNS

Same basic usage as [PROXY](#proxy), but DNS resolution is routed through the proxy chain.

SOCKS5 is the default proxy type.

Usage:

```bash
PROXYDNS <protocol> <proxyIP> <proxyPORT> <command> [arguments]
```

Local SOCKS5 shorthand:

```bash
PROXYDNS 9050 CURL https://target/
```

Use this variant when DNS leakage should be avoided.

Back to [Index](#index).

---

<a id="scp"></a>
## SCP

`scp` wrapper using the same basic SSH compatibility settings as [SSH](#ssh).

Usage:

```bash
SCP [scp-options] source destination
```

Example:

```bash
SCP -i key file user@target:/tmp/
```

Example with a non-default SSH port:

```bash
SCP -P 2222 file user@target:/tmp/
```

Back to [Index](#index).

---

<a id="ssh"></a>
## SSH

SSH wrapper intended for CTF/lab environments and temporary or incompatible systems.

Current defaults:

- disables SSH host-key checking
- disables known-host persistence
- ignores `~/.ssh/config`
- disables automatic IdentityFile selection
- disables the SSH agent
- keeps idle sessions alive
- still allows explicit identities using `-i`
- prefers `keyboard-interactive`, then `publickey`, then `password`

Usage:

```bash
SSH [ssh-options] host
```

Example:

```bash
SSH user@target
```

With explicit key:

```bash
SSH -i key -l user target
```

Back to [Index](#index).

---

<a id="sshsrv"></a>
## SSHSRV

Run a SSH-Server as an unprivileged user.

Usage:
```bash
SSHSRV -h
  -p | --port 	listening port
  -l | --ip    	listening IP (optional)
  -i | --cert 	hostkey-file (optional)
  -w | --wdir  	workdir (optional)      
  -u | --user 	username:shell:ed25519 (optional)

## Linux
 ssh-keygen -q -t ed25519 -N '' -f key
 ssh user@host -p port -i key
## Windows
 ssh-keygen -q -t ed25519 -f C:\path\2\key -N '""'
 icacls "C:\pfad\2\key" /inheritance:r /grant:r "$\($env:USERNAME\):R"
 ssh -i "C:\pfad\2\key" user@host -p port
```

Examples:

```bash
SSHSRV -p 4711  
```
```bash
SSHSRV -p 4711 -u btzl:/bin/bash:AAAAC3NzaC1lZDI1NTE5AAAAIIXr55AiEvjfr+x53pVIGZ8+aRwWRTp1O4SqZNePJTPW 
```

Back to [Index](#index).

---


<a id="sslcat"></a>
## SSLCAT

connect interactively to a TLS service using `socat`.

Usage:

```bash
SSLCAT <IP/HOSTNAME> <PORT>
```

Example:

```bash
SSLCAT 10.10.10.10 443
```

Output can be redirected normally:

```bash
SSLCAT 10.10.10.10 443 > output.txt
```

or piped:

```bash
SSLCAT 10.10.10.10 443 | tee output.txt
```

Back to [Index](#index).

---

<a id="sslsrv"></a>
## SSLSRV

Start a simple TLS listener using `openssl s_server` with a temporary automatically generated self-signed certificate.

Usage:

```bash
SSLSRV <PORT>
```

Example:

```bash
SSLSRV 4711
```

The script is verbose.

Back to [Index](#index).

---

<a id="tcp"></a>
## TCP

Redirect a local TCP port to another IP-Address using `socat`.

Usage:

```bash
TCP <LISTENPORT> <TARGETIP> <TARGETPORT>
```

IPv4 example:

```bash
TCP 8080 192.168.47.11 80
```

IPv6 example:

```bash
TCP 8080 2001:db8::10 80
```

The target address is automatically treated as IPv6 when it contains `:`.

Back to [Index](#index).

---

<a id="tmux"></a>
## TMUX

Small tmux session launcher.

Without arguments:

```bash
TMUX
```

lists existing tmux sessions.

Attach to or create a named session:

```bash
TMUX <session>
```

If a matching session already exists, it is attached.

If `~/.tmux/sessions.py` exists, the wrapper can use it to create predefined sessions before attaching.

Related project:

- [BarelyStable/tmux-sessions](https://github.com/BearlyStable/tmux-sessions)

Back to [Index](#index).

---

<a id="tohex"></a>
## TOHEX

`xxd` wrapper converts StringOrFile into a HEX-String 


Usage:

```bash
TOHEX <STRING/FILE> 
```

Example:

```bash
TOHEX "Hallo Welt" 
```

```bash
TOHEX /usr/bin/nc
```


Back to [Index](#index).

---

<a id="tor"></a>
## TOR

Check whether the current connection appears to originate from the Tor network.

Usage:

```bash
TOR
```

The check uses:

```text
https://check.torproject.org/api/ip
```

Back to [Index](#index).

---

<a id="tor2ip"></a>
## TOR2IP

Expose an TOR-Service as a local TCP port using Tor's SOCKS proxy.

Usage:

```bash
TOR2IP <TARGET:PORT> <LOCALPORT> [TORPORT]
```

Example:

```bash
TOR2IP somec00lserver.onion:80 4444 9050
```

Default Tor SOCKS port:

```text
9050
```

The connection is using `socat` with SOCKS4A so that the `.onion` hostname is resolved through Tor.

Back to [Index](#index).

---

<a id="urlencode"></a>
## URLENCODE

URL-encode a string.

Usage:

```bash
URLENCODE <text>
```

Example:

```bash
URLENCODE Hällo Wörld?
```

Back to [Index](#index).

---

<a id="wget"></a>
## WGET

`wget` wrapper for CTF/lab environments.

- ignores TLS certificate validation errors
- uses `$BROWSER` as User-Agent-Header if set

Usage:

```bash
WGET [wget-options] URL
```

Example:

```bash
WGET https://target/file.txt
```

Back to [Index](#index).

---

<a id="wifi"></a>
## WIFI

Scan for nearby Wi-Fi networks using `iwlist`.

Usage:

```bash
WIFI [interface]
```

Default interface:

```text
wlan0
```

Example:

```bash
WIFI wlan1
```

Back to [Index](#index).

---

<a id="xcom"></a>
## XCOM

Open a serial console using `screen`.

Supported devices:

```text
/dev/ttyUSB#
/dev/ttyACM#
```

Usage:

```bash
XCOM <device-number> [BAUDRATE]
```

Example:

```bash
XCOM 0 115200
```

Default baud rate:

```text
115200
```

Back to [Index](#index).

---

<a id="xrdp-attack"></a>
## xrdp.attack

Helper for iterating a `user:password` list against an RDP target using `xfreerdp`.

Usage:

```bash
xrdp.attack <userpass.list> <targetip> [targetport] [domain]
```

The input file is expected to contain entries in the form:

```text
username:password
```

Back to [Index](#index).

---

<a id="environment-variables"></a>
# Environment variables

## BROWSER

Some HTTP tools use the environment variable `$BROWSER` as User-Agent.

Example:

```bash
export BROWSER="Mozilla/5.0 ..."
```

Affected tools include:

```text
CURL
WGET
MIRROR
```

Back to [Index](#index).

---

<a id="security-notice"></a>
# Security notice
Some wrappers intentionally disable or weaken mechanisms.
These tools are intended for:
- CTFs
- labs
- security research
- temporary or intentionally untrusted systems
- fun


Do not use these defaults for normal administration of trusted production systems.

Back to [Index](#index).
