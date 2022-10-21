---
title: "PowerShell Heaven - Optimizing your Microsoft Graph Usage"
date: 2022-10-20T10:00:00+02:00
last_modified_at: 2022-10-21T10:00:00+02:00
og_publish_date: 2022-10-20T12:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/post-powershell-heaven-optimizing-your-ms-graph-usage-001.png
categories:
  - blog
tags:
  - Awareness
  - PowerShell
  - Microsoft365
  - MicrosoftTeams
  - MicrosoftExchange
  - AzureAD
  - PnP
  - PnPPowerShell
toc: true
toc_label: "Contents"
toc_icon: "list-alt"
---

First off, I am a great fan of PowerShell. It is a very versatile and powerful scripting language that, ever since the '.NET Core revolution', works cross-platform as well. Awesome!

While the language in itself is cool, its installable extensions called **[PowerShell Modules][17]**, usually delivering 'one-liner' cmd-lets that are tailor-made to configure a certain platform or application, are what makes PowerShell truly Powerful!

![PowerShell Heaven - Optimizing your Microsoft Graph Usage](/assets/images/post-powershell-heaven-optimizing-your-ms-graph-usage-001.png "PowerShell Heaven - Optimizing your Microsoft Graph Usage")  
*The wonders of the `Invoke-PnPGraphMethod` cmd-let!*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

## A Plethora of Modules

While both the PowerShell 'core language' and its extensibility in the form of Modules are absolutely great I did notice an explosive rise in the number of Microsoft-built 'Modules' available. Each covering a seemingly different segment of the Microsoft Cloud Services.  
This represents somewhat of a double-edged sword.  
On the one hand PowerShell and its Modules beat configuring stuff through a GUI any day of the week.  
However, Modules can sometimes be hard to unify, they might have overlapping capabilities, have independent life-cycle and 'major update' (read: 'breaking changes') schedules and sometimes have massive disk space requirements (like the AZ PowerShell Modules). So, using more modules in a solution, will make such a solution harder to manage.  
Compared to the 'On Premises' era, where you would probably use Active Directory, Exchange, SQL and of course the famous (now legacy) [SharePointPnPPowerShellOnline][3] (not to be mistaken with my favorite Module still, the [PnP.PowerShell][4] Module) Modules, to manage a large part of the Microsoft ecosystem, we now have a Cloud reality and the number of Modules skyrocketed.

As a side-note, most of the MS provided modules can be found [here][1].  
Some very obscure ones like the [MSCommerce][2] Module (very important to limit Self-Service licensing capabilities in your tenant) are not even mentioned BTW, so expect the list to be larger.

## PowerShell Modules: Always at least one step behind

One important thing to realize is that PowerShell Modules' cmd-lets always represent an 'abstraction' of the capabilities of the underlying API they expose. As such these modules are at least 'one step behind' in delivering the said API's capabilities.  
Following this, another thing to realize is that even though the authors of the Module will always try to stay as close to an API's intended purpose as possible, choices (as in: 'design decisions') have to be made at some point and 'bias'[^1] is introduced.

## PowerShell Modules: Latest updates and trends

When it comes to specifying one of the biggest 'trends' concerning PowerShell Modules used for manipulating the Microsoft Cloud it would be 'The Slow but Certain Move to the Microsoft Graph'.  
Some important Microsoft 365 management Modules like 'Azure AD' and 'MSOnline' are now [scheduled for deprecation][7], they are slowly being replaced with MS Graph enabled alternatives, like 'the king' of Graph-enabled modules: the [Microsoft Graph PowerShell SDK][5]. Sadly not everyone at Microsoft have gotten the message, as new capabilities and documentation references to the mentioned 'soon to be legacy' Modules are still being added to this day.  
A very noteworthy Module, the [PnP.PowerShell][4], is also slowly transitioning to use the MS Graph under the hood.

We can see the 'ExchangeOnline' Module recently got an update to 'v3' where modern sign-in/connection methodologies were implemented and, by removing the WinRM dependency, this module has now gained reliable and supported cross-platform capabilities.  

These are just a few of recent changes and updates within the PowerShell Module ecosystem and you really have to be 'on top' of all of these changes to make sure the Modules you use 'play nice' together and update your scripts when a breaking change[^2] happens.

## The MS Graph is here to stay

I want to express my hope that more and more Modules covering the Microsoft cloud ecosystem will be transitioned to the Microsoft Graph and that we have **less** Modules to manage (going against the trend, I know) in a few years from now.

It is clear as day IMHO that the Microsoft Graph is a keeper and will be with us for a long time. So while most actively developed Modules (Microsoft-provided, third-party and community Modules alike) are making the transition, this got me thinking.

**:question:** Why not stay as close to the source (ie. the Microsoft Graph **API**) as you can get while developing PowerShell solutions?
{: .notice--info}

My reasoning:

- The Microsoft Graph [documentation][8] is great (also for the beta endpoint) and you can really count on it to be accurate. The examples provided are also excellent.
- Installing numerous Modules (especially in pipeline operations, where Modules need to be installed on each run of the pipeline) can be quite a lengthy process, adding to the overhead of total script execution.
- Being dependent on Module maintainers to release a certain parameter for a cmd-let to accommodate fresh new additional functionality for the Graph API, is undesirable.
- As an IT Pro you might broaden your professional reach, closing the gap to development paradigms and solutions by learning and using more of the underlying API.
- And finally, the most important reason I can think of: **Build better scripts**, using your own business-driven 'bias'.

So while the Microsoft Graph PowerShell SDK is available and the peeps of the PnP.PowerShell initiative are busy making the MS Graph transition, all we really need is a steady/stable authentication process to connect to the Microsoft Graph and query the API (GET, PUT, DELETE, etc.) directly.  
A great Module is available, [which I recommended in a previous blog post][9], called [EasyGraph][10].  
BUT... wait for it. The aforementioned and acclaimed PnP.PowerShell Module developers have added that same 'query the MS Graph directly' functionality in their Module. Now we can benefit from:

- the vast array of cmd-lets already present in this Module;
- all the different authentication methods that this Module provides;
- [having the option to query the Graph][16] **directly**.

FANTASTIC!

## Example: Using the MS Graph through PowerShell

So, the best reason to query the Graph directly is simply to write better scripts that perform better for your specific business scenario. I will be explaining that now using both the 'use existing cmd-let' and 'query the Graph directly' options, making use of the PnP.PowerShell Module.

Imagine we had the following requirement:

Deliver a Report:  
**:black_square_button:** listing all Teams  
**:black_square_button:** with their Owner(s)
{: .notice--success}

### Option 1 - Built-in Module cmd-let

Now, you could use the built-in cmd-let [`Get-PnPMicrosoft365Group`][11] with the `-IncludeOwners` parameter. Which is probably fine by itself.

```powershell
# Fetch all Teams including Owners
$Result = Get-PnPMicrosoft365Group -IncludeOwners
# Show results of the first returned Team
$Result[0] | Select-Object -Property @(@{L = "TeamDisplayName"; E = {$_.DisplayName}}, @{L = "TeamMailNickname"; E = {$_.MailNickname}}) -ExpandProperty Owners | Select-Object TeamDisplayName,TeamMailNickname,Email
```

The first command in this snippet executes a [batched operation][12] (Click the link to understand the 'bias' that will always be inherent for an abstraction like this.[^3]) to fetch the Owners of each Team (with pretty good performance, I might add).

However we could make a direct call that performs even better by using just **one** API call (no batching of calls required) and return only the properties we need, not more. Because if you would run...:

```powershell
$Result[0] | Select-Object *
```

... it is clear that a lot more (some being irrelevant to our need) properties are returned. The cmd-let is actually over-delivering here.

### Option 2 - Direct Query

Now we will devise a **single call** that returns only the information of the Teams we require. No more, no less.  
In comes the power of '[ODATA query options][14]'.  
[ODATA][13] is a protocol employed by the MS Graph to allow for, among other things, 'structured querying', very much like querying a database.  
For example, we could use the `$select`, `$filter` and/or `$expand` operations to structure an API Query URL in such a way that it will deliver exactly the right data for our usage.

```powershell
# Get endpoint
$Uri = "v1.0/groups/?`$select=id,displayName,mailNickname&`$filter=resourceProvisioningOptions/Any(x:x eq 'Team')&`$expand=owners(`$select=mail)"
# Place the call
$Result = Invoke-PnPGraphMethod -Method Get -Url "$($Uri)" -ContentType "application/json" -All -ErrorAction Stop
# Show results of the first returned Team
$Result[0].value | Select-Object -Property @(@{L = "TeamDisplayName"; E = {$_.displayName}}, @{L = "TeamMailNickname"; E = {$_.mailNickname}}) -ExpandProperty owners | Select-Object TeamDisplayName,TeamMailNickname,mail
```

The cmd-let `Invoke-PnPGraphMethod` will touch the `groups` endpoint through a GET request method, limit the properties returned through the `$select` operation and `$expand` the owner information for each Team.  
The specific [documentation][15] for this endpoint will show you some examples and the Graph permissions required to make the call. This is important if you want to adhere to a 'least privilege' operational stance. Notice the clarity and repeated structure of this documentation for all endpoints.

While the endpoint `$Uri` construct might scare some people, I tend to prefer this over the 'one size, fits all' approach of the default Module cmd-lets.

Now the only returned properties will be the ones that are explicitly requested (not possible with Option 1 at the moment) as you will see after running:

```powershell
$Result[0].value | Select-Object *
```

Probably, (especially if you have many Teams in your tenant) the query presented by Option 2 will return a bit quicker compared to Option 1.

No extra calls (except for the obvious 'paging' of the results, which is also present in Option 1) will be executed, making this somewhat more performant[^4] than Option 1.

Also, you have learned more about the nature of the Microsoft Graph and the nature of PowerShell Module 'API abstractions' which is always a good thing!

<!-- Begin FootNotes -->
[^1]: a bias is an inclination toward something, or a predisposition, partiality, prejudice, preference, or predilection.
[^2]: Microsoft removed the operational status for some ExchangeOnline cmd-lets when used in an App Only context. [Learn more][6].
[^3]: In this case the 'bias' manifests itself by a preference to use a 'batching strategy' in favor of using the 'built-in ODATA query option' `$expand` to fetch the Team's Owner information. This is a logical preference for the Module's implementation of the MS Graph API because batching scales better if multiple properties need to be expanded. In our example we only need **one** property, so the use of batching would be overkill.
[^4]: The word 'performant' in this post means something along the lines of 'speedy (both the **number** of API calls and **used bandwidth** are taken into consideration), accurate, flexible and capable'.

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

[1]: https://learn.microsoft.com/en-us/powershell/module/
[2]: https://learn.microsoft.com/en-us/microsoft-365/commerce/subscriptions/allowselfservicepurchase-powershell?view=o365-worldwide
[3]: https://github.com/pnp/PnP-PowerShell
[4]: https://github.com/pnp/powershell
[5]: https://learn.microsoft.com/en-us/powershell/microsoftgraph/installation?view=graph-powershell-1.0
[6]: https://learn.microsoft.com/en-us/powershell/exchange/app-only-auth-powershell-v2?view=exchange-ps
[7]: https://office365itpros.com/2022/03/17/azure-ad-powershell-deprecation/
[8]: https://learn.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0
[9]: /blog/post-powergrip-the-graph/#easygraph
[10]: https://365lab.net/2020/07/28/new-module-easygraph-for-accessing-microsoft-graph-from-powershell/
[11]: https://pnp.github.io/powershell/cmdlets/Get-PnPMicrosoft365Group.html
[12]: https://github.com/pnp/powershell/blob/530c96fd3cd558eb06595155bef9679a4c51766b/src/Commands/Utilities/Microsoft365GroupsUtility.cs
[13]: https://www.odata.org/
[14]: https://learn.microsoft.com/en-us/odata/concepts/queryoptions-overview
[15]: https://learn.microsoft.com/en-us/graph/api/group-list?view=graph-rest-1.0&tabs=http
[16]: https://pnp.github.io/powershell/cmdlets/Invoke-PnPGraphMethod.html
[17]: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_modules

<!-- End References -->
