LINK: link to the repository

Instructions included with the repository.

## SQL Injection

Description:

In one of the routes specifically the one handling deleting a post it is handled in a way that allows for SQL injection:

```
await prisma.$executeRaw(`DELETE FROM 'Post' WHERE id=${postId}`);
```

Since `postId` comes from query parameters a malicious actor might be able to abuse this to perform unauthorized operations in the database.

How to fix:

When using a query builder like Prisma avoiding using raw SQL yourself is often smart. Any operation that can be done with the query builder should be done so. This is of course because:

1. If not then what is the point of using query builder in the first place?
2. It handles SQL injection by default.

If you absolutely have to use raw SQL the above faulty statement could have been written safely like so:

```
await prisma.$executeRaw`DELETE FROM 'Post' WHERE id=${postId}`;
```

This is calling the function via template literal and in the case of `$executeRaw` it handles escaping parameters for you.

## Sensitive Data Exposure

Description:

Our application exposes an API endpoint called `/api/users` which you can use to get all the users. This is a bit problematic since it also leaks users' emails. There might also be other endpoints like this.

Once our app goes live this might not be nice as our users emails very easily can end up on some spamlist. Likewise you might choose to use an acronym but email exposes your real email. Mostly users expect the email submitted to remain private unless explicitly told otherwise.

How to fix:

Make sure any API that exposes sensitive information is secured. Practically this means making users log-in and verifying them before returning the users. In the Next.JS context I have found NextAuth.js to be a good solution: https://next-auth.js.org/. Typically rolling out your own authentication methods isn't the smartest way to go about it.

There also has to be API endpoints that can be accessed by anyone but extra care should be made to make sure they only expose information you don't mind anyone having access to. Doubly so when dealing with your users' sensitive data.

## Security Misconfiguration

Description:

The owner of this blog system has not configured HTTPS at all. This doesn't really show in the code but trust me it really is true. Currently not that much sensitive data is shared but for example passwords transferring over unsecured connection seems unpleasant. Indeed it seems unlikely that the user would feel good logging in to a site using HTTP.

Specifically HTTP is vulnerable to man-in-the-middle and eavesdropping attacks. In addition to the scenario mentioned above attackers might be able to modify the page and inject malware. Modern browsers have pretty noticeable warning these days when browsing a page in HTTP.

How to fix:

Simplest way to configure HTTPS is to use a managed hosting system. At least from my experience e.g. Vercel handles this out of the box. Typically I like to configure this kind of stuff as little as possible myself. It seems like there are some good guides on how to do it without using managed hosting solution like: https://developers.google.com/web/fundamentals/security/encrypt-in-transit/enable-https.

## XSS

Description:

Our blog has a rather naive way to display rich content to user. Some blogs allow markdown to be used when making the content that then gets converted to HTML. Our implementation has skipped a few steps in this. Here is how it's done:

```
<div dangerouslySetInnerHTML={{ __html: post.content }} />
```

Idea is that writers write raw HTML or some rich editor converts it to HTML. Then we render it in a div. This works put as you might have guessed with the use of `dangerously` this is a problematic approach. It exposes for a variety of XSS attacks. Essentially it allows arbitrary Javascript getting executed. So we might submit a blog post that for example redirects the user to malicious site.

How to fix:

If you for some reason need to render user submitted HTML then maybe first consider: don't. Some rich editors allow HTML but taking Markdown out might be the smarter plan.

## Using Components with Known Vulnerabilities

Description:

The project is using Next.JS of version 9.4.0. This version of the framework contains a security flaw that allows open redirects to happen. An attacker might be able to use it to conduct a phishing attack by redirecting to an attackers domain from a trust domain. This security flaw was fixed with version 9.5.4 and you can see the full details here: https://github.com/vercel/next.js/releases/tag/v9.5.4

In general dependencies having vulnerabilities is a big issue in Javascript world. Another example of an how an attack can be conducted via vulnerable dependency can be found in this blog: https://medium.com/intrinsic/common-node-js-attack-vectors-the-dangers-of-malicious-modules-863ae949e7e8

How to fix:

Upgrade Next.JS to the latest version where the weakness has been fixed. Simply `npm i next@latest` will do the trick. Additionally `npm audit` is an useful command to find out packages with problems. It also detects problems with dependencies of dependencies and so on. Adding a step to CI/CD that checks for packages with problem might be helpful to avoid such problems in the future.
