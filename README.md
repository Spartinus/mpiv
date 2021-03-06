A fork of [MPIV](https://greasyfork.org/en/scripts/404-mouseover-popup-image-viewer/) (Mouseover Popup Image Viewer).

### Installation

* on GreasyFork: [link](https://greasyfork.org/scripts/394820)
* directly from GitHub: [script.user.js](https://github.com/tophf/mpiv/raw/master/script.user.js) (Tampermonkey/Violentmonkey should be installed first)

### Usage

Action | Trigger
---|---
**activate** | move mouse cursor over thumbnail
**deactivate** | move cursor off thumbnail, or click, or zoom out fully
**prevent/freeze** | hold down <code>Shift</code> while entering/leaving thumbnail
**force-activate<br>(for small pics)** | hold <code>Ctrl</code> while entering image element
&nbsp; |
**start zooming** | configurable: automatic or via right-click / <code>Shift</code> while popup is visible
**zoom** | mouse wheel
**flip through album** | mouse wheel, <code>j</code> / <code>k</code> or <code>left</code> / <code>right</code> keys
&nbsp; |
**open in tab** | press <code>t</code> while popup is visible
**download** | press <code>d</code> while popup is visible
&nbsp; |
**change settings** | userscript manager toolbar icon -> User Script Commands -> `MPIV: configure`

![config UI screenshot](https://i.imgur.com/RKzEHiA.png)

### Technical notes

* Ancient browsers aren't supported because the code was refactored to the common JS norms and ES2015+ syntax
* Quite a few rules were updated/enhanced, some added, some dead hostings removed
* ShadowDOM support added for sites built with Web Components e.g. Polymer
* The internal status updates are not exposed by default on the `<html>` node because doing so slows down complex sites due to recalculation of the *entire* page layout. Instead only the hovered node (as reported by the matching rule) receives status updates on its `mpiv-status` attribute (it's not the `class` nor `data-` attribute to avoid confusing sites with unknown stuff being present in these standard places). If you were using the global status feature to customize CSS of those statuses, you'll need to enable it manually in the MPIV's config dialog.
* New rule property `"u"` (a single string or an array of strings) that performs a very fast plain-string check. Only when it succeeds, the slow regexp `"r"` is checked. Special symbols may be specified in `"u"` property to increase the reliability of matching: `||`, `|`, `^` - same syntax as in AdBlock filters, see the source code of the script for usage examples.

    * `||foo.bar/path`, here `||` means "domain or subdomain" so the pattern matches domains like `foo.bar` or `subdomain.foo.bar` and doesn't match unrelated domains partially like for example `foofoo.bar`
    * `|foo` matches things that start with foo (the entire URL is checked so that means `http` at least, usually)
    * `^` is a URL part separator (like `/` or `?` or `:`) but not a letter/number, neither any of `%._-`. Additionally, when used at the end like `foo^` it also matches when the source ends with `foo`
* New rule property `"anonymous": true` to make the requests for this rule anonymously (i.e. without sending cookies)
