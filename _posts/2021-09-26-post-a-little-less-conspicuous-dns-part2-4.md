---
title: "A little less conspicuous - Part 2: The DNS Sinkhole"
date: 2021-09-26T12:00:00+02:00
last_modified_at: 2021-09-26T12:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/screen20210925005.png
categories:
  - blog
tags:
  - Marketing
  - SurveillanceCapitalism
  - Awareness
  - Securability
  - Anonymous
  - HyperV
  - DietPi
  - DNS
  - Pi-hole
  - DNSMasq
  - Unbound
  - DoT
  - DNS-over-TLS
variables:
  ServerName1: DNS-V1-01
  ServerIP1: 192.168.0.50
  ExistingADDomain: mydomain.local
  ExistingADDomainDNSServer: 192.168.0.10
  DNSHostName1: pihole.mydomain.local
---

This is a follow-up article for [A little less conspicuous - Part 1: Introduction][1]. Please read that first, if you came here through a direct link.

Setting up a Pi-hole is something everyone (organizations and consumers alike) should consider as a lot of sites exist 'out there' that mean you and your family/colleagues harm or aim to collect an indecent amount of information about you. Also we have to be quick before DoH takes over the world making your Pi-hole an obsolete dust-collecting artifact.

"Why?" You might ask. At the very least employing the services of a DNS Sinkhole in your network, which is what the 'Pi-hole' package is and does, can cause clicking on links by unsuspecting network users in some phishing emails to become a harmless event. This is because the FQDN's embedded in the links are transformed to a safe non-routable/unspecified address (like 0.0.0.0), by default when they are resolved (remember the phone book analogy in Part 1). Of course, the FQDNs from sites of malicious actors need to be known and embedded in at least one of the blacklists used by Pi-hole in advance to be of any use. Nonetheless, we can say that **security** is raised in some degree by using a DNS sinkhole.

A distinctive added bonus is that you are now also capable of filtering out sites hosting an advertisement payload and various tracking/telemetry gathering sites as well, which will increase your **privacy** level.

Finally because you are blocking the go-to 'address' of certain sites, you prevent the payload of these sites from being downloaded, uploaded and/or executed which will gain you some device/network **performance** as well.

![Pi-hole](/assets/images/screen20210925005.png "Pi-hole"){: style="border: 1px solid grey; padding:16px"}

As you can see in the image above almost half of my 'simple' network DNS traffic is being blocked, without disrupting functionality I should add. These blocked sites are mostly involved with telemetry gathering in my case.

**:warning: Disclaimer** \
This article and follow-ups have no ambitions to totally anonymize anyone on the internet, but just to make it a little bit more difficult for certain parties (ie. ISP's, governments, advertising moguls and the likes) to track you or your small company. \
\
The proposed setup is meant for demo purposes only and certainly not production ready without extra precautions and explanations put in place. \
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
| ServerName1                | {{ page.variables.ServerName1 }}                |
| ServerIP1                  | {{ page.variables.ServerIP1 }}                  |
| ExistingADDomain          | {{ page.variables.ExistingADDomain }}          |
| ExistingADDomainDNSServer | {{ page.variables.ExistingADDomainDNSServer }} |
| DNSHostName1               | {{ page.variables.DNSHostName1 }}               |

## Finishing the initial 'clean' DietPi installation

1. Initiate the DietPI installation described in [Part 1: Introduction](/blog/post-a-little-less-conspicuous-introduction-part1-4#dietpi-initial-installation) and continue from the `dietpi-config` screen
  ![DietPi-Config](/assets/images/screen20210925004.png "DietPi-Config"){: style="border: 1px solid grey; padding:16px"}
  *Figure 1: DietPi-Config*
2. Choose your **timezone** (Language/Regional Options / Timezone)
3. Set up your **keyboard** (Language/Regional Options / Keyboard)
   * IMPORTANT: make absolutely sure you choose the right keyboard layout here, since we will be changing passwords of the system later on and you can not see the characters typed. You might end up with an unexpected password (mine was 'Gen 105 Key with US INTL with deadkeys' btw)
4. Change your **hostname** to something meaningful (Security Options / Change Hostname)
   * I use the name **{{ page.variables.ServerName1 }}** to denote this machine.
5. Set up your **network** (Network Options: Adapters)
   * Switch from DHCP to **Static** (use 'Copy current address to static'). A static address is needed for server operations.
     * I use the address **{{ page.variables.ServerIP1 }}** to denote this machine
   * Switch **IPv6** to 'off', it is not needed. (Network Options: Adapters / IPv6)
   * Make sure you are connected (Network Options: Adapters / Test)
6. Leave the "DietPi-Config" screen
7. If you are prompted to reboot: do so
8. Finish the initial install by running '**Install**' on the `dietpi-software` screen
  ![DietPi-Software](/assets/images/screen20210925003.png "DietPi-Software"){: style="border: 1px solid grey; padding:16px"}
  *Figure 2: DietPi-Software*
9. Change default global password, when prompted
10. Change passwords for accounts (root and dietpi), when prompted
    * NOTE: These passwords should be different and make a note of them in your password vault.

**:heavy_check_mark: Congratulations** \
You now have a running instance of a DietPI Linux distribution!
{: .notice--success}

## Setting up the DNS Sinkhole Machine

* Install and Configure DietPi Software
  * Login to your machine as `root`
  * Run `dietpi-software`
    ![DietPi-Software](/assets/images/screen20210925003.png "DietPi-Software"){: style="border: 1px solid grey; padding:16px"}
    *Figure 3: DietPi-Software*
    * Choose '**Browse Software**'
      * Select both **Pi-hole** and **Unbound**
    * Choose '**Install**' and configure as follows (Pi-hole features a wizard during installation)
    * **Pi-hole** (Sinkhole DNS Server) we use Lighttpd as webserver
      * Use any 'Upstream DNS Servers'
        * Does not really matter what you choose here. These addresses will revert to Unbound loopback when Unbound is detected
      * Finally the wizard will end like this:
        ![Pi-hole Config Finish](/assets/images/screen20210913001.png "Pi-hole Config Finish"){: style="border: 1px solid grey; padding:16px"}
        *Figure 4: Pi-hole Config Finish*
      * Make a note of the 'pihole' account credentials
        * pihole:\<default global password\>
      * Pi-hole makes use of DNSMasq which is in itself a DNS Server
        * If you have (an) existing DNS server(s) on your network, for instance when running Active Directory you should:
          * Disable/remove any DNS forwarders on the existing DNS Server(s)
          * Add this conf file to enable DNS conditional forwarding for the Pi-hole: `sudo nano /etc/dnsmasq.d/05-conditional-forwarding.conf`

          ```conf
          server=/{{ page.variables.ExistingADDomain }}/{{ page.variables.ExistingADDomainDNSServer }}
          ```

      * `reboot`
    * **Unbound** (Recursive Caching DNS Server)
      * This is the first step in making all DNS request more secure as you will be forwarding these requests to designated forwarders over a secure connection (TLS)
      * Add this to the bottom of the default config: `sudo nano /etc/unbound/unbound.conf.d/dietpi.conf`

        ```conf
        server: # Do not repeat this YAML line!
          # Forwarding
          tls-cert-bundle: /etc/ssl/certs/ca-certificates.crt
          forward-zone:
            name: "."
            forward-tls-upstream: yes         # use DNS-over-TLS forwarder
            forward-first: no         # do NOT send directly
            forward-addr: 91.239.100.100@853#anycast.censurfridns.dk  # Uncensored (DK)
            forward-addr: 116.202.176.26@853#dot.libredns.gr    # LibreDNS (DE)
        ```

      * It is possible to use your own DNS forwarders (forward-addr), but these should support DNS-over-TLS (DoT) and it is recommended they support DNSSEC validation as well.
        * Check [this list][5] for good ones.
      * Unbound will use round-robin to communicate with these forwarders so it is wise to have a few more available here, but at least two!
      * Check config: `unbound-checkconf`
      * When no errors: `reboot`
      * On login:
        * Test your resolvers: `dig mozilla.org @127.0.0.1 -p 5335`
          * Should return ip address somewhere
        * Check service status: `systemctl status unbound`
          * Should have no errors and 'active' state
  * OPTIONAL: Reconfigure Pi-hole Admin UI to use SSL (aka Configure HTTPS)
    * **Lighttpd** (WebServer for Pi-hole)
      * This Webserver is automatically installed when adding Pi-hole. Follow the instructions below to get the Admin UI on port 443
      * IMPORTANT: Skip this part if you are testing this on a local machine with no access to a manageable local or internet reachable DNS domain. You could create a self signed certificate, but why bother.
      * IMPORTANT: installing your certificate is probably the most difficult part of this exercise and it can be skipped all together. Just be aware that logging in on an unsecured Web Admin interface exposes your password to the network.
      * First install the OpenSSL module for Lighttpd to get this all working: `apt install lighttpd-mod-openssl`
      * Now obtain a webserver certificate with private key for a FQDN of your choosing (**{{ page.variables.DNSHostName1 }}**) AND the trusted root authority certificate chain when using a local Certificate Authority through:
        * **Let's Encrypt** method (ok-ish)
          * Follow <https://discourse.pi-hole.net/t/enabling-https-for-your-pi-hole-web-interface/5771> for starters on configuring SSL on Pi-hole Admin Site with Certbot and LetsEncrypt
          * You need to have registered an Internet reachable domain where you can add TXT records for this to work
          * Because of very frequent automatic certificate renewal and hassle with copying certificates I don't like this option so much... it's nice to have your own Certificate Authority (CA) with certificates that can last 2 years. If you have access to one, that is.
        * **External CA** method (fine)
          * Just order the certificates and proceed with the instructions under 'Windows CA'. You can probably get them in the right (pem) format to be directly usable for Lighttpd
        * **Windows CA** method (preferred)
          * First add an A host record for the **{{ page.variables.DNSHostName1 }}** host in the **{{ page.variables.ExistingADDomain }}** DNS domain pointing to the IP address **{{ page.variables.ServerIP1 }}**
          * Request a WebServer certificate as usual through the CA
            * There exists some guidance here to make a correct request <https://support.citrix.com/article/CTX128656>
          * Export the certificate including the private key (as {{ page.variables.DNSHostName1 }}.pfx) and note the \<password!\> (use DER format when possible)
          * Export the CA root certificate chain (certnew.p7b) (use DER format when possible)
          * Place the resulting certificates on your NAS or Fileshare
          * Connect the Pi-hole machine to a Samba (aka Windows NetBIOS) drive (`dietpi-config -> Network Options: Misc -> Network Drives`. Just name the mount point 'samba' when asked (you may remove this mount point after these proceedings)
          * Convert the 'pfx' file to a 'pem' file. Run: `openssl pkcs12 -in /mnt/samba/<cert_folder>/{{ page.variables.DNSHostName1 }}.pfx -out /mnt/samba/<cert_folder>/{{ page.variables.DNSHostName1 }}.pem -nodes -password pass:<password!>`.
            * NOTE: replace \<password!\> with your chosen password on exporting the pfx file.
            * NOTE: Have the \<cert_folder\> path ready on the connected network drive
          * Convert 'p7b' file to a CAChain 'pem' file. Run: `openssl pkcs7 -inform DER -print_certs -in /mnt/samba/<cert_folder>/certnew.p7b -out /mnt/samba/<cert_folder>/fullchain.pem`
          * Create the folder to hold the certificates `mkdir /etc/lighttpd/certs`
          * Copy both pem files to that location `cp /mnt/samba/<cert_folder>/*.pem /etc/lighttpd/certs`
          * Add a SSL configuration file as follows: `sudo nano /etc/lighttpd/conf-enabled/20-ssl.conf`. The file contents should look like this:

          ```conf
          $HTTP["host"] == "{{ page.variables.DNSHostName1 }}" {
            # Ensure the Pi-hole Block Page knows that this is not a blocked domain
            setenv.add-environment = ("fqdn" => "true")

            # Enable the SSL engine with a cert, only for this specific host
            $SERVER["socket"] == ":443" {
              ssl.engine = "enable"
              ssl.pemfile = "/etc/lighttpd/certs/{{ page.variables.DNSHostName1 }}.pem"
              ssl.ca-file =  "/etc/lighttpd/certs/fullchain.pem"
              ssl.honor-cipher-order = "enable"
              ssl.cipher-list = "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH"
            }

            # Redirect HTTP to HTTPS
            $HTTP["scheme"] == "http" {
              $HTTP["host"] =~ ".*" {
                url.redirect = (".*" => "https://%0$0")
              }
            }
          }
          ```

          * Restart the lighttpd service `systemctl restart lighttpd`
          * Check service status: `systemctl status lighttpd`
            * Should have no errors and 'active' state
            * Ignore the message about mod_openssl not being in the server.modules list
      * Now, if everything went correctly, you should be able surf to you Pi-hole admin UI through SSL: https://{{ page.variables.DNSHostName1 }}/admin
  * Surf to the Admin WebUI of your Pi-hole and add some blacklists to the Pi-hole (Group Management / Adlists) mentioned on this page: <https://avoidthehack.com/best-pihole-blocklists>. I picked the green ones mentioned in the 'The Firebog' with great results.
* Reconfigure DietPi OS
  * `dietpi-config`
    * 'Network Options: Adapters' -> Ethernet -> Static DNS: 127.0.0.1
  * `reboot`
* Configure owned LAN/WAN services and devices
  * Set both Primary and Secondary DNS to {{ page.variables.ServerIP1 }} on:
    * Local machine network adapters when not configured through DHCP
    * Local/Network machine browsers NOT to use their own 'Secure DNS' or other built-in DNS service
    * DHCP Server 'Client Scope' Configuration(s)
    * Router WAN configuration
  * Optional: Disable 'Outgoing WAN connections' using ports 53, 853 (TCP/UDP) for all devices except {{ page.variables.ServerIP1 }}
    * Pi-hole (actually Unbound) will only use TCP port 853 btw, and after following Part 4 of these series not even that. So you could do the blocking for your whole network at a later time or split up the proposed firewall rules
  * Optional: Totally block any device with hard-coded (Secure) DNS settings from accessing the internet.

**:heavy_check_mark: Congratulations** \
You now have successfully configured a DietPI Pi-hole DNS Sinkhole that can be managed through a secured Admin Web interface and you have configured your network devices to use the Pi-hole as their DNS server!
{: .notice--success}

Now go on with the [Part3: The Tor Proxy][3] installation to have a functioning Tor + Privoxy HTTP Proxy.

## Continue reading

This series comprises of 4 parts:

1. Part 1: [A little less conspicuous - Introduction][1]
2. **This post** - Part 2: [A little less conspicuous - The DNS Sinkhole][2] on running your own DNS Pi-hole with secure Admin WebUI + Unbound + TLS resolvers on DietPi
   * Requirements: None, can run Standalone
   * Achieves: General Ad blocking for your network + Obscure DNS traffic for your ISP by using DNS-over-TLS resolvers
3. Part 3: [A little less conspicuous - The Tor Proxy][3] on running your own Tor SOCKS5 Proxy + Privoxy HTTP Proxy on DietPi
   * Requirements: None, can run Standalone
   * Achieves: Have the option to 'go anonymous' on the web when you want it, also for mobile devices (iOS, Android)
4. Part 4: [A little less conspicuous - Obscuring DNS traffic][4] on routing your DNS Traffic through Tor
   * Requirements: Parts 2 and 3
   * Achieves: Obscure already Secure DNS-over-TLS Traffic from your DNS resolvers

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
[5]: https://www.privacytools.io/providers/dns/
[6]: https://discourse.pi-hole.net/t/enabling-https-for-your-pi-hole-web-interface/5771

<!-- End References -->
