---
title: Notable bugs fixed in Firefox 3
slug: Mozilla/Firefox/Releases/3/Notable_bugs_fixed
page-type: guide
sidebar: firefox
---

This article offers a list of important bug fixes offered by Firefox 3 that are not necessarily obvious in the documentation.

- if an error occurs parsing an overlay, the overlay is not applied. Parse errors are logged to the error console. ([Firefox bug 335755](https://bugzil.la/335755))
- bug fixed where `<menupopup>` elements can be placed inside a binding when attached to a menu or menu-like element. ([Firefox bug 345896](https://bugzil.la/345896))
- the button's `dlgType` property now works properly. ([Firefox bug 308591](https://bugzil.la/308591))
- the `canBubble` argument to {{ Domxref("event.initEvent") }} now works properly so that events can be fired which don't bubble. ([Firefox bug 330190](https://bugzil.la/330190))
- the `DOMAttrModified` event now handles namespaced attributes properly. ([Firefox bug 362391](https://bugzil.la/362391))
- XML processing instructions, such as `<?xml-stylesheet ?>`, are now added to a XUL document's DOM. This means {{ Domxref("Node.firstChild", "document.firstChild") }} isn't guaranteed to be the root element, use {{ Domxref("document.documentElement") }} instead. Also, `<?xml-stylesheet ?>` and `<?xul-overlay ?>` processing instructions now have no effect outside the document prolog. ([Firefox bug 319654](https://bugzil.la/319654))
- [`getElementsByAttributeNS()`](https://web.archive.org/web/20201210015651/https://developer.mozilla.org/en-US/docs/Archive/Mozilla/XUL/Method/getElementsByAttributeNS) functions have been added to XUL elements and documents. ([Firefox bug 239976](https://bugzil.la/239976))
- event listeners are maintained when moving or removing an element from a XUL document. ([Firefox bug 286619](https://bugzil.la/286619))
- mutation events are now fired for non-displayed documents. ([Firefox bug 201236](https://bugzil.la/201236))
- various issues with elements drawing in the wrong order are fixed. ([Firefox bug 317375](https://bugzil.la/317375))
- [`getElementsByTagName()`](/en-US/docs/Web/API/Element/getElementsByTagName) has been fixed to work correctly on subtrees that have elements with namespace prefixes in their tag names ([Firefox bug 206053](https://bugzil.la/206053)).
- The `DOMNodeInserted` and `DOMNodeRemoved` events now properly apply to the correct nodes ([Firefox bug 367164](https://bugzil.la/367164)).
- `\d`, one of special characters in regular expressions, has been fixed to match only Basic Latin alphabet digits (equivalent to `[0-9]`). ([Firefox bug 378738](https://bugzil.la/378738))
- The image-sniffing-services category allows for image decoders implemented as extensions to correctly decode images sent with incorrect mime-types. ([Firefox bug 391667](https://bugzil.la/391667))
- Right-clicks on form controls no longer brings up a context menu by default ([Firefox bug 404536](https://bugzil.la/404536). See [Offering a context menu for form controls](https://web.archive.org/web/20210612231358/https://developer.mozilla.org/en-US/docs/Archive/Add-ons/Offering_a_context_menu_for_form_controls) to learn how to enable this on a case-by-case basis.

### See also

- [Firefox 3 for developers](/en-US/docs/Mozilla/Firefox/Releases/3)
