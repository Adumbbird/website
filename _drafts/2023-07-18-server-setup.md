---
layout: post
title:  "Setting up my server"
summary: "I document all the steps I take when setting up my services on my home server."
author: adumbbird
category: software
thumbnail: /assets/img/posts/HumbleBundleDiscord.png
keywords: software, automation, server, home server, windows, windows server, windows 11, emby, ombi, sonarr, radarr, jackett, transmission, expressvpn
---

It's been a while! Time has come for me to figure out what i'm going to do with my server as my evaluation copy of Win server 2022 was going to expire. Turns out, it's really expensive to lisence a server with 28 physical cores. So I did the only logical thing and switched to Win 11. I'm not sure how I feel about it yet, but it's working for now.

I decided to document the process of getting everything set up again so maybe it will help you. I'm going to break this up into a few posts, so this one will be about getting the server set up and all that entails, and then create more posts for different services i'm setting up. 

I'll be going through setting up the following services:
- Emby
- Ombi
- Sonarr
- Radarr
- Jackett
- ExpressVPN
- Transmission
- Tdarr
- Plex
- AMP by CubeCoders
- Dynamic DNS Client for namecheap
- bar-assistant
- salt-rim-vue
- github-cli
- nginx
- jekyll


Now as a disclaimer, i'm not advocating any sort of piracy. I'm just documenting the process of setting up these services. I'm not responsible for what you do with them. It is legal to download content that you own, so if you have a dvd of a movie, it's legal to download a copy of that movie. The above services just make that process a bit easier. 

Another process I am still working out, is using Firebase by Google as a way to authenticate myself for these services. I'm hoping to get that working so I only have to sign in once to access all of these services.

# Setting up the server

So it actually turns out that Win 11 pro is a lot easier to set up than Win server 2022. Luckily, windows was smart enough to see my ReFS arrays and just mount them. I didn't have to do anything to get them to show up. All I needed to do was fix the drive letters. 

The server is laid out with two separate arrays. One holds my dev files, service files, and other things that I don't want to lose. The other houses my digital media and periodic backups of the service configurations The OS is also set up with a raid 1 array.

- I:\ is the 'important' array.   
- D:\ is the data array.

I also created a local user called adumbserviceaccount and added it to the administrators group.

## Setting up the services for automated media

I'm going to be using NSSM.exe to set up the services. NSSM stands for Non-Sucking Service Manager. It's a great tool that allows you to set up services to run on startup. You can find it at https://nssm.cc/. I copied the nssm.exe file to my C:\windows folder so that I can use it from anywhere.

To set up a service to run on startup, I used NSSM.exe. I'll include examples of how I set up each service.



### Emby

To install emby, I went to https://emby.media/download.html and snagged the file for my OS. You can also grab the files from https://github.com/MediaBrowser/Emby.Releases/releases. Under windows, there is an option for a portable installation which is what I used. I just extracted the zip file to a folder on my server and ran the Emby-Server.exe. Since I have this portable folder on my I:\ drive, it saves the configuration files there as well. Within Emby, I added a plugin to also back up data, as well as pointed the metadata to a folder on my D:\ drive. Double redundancy :)

{% highlight posh %}
nssm.exe install emby "I:\Emby-Server\system\EmbyServer.exe -service"
nssm.exe set emby AppDirectory "I:\Emby-Server\system"
nssm.exe set emby AppParameters "-service"
nssm.exe set emby AppExit Default Exit
nssm.exe set emby Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}

Note: You can also just use "nssm.exe install emby" and it will open a GUI to help you set up the service. You can also use "nssm.exe edit emby" to edit the service.

Also go into your start-up programs and disable the emby server from starting up. From here, start the service and you should be good to start configuring emby via the web interface. By default it is http://localhost:8096.

For configuration, see [Configuring-Emby].

### Jackett

https://github.com/Jackett/Jackett

The installer wants to use C:\ProgramData, which is fine if thats what you want to do. Instead I decided to grab the latest binaries from https://github.com/Jackett/Jackett/releases and extract them to a folder on my I:\ drive. 

From here, I set up the service with the following commands:

{% highlight posh %}
nssm.exe install jackett "I:\Jackett\Jackett\JackettConsole.exe"
nssm.exe set jackett AppDirectory "I:\Jackett\Jackett"
nssm.exe set jackett AppParameters "-d I:\Jackett\ -x"
nssm.exe set jackett AppExit Default Exit
nssm.exe set jackett Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}

Start the service and you should be good to start configuring emby via the web interface. By default it is http://localhost:8096.

For configuration, see [Configuring-Jackett].

### Radarr

We'll do the same with Radarr as the installer doesn't let us change the install path. Grabbing the zip from https://github.com/Radarr/Radarr/releases and extracting it to a folder on my I:\ drive. I nesteled it into a sub folder called Radarr so that I can house the config files in the same place. Radarr needs a bit more configuration than the others since we're doing this manually.

We'll need to also download sqlite. Winget comes in handy here as we can just run the following command to install it.

{% highlight posh %}
winget install sqlite.sqlite
{% endhighlight %}

We can then set up the service with the following commands:

{% highlight posh %}
nssm.exe install radarr "I:\Radarr\Radarr\Radarr.Console.exe"
nssm.exe set radarr AppDirectory "I:\Radarr\Radarr"
nssm.exe set radarr AppParameters "/data=I:\Radarr\"
nssm.exe set radarr AppExit Default Exit
nssm.exe set radarr Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}

It listens to port 7878 by default, so you can access it at http://localhost:7878.

For configuration, see [Configuring-Radarr].

### Sonarr

To get to the latest version of Sonarr, we'll need to grab the zip from the main website. You'll see a section at the bottom of the windows instructions called "Alternatives for advanced users." This is where you can get the zip. https://sonarr.tv/#downloads-v3

Once we have that, we extract it to a folder on our I:\ drive. I also nested it into a sub folder called Sonarr so that I can house the config files in the same place. Sonarr needs a bit more configuration than the others since we're doing this manually.

If you haven't already installed sqlite, you can do that with the following command:

{% highlight posh %}
winget install sqlite.sqlite
{% endhighlight %}

Then you should be able to set up the service:

{% highlight posh %}
nssm.exe install sonarr "I:\Sonarr\Sonarr\Sonarr.Console.exe"
nssm.exe set sonarr AppDirectory "I:\Sonarr\Sonarr"
nssm.exe set sonarr AppParameters "/data=I:\Sonarr\"
nssm.exe set sonarr AppExit Default Exit
nssm.exe set sonarr Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}

Sonarr uses port 8989 by default, so you can access it at http://localhost:8989.

And check out [Configuring-Sonarr] for configuration.

### Ombi

https://github.com/Ombi-app/Ombi/releases

{% highlight posh %}
nssm.exe install ombi "I:\Ombi\Ombi\Ombi.exe"
nssm.exe set ombi AppDirectory "I:\Ombi\Ombi"
nssm.exe set ombi AppParameters "--storage I:\Ombi\ --baseurl /ombi"
nssm.exe set ombi AppExit Default Exit
nssm.exe set ombi Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}

The first time you go to http://localhost:5000/, you'll be at a "welcome to ombi" page. You'll need to set up an admin account here. Once you do that, you'll be able to log in and start configuring ombi.

I go into more detail about configuring ombi in [Configuring-Ombi].

### Transmission

subfolder

-i 
 change the path to our subfolder 
Make sure to grab all the features. We want the qt client, the daemon, the cli tools, and the web client. 


Open powershell/terminal as administrator and run the following commands:
{% highlight posh %}

cd I:\Transmission\Transmission
.\transmission-qt.exe --g I:\Transmission
{% endhighlight %}

A window will open stating that transmission is a file sharing program. Click "I Agree" if you do so and then close the window.

Make sure that transmission fully closed before continuing. You can check this by opening task manager and looking for transmission-qt.exe. If it's still running, end the task.

By doing this, transmission created a new file called settings.json in the I:\Transmission folder. We'll need to edit this file to make sure that transmission is set up the way we want it.

You can check out my configuration at [Configuring-Transmission].
{% highlight json %}
{
    "alt-speed-down": 50,
    "alt-speed-enabled": false,
    "alt-speed-time-begin": 540,
    "alt-speed-time-day": 127,
    "alt-speed-time-enabled": false,
    "alt-speed-time-end": 1020,
    "alt-speed-up": 50,
    "announce-ip": "",
    "announce-ip-enabled": false,
    "anti-brute-force-enabled": false,
    "anti-brute-force-threshold": 100,
    "bind-address-ipv4": "0.0.0.0",
    "bind-address-ipv6": "::",
    "blocklist-date": 0,
    "blocklist-enabled": false,
    "blocklist-updates-enabled": true,
    "blocklist-url": "http://www.example.com/blocklist",
    "cache-size-mb": 4,
    "compact-view": true,
    "default-trackers": "",
    "dht-enabled": true,
    "download-dir": "D:\\Downloads\\complete",
    "download-queue-enabled": true,
    "download-queue-size": 10,
    "encryption": 1,
    "filter-mode": "show-all",
    "filter-trackers": "",
    "idle-seeding-limit": 30,
    "idle-seeding-limit-enabled": true,
    "incomplete-dir": "D:\\Downloads\\incomplete",
    "incomplete-dir-enabled": true,
    "inhibit-desktop-hibernation": false,
    "lpd-enabled": true,
    "main-window-height": 689,
    "main-window-layout-order": "menu,toolbar,filter,list,statusbar",
    "main-window-width": 762,
    "main-window-x": 1544,
    "main-window-y": 114,
    "message-level": 4,
    "open-dialog-dir": "D:/downloads/",
    "peer-congestion-algorithm": "",
    "peer-limit-global": 500,
    "peer-limit-per-torrent": 50,
    "peer-port": 51413,
    "peer-port-random-high": 65535,
    "peer-port-random-low": 49152,
    "peer-port-random-on-start": false,
    "peer-socket-tos": "le",
    "pex-enabled": true,
    "port-forwarding-enabled": true,
    "preallocation": 1,
    "prefetch-enabled": true,
    "prompt-before-exit": false,
    "queue-stalled-enabled": true,
    "queue-stalled-minutes": 15,
    "ratio-limit": 2,
    "ratio-limit-enabled": true,
    "read-clipboard": false,
    "remote-session-enabled": false,
    "remote-session-host": "localhost",
    "remote-session-https": false,
    "remote-session-password": "",
    "remote-session-port": 9091,
    "remote-session-requres-authentication": false,
    "remote-session-username": "",
    "rename-partial-files": false,
    "rpc-authentication-required": true,
    "rpc-bind-address": "192.168.50.174",
    "rpc-enabled": true,
    "rpc-host-whitelist": "192.168.*.*",
    "rpc-host-whitelist-enabled": true,
    "rpc-password": "{ffb77a3909436463695c5c85a64804e566768abe5QHptKG2",
    "rpc-port": 9091,
    "rpc-socket-mode": "0750",
    "rpc-url": "/transmission/",
    "rpc-username": "bird",
    "rpc-whitelist": "127.0.0.1,::1,192.168.*.*",
    "rpc-whitelist-enabled": true,
    "scrape-paused-torrents-enabled": true,
    "script-torrent-added-enabled": false,
    "script-torrent-added-filename": "",
    "script-torrent-done-enabled": false,
    "script-torrent-done-filename": "",
    "script-torrent-done-seeding-enabled": false,
    "script-torrent-done-seeding-filename": "",
    "seed-queue-enabled": false,
    "seed-queue-size": 10,
    "show-backup-trackers": false,
    "show-extra-peer-details": false,
    "show-filterbar": true,
    "show-notification-area-icon": false,
    "show-options-window": true,
    "show-statusbar": true,
    "show-toolbar": true,
    "sort-mode": "sort-by-name",
    "sort-reversed": false,
    "speed-limit-down": 100,
    "speed-limit-down-enabled": false,
    "speed-limit-up": 100,
    "speed-limit-up-enabled": false,
    "start-added-torrents": true,
    "start-minimized": false,
    "statusbar-stats": "total-ratio",
    "tcp-enabled": true,
    "torrent-added-notification-enabled": true,
    "torrent-added-verify-mode": "fast",
    "torrent-complete-notification-enabled": true,
    "torrent-complete-sound-command": [
        "canberra-gtk-play",
        "-i",
        "complete-download",
        "-d",
        "transmission torrent downloaded"
    ],
    "torrent-complete-sound-enabled": true,
    "trash-original-torrent-files": false,
    "umask": "022",
    "upload-slots-per-torrent": 8,
    "user-has-given-informed-consent": true,
    "utp-enabled": true,
    "watch-dir": "",
    "watch-dir-enabled": false
}

{% endhighlight %}

Note: To get the remote functionality working behind the vpn, ensure your "rpc-bind-address" is set to your local ip address, and your "rpc-host-whitelist" is set to your local subnet. 

Then we can set up the service with the following commands:


{% highlight posh %}
nssm.exe install transmission "I:\Transmission\Transmission\transmission-daemon.exe"
nssm.exe set transmission AppDirectory "I:\Transmission\Transmission"
nssm.exe set transmission AppParameters "-g I:\Transmission\ -f"
nssm.exe set transmission AppExit Default Exit
nssm.exe set transmission Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}

Transmission uses port 9091 by default, so you can access it at http://localhost:9091.

### ExpressVPN

winget install expressvpn.expressvpn

Click sign in. Either copy and past your activation code, or use the "Sign in with email sign in link" option. Launch on startup, either ok or no thanks to helping them improve

Click the hamburger menu in the top left and go to options. Make sure that "Launch ExpressVPN on Windows startup" is checked, as well as "Connect to the last used location when ExpressVPN is launched". 

Then you'll want to click on "Manage connection on a per-app basis", and "Only allow selected apps to use the vpn". I like to have my torrent client go through the VPN for added security, so I add it here. Click "Add Application" and navigate to the transmission-daemon.exe file. I put it in I:\Transmission\Transmission. Click "Add Application" and then "Done".

### Tdarr

I used winget to install some prerequisites:

{% highlight posh %}
    winget install handbrake.handbrake.cli
    winget install gyan.ffmpeg
    winget install moritzbunkus.mkvtoolnix
{% endhighlight %}

download and extract
https://github.com/HaveAGitGat/Tdarr/releases

I extracted it to I:\Tdarr and ran Tdarr_Updater.exe. This will install both the node and the server in the same directory. The window will close when it's finished. 

First you'll want to execute Tdarr_server.exe and then Tdar_node.exe. Once those have ran, you can close the windows. 

Then you'll want to go to ..Tdar\configs and edit the Tdar_Node_Config.json. This is mine:

{% highlight json %}
{
    "nodeName": "host-node",
    "serverIP": "127.0.0.1",
    "serverPort": "8266",
    "handbrakePath": "C:\\Users\\adumb\\AppData\\Local\\Microsoft\\Winget\\Links\\HandBrakeCLI.exe",
    "ffmpegPath": "C:\\Users\\adumb\\AppData\\Local\\Microsoft\\Winget\\Links\\ffmpeg.exe",
    "mkvpropeditPath": "C:\\Program Files\\MKVToolNix\\mkvpropedit.exe",
    "pathTranslators": [
        {
            "server": "D:\",
            "node": "D:\"
        }
    ],
    "logLevel": "info",
    "priority": -1,
    "cronPluginUpdate": ""
}
{% endhighlight %}


Then we can set up the service with the following commands:
{% highlight posh %}
    nssm.exe install tdarr_server "I:\Tdarr\Tdarr_Server\tdarr_server.exe"
    nssm.exe set tdarr_server AppDirectory "I:\Transmission\Transmission"
    nssm.exe set tdarr_server AppExit Default Exit
    nssm.exe set tdarr_server Start SERVICE_DELAYED_AUTO_START

    nssm.exe install tdarr_node "I:\Tdarr\Tdarr_Node\tdarr_node.exe"
    nssm.exe set tdarr_node AppDirectory "I:\Tdarr\Tdarr_Node"
    nssm.exe set tdarr_node AppExit Default Exit
    nssm.exe set tdarr_node Start SERVICE_DELAYED_AUTO_START
{% endhighlight %}
## Additional Services

### AMP by CubeCoders
download from https://cubecoders.com/AMPInstall

Run the .msi and select custom setup. We want to change the location for the 'Instance Manager Components' to I:\AMP. I also changed 'AMP Datastore' to D:\AMPDatastore. Then you can finish the rest of the install. 

It will launch a new window and install all of the dependencies.

The instance manager will open and you'll create an admin account. It will also install the service for you.
### Dynamic DNS Client for namecheap
 download the exe from https://www.namecheap.com/support/knowledgebase/article.aspx/111/11/using-namecheap-dynamic-dns-client-version-20x-beta

 I put it in I:\NamecheapDDNSClient

{% highlight posh %}
nssm.exe install namecheapddns "I:\NamecheapDDNSClient\Dynamic DNS Client.exe"
nssm.exe set namecheapddns AppDirectory "I:\NamecheapDDNSClient\"

{% endhighlight %}
### Bar Assistant

### Salt Rim Vue

### Nginx

Install Nginx: Download the latest stable version of Nginx for Windows from the [official website](https://nginx.org/en/download.html) and extract it. I put it in I:\nginx.

Backup the Default Configuration (Optional): Before making any changes, consider backing up the default Nginx configuration located at I:\nginx\conf\nginx.conf.

Create a New Configuration File: Create a new Nginx configuration file for your reverse proxy settings. You can create a new file in the I:\nginx\conf\ directory. Give it a descriptive name, such as reverse-proxy.conf.

Edit the Configuration File: Open the newly created configuration file in a text editor and add the reverse proxy configuration as shown earlier.

Test the Configuration: Before applying the new configuration, test it for syntax errors. Open a command prompt with administrator privileges and run:
{% highlight posh %}

It took me some time to figure out how to get reverse proxy working. I havent been able to update the services to accept firebase yet but it's much easier to connect to each one like this. 

{% highlight posh %}
 <nginx config goes here>
{% endhighlight %}

Instead of opening up port 80, I forwarded port 8080 instead.

With Namecheap dynamic DNS set up and configured. I can go to http://host.birdsbored.com:8080/SERVICE to access the service. 
