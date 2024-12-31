# CTF - Bash Settings

### why ?
Because Aliases are not chainable.

The advantage should result by pivoting (PROXY) and the fast use without having to re-enter recurring parameters.



## USAGE
 * BASE64 -> base64 with input parameter: `BASE64 <your_value_here>`
 * CURL -> no nagging about selfsigned certificates  ($BROWSER sets the User-Agent)
 * HTTPS-GET -> HTTPS-GET with socat/openssl: `HTTPS-GET <IP/Hostname> [PORT] [URL]` 
 * HTTPS-OPTIONS -> HTTPS-OPTIONS with socat: `HTTPS-OPTIONS <IP/Hostname> [PORT] `  
 * MIRROR -> MIRROR a website: `MIRROR <URL>  [DEPTH]`
 * PROXY -> run command with proxychains (chainable)
   * usage: ` PROXY <protocol> <proxyIP> <proxyPORT> <command> [-with args] `
   * example: ` PROXY http localhost 3128 curl ipinfo.io`
   * example: ` PROXY socks4 23.95.95.102  3139 CURL https://ipapi.co/ip`
 * PROXYDNS -> run command with proxychains without DNS-leakage (needs socks5); see PROXY
 * SSH -> ssh without pain on incompatible systems
 * SCP -> scp without pain on incompatible systems
 * WGET -> no nagging about selfsigned certificates  ($BROWSER sets the User-Agent)
 * XCOM -> Serial Communictions via /dev/ttyUSB# or /dev/ttyACM#:  `XCOM <device-number> [BAUDRATE]`

