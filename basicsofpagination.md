## Basics of Pagination

To start with, it's important to know a few facts about receiving paginated items:

1. Different API calls respond with different defaults. For example, a call to
[List public repositories](/rest/reference/repos#list-public-repositories)
provides paginated items in sets of 30, whereas a call to the GitHub Search API
provides items in sets of 100
2. You can specify how many items to receive (up to a maximum of 100); but,
3. For technical reasons, not every endpoint behaves the same. For example,
[events](/rest/reference/activity#events) won't let you set a maximum for items to receive.
Be sure to read the documentation on how to handle paginated results for specific endpoints.

Information about pagination is provided in [the Link header](https://datatracker.ietf.org/doc/html/rfc5988)
of an API call. For example, let's make a curl request to the search API, to find
out how many times Mozilla projects use the phrase `addClass`:

```shell
$ curl -I "https://api.github.com/search/code?q=addClass+user:mozilla"
```

The `-I` parameter indicates that we only care about the headers, not the actual
content. In examining the result, you'll notice some information in the Link header
that looks like this:

    Link: <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=2>; rel="next",
      <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=34>; rel="last"

Let's break that down. `rel="next"` says that the next page is `page=2`. This makes
sense, since by default, all paginated queries start at page `1.` `rel="last"`
provides some more information, stating that the last page of results is on page `34`.
Thus, we have 33 more pages of information about `addClass` that we can consume.
Nice!

**Always** rely on these link relations provided to you. Don't try to guess or construct your own URL.

### Navigating through the pages

Now that you know how many pages there are to receive, you can start navigating
through the pages to consume the results. You do this by passing in a `page`
parameter. By default, `page` always starts at `1`. Let's jump ahead to page 14
and see what happens:

```shell
$ curl -I "https://api.github.com/search/code?q=addClass+user:mozilla&page=14"
```

Here's the link header once more:

    Link: <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=15>; rel="next",
      <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=34>; rel="last",
      <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=1>; rel="first",
      <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=13>; rel="prev"

As expected, `rel="next"` is at 15, and `rel="last"` is still 34. But now we've
got some more information: `rel="first"` indicates the URL for the _first_ page,
and more importantly, `rel="prev"` lets you know the page number of the previous
page. Using this information, you could construct some UI that lets users jump
between the first, previous, next, or last list of results in an API call.

### Changing the number of items received

By passing the `per_page` parameter, you can specify how many items you want
each page to return, up to 100 items. Let's try asking for 50 items about `addClass`:

```shell
$ curl -I "https://api.github.com/search/code?q=addClass+user:mozilla&per_page=50"
```

Notice what it does to the header response:

    Link: <https://api.github.com/search/code?q=addClass+user%3Amozilla&per_page=50&page=2>; rel="next",
      <https://api.github.com/search/code?q=addClass+user%3Amozilla&per_page=50&page=20>; rel="last"

As you might have guessed, the `rel="last"` information says that the last page
is now 20. This is because we are asking for more information per page about
our results.





I am going to provide the following examples for navigation with before and after using pagination. It is important to note that the API calls I am making are custom to my organization and will need to be tweaked for your use case.


To get the link header for your organization, you need to make an API call. For this example I used:

curl -I -H "Accept: application/vnd.github+json" -H "Authorization: Bearer ghp_*****j8fq"   https://api.github.com/enterprises/advacado-corp/audit-log

This provided me with the following output:

```
HTTP/2 200 
server: GitHub.com
date: Mon, 17 Oct 2022 15:15:37 GMT
content-type: application/json; charset=utf-8
content-length: 20854
cache-control: private, max-age=60, s-maxage=60
vary: Accept, Authorization, Cookie, X-GitHub-OTP
etag: "fc5b15308c775934ca63719ff22d9fe623e7e8226235181203424347cec50130"
x-oauth-scopes: admin:enterprise, admin:gpg_key, admin:org, admin:org_hook, admin:public_key, admin:repo_hook, admin:ssh_signing_key, codespace, delete:packages, delete_repo, gist, notifications, project, repo, user, workflow, write:discussion, write:packages
x-accepted-oauth-scopes: admin:enterprise
github-authentication-token-expiration: 2022-12-28 16:47:19 UTC
x-github-media-type: github.v3; format=json
link: <https://api.github.com/enterprises/13827/audit-log?after=MS42NjQzODM5MTkzNDdlKzEyfDM0MkI6NDdBNDo4RTFGMEM6NUIyQkZCMzo2MzM0N0JBRg%3D%3D&before=>; rel="next"
x-github-api-version-selected: 2022-08-09
x-ratelimit-limit: 5000
x-ratelimit-remaining: 4998
x-ratelimit-reset: 1666023299
x-ratelimit-used: 2
x-ratelimit-resource: core
access-control-expose-headers: ETag, Link, Location, Retry-After, X-GitHub-OTP, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Used, X-RateLimit-Resource, X-RateLimit-Reset, X-OAuth-Scopes, X-Accepted-OAuth-Scopes, X-Poll-Interval, X-GitHub-Media-Type, X-GitHub-SSO, X-GitHub-Request-Id, Deprecation, Sunset
access-control-allow-origin: *
strict-transport-security: max-age=31536000; includeSubdomains; preload
x-frame-options: deny
x-content-type-options: nosniff
x-xss-protection: 0
referrer-policy: origin-when-cross-origin, strict-origin-when-cross-origin
content-security-policy: default-src 'none'
vary: Accept-Encoding, Accept, X-Requested-With
x-github-request-id: C985:4933:1054293:2175681:634D7198
```

The important part of the output here is the link this link needs to be generated rather than manually imputed. Copy the entire link into the following output and specify the number of pages, in this example I chose 50:

curl -i -H "Accept: application/vnd.github+json" -H "Authorization: Bearer ghp_*****j8fq" https://api.github.com/enterprises/13827/audit-log?after=MS42NjQzODM5MTkzNDdlKzEyfDM0MkI6NDdBNDo4RTFGMEM6NUIyQkZCMzo2MzM0N0JBRg%3D%3D&before=>&per_page=50

This will generate a page with 50 items, but it will also generate a new link in the header that creates the next page of information for you to parse through.

link: <https://api.github.com/enterprises/13827/audit-log?after=MS42NjQzODMzMzk2MzZlKzEyfFdxSzIxdGU0MlBWNUp5UzhBWDF6LWc%3D&before=>; rel="next", <https://api.github.com/enterprises/13827/audit-log?after=&before=>; rel="first", <https://api.github.com/enterprises/13827/audit-log?after=&before=MS42NjQzODM5MTcyMjllKzEyfDI4NDE6NEVFNDoxODBDRkM5OjY5REE0MzI6NjMzNDdCQUQ%3D>; rel="prev"
