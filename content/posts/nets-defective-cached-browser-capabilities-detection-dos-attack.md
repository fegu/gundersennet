---
_edit_last: "1"
author: wpgundersen
categories:
  - webtech
date: "2021-01-30T23:38:49+00:00"
guid: https://gundersen.net/?p=512
parent_post_id: null
post_id: "512"
tags:
  - .net
  - asp.net
  - browser-capabilities
  - browsercaps
  - c#
  - chrome
title: .Net's defective-by-default browser capabilities detection lends itself to DoS
url: /nets-defective-cached-browser-capabilities-detection-dos-attack/
wp_featherlight_disable: ""

---
Ever since .Net v4, in all versions including the last classic .Net version v4.8, the built-in browser detection accessible in the Request.Browser (a HttpBrowserCapabilities object) has a serious flaw which will bite you as soon as your site gets several visitors per minute.

What happens, if you rely on this object, is that users will randomly report having their browser mis-identified.

This post shows why it happens and how to solve it. The issue is also easy to reproduce locally, worth a few fun minutes. If you are so inclined, the problem also lends itself to a DoS attack.

Even though this problem seems only to affect classic .Net (not .Net Core), there is a huge number of running web apps out there, and still new being developed, on this framework.

We have a website relying on some of the later browser features, so we are using browser detection to tell users with older browsers to upgrade, since we absolutely need these features. It works fine for a while, then randomly, users with recent browser versions are suddenly told they need to upgrade. Once this occures it typically lasts for the rest of the day.

To reproduce, create an asp.net project (web forms, MVC, doesn't really matter) with this code in a .aspx file:

```
<script language="c#" runat="server">
public void Page_Load(){
    Response.Write(Request.Browser.Browser
    + " v" + Request.Browser.MajorVersion);
}
</script>

```

Visit this page, locally or on a server, and you get for instance `Chrome v88`. So far, so good.

The browser is classified from the User-Agent HTTP header. My Chrome sends this: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.104 Safari/537.36`

Now, quickly change the `Chrome/88` in the middle of the string to `Chrome/86` (or any other number) and retry. For instance in Curl:

```
curl http://localhost:58571 -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4324.104 Safari/537.36"
```

Still v88? How strange... Let's refresh a few times. Still v88. Let's think for a minute...hmm...refresh again. Gone! Now, at last, it shows v86!

I had to look into the [offending source code](https://referencesource.microsoft.com/System.Web/Configuration/HttpCapabilitiesEvaluator.cs.html#https://referencesource.microsoft.com/System.Web/Configuration/HttpCapabilitiesEvaluator.cs.html,3774a0750f8cc853,references) from the .Net framework.

What happens is, for performance, the result of the browser classification is stored in a hash-table with the User-Agent string as the key. But not the entire string, no, that would have saved us a lot of headache, no, the key is just the first 64 characters (source line 46).

The Chrome version number is located at position 90 and thus not part of the key at all. To add to the frustration, the cache has a sliding-window (source line 247,304, 369) 1 minute expiry (source line 147). This means: as long as the cache key is read at least once a minute, it stays. Hilarity ensues.

The fix is straight-forward, put this in your `web.config`

```
<configuration>
    <system.web>
        <browserCaps userAgentCacheKeyLength="512" />
    </system.web>
</configuration>
```

Why 512? It turns out that .Net caps the User-Agent string at 512 characters to avoid resource attacks (source line 188). I imagine someone at Microsoft deciding that 512 characters ought to be enough for anybody.

## Denial-of-Service implication

Without this line in your web.config, your site is also vulnerable to a DoS attack if you, like us, rely on browser identification to enable some functionality. An attacker would wait for low-traffic hours, typically the middle of the night, to issue requests with the first 64 bytes looking like the most commonly used User-Agents on the web, but the rest of the string tweaked with low version numbers or other content to lead the browser capabilities to think "it's an older browser, sir, but it checks out".

As long as one request per User Agent is issued per minute, these are permanently mis-identified. An exceptionally low-cost and low-effort DoS attack.
