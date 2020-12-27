LINK: link to the repository

Instructions included with the repository.

## SQL Injection

Description:

how to fix it...

## Broken Access Control

## Sensitive Data Exposure

Description:

The owner of this blog system has not configured HTTPS at all. This doesn't really show in the code but trust me it really is true. Currently not that much sensitive data is shared but for example passwords transferring over unsecured connection seems unpleasant. Indeed it seems unlikely that the user would feel good logging in to a site using HTTP.

Specifically HTTP is vulnerable to man-in-the-middle and eavesdropping attacks. In addition to the scenario mentioned above attackers might be able to modify the page and inject malware. Modern browsers have pretty noticeable warning these days when browsing a page in HTTP.

How to fix:

Simplest way to configure HTTPS is to use a managed hosting system. At least from my experience e.g. Vercel handles this out of the box. Typically I like to configure this kind of stuff as little as possible myself. It seems like there are some good guides on how to do it without using managed hosting solution like: https://developers.google.com/web/fundamentals/security/encrypt-in-transit/enable-https.

## XSS

## Using Components with Known Vulnerabilities

Description:

The project is using Next.JS of version 9.4.0. This version of the framework contains a security flaw that allows open redirects to happen. An attacker might be able to use it to conduct a phishing attack by redirecting to an attackers domain from a trust domain. This security flaw was fixed with version 9.5.4 and you can see the full details here: https://github.com/vercel/next.js/releases/tag/v9.5.4

In general dependencies having vulnerabilities is a big issue in Javascript world. Another example of an how an attack can be conducted via vulnerable dependency can be found in this blog: https://medium.com/intrinsic/common-node-js-attack-vectors-the-dangers-of-malicious-modules-863ae949e7e8

How to fix:

Upgrade Next.JS to the latest version where the weakness has been fixed. Simply `npm i next@latest` will do the trick. Additionally `npm audit` is an useful command to find out packages with problems. It also detects problems with dependencies of dependencies and so on. Adding a step to CI/CD that checks for packages with problem might be helpful to avoid such problems in the future.
