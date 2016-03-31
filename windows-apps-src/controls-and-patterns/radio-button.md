---
Radio buttons let users select one option from two or more choices.
Guidelines for radio buttons
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
Radio buttons
template: detail.hbs
---
# Radio buttons
Radio buttons let users select one option from two or more choices. Each option is represented by one radio button; a user can select only one radio button in a radio button group.

(If you're curious about the name, radio buttons are named for the channel preset buttons on a radio.)

![Radio buttons](images/ws_radio_buttons.png)

<span class="sidebar_heading" style="font-weight: bold;">Important APIs</span>

-   [**RadioButton class (XAML)**](https://msdn.microsoft.com/library/windows/apps/br227544)


## Example

![A screenshot that illustrates the standard radio button control](images/RadioButton_Standard.png)

## Is this the right control?

Use radio buttons to present users with two or more mutually exclusive options, as here.

![A group of radio buttons](images/radiobutton_basic.png)
Radio buttons add clarity and weight to very important options in your app. Use radio buttons when the options being presented are important enough to command more screen space and where the clarity of the choice demands very explicit options.

Radio buttons emphasize all options equally, and that may draw more attention to the options than necessary. Consider using other controls, unless the options deserve extra attention from the user. For example, if the default option is recommended for most users in most situations, use a [drop-down list](lists.md) instead.

If there are only two mutually exclusive options, combine them into a single [checkbox](checkbox.md) or [toggle switch](toggles.md). For example, use a checkbox for "I agree" instead of two radio buttons for "I agree" and "I don't agree."

![Two ways of presenting a binary choice](images/radiobutton_vs_checkbox.png)
When the user can select multiple options, use a [checkbox](checkbox.md) or [list box](lists.md) control instead.

![Selecting multiple options with check boxes](images/checkbox2.png)
Don't use radio buttons when the options are numbers that have fixed steps, like 10, 20, 30. Use a [slider](slider.md) control instead.

If there are more than 8 options, use a [drop-down list](lists.md), a single-select [list box](lists.md), or a [list view](lists.md) instead.

If the available options are based on the app’s current context, or can otherwise vary dynamically, use a single-select [list box](lists.md) instead.

**Note**  A group of radio buttons behaves like a single control when accessed via the keyboard. Only the selected choice is accessible using the Tab key but users can cycle through the group using arrow keys.

## Recommendations

-   Make sure that the purpose and current state of a set of radio buttons is clear.
-   Always give visual feedback when the user taps a radio button.
-   Give visual feedback as the user interacts with radio buttons. Normal, pressed, checked, and disabled are examples of radio button states. A user taps a radio button to activate the related option. Tapping an activated option doesn’t deactivate it, but tapping another option transfers activation to that option.
-   Reserve visual effects and animations for touch feedback, and for the checked state; in the unchecked state, radio button controls should appear unused or inactive (but not disabled).
-   Limit the radio button’s text content to a single line. You can customize the radio button’s visuals to display a description of the option in smaller font size below the main line of text.
-   If the text content is dynamic, consider how the button will resize and what will happen to visuals around it.
-   Use the default font unless your brand guidelines tell you to use another.
-   Enclose the radio button in a label element so that tapping the label selects the radio button.
-   Place the label text after the radio button control, not before or above it.
-   Consider customizing your radio buttons. By default, a radio button consists of two concentric circles—the inner one filled (and shown when the radio button is checked), the outer one stroked—and some text content. But we encourage you to be creative. Users are comfortable interacting directly with the content of an app. So you may choose to show the actual content on offer, whether that’s presented with graphics or as subtle textual toggle buttons.
-   Don't put more than 8 options in a radio button group. When you need to present more options, use a [drop-down list](lists.md), [list box](lists.md), or a [list view](lists.md) instead.
-   Don't put two radio button groups next to each other. When two radio button groups are right next to each other, it's difficult to determine which buttons belong to which group. Use group labels to separate them.

## Additional usage guidance

This illustration shows the proper way to position and space radio buttons.

![A set of radio buttons](images/radiobutton_layout1.png)
## Related topics

**For designers**
* [Guidelines for buttons](buttons.md)
* [Guidelines for toggle switches](toggles.md)
* [Guidelines for checkboxes](checkbox.md)
* [Guidelines for drop-down lists](lists.md)
* [Guidelines for list view and grid view controls](lists.md)
* [Guidelines for sliders](slider.md)
* [Guidelines for the select control](lists.md)


**For developers (XAML)**
* [**Windows.UI.Xaml.Controls RadioButton class**](https://msdn.microsoft.com/library/windows/apps/br227544)
* [Adding radio buttons](https://msdn.microsoft.com/library/windows/apps/xaml/hh780620)
<!--HONumber=Mar16_HO1-->
