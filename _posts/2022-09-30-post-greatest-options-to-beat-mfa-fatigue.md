---
categories:
- blog
date: 2022-09-30 10:00:00+02:00
header:
  og_image: /assets/images/post-greatest-options-to-beat-mfa-fatigue-001.jpeg
last_modified_at: 2022-09-30 10:00:00+02:00
og_publish_date: 2022-09-30 12:00:00+02:00
tags:
- MicrosoftAuthenticatorApp
- AnnoyingImmediateAuthenticatorAppNotifications
- AdditionalAuthenticatorAppAuthenticationMethodPolicies
- AuthenticatorApps
- AuthenticatorAppPrompt
- AuthenticatorAppEntries
- AuthenticatorApp
- AuthenticatorAppRequest
- MfaPrompt
- AuthenticatorPrompt
- MfaFatigue
- OldAuthenticatorAppEntry
- Authenticator
- AzureAdConditionalAccessPolicy
- AuthenticationSequence
title: The Greatest Options to Beat MFA Fatigue
toc: true
toc_icon: list-alt
toc_label: Contents
---

Lately, a lot has been said about the phenomenon called 'MFA fatigue'. This method of gaining access to someone's account (we will focus on (Microsoft 365[^1] Work/School accounts) plays on the tendency of people to be 'done with it' which will make them press that 'Approve' button in their authenticator apps at some point. This is especially the case where the Microsoft Authenticator App 'push notifications' (without number matching) feature has been configured. Attackers only have to know the sign-in name, which usually is the same as their email address, to start annoying people. And the target only has to press the Approve button on the MFA prompt. Things are different when:

* number matching has been configured on the authentication method.
* a password is required to login and only as a second step the Authenticator App request is sent. (It is unlikely the attacker knows your password, so the Authenticator App request will never be sent)

Focusing on the 'Authenticator-only' approach boasted with Password-less sign-in feature, attackers will know that Microsoft is actively monitoring 'risky sign-ins' especially for their commercial offerings like Microsoft 365. This happens without the need to have the appropriate licenses for your tenant (Azure Premium P2 at the time of writing). If you don't have the licenses, Microsoft will still use the data to make their cloud service safer, except you don't have the insight in whats happening or have some configurable options.  
Knowing this constant monitoring of sign-in traffic is happening, attackers will only 'probe' accounts with a couple of requests at a time, and they will spread their total attack time-frame over multiple weeks, making the target lose his/her mind in a progressive manner. "Where are those \**beep*\* \**beep*\* pop-ups coming from!?"

So the attack stays under the radar until the target starts complaining to the service desk or administrators in charge of their tenant. Let us hope they make that effort...

## How to mitigate the 'MFA Fatigue'?

![The Greatest Options to Beat MFA Fatigue](/assets/images/post-greatest-options-to-beat-mfa-fatigue-001.jpeg "The Greatest Options to Beat MFA Fatigue")  
*Yawn... Approve!*
{: style="border: 1px solid grey; padding:16px; font-size:10px; margin-right:250px"}

## Not an option...

You might expect that you can change some Azure AD Conditional Access policy to change the behavior, but since these policies only have effect *after* a successful sign-in, they are of no use mitigating MFA fatigue.

## Option 1

The simplest method would be: do not use a Password-less (especially the push notification (without number matching)) sign-in feature as the only authentication method for the targeted account. Instead make use of either the password, certificate or FIDO2/Windows Hello methods as a first factor and use the authenticator app prompt as a second factor only ([Read more][1]). This will stop any annoying immediate Authenticator App notifications, because the attacker needs to comply with the first factor in the authentication sequence before the target gets prompted in the App. The suggested first factors are 'silent' to the user and the attacker is unlikely to have knowledge or access to this factor  
With regards to the first factor 'password' option you might say: "Well but that is a terrible experience!"Indeed it is: it makes the login processes lengthy again and weren't we about to phase out passwords forever?  
So requiring the use of FIDO2/Windows Hello or Certificate Based Authentication (CBA) as a first factor is highly recommended here. This option is most definitely the **most silent** method for the user, but will require additional configuration and maintenance for admins...

## Option 2

At least enable 'number matching' on the 'authentication method' configuration blade. This way even if the Authenticator prompt occurs (still annoying) the target won't have a clue which number (from 1 to 99) has to be inputted and will most likely deny or ignore the request.

Last week Microsoft introduced [additional Authenticator App authentication method policies][2]. Number matching can now be enabled on the 'push notification' method, which I highly recommend to enable immediately for everyone, even though the feature is in preview.

## Option 3

Now for the elephant in the room. The reason we are all in this 'MFA Fatigue'-drama all comes down to an architectural decision Microsoft made in the time when the cloud landscape was born (somewhere around 2007-ish, I guess). The company decided it would be a good idea to make the 'sign-in name' (aka User Principal Name (UPN)) that everyone would use to gain access to the cloud environment the very same as the 'email address' ('SMTP:' proxyAddresses or SIP) that the whole world can 'know' in an instant and is highly visible. All for the sake of ease-of-use, security-by-design was not a thing back then, clearly (is it now??)...  
Technically it would be possible to have these properties have separate values, but even Microsoft says this will have some nasty side-effects and will certainly ruin the Microsoft Cloud experience for users. So that's really not the way to go, at least at the time of me writing this piece, anyway.

So our only option, for now, is to change both UPN AND the primary SMTP address and make it more difficult for attackers to know the sign-in name, before they eventually track it down. Except of course when the attacker is in close day-to-day contact with the target, they will pick up the new sign-in address immediately. Therefore I will assume that the attackers are using email addresses they found in some online database and are not directly connected to the target.  
Furthermore the original email address of the target must still remain operational, so we need to create a 'good old' Distribution List for that email address, with the target as its only member AND have the target setup with 'Send As' permissions so they can still send emails using their old email address. Be careful NOT to use the 'old' email address as an account 'Alias', because it is very easy to find the actual UPN of anyone when you know only one account alias. Which is yet another major architectural oversight by Microsoft[^2].

**:heavy_exclamation_mark: WARNING**  
One big, possibly disruptive factor is that the account will have its OneDrive address changed because of the UPN change (ie. https://contoso-my.sharepoint.com/:f:/g/personal/bgates_contoso_com/). Content and Shared Links will remain, but the links will have a slightly different URL and 'old' links will not be resolvable. This might be a problem if this targeted user has a lot of private Teams Chats and shares a lot of files in this/her OneDrive.
{: .notice--warning}

So, in any case make this change after business hours as email flow is disrupted for a short while to the target's original email address.

The procedure looks like this:

1. **Microsoft 365 admin center**: Change the targets Primary email address and username (UPN) to something that was not already an Email Alias
2. **Microsoft 365 admin center**: Remove the previous primary email address as an Email Alias
3. **Exchange admin center**: Change the targets Alias (found under 'Manage contact information') to the UPN minus the '@domain' part.
   1. NOTE: This is NOT in email address format
4. **Exchange admin center**: Change the SIP address (found under 'Manage email address types') to the same value as the new UPN
5. **Exchange admin center**: make sure there is no trace of the 'old' UPN (found under 'Manage email address types')
6. Wait 5 minutes!
7. **Exchange admin center**: Create a new Distribution List, where:
   1. the original (old) email address of the target is configured
   2. target user should be its only Member
   3. both messages from people inside and outside my organization should be accepted
   4. target user should have 'Send As' permissions
   5. joining and leaving the group should be set to 'Closed'

**:information_source: INFO**  
While existing Authenticator App entries will still work for the account, the wrong (old) UPN is displayed in the App, so the user should enter the Microsoft Account 'My Sign-ins -> Security' [page][3] and remove any old Authenticator App entry (both on that page and in the Authenticator App) and re-add using the new UPN.
{: .notice--info}

**:information_source: INFO**  
While configured desktops for the user will automatically adjust all Microsoft 365 Desktop Apps and OneDrive to use the new UPN (Nice!), things are different in the mobile space. Any Mobile App that is configured for/by the user (Outlook, SharePoint, Teams, etc.) might need to the account re-added as well, because the old UPN will linger there. Single exception here is the OneDrive App, I found, which will (after a crash) show the correct UPN.
{: .notice--info}

The user should inform close contacts OUTSIDE of the organization of their email address change. Alternatively the user can also opt to keep sending emails from their original email address, but he/she would probably need instructions how to do that.  
The 'Send As' function in the Outlook Web App is very easy to use when the 'From' field is shown on a new message, but it does require discipline as you can not set the default FROM address to a Distribution List, sadly.  
An inventive Exchange Admin could imagine to setup a 'mail flow rule' (aka transport rule) to change the 'Reply-To' header of each mail sent from this user, but alas, this will not work as well.

## Parting words

There you have it! Despite the mentioned repercussions this method of mitigating 'MFA fatigue' might be worth it for some and not for others. At least for a time attackers won't know the sign-in address to spam.

It would be nice if Microsoft at some point acknowledges the underlying fundamental flaw of the current Cloud sign-in scheme and decide to separate the sign-in address from the regular email addresses of users and thus having the option to always, without notification or disturbing ramifications, change the sign-in address of any user on a whim.

<!-- Begin FootNotes -->

[^1]: Microsoft 365, formerly Office 365, is a line of subscription services offered by Microsoft which adds to and includes the Microsoft Office product line. The brand was launched on July 10, 2017, for a superset of Office 365 with Windows 10 Enterprise licenses and other cloud-based security and device management products.
[^2]: Try this out: start Microsoft Teams and enter the 'Chat' area. Start a new chat and type in an email address (or alias) of someone you suspect of having a Microsoft 365 account but is not already in your contact list. Select the option of Searching for the contact externally, but do NOT start typing a message. Instead take a look at the contact card of the person you are 'contacting' by hovering over the name. This will show the actual UPN under the Contact section. So now you have the Sign-in name without the target knowing...

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

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://learn.microsoft.com/en-us/azure/active-directory/authentication/concept-authentication-methods
[2]: https://learn.microsoft.com/en-us/azure/active-directory/authentication/how-to-mfa-additional-context
[3]: https://mysignins.microsoft.com

<!-- End References -->
