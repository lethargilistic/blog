# Uploading to the Internet Archive with Python
(2019-10-05, [dev.to](https://dev.to/lethargilistic/uploading-to-the-internet-archive-with-python-bg0))

I am a digital (and physical) pack rat. I've always felt a deep anxiety over losing things (which I frequently do), because I forget things about my own life when the evidence is no longer there. This is part of what makes me a strong supporter of the Internet Archive's mission: archiving humanity and especially the internet. And they have a Python library available to help with that, `internetarchive` on PyPI, which you can install with `pip`.

[The documentation is here](https://archive.org/services/docs/api/internetarchive/index.html), but it doesn't provide much of a linear breakdown, hence this article.

Aside, the `internetarchive` package comes with a command line tool called `ia` that already provides a high-level API for interacting with the Archive's data. Technically, it would behoove you to see if that already meets your needs, but I wanted to use the Python library.

# Logging in
First thing's first: you have to create the config file for `internetarchive`. This is not one of those tutorials where you can skip to the fun stuff. You must authenticate before you can work with the Archive's toys.

## Option 1: `ia configure`
If you never want to write your credentials into your code, that command line tool `ia` provides `ia configure` for this. Fill it out and you'll have your config file.

## Option 2: `internetarchive.configure`
The `configure` function within `internetarchive` will also create the config file for you. Running this program once will create your config file. You can delete this after you run it.

```python
from internetarchive import configure
configure('myemail!@example.com', 'password')
```

## Double-checking your config file
The config file will be found at `$HOME/.ia` or `$HOME/.config/ia.ini` unless you specify a custom path elsewhere. It will be created correctly, since it doesn't have any moving parts. It should look something like this:

```
[s3]
access = aCcEsSkEy
secret = SeCrEtKeY

[cookies]
logged-in-user = my_email!%40example.com; expires=Sun, 04-Oct-2020 02:08:30 GMT; Max-Age=31536000; path=/; domain=.archive.org
logged-in-sig = SiGnAtUrEalsdnfanfaFEKA:WEFASDfadsfvaodsnfasdFAsdvnieranv; expires=Sun, 04-Oct-2020 02:08:30 GMT; Max-Age=31536000; path=/; domain=.archive.org

[general]
screenname = lethargilistic
```

[Those are IA-S3 keys](https://archive.org/services/docs/api/ias3.html) rather than Amazon S3 proper, but that's a background detail. You can find the key associated with your account [here](https://archive.org/account/s3.php).

The `internetarchive` library auto-populated all of this config file information, including those keys, without me having to write them anywhere. I'm not sure if that key transfer is secured, to be honest, but there is also a way to add the keys to your code manually. You can write that part of the config file inline:

```python
from internetarchive import get_session
c = {'s3': {'access': 'aCcEsSkEy', 'secret': 'SeCrEtKeY'}}
session = get_session(config=c)
```

Creating a session is not necessary if your config file already exists. If you've written these lines, you can delete them.

# Uploading
Having created our config file, we're now ready to send things to the Archive.

## You get an item by its identifier
Every single thing within the Internet Archive's system has a unique "identifier." This is a string that contains ASCII letters, numbers, hyphens, underscores, or periods. To access any item within the archive, you request it with its identifier:

```python
from internetarchive import get_item
cool_podcast = get_item('amicus_lectio_0013')
```

That item identifier is already taken, and I control it, so you cannot upload to it, but I can. You can view the metadata within this [`Item`](https://archive.org/services/docs/api/internetarchive/internetarchive.html#internetarchive.Item) object with `cool_podcast.metadata` or download it with `cool_podcast.download()`. If you want to see a progress bar, `download()` has a `verbose` flag.

But how do you register an identifier for the item that you're going to upload? It happens automatically during the upload process, so you don't have to think about it as long as your identifier is unique already. If you use `get_item` on an identifier that is not registered yet, but you do not upload anything, then the identifier will not be registered.

So, at this point, you're ready to upload, but, before that, let's have a word about metadata.

## Uploading without context
Please do not do this! Metadata is gold! You should include everything you know about an item when you upload it to the Archive.

A great deal of the Internet Archive's utility as a repository of information comes from its rich metadata. There are people at the Archive who regularly comb through the millions of records and sort things out after the fact. You can make their jobs a lot easier and make your item far more discoverable by actually describing it while it still has your full attention.

You *can* upload items without metadata, but I will not show you how to do it.

## Uploading with metadata
The `upload()` function takes metadata as an argument. You can set it up as a dictionary.

The Internet Archive's service tries as much as possible to be metadata agnostic, which means you can use anything you want for keys and upload away. What's important to you is important to the Archive's records. That said, [the Archive does reserve a number of keys for display and filtering purposes](https://archive.org/services/docs/api/metadata-schema/index.html#metadata-schema). The following is a list of the most basic keys.

* `title`: The human-readable title. *Required.*
* `mediatype`: FILL THIS OUT; SEE NEXT SECTION. *Required.*
* `collection`: FILL THIS OUT; SEE NEXT SECTION. *Required.*
* `date`: The "YYYY-MM-DD' date the item was created or published, outside the context of the Archive item. A separate key, `addeddate`, will be auto-populated to indicate day you added it to the Archive. "YYYY" or "YYYY-MM" are also acceptable. You can write the value in brackets (e.g., "[YYYY]" or "YYYY-[MM]") to indicate you are uncertain.
* `description`: The human-readable description of the item. It supports HTML.
* `subject`: A list of strings that denote topics the item relates to. A podcast episode might include "podcast," the show title, and its topics.
* `creator`: A list of strings that denote entities that created the item. If you want to list more than one entity, each entity should be its own string in the list you send. If it's an ongoing or collaborative show, I usually also include the show title here with each participant as a separate string.
* `language`: The item's language. For example, "Spanish" or "Urdu." "English (handwriting)" is separate from "English."
* `licenseurl`: The canonical URL that points to the copyright license. If you use a common license like any of [the ones from Creative Commons](https://creativecommons.org/share-your-work/licensing-types-examples/), the item page will display the license's name and the proper symbols. Most of my work (including this article!) is licensed under "https://creativecommons.org/publicdomain/zero/1.0/".

The *Required* key-value pairs will technically auto-populate with general values (the `title` will match the identifier and the `mediatype` will be "data"), but it is **imperative** that you fill them out before you run the upload function. Some required fields are write-once and require admin privileges to change after the initial upload, see the next section for details.

Uploading metadata to the Archive is all-or-nothing. If any of the key-value pairs causes an error of some kind on the back end, then none of the metadata in that dictionary will be reflected after the upload. This happens if you accidentally include an admin-access-restricted key in your metadata, for example. Again, you can see the next section for more information about that.

So, without further ado, to upload something to the archive, you can write:

```python
from internetarchive import get_item
cool_podcast = get_item('amicus_lectio_0013')

md = {'title': '"Pokémon Go and the Law: Privacy, Intellectual Property, and Other Legal Concerns" by Tiffany C. Li (2016) - Amicus Lectio 0013'
      'mediatype': 'audio',
      'collection': 'opensource_audio',
      'date': '2019-09-17',
      'description': '<div><i>Pokémon GO</i> was an immediate sensation when Niantic released it in 2016, and it continues to be one of the highest-grossing apps on mobile devices. While the hype was still high, Tiffany C. Li wrote about potential legal rankles Niantic might face on the road to becoming a Poké Fan Master.<br /></div><div><br /></div><div><a href="https://osf.io/preprints/lawarxiv/gexpm/" rel="nofollow">The Paper.</a></div><div><br /></div><div>Mike Overby (<a href="https://twitter.com/lethargilistic" rel="nofollow">@lethargilistic</a>) reads <em>Amicus Lectio</em> (<a href="https://twitter.com/amicuslectio" rel="nofollow">@AmicusLectio</a></div>).',
      'subject': ['law', 'pokemon', 'pokemon go', 'amicus lectio',
                  'privacy', 'trespass', 'augmented reality', 'copyright',
                  'trademark', 'intellectual property'],
      'creator': 'Ruha Benjamin',
      'language': 'English',
      'licenseurl': 'http://creativecommons.org/publicdomain/zero/1.0/'}

cool_podcast.upload('path_to_your_file.mp3', metadata=md, verbose=True)
```

Some stray notes:
* The `verbose` flag optionally displays a progress bar for you.
* If you want to run this as a test before you actually upload, set the `dry_run` flag to `True`.
* If you want to upload multiple files with one call to this function, you can include a list of filepath strings, too. Python file objects also work.
* The function returns a `requests.Response` object, so you can check the HTTP status code.

Congrats, the item is live and you've archived something forever on the Internet Archive! You can now visit https://archive.org/amicus_lectio_0013 (rather, whatever identifier you used) to check if it is appearing correctly.

### *DO NOT FORGET THE WRITE-ONCE METADATA*
Some metadata keys are reserved for use by Internet Archive's staff only. Updating metadata is all-or-nothing. If you accidentally include any of these fields after the first upload, none of the metadata within the dictionary you send will be reflected on the item within the Archive.

I make special mention of this because you have **only one chance** to **set values for write-once fields** on the **first upload.**

To find out if a field is write-once, consult [the documentation's list of reserved metadata](https://archive.org/services/docs/api/metadata-schema/index.html). Write-once fields are indicated by "**edit access**: IA admin."

These are the most important write-once metadata, and I've included the most common values as examples:
* mediatype: The [mediatype](https://help.archive.org/hc/en-us/articles/360014321172-Archive-org-site-architecture-and-glossary) indicates the overall silo your Archive item will be within.
  * `data`, as in data files in formats like XML or CSV. *The default.*
  * `texts`
  * `audio`
  * `movies`, as in all video content.
  * `image`
  * `software`
  * `web`, as in websites.
* `collection`: [Collections](https://help.archive.org/hc/en-us/articles/360016399952-Collections-A-Basic-Guide-) pair your item with others like it and provides further filtering. Input the identifier of the collection you wish to be included in. These are the collections everyone has upload access to.
  * If you want ["Community Texts"](https://archive.org/details/opensource), use `opensource`. *The default.*
  * If you want ["Community Audio"](https://archive.org/details/opensource_audio), use `opensource_audio`.
  * If you want ["Community Video"](https://archive.org/details/opensource_movies), use `opensource_movies`.
  * If you want ["Community Data"](https://archive.org/details/opensource_media), use `opensource_media`.
  * If you want ["Community Images"](https://archive.org/details/opensource_image), use `opensource_image`.
  * If you want ["Community Software"](https://archive.org/details/open_source_software), use `open_source_software`.
  * If you want to upload as a "Test Item" which will be deleted after 30 days, use `test_collection`.

If you accidentally upload a file without these metadata set, or you set them incorrectly, you will need to send an email to [`info@archive.org`](mailto:info@archive.org) with your request to change them. This is not an imposition upon them, although there's no guarantee of when they'll get back to you. They provide this sample email for you to use:

```
 To: info@archive.org

 Subject: Please move my item(s)

 Body:

 Please move these items:

 archive.org/details/[item1identifier]

 archive.org/details/[item2identifier]

 To this collection:

 archive.org/details/[collectionidentifier]
```

#Updating metadata
Oh no! I just realized that I wrote the wrong name in the `creator` field.

[Ruha Benjamin](https://www.ruhabenjamin.com/) wrote [*Race After Technology: Abolitionist Tools for the New Jim Code*](https://twitter.com/lethargilistic/status/1139674477346746369)—available now [from Polity Books](http://politybooks.com/bookdetail/?isbn=9781509526390) or [wherever books are sold](https://www.booksamillion.com/p/Race-After-Technology/Ruha-Benjamin/9781509526406?id=7702471509917). She has nothing to do with my cool podcast about legal scholarship, [*Amicus Lectio*](https://twitter.com/AmicusLectio)—available now [on the Internet Archive](https://archive.org/search.php?query=subject%3A%22Amicus+Lectio%22&sort=-publicdate) because their hosting is free!

We need to correct this metadata! To do that with Python, we would write:

```python
from internetarchive import get_item
cool_podcast = get_item('amicus_lectio_0013')

#To be clear, this dictionary can still include any number of pairs.
md = {'creator': 'Mike Overby'}

cool_podcast.modify_metadata(md)
```

Phew. Now the item is live *and correct* at https://archive.org/amicus_lectio_0013.

I hope you enjoy throwing stuff onto the human race's digital pile! Now that you know how the metadata works, [it'll be easier to explore it](https://archive.org/search.php?query=%28Computer%29+AND+mediatype%3A%28movies%29+AND+date%3A%5B1950-01-01+TO+1959-12-31%5D&page=2), too!

# Sidebar: "The book I uploaded doesn't look right?"
This tutorial covers how to upload individual items to an identifier, but the way certain kinds of Archive items appear is responsive to different kinds of structured input. For example, the way [it organizes the images of book pages so they function more like a proper book](https://archive.org/details/in.ernet.dli.2015.49974/page/n25) requires you to format the images in a specific way with specific names and send it as a zip file. I've only done this [once before](https://archive.org/details/1916-Bock-Novelist/), but [here is an explanation of that process for books](http://adellefrank.com/blog/how-to-upload-book-to-internet-archive).

# Further Sidebar: Shortcut functions
The `internetarchive` package also includes top-level [`internetarchive.download()`](https://archive.org/services/docs/api/internetarchive/internetarchive.html#internetarchive.api.download) and [`internetarchive.upload()`](https://archive.org/services/docs/api/internetarchive/internetarchive.html#internetarchive.api.upload) functions. I used the `Item` approach for this tutorial because that made it easier to explain the Archive's system. Personally, I think the `Item` approach encourages better habits, too. For instance, you need to create the `Item` object to check if the identifier is already registered on the Archive. 
