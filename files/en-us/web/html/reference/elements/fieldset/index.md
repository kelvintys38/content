---
title: "<fieldset>: The Field Set element"
slug: Web/HTML/Reference/Elements/fieldset
page-type: html-element
browser-compat: html.elements.fieldset
sidebar: htmlsidebar
---

The **`<fieldset>`** [HTML](/en-US/docs/Web/HTML) element is used to group several controls as well as labels ({{HTMLElement("label")}}) within a web form.

{{InteractiveExample("HTML Demo: &lt;fieldset&gt;", "tabbed-standard")}}

```html interactive-example
<form>
  <fieldset>
    <legend>Choose your favorite monster</legend>

    <input type="radio" id="kraken" name="monster" value="K" />
    <label for="kraken">Kraken</label><br />

    <input type="radio" id="sasquatch" name="monster" value="S" />
    <label for="sasquatch">Sasquatch</label><br />

    <input type="radio" id="mothman" name="monster" value="M" />
    <label for="mothman">Mothman</label>
  </fieldset>
</form>
```

```css interactive-example
legend {
  background-color: #000;
  color: #fff;
  padding: 3px 6px;
}

input {
  margin: 0.4rem;
}
```

As the example above shows, the `<fieldset>` element provides a grouping for a part of an HTML form, with a nested {{htmlelement("legend")}} element providing a caption for the `<fieldset>`. It takes few attributes, the most notable of which are `form`, which can contain the `id` of a {{htmlelement("form")}} on the same page, allowing you to make the `<fieldset>` part of that `<form>` even if it is not nested inside it, and `disabled`, which allows you to disable the `<fieldset>` and all its contents in one go.

## Attributes

This element includes the [global attributes](/en-US/docs/Web/HTML/Reference/Global_attributes).

- [`disabled`](/en-US/docs/Web/HTML/Reference/Attributes/disabled)
  - : If this Boolean attribute is set, all form controls that are descendants of the `<fieldset>`, are disabled, meaning they are not editable and won't be submitted along with the {{htmlelement("form")}}. They won't receive any browsing events, like mouse clicks or focus-related events. By default browsers display such controls grayed out. Note that form elements inside the {{HTMLElement("legend")}} element won't be disabled.
- `form`
  - : This attribute takes the value of the [`id`](/en-US/docs/Web/HTML/Reference/Global_attributes/id) attribute of a {{HTMLElement("form")}} element you want the `<fieldset>` to be part of, even if it is not inside the form. Please note that usage of this is confusing — if you want the {{HTMLElement("input")}} elements inside the `<fieldset>` to be associated with the form, you need to use the `form` attribute directly on those elements. You can check which elements are associated with a form via JavaScript, using {{domxref("HTMLFormElement.elements")}}.
- `name`
  - : The name associated with the group.

    > [!NOTE]
    > The caption for the fieldset is given by the first {{HTMLElement("legend")}} element nested inside it.

## Styling with CSS

There are several special styling considerations for `<fieldset>`.

Its {{cssxref("display")}} value is `block` by default, and it establishes a [block formatting context](/en-US/docs/Web/CSS/CSS_display/Block_formatting_context). If the `<fieldset>` is styled with an inline-level `display` value, it will behave as `inline-block`, otherwise it will behave as `block`. By default there is a `2px` `groove` border surrounding the contents, and a small amount of default padding. The element has {{cssxref("min-inline-size", "min-inline-size: min-content")}} by default.

If a {{htmlelement("legend")}} is present, it is placed over the `block-start` border. The `<legend>` shrink-wraps, and also establishes a formatting context. The `display` value is blockified. (For example, `display: inline` behaves as `block`.)

There will be an anonymous box holding the contents of the `<fieldset>`, which inherits certain properties from the `<fieldset>`. If the `<fieldset>` is styled with `display: grid` or `display: inline-grid`, then the anonymous box will be a grid formatting context. If the `<fieldset>` is styled with `display: flex` or `display: inline-flex`, then the anonymous box will be a flex formatting context. Otherwise, it establishes a block formatting context.

You can feel free to style the `<fieldset>` and `<legend>` in any way you want to suit your page design.

## Examples

### Basic fieldset

This example includes a `<fieldset>` with a `<legend>`, with a single control inside it.

```html
<form action="#">
  <fieldset>
    <legend>Do you agree?</legend>
    <input type="checkbox" id="chbx" name="agree" value="Yes!" />
    <label for="chbx">I agree</label>
  </fieldset>
</form>
```

#### Result

{{ EmbedLiveSample('Basic_fieldset', '100%', '80') }}

### Disabled fieldset

This example shows a disabled `<fieldset>` with two controls inside it. Note how both the controls are disabled due to being inside a disabled `<fieldset>`.

```html
<form action="#">
  <fieldset disabled>
    <legend>Disabled login fieldset</legend>
    <div>
      <label for="name">Name: </label>
      <input type="text" id="name" value="Chris" />
    </div>
    <div>
      <label for="pwd">Archetype: </label>
      <input type="password" id="pwd" value="Wookie" />
    </div>
  </fieldset>
</form>
```

#### Result

{{ EmbedLiveSample('Disabled_fieldset', '100%', '110') }}

## Technical summary

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">
        <a href="/en-US/docs/Web/HTML/Guides/Content_categories"
          >Content categories</a
        >
      </th>
      <td>
        <a href="/en-US/docs/Web/HTML/Guides/Content_categories#flow_content"
          >Flow content</a
        >,
        sectioning root,
        <a href="/en-US/docs/Web/HTML/Guides/Content_categories#listed"
          >listed</a
        >,
        <a
          href="/en-US/docs/Web/HTML/Guides/Content_categories#form-associated_content"
          >form-associated</a
        >
        element, palpable content.
      </td>
    </tr>
    <tr>
      <th scope="row">Permitted content</th>
      <td>
        An optional {{HTMLElement("legend")}} element, followed by flow
        content.
      </td>
    </tr>
    <tr>
      <th scope="row">Tag omission</th>
      <td>None, both the starting and ending tag are mandatory.</td>
    </tr>
    <tr>
      <th scope="row">Permitted parents</th>
      <td>
        Any element that accepts
        <a href="/en-US/docs/Web/HTML/Guides/Content_categories#flow_content"
          >flow content</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Implicit ARIA role</th>
      <td><a href="/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/group_role"><code>group</code></a></td>
    </tr>
    <tr>
      <th scope="row">Permitted ARIA roles</th>
      <td>
        <a href="/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/radiogroup_role"><code>radiogroup</code></a>,
        <a href="/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role"><code>presentation</code></a>, <a href="/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/none_role"><code>none</code></a>
      </td>
    </tr>
    <tr>
      <th scope="row">DOM interface</th>
      <td>{{domxref("HTMLFieldSetElement")}}</td>
    </tr>
  </tbody>
</table>

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- The {{HTMLElement("legend")}} element
- The {{HTMLElement("input")}} element
- The {{HTMLElement("label")}} element
- The {{HTMLElement("form")}} element
