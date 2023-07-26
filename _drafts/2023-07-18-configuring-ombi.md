---
layout: post
title:  "Configuring Ombi"
summary: "This is how I configured Ombi"
author: adumbbird
category: software
thumbnail: /assets/img/posts/HumbleBundleDiscord.png
keywords: software, automation, server, home server, windows, windows server, windows 11, emby, ombi, sonarr, radarr, jackett, transmission, expressvpn
---

# Installation

# Wizard walkthrough

click OK
Optionally add media servers. I'll configure this later. 
Create a local admin
Ombi Config - see [here](https://docs.ombi.app/settings/customization/)
- Application Name: this will replace most references to the word "Ombi" 
- Application URL: This is for any external links that Ombi generates through the notification methods. This should be publically accessible. It should be a full URL including the protocol (http/https) and port if required.
- Custom Logo: This will add the logo to the login, landing, and any notification.
Once you are finished, you can log in with the local admin account.

# General Configuration

When finished with the wizard, you can click on Users in the left sidebar. Here you can add users and configure their permissions. There are import options for Plex and Emby users once you have configured those services.

In settings, there are tabs at the top. I'll go through some of the important ones.

## Configuration:
- User Management: This is where you can configure default permissions for users.
- Authentication: Here you can set logging in without a password, Plex OAuth, and using Authentication with a Header Variable

Media Servers is how you'll configure plex and Emby

## Media Server

### Emby
Click add server
Enter in your emby server  ip, port, and apikey (you can find this in your emby server settings). 
Click on Discover Server Information to auto populate the rest of the fields.
Click on Test Connectivity to make sure it works. 
Click Submit. 
Now you can enable the config with the slider at the top, and then trigger a Sync. 
### Plex
You can use your plex login to auto populate the information, or you can manually add the server

## TV
This is where you will configure ombi to send requests to Sonarr. 
### Sonarr
Enter your Sonarr Hostname, port, and API key.
Click on Test Connectivity to make sure it works.

From here you can click Load Qualities, Load Folders, and Load Tags to auto populate the fields. Then you can select the default profiles for new requests. 

# My setup
Information:
- BaseURL = /ombi

Emby Config:
- ServerName: AdumbEmby
- hostname: localhost
- ServerID: (auto populated)