---
title: "PushManager: unregister() method"
short-title: unregister()
slug: Web/API/PushManager/unregister
page-type: web-api-instance-method
status:
  - deprecated
  - non-standard
browser-compat: api.PushManager.unregister
---

{{ApiRef("Push API")}}{{deprecated_header}}{{non-standard_header}}{{AvailableInWorkers}}

The **`unregister()`** method was used to ask the system to
unregister and delete the specified endpoint.

> [!NOTE]
> In the updated API, a subscription can be unregistered via the {{domxref("PushSubscription.unsubscribe()")}} method.

## Syntax

```js-nolint
unregister(pushEndpoint)
```

### Parameters

- `pushEndpoint`
  - : A pushEndpoint to be unregistered.

### Return value

A `DOMRequest` object to handle the success or failure of the method call.

If the method call is successful, the request's `result` will be a
[PushRegistration](#pushregistration) object representing the endpoint that has been
unregistered.

### PushRegistration

Those objects are anonymous JavaScript objects with the following properties:

- `pushEndpoint`
  - : A string representing the URL of the unregistered endpoint.
- `version`
  - : `Undefined` when `unregister.onsuccess` is called.

## Examples

```js
const req = navigator.push.unregister(pushEndpoint);

req.onsuccess = (e) => {
  const endpoint = req.result;
  console.log(`Unregistered endpoint: ${endpoint}`);
};

req.onerror = (e) => {
  console.error(`Error unregistering the endpoint: ${e.error}`);
};
```

## Specifications

This feature is not part of any specification anymore. It is no longer on track to become a standard.

## Browser compatibility

{{Compat}}

## See also

- {{domxref("PushManager")}}
