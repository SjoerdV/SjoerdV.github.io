---
title: "The importance of solid Software Architecture in a cloud world"
date: 2020-08-12T13:00:00+02:00
last_modified_at: 2020-08-12T13:00:00+02:00
header:
  # image: /assets/images/header-banner.png
  og_image: /assets/images/quint2.jpg
categories:
  - blog
tags:
  - InformationManagement
  - DataGovernance
  - SoftwareArchitecture
  - SoftwareQuality
  - QUINT2
  - Awareness 
  - PowerPlatform
  - PowerApps
  - PowerAutomate 
  - PowerShell
  - Reliability 
  - Efficiency
  - Portability
  - Maintainability
  - Testing
  - Reusability  
  - Manageability
  - Licensing
---
When I was well into my master studies on 'Artificial Intelligence' one of the topics covered in my curriculum of 2001 was concerning Software Architecture. In other words: *"How to build good quality software"*.

The image that stuck with me ever since was that of the Quality Framework Model (ISO9126) called QUINT which was later updated to the [QUINT2 extended ISO model][5].

![QUINT2 extended ISO model](/assets/images/quint2.jpg "QUINT2 extended ISO model"){: style="border: 1px solid grey; padding:16px"}
*Figure 1: QUINT2 extended ISO model*

## Measuring quality

![Quality Check](/assets/images/MultiSelectMirrored.svg "Quality Check")
*Figure 2: Quality Check*
{: style="border: 1px solid grey; padding:16px; margin: 5px; float: right; width: 150px; font-size:10px; color:LightGrey; background-color:black"}

Using this model and ticking off the quality properties in this diagram for each product/tool/service I want to use or adopt always prevented me from making terrible mistakes in choosing the right product/tool/service for the job. The worst thing that could happen is building a solution based on a product/tool/service would inherently be a failure in the long run because of basic design flaws.

Mind you, this model just one of the tools you should employ when choosing or creating any software portfolio. For instance Betamax was the vastly superior video recording format/apparatus compared to VHS in the 1970's, but pricing and marketing is not considered in the QUINT model... Furthermore who even talks about video tapes anymore as they were obsolete as of 1997[^1].
{: .notice--warning}

In this article I want to focus on some of the aspects of Software Architecture that seem to be falling short in a cloud world and express my hopes for the future.

## Reliability and Efficiency

![Reliability](/assets/images/Bullseye.svg "Reliability")
*Figure 3: Reliability*
{: style="border: 1px solid grey; padding:16px; margin: 5px; float: right; width: 150px; font-size:10px; color:LightGrey; background-color:black"}

Ever heard of an *a-synchronous service* or a *JavaScript promise?* These are 'patterns' used to gain some form of **parallelism** in software. The thought being that embracing these practices will 'speed things up' for any reasonably complex Internet-based service where latency and/or an unreliable connection is to be expected.

For instance an individual service can initiate and complete additional tasks while waiting on another service to complete its job.

A side effect of adopting these patterns is that it makes the code that employs them more complex, more abstract and harder to debug and test. I would consider this a problem in the long run.

## Portability

![Portability](/assets/images/CloudImportExport.svg "Portability")
*Figure 4: Portability*
{: style="border: 1px solid grey; padding:16px; margin: 5px; float: right; width: 150px; font-size:10px; color:LightGrey; background-color:black"}

A lot of what has been happening in the vigorous and ruthless software development world has to do with getting great looking, reasonably functional and very marketable software to consumers and businesses alike while at the same time locking the user data and telemetry in this software. At some point in time an ambitious AI/ML technician can have a field day with the data in order to propose/create even more amazing software.

This all seems very logical and is probably the basis of a sound IT business model. The problem of course is that I (the consumer / business) can not really choose to have my data ported to any competitive product easily at any point in time simply because I think the functionally (and/or pricing) is better elsewhere. No, instead this process will cost a lot of manpower and surely some data will be lost.

Microsoft have such software in their SaaS cloud portfolio. Examples include Sway, Whiteboard, Planner, ToDo, OneNote, Flow, PowerApps, Bookings, Forms, Yammer. User data in these services can not easily be transferred to a common format and be used in other products. Probably more of these type of services are in development right now.

NOTE: I deliberately left Microsoft Teams out of this list because it has some major components that are very portable and efforts are ongoing to have more and more related services fall under a more portable regime. For instance I think that it will be possible at some point in time to have a full fidelity back-up possibility for all Teams workloads.
{: .notice--info}

In contrast, services that have a very long track record certainly have portability covered to a large extend. Examples include Exchange and SharePoint.
{: .notice--success}

## Maintainability

![Maintainability](/assets/images/ProvisioningPackage.svg "Maintainability")
*Figure 5: Maintainability*
{: style="border: 1px solid grey; padding:16px; margin: 5px; float: right; width: 150px; font-size:10px; color:LightGrey; background-color:black"}

Being able to maintain a product/service encompasses a few important things that are important in my opinion:

- Manageability
- Testing and Reusability
- Licensing

### Manageability

Code needs to be *readable*, *documented* and *transferable*. Especially in this day and age where people frequently go from one job to the next it is so very important to be able to transfer knowledge as quickly and efficiently as possible.

One thing I personally really 'dislike' is going through some elaborate Word document, which is in fact an abstraction of someone's or a team's idea of what the software does, in the hope someone else who is reading the document can understand what exactly has been build. That is all living under the premise that the documentation is 'up-to-date' with the actual coded solution.
{: .notice--info}

In my view 'Code' and 'Documentation' should in fact go hand in hand within the same asset. Maybe having some 'overview' document describing the full scope in it's basics, but that should be it. This [great article][2] on the subject mentions the 'Basics' on how to do that.

### Testing and Reusability

I have to consider 'Testing and Reusability' together, as in my opinion you can not do 'true' Testing, whether it being Unit, Integration or UI Testing, without *reusing* your code.

Always being able to 'reuse' code or assets, without making any changes for separate environments is a number one requirement. For instance conducting Integration Testing in a staging environment would require to spin-up an environment specifically for testing purposes by deploying the exact code that would be used for the QA and Production environments. Do it any other way and the test should be discarded.

Now here comes the real conundrum we are seeing throughout the board with the SaaS and some of the aPaas products Microsoft provides.

The workloads which are built and marketed to support business processes are called **a)** PowerApps, PowerAutomate and even Logic Apps (for Low Code development) and **b)** SharePoint Online (for Document and Item storage).Especially the Low Code development tools seem not to have been built specifically to support testing or reusing adequately[^2].

![Power Automate - Flow - SharePoint Connector/Trigger](/assets/images/2020-08-11-22-27-10.png "Power Automate - Flow - SharePoint Connector/Trigger"){: style="border: 1px solid grey; padding:16px"}
*Figure 6: Power Automate - Flow - SharePoint Connector/Trigger*

What can be noticed after inspecting the Power Automate service and components (i.e. Flow actions and triggers):

- The Site Address (1) and List Name (2) input boxes of Flow Actions/Triggers (in this case the SharePoint ones) [do not support dynamic variables/parameters][3], so they are fixed to one particular location. Auto-resolving dynamic endpoints while in the designer should be a bare minimum to even consider using this tool.
- The API is limited to only facilitate automatic Export and Import of flows part of a Solution (using CDS)[^3]. This should be a basic offering for all type of flows.
- Export and Import of a Flow is supported but it will not let you dynamically alter the fixed location of configured Actions/Triggers.
- A Flow can only be connected to a specific endpoint defined by the Trigger action, this can not be converted to a dynamic endpoint without building a post-export script that will modify the JSON files in the export package. This step seems not very 'supported' in the long run because the flow export definition format can certainly change, creating a dependency on this format seems unwise.

The last three concerns are mentioned [in the comments of this blog post][4] and have not been addressed when writing this post.

Now I am pretty knowledgeable of other low code platforms like the [Nintex Platform][10] which had all of these capabilities since their SharePoint 2010 Workflow product over 10 years ago.

### Licensing

Nowadays 'Licensing' should certainly be part of solid Software Architecture. The QUINT2 model does not mention this quality property because licensing only became important after the cloud revolution around 2010.

To be able to invest in products/services, especially cloud-hosted products/services, it makes good sense that a consumer/business can rely on a stable pricing scheme. While consumers consider the financial loss when a service becomes unavailable or too expensive as a minor nuisance this can become quite serious when a business is involved.

Strategies, budgets and custom products are devised based on the historic reliability of product availability and license cost. When these factors change too much for a given product/service a business should not rely on them.

What we see in the Microsoft SaaS landscape is that we can certainly still build on the core services like SharePoint, OneDrive and Exchange as they are covered in even the most basic license SKUs. But the additional licenses are needed for Power Apps, Power Automate. On top of that the introduction of 'Per User/Per App plans', 'Premium Connectors', 'Portals', 'CDS' and various Add-in functionality like 'RPA' has made this a powerful but rather shady licensing landscape, difficult to traverse and full of unexpected surprises on the horizon. More on licensing provided by Microsoft can be found [here][8] and [here][9]. A comprehensive Power Platform licensing/pricing guide can be found [here][11]. I Expect this platform to remain very turbulent for the foreseeable future.

## The future

![Future](/assets/images/EventTentative.svg "Future")
*Figure 7: Future*
{: style="border: 1px solid grey; padding:16px; margin: 5px; float: right; width: 150px; font-size:10px; color:LightGrey; background-color:black"}

Resolving the concerns mentioned in this article is no small feat. It really requires a mindset of believing in one's product/service and it might just take some time.

Getting to terms with the 'Reliability and Efficiency' challenges, my hopes are on a reasonably 'latency free' internet so that we can return to basic programming patterns and make software simple again.
One word: **"5G"**, people! We will need it. Not for speed per se, but more so for reliability (i.e. latency reduction and connection stability) and efficiency (less abstract programming and simpler testing) purposes.

For solving the 'Portability' section my hopes for the future are that big tech will embrace more open/common data formats. A nice example of such efforts is docs.microsoft.com that is totally built on GitHub and uses simple markdown to store documentation and issues. Markdown in itself can be ported to anything you want as it is in essence 'plain text'. And... because it's just text, it is blazing fast!

A pretty good marketing quote that I would like to encounter might sound like: Yes, you can export all your (meta)data to to open XML and standard blobs if you want, but why should you? Our service is the very best! Our advise: Use it as a backup only!
{: .notice--success}

Manageability is surely the hardest thing to solve because so many conflicting interests are embedded in the discussion.
For me all the current impracticalities and basic limitations in the Maintainability of the Power Platform platform urges me to search for alternatives and to maximize the use of stable well established technologies and stay away from turbulent and (too) changeable technologies for building line of business solutions.

For example I would embrace Azure AD, SharePoint, Exchange and the Graph and tying it all together with some PowerShell lovin'. Time will tell if the Power Platform will end up with a truly stable and reliable platform core which will make long term investments a good idea.

**I hope you enjoyed this post and maybe you have some comments or feedback to give. Don't be shy!**
{: .notice--success}

<!-- Begin FootNotes -->
[^1]: Dig into the "[Videotape format war][1]" on Wikipedia.
[^2]: Or they have been built to support these cases but nobody has bothered to implement the features necessary to support reuse.
[^3]: When using [CDS with a Solution][6] there is an [API available][7].
<!-- End FootNotes -->

<!-- Begin Abbreviations -->
*[SaaS]: Software as a Service, in this context the Microsoft 365 and Azure AD ecosystems are depicted
*[tenant]: A representation of an organization. It's a dedicated instance of shared Microsoft SaaS products offering Office 365 or Azure AD services.
*[pipeline]: A pipeline in the context of Microsoft Azure Pipelines is a full-featured continuous integration (CI) and continuous delivery (CD) service. It works with your preferred Git provider and can deploy to most major cloud services, which include Azure services
*[GUI]: Graphical User Interface
*[B2B]: Business to Business
*[OS]: Operating System
*[AI]: Artificial Intelligence
*[ML]: Machine Learning
*[SKU]: Stock Keeping Unit, in the Microsoft ecosystem this term is used to reference 'product/service/workload licenses' which are sold.
*[CDS]: Common Data Store, which is a database storing entities and relations built for reuse
*[RPA]: Robotic Process Automation
<!-- End Abbreviations -->

<!-- Begin References -->
[1]: https://en.wikipedia.org/wiki/Videotape_format_war
[2]: https://medium.com/@andrewgoldis/how-to-document-source-code-responsibly-2b2f303aa525
[3]: https://powerusers.microsoft.com/t5/Power-Automate-Ideas/Update-file-properties-with-dynamic-url/idc-p/385315
[4]: https://flow.microsoft.com/en-us/blog/import-export-bap-packages/
[5]: https://managementmodellensite.nl/quint2
[6]: https://flow.microsoft.com/en-us/blog/solutions-in-microsoft-flow/
[7]: https://docs.microsoft.com/en-us/power-automate/web-api
[8]: https://docs.microsoft.com/en-us/power-platform/admin/pricing-billing-skus
[9]: https://docs.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq
[10]: https://www.nintex.com/
[11]: https://www.avantiico.com/guide-to-microsoft-power-apps-power-automate-and-power-virtual-agents-may-2020-licensing/
<!-- End References -->
