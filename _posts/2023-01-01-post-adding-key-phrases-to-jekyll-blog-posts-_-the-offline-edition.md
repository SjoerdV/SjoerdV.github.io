---
categories:
- blog
comments:
  id: 114563316554577872
date: 2023-01-01 10:00:00+02:00
header:
  og_image: /assets/images/post-adding-key-phrases-to-jekyll-blog-posts-_-the-offline-edition-001.png
last_modified_at: 2023-01-01 10:00:00+02:00
og_publish_date: 2023-01-01 10:00:00+02:00
tags:
- Openai
- OpenaiChatgptModel
- ProprietaryOnlineAiMonstrosities
- Chatgpt3
- AiWorld
- Chatgpt
- OfflineGoodness
- Twitter
- Metaverse
- TrueUtopia
- OfflineEdition
- ExcellentOpenSourceAlternatives
- OpenSourceSoftwareMovement
- InterestedMegalomaniacs
- NlpMagicSauce
- KeyPhrases
- StaticPages
- NaturalLanguageProcessing
- VisualStudioCode
title: Adding Key Phrases to Jekyll Blog Posts - The Offline Edition
toc: true
toc_icon: list-alt
toc_label: Contents
toc_sticky: true
---

Now OpenAI's ChatGPT3 is the hottest ticket in town, it's about time to call it what it is:

An white guy in an over-priced suit, impersonating every bubble imaginable, continuously spuing utter fascist crap into the world without bothering to hide its blatant biases and presenting everything as 'The Truth'. I would not want to drink a beer with it if it was a person. I would want it incarcerated indefinitely.

So let us just appreciate a lot of the 'offline goodness' and 'limited scope' that thankfully still exists in the AI world and add just one more New Year's resolution: please stop feeding proprietary online AI monstrosities, like the one described earlier, because they are owned by insane, self-interested megalomaniacs, do not let the name '**Open**AI' fool you...  
Strangely enough, those 'monstrosities' do have a lot in common with 'him' that bought Twitter not so long ago or 'the one' that thinks a metaverse represents a true utopia, IMHO.

![Key phrases - the offline edition ](/assets/images/post-adding-key-phrases-to-jekyll-blog-posts-_-the-offline-edition-001.png)
*Key phrases - the offline edition*
{: style="border: 1px solid grey; padding:16px; font-size:10px"}

## Get inspired by auto-tagging key phrases

While we can not look into the OpenAI ChatGPT model, because 'it is too large', 'we wouldn't understand what we are looking at' and most of all 'how would we make money otherwise?', there are excellent open source alternatives for Key Phrase extraction out there.

So let me show you something practical.

This post may be interesting for all of you who are blogging using a Static Pages solution and want to have some 'Key Phrase suggestions' called 'tags' inserted, on-demand, and offline, into the post you are actively editing.  
In my case, I am using a Jekyll static site generator hosted on GitHub Pages and I write my posts in Markdown syntax with a YAML frontmatter section in each 'post' file. This frontmatter contains metadata about the post like the 'title', 'date' and most importantly, the 'tags'. This metadata will be rendered appropriately when the pages are built and requested into a browser. The markdown source of a post will look something like this:

```markdown
---
title: My first blog post
date: 2023-01-01 10:00:00+02:00
header:
  og_image: /assets/images/post-my-first-blog-post-001.png
categories:
- blog
tags:
- FirstBlog
- Post
- ANiceThing
---
## This is my first blog post

Just writing along, is nice thing to do!
```

Wouldn't it be nice to get some inspiration for the tags in your post?

## VSCode: the front-end for all your workflows

First off, read all the documentation on embedding the python script `add_keyphrases_to_jekyll_blog_post` into your personal VSCode workflow on my [GitHub source repository][4]. You will need to download the source code and install some python packages using `pip`. Be warned, the models take up quite some disk space. Of course, you could also take the code and adjust it to fit your particular workflow.

I use VSCode as an authoring tool for these posts. The cool thing about VSCode is that you can make it exactly fit your specific workflow by using the built-in 'Tasks' and 'Keyboard Shortcut' functions.

The repository with the folder `add_keyphrases_to_jekyll_blog_post` has an `nlp.json` file that contains the configuration for the main NLP tools. Furthermore the `tasks.json` and `keybindings.json` files will allow you to execute the Task (ie. executing the python script) using a Keyboard Shortcut in VSCode. In this case I have configured the CTRL+SHIFT+F10 key combination.  
First copy and paste the text of the keybinding JSON bit into your own 'User' key bindings JSON file. Then you will need to have a post active in the VSCode editor when using the key combination.

## Taste the NLP magic sauce

The python script that is launched, `add_keyphrases_to_jekyll_blog_post.py`, employs state-of-the-art, offline, NLP python packages like [KeyBERT, using the default model][2] called 'all-MiniLM-L6-v2', which is being fed by a 'vectorizer' using a [Spacy language model for KeyphraseVectorizers][3]. This and the number of key phrases it should deliver are configured in the `nlp.json` file.  
That all sounds pretty technical, but it just means that in a few lines of python code, you can feed the text content of your blog to a NLP model and the output will be a user-defined number of key phrases representing your post.  
Included in the mix is the application of a specific HTML scraping library called 'Beautiful Soup', which is excellent for scraping and tidying up textual input.

Yes, you heard it here first: the execution of the input scraping and cleaning AND the key phrase extraction task AND the use of the needed language models can all be done **offline**, away from prying eyes and for free under an MIT/BSD licenses[^2][^3][^4].

NOTE: When a 'tags' key already exists in your frontmatter, the script will ask if you want to overwrite these tags. So be sure to check the terminal output in VSCode.

## References

The necessary reference articles to KeyBERT and KeyphraseVectorizers are provided as an [answer on StackOverflow][1] to the question "Gpt 3 keywords extractor".  
Furthermore I used this [great article on Medium][5] to gain insight on the NLP techniques I could use to fit my 'Blogging workflow'.
[Beautiful Soup][7] is the go-to library to scrape the plain text from your markdown/HTML content.This library is also used extensively in one of the greatest free Python Programming eLearning efforts called '[Python for Everybody][8], which I wholeheartedly recommend checking out. Learn Python before ChatGPT gets any better at it!

I want to stress that without these free/freemium sources I wouldn't have come up with this solution in the first place, so I want to thank the free portion of the internet that is under great threat from [proprietary technologies like ChatGPT][6] and wish everyone a great, free, peaceful and educational 2023.

<!-- Begin FootNotes -->

[^1]: Microsoft 365, formerly Office 365, is a line of subscription services offered by Microsoft which adds to and includes the Microsoft Office product line. The brand was launched on July 10, 2017, for a superset of Office 365 with Windows 10 Enterprise licenses and other cloud-based security and device management products.
[^2]: KeyBERT: https://github.com/MaartenGr/KeyBERT/blob/master/LICENSE
[^3]: KeyphraseVectorizers: https://github.com/TimSchopf/KeyphraseVectorizers/blob/master/LICENSE
[^4]: python-beautifulsoup: https://github.com/akalongman/python-beautifulsoup/blob/master/LICENSE

<!-- End FootNotes -->

<!-- Begin Abbreviations -->

*[FOSS]: Free and Open Source Software as defined by the Free Software movement and the Open Source Software movement.
*[VSCode]: Visual Studio Code
*[NLP]: Natural Language Processing

<!-- End Abbreviations -->

<!-- Begin References -->

[1]: https://stackoverflow.com/a/72713393
[2]: https://www.sbert.net/docs/pretrained_models.html#model-overview
[3]: https://github.com/TimSchopf/KeyphraseVectorizers#reuse-a-spacy-language-object
[4]: https://github.com/SjoerdV/PublicPythonProjects/blob/main/docs/README.md
[5]: https://towardsdatascience.com/enhancing-keybert-keyword-extraction-results-with-keyphrasevectorizers-3796fa93f4db
[6]: https://en.wikipedia.org/wiki/ChatGPT
[7]: https://www.crummy.com/software/BeautifulSoup/
[8]: https://www.py4e.com/

<!-- End References -->
