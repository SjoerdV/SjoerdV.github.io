---
categories:
- blog
date: 2023-09-03 10:00:00+02:00
header:
  og_image: /assets/images/post-giscus-for-comments-001.png
last_modified_at: 2023-09-03 10:00:00+02:00
og_publish_date: 2023-09-03 10:00:00+02:00
tags:
- GitHubDiscussion
- GitHubAccount
- TechnicalBlog
- Blog
- GitHub
- CommentsSystem
- AppPermissions
- GiscusApp
- SeparateThirdPartyApp
- Discussions
- DeveloperPlatform
- Discussion
- Comments
- MobileApp
- Visitors
- Giscus
- PlatformCapabilities
- Api
title: Moving the comments system for this blog to GitHub Discussions through giscus
toc: true
toc_icon: list-alt
toc_label: Contents
toc_sticky: true
---

Since this blog is hosted on GitHub it would be only fitting to use the platform capabilities a bit more.

I have only considered this since:

* Discussions can now be [exported quite easily][2], which would be necessary if I needed to move my Blog elsewhere at some point
* Discussions can also be held [**directly** on GitHub][3], so you will not be required to trust a separate third party App (see below)

So, as of today (September 3rd 2023) comments to posts are integrated with the GitHub 'Discussions' feature.

## How will this work?

To be able to react to a blog post you are going to need a GitHub account. Since this is a technical blog, I will assume that almost all visitors will have such an account.

Secondly, and this is optional, if you want to comment directly on this website you will need to allow the app [giscus][1] permissions to post on your behalf.

## Extra

I have pushed a discussion 'placeholder' for each post (and upcoming posts) so you have two options to comment.

![Moving the comments system for this blog to GitHub Discussions through giscus](/assets/images/post-giscus-for-comments-001.png)
*Moving the comments system for this blog to GitHub Discussions through giscus*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

1. Reply directly from the website (by clicking the 'Sign in with GitHub' button and approve the permissions for the mentioned giscus App) OR
1. Click the 'Comments' link on each Post page to navigate directly to the GitHub Discussion in your browser or Mobile App and post a reaction to the existing Discussion placeholder

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

[1]: https://github.com/giscus/giscus#readme
[2]: https://github.com/orgs/community/discussions/3315
[3]: https://github.com/SjoerdV/SjoerdV.github.io-comments/discussions

<!-- End References -->
