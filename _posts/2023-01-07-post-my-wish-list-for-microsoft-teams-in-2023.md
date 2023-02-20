---
categories:
- blog
date: 2023-01-11 10:00:00+02:00
header:
  og_image: /assets/images/post-my-wish-list-for-microsoft-teams-in-2023-001.png
last_modified_at: 2023-01-11 10:00:00+02:00
og_publish_date: 2023-01-11 10:00:00+02:00
tags:
- WishList
- MicrosoftTeamsProduct
- MicrosoftTeams
- Teams
- Team
- TeamsClient
- OpenSourceSoftwareMovement
- Foss
- CrossPlatformSupport
- BlazingFastClient
- DesktopApps
- DedicatedMobileApps
- ProgressiveWebApps
- WebApps
- WebApp
- FeatureParity
- DesktopSharing
- VideoCalling
- MicrosoftGraph
- Compliance
- Compatibility
- BrighterFuture
- Suggestions
title: My Wish List for Microsoft Teams in 2023
toc: true
toc_icon: list-alt
toc_label: Contents
toc_sticky: true
---

With this post I will propose some ideas on 'making Teams better' in 2023. The paragraphs below are 'prioritized', so first things first. I know most of this post goes against everything the 'current age' commercial and productivity drone (note the gender neutrality here even including artificial lifeforms, patting myself on the back) is engaged in. But without poking the bear a bit, what else is there but moronic thoughtless complacency.

A 'feature stop' of **at least** six months would be highly recommended if the wishes listed below would have any chance of being successfully implemented. That in itself would be less likely than me shitting out a leprechaun who will offer me a 'pot-o-gold' if I only promise to shit some more. So I will not be putting it on my wish list ;-).

OK, here goes.

![My Wish List for Microsoft Teams in 2023](/assets/images/post-my-wish-list-for-microsoft-teams-in-2023-001.png)
*My Wish List for Microsoft Teams in 2023*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

## Make it FOSS

The Teams server / client modules including all its derivative software (like Desktop and WebApp, for all OS-es on iOS, Android, MacOS, Windows, Linux, etc., for all architectures (x86, ARM)) should be made Free and Open Source. A [dual-licensing construction like Qt has][8] is interesting as well. There are a lot of variables in play with which to compose a working FOSS/Commercial model work, so a lot of research is necessary on the subject.  

Why, you ask? Well...

1. I always like the option to start **discovering** any product (both client and server components) under a non-paid FOSS license and have the option to upgrade to a paid commercial one (with yearly/monthly fees per user) later on. Trying out 'stuff' leads to understanding which leads to adoption. So this would be in any company's interest.
2. I would like to **trust**, in both quality as well as privacy/security aspects, any application that is such a core part of my working life. Adopting a FOSS model is one necessary (not a sufficient one, mind you!) step towards truly independent Security and Privacy audits for a product/service.
3. If I don't like some of the changes or want to add functionality I would like to be able to fork the project and **make my own** client, for instance.  
The current closed nature of Teams software development is frustrating. The community is not being heard enough. Even if the reader disagrees with the previous statement then you must agree that the level of influence the community has on product development is negligible and it is clear that internal goals and backlogs are prioritized over what the community wants. For instance, decommissioning the good old UserVoice sites (replacing it with a lesser [Feedback Portal][10] and thus masking the true creation dates of entries in the old platform) does not make me forget the amount of useful functionality proposed by the community which just wasn't picked up... ever! Some entries being created in 2017 (i.e. 6 years ago), upvoted in an incredible manner, just to never be prioritized by Microsoft product teams. If that isn't a continuous 'slapping in the face' of the community, I don't know what is.  
So fact is Microsoft do **talk** a lot about listening to the community but **do not really act** on it. Adopting FOSS for the Teams product is a sure strategy to be listening very closely!

![UserVoice through Way Back Machine](/assets/images/post-my-wish-list-for-microsoft-teams-in-2023-002.png)
*UserVoice (decommissioned throughout 2021) through Way Back Machine*
{: style="border: 1px solid grey; margin-left: 20px; padding:16px; font-size:10px"}

{:start="4"}
4. Since having the Commercial license of the product would be the only way to:
   1. have access to **enterprise support**;
   2. have Microsoft **host the service** for you as a SaaS offering;
   3. have your **company branding** present in the Client Apps;
   4. have access to a lot of other **software quality** related upgrades/perks, I can imagine.

    We can always calculate a clear ROI tip-over point for any company to switch the licensing scheme from FOSS to Commercial. It will also force Microsoft to invest in the quality of its first line support again, because it is a real pain at this point in time.  
    The general appeal of the Commercial license is as such directly related to its competitiveness compared to the FOSS license (within the same company). I think this is a very healthy situation, in which the current 'bonus culture' (aka Perks and Benefits) can be justified again and make some actual sense because its now directly related to the quality of the service offered in the Commercial license.
5. Finally, from an **external competitiveness** standpoint, other Open Source projects (like NextCloud with their Hub and Talk Products) that [aim to be the center of Office work][9] are catching up quickly and they have a lot of good things going for them:
    > The overall aim is to have a local, GDPR compliant, open source, standards-compliant, easy to use, performant and decentralized Office which does not suffer from vendor lock-in or data leaks to third parties and countries.

I would like to stress that I am NOT proposing making the Teams software free 'of charge', as I and a lot of companies would be prepared to pay for the hosted service run by a cloud provider at scale with enterprise support, also making reliability and uptime (Microsoft do have a great track record already) very much desired properties. So just to iterate, Microsoft can still make lots of money (as they should!) if this move was contemplated, but in a more healthy, sustainable, community friendly, truly connected, competitive and serviceable manner.

**UPDATE 2023/01/31**: A good first step in doing something 'good' from a licensing point of view Microsoft could start to adopt and advocate the [Principles of Fair Software Licensing for Cloud Customers][13].

## Adopt an 'Omni-browser, Omni-OS, Omni-Arch compatibility' Policy for the Client

No excuses anymore! Ditch the desktop apps and dedicated mobile apps if needed to make room for this effort, but make 'it' work for Chromium (Chrome, Edge, Brave, etc.), Webkit (Safari) and Gecko (Firefox), on all OS-es (Windows, MacOS, Linux and iOS/Android), on all major architectures (x86, ARM). OK, RISC-V may be skipped ;-).

'The battle of the App Stores' is so 2010... all we need is a browser!
{: .notice--info}

Of course 'it' should mean **achieving and maintaining** 'feature parity' across the board. Desktop Sharing and incoming/outgoing Video calls should just work all the time, on every device, on every architecture, on every OS, on every browser.

{% capture notice-text %}
I would like to bring my ARM-based RaspberryPi, with Debian and Firefox to the toilet, plug it into the HDMI touchscreen display that hangs there and want to video call my mum? Right!
{% endcapture %}

<div class="notice--primary">
  <h4 class="no_toc">What if...</h4>
  {{ notice-text | markdownify }}
</div>

If you would count all the wasted hours this year, globally, that where due to incompatibilities or unsupported configurations during video calls we could probably feed a large third world country or fund NATO for the same time frame from the lost revenues[^2].

## Introduce a New 'Blazingly Fast' Client

The New Teams 2.0 client (already released for Windows 10/11 Consumers) has [swapped Electron for WebView2][1] as its core. While performance has drastically improved it still makes it a product not to my liking (i.e. review the first two items on this list). WebView2 is proprietary and is notoriously 'chatty'. So it is very much is a privacy nightmare and potentially a security one as well (remember ActiveX?).

After the prerequisites above are indeed satisfied we can finally turn to the software itself.

My wish would be: leave the telemetry out of the client this time (or make it opt-in at least) and please make it perform as fast (read *Super* Fast!) as possible. The amount of time we (the human race) collectively waste on slow apps is truly enormous. Furthermore the train of thought to log all data all the time needs a radical overhaul if only to conserve energy "for a brighter future".

If anything, I prefer to create 'Web Apps' for any service I regularly visit, but not the kind (i.e. PWA) that only work on a limited type of browsers (i.e. Chromium and Safari-based).  
While I am sure that Microsoft did try to please, it seems the effort did not resonate well with the FOSS community. I am deducing this from the backlash in [the announcement thread][2].  
Mozilla leaves also something to be desired by the way, as they decided to scrub PWA support from their Firefox browser somewhere along the line, which I think is a major oversight. I am not alone in this as it is ranked **number one** on [Mozilla's Idea platform][7].

As a side note for Linux an excellent alternative to PWAs is available called 'WebApp Manager'. This allows you to create desktop entries to a URL using any browser you have installed on your system. These Apps will be presented without any browser chrome, just like PWAs. [Read more about it][5].
{: .notice--info}

UPDATE (2023/01/17): While Microsoft states [meetings on Firefox are still unsupported][6]. I can say that the experience in Firefox 108+ under Linux with Firefox (at least with XOrg) is now on par with other OS-es and browsers:

1. **Screen sharing** (both incoming and outgoing) is working.
2. As is **video calling** (also both incoming and outgoing) is working.
3. **No 'user agent switching'**, which was a common hack is needed anymore.

I must congratulate Microsoft and Mozilla for collaborating and make this happen, as it was a long standing issue.  
You can even test audio/video/screen sharing capabilities (without bothering your colleagues) yourself by inviting yourself to a meeting you started on one device (desktop) and joining the call anonymously on another device (phone). Excellent!

## Achieve Feature Parity for the Microsoft Graph API

For developers their certainly are some things to be desired when it comes to manipulating Teams through its APIs. I would like to propose that all functions, entities, operations, etc. for each individual feature in the GUI landscape of the Microsoft Teams product should be exposed in the all encompassing backend API called the Microsoft Graph **immediately upon release of said feature**.

Important to maintain, from then on, are several things:

1. Every new feature introduced to Teams should be backed by the Graph from the get-go. Without good thinking about architecture, governance, compliance, automation and backend APIs while developing a feature (i.e. I mean really thinking it through!) will unequivocally lead to a nightmare like we saw (and still seeing?) with the 'Private channels' feature, the 'Shared channel' feature, or why copying and moving files in SharePoint and OneDrive behaves differently when using the Browser as compared to using the Sync Client and executing the mnetioned actions a) in the same library, b) between libraries in the same site and c) between libraries in different sites also deliver different results. Explain that to a end-user...
1. Operations for both **Application** permissions (**Roles**) and **Delegated** permissions (**Scopes**) should be available and supported from the moment a feature is released as GA. A big pain-point of mine still is why I can't [update a Team Logo/Photo for a freshly created Team][3] using the same Application (Role) permissions I use to provision that Team?

It is my strong opinion that any release should be postponed if above conditions are not met.

Delivering end-user functionality without 'really' thinking about the back-end, compliance possibilities and how such a feature is positioned in the overall SKU landscape, like we saw with the dreaded 'Teams Wiki' feature, is bound to [bite you in the ass][11] at some point.

## Solve Compliance Issues

Just to name one example (the list is too long, btw). Backing-up and Restoring Microsoft Teams has changed from being ['hacky' and error prone to a pretty expensive commercial offering][4] which still needs to proof itself. Getting to a certain **guaranteed** point in time for all parties (considering assets might be 'shared') and SKU's involved remains next to impossible. Because of the 'connected, but separate' nature of all Teams' services (i.e. Exchange, OneDrive, SharePoint, Yammer, Planner, Stream, PowerBI and more using various (External) Sharing schemes) this is to be expected to some extend, but if Microsoft really wanted this to work and made it a priority, a lot more interdepartmental cooperation within Microsoft would be needed, stimulated and ultimately enforced.

Microsoft Purview (The Microsoft 365[^1] Compliance Layer, with a lot of back-end pieces in the Exchange domain) should be more aligned with the Microsoft Graph as well. [Some operations are still only possible through the Web GUI, which allows for user error][12]. They also cost enormous amounts of time to maintain, time nobody haves or wants to invest. In my view Compliance and GUI's exclude one another. True compliance enforcement requires a stable back-end API, set once and run continuously through a pipeline.

## Conclusion

Personally I think all the paragraphs above deserve far more attention than simply 'continuously adding new features to the product that, let's face it, nobody asked for'.  
Take a step back, look at the product and fix the parts that were neglected and go add stuff the community explicitly asked for and most importantly: **communicate** why certain highly demanded features will probably never make it in the product. The community I know will be very understanding if given the proper reasons why something 'can't be done'.

I know I proposed a lot of fundamental changes to get Teams in a better shape, most of which require changes in perception from senior management. Nonetheless, you may or might not agree with any of what was written. If you want to share your opinion, feel free to do so in the comments.

<!-- Begin FootNotes -->

[^1]: Microsoft 365, formerly Office 365, is a line of subscription services offered by Microsoft which adds to and includes the Microsoft Office product line. The brand was launched on July 10, 2017, for a superset of Office 365 with Windows 10 Enterprise licenses and other cloud-based security and device management products.
[^2]: I have no real numbers btw, so a grain of salt here. But just imagine the major costs involved...

<!-- End FootNotes -->

<!-- Begin Abbreviations -->

*[FOSS]: Free and Open Source Software as defined by the Free Software movement and the Open Source Software movement.
*[WSL]: Windows Subsystem for Linux (WSL) is a feature of Windows that allows developers to run a Linux environment without the need for a separate virtual machine or dual booting.
*[PWA]: Progressive Web Apps
*[ROI]: Return On Investment

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://www.thurrott.com/podcasts/269114/microsoft-teams-now-60-faster-but-when-will-teams-2-0-arrive
[2]: https://techcommunity.microsoft.com/t5/microsoft-teams-blog/microsoft-teams-progressive-web-app-now-available-on-linux/ba-p/3669846
[3]: https://learn.microsoft.com/en-us/graph/api/profilephoto-update?view=graph-rest-1.0&tabs=http
[4]: https://office365itpros.com/2022/07/26/teams-backup-export-api/
[5]: https://fosspost.org/how-to-install-webapps-on-linux/
[6]: https://learn.microsoft.com/en-us/microsoftteams/limits-specifications-teams#browsers
[7]: https://connect.mozilla.org/t5/ideas/idb-p/ideas/tab/most-kudoed
[8]: https://www.qt.io/licensing/
[9]: https://betanews.com/2022/08/26/nextcloud-works-with-governments-to-create-ms-office-rival-for-the-eu/#comments
[10]: https://feedbackportal.microsoft.com/feedback/forum/ad198462-1c1c-ec11-b6e7-0022481f8472
[11]: https://petri.com/m365-changelog-wiki-retirement-and-the-future-of-note-taking-in-teams-channels/
[12]: https://github.com/MicrosoftDocs/office-docs-powershell/issues/8121
[13]: https://www.fairsoftware.cloud/principles/

<!-- End References -->
