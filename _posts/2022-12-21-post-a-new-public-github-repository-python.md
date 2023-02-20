---
categories:
- blog
date: 2022-12-21 10:00:00+02:00
header:
  og_image: /assets/images/post-a-new-public-repo-python-001.png
last_modified_at: 2022-12-21 10:00:00+02:00
og_publish_date: 2022-12-21 10:00:00+02:00
tags:
- PowershellExtensions
- Powershell
- PowershellScript
- WindowsSubsystem
- PowerfulScriptingLanguage
- MicrosoftEcosystem
- ScriptingLanguage
- PublicPythonProjects
- Scripts
- Python
- PythonProgrammingLanguage
- Toolkit
- Microsoft
- Linux
- MajorPlatforms
title: A New Public GitHub Repository - Python
toc: true
toc_icon: list-alt
toc_label: Contents
toc_sticky: true
---

First off, I am **still** a great fan of PowerShell. It is a very versatile and powerful scripting language that, ever since the '.NET Core revolution', works cross-platform as well. Awesome! Especially the tight integration to the Microsoft Cloud eco-system and the Windows OS is a 'no brainer' for me, professionally speaking.

Except... it works cross-platform for only those classes, modules and libraries that are available in .NET Core, which might not be enough.

For some challenges, like creating a 'System Tray (systray) / App Indicator' app that works on Windows, Linux and even MacOS, PowerShell is not really an option.

## In comes Python3

![In comes Python3!](/assets/images/post-a-new-public-repo-python-001.png)
*In comes Python!*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

Because I see the importance of having knowledge of both PowerShell and Python I have setup a repository dedicated to sharing some of the scripts I developed.

Visit my GitHub 'PublicPythonProjects' repository: [https://github.com/SjoerdV/PublicPythonProjects](https://github.com/SjoerdV/PublicPythonProjects)
{: .notice--success}

While PowerShell 7+ and Python3 have a lot of things in common (like their basic syntax), they are vastly different under the hood. Foremost Python is a true **programming language** and has the advantage of being older and therefor more mature, with far more libraries and integrations available. PowerShell is still a **scripting language** in its core, although the use of .NET classes makes it more capable than simple scripting.

Both language do not really support a 'strongly typed' programming style though. They are both languages meant for speed. If you want more control try Go or Rust.

In general I have to say that most of the time Python program **performance** feels superior than its PowerShell script equivalent.

Both languages can be used efficiently in a single IDE though. [Visual Studio Code][1] is excellent for this. It has sufficient [extensions][2] available to maximize your productivity for both languages. Besides the default provided Python and PowerShell extensions you could take a look at the [Jupyter extension][3] that really utilizes Python to the best of its abilities.

## Example: PowerShell wins!

Reference: [https://github.com/SjoerdV/PublicPythonProjects/tree/main/pwsh_vs_python](https://github.com/SjoerdV/PublicPythonProjects/tree/main/pwsh_vs_python)

In VSCode, I love the way PowerShell handles JSON/XML objects, compared to Python. PowerShell allows you to navigate the objects through intellisense auto complete (using the CTRL+SPACE combination) far better than the clunky way you have to write out the object references in Python.

![Intellisense auto complete when interacting with JSON objects in PowerShell](/assets/images/screen20221222012427.png)
*Intellisense auto complete when interacting with JSON objects in PowerShell*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

For example, imagine loading the JSON object below in both languages using the file `my_data.json`.

```json
{
  "Programs": [
    {
      "Name": "MyProg1"
    },
    {
      "Name": "MyProg2"
    }
  ]
}
```

In PowerShell

```powershell
# Returns JSON as object
$json = Get-Content -Raw -LiteralPath ".\my_data.json" -Encoding UTF8 | ConvertFrom-Json
# Display value
$json.Programs[0].Name  # With intellisense and autocomplete
```

In Python

```python
import json
# Opening JSON file
jsondatafile = open('my_data.json',encoding="utf8")
# Returns JSON object as a dictionary
jsonobject = json.load(jsondatafile)
# Display value
print(jsonobject['Programs'][0]['Name'])  # Clunky... Have to type in the entire sequence.
```

## Example: Python wins!

Reference: [https://github.com/SjoerdV/PublicPythonProjects/tree/main/appindicator](https://github.com/SjoerdV/PublicPythonProjects/tree/main/appindicator)

Like I mentioned earlier, I had the need to detect if a specific process had started on both Windows and Linux (of course I had to compensate for the fact that on Windows processes are usually '.exe' files and in Linux they could have any name). This should be accompanied by a visual cue. A system tray app with a link to the Desktop Notification service and a tray icon that would switch color would be a good fit.

The linked System Tray program above that does all this could never have been developed with PowerShell in a cross-platform manner. This is because PowerShell would be forced to use the `System.Windows.Forms` library (solely available on Windows as the name implies) to manipulate the GUI.

With Python the only libraries needed where `pystray` and `psutil`. These work on all major platforms!

## Fatality!

The foundational strengths of the Python programming language are truly worth having in your toolkit and if you have a professional focus aimed at the Microsoft ecosystem, PowerShell is inescapable and also a lot of fun to work with.

Also take into account that both Microsoft and the FOSS community are slowly growing towards each other. Not necessarily because they want to, but foremost because they have to. Google and Amazon are gulping up any 'nice' FOSS initiatives that come along and discarding them at the same rate to further their own interests. Microsoft know they will be left behind if they don't do the same. The fact that Microsoft and Canonical are holding each other as tight as they are and the fact Microsoft has created the WSL promoting Ubuntu before any other is proof of this knowledge.

It is only logical that if you were born into the Microsoft ecosystem, you now also start adopting FOSS technology like Python. And if you are a *nix kind of person, maybe start showing an interest in PowerShell.

The key takeaways here are: Doing more with more! AND Keep learning!

<!-- Begin FootNotes -->

[^1]: Microsoft 365, formerly Office 365, is a line of subscription services offered by Microsoft which adds to and includes the Microsoft Office product line. The brand was launched on July 10, 2017, for a superset of Office 365 with Windows 10 Enterprise licenses and other cloud-based security and device management products.

<!-- End FootNotes -->

<!-- Begin Abbreviations -->

*[FOSS]: Free and Open Source Software as defined by the free software movement and the open-source software movement.
*[WSL]: Windows Subsystem for Linux (WSL) is a feature of Windows that allows developers to run a Linux environment without the need for a separate virtual machine or dual booting.

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://code.visualstudio.com/
[2]: https://code.visualstudio.com/docs/editor/extension-marketplace
[3]: https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter

<!-- End References -->
