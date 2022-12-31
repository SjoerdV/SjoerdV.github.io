---
categories:
- blog
date: 2020-08-03 10:00:00+02:00
header:
  og_image: /assets/images/teams-together-mode-big-four.png
last_modified_at: 2020-08-03 10:00:00+02:00
tags:
- AnnouncementsGalore
- Announcements
- NewFeatures
- AzureAdEcosystems
- FeatureSprawl
- TodaysMicrosoft
- HistoricDeployments
- BusinessOverview
- OpinionMicrosoft
- MicrosoftSaasProducts
- MicrosoftAzurePipelines
- AzureServices
- Trend
- AzureAdServices
- InformationManagement
- DataGovernance
- GDPR
- Privacy
- Competition
- Awareness
- Outlook
- PowerShell
title: Protect your privates! But first you'll have to find them...
---

When 'getting a business overview', 'mixing business with pleasure' and 'storming consumer markets' come together. Better pay attention and stay focused!

## Announcements galore

Being a responsible, involved IT Pro keeps getting more and more important and difficult in todays Microsoft 365 'feature sprawl'. Every day the Message Centers of various cloud services are bombarded with announcements of introductions of new features. Sometimes little notice is given before 'something' is actually being introduced while an increasing amount of features are solely announced but are not even close to actually being released. The latter leads to a lot of 'static', inaccurate and - later on - 'revised' announcements. Ploughing through this pile of announcements is every Cloud Practitioner's duty of course, but it is becoming exceedingly more and more cumbersome.

![Microsoft 365 Admin Center - Message Center](/assets/images/2020-07-31-16-37-47.png "Microsoft 365 Admin Center - Message Center"){: style="border: 1px solid grey; padding:16px"}
*Figure 1: Microsoft 365 Admin Center - Message Center*

In my opinion Microsoft should focus some efforts on letting IT Pros have the ability to sort and filter on the **date when a feature was or will become effectuated**, on a **per-tenant** and **release ring** basis. The 'data' needed for displaying *historic* deployments is already present in the software giant's deployment pipelines and the *future* data is already in the planning tooling per global region or data center, so why not share it? Furthermore the ability to get a nice overview of features being deployed in the coming month would be something I would applaud. And so doing would make 'policy decision' and 'change advisory' meetings more effective and to the point. Ultimately this would free up time with both Ops and Dev Teams to optimize the platform usage and actually create new solutions or automate recurring tasks.
{: .notice--info}

For the time being IT Pros must always be on a sharp lookout for all kinds of features, evaluating them accordingly and submitting them in a timely manner (if possible!) for discussion and decision-making in IT Governance and Policy-deciding boards/meetings.

## Mind the mix

Another trend that we are seeing is that Microsoft is obviously keen to get back into the consumer market. A great example of this is the latest TikTok acquisition talks that were first [undermined][7] and several days later [resumed again][8] after President Trump interfered on both occasions. A second eyeopener was the introduction of features to business services like Outlook on the Web that allow a user to [mix corporate and personal assets][1] in one environment. Although this particular feature seems 'handy' at first, it's certainly not something that is universally desired from both security and personal mental health perspective. Especially the European norms of sound 'corporate social responsibility' promote a healthy work/life balance, which this policy is undermining. Also these features come with the default setting of 'On' for any given tenant and can only be switched off by running an Exchange Online PowerShell script modifying the default Mailbox policy, like so:

```powershell
Set-OwaMailboxPolicy -TeamSnapCalendarsEnabled:0 -Identity OwaMailboxPolicy-Default
```

![Add TeamSnap events to Outlook on the Web](/assets/images/2020-07-31-21-47-51.png "Add TeamSnap events to Outlook on the Web"){: style="border: 1px solid grey; padding:16px"}
*Figure 2: Add TeamSnap events to Outlook on the Web*

May I suggest you read [managing your SaaS Governance][4] from my previous blog on how to get started automating your SaaS Governance policy deployments.
{: .notice--success}

## Providing steady sustainable future growth

Related to this, the very reason Microsoft was not 'invited' to the Congressional Hearing of last Wednesday July 29th 2020 called ["Online Platforms and Market Power: Examining the Dominance of Amazon, Apple, Facebook and Google."][2] is just that the company is not considered a 'top player' in the Consumer market and given the **consumer** market share over the years that is probably a correct estimation[^1].

Considering this, it might be that Microsoft has found the perfect way of 'growing' in a more sustainable 'under the radar' way (consistently being second or third in a specific market segment, still generating massive revenues and good value for shareholders), being seen as less predatory and greedy than 'the big four' subjected to the hearing and while never being in fear of hostile takeover because of the sheer size and number of markets the company is active in.

![Big Four in Congress (Fake Image)](/assets/images/teams-together-mode-big-four.png "Big Four in Congress (Fake Image)"){: style="border: 1px solid grey; padding:16px"}
*Figure 3: 'The Big Four' together in Congress (Yes image is fake, of course!)*

The biggest issue the four companies most recently being heard are having is that it has become fairly obviously (even to US senators) that they are acting as a 'supplier', 'broker' and 'consumer' in their particular market segments and therefore undermining the forming and sustaining of any healthy competition. Google (Alphabet) have even perfected this by performing the same trick in not one but two market segments (Mobile Apps and Advertisement).

Microsoft however, as a company, seem to have learned from [passed indiscretions][5] to grow without being too obvious, invasive or having a disproportionately large market share in any business sector. Wisdom comes with age, even in 'tech', as it seems! As a side note, the CEOs of the four tech companies have certainly learned from Gates how *not* to behave during such a hearing...

Anyway, being seen as a largely 'B2B focused' IT company and employing some remarkable management folks keeping market shares competitive but not too dominant, is truly amazing and allows for innovation instead of deposing council (again). Keeping on the right side of End Users, Developers, IT Pros and Partners seems to be the biggest challenge Microsoft will continue to face. While at the same time keep resisting the urge of taking decisions for obvious short-term financial gain which will undermine the broad community as a whole.

Providing packages to the 'Open Source' community like the 'Teams Client for Linux' and providing [significant contributions to existing Open Source projects on GitHub][3] surely have already had the desired effect on the communities 'good will'.
{: .notice--info}

## Tying it together

Big lessons could be learned here:

1. Any company, as we should see in the not too distant future, can be deemed **too big**. Therefore keeping all parties involved *reasonably* happy, and this could be a competitor mind you, might be the best move a company can make. But regulations are probably needed to enforce this as such an idea goes against shareholder interests.
1. Keeping your **community** *active,* *close* and *informed* seems like the best long term strategy any company can make. So stop obfuscating stuff and imposing on lives.
1. A persons **life** is not the same as a persons **job**. So stop acting on the opposite.

**I am very interested in your thought on this, please leave a reply to share your insight.**
{: .notice--warning}

<!-- Begin FootNotes -->
[^1]: The graph in [this article][6] shows heavy competition in consumer 'OS' market share as an example. Clearly an indication that this 'market segment' is healthy and working correctly.
<!-- End FootNotes -->

<!-- Begin Abbreviations -->
*[SaaS]: Software as a Service, in this context the Microsoft 365 and Azure AD ecosystems are depicted
*[tenant]: A representation of an organization. It's a dedicated instance of shared Microsoft SaaS products offering Office 365 or Azure AD services.
*[pipeline]: A pipeline in the context of Microsoft Azure Pipelines is a full-featured continuous integration (CI) and continuous delivery (CD) service. It works with your preferred Git provider and can deploy to most major cloud services, which include Azure services
*[GUI]: Graphical User Interface
*[B2B]: Business to Business
*[OS]: Operating System
<!-- End Abbreviations -->

<!-- Begin References -->
[1]: https://office365itpros.com/2020/02/27/adding-your-personal-calendar-owa/
[2]: https://www.youtube.com/watch?v=WBFDQvIrWYM
[3]: https://resources.whitesourcesoftware.com/blog-whitesource/git-much-the-top-10-companies-contributing-to-open-source
[4]: /blog/post-sharing-is-caring-part1-3/
[5]: https://www.youtube.com/watch?v=m_2m1qdqieE
[6]: https://www.extremetech.com/computing/161666-the-demise-of-microsofts-monopoly-the-pc-market-by-the-numbers
[7]: https://www.wired.com/story/trump-order-tiktok-sale-microsoft-circles/
[8]: https://blogs.microsoft.com/blog/2020/08/02/microsoft-to-continue-discussions-on-potential-tiktok-purchase-in-the-united-states/
<!-- End References -->
