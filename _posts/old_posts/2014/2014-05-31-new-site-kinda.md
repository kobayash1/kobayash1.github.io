---
layout: post
title: "New Site! Kinda"
description: "No more WordPress"
category: blog
headline: "Good riddance"
comments: true
share: true
tags: [blog, wordpress, jekyll, github, cloudflare]
---
Premise:  I had a WordPress blog on chrisbayot.com, and this freely hosted GitHub Pages jekyll site on blog.chrisbayot.com.  One of them had to go.

The WordPress had to go.  It's not as fast nor as secure as the jekyll site.  It was also hosted on a cloud server I had to pay for (albeit really cheap).  The plan of action was simple.  Trash WordPress, and move the jekyll blog to chrisbayot.com.

Here was the problem:  when using custom domains with static sites hosted on GitHub Pages, they much prefer for them to be subdomains.  This meant I would keep blog.chrisbayot.com, but nothing on chrisbayot.com.  Not wanting that, I got really stubborn and dug some more around the net.

According to GitHub, static sites hosted by them when using custom subdomains get extra benefits as opposed to those using custom apex domains.  If you use blog.example.com for your GitHub Pages site, your site will get benefits that example.com won't enjoy.

> We strongly recommend that you use a custom subdomain instead of a custom apex domain for the following three reasons:
> 
> * A custom subdomain gives your GitHub Pages site the benefit of our Content Delivery Network.
> * A custom subdomain will not be affected by changes in the underlying IP addresses of GitHub's servers.
> * A custom subdomain allows pages to load significantly faster than a custom apex domain because its Denial of Service protection can be implemented more efficiently.
>
> <a href="https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages"><small><cite title="Setting Up a Custom Domain with GitHub Pages">GitHub</cite></small></a>

Allow me to quote some of the articles I found on the web that explain everything:

> GitHub Pages with a custom domain is unbelievably slow.
>
> When a domain has a DNS A record (pointing to GitHub Pages’ IP), GitHub’s DDoS mitigation technology has no alternative than to make a long redirect.  What I’ve noticed is that sometimes it’s not just a big long redirect but multiple 302 redirects.
> <a href="http://instantclick.io/github-pages-and-apex-domains"><small><cite title="GitHub Pages and Apex Domains">InstantClick</cite></small></a>

InstantClick's solution was to simply use the www subdomain, or use CloudFlare as the DNS provider.  Why CloudFlare?

> [Y]ou can now safely use a CNAME record, as opposed to an A record that points to a fixed IP address, as your root record in CloudFlare DNS without triggering a number of edge case error conditions because you’re violating the DNS spec.
> 
> What we needed was a way to support a CNAME at the root, but still follow the RFC and return an IP address for any query for the root record. To accomplish this, we extended our authoritative DNS infrastructure to, in certain cases, act as a kind of DNS resolver. What happens is that, if there's a CNAME at the root, rather than returning that record directly we recurse through the CNAME chain ourselves until we find an A Record. At that point, we return the IP address associated with the A Record. This, effectively, "flattens" the CNAME chain.
> 
> The biggest benefit is that this allows the flexibility of having CNAMEs at the root without breaking the DNS specification.
> <a href="http://blog.cloudflare.com/introducing-cname-flattening-rfc-compliant-cnames-at-a-domains-root"><small><cite title="Introducing CNAME Flattening:  RFC-Compliant CNAMEs at a Domain's Root">CloudFlare</cite></small></a>

Without going into too much detail, pointing the GitHub page to an apex domain involves an old process that invokes the aforementioned problems.  CloudFlare created a solution that allows one to point an apex domain to an alias, as opposed to an IP address.  CloudFlare wasn't the first to offer this kind of service, but they may be the first to do it in a way that doesn't defy the DNS standards and how DNS works.

After changing the nameservers on chrisbayot.com from Digital Ocean's to CloudFlare's, I created the CNAME record to point the root to kobayash1.github.io.  And now here we are!  My static site hosted on kobayash1.github.io resolves to chrisbayot.com.  The site doesn't suffer from the redirects that one would expect from a GitHub Pages site pointing to a top level domain.  In fact, it even runs on CloudFlare's own CDN and gets a lot of their own nifty security features.

And on top of that, between the services CloudFlare, GitHub, and jekyll provide, all of this is *free*.
