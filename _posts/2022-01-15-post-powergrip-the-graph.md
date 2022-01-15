---
title: "Getting a Powerful Grip on the Graph"
date: 2022-01-15T18:00:00+02:00
last_modified_at: 2022-01-15T18:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/post-powergrip-the-graph-001.png
categories:
  - blog
tags:
  - Microsoft365
  - MicrosoftGraph
  - PowerShell
  - EasyGraph
---
The Microsoft Graph is the latest of APIs Microsoft provides for their productivity cloud services. In the past nearly every individual workload in the Microsoft 365[^1] ecosystem had at least one public API made available for broad consumption. This meant Exchange Online, SharePoint Online/OneDrive, Azure AD, etc. offered their own unique ways of requesting and manipulating data and configuration.

![Getting a Powerful Grip on the Graph](/assets/images/post-powergrip-the-graph-001.png "Getting a Powerful Grip on the Graph"){: style="border: 1px solid grey; padding:16px"}

The [Microsoft Graph][3] is aimed to unify these scattered management endpoints and provide a singular set of endpoints that behave in the same way. We are still a long way from this end-goal, but 'The Graph' is proving its usefulness and is getting better every day.

## PowerShell

My favorite way of interacting with any API[^2] is through PowerShell. It's quick, straightforward, cross-platform and does not have a large operational and management overhead.

:pencil2: **Important** \
In the Microsoft 365 ecosystem all API's are only accessible with the right method and details of authentication as you might expect.
{: .notice--info}

Usually any modern cloud-based API exposes REST endpoints (like ```https://mysite/_api/resource('myresource')```) capable of interpreting and executing ODATA operations (like ```/?select=Name&filter='Id -eq 1'```). A great API has all necessary CRUD actions on the actual data covered and also provides some form of modern 'token' authentication flow.

A typical PowerShell command would look like this storing the result in the ```$Result``` variable:

```powershell
$Headers = @{
  "Authorization" = "Bearer $($AuthToken.AccessToken)"
  "Content-type"  = "application/json"
}
$Uri = "https://mysite/_api/resource('myresource')/?select=Name&filter='Id -eq 1'"
$Result = Invoke-RestMethod -Headers $Headers -Uri $Uri -Method GET
```

So that's easy you might say, except well... there are some issues.

## The Issues

### Number One

The first issue I would like to address is that the majority of posts online concerning MS Graph interactions have to do with fiddling with the right headers and tokens needed to get you started on using the Graph. I feel that anyone's time is better spent on creating valuable time-saving automation solutions than have to go through that 'tinkering'-phase.

Just have you Azure AD App registration, consented API permissions and authentication certificate ready and... Start automating!.

### Number Two

Even though Microsoft has provided a [Microsoft Graph PowerShell SDK][4] there are some things that actually make using these modules not so straight forward:

* Installing the entire 'Microsoft.Graph' module collection takes a very long time. It's big! So you better make a selection of specific modules beforehand. Also the time it takes to install all modules makes this particularly unsuitable for 'agent based' operations (like DevOps pipelines) where each execution is run from a 'fresh' agent. In this case the pipeline spends most of its time installing the right modules.
* No examples are provided in the official documentation of the provided [Service Module cmdlets][5]. This makes mapping the actual PowerShell parameter values to the Graph endpoints very hard. The community is clearly not using these modules a lot as well, because there are hardly any practical examples and discussions on the modules to be found online.
* Cross-platform support for App-Only certificate authentication is lacking (as of January 2022)
* Switching from the 'v1.0' endpoints to the 'beta' endpoints is cumbersome and you can easily loose track of which version you are working with
* Because of the inherent nature of PowerShell you might be left out of certain configuration options that are already available in the Graph REST API because a parameter simply has not been implemented yet in the current version of the module. So you are always at least one step behind.

### Number Three

Because modern day APIs all live in the cloud they are subject to:

* unexpected downtime caused by
  * networking issues
  * configuration issues
* high link latency causing time-outs
* upstream service disruption
* service throttling

Almost all of these issues you have no control over, they just happen and may last for quite some time or (even worse) occur intermittently or are linked to a specific time of day where a downtime event is more or less likely to occur.

So you always have to build any solution, but especially unattended PowerShell solutions, with these circumstances in mind. The Microsoft native ```Invoke-WebRequest``` [cmdlet][9] and ```Invoke-RestMethod``` [cmdlet][10] both support ```MaximumRetryCount``` and ```RetryIntervalSec``` parameters and can act on the StatusCode (```Invoke-RestMethod``` had this added in the PowerShell 7 version) of the response. These Retry options act when a response code in the 400 to 599 range and 304 is returned.

You might need some more flexibility.

## The Solution

### EasyGraph

For tackling the first two issues: in comes [EasyGraph][6]. This module developed by Andreas Lindahl (over 2 years ago!) is truly a work of art. As it is basically a wrapper around the Graph REST API it is lean, clean, fast, works cross-platform, provides all methods of authentication you could possibly want, is [clearly documented][7] and is overall super-useful.

Some key features I particularly like are:

* Auditing the code Andreas provided was also a breeze. Version 1.3 was given a clean bill of health: No obscure or ambiguous characters, no telemetry endpoints. All in all: very clean as a whole.
* The module is very lightweight (less then 50KB), it installs in a flash! NOTE: Makes you wonder what Microsoft is putting in their modules to make them so freakishly huge. (Well it is [open source][7], but I daren't look ;-)).
* The module has been untouched, with no issues submitted in the last two years and it just works. Who needs continuous delivery ;-)?
* App-Only Authentication through a PFX certificate file is supported. This makes it is possible to run any code you build using this module cmdlets from a very lightweight Linux machine or docker instance running PowerShell 7+.
* All 'Token handling' is done by the module so there are no worries of token expiration and refreshing
* When constructing API calls just reference the full Graph REST API documentation to know how to format your requests. This documentation includes examples, which makes this an easy chore.
* Paging of results is a built-in feature, just add the ```-All``` switch and you will get all results from a specific endpoint
* All inputs and outputs to and from the module's cmdlets only require and result in **pure** PowerShell 'objects', so you don't have to fiddle with JSON or XML at all.

Obviously this is the module I will be using extensively. Thanks Andreas!

If I had one comment to make it would be that, it doesn't have features to make endpoint calls more resilient and robust, like adding built-in module 'retry parameters'. But this can be fixed to some extend. Read on.

### 'Retry' Code

REST API endpoints like the Graph REST API behave like any normal web service should and send back [standardized return codes][8] like 200, 201, 308, 400, 401, 403, 418 (very important!), 500, 503. When something goes awry (codes 400 up until 599) you need to act on these circumstances and try to mitigate the situation.

The EasyGraph module already has you covered as it treats any response codes the built-in ```Invoke-Webrequest```[11] cmdlet would consider an error as such (except the '429' response code aka 'Next Page').

When the endpoint does not respond at all, of course no return code is sent back, but the function will eventually timeout and return at least a textual error code. Make sure to have the call wrapped in a ```try {...}; catch {...}``` block.

One way of making your interactions with the Microsoft Graph through the EasyGraph PowerShell modules even more robust and resilient is by wrapping the API calls in a Retry function with which you can add (parts of) the return code textual strings to match when an error is expected (See the ```ExcludeErrorMatches``` parameter). This in turn will prevent the EasyGraph call from being retried. Have a look:

```powershell
<#
Reference: https://gist.githubusercontent.com/alexbevi/34b700ff7c7c53c7780b/raw/8925255eb7be0cf4db180b79b86a315b1ca1077c/Execute-With-Retry.ps1
This function can be used to pass a ScriptBlock (closure) to be executed and returned.
The operation retried a few times on failure, and if the maximum threshold is surpassed, the operation fails completely.
Params:
    ScriptBlock         - The ScriptBlock to be executed
    SecondsDelay        - Number (in seconds) to wait between retries
                          (default: 5)
    Retries             - Number of times to retry before accepting failure
                          (default: 5)
    ExcludeErrorMatches - An array of strings that need to be skipped when returned
                          (default: @())
    VerboseOutput       - More info about internal processing
                          (default: false)
Examples:
Start-RetryScriptBlock { $connection.Open() }
$result = Start-RetryScriptBlock -ScriptBlock { $command.ExecuteReader() } -SecondsDelay 1 -Retries 2
#>
function Start-RetryScriptBlock {
  [CmdletBinding()]
  param(
    [Parameter(ValueFromPipeline, Mandatory)]
    $ScriptBlock,
    $SecondsDelay = 10,
    $Retries = 1,
    [array]$ExcludeErrorMatches = @(),
    $VerboseOutput = $true
  )

  $currentRetry = 0
  $success = $false
  $cmd = $ScriptBlock.ToString()

  do {
    try {
      $result = . $ScriptBlock
      $success = $true
      if ($VerboseOutput -eq $true) {
        $write = Write-Output "Successfully executed [$cmd]"
      }

      $Error.Clear()
      return $result
    }
    catch [System.Exception] {
      $currentRetry = $currentRetry + 1
      $errorExcluded = $false

      foreach ($ExcludeErrorMatch in $ExcludeErrorMatches) {
        if ($_.ToString() -match $ExcludeErrorMatch) {
          $errorExcluded = $true
        }
      }

      if (!$errorExcluded) {
        if ($VerboseOutput -eq $true) {
          $write = Write-Output "Failed to execute [$cmd]: $($_.ToString())"
        }

        if ($currentRetry -gt $Retries) {
          $write = Write-Output "Retry limit exceeded. Could not execute [$cmd]. The error: $($_.ToString())"
          throw "Retry limit exceeded. Could not execute [$cmd]. The error: " + $_.ToString()
        }
        else {
          if ($VerboseOutput -eq $true) {
            $write = Write-Output "Waiting $SecondsDelay second(s) before attempt #$currentRetry of [$cmd]"
          }
          Start-Sleep -s $SecondsDelay
        }
      }
      else {
        $Error.Clear()
        $success = $true
      }
    }
  } while (!$success);
}
```

:pencil2: **Important** \
This function can be used to wrap all kinds of 'ScriptBlocks' so not only the EasyGraph module cmdlet's executions. It is very versatile.
{: .notice--info}

### Tying it all together

An example of using the EasyGraph module together with the ```Start-RetryScriptBlock``` function in an actual Graph REST API call would look something like this:

```powershell
# set method
$apiMethod = "GET"
# set version
$apiVersion = "v1.0" # might be either 'v1.0' or 'beta'
# construct uri
$apiUri = "$([System.Web.HttpUtility]::UrlEncode("/groups/$groupid/team"))"
# construct body
$apiBody = $teamconfig | Select-Object memberSettings,guestSettings,funSettings,messagingSettings
# place the call
$apiResult = Start-RetryScriptBlock -ScriptBlock { Invoke-EasyGraphRequest -Resource "$($uri)" -Method "$($apiMethod)" -APIVersion "$($apiVersion)" -Body $body -ContentType "application/json" -ErrorAction Stop } -Retries 3 -SecondsDelay 5 -ExcludeErrorMatches @("Team already exists","\(409\) Conflict")
```

This snippet will create a Team from a Group (the ```$groupid``` (```[string]``` typed) and ```$teamconfig``` (```[pscustomobject]``` typed) variables are implied).

:pencil2: **Important** \
Please note that the ```$apiUri``` variable value is being 'URL encoded' for safety.
{: .notice--info}

:pencil2: **Important** \
Please note that the call might return an error if the Group has already been converted to a Team, so the possibly expected return values are added in the ```-ExcludeErrorMatches``` parameter. This will prevent unnecessary retry actions.
{: .notice--info}

:pencil2: **Important** \
The Graph is usually good to recover after a 5 second pause between retry calls in the event you get throttled. But your mileage may vary, you might need 15 second intervals to be truly safe from throttling. In the above example a single call might take a little more than 10 seconds (i.e. 3 retries with 5 second pauses in between) before truly failing and presumably caught by your code's exception handling.
{: .notice--info}

Power on!

<!-- Begin FootNotes -->

[^1]: Microsoft 365, formerly Office 365, is a line of subscription services offered by Microsoft which adds to and includes the Microsoft Office product line. The brand was launched on July 10, 2017, for a superset of Office 365 with Windows 10 Enterprise licenses and other cloud-based security and device management products. Source: [Wikipedia][1]
[^2]: An application programming interface (API) is a connection between computers or between computer programs. It is a type of software interface, offering a service to other pieces of software. A document or standard that describes how to build or use such a connection or interface is called an API specification. A computer system that meets this standard is said to implement or expose an API. The term API may refer either to the specification or to the implementation. Source: [Wikipedia][2]

<!-- End FootNotes -->

<!-- Begin Abbreviations -->

*[SPO]: SharePoint Online
*[REST]: REpresentational State Transfer
*[API]: Application Programming Interface
*[CRUD]: Create, Read, Update, Delete. Typical database operations on typical entities

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://en.wikipedia.org/wiki/Microsoft_365
[2]: https://en.wikipedia.org/wiki/API
[3]: https://docs.microsoft.com/en-us/graph/overview
[4]: https://docs.microsoft.com/en-us/graph/powershell/installation
[5]: https://docs.microsoft.com/en-us/powershell/module/?view=graph-powershell-1.0
[6]: https://365lab.net/2020/07/28/new-module-easygraph-for-accessing-microsoft-graph-from-powershell/
[7]: https://github.com/microsoftgraph/msgraph-sdk-powershell
[8]: https://en.wikipedia.org/wiki/List_of_HTTP_status_codes
[9]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.2
[10]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-7.2
[11]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.2#example-7--catch-non-success-messages-from-invoke-webrequest

<!-- End References -->
