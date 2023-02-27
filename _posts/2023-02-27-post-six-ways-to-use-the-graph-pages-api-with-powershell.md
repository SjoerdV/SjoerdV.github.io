---
categories:
- blog
date: 2023-02-27 10:00:00+02:00
header:
  og_image: /assets/images/post-six-ways-to-edit-sharepoint-pages-with-powershell-001.png
last_modified_at: 2023-02-27 10:00:00+02:00
og_publish_date: 2023-02-27 10:00:00+02:00
tags:
- ApiImplementations
- PreferredApis
- ApiEndpoints
- MicrosoftGraph
- MicrosoftGraphApi
- MsGraphSitepagesApi
- SharepointRestApiV2
- SharepointRestApiV1
- Csom
- Jsom
- Soap
- ModernSharepointPages
- PowerAutomate
- StandardSharepointRestConnector
- PowershellSolutions
- Powershell
- PnPPowershellModule
- Scripts
title: Six Ways to Edit SharePoint Pages with PowerShell
toc: true
toc_icon: list-alt
toc_label: Contents
toc_sticky: true
---

Six ways, really? Certainly!  
Although I will only dive into the two most prominent ways because they are the most recent and most recommended, but I wanted to show the long history of API implementations that SharePoint has known and still maintains up until today. That's 'backward compatibility' for ya!

With the [introduction of the new MS Graph sitePages API][1] (in beta), Microsoft added a great way to manipulate modern SharePoint pages in addition to the already present methods.

The **two** methods I will actually implement in this post are:

* [SharePoint REST API v2][7] (using Graph endpoint translations)
* [Microsoft Graph API][11] the [beta version][12] specifically (using direct Graph endpoints)

There also exist the **three** 'old school' CSOM API implementations that I must mention but will not get into because they are not being actively developed. In my opinion they also constitute a pending? 'Technical Dept' if they are still used in any organization.

* the '[.NET Framework redistributable assemblies][10]' implementation
  * example: [SharePoint Online Management Shell][2]
  * example: [PnP PowerShell][3] (Archived)
* the '[SharePoint REST API v1][8]' implementation
  * example: [PnP PowerShell][3] (Archived)
* the '[JavaScript library (JSOM)][9]' implementation
  * no PowerShell examples

Finally I want to mention **one** really old, deprecated, but still operational '[ASP.NET (asmx) web services][4]' SOAP API implementation. Just enter `https://<tenant>.sharepoint.com/_vti_bin/lists.asmx` and you know what I am talking about.  
I was wondering if authentication still worked against this API, since Azure AD had a lot of changes over the years, as did the OAuth authorization protocol and [it seems it does][5]. Remarkable!  
Maybe it is time Microsoft brought this API down, just to make the service a bit safer? I am getting a CAML shiver even writing this. Just a suggestion...

![Six Ways to Edit SharePoint Pages with PowerShell](/assets/images/post-six-ways-to-edit-sharepoint-pages-with-powershell-001.png)
*Six Ways to Edit SharePoint Pages with PowerShell*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

## Preparation

I prefer leveraging API's through PowerShell, as it still is the most flexible tool for the job that can be handled by both IT Pros and Developers alike. In this case both preferred APIs are handily available with the very capable [PnP.PowerShell module][6]. So install that module preferably on top of [PowerShell 7+][14].

NOTE: Alternatively for Method 2 below you could also use the [Microsoft Graph PowerShell SDK][16] module or my personal favorite: the lean and mean [EasyGraph][17] module.
{: .notice--info}

ATTENTION: If you get any errors running the scripts below on more 'complex' pages and are running the current release version (1.12.0) or below of the PnP.PowerShell module, you might want to consider installing a [nightly version][15] of the module. A [fix][18] was committed (thanks Gautam Sheth!).
{: .notice--warning}

In any SharePoint Site you would like, just create and publish a single modern page with a single section containing a single 'Text' WebPart containing the text 'I want more.'.  
You should have at least contribute permissions for the library and no further authoring settings on the page library should be active, just to rule out any 'strange-ness'.

![A simple SharePoint webpart page](/assets/images/post-six-ways-to-edit-sharepoint-pages-with-powershell-002.png)
*A simple SharePoint webpart page*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

Open a terminal and make a [connection with SharePoint][13] (and the Graph) running `Connect-PnPOnline`. Put the site URL in a variable called `$SiteUrl`.

NOTE: While using The Graph you can connect to the root site of your tenant (if you have permissions) and do all operations from there.
{: .notice--info}

Now we are going to update the text on this page. You could imagine a scenario where someone in your organization would publish pages with some 'placeholder text' which can be replaced after running a script like the ones that follow.

## Method 1: SharePoint REST API v2

NOTE: The API endpoints used in this example can also be used with standard SharePoint REST connector in Power Automate.
{: .notice--success}

Please pay attention to the '`ONE TIME`' remarks (there are two per method) in the scripts below, you will have to fill in something yourself.

```powershell
######################
# Fetch Page Details #
######################
## place the GET call for sites
$uri = "https://$SiteUrl/_api/v2.0/sites?`$select=id,webUrl,title"
$result = Invoke-PnPSPRestMethod -Url "$($uri)" -Method "GET" -ContentType "application/json;odata=nometadata" -ErrorAction Stop
### ONE TIME: print the '$result' variable and put the desired site id in the '$siteid' variable below
$siteid = "$SiteUrl,11111111-1111-1111-1111-111111111111,22222222-2222-2222-2222-22222222222"

## place the GET call for pages passing the site id
$uri = "https://$SiteUrl/_api/v2.0/sites('$siteid')/pages?`$select=id,title"
$result = Invoke-PnPSPRestMethod -Url "$($uri)" -Method "GET" -ContentType "application/json;odata=nometadata" -ErrorAction Stop
### ONE TIME: print the '$result' variable and put the desired page id in the '$pageid' variable below
$pageid = "33333333-3333-3333-3333-333333333333"

## place the GET call for page canvas passing the site id and page id
$uri = "https://$SiteUrl/_api/v2.0/sites('$siteid')/pages/$pageid/?`$select=id,title&`$expand=canvasLayout"
$result = Invoke-PnPSPRestMethod -Url "$($uri)" -Method "GET" -ContentType "application/json;odata=nometadata" -ErrorAction Stop


#####################
# Do business logic #
######################
## Clean up the page definition to make it ready for manipulation and to write back at some point
$bodyJson = $result | ConvertTo-Json -Depth 100
$newBody = @()
$matchStr = "`@odata|^\s\s`"id`":"
foreach ($line in $($bodyJson -split "\n")) {
  if ($line -notmatch $matchStr) {
    $newBody += $line
  }
}
## Replace the text
$newbody = $newBody -replace 'more','much more'
## Convert to a single string
$body = $newBody -join '' #IMPORTANT: Convert the result to a single string


###################
# Update the Page #
###################
## place the PATCH call to update page contents
$uri = "https://$SiteUrl/_api/v2.0/sites('$siteid')/pages/$pageid"
$result = Invoke-PnPSPRestMethod -Url "$($uri)" -Method "PATCH" -Content $body -Raw -ContentType "application/json;odata=nometadata" -ErrorAction Stop


####################
# Publish the Page #
####################
## place the POST call to publish
$uri = "https://$SiteUrl/_api/v2.0/sites('$siteid')/pages/$pageid/oneDrive.publish"
$result = Invoke-PnPSPRestMethod -Url "$($uri)" -Method "POST" -ContentType "application/json" -ErrorAction Stop
```

## Method 2: Microsoft Graph beta API

WARNING: The API endpoints (prefixed with https://graph.microsoft.com/) used in this example can only be used with premium HTTP connector in Power Automate.
{: .notice--warning}

Please pay attention to the '`ONE TIME`' remarks (there are two per method) in the scripts below, you will have to fill in something yourself.

```powershell
######################
# Fetch Page Details #
######################
## place the GET call for sites
$uri = "beta/sites?`$select=id,webUrl,displayName"
$result = Invoke-PnPGraphMethod -Url "$($uri)" -Method "GET" -ContentType "application/json;odata=nometadata" -ErrorAction Stop
### ONE TIME: print the '$result' variable and put the desired site id in the '$siteid' variable below
$siteid = "$SiteUrl,11111111-1111-1111-1111-111111111111,22222222-2222-2222-2222-22222222222"

## place the GET call for pages passing the site id
$uri = "beta/sites/$siteid/pages?`$select=id,title"
$result = Invoke-PnPGraphMethod -Url "$($uri)" -Method "GET" -ContentType "application/json;odata=nometadata" -ErrorAction Stop
### ONE TIME:print the '$result' variable and put the desired page id in the '$pageid' variable below
$pageid = "33333333-3333-3333-3333-333333333333"

## place the GET call for page canvas passing the site id and page id
$uri = "beta/sites/$siteid/pages/$pageid/?`$select=id,title&`$expand=canvaslayout"
$result = Invoke-PnPGraphMethod -Url "$($uri)" -Method "GET" -ContentType "application/json;odata=nometadata" -ErrorAction Stop


#####################
# Do business logic #
######################
## Clean up the page definition to make it ready for manipulation and to write back at some point
$bodyJson = $result | ConvertTo-Json -Depth 100
$newBody = @()
$matchStr = "`@odata|^\s\s`"id`":"
foreach ($line in $($bodyJson -split "\n")) {
  if ($line -notmatch $matchStr) {
    $newBody += $line
  }
}
## Replace the text
$newbody = $newBody -replace 'more','much more'
## Convert to a single string
$body = $newBody -join '' #IMPORTANT: Convert the result to a single string


###################
# Update the Page #
###################
## place the PATCH call to update page contents
$uri = "beta/sites/$siteid/pages/$pageid"
$results = Invoke-PnPGraphMethod -Url "$($uri)"-Method "PATCH" -Content $body -Raw -ContentType "application/json" -ErrorAction Stop


####################
# Publish the Page #
####################
## place the POST call to publish
$uri = "beta/sites/$siteid/pages/$pageid/publish"
$result = Invoke-PnPGraphMethod -Url "$($uri)" -Method "POST" -ContentType "application/json" -ErrorAction Stop
```

## Result

After running these scripts you should see the text on the page being replaced. After repeated runs it would spell 'I want much much much more.'. In my case I need a more Sashimi now. Cheers!

<!-- Begin FootNotes -->

[^1]: Microsoft 365, formerly Office 365, is a line of subscription services offered by Microsoft which adds to and includes the Microsoft Office product line. The brand was launched on July 10, 2017, for a superset of Office 365 with Windows 10 Enterprise licenses and other cloud-based security and device management products.

<!-- End FootNotes -->

<!-- Begin Abbreviations -->

*[API]: Application Programming Interface, a collection of dedicated Uri's built to deliver specific responses based on specific input without the overhead of a Graphical User Interface.
*[CSOM]: .NET client-side object model
*[.NET]: Dot NET is a developer platform with tools and libraries for building any type of app, including web, mobile, desktop, games, IoT, cloud, and microservices.
*[tenant]: A representation of an organization. It's a dedicated instance of shared Microsoft SaaS products offering Office 365 or Azure AD services.
*[PnP]: Patterns and Practices initiative. Embraced by  Microsoft, supported by the community

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://devblogs.microsoft.com/microsoft365dev/sharepoint-pages-microsoft-graph-api-is-now-available-for-public-preview/
[2]: https://learn.microsoft.com/en-us/powershell/sharepoint/sharepoint-online/connect-sharepoint-online
[3]: https://github.com/pnp/PnP-PowerShell
[4]: https://learn.microsoft.com/en-us/sharepoint/dev/general-development/choose-the-right-api-set-in-sharepoint?redirectedfrom=MSDN#DeprecatedAPIs
[5]: https://stackoverflow.com/questions/35502400/sharepoint-office-365-oauth-service-authentication-to-list-asmx
[6]: https://pnp.github.io/powershell/
[7]: https://learn.microsoft.com/en-us/sharepoint/dev/apis/sharepoint-rest-graph
[8]: https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/get-to-know-the-sharepoint-rest-service?tabs=csom
[9]: https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-javascript-library-code-in-sharepoint
[10]: https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-sharepoint-client-library-code#basic-operations-with-the-sharepoint-net-client-object-model
[11]: https://learn.microsoft.com/en-us/graph/use-the-api
[12]: https://learn.microsoft.com/en-us/graph/api/overview?view=graph-rest-beta&preserve-view=true
[13]: https://pnp.github.io/powershell/articles/connecting.html
[14]: https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.3
[15]: https://pnp.github.io/powershell/#getting-up-and-running
[16]: https://learn.microsoft.com/en-us/powershell/microsoftgraph/installation?view=graph-powershell-1.0
[17]: https://github.com/andlin03/EasyGraph
[18]: https://github.com/pnp/powershell/pull/2802

<!-- End References -->
