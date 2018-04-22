---
layout: post
title: "SFDX With Charles Proxy"
description: ""
category: sfdx
tags: [sfdx]
---
{% include JB/setup %}

The SFDX CLI is a pretty powerful tool that can help a lot with solving problems or debugging your Salesforce instance. One feature I particularly like is using the `--perfloglevel` option to see exactly what's happening when we make an API call. I use the awesome [jq](https://stedolan.github.io/jq/) utility to quickly take a look at what's happening. For example:
```
(clotf) ➜  ~ sfdx force:data:record:get -u DevHub --json --perfloglevel basic -s account -i 0014600000V8jNg | jq ".perfMetrics[1].perfMetrics" -r | jq
{
  "version": "1.0.0",
  "callTree": {
    "name": "http-request",
    "attachment": {},
    "startTime": 1390938186527800,
    "endTime": 1390938225452642,
    "ownTime": 6874226,
    "childTime": 32050616,
    "totalTime": 38924842,
  },
  "summary": [
    {
      "metrics": "db-conn-wait",
      "totalTime": 1025650,
      "totalCalls": 5
    },
    {
      "metrics": "rest-api",
      "totalTime": 30238219,
      "totalCalls": 1
    },
    {
      "metrics": "db-exec",
      "totalTime": 7080053,
      "totalCalls": 12
    },
    {
      "metrics": "cache-memcached",
      "totalTime": 8239041,
      "totalCalls": 47
    },
    {
      "metrics": "http-request",
      "totalTime": 38924842,
      "totalCalls": 1
    },
    {
      "metrics": "request-tracing-enable",
      "totalTime": 5735,
      "totalCalls": 5
    }
  ]
}
```

I trimmed out the full callTree, but I have it [available in a gist](https://gist.github.com/cdcarter/ff39e4eac4bf7ef3366ccad0464f3ba6) if you want to see *all* the data you can get with the perfloglevel option.

I wanted to figure out how it...works. This is awesome data, so why can't I get at it with normal API calls? ...I bet I can.

I'm going to use the tool [Charles Proxy](https://www.charlesproxy.com/) on macOS to "man in the middle" the SFDX cli and watch the exact HTTP requests it's making. This means we can try them in our own API client, CumulusCI. First, I installed Charles Proxy and entered my administration password to let it set up the SSL proxy. Make sure this is permissable with your administrator before you go installing new software! Once it's installed, use the toolbar to go to Help -> SSL Proxying -> Install Charles Root Certificate. Keychain Access will pop open. Double click the newly created and adjust the Trust settings to "Always Trust". Check the latest Charles [documentation for exactly how to do this on macOS](https://www.charlesproxy.com/documentation/using-charles/ssl-certificates/) in case the process has changed.

Once you've done that, navigating to websites in Safari will start them popping up in the Charles Proxy sidebar. Wow! But if I run my DX command again...nothing shows up. What gives? We need to export a pile of environment variables to tell SFDX (and Node.js) to use our proxy and allow the self-signed certificate:
```
export http_proxy="http://localhost:8888"
export https_proxy="http://localhost:8888"
export NODE_TLS_REJECT_UNAUTHORIZED=0
```

Once I do that, I can run my request and see the full details in Charles! Awesome
<img src="https://imgur.com/a/yg729ks.jpg"/>

