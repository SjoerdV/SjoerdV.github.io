---
title: "Sharing is caring! Or taking your next step toward better SaaS Governance and External Identity Management for Microsoft 365 - Part 2 of 3"
date: 2020-07-20T10:00:00+02:00
last_modified_at: 2020-07-20T10:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/sharing-is-caring-part2-3-001.png
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
  - YAML
  - PlatformOps
  - PlatformAsCode
  - StarterKit
  - PowerShell
  - PnP
---
This is the second part of a three part blog series, building on the premise to open up your Microsoft 365 Cloud Service tenant to External Parties. This second part will describe the *method* of applying the configuration you and your organization decided on.

The [first part][1] described deciding on *the level* of 'external sharing' your organization cares to embrace.

## Click or automate? That is the question

So you have decided to take control of External Sharing. Now it is time to consider, ***how*** to make the change. This truly is the 'core' question of this post series.

At first, you might be tempted to change the necessary settings through the GUI, Microsoft has so graciously provided, and then documenting that change in some large document filled with screenshots and reference links? Consider the following:

1. Isn't a GUI built for browsing, quickly gaining insight and maybe even change a limited, foreseeable and predictable amount of configuration? In this day and age should you really be using it to configure the vast amount of settings that could have its location in the GUI changed without notice? Their goes your carefully organized ever expanding 'Settings Word document'.
1. Haven't you ever made a mistake while clicking through the GUI? Of course quickly trying to undo what you did, but is rollback even possible? Who will know or even find the specific setting you changed?
1. There are tens or maybe hundreds of thousands of settings in the Microsoft 365 ecosystem. Writing down all those settings in 'a large Settings Word document' and effectively trying to duplicate what was is already configured doesn't seem a very efficient and error prone.
1. How to document the settings that are not exposed through a GUI? You'll probably have to copy paste some line of code inside the document as well.

To keep track of all settings configured in Microsoft SaaS services and making sure they are applied you need to consider [PaC][2]. Additionally you need to consider having a single authorized and trusted pipeline that has the necessary (manual) 'gates' or 'approvals' configured which will deploy your configuration over and over again after each validated change. Continuously! Especially in a Microsoft ecosystem where you already have your identities in the cloud it is only logical to use the power of [Azure Pipelines][3][^1] for this purpose.

![PaC Architecture for Microsoft 365 Cloud Service deployment](/assets/images/sharing-is-caring-part2-3-001.png "PaC Architecture for Microsoft 365 Cloud Service deployment"){: style="border: 1px solid grey; padding:16px"}
*Figure 1: PaC Architecture for Microsoft 365 Cloud Service deployment*

## PlatformOps

The proposed idea is based on Microsoft's own ['PlatformOps' initiative][2] which in turn makes use of the Zero Trust mindset as explained in many articles on the Internet. In Zero Trust, PaC deployments (in our case simplified to deploying Microsoft SaaS configuration only) can only be performed by a trusted pipeline and no one single individual.

Some additional good reads are:

* [Implementing a Zero Trust approach with Azure Active Directory][7], Author(s): Philippe Beraud, Jean-Yves Grasset, Arnaud Jumelet (Microsoft France), June 2019
* [PlatformOps in a Microsoft Enterprise-scale landing zone][8], Author(s): [Anders Bonde][9], July 2020

The grand scheme of implementing PlatformOps might only be feasible for Fortune 500 companies and large enterprises, because of the cost involved maintaining such an infrastructure. However, in my opinion, small scale initiatives based on it, like the one proposed in this post, certainly are achievable for much smaller companies and can deliver on fully rationalizing any configuration. In turn this would and should lead to better and faster troubleshooting, because all configuration is known (transparent) to anyone with access to the repository hosting the configuration. It would lead to faster time to market for additional solutions and configurations, because implications can be easily tested up front. And because the configuration is both 'actionable', 'searchable' and 'commentable' it can serve perfectly as 'single point of truth' documentation for your Microsoft 365 landscape.

> Personal note: In my opinion Microsoft is not quite of the hook here.
>
> 1. I would applaud it if the product groups could devise a policy that if a setting, policy or other configuration object in the full scope of the Microsoft 365 ecosystem was previously set by means of the trusted pipeline credentials or with a specific App Principle / Service Account, that this would prevent any other individual user with an Admin role from making a change on that particular setting, policy or object. Although regular Admins should be able to *read* all information in the Admin Centers and Portals they have permissions for, all configuration changes that impact organizational IT policies are now and should be managed by our fancy new pipeline, right?
> 2. Some licensing conditions are currently not particular suited for this setup. In particular you would need at least two but preferably more totally disjoint but identical Microsoft 365 tenants to test and accept deployed settings. The fact that Microsoft does not give out more than one Education licensed tenant per organization for instance, would mean that some features can not be tested properly in those organization. For example, if you want to test a policy or setting concerning 'Class Notebooks' these would only be available on your Production tenant.
{: .notice--info}
^
If you are interested in getting started with an **Azure DevOps for Microsoft 365 - Starter Pipeline,** have a look at my [GitHub 'CaCAzureDevOpsPipeline' Project][6] featuring a Configuration-as-Code scaffold holding your Microsoft 365 SaaS Configuration with a basic multi-stage conditional YAML pipeline. This repository comes with instructions.
{: .notice--success}

Now you are in control *how* and *when* your configuration is applied without the need for any user interaction nor through any GUI activity, Truly a big step!

## Configuring the pipeline

Ok, so no GUI-'tinkering'... use a pipeline... What should be in such pipeline, then? Microsoft have gone out of their way to have almost anything you can configure in the Microsoft Cloud, exposed through some sort of an API. To make the best use of any API, for me that is, is through **PowerShell**. Considering the availability of [PnP PowerShell][4][^2] module, you might start using it! These are the commands you need, scoped to the subject of this article series:

```powershell
# 1. Tenant Level External Sharing Settings (Used by all individual SharePoint Online Sites)
Set-PnPTenant -SharingCapability:ExistingExternalUserSharingOnly
Set-PnPTenant -DefaultLinkPermission:View
Set-PnPTenant -DefaultSharingLinkType:Direct
# 2. OneDrive Level External Sharing Settings (Inherited by all individual OneDrive Personal Sites)
## Only set this if you want your OneDrive to behave differently (more restrictive) than Tenant Level settings
Set-PnPTenantSite https://yourtenant-my.sharepoint.com -Sharing:ExistingExternalUserSharingOnly
Set-PnPTenantSite https://yourtenant-my.sharepoint.com -DefaultLinkPermission:View
Set-PnPTenantSite https://yourtenant-my.sharepoint.com -DefaultSharingLinkType:Direct
```

Now it's up to you to embed a declarative configuration such as the one above in your own pipeline, maybe use a more generic approach than the lines above. Also have a look at the [2020-07: Upcoming PnP PowerShell developments and recommendations][11] post I wrote on what to expect concerning PnP PowerShell and the use of 'App Only' authentication methods.

The 'Starter Pipeline' I mentioned earlier already has username/password as well as App-Only authentication covered.
{: .notice--info}

## Next time

The next blog in this series will explain *managing* the lifecycle of the identities the 'External Sharing' configuration will produce and offer insight in how to automate this using the 'Starter Kit' offered in this post.

<!-- Begin FootNotes -->
[^1]: Part of Azure DevOps, which is a crucial service fully integrated in your Azure AD environment
[^2]: Simply the best community driven PowerShell module development initiative ever.
<!-- End FootNotes -->

<!-- Begin Abbreviations -->
*[MCAS]: Microsoft Cloud App Security
*[OOTB]: Out-of-the-Box
*[CA]: Conditional Access
*[MFA]: Multi Factor Authentication
*[SaaS]: Software as a Service, in this context the Microsoft 365 and Azure AD ecosystems are depicted
*[PaC]: Platform as Code
*[tenant]: A representation of an organization. It's a dedicated instance of shared Microsoft SaaS products offering Office 365 or Azure AD services.
*[PnP]: Patterns and Practices initiative. Embraced by  Microsoft, supported by the community
*[API]: Application Programming Interface, a collection of dedicated Uri's built to deliver specific responses based on specific input without the overhead of a Graphical User Interface.
*[pipeline]: A pipeline in the context of Microsoft Azure Pipelines is a full-featured continuous integration (CI) and continuous delivery (CD) service. It works with your preferred Git provider and can deploy to most major cloud services, which include Azure services
*[YAML]: Yet Another Markup Language
*[GUI]: Graphical User Interface
<!-- End Abbreviations -->

<!-- Begin References -->
[1]: /blog/post-sharing-is-caring-part1-3/
[2]: https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/platform-automation-and-devops
[3]: https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops
[4]: https://github.com/pnp/PnP-PowerShell#installation
[5]: https://pnp.github.io
[6]: /CaCAzureDevOpsPipeline
[7]: https://www.microsoft.com/en-us/download/details.aspx?id=58366
[8]: https://www.linkedin.com/pulse/platformops-microsoft-enterprise-scale-landing-zone-anders-bonde
[9]: https://www.linkedin.com/in/bonde
[10]: https://www.erwinmcm.com/pnp-teams-cmdlets/
[11]: /blog/post-pnp-powershell-developments/
<!-- End References -->
