# [misdirection](https://packagist.org/packages/nglasl/silverstripe-misdirection)

_The current release is **3.1.1**_

> This module allows both simple and regular expression link redirections based on customisable mappings, either hooking into a page not found or replacing the default automated URL handling.

## Requirement

* SilverStripe 3.1 → **4.0**

This module does **not** require the CMS.

**This repository is no longer supported, however this module is still supported [here](https://github.com/symbiote/silverstripe-misdirection).**

## Getting Started

* [Place the module under your root project directory.](https://packagist.org/packages/nglasl/silverstripe-misdirection)
* `/dev/build`
* Select `Misdirection` through the CMS.
* Create a link mapping.
* `/mapping`

## Overview

### Link Mappings

These allow both simple and regular expression link redirections, and can be used for legacy page redirection, vanity URLs (more below), or redirection based on specific URL patterns.

![link-mapping](https://raw.githubusercontent.com/nglasl/silverstripe-misdirection/master/client/images/misdirection-link-mapping.png)

The link mapping with the highest priority (followed by greatest specificity) will be used, and replaces the default automated URL handling out of the box. This default behaviour may be configured to only hook into a page not found:

```yaml
nglasl\misdirection\MisdirectionRequestFilter:
  enforce_misdirection: false
```

When there are multiple matches, the link mapping first created will be used. This default behaviour may be configured to prioritise the link mapping most recently created:

```yaml
nglasl\misdirection\LinkMapping:
  priority: 'DESC'
```

### Vanity URLs and Fallbacks

![vanity-URLs-and-fallbacks](https://raw.githubusercontent.com/nglasl/silverstripe-misdirection/master/client/images/misdirection-vanity-URLs-and-fallbacks.png)

#### Vanity URLs

While it is possible to create these manually (as above), a content author may directly create a link mapping from a page. However, it should be noted that these are instantiated with a low priority of `2`, and therefore other link mappings with higher priority will take precedence.

#### Fallbacks

When a user happens to encounter a page not found, a specified rule may be triggered to prevent displaying this. It is possible for an administrator to configure a global fallback through the site configuration, however a specific page setting will take precedence.

* Select `Settings`
* Select `Pages`

### What's Happening?

The link mappings are processed server side to prevent inefficient and problematic mappings, using a request filter.

When you want to see exactly what is happening behind the scenes for a given URL, the model admin provides a powerful testing interface!

![testing](https://raw.githubusercontent.com/nglasl/silverstripe-misdirection/master/client/images/misdirection-testing.png)

Once a maximum number of requests has been reached, the server will respond with a page not found. The following is the default configuration:

```yaml
nglasl\misdirection\MisdirectionRequestFilter:
  maximum_requests: 9
```

![testing-maximum-requests](https://raw.githubusercontent.com/nglasl/silverstripe-misdirection/master/client/images/misdirection-testing-maximum-requests.png)

#### Bypassing Misdirection

It is possible to bypass the request filter completely by appending `?misdirected=1` to the URL. This is fantastic for debugging, however does not apply to the testing interface for obvious reasons.

### Default Automated URL Handling

This may be completely replaced, in which case legacy URLs will no longer resolve based on page version history.

```yaml
nglasl\misdirection\MisdirectionRequestFilter:
  replace_default: true
```

When a page is moved, the appropriate link mappings are automatically created and maintained. This allows full control over which legacy URLs remain in the system.

## Maintainer Contact

	Nathan Glasl, nathan@symbiote.com.au
