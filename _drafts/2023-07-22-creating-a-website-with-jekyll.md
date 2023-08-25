---
layout: post
title:  "Configuring Emby"
summary: "This is how I configured emby"
author: adumbbird
category: software
thumbnail: /assets/img/posts/HumbleBundleDiscord.png
keywords: software, automation, server, home server, windows, windows server, windows 11, emby, ombi, sonarr, radarr, jackett, transmission, expressvpn
---

Here we will be creating a website to host using firebase authentication and jekyll.
## jekyll dependencies
{% highlight posh %}
    winget install rubyinstallerteam.rubywithdevkit3.2.1 
{% endhighlight %}

restart the terminal

{% highlight posh %}
    gem install jekyll bundler
{% endhighlight %}

if using git, create a .gitignore file and add the following lines to it:
{% highlight posh %}
### Jekyll ###
    _site
    .jekyll-metadata
    *-cache/
### NPM ###
    /node_modules/
## Compiled Source ##
    *.com
    *.class
    *.dll
    *.exe
    *.o
    *.so
### Compressed Packages ###
    *.7z
    *.dmg
    *.gz
    *.iso
    *.jar
    *.rar
    *.tar
    *.zip
### Logs and Databases ###
    *.log
    *.sql
    *.sqlite
### Linux ###
    *~
    .fuse_hidden*
    .Trash-*
    .nfs*
### MacOS ###
    *.DS_Store
    .AppleDouble
    .LSOverride
    Icon
    ._*
    .DocumentRevisions-V100
    .fseventsd
    .Spotlight-V100
    .TemporaryItems
    .Trashes
    .VolumeIcon.icns
    .com.apple.timemachine.donotpresent
    .AppleDB
    .AppleDesktop
    Network Trash Folder
    Temporary Items
    .apdisk
### Windows ###
    Thumbs.db
    ehthumbs.db
    ehthumbs_vista.db
    Desktop.ini
    $RECYCLE.BIN/
    *.cab
    *.msi
    *.msm
    *.msp
    *.lnk
{% endhighlight %}

I'm only going to go over instructions for windows. 

You're first going to want to install ruby with dev tools. I used winget to install it and it made the whole process super easy. 

{% highlight posh %}
    winget install RubyInstallerTeam.RubyWithDevKit.3.2
{% endhighlight %}

Once thats finished, restart the terminal and install jekyll and bundler using the following code:

{% highlight posh %}
    gem install jekyll bundler
{% endhighlight %}

Once thats finished, proceed to the directory where you would like to develop your website.

{% highlight posh %}
    cd 'Put the parent directory here'
    jekyll new 'name of your site'
    cd 'name of your site'
    bundle exec jekyll serve
{% endhighlight %}

This will build your site and host it by default at http://localhost:4000.

See (Jekyll)[https://jekyllrb.com/docs/] for more information. 


I highly recommend following (this tutorial)[https://jekyllrb.com/docs/step-by-step/01-setup/] as its pretty solid and goes through all the basics of using jekyll to build a website. 

There are themes that you can use and I created one to use for myself that you can use if you'd like. 

You can find the theme (here)[https://github.com/BirdsBored-LLC/birdsbored-jekyll-theme]. 