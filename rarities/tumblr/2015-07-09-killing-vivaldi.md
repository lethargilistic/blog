# Killing Vivaldi
*I still really like Vivaldi in theory, but I haven't used it in a long time. I'm not actually much of a power user of anything because it generally means hiding my decisions in settings and needing to set everything back up again after I change computers is a pain in the ass. I remember being very legitimately happy about solving this problem with programming. Sometimes it's nice to work around tools that are still mostly proofs of concept and barely work, just to remind yourself that you're alive and that things don't get perfect overnight.* (2020-07-01)

-----

(2015-07-09, [lethargilistic.tumblr](https://lethargilistic.tumblr.com/post/123623474136/killing-vivaldi))

I really like the [Vivaldi browser](https://vivaldi.com/). I’ve become sick of Chrome becoming a bigger, badder beast and it’s exciting to see a browser come on the market that wants to support what Chrome does (because at some point web devs at large seemed to decide that supporting a single browser’s features was a good plan for the future of the web for no reason), but also work on making things smaller and adding new features. It’s still really big right now, but at least they’re working on it.

Right now, the browser is in Technical Preview 3 and is missing a lot of promised features, such as an email client, and it’s CPU intensive to an excessive degree. I can deal with that, since I rarely let out my true inner power user.

One thing I can’t deal with is that it has a bug that crashes the program’s view silently, but leaves all of the processes going in the background. Your computer will be working on Vivaldi’s model like nothing happened (including continue to play sounds from videos), but you will be unable to open up a new instance of the Vivaldi browser and your only recourse will be to kill each of the processes individually. Until today, I was doing this manually.

Today, in my Networking class we were talking about nslookup and DNS services. I’d never heard of that command, and it made me start thinking about what other commands there might be. Is there one that will print out all the processes running? Yeah. In Unix, I was already aware of ps. In Windows, the command is tasklist. Well, that’s cool. Oh, and it shows pids, too.

**Oh, and it shows pids, too.**

You can kill processes if you have their pids. What if I could just kill them all with a program?

I knew that Python has built-in libraries for interacting with a system’s command line, specifically one is called [os](https://docs.python.org/3/library/os.html), and its system() function will open a terminal and do whatever you want. So I tried that, aiming to get the text returned by the command and found that you can’t actually to that. system() only returns the error code of the command. I did a little digging and found another library called [subprocess](https://docs.python.org/3/library/subprocess.html) that is meant to replace system() and provide more features, including the ability to access a command’s stdout (e.g. exactly what I wanted).

```python
p = Popen('tasklist"', stdout=PIPE).stdout
```

So that returns a readout of every task on the computer as a number of character bytes, which you can then convert to a string trivially. Can we make the command itself better? What if we could limit it to just Vivaldi processes (all called vivaldi.exe, ala chrome)? We can with some arguments.

```python
p = Popen('tasklist /svc /fi "imagename eq vivaldi.exe"', stdout=PIPE).stdout
```

/svc basically shortens your results to just the process name, the pid, and the services that use the process. This means each line has just one number in it. /fi means “filter” and our filter string is asking for all processes with image names (process names) equalling “vivaldi.exe.”

So now we have a list of all the vivaldi processes, one on each line, with one number associated with each. Let’s just scrape all the pid numbers out of there with one regular expression and findall, which returns all matches as one list of strings.

```python
vivaldiPids = findall('\d+', tasklist)
```

Then you can just step through all of the pid strings, convert them to integers, and tell your OS to kill them.

```python
for pid in vivaldiPids:
    os.kill(int(pid), 1)
```

The rest of the program can be found [on my github](https://github.com/lethargilistic/MiscellaneousPython/blob/master/killVivaldi.py). Hope they fix this soon, but having a two-click workaround is fine in the meantime.

PS, I have found having a keyboard shortcut to my command line infinitely useful since creating it. To do this:

    Open a terminal (use Run, type “cmd”)
    Right-click theterminal’s taskbar icon
    Right-click “command prompt”
    Click Properties
    Click the field by Shortcut key
    Press whatever key combination you want. I use Ctrl + Alt + T for “Terminal.”

Maybe you won’t use it today. Maybe you won’t use it tomorrow, but there will come a day when you want to do something very specific that will be made very easy via a single command (ipconfig, netstat, nslookup, tasklist), and that is the day you will press that button combination rather than look it up online and thank me. (Microsoft, plz make a native shortcut for this.)

*#Vivaldi #Python #Github #command line #Windows #Regular Expressions #articles #Vivaldi Browser*
