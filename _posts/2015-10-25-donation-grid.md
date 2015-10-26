---
layout: post
title: "Donation History Grid"
description: "Version 1.0"
category: donationhistory
tags: []
---
{% include JB/setup %}

It's a common request, especially from Development Directors who came from Blackbaud products.
 
 > Can't I just see a yearly summary of their giving on their contact record?

Well now you can:

![LEX Screenshot](https://camo.githubusercontent.com/b79d950b5669a6fe832c7d855fa4a0d4374e7686/687474703a2f2f63646361727465722e6769746875622e696f2f53616c6573666f726365446f6e6174696f6e486973746f72792f6c65782d64656d6f2e706e67)

Don't worry, it's not Lightning only, it works right in your regular old Salesforce page layouts:

![Aloha screenshot](https://camo.githubusercontent.com/166942099e506fcc61d47e6e6c082b371cb2f005/687474703a2f2f63646361727465722e6769746875622e696f2f53616c6573666f726365446f6e6174696f6e486973746f72792f616c6f68612d64656d6f2e706e67)

[Caroline Renard](http://www.carolinerenard.com/) saw [a blog post](http://www.spotlightonsalesforce.com/blog/2015/8/7/cleaning-up-fields-on-a-contact-record-need-better-title) by [Michelle Paul](http://twitter.com/fuzzydinosaur) and [Allison Klein](http://twitter.com/nyalli) where they used an old bit of [Evan Callahan](http://twitter.com/groundwired)'s [Groundwire Base](https://github.com/Groundwire/GWBase/).

![GW Code](http://i.imgur.com/imjWsMZ.png)

Caroline asked me "what would it take to re-implement that, show record types, and make it work with the NPSP and PatronManager?" I replied "a donation to [TeenTix](http://teentix.org), a weekend, and I'll throw in soft credit display as well." And like that, it came together, and it's [open source](https://github.com/cdcarter/SalesforceDonationHistory) and freely available for your organization!

Interested in installing it? [Follow the directions here](http://cdcarter.github.io/SalesforceDonationHistory/install.html). And look forward to a blog post next week which will describe how you can customize it even further to show more than one grid per page with different summaries!
