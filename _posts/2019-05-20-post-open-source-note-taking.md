---
title: "Embarking on the 'Open Source Note Taking' journey"
date: 2019-05-20T16:00:00+01:00
last_modified_at: 2019-11-22T16:00:00+01:00
header:
  image: /assets/images/ConvertOneNote2MarkDown.png
  teaser: /assets/images/ConvertOneNote2MarkDown-th.png
  og_image: /assets/images/ConvertOneNote2MarkDown-th.png
categories:
  - blog
tags:
  - GitHub
  - OneNote
  - Markdown
  - Visual Studio Code
  - Conversion
  - Powershell
  - Open Source
---
<!-- Begin Abbreviations -->
*[GitHub]: GitHub is Git repository hosting service. Used more and more by non-technical people to share their business solutions
*[OneNote]: You can use OneNote to record ideas and information on the go. OneNote is designed for use on any device, and it's available on all major operating systems including iOS and Android
*[Markdown]: Markdown is a lightweight markup language with plain-text-formatting syntax, created in 2004 by John Gruber with Aaron Swartz. Markdown is often used to format readme files, for writing messages in online discussion forums, and to create rich text using a plain text editor.
*[VSC]: Visual Studio Code, is a code editor redefined and optimized for building and debugging modern web and cloud applications.
*[ISE]: The Windows PowerShell Integrated Scripting Environment is a host application for Windows PowerShell
<!-- End Abbreviations -->
Every once in a while you start rethinking all the digital tools you use to organize yourself, maybe even to take some of those methods to business life and share with colleagues. This is a story about such a shift in tooling.

One of the things I often do (a lot of people do) is 'taking notes' and as a Microsoft-aficionado I am fondly using OneNote[^2] up until recently.

A month or so ago however Microsoft communicated the decision not to place an upgraded version of OneNote (2016 -> 2019) in their Office Pro Plus product suite, effectively [halting OneNote development][1] as it would seem.

This triggered some 'rethinking' of my 'note taking strategy' and quickly discovered the wonders of the Markdown[^3] syntax and the wonderful extensions that exist for [VSC][5][^4] to make really attractive notes. By the way, that trigger was the adoption of VSC for my Powershell editing in favor of the Powershell ISE[^5] bundled with Windows, so combining those workflows made even more sense.

As a side note, the [Microsoft Docs][2] pages are created in MarkDown hosted on GitHub and [GitHub itself was acquired by Microsoft][6] in June 2018. Furthermore the platform is gaining popularity with a large group of bloggers, who are also non-techy. So the thought of adopting Markdown for note taking is not something that would take me to "unsailed seas".
{: .notice--info}

The main benefit of Markdown is that you have access to a plain text copy of your notes anywhere. This means you can share them with anyone who has a plain text editor (which is everyone with a device and an OS). It is also possible to convert the Markdown formatted files to various open ISO formats (HTML, PDF) which can *also* be shared easily and look as beautiful as you can make them.

Of course there was that arduous 'migration of existing notes' to go through, doing this manually would not have been achievable as I accumulated OneNote artifacts since OneNote 2003. So I started searching online for ways to do this, but never found any tool that suited my need. Getting out of the OneNote ecosystem was a pain as it seems. Then I decided to build a migration tool myself using Powershell (which I am quite adept in) and using the OneNote 2013 InterOp API I managed to fix something up that did the job, preserving the OneNote structure (Notebooks, Sections, Pages (with indentations)).

If you are interested in converting your OneNote notebooks to Markdown, please have a look at my [Portfolio page][4] and subsequent Github repository hosting a Powershell script that does just that.
{: .notice--success}

## \*\* Updated November 11th 2019 \*\*

Microsoft did [announce plans to revitalize OneNote][3] so a roadmap is back on the table. Am am not seeing any changes in format support though, so having your notes as plain text files still seems reasonable.
{: .notice--warning}

## Bibliography

[^2]: OneNote definition source: Microsoft
[^3]: Markdown definition source: Wikipedia
[^4]: VSC definition source: Microsoft
[^5]: ISE definition source: Microsoft

<!-- Begin References -->
[1]: https://www.theverge.com/2018/4/18/17252312/microsoft-office-2019-onenote-windows-10-app
[2]: https://docs.microsoft.com
[3]: https://www.theverge.com/2019/11/7/20953691/microsoft-onenote-to-do-integration-fluid-framework-future-features-ignite-2019
[4]: /portfolio/ConvertOneNote2MarkDown/
[5]: https://code.visualstudio.com/download
[6]: https://news.microsoft.com/2018/06/04/microsoft-to-acquire-github-for-7-5-billion/
<!-- End References -->
