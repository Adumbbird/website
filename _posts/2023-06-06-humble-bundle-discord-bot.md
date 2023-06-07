---
layout: post
title:  "Humble Bundle Discord Bot"
summary: "A discord bot that will post the latest humble bundle deals to a discord channel."
author: adumbbird
date: '2023-06-06 22:00:00 +0530'
category: software
thumbnail: /assets/img/posts/HumbleBundleDiscord.png
keywords: software, automation, humble,bundle, discord, bot
---

I spent some time playing around in azure, in order to create some functions that would post the latest humble bundle deals to a discord channel. I have a few ideas for other things I could do with this, but for now it just posts the latest humble bundle deals to a discord channel. I've pulled out some of the code to highlight here as an example of how to pull the data.

{% highlight posh %}
#pull the initial page from humblebundle
$url = 'https://www.humblebundle.com/bundles'
$html = Invoke-WebRequest -Uri $url -UseBasicParsing
$html |ForEach-Object { 
        $_ -match '<script id="landingPage-json-data" type="application\/json">\n(?<content>.*)\n<\/script>' 
    }
$results =$matches[0].remove(0,60).replace('</script>','')|ConvertFrom-Json

#once we have the data, we can pull out the urls for the different bundles
$Gameurls = $results.data.games.mosaic.products| select-object product_url,'start_date|datetime','end_date|datetime',marketing_blurb,detailed_marketing_blurb,tile_image
$Softwareurls = $results.data.software.mosaic.products| select-object product_url,'start_date|datetime','end_date|datetime',marketing_blurb,detailed_marketing_blurb,tile_image
$booksURL = $results.data.books.mosaic.products| select-object product_url,'start_date|datetime','end_date|datetime',marketing_blurb,detailed_marketing_blurb,tile_image

{% endhighlight %}

Then the function saves the data to azure table storage. I have another function that pulls the data from the table, gets a list of webhooks that it needs to send to, and then sends the data to the webhooks. 

I'm not going to post the full code here, but you can find it on my github [here](https://github.com/BirdsBored-LLC/HumbleBundleDiscordBot)

I hope this helps. I can also add other webhooks to the bot, so if you have a webhook that you want to add, let me know and I'll add it to the bot!
Adam
