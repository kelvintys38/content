---
title: runtime.lastError
slug: Mozilla/Add-ons/WebExtensions/API/runtime/lastError
page-type: webextension-api-property
browser-compat: webextensions.api.runtime.lastError
sidebar: addonsidebar
---

This value is used to report an error message from an asynchronous API, when the asynchronous API is given a callback. This is useful for extensions that are using the callback-based version of the WebExtension APIs.

You don't need to check this property if you are using the promise-based version of the APIs: instead, pass an error handler to the promise:

```js
const gettingCookies = browser.cookies.getAll();
gettingCookies.then(onGot, onError);
```

The `runtime.lastError` property is set when an asynchronous function has an error condition that it needs to report to its caller.

If you call an asynchronous function that may set `lastError`, you are expected to check for the error when you handle the result of the function. If `lastError` has been set and you don't check it within the callback function, then an error will be raised.

## Syntax

```js-nolint
let myError = browser.runtime.lastError;  // null or Error object
```

### Value

An {{jsxref("Error")}} object representing the error. The {{jsxref("Error.message", "message")}} property is a `string` with a human-readable description of the error. If `lastError` has not been set, the value is `null`.

## Examples

Set a cookie, using a callback to log the new cookie or report an error:

```js
function logCookie(c) {
  if (browser.runtime.lastError) {
    console.error(browser.runtime.lastError);
  } else {
    console.log(c);
  }
}

browser.cookies.set({ url: "https://developer.mozilla.org/" }, logCookie);
```

The same, but using a promise to handle the result of `setCookie()`:

```js
function logCookie(c) {
  console.log(c);
}

function logError(e) {
  console.error(e);
}

const setCookie = browser.cookies.set({
  url: "https://developer.mozilla.org/",
});

setCookie.then(logCookie, logError);
```

> [!NOTE]
> `runtime.lastError` is an alias for {{WebExtAPIRef("extension.lastError")}}. They are set together, and checking either one will work.

{{WebExtExamples}}

## Browser compatibility

{{Compat}}

> [!NOTE]
> This API is based on Chromium's [`chrome.runtime`](https://developer.chrome.com/docs/extensions/reference/api/runtime#property-lastError) API. This documentation is derived from [`runtime.json`](https://chromium.googlesource.com/chromium/src/+/master/extensions/common/api/runtime.json) in the Chromium code.

<!--
// Copyright 2015 The Chromium Authors. All rights reserved.
//
// Redistribution and use in source and binary forms, with or without
// modification, are permitted provided that the following conditions are
// met:
//
//    * Redistributions of source code must retain the above copyright
// notice, this list of conditions and the following disclaimer.
//    * Redistributions in binary form must reproduce the above
// copyright notice, this list of conditions and the following disclaimer
// in the documentation and/or other materials provided with the
// distribution.
//    * Neither the name of Google Inc. nor the names of its
// contributors may be used to endorse or promote products derived from
// this software without specific prior written permission.
//
// THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
// "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
// LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
// A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
// OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
// SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
// LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
// DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
// THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
// (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
// OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
-->
