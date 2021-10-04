---
title: "A little less conspicuous - Part 3: The Tor Proxy"
date: 2021-10-02T12:00:00+02:00
last_modified_at: 2021-10-02T12:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/tor-privoxy.png
categories:
  - blog
tags:
  - Marketing
  - SurveillanceCapitalism
  - Awareness
  - Security
  - Privacy
  - Anonymity
  - HyperV
  - DietPi
  - Tor
  - Privoxy
  - Proxy
variables:
  ServerName2: TOR-V1-01
  ServerIP2: 192.168.0.51
  ExistingADDomain: mydomain.local
  ExistingADDomainDNSServer: 192.168.0.10
---

This is a follow-up article for [A little less conspicuous - Part 1: Introduction][1]. Please read that first, if you came here through a direct link.

Also it is recommended to read the previous parts of the series as well, before reading this post.

![Tor](/assets/images/tor-privoxy.png "Tor"){: style="border: 1px solid grey; padding:16px"}

Setting up a Tor Proxy[^1] is not something to rush into and you really have to familiarize yourself with some of the gritty aspects of consuming a Tor service. Also Tor is regarded as the domain of the 'not-so-savory' by some. I say: it's *just* a service which you should have in your arsenal when browsing, but expect it to be compromised or unavailable at any time, so do not depend on it. Nonetheless it is a marvelous tool for anonymizing (some of) your internet traffic and at least mess up some ad/tracking agents following your every move, if that is your aim and you use it right. Tor can do a lot more btw, but this article is not about that.

:pencil2: **Did you know...** \
...that Tor was actually created by the US Government Agency called U.S. Naval Research Lab (NRL)[^2]
{: .notice--info}

"Why use Tor for anything?", you might ask. Well, at the very least employing the services of a Tor Proxy in your network, which is what the 'Tor' package is and does, allows you to surf the web a bit *more privately*, **when you need it** on connected devices/browsers. Mind you that I did not say 'surf the web *more securely*', because although everything that goes on *within* the Tor network is encrypted and hidden from anyone's single view, this also entails that malicious actors could hide at any corner, especially when your traffic *leaves* the Tor network at the so called exit nodes but entry nodes could also be hijacked. These actors could possibly intercept and/or manipulate your traffic. Still not something to really worry about unless you actually are of the 'unsavory' persuasion or live in a country with an oppressive regime.

Just last week I stumbled on a [great article from SOC Investigation][6] with some more in-depth views on the matter which is 'Tor' Architecture and its security implications.

:pencil2: **Important credo** \
"When using Tor: Sent well encrypted traffic in -> Get safe and anonymized traffic out."
{: .notice--info}

A good read on what to do and, better yet, what **not** to do while using Tor is [provided by Fossbytes][5]. A few of those points that caught my attention and I divided them up into 'protect yourself' (most important!) and 'hide yourself' aspects.

* **Security aspects** to consider while using Tor (Highly important to adhere to!)
  * Do not use services that use unencrypted protocols like FTP, POP3, IMAP, SMTP, HTTP sites, etc. Especially when those services require authentication or require form data containing any personal data. The Tor network will not magically encrypt your data when it leaves the Tor network and hits the intended internet service endpoint. It will only hide the originating source IP address (ie. your location) from any intermediary machine in the Tor chain except the entry node you are connected to.
  * Do not use SMS/text based 2FA service while using Tor, you could expose your telephone number (Authenticator Apps are usually fine, when you use them for accounts that have totally fictitious user data)
* **Anonymization aspects** to consider while using Tor (I personally don't really care that much about this, I'll settle for making it a little more difficult for any third party to piece my internet footprint together)
  * Use only the Tor Browser when using Tor, as this will prevent PII leakage through the Tor network. Just using regular Private/Incognito browsers might not be sufficient to maintain anonymity. (That being said, the remainder of this article will allow you to do just this: Use Tor on any device)
  * Adopt an entirely different identity for yourself while using Tor. For instance do not use the same social media credentials as you do outside Tor. It's even better to use entirely different social media and totally different sites all together than you normally would.
  * Do not download large files, use a torrent program or watch streaming video over Tor as the lengthy connections will make your location far easier to ascertain by interested parties. It is also very slow.

Nonetheless, we can say that the general **privacy** level of your internet interactions is raised considerably by using a Tor Proxy. At the same time it is definitely not a good idea to use Tor for all your internet traffic all the time. For instance you will notice a great increase in the number of re-Captchas you need to fill out during your daily browsing and there comes a point that clicking your thirtieth 'airplane' picture of the day simply isn't worth the hassle. And I'll say it again, using Tor is not a golden ticket for anything.

:pencil2: **'On demand' Tor usage example** \
One great 'on demand' Tor usage example is *checking* if your favorite airline ticket discounter site promotes airline tickets in other countries with discounted rates and then go stalk them about wanting to get the same price. Make a screenshot of the page when your Tor 'exit'-node happens to be in, let's say Madagascar. Please try to resist the urge of using your credit card details when actually using Tor if 'anonymity' is your goal or it would defeat the purpose. However 'security-wise' you can probably pay safely if the site uses HTTPS and your payment provider uses some form of Two/Multi Factor Authentication (SMS methods exempted).
{: .notice--info}

:pencil2: **Teaser - 'Permanent' Tor usage example** \
So although 'on demand' usage is recommended, we can use Tor **all the time** for the sole purpose of delivering us 'DNS-over-TLS'(DoT) data over Tor (aka DoToT). The already encrypted, 'small package' TLS traffic is a very good candidate to permanently route over Tor, which is what Part 4 of this series will get in to.
{: .notice--info}

"Why not use a VPN instead of this Tor business?" is also a very good question. As it stands, commercially available VPN providers suffer the exact same problem public DoH services have - and then some. While using DoH services you *only* send DNS queries to Google or another (single?) third party to feast on, with a VPN you are potentially sending **all** your traffic through a single party. You should feel *really* trusting towards such a party before going down this path, me thinks. \
What you *could* do however, is sending *only* your Tor traffic over such a commercially available VPN service. In fact you are then tunneling Tor-over-VPN and effectively hiding your IP address from prying eyes, except the VPN provider itself of course.

**:pencil2: Important credo** \
"If hiding your IP address from your ISP or Tor entry nodes seems prudent -> Use Tor in combination with a commercially available VPN provider. Despite their trusting privacy statement you should still send **only** Tor traffic over this VPN."
{: .notice--info}

Let us now commence with installing our Tor Proxy.

But first turn your attention to something very important...

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

| Variable                  | Example Value                                  |
| ------------------------- | ---------------------------------------------- |
| ServerName2               | {{ page.variables.ServerName2 }}               |
| ServerIP2                 | {{ page.variables.ServerIP2 }}                 |
| ExistingADDomain          | {{ page.variables.ExistingADDomain }}          |
| ExistingADDomainDNSServer | {{ page.variables.ExistingADDomainDNSServer }} |

## Finishing the initial 'clean' DietPi installation

If you followed Part 2 as well, the following assumes you use a **separate DietPi instance** (ie. a second machine) to setup the Tor HTTP Proxy.

1. Initiate the DietPI installation described in [Part 1: Introduction](/blog/post-a-little-less-conspicuous-introduction-part1-4#dietpi-initial-installation) and continue from the `dietpi-config` screen
  ![DietPi-Config](/assets/images/screen20210925004.png "DietPi-Config"){: style="border: 1px solid grey; padding:16px"}
  *Figure 1: `dietpi-config`*
2. Choose your **timezone** (Language/Regional Options / Timezone)
3. Set up your **keyboard** (Language/Regional Options / Keyboard)
   * IMPORTANT: make absolutely sure you choose the right keyboard layout here, since we will be changing passwords of the system later on and you can not see the characters typed. You might end up with an unexpected password (mine was 'Gen 105 Key with US INTL with deadkeys' btw)
4. Change your **hostname** to something meaningful (Security Options / Change Hostname)
   * I use the name **{{ page.variables.ServerName2 }}** to denote this machine.
5. Set up your **network** (Network Options: Adapters)
   * Switch from DHCP to **Static** (use 'Copy current address to static'). A static address is needed for server operations.
     * I use the address **{{ page.variables.ServerIP2 }}** to denote this machine
   * Switch **IPv6** to 'off', it is not needed. (Network Options: Adapters / IPv6)
   * Make sure you are connected (Network Options: Adapters / Test)
6. Leave the "DietPi-Config" screen
7. If you are prompted to reboot: do so
8. Finish the initial install by running '**Install**' on the `dietpi-software` screen
  ![DietPi-Software](/assets/images/screen20210925003.png "DietPi-Software"){: style="border: 1px solid grey; padding:16px"}
  *Figure 2: `dietpi-software`*
9. Change default global password, when prompted
10. Change passwords for accounts (root and dietpi), when prompted
    * NOTE: These passwords should be different and make a note of them in your password vault.

**:heavy_check_mark: Congratulations** \
You now have a running instance of a DietPI Linux distribution!
{: .notice--success}

## Setting up the Tor HTTP Proxy Machine

* Install and Configure DietPi Software:
  * Login to your machine as `root`
  * Install Tor: `sudo apt install tor`
    * **Tor Proxy** (SOCK5 Proxy)
      * Follow: <https://www.maketecheasier.com/set-up-tor-proxy-with-raspberry-pi/>
      * Make sure the default config (`sudo nano /etc/tor/torrc`) contains the following:

      ```conf
      SocksPort 9050
      Log notice file /var/log/tor/notices.log
      RunAsDaemon 1
      DataDirectory /var/lib/tor
      ```

      * Restart service: `sudo systemctl restart tor*`
      * Check service status: `systemctl status tor*`
        * Should have no errors and 'Active: active' for both Tor services
      * You now have a SOCKS5 proxy running for Tor
  * Install Privoxy: `sudo apt-get install privoxy`
    * **Privoxy** (HTTP Proxy)
      * Follow: <https://neoctobers.readthedocs.io/en/latest/rpi/socks_to_http.html>
      * Make sure the default config (`sudo nano /etc/privoxy/config`) contains the following:

      ```bash
      confdir /etc/privoxy
      logdir /var/log/privoxy
      actionsfile match-all.action # Actions that are applied to all sites and maybe overruled later on.
      actionsfile default.action   # Main actions file
      actionsfile user.action      # User customizations
      filterfile default.filter
      filterfile user.filter      # User customizations
      logfile logfile

      # listen on 9051
      listen-address  {{ page.variables.ServerIP2 }}:9051

      toggle  1
      enable-remote-toggle  0
      enable-remote-http-toggle  0
      enable-edit-actions  0
      enforce-blocks  0

      # permit access from LOCAL
      permit-access  127.0.0.0/8

      # permit access from LAN
      permit-access  192.168.0.0/16

      buffer-limit  4096
      enable-proxy-authentication-forwarding  0

      # forward to Tor (mind the dot at the end!)
      forward-socks5 / 127.0.0.1:9050 .

      forwarded-connect-retries  0
      accept-intercepted-requests  0
      allow-cgi-request-crunching  0
      split-large-forms  0
      keep-alive-timeout  5
      tolerate-pipelining  1
      socket-timeout  300
      max-client-connections  128
      ```

      * The Privoxy service will initially fail on every reboot of the machine because its service is not managed by DietPi and as such Privoxy will try to start before network connections are activated. Fix this as follows:
        * Start `dietpi-services`
          * Select 'Add'
          * Type: `privoxy`
          * Now the Privoxy service should be visible in the menu. Open it
          * Configure its 'Mode' to be 'DietPi controlled'
        * `reboot`
      * You now have a HTTP Proxy configured with exposed port 9051 which will pass all traffic through the Tor SOCKS5 proxy and works on reboot. However when using this proxy it will not send DNS traffic over the local Pi-hole. We will fix this when you read Part 4 in this series.

* Configure owned LAN/WAN services and devices to use the HTTP/HTTPS Proxy **{{ page.variables.ServerIP2 }}:9051**
  * **Testing**: Adjust the HTTP Proxy configuration on **{{ page.variables.ServerName2 }}** (the Localhost) on OS level
    * Start `dietpi-config` -> Network Options: Adapters -> Proxy
    * Reboot when prompted
    * On login
      * Run `wget https://duckduckgo.com/assets/logo_header.alt.v108.svg` and watch for the PROXY message
    * On success: Revert the default proxy settings
    * Reboot when prompted
  * **Recommended**: On any networked client, Install the Tor Browser ([Download][7]).
    * Open the Tor Browser
      * IMPORTANT: Do not try to fiddle in your new Tor Proxy settings just yet, as that will lead to Tor-over-Tor traffic, which does not work. Part 4 of this series will explain how to use your own Tor Proxy with the Tor Browser.
    * Surf to: [DuckDuckGo .onion](https://duckduckgogg42xjoc72x3sjasowoarfbgcmvfimaftt6twagswzczad.onion/) and [DuckDuckGo 'regular'](https://duckduckgo.com/). Both should resolve correctly
      * You are now using the Tor Browser built-in SOCKS5 Proxy to connect to Tor. Too bad Pi-hole is not being used to filter out malicious/advertising sites... And why did I just set up that Tor Proxy on a separate machine then? -> Hang on, Part 4 will come to the rescue!
  * **Optional** (not recommended if you value the privacy aspect more than just testing this out): On any networked client, start 'plain' Firefox:
    * IMPORTANT: executing the steps below is especially not recommended to try out if you use unsecure internet services (like the ones mentioned in the ‘Security aspects’ bullet list above)
    * Use 'private' mode only
    * Change the advanced configuration `about:config` as follows
      * network.proxy.http -> {{ page.variables.ServerIP2 }}
      * network.proxy.ssl -> {{ page.variables.ServerIP2 }}
      * network.proxy.http_port -> 9051
      * network.proxy.ssl_port -> 9051
      * network.proxy.type -> 1
      * network.proxy.no_proxies_on -> .{{ page.variables.ExistingADDomain }}, 192.168.0.0/16
      * network.dns.disabled -> false
      * network.dns.blockDotOnion -> false
      * network.trr.mode -> 5
      * network.dns.disableIPv6 -> true
      * browser.fixup.alternate.enabled -> false
      * browser.urlbar.formatting.enabled -> false
      * browser.urlbar.trimURLs -> false
      * dom.securecontext.whitelist_onions -> true
    * Surf to: [DuckDuckGo .onion](https://duckduckgogg42xjoc72x3sjasowoarfbgcmvfimaftt6twagswzczad.onion/) and [DuckDuckGo 'regular'](https://duckduckgo.com/). Both should resolve correctly
      * Congratulations, you are now using your Tor Proxy to connect to Tor. Too bad Pi-hole is not being used to filter out malicious/advertising sites while using Tor... But read on!
  * **Optional** (not recommended if you value the privacy aspect more than just testing this out): On an Android or iOS device
    * IMPORTANT: executing the steps below is especially not recommended to try out if you use unsecure internet services (like the ones mentioned in the 'Security aspects' bullet list above)
    * Configure your system and/or wifi connection to use the HTTP proxy.
    * Surf to: [DuckDuckGo .onion](https://duckduckgogg42xjoc72x3sjasowoarfbgcmvfimaftt6twagswzczad.onion/) and [DuckDuckGo 'regular'](https://duckduckgo.com/). Both should resolve correctly
      * Congratulations, you are now using your Tor Proxy to connect to Tor. Too bad Pi-hole is not being used to filter out malicious/advertising sites while using Tor... But read on!

Within a Tor session on any of these devices/browsers, check if your IP is truly different and DNS servers are updated:

* => https://www.myip.com
  * Check your current external IP address
* => https://dnsleaktest.com
  * Check against dns-leaks and what dns-server you currently use, these should not be the ones configured in Unbound

**:heavy_check_mark: Congratulations** \
You now have successfully configured a DietPI Tor HTTP Proxy and you have configured your network devices/browsers to use Tor!
{: .notice--success}

Now go on with combining Part 2 and 3 of this series by reading [Part 4: Obscuring DNS traffic][4] configuration which aims to have all your DNS queries on the network be encrypted and anonymized through DNS-over-TLS-over-Tor (DoToT). As a bonus you will also be able to use the Tor Browser (configured to use your Tor Proxy) and your Pi-hole (to filter out unwanted DNS queries) together.

## Continue reading

This series comprises of 4 parts:

1. Part 1: [A little less conspicuous - Introduction][1]
2. Part 2: [A little less conspicuous - The DNS Sinkhole][2] on running your own DNS Pi-hole with secure Admin WebUI + Unbound + TLS resolvers on DietPi
   * Requirements: None, can run Standalone
   * Achieves: General Ad blocking for your network + Obscure DNS traffic for your ISP by using DNS-over-TLS resolvers
3. **This post** - Part 3: [A little less conspicuous - The Tor Proxy][3] on running your own Tor SOCKS5 Proxy + Privoxy HTTP Proxy on DietPi
   * Requirements: None, can run Standalone
   * Achieves: Have the option to 'go anonymous' on the web when you want it, also for mobile devices (iOS, Android)
4. Part 4: [A little less conspicuous - Obscuring DNS traffic][4] on routing your DNS Traffic through Tor
   * Requirements: Parts 2 and 3
   * Achieves: Obscure already Secure DNS-over-TLS Traffic from your DNS resolvers + Use Tor Browser and Pi-hole together
<!-- Begin FootNotes -->

[^1]: a Proxy is NOT a 'Relay', 'Bridge' or a 'Node', that's something entirely different in the Tor context and is not something for the faint of heart, like me.
[^2]: See [https://www.torproject.org/about/history/](https://www.torproject.org/about/history/)

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
[5]: https://fossbytes.com/tor-anonymity-things-not-using-tor/
[6]: https://www.socinvestigation.com/the-tor-architecture-and-its-inherent-security-implications/
[7]: https://www.torproject.org/download/

<!-- End References -->
