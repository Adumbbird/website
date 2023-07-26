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