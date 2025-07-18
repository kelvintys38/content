---
title: notifications.onClosed
slug: Mozilla/Add-ons/WebExtensions/API/notifications/onClosed
page-type: webextension-api-event
browser-compat: webextensions.api.notifications.onClosed
sidebar: addonsidebar
---

Fired when a notification is closed, either by the system or by the user.

## Syntax

```js-nolint
browser.notifications.onClosed.addListener(listener)
browser.notifications.onClosed.removeListener(listener)
browser.notifications.onClosed.hasListener(listener)
```

Events have three functions:

- `addListener(listener)`
  - : Adds a listener to this event.
- `removeListener(listener)`
  - : Stop listening to this event. The `listener` argument is the listener to remove.
- `hasListener(listener)`
  - : Check whether `listener` is registered for this event. Returns `true` if it is listening, `false` otherwise.

## addListener syntax

### Parameters

- `listener`
  - : The function called when this event occurs. The function is passed these arguments:
    - `notificationId`
      - : `string`. ID of the notification that closed.
    - `byUser`
      - : `boolean`. `true` if the notification was closed by the user, or `false` if it was closed by the system. This argument is not supported in Firefox.

## Examples

In this simple example we add a listener to the `notifications.onClosed` event to listen for system notifications being closed. When this occurs, we log an appropriate message to the console.

```js
browser.notifications.onClosed.addListener((notificationId) => {
  console.log(`Notification ${notificationId} has closed.`);
});
```

{{WebExtExamples}}

## Browser compatibility

{{Compat}}

> [!NOTE]
> This API is based on Chromium's [`chrome.notifications`](https://developer.chrome.com/docs/extensions/reference/api/notifications) API.
