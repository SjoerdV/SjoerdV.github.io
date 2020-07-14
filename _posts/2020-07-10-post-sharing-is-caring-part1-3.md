---
title: "Sharing is caring! Or taking your next step toward better SaaS Governance and External Identity Management for Microsoft 365 - Part 1 of 3"
date: 2020-07-13T10:00:00+02:00
last_modified_at: 2020-07-13T10:00:00+02:00
header:  
  og_image: /assets/images/2020-07-11-01-08-42.png
categories:
  - blog
tags:
  - InformationManagement
  - IdentityManagement
  - DataGovernance
  - MicrosoftTeams
  - SharingIsCaring
  - AzureAD
  - AzureADB2B
  - ConditionalAccess
  - MCAS
  - Microsoft365
  - AzureDevOps
  - AzurePipelines
  - PlatformAsCode
  - StarterKit
---
<!-- Begin Abbreviations -->
*[MCAS]: Microsoft Cloud App Security
*[OOTB]: Out-of-the-Box
*[CA]: Conditional Access
*[MFA]: Multi Factor Authentication
*[SaaS]: Software as a Service, in this context the Microsoft 365 and Azure AD ecosystems are depicted
*[PaC]: Platform as Code
*[tenant]: A representation of an organization. It's a dedicated instance of shared Microsoft SaaS products offering Office 365 or Azure AD services.
<!-- End Abbreviations -->

This is the first part of a three part blog series, building on the premise to open up your Microsoft 365 Cloud Service tenant to External Parties. This first part will describe deciding on *the level* of 'external sharing' your organization cares to embrace.

## Security vs Usability

When you, your colleagues and/or managers are burdened with the tremendous task of traversing the company through the COVID19 crisis by continuing delivering your services **AND** at the same time making sure all employees can stay as productive as they were before this calamity **AND** maybe even enhance their experience **AND** consolidate the collaboration tools available to employees for a more favorable cashflow and stay a healthy business, you justly decide to make an investment in the cloud and have Microsoft help you in your quest.

Incoming Microsoft 365, the latest and greatest! Run the wizard, import your users to the cloud, assign licenses, fiddle with some Teams settings and... Enter your first Teams meeting!

I wonder how many companies, especially SMB, have done exactly that. Or even sign-up for the ['free' Microsoft Teams campaign][4] Microsoft launched in April 2020. Seeing the huge [increase in Teams usage][5] doesn't really say anything about how many **new** companies have adopted the Microsoft Cloud however. In any case it does mean that existing companies have activated the Microsoft Teams potential and might have done so by flicking a single switch in the Teams Admin center.

Which ever case may be true for your company one of the most important policy decisions your company has to make is how to deal with (or re-evaluate) this particular setting in the [OneDrive Admin Center][1]:

![External Sharing in OneDrive AC](/assets/images/2020-07-11-01-08-42.png "External Sharing in OneDrive AC"){: style="border: 1px solid grey; padding:16px"}
*Figure 1: External Sharing in OneDrive Admin Center*

The same setting is available in the [SharePoint Admin Center][2] (copy/paste this link and change the 'yourtenant' part):

![External Sharing in SharePoint AC](/assets/images/2020-07-11-01-29-40.png "External Sharing in SharePoint AC"){: style="border: 1px solid grey; padding:16px"}
*Figure 2: External Sharing in SharePoint Admin Center*

## External Sharing

If you have not seen these sliders before or did not plan some time to reconsider the existing setting and you already have enabled Microsoft Teams for your company then you might want to check and discuss this External Sharing configuration with IT Management and Business policy makers. This is why.

When you created your Office 365 tenant, in a hurry to get started with Microsoft Teams, (or flicked that switch to enable Teams) chances are your tenant came with the most relaxed external sharing settings available: **all sliders to the top.** You might not have realized or ignored the fact that Teams uses the SharePoint Online service as its file storage backend for Teams 'Teams' and the OneDrive storage backend for Teams 'Chats'. So why even bother looking at the SharePoint or OneDrive Admin Centers, if all you want is Teams, right? Well...

While leaving everything, as you got it: OOTB, is the most simple, speedy and goal-fulfilling course of action and furthermore people have noticed that sharing files on their personal OneDrives gives them options like 'Anonymous Sharing Links' which can even 'expire' properly. Feels safe enough, right? You have to ask yourself: *"Is this for now?"*, *"Do I want everyone, even the janitor with an account to allow him/her to call in sick, to be able to invite external users?"*, *"Do I want people I don't know having the possibility to get to my company's data or privacy sensitive data? Period?"*, *"How easily does data leak from those 'Public' Teams I heard about?"*.

Good questions!

Now, the images displayed on this page are of course not random. These settings represent what I, in my personal professional opinion, consider the best balance between security and usability for a large majority of organizations when External Sharing is concerned. Of course I am interested in what you, the reader, think of this. In any case the proposed 'slider settings' will:

- not allow any company employee to add external users to sites, teams, files or whatever company asset in the Microsoft 365 ecosystem directly
- need you to adopt the much more manageable, secure and compliant [**Azure B2B**][3] concept, by giving select, trusted and responsible employees the much coveted 'Guest Inviter' Azure AD role. Trust me, they will love you for it!
- allow you to apply CA / Terms of Use policies for External Users specifically, like forcing their use of MFA. Which is a no-brainer surely!
- give you the possibility to 'Sandbox' the External User and monitor and/or restrict their activities on your tenant by configuring MCAS[^1] appropriately. Of course not without giving the External User clear indication that this is happening.

Some clear internal and external communications, change management, organizational prowess and knowledgeable people are surely needed to facilitate this, but it is better to undertake this early in the game than later on, when you might have regretted the initial 'wide open' policy and when people have become accustomed to very relaxed security settings and resist the new measures.

Note: Be aware that changes in the sliders depicted in this post (from relaxed to more restricted) might not be reflected on existing containers (SharePoint Sites / Personal Sites). For instance unless you completely disable 'External Sharing' (not recommended!) but only restrict it, external users who have been given access to a site with a more relaxed global setting will still have access.
{: .notice--info}

Note: You might consider dropping the OneDrive slider one nudge to *'Only People in your Organization'* if you are not comfortable relying on some fancy 'Security by Obscurity' as the settings depicted in the images do allow for ***any*** OneDrive owner to 'guess' an external email address that has been added by a Guest Inviter to the organization for their File Sharing purposes. Personally I think that this risk is negligible for regular companies and if you have the CA/MCAS combo in place you have full audibility. But sure, there still still is that risk of data leakage.
{: .notice--warning}

## Next time

The next blog in this series will explain a modern way of *applying* your selected configuration in a controlled manner and offer a 'Starter Kit' of sorts which you can use in your organization to deploy Microsoft 365 SaaS configuration.

## Bibliography

[^1]: if you have at least some Azure AD Premium Plan 2 licenses available

<!-- Begin References -->
[1]: https://admin.onedrive.com/?v=SharingSettings
[2]: https://yourtenant-admin.sharepoint.com/_layouts/15/online/AdminHome.aspx#/sharing
[3]: https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b#easily-add-guest-users-in-the-azure-ad-portal
[4]: https://support.microsoft.com/en-us/office/sign-up-for-teams-free-70aaf044-b872-4c32-ac47-362ab29ebbb1
[5]: https://office365itpros.com/2020/04/30/office365-teams-power-growth/
<!-- End References -->
