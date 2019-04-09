# Resetting button styles

Button styles differ slightly between different browsers, so in order to apply custom styles to
a button, it might be preferrable to reset the buttons styles, to have a common starting point
for all browsers.

The following removes all styling from the native `<button>` element in Firefox (tested v65.0.1), Chrome (tested v73.0), macOS Safari (tested v12.0.3), iOS Safari (tested v11.3), Edge (tested v18) and Internet Explorer (tested v11).

```sass
  .button
    font-family: inherit
    font-size: 100%
    border: 0
    border-radius: none
    padding: 0
    margin: 0
    background: transparent

    &:focus
      // Make sure to re-implement a decent focus state
      outline: none
```

**For the sake of a11y, please make sure to implement a `:focus` state after removing the default styles**

A great resource on the topic, from where most of the rules are copied, is this [CSS Tricks article](https://css-tricks.com/overriding-default-button-styles/).
