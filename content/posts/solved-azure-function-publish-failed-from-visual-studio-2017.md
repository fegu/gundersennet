---
author: wpgundersen
categories:
  - webtech
date: "2020-03-25T11:23:05+00:00"
guid: https://gundersen.net/?p=475
parent_post_id: null
post_id: "475"
tags:
  - azure
  - functions
  - serverless
  - visual-studio
title: 'Solved: Azure Function "Publish Failed" from Visual Studio 2017'
url: /solved-azure-function-publish-failed-from-visual-studio-2017/

---
The "Publishing failed" error message will make you feel cursed. It appears when publishing an Azure serverless Function directly from Visual Studio 2017. Some never see it, but those who do can't seem to get rid of it.

{{< figure src="/wp-content/uploads/2020/03/publishfailed%5F450px.png" alt="" caption="" >}}

I tried all the recommended internet forum voodoo of creating new deployment profiles, restarting Visual Studio, cleaning the project and downloading deployment profiles. If it did go away for once, it always came back.

What finally worked for me was upgrading Visual Studio 2017 to the latest version (as of now v15.9.21) and switching from Web Deploy to Zip Deploy. This is done by checking the new "run from package file" option.

{{< figure src="/wp-content/uploads/2020/03/zipdeploycheckbox%5F450px.png" alt="" caption="" >}}

Zip Deploy has [other benefits as well](https://docs.microsoft.com/en-us/azure/azure-functions/run-functions-from-deployment-package), such as shorter cold starts for large deployments and ability to replace a running function in production without downtime.

You can see the deployment profile type by expanding the PublishProfiles folder.

{{< figure src="/wp-content/uploads/2020/03/publishprofiles.png" alt="" caption="" >}}
