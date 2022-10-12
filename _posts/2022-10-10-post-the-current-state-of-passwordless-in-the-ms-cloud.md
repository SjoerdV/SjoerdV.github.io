---
title: "The Current State of 'Password-less' in the Microsoft Cloud"
date: 2022-10-11T10:00:00+02:00
last_modified_at: 2022-10-11T10:00:00+02:00
og_publish_date: 2022-10-11T12:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/post-the-current-state-of-passwordless-in-the-ms-cloud-001.png
categories:
  - blog
tags:
  - Awareness
  - Security
  - Privacy
  - Microsoft365
  - MicrosoftTeams
  - MicrosoftExchange
  - AzureAD
  - MFA
  - Passwordless
  - WindowsHello
  - MicrosoftAuthenticatorApp
  - FIDO2
  - CBA
  - Intune
toc: true
toc_label: "Contents"
toc_icon: "list-alt"
---

'Password-less' is the new buzzword in 'Microsoft Cloud Security' land.  
Basically it ushers in 'The Future of Authentication', promising easy, but very secure, access to your Cloud resources by eliminating the need to think up, remember (and periodically change!) the traditional password that goes with a user name.  
This is a great idea because easy to remember passwords are still one of the major attack vectors any hardcore hacker or 'script kiddie' will employ to hack *all y'all*.

![The Current State of 'Password-less' in the Microsoft Cloud](/assets/images/post-the-current-state-of-passwordless-in-the-ms-cloud-001.png "The Current State of 'Password-less' in the Microsoft Cloud")  
*Go Password-less!*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

Most organizations, especially the ones using Big Tech's ecosystems, have already added MFA to the mix, making pwning[^1] the password of some unsuspecting user less relevant. However, while it is well known that a second authentication factor consisting of a only 'SMS' or 'alternate email' can be hijacked quite easily (Channel-jackable), the [latest hacking technique][2] employs a MitM to spoof the Big Tech logon page and redirect the acquired tokens to the attacker, who will now have access to the users data (if no other measure have been put into place). This can happen even when the user is using the Microsoft Authenticator App push or notification features as their second authentication factor (Channel-jackable).

**:heavy_exclamation_mark: Ouch!**  
{: .notice--warning}

The technique is generally used in Real-Time 'spear phishing' campaigns (ie. very targeted!) and may happen because the Microsoft authentication servers are not verifying the requestor URL as part of the authentication sequence.

Another method that is quite popular relies on the set-up that the Microsoft Authenticator App is the only element in the authentication process (satisfying both first and second factors). In this scenario the attacker only has to know the target's sign-in name (UPN) to start 'annoying' the target with random push notifications on their App which can lead to MFA fatigue.

So, in my search for alternate options which are more secure in the 'wild wild web' authentication conundrums I came to a sad conclusion.  
There are no ‘Open’, ‘Cross-platform’, ‘Phishing resistant’ [Password-less sign-in methods][1] for the Microsoft Cloud, even after the concept had been introduced in 2018 already.

What *are* the options available?

- **Windows Hello for Business** -> Windows only. Period! But that’s fine, works nicely for the most part (excluding the issues with the [latest Windows 11 22H2 update][6]) and remains 'phishing proof'.  
Great option if you want and can have all your users firmly in the Microsoft ecosystem.
- **Microsoft Authenticator App** -> *WARNING*: These Apps (available on iOS and Android) are like I previously explained: vulnerable to [phishing][2] AND [MFA Fatigue][3]. Otherwise this option is great for universal access anywhere and from every device.
- **FIDO2 security keys** -> *WARNING*: Vendor lock-in detected. These 'hardware tokens' are only ‘universally usable’ in Windows context with ‘specific’ Chromium browsers. I won’t consider [FIDO2][4] a viable option to access the Microsoft Cloud until [full Linux+Firefox and Mobile][4] support is available.  
*Here follows an opinion*: The dependence of this 'FIDO2 Standard' on implementers who, of course, favor their own clients and software (in this case Microsoft favoring Windows and Edge), effectively (almost) forgetting about alternate ecosystems, together with the 'closed nature' of the hardware development might cause this Standard to self-destruct at some point.  
This state of affairs should be considered a serious risk for any (aspiring) CISO to invest in this method.

Finally, although not truly Password-less, because the 'password method' needs to remain enabled for this Authentication Method to work (at least in the Preview), I would like to mention the following method anyway because it might just emerge as the über-Password-less option in the future (I have high hopes in any case):

- **Certificate Based Authentication** (in Preview) -> *WARNING*: Certificates have to be linked to User Sign-in name (UPN) instead of requiring only certificate Thumbprint matching on any Azure AD account property **AND** Mobile support is tricky **AND** managing the CRL location is a burden **AND** managing the PKI infrastructure (Especially lack of OCSP and LDAP revocation checking) is not on par in this Preview. [Read some more about CBA][5].  
Earlier this year Microsoft updated this offering in such a way that an on premises ADFS set-up is no longer required to enable CBA. All the authentication assets are now configured by Azure AD (and Intune).
Most of the hurdles mentioned above can be made 'manageable' by using Intune and its Simple Certificate Enrollment Protocol (SCEP) profiles. I found a [great blog][8] about just this subject  
*Here follows an opinion*: But there are some things I dislike about this approach.
  - I would like to add CBA to un-managed devices (No Intune!) as well
  - Furthermore [requiring an on-premises Windows Server machine joined to a local AD][7] that is running the Intune connector seems both archaic and like yet another vendor lock-in / up-selling strategy.
  - I would like to see an option to have the end user upload their own certificate (the public part) as a sign-in method to the Security info section of their Account Portal. I would imagine that Microsoft can make it so that, on upload, the Subject Key Identifier (SKI) hash of the Public Key is automatically added to the Azure AD profile after the certificate is checked to originate from the correct Root CA certificate and that the hash is configured to serve as 'binding' during authentication. The private key being on the Users machine/browser, of course.

So even though enrolling and managing these User certificates is a hassle, together with a lesser user experience compared to FIDO2, it still might become the only phishing resistant, cross-platform and cross-browser authentication method because the techniques involved are very well established on all platforms.

Will we see this method leaving behind the proprietary FIDO2 hardware solutions, while it becomes more mature? We will see.

As a parting note: I would really like to be able to use CBA (as a first factor, effectively replacing a password) in combination with the Authenticator App as a second factor at some point in the future. This would prevent both phishing and MFA fatigue in all likely business scenario's. Sadly this configuration is not (yet?) possible, so [join the discussion][9].

<!-- Begin FootNotes -->
[^1]:What does it all mean -> <https://haveibeenpwned.com/About>

<!-- End FootNotes -->

<!-- Begin Abbreviations -->

*[PII]: Personal Identifiable Information is comprised of any representation of information that permits the identity of an individual to whom the information applies to be reasonably inferred by either direct or indirect means.
*[ISP]: An Internet Service Provider (ISP) is a company that provides Internet access to organizations and home users.
*[denial of service]: The act of sending so many data packets to an internet service that it crashes.
*[spoofing]: Posing to **be** a certain party in a chain of communications and **act** (usually fraudulently) on behalf of that party having the chain believe it was in fact the original party.
*[cookies]: a persistent and/or reusable data asset meant to store application data and/or identity information of a network/device/browser.
*[DNS]: The Domain Name System (DNS) is the phone book of the Internet. Humans access information online through domain names, like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources.
*[NTP]: The Network Time Protocol (NTP) is a networking protocol for clock synchronization between computer systems over packet-switched, variable-latency data networks.
*[DHCP]: Dynamic Host Configuration Protocol (DHCP) is a client/server protocol that automatically provides an Internet Protocol (IP) host with its IP address and other related configuration information such as the subnet mask and default gateway. RFCs 2131 and 2132 define DHCP as an Internet Engineering Task Force (IETF) standard based on Bootstrap Protocol (BOOTP), a protocol with which DHCP shares many implementation details. DHCP allows hosts to obtain required TCP/IP configuration information from a DHCP server.
*[TLS]: Transport Layer Security (TLS) is a cryptographic protocol that provides end-to-end security of data sent between applications over the Internet.
*[DietPi]: Debian based Linux distribution, stripped to the bare essentials. DietPI allows you to easily install optional "ready to run" and optimized software for your device. As well as automating the Linux installation and configuration process for you, DietPi-Software also applies specific optimizations unique to your hardware model, ensuring you get the maximum performance from your device.
*[Pihole]: Pihole is a Linux network-level advertisement and Internet tracker blocking application which acts as a DNS sinkhole and optionally a DHCP server, intended for use on a private network.
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
*[QUIC]: QUIC is a general-purpose transport layer network protocol initially designed by Jim Roskind at Google, implemented, and deployed in 2012, announced publicly in 2013 as experimentation broadened, and described at an IETF meeting. QUIC is used by more than half of all connections from the Chrome web browser to Google's servers.
*[GPSCS]: Global Private and Secure Communication Services. This is an abbreviation I use in this article, it's not a term that is widely used.
*[FOSS]: Free and Open Source Software as defined by the free software movement and the open-source software movement.
*[MAC]: Media Access Control
*[ROM]: Read Only Memory
*[UPN]: User Principal Name, this is the main 'sign-in address' and usually the 'primary email address' of a user account in the Microsoft Cloud eco-system.
*[MFA]: Multi Factor Authentication
*[MitM]: In cryptography and computer security, a man-in-the-middle, attack is a cyberattack where the attacker secretly relays and possibly alters the communications between two parties who believe that they are directly communicating with each other, as the attacker has inserted themselves between the two parties.
*[CISO]: A chief information security officer is a senior-level executive within an organization responsible for establishing and maintaining the enterprise vision, strategy, and program to ensure information assets and technologies are adequately protected.
*[CBA]: Certificate Based Authentication
*[OCSP]: The Online Certificate Status Protocol is an Internet protocol used for obtaining the revocation status of an X.509 digital certificate.
*[CRL]: In cryptography, a certificate revocation list (or CRL) is "a list of digital certificates that have been revoked by the issuing certificate authority (CA) before their scheduled expiration date and should no longer be trusted"
*[ADFS]: Active Directory Federation Services, a software component developed by Microsoft, can run on Windows Server operating systems to provide users with single sign-on access to systems and applications located across organizational boundaries.

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://learn.microsoft.com/en-us/azure/active-directory/authentication/concept-authentication-passwordless
[2]: https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/all-your-creds-are-belong-to-us/ba-p/855124
[3]: /blog/post-greatest-options-to-beat-mfa-fatigue/
[4]: https://learn.microsoft.com/en-us/azure/active-directory/authentication/fido2-compatibility
[5]: https://learn.microsoft.com/en-us/azure/active-directory/authentication/concept-certificate-based-authentication
[6]: https://www.bleepingcomputer.com/news/microsoft/windows-11-22h2-blocked-due-to-windows-hello-issues-on-some-systems/
[7]: https://learn.microsoft.com/en-us/mem/intune/protect/certificates-scep-configure#servers-and-server-roles
[8]: https://hmaslowski.com/f/azure-ad-certificate-based-authentication-cba-in-public-preview
[9]: https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad/cba-mfa-and-aadsts54008-certificate-is-not-supported-as-first/m-p/3422036

<!-- End References -->
