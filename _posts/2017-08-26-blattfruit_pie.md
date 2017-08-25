---
layout: post
title:  "Blattfruit Pie"
author: "Me!"
date:   2017-08-25
categories: bash
project: true
image: ""
excerpt: "A little 'trojan' I wrote to troll my brother."
---
Several years ago my stupid brother (the target) liked to prank my family by changing the backgrounds of laptops, phones, and tablets to his stupid face. It was funny, but got annoying, and he took it too far.  He changed my mother's background to the ["Jeff Goldblum is Watching You Poop"](http://knowyourmeme.com/memes/jeff-goldblum-is-watching-you-poop) meme.  She wasn't pleased. I devised this project as retaliation on her behalf.

### Github: [dugger/blattfruit_pie](https://github.com/dugger/blattfruit_pie)

My brother introduced me to video games.  Playing Sierra's Quest games on his Commodore Amiga is one of the fondest memories I have of my childhood.  There is a piece of dialog in Space Quest III I've always found funny. When you order your food at Monolith Burger, The Pushy Counter Clerk asks "Would you like a Blattfruit Pie with that?", your options are Yes and Yes. I picked this idea as the center of my trolling for nostalgia and because I knew it would be instantly recognizable to my target.

![Would you like a Blattfruit Pie with that?]({{ site.url }}/assets/img/blattfruit-applescript.png)

The original version was a simple dialog box built using AppleScript.  It lacked the styling of the dialog box from the game, but it got the point across.
{% highlight AppleScript %}
tell application "Finder"
  activate
  display dialog "Would you like a blattfruit pie with that?" with title "Pushy Counter Clerk" buttons {"Yes", "Yes"}
end tell
{% endhighlight %}

The dialog was triggered using a cron task that I installed on the target's machine; a little bash script would curl a text file I stored in a public Dropbox folder, read the unix timestamp from the file, and trigger the dialog if the timestamp had passed.  I had to install it manually on his machine, and I had to have his password in order to allow the dialog app to be run (since the app isn't signed).  Despite all this, I installed it successfully, and for the next nine months my family reveled in his confused excitement at discovering some secret easter egg in OS X.

![Would you like a Blattfruit Pie with that?]({{ site.url }}/assets/img/blattfruit.png)

In the end he got a new computer and I never got to reinstall it there, so it died.  I confessed to it not long after, and we had a good laugh, but I've never been able to put this project to bed.  I've always wanted to learn more C / Objective C / Xcode, and this provided a fine base for doing that.  So far I've rebuilt the dialog in Xcode and styled it to look just like the game, and I've got a list of new features I want to add.  Someday I'll install the new and improved version on his computer...
