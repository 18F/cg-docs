---
title: Custom Headers
permalink: /pages/documentation/custom-headers/
layout: docs
navigation: pages
sidenav: pages-documentation
---

 ⚠️ **This feature is currently experimental, make sure you know what you are doing!** ⚠️

You can configure custom headers for your site by adding information under the `headers` key in the [Federalist configuration file](/documentation/federalist-json).

## Supported Custom Headers
- `Cache-Control`

## Syntax

The `headers` key in [Federalist configuration file](/documentation/federalist-json) expects a list of configurations that include a path pattern to match against the paths of individual files in your site and the headers to apply. Ex.
```
// federalist.json
{
  ...
  "headers": [
    {
      "/*.html": {
        "cache-control": "no-cache"
      },
      "/*": {
        "cache-control": "max-age=60"
      }
    },
    ...
  ]
  ...
}
```

In the above example `"/*.html"` is a *path pattern*, `"cache-control"` is a *header name*, and `"no-cache"` is a *header value*.

### Path Pattern Matching Rules
Paths are matched in the order in which they are specified, from top to bottom. The headers for the first matching pattern are the *only* headers applied to the file. Paths should always begin with a `"/"` and are evaluated from the root of your site, preview url paths are accounted for. The path pattern is evaluated against the actual path and filename, so even if the file `"/foo/index.html"` is available at the url path `"/foo"`, the pattern must match `"/foo/index.html"`.

We currently support:
- explicit matching: `/index.html`
- wildcard matching: `/*`, `/*.html`
- segment wildcard matching: `/:foo/index.html`
- any combination of the above: `/:foo/bar/*.html`

#### Explicit Matches
Explicit matches must **exactly** match the path and filename.

| Pattern | Path | Match |
| ------- | ---- | :---: |
| `/index.html` | `/index.html` | ✅ |
| `/foo/app.js` | `/foo/app.js` | ✅ |
| `/foo` | `/foo/app.js` | ❌ |

#### Wildcard (*) Matches
The wildcard `"*"` matches everything **including** `"/"`. If the `"*"` is followed by a file extension (Ex. `".html"`), then that extension is match against the **last** extension of the file. The `"*"` must be the first character in the pattern segment, otherwise it will be treated as a normal character.

| Pattern | Path | Match |
| ------- | ---- | :---: |
| `/*` | `/index.html` | ✅ |
| `/*` | `/foo/bar/app.js` | ✅ |
| `/*.js` | `/foo/bar/app.js` | ✅ |
| `/*.js` | `/foo/bar/app.html` | ❌ |
| `/*.js` | `/foo/bar/app.abc123.js` | ✅ |
| `/foo/*.js` | `/foo/bar/app.abc123.js` | ✅ |
| `/bar/*.js` | `/foo/bar/app.abc123.js` | ❌ |
| `/f*.js` | `/foo.js` | ❌ |
| `/f*.js` | `/f*.js` | ✅ |

#### Segment Wildcard (:) Matches
The segment wildcard `":foo"` (where "foo" can be anything) matches everything **except** `"/"`. The `":"` must be the first character in the pattern segment, otherwise it will be treated as a normal character.

| Pattern | Path | Match |
| ------- | ---- | :---: |
| `/:foo` | `/index.html` | ✅ |
| `/:bar` | `/foo/index.html` | ❌ |
| `/:bar/*.html` | `/foo/index.html` | ✅ |
| `/f:bar` | `/foo` | ❌ |
| `/f:bar` | `/f:bar` | ✅ |

## Examples
- [federalist-uswds-gatsby2](https://github.com/18F/federalist-uswds-gatsby2/blob/master/federalist.json)

## Caching Headers
Adding caching related headers to your site is considered a best practice and can greatly improve your site's performance. However, if done incorrectly, it is possible to create undesired outcomes such as users not seeing the latest version of your site. **Make sure you have a good understanding of how caching works before configuring these headers yourself.** See [MDN Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) and [MDN Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) for more information.

### Defaults
The current default is for all files to be cached for 1 minute:

```
cache-control: max-age=60
```

### Caching Rules of Thumb
While the configuration that makes sense will depend on your particular site; here are some rules of thumb.

Images, Javascript, and CSS files are good candidates for long-term caching as they can be large AND their urls/filenames are not typically known to the users.

1. Make sure your build system generates unique filenames for the assets you want to cache for a longer period. For example, the filenames should look something like `app-87648563467.js` or `styles.4574395739578.css`.
2. Configure cache-control to cache for 1 year:
```
public, max-age=31536000, immutable
```

Html files are typically NOT good candidates for long-term caching since the url depends on the actual filename and they are typically smaller files. Given your need for how quickly you want users to get your latest changes, you can either:
- Configure cache-control to never cache:
```
public, max-age=0, must-revalidate
```

- Configure cache-control to cache for a short duration (ie 5 minutes):
```
public, max-age=300 must-revalidate
```
