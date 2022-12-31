---
categories:
- blog
date: 2021-10-03 13:00:00+02:00
header:
  og_image: /assets/images/a-little-less-conspicuous-introduction-part1-4-001.png
last_modified_at: 2021-10-03 13:00:00+02:00
tags:
- ObscureDnsTraffic
- TorProxy
- DnsTraffic
- OwnTorProxy
- OwnDnsPi
- PiHoleDnsServer
- PowerfulDnsService
- TorBrowser
- TorNetwork
- Tor
- OwnTorSocks5Proxy
- OnionRouter
- DnsSinkhole
- DnsLeak
- Marketing
- SurveillanceCapitalism
- Awareness
- Security
- Privacy
- Anonymity
- HyperV
- DietPi
- DNS
- Pihole
- DNSMasq
- Unbound
- DoT
- DNS-over-TLS
- Privoxy
title: 'A little less conspicuous - Part 4: Obscuring DNS traffic'
variables:
  ExistingADDomain: mydomain.local
  ExistingADDomainDNSServer: 192.168.0.10
  ServerIP1: 192.168.0.50
  ServerIP2: 192.168.0.51
  ServerName1: DNS-V1-01
  ServerName2: TOR-V1-01
---

This is the final follow-up article for [A little less conspicuous - Part 1: Introduction][1]. Please read that first, if you came here through a direct link.

Also it is recommended to read the previous parts of the series as well, before reading this post.

![A little less conspicuous](/assets/images/a-little-less-conspicuous-introduction-part1-4-001.png "A little less conspicuous"){: style="border: 1px solid grey; padding:16px"}

You have come to the pinnacle of this series and we are now going to tie together all that you have learned in previous episodes.

While using the Pi-hole DNS Server (Part 2) and the Tor Proxy (Part 3) separately is totally valid, why not go the extra mile and fully secure and anonymize your DNS traffic on a permanent basis and at the same time configure your Tor Browser ([Download][12]) to use your Tor Proxy and the Pi-hole together.

"Why go through all this trouble to hide only this specific (DNS) portion of my traffic?" you might ask. I will just have to refer you to the title and opening statement in Part 1 of this series. Be 'A little less conspicuous' and 'It really wouldn't harm you, or anyone for that matter and makes just plain sense.'

Nonetheless, we can say that the general **security**, **privacy** and even **anonymity** level of at least some of your internet interactions will be raised by using a permanent DNS-over-TLS-over-Tor (DoToT) set-up on your local network. Also you have the option to totally 'emerge' yourself by using the Tor Browser configured with your Tor Proxy.

And now for something very important...

**:warning: Disclaimer** \
This article and follow-ups have no ambitions to totally anonymize anyone on the internet, but just to make it a little bit more difficult for certain parties (ie. ISP's, governments, advertising moguls and the likes) to track you or your small company. \
\
The proposed setup is meant for demo purposes only and is certainly not production-ready without extra precautions and explanations put in place. \
\
The packages, distributions and commands used and referred to in this article are valid for the most recent software versions available at the date of article publication. There is no guarantee the routines and packages described here will keep working at a later date.\
\
Furthermore rest assured, since the internet (no capital here!) was created with absolutely no security or privacy in mind, it will never deliver on any promise of these concepts. But why hand-over all your data on a platter and be profiled? It's like peeing naked in the middle of Grand Central Station and have everyone stare at you, would you really? At least put on a dress and pee in a bucket underneath you, doing that some people won't even notice you are there. \
\
This article is here to provide you a bucket. Please bring your own dress.
{: .notice--warning}

## Machine variables

The following variables will be used in the guidance offered by this article.

The names of the machines have already been used in Part 2 and 3 of this series to set-up the individual machines.

| Variable                  | Example Value                                  |
| ------------------------- | ---------------------------------------------- |
| ServerName1               | {{ page.variables.ServerName1 }}               |
| ServerIP1                 | {{ page.variables.ServerIP1 }}                 |
| ServerName2               | {{ page.variables.ServerName2 }}               |
| ServerIP2                 | {{ page.variables.ServerIP2 }}                 |
| ExistingADDomain          | {{ page.variables.ExistingADDomain }}          |
| ExistingADDomainDNSServer | {{ page.variables.ExistingADDomainDNSServer }} |

## Setting up DNS-over-TLS-over-Tor (DoToT)

Please refer to the **red** textual markings in the [architecture diagram](/blog/post-a-little-less-conspicuous-introduction-part1-4/#full-architecture-to-be-achieved) laid out in Part 1 of this series to get a grip on what you are trying to achieve.

* Configure DietPi Software:
  * On **{{ page.variables.ServerName1 }}**
    * Read the article: [DNS over TLS and TOR with Pi-Hole][5] as a reference, but substitute 'stubby' with 'unbound'
    * Login to your machine as `root`
    * Install ProxyChains: `sudo apt-get install proxychains`
      * **ProxyChains** (wrapping specific programs to use a proxy)
        * Change the default config as follows: `sudo nano /etc/proxychains.conf`
          * Comment out the line `proxy_dns` so that it appears like this: `# proxy_dns`
          * At the end of the file replace the line starting with `socks4` with `http {{ page.variables.ServerIP2 }} 9051`
        * Check the workings (Should return different IP addresses):
          * `proxychains curl ifconfig.me`
          * `curl ifconfig.me`
    * Reconfigure Unbound (to use ProxyChains)
      * **Unbound** (Recursive Caching DNS Server)
        * Stop the Unbound service
          * `sudo systemctl stop unbound.service`
        * Change the unbound config so it starts up through ProxyChains
          * `sudo nano /etc/systemd/system/multi-user.target.wants/unbound.service`
            * Change the ExecStart line to be `ExecStart=/usr/bin/proxychains /usr/bin/unbound -d -p $DAEMON_OPTS`
        * Speed up regular performance by upping the TTL for Unbounds cached DNS entries
          * `sudo nano /etc/unbound/unbound.conf.d/dietpi.conf`
            * Change the line starting with `cache-min-ttl` to `cache-min-ttl 3500`
        * Check unbound config
          * `unbound-checkconf` (Should return no errors)
        * Start the unbound service
          * `sudo systemctl start unbound.service`
    * Check full workings
      * First execution should take several seconds, Second execution should return instantly:
        * `dig @127.0.0.1 -p 5335 google.to`
        * `dig @127.0.0.1 -p 5335 google.to`
      * `dietpi-services`
        * Status -> Log should show connections to **{{ page.variables.ServerIP2 }}**
  * On **{{ page.variables.ServerName2 }}**
    * Login to your machine as `root`
    * Reconfigure Privoxy
      * **Privoxy** (HTTP Proxy)
        * Replace the following line in the default config file (`sudo nano /etc/privoxy/config`)...

        ```conf
        # forward to Tor (mind the dot at the end!)
        forward-socks5 / 127.0.0.1:9050 .
        ```

        * ...with the following

        ```conf
        ## forward to Tor, but forward DNS traffic to Pi-hole (mind the dots at the end of each line!)
        forward-socks4 / 127.0.0.1:9050 .       # HTTP traffic goes through Tor, no DNS handling
        forward-socks4 :443 127.0.0.1:9050 .    # HTTPS traffic goes through Tor, no DNS handling
        forward-socks4 :853 127.0.0.1:9050 .    # TLS traffic goes through Tor, no DNS handling
        forward-socks4 :53 {{ page.variables.ServerIP1 }}:53 .           # DNS traffic is forwarded to local DNS (Pi-hole)
        forward-socks4a .onion 127.0.0.1:9050 . # .onion domains go through Tor
        forward-socks4a .exit 127.0.0.1:9050 .  # .exit domains go through Tor
        ```

        * This configuration will make sure that you effectively create your own controlled version of a 'DNS leak', but since all DNS traffic (except request for the obvious Tor hidden services: .onion and .exit) will be forwarded from the Pi-hole to our Unbound forwarders using TLS, which will in turn be forwarded through Tor as well, this potential leak is remediated. The target DNS resolvers (configured in Unbound) will not be able to track you.
      * `reboot`
    * Check full workings
      * Run `ss`
        * Connections from **{{ page.variables.ServerIP1 }}** should be present
  * Configure owned LAN/WAN services and devices
    * **Recommended**: On any networked client with the Tor Browser:
      * Download this [zip file][10] to employ [Firefox Auto Configuration][11] for the Tor Browser.
      * Read the 'Instructions.txt' file in the zip file and place the mentioned packed files in their proper locations.
        * IMPORTANT: You might need to edit the 'firefox.cfg' file and amend the IP addresses used for the Tor Proxy (multiple places)
      * Open the Tor Browser
      * Surf to: [DuckDuckGo .onion](https://duckduckgogg42xjoc72x3sjasowoarfbgcmvfimaftt6twagswzczad.onion/) and [DuckDuckGo 'regular'](https://duckduckgo.com/). Both should resolve correctly
        * Fantastic! You now have effectively disabled the Tor Browser built-in SOCKS5 Proxy and replaced it with your own Tor Proxy keeping all other Tor Browser default secure settings and extensions in place, additionally all DNS traffic is being routed through your Pi-hole first, before being resolved through Tor.
    * Optional, but recommended: Disable 'Outgoing WAN connections' using ports 53 and 853 (TCP/UDP) for **all** devices on your network in your firewall.
      * Since we now send all DNS traffic to the Pi-hole and unknown domains will get forwarded as TLS traffic over Tor by the ProxyChains + Unbound set-up, outgoing DNS ports are not used anymore on your network and can thus be blocked.
      * NOTE: Maybe have a single machine exception in your firewall, in case any one of your machines (**{{ page.variables.ServerName1 }}** and **{{ page.variables.ServerName2 }}**) 'die' on you.

On any Tor Proxy connected machine in your network, check if your DNS Configuration works correctly:

* => [https://dnsleaktest.com][6]
  * Check against DNS-leaks and what DNS-server you currently use, these should be the ones configured as resolvers in Unbound
* => [https://cmdns.dev.dns-oarc.net][7]
  * Check your DNS features
* => [http://www.dnssec-or-not.com][8]
  * If you use DNSSEC
* => [https://tools.dnsstuff.com][9]
  * DNS-Tools and more

**:heavy_check_mark: Congratulations** \
You now have successfully configured a DNS-over-TLS-over-Tor chain and your networked devices/browsers that were already configured to use the Pi-hole DNS server will automatically start using this configuration! Furthermore if you use your Tor Proxy actively all DNS traffic will be routed to ypur Pi-hole *before* entering the Tor network for resolving as already encrypted TLS traffic.
{: .notice--success}

This marks the end of this series, I hope you found it useful.

## The end

This series comprises of 4 parts:

1. Part 1: [A little less conspicuous - Introduction][1]
2. Part 2: [A little less conspicuous - The DNS Sinkhole][2] on running your own DNS Pi-hole with secure Admin WebUI + Unbound + TLS resolvers on DietPi
   * Requirements: None, can run Standalone
   * Achieves: General Ad blocking for your network + Obscure DNS traffic for your ISP by using DNS-over-TLS resolvers
3. Part 3: [A little less conspicuous - The Tor Proxy][3] on running your own Tor SOCKS5 Proxy + Privoxy HTTP Proxy on DietPi
   * Requirements: None, can run Standalone
   * Achieves: Have the option to 'go anonymous' on the web when you want it, also for mobile devices (iOS, Android)
4. **This post** - Part 4: [A little less conspicuous - Obscure DNS traffic][4] on routing your DNS Traffic through Tor
   * Requirements: Parts 2 and 3
   * Achieves: Obscure already Secure DNS-over-TLS Traffic from your DNS resolvers + Use Tor Browser and Pi-hole together

<!-- Begin FootNotes -->

<!-- End FootNotes -->

<!-- Begin Abbreviations -->
*[PII]: Personal Identifiable Information is comprised of any representation of information that permits the identity of an individual to whom the information applies to be reasonably inferred by either direct or indirect means.
*[ISP]: An Internet Service Provider (ISP) is a company that provides Internet access to organizations and home users.
*[denial of service]: The act of sending so many data packets to an internet service that it crashes.
*[spoofing]: Posing to **be** a certain party in a chain of communications and **act** (usually fraudulently) on behalf of that party having the chain believe it was in fact the original party.
*[cookies]: a persistent and/or reusable data asset meant to store application data and/or identity information of a network/device/browser.
*[DNS]: The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through domain names, like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources.
*[NTP]: The Network Time Protocol (NTP) is a networking protocol for clock synchronization between computer systems over packet-switched, variable-latency data networks.
*[DHCP]: Dynamic Host Configuration Protocol (DHCP) is a client/server protocol that automatically provides an Internet Protocol (IP) host with its IP address and other related configuration information such as the subnet mask and default gateway. RFCs 2131 and 2132 define DHCP as an Internet Engineering Task Force (IETF) standard based on Bootstrap Protocol (BOOTP), a protocol with which DHCP shares many implementation details. DHCP allows hosts to obtain required TCP/IP configuration information from a DHCP server.
*[TLS]: Transport Layer Security (TLS) is a cryptographic protocol that provides end-to-end security of data sent between applications over the Internet.
*[DietPi]: Debian based Linux distribution, stripped to the bare essentials. DietPI allows you to easily install optional "ready to run" and optimized software for your device. As well as automating the Linux installation and configuration process for you, DietPi-Software also applies specific optimizations unique to your hardware model, ensuring you get the maximum performance from your device.
*[Pi-hole]: Pi-hole is a Linux network-level advertisement and Internet tracker blocking application which acts as a DNS sinkhole and optionally a DHCP server, intended for use on a private network.
*[Lighttpd]: Security, speed, compliance, and flexibility -- all of these describe lighttpd (pron. lighty) which is rapidly redefining efficiency of a webserver; as it is designed and optimized for high performance environments.
*[Unbound]: Unbound is a simple but powerful DNS service that you can install, set up, and manage yourself.
*[Tor]: Tor, short for The Onion Router, is free and open-source software for enabling anonymous communication. It directs Internet traffic through a free, worldwide, volunteer overlay network, consisting of more than six thousand relays, for concealing a user's location and usage from anyone conducting network surveillance or traffic analysis. Using Tor makes it more difficult to trace the Internet activity to the user. Tor's intended use is to protect the personal privacy of its users, as well as their freedom and ability to conduct confidential communication by keeping their Internet activities unmonitored.
*[Privoxy]: Privoxy is a non-caching web proxy with advanced filtering capabilities for enhancing privacy.
*[CA]: A certification authority (CA) is responsible for attesting to the identity of users, computers, and organizations. The CA authenticates an entity and vouches for that identity by issuing a digitally signed certificate. The CA can also manage, revoke, and renew certificates.
*[FQDN]: A fully qualified domain name (FQDN), sometimes also referred to as an absolute domain name, is a domain name that specifies its exact location in the tree hierarchy of the Domain Name System (DNS). It specifies all domain levels, including the top-level domain and the root zone.
*[DoT]: DNS over TLS (DoT) is a network security protocol for encrypting and wrapping Domain Name System queries and answers via the Transport Layer Security protocol. The goal of the method is to increase user privacy and security by preventing eavesdropping and manipulation of DNS data via man-in-the-middle attacks.
*[DoToT]: DNS over TLS over Tor
*[DoH]: DNS over HTTPS is a protocol for performing remote Domain Name System resolution via the common HTTPS protocol. A goal of the method is to increase user privacy and security by preventing eavesdropping and manipulation of DNS data by man-in-the-middle attacks by using the HTTPS protocol to encrypt the data between the DoH client and the DoH-based DNS resolver.
*[IaC]: Infrastructure as Code (IaC) is the management of infrastructure (networks, virtual machines, load balancers, and connection topology) in a descriptive model, using the same versioning as DevOps team uses for source code.
*[DNSSEC]: The Domain Name System Security Extensions is a suite of extension specifications by the Internet Engineering Task Force for securing data exchanged in the Domain Name System in Internet Protocol networks.

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: /blog/post-a-little-less-conspicuous-introduction-part1-4/
[2]: /blog/post-a-little-less-conspicuous-dns-part2-4/
[3]: /blog/post-a-little-less-conspicuous-tor-part3-4/
[4]: /blog/post-a-little-less-conspicuous-dns-over-tls-over-tor-part4-4/
[5]: https://github.com/piskyscan/dns_over_tls_over_tor#install-proxychains
[6]: https://dnsleaktest.com
[7]: https://cmdns.dev.dns-oarc.net
[8]: http://www.dnssec-or-not.com
[9]: https://tools.dnsstuff.com
[10]: /assets/docs/a-little-less-conspicuous-dns-over-tls-over-tor-part4-4_tor-browser-config.zip
[11]: https://support.mozilla.org/en-US/kb/customizing-firefox-using-autoconfig
[12]: https://www.torproject.org/download/

<!-- End References -->
