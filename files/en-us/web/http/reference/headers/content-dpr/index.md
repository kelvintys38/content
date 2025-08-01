---
title: Content-DPR header
short-title: Content-DPR
slug: Web/HTTP/Reference/Headers/Content-DPR
page-type: http-header
status:
  - deprecated
  - non-standard
browser-compat: http.headers.Content-DPR
sidebar: http
---

{{deprecated_header}}{{securecontext_header}}{{Non-standard_header}}

The HTTP **`Content-DPR`** {{Glossary("response header")}} is used to confirm the _image_ device to pixel ratio (DPR) in requests where the screen {{HTTPHeader("DPR")}} client hint was used to select an image resource.

> [!NOTE]
> The `Content-DPR` header was removed from the client hints specification in [draft-ietf-httpbis-client-hints-07](https://datatracker.ietf.org/doc/html/draft-ietf-httpbis-client-hints-07).
> The [Responsive Image Client Hints](https://wicg.github.io/responsive-image-client-hints/) specification proposes to replace this header by specifying intrinsic resolution/dimensions in EXIF metadata.

If the `DPR` client hint is used to select an image, the server must specify `Content-DPR` in the response.
If the value in `Content-DPR` is different from the {{HTTPHeader("DPR")}} value in the request (i.e., image DPR is not the same as screen DPR), the client must use the `Content-DPR` for determining intrinsic image size and scaling the image.

If the `Content-DPR` header appears more than once in a message, the last occurrence is used.

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">Header type</th>
      <td>
        {{Glossary("Response header")}},
        <a href="/en-US/docs/Web/HTTP/Guides/Client_hints">Client hint</a>
      </td>
    </tr>
    <tr>
      <th scope="row">{{Glossary("Forbidden request header")}}</th>
      <td>No</td>
    </tr>
    <tr>
      <th scope="row">
        {{Glossary("CORS-safelisted response header")}}
      </th>
      <td>No</td>
    </tr>
  </tbody>
</table>

## Syntax

```http
Content-DPR: <number>
```

## Directives

- `<number>`
  - : The image device pixel ratio, calculated according to the following formula:
    Content-DPR = _Selected image resource size_ / (_Width_ / _DPR_)

## Examples

See the [`DPR`](/en-US/docs/Web/HTTP/Reference/Headers/DPR#examples) header example.

## Browser compatibility

{{Compat}}

## See also

- Device client hints
  - {{HTTPHeader("Device-Memory")}}
  - {{HTTPHeader("DPR")}}
  - {{HTTPHeader("Viewport-Width")}}
  - {{HTTPHeader("Width")}}
- {{HTTPHeader("Accept-CH")}}
- [HTTP Caching: Vary](/en-US/docs/Web/HTTP/Guides/Caching#vary) and {{HTTPHeader("Vary")}}
- [Improving user privacy and developer experience with User-Agent Client Hints](https://developer.chrome.com/docs/privacy-security/user-agent-client-hints) on developer.chrome.com (2020)
