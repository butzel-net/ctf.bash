# CTF - Bash 

### why ?
Because Aliases are not chainable.
 The main benefits are the ability to pivot (PROXY) and quicker usage, as you no longer need to type out recurring parameters."
## Installation
```
mkdir -p $HOME/bin/
rm README.md ; chmod +x *
mv * $HOME/bin/.
echo 'PATH="$HOME/bin:$PATH"' >> $HOME/.bashrc
echo "export BROWSER='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36 Edg/137.0.0.0'"  >> $HOME/.bashrc
```
## USAGE
 * BASE64        -> Execute base64 with input parameter: `BASE64 <your_value_here>`
 * CURL          -> Bypass SSL verification / ignore self-signed certificates ($BROWSER sets the User-Agent)
 * HTTPS-GET     -> HTTPS GET request via socat/openssl: `HTTPS-GET <IP/Hostname> [PORT] [URL]` 
 * HTTPS-OPTIONS -> HTTPS OPTIONS request via socat: `HTTPS-OPTIONS <IP/Hostname> [PORT]`  
 * MIRROR        -> Mirror a website: `MIRROR <URL> [DEPTH]`
 * PROXY         -> Execute command via proxychains (chainable)
   * usage:   `PROXY <protocol> <proxyIP> <proxyPORT> <command> [args]`
   * example: `PROXY http localhost 3128 curl ipinfo.io`
   * example: `PROXY socks4 23.95.95.102 3139 CURL https://ipapi.co/ip`
   * example: `PROXY 9050 curl -q https://check.torproject.org/api/ip` # (shortcut for socks5 127.0.0.1 9050)
   * example: `PROXY 9050 PROXY socks4 23.95.95.102 3139 CURL https://ipapi.co/ip` # Chain of 2 proxies
 * PROXYDNS      -> Execute command via proxychains preventing DNS leaks (requires socks5); see PROXY
 * SSH           -> SSH wrapper to bypass errors on legacy/incompatible systems
 * SCP           -> SCP wrapper to bypass errors on legacy/incompatible systems
 * TOR           -> Verify active Tor routing
 * TOR2IP        -> Map a localhost port to an onion server port
 * WGET          -> Bypass SSL verification / ignore self-signed certificates ($BROWSER sets the User-Agent)
 * XCOM          -> Serial communications via /dev/ttyUSB# or /dev/ttyACM#: `XCOM <device-number> [BAUDRATE]`
 
## Variables:
  * `export BROWSER=<USER-AGENT-STRING>` for CURL, WGET, MIRROR

### Dependencies:
- Gentoo/Pentoo: 
  * `emerge -q net-misc/curl app-crypt/johntheripper net-misc/proxychains net-analyzer/netcat app-misc/screen net-misc/socat  net-misc/openssh net-misc/wget`
- Debian based:
  * `apt install -y curl john proxychains4  netcat-traditional  screen socat openssh-client wget `

### GUI-Starter
 * FIREFOX -> start firefox with profile selection
