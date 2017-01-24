---
layout: post
title:  "Domain Masking"
date:   2017-01-23 19:25:59 +0000
categories: url domain masking
---

[Domain Masking](https://en.wikipedia.org/wiki/Domain_masking) is the act of hiding the actual domain or URL where the content is being hosted from the user.

This can be achieved by redirecting the user to a different URL from the one inputed without updating the browser's
URL field. Example:

![We're Jerks!]({{ site.url }}/{{ site.baseurl }}/assets/wearejerks1.png)

Jerks might be fine with this website, but everybody else might want to blacklist it from their networks (using firewalls or proxies), search engine bots (like Google) or advertising.

A shady way jerks could work around being blocked is by using domain masking, which can be achieved by using [URL redirection](https://en.wikipedia.org/wiki/URL_redirection).

You might have seen this before: Let's say you type "www.google.com" in your browser, but you're in France. Google will
redirect you to "www.google.fr" based on your location.

You will see the URL field in your browser change to "www.google.fr" though. That's not the case for domain masking: The URL field in your browser won't change even though you have been redirected to a different domain.

Jerks could create a domain named "www.wearecool.com" for instance and keep the web-site's content in this domain, which actually serves pages to users.

![We're Pretending to be Cool, but We're Actually Jerks!]({{ site.url }}/{{ site.baseurl }}/assets/wearecool1.png)

When a jerk types "www.wearejerks.com" in their web browsers, they would be directed to "www.wearecool.com" and the user's
URL would not change.

![We Still are Jerks!]({{ site.url }}/{{ site.baseurl }}/assets/wearejerks1.png)

As far as jerks are concerned, they are still jerks, but from the point of view of everyone else (such as ad placement code in the web pages for instance), they would be seen as "cool", given that the actual content is served from "www.wearecool.com".

This cheat still requires redirection though (which is only hidden from the user), so if you inspect the actual URL calls made under the hood of your browser, you still should be able to see calls to "www.wearecool.com":

![Chrome Net-Internals Events]({{ site.url }}/{{ site.baseurl }}/assets/chromenetinternalsevents1.png)

All you need to do is open the following link in a tab in your browser (Chrome):

[chrome://net-internals/#events](chrome://net-internals/#events)

After that, you could open the website and watch all calls required to render the page, including the redirection calls we've talked about (the masked ones).

In our example, even though we could not see "www.wearecool.com" in the browser (in the URL field and even when we hover the mouser pointer over page links, if [URL rewriting](https://en.wikipedia.org/wiki/URL_rewriting) is being used), we should still be able to see calls to "www.wearecool.com" using tools such as Chrome Net-Internals Events or [Chrome Developer Tools](https://developer.chrome.com/devtools).

A more user friendly way to do this is perhaps by extracting all links from a page using [Link Klipper](http://www.codebox.in/products/linkklipper/), which enables you to save all extracted links to a `*.csv` file, which can be opened with a simple plain text or spreadsheet editor, such as Text Editor, Notepad, [LibreOffice Calc](https://en.wikipedia.org/wiki/LibreOffice_Calc) or Excel:

![Link Klipper CSV File]({{ site.url }}/{{ site.baseurl }}/assets/linkklipper1.png)

It should be more or less obvious to see if a redirection to another domain happened:

There are other more sophisticated ways to do domain masking though: Using a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy) for instance:

![Reverse Proxy in Action]({{ site.url }}/{{ site.baseurl }}/assets/reverseproxy1.png)
[(Image from Wikipedia)](https://en.wikipedia.org/wiki/Reverse_proxy)

The calls from the end users go through a proxy server, which actually retrieves content. We need to find out more about the server to determine if that's the case.

There are many on-line tools to extract information about a server, one of them is ["Browser Spy"](http://browserspy.dk/). Just type the domain you want to inspect:

![Browser Spy]({{ site.url }}/{{ site.baseurl }}/assets/browserspy1.png)

You will notice the field "Web Server: ECS ECS (oxr/8382)", which means that this particular server is hosted by
Amazon.com's cloud. If the "Web Server" was ["Nginx"](https://en.wikipedia.org/wiki/Nginx), that would be evidence that a reverse proxy is being used, but given that is hosted in Amazon.com's EC2 cloud, it's not possible to tell (could be the case that the ECS server is actually running a "Nginx" instance).

Another way to get more information about the web-site is by using ["Build With"](https://builtwith.com), another on-line tool. Just navigate to "Build With" and type the domain you want to inspect:

![Built With]({{ site.url }}/{{ site.baseurl }}/assets/builtwith1.png)

The report says that they are using [Varnish](https://varnish-cache.org/docs/4.0/tutorial/introduction.html), which is a caching HTTP reverse proxy, so they are indeed using a reverse proxy. Its primary use is for caching though (to reduce response time), not masking, so one can't tell with 100% assurance that something shady is going on in there.

"Built With" returns all sorts of information, including the technology stack used to develop the website and third-party services, such as advertising:

![Built With: Advertising Providers]({{ site.url }}/{{ site.baseurl }}/assets/advertising1.png)

You can see that this website is served with ads from Taboola, DoubleClick, Google and Facebook.

Keep in mind that I'm a coder, not a network security expert. I hope this is useful information though.
