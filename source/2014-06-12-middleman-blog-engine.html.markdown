---
title: Middleman Blog Engine
---

When I decided to start this project I knew I needed a way to easily write articles and view them locally. Since static websites are the latest craze, and can be hosted almost anywhere, I did a bit of searching and found the [blog](https://github.com/middleman/middleman-blog) extension for [Middleman](http://middlemanapp.com/). I also found a guide on running [Middleman on Heroku](http://randomerrata.com/post/56163474367/middleman-on-heroku). I decided that since I'm already managing my blog with Git, I might as well post the whole thing [on Github](https://github.com/wjlafrance/blog) so you can see my configuration.

READMORE

If you're interested in playing around with this blog engine, go ahead and download my blog. This assumes you have [rvm](http://rvm.io/) (Ruby Version Manager) installed.

    $ git clone git@github.com:wjlafrance/blog.git
    $ cd blog
    $ rvm install 2.1.1
    $ rvm use 2.1.1
    $ gem install bundler && bundle install

Creating a new article is easy. There's no fancy script. I just `touch` a file, open the directory in [my favorite editor](http://www.sublimetext.com/), and run Middleman's embedded server.

    [lafrance@alaska ~/dev/blog]$ touch source/2014-06-12-middleman-blog-engine.html.markdown
    [lafrance@alaska ~/dev/blog]$ subl .
    [lafrance@alaska ~/dev/blog]$ middleman
    == The Middleman is loading
    == The Middleman is standing watch at http://0.0.0.0:4567
    == Inspect your site configuration at http://0.0.0.0:4567/__middleman/

Open your browser to the [site Middleman is hosting](http://127.0.0.1:4567) and you should be looking at your new blog.

I'm still working on perfecting the layout and style, as I don't want this blog to betray the fact that it was [created by a developer with no designer help](http://en.wikipedia.org/wiki/Programmer_art). I welcome any pull requests on Github with improvements, whether technical, visual, or even grammatical. 

Thanks for reading, and I'll see you next week!

(PS: If you're curious why my computer is no longer named `defiant`, there's a few reasons. My SSD wore out, so I had to break my [Fusion Drive](/2012/11/02/setting-up-fusion-disk-on-an-older-mac/), and decided to do a clean install of OS X. I've also been short on starships lately, so I've decided that I'll now name my devices after places, which I swear I didn't copy from Apple. I'm about 90% of the way though the excellent book [Looking For Alaska](http://en.wikipedia.org/wiki/Looking_for_Alaska), so I named my computer `alaska`. That way, if I ever misplace it I'll literally be looking for Alaska.)

