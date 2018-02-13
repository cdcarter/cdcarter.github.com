---
layout: post
title: "Administrating a DevHub"
description: ""
category: sfdx
tags: [sfdx]
---
{% include JB/setup %}

I help administer a SalesforceDX Dev Hub that over 40 developers, quality engineers, doc writers, product managers, and other team members use. Between automated builds and developer activities, we average about NUMBER scratch orgs a day! We don't run any business process in the DevHub, but if you're supporting a large team using a DevHub, you might like these tricks up your sleeve.

## ScratchOrgInfo Command

A few times a week, I find myself having to log into my Dev Hub to look up a ScratchOrgInfo record and see if it was deleted, if it failed to signup, what features were actually used, etc... I get all sorts of support requests from our developers and the first line of defense is to pull up the ScratchOrgInfo and see where we started. 

Using the excellent [jq](https://stedolan.github.io/jq/) (a simple `brew install jq` away) I built this simple function that I added to my `.bash_profile`.

```
function scratchorginfo() {
    local DEVHUB="DevHub"
    sfdx force:data:soql:query -q "SELECT AlmReference,DeletedDate,Description,DurationDays,Edition,ErrorCode,ExpirationDate,Features,HasSampleData,LastLoginDate,Name,Namespace,OrgName,ScratchOrg,SignupInstance,Status,SignupUsername FROM ScratchOrgInfo WHERE ScratchOrg = '$1'" -u $DEVHUBALIAS --json |
    jq '.result.records[0] | del(.attributes)'
}
```

Usage is simple:
```
(cdcarter) ➜  ~ scratchorginfo 00D54000000D6mg
{
  "AlmReference": null,
  "DeletedDate": null,
  "Description": null,
  "DurationDays": 1,
  "Edition": "Developer",
  "ErrorCode": null,
  "ExpirationDate": "2018-02-14",
  "Features": null,
  "HasSampleData": false,
  "LastLoginDate": null,
  "Name": "00016024",
  "Namespace": null,
  "OrgName": "CumulusCI - Dev Workspace",
  "ScratchOrg": "00D54000000D6mg",
  "SignupInstance": "CS40",
  "Status": "Active",
  "SignupUsername": "test-dev-org@example.com"
}
```

## Grab A Coworkers Scratch Org

If everyone on your team is using the same connected app, as an admin, you can use the JWT flow to connect to your teammates scratch orgs. 
