---
categories:
- blog
date: 2020-07-19 14:00:00+02:00
header:
  og_image: /assets/images/pnp-powershell.png
last_modified_at: 2020-07-19 14:00:00+02:00
tags:
- UpcomingPnpPowershellDevelopments
- PnpPowershellInitiative
- OnPremisesPnpPowershellModules
- SingleMicrosoftGraphApi
- PnpPowershellModules
- PowershellModuleDevelopmentInitiative
- PnpPowershellModule
- MicrosoftGraphFunctionality
- PnpPowershell
- AzureAdEcosystems
- AzureAdServices
- CloudApiDevelopments
- MsGraphDevelopments
- AzureAdApp
- MicrosoftServices
- Microsoft365
- PnP
- PowerShell
- PowerShell7
- DotNet
- MicrosoftGraph
- AzureAD
- AppOnly
- Automation
title: '2020-07: Upcoming PnP PowerShell developments and recommendations'
---

## Upcoming PnP PowerShell developments

At the moment (July 2020), the large variety of Microsoft Services in the Microsoft 365 ecosystem require a lot of separate modules. However, work is ongoing at Microsoft to unify the desired Microsoft 365 and Azure AD operations spanning multiple (if not all?) services in a single Microsoft Graph API. The [PnP PowerShell][1][^1] module is using these MS Graph developments to provide a 'one module solution' and is already equipped to make use of the Graph API. Recently some interesting [Microsoft Graph-based, Teams specific cmdlets][2] were added as well. Great stuff!

![PnP PowerShell](/assets/images/pnp-powershell.png)

As you may have noticed I am a big fan of the **PnP PowerShell** initiative and some exciting things are happening here, like pending support for **PowerShell 7** (which has been built on the platform independent '.NET Core' and has some nice backward compatibility with 'normal' PowerShell). As a consequence:

1. Supporting the various Online and On Premises PnP PowerShell modules will will become a little more difficult as the Microsoft's .NET Standard baseline will drop support for the **SharePoint 'On Premises' CSOM for .NET Framework**  implementation[^2], so apps built on .NET Core will not be able to make use of it, without which you can not hope to manage SharePoint 'On Premises'. Catering to both the Online and On Premises CSOM implementations requirements could prove tough on supportability since their underlying .NET base classes are effectively forked[^3] and Microsoft has no plans on porting cloud API developments to the 'On Premises' products, at the time of writing. Of course the extra dimension (platform independent-ness) .NET Core brings, will surely shift the focus off the 'On Premises' product line for both Microsoft and PnP development teams even more than it already has.
1. It seems that support for using **SharePoint Online basic 'user credentials'** (`SharePointOnlineCredentials` class) to connect to SharePoint through .NET Standard will be dropped as well. This is really not an issue since PnP PowerShell already implements a modern authentication flow so you can keep[^4] using username/password combinations in the Online product as an authentication method (with CA policies applied!)[^5]. The [PnP-Sites-Core][13] project [will probably support this connection method][11] in combination with .NET Standard as well, but we will have to see. That means PnP PowerShell can make use of that. You do need to turn off the '`LegacyAuthProtocolsEnabled`' switch on your Office 365 tenant to actually make use of the modern authentication flow and make sure your account will not trigger an MFA CA rule if you intend on using your script in an unattended manner.

## Recommendations

Considering all this it might be time to change your PnP PowerShell authentication method of choice to [Azure AD App-Only][9] just to be sure you remain fully compatible with the new .NET Standard and Core developments in the future. A nice overview of the various 'Connect options' is being maintained [here][10]. Finally, I would recommend NOT going the ClientId/ClientSecret route and start adopting Certificate/Thumbprint methods as the latter would permit forgoing the somewhat archaic feeling [SharePoint App-Only][14] route if you intend to use PnP PowerShell mainly for  SharePoint Online asset configuration that is. If you intend to use only the Microsoft Graph functionality of the PnP PowerShell modules you could consider the ClientId/ClientSecret route.

<!-- Begin FootNotes -->
[^1]: Simply the best community driven PowerShell module development initiative ever. Part of the [PnP][7] initiative.
[^2]: Read all about .NET Standard support for the various SharePoint CSOM implementation in this [article][4].
[^3]: Read all about the .NET architecture [here][3].
[^4]: Method of connecting to SharePoint in an 'unattended' manner permitting a 'user context' (still needed for some operations, because of legacy CSOM operations) and is both securable with and monitorable by CA policies.
[^5]: This is probably a good thing since the way you should enforce a 'least privilege' methodology for alternate (recommended) 'App-Only'-based connection mechanics is as of yet unknown to me. **First of all** the App-Only credential/certificate approach that is being pushed is an 'all or nothing' approach at the moment. For instance once you grant an app the '`Group.ReadWrite.All`' permission in Azure AD, there is no way to block the app access to a *specific* group/site. Service Principles seem to have too much power and lack fine-grained control as observed by others [here][5] and [here][8] as well. **Secondly** the App-Only approach has some unexpected bugs mentioned [here][6] and [here][12] at the moment. If you have more information on the subject, please leave a comment.
<!-- End FootNotes -->

<!-- Begin Abbreviations -->
*[CA]: Conditional Access
*[SaaS]: Software as a Service, in this context the Microsoft 365 and Azure AD ecosystems are depicted
*[tenant]: A representation of an organization. It's a dedicated instance of shared Microsoft SaaS products offering Office 365 or Azure AD services.
*[PnP]: Patterns and Practices initiative. Embraced by  Microsoft, supported by the community
*[API]: Application Programming Interface, a collection of dedicated Uris built to deliver specific responses based on specific input without the overhead of a Graphical User Interface.
*[CSOM]: .NET client-side object model
*[.NET]: Dot NET is a developer platform with tools and libraries for building any type of app, including web, mobile, desktop, games, IoT, cloud, and microservices.
<!-- End Abbreviations -->

<!-- Begin References -->
[1]: https://github.com/pnp/PnP-PowerShell#installation
[2]: https://www.erwinmcm.com/pnp-teams-cmdlets/
[3]: https://docs.microsoft.com/en-us/archive/msdn-magazine/2017/september/net-standard-demystifying-net-core-and-net-standard
[4]: https://developer.microsoft.com/en-us/microsoft-365/blogs/net-standard-version-of-sharepoint-online-csom-apis/
[5]: https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/37867180-restricting-access-of-azure-service-principals-u
[6]: https://github.com/pnp/PnP-PowerShell/issues/2618
[7]: https://pnp.github.io
[8]: https://microsoftgraph.uservoice.com/forums/920506-microsoft-graph-feature-requests/suggestions/37796059-restrict-permissions-to-app-only-azure-ad-applicat
[9]: https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/security-apponly-azuread
[10]: https://github.com/pnp/PnP-PowerShell/wiki/Connect-options#connect-using-scopes-and-credentials
[11]: https://github.com/pnp/PnP-Sites-Core/issues/2703#issuecomment-658367877
[12]: https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/security-apponly#what-are-the-limitations-when-using-app-only
[13]: https://github.com/pnp/PnP-Sites-Core
[14]: https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/security-apponly-azureacs
<!-- End References -->
