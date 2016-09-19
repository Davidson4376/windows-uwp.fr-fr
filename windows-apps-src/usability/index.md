---
description: Learn how to make your app inclusive and accessible to people around the world.
keywords: uwp app accessibility, globalization, design inclusive apps, accessibility app requirements
title: Usability in UWP apps - Windows app development
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 589e3290a47a9245ddb5e43f64d13bae1244ac8b
ms.openlocfilehash: d7246bc898e36155996941875b6c00da7891809d

---
# Usability for UWP apps

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

It’s the little touches, an extra attention to detail, that can transform a good user experience into a truly inclusive user experience that meets the needs of users around the globe.

The design and coding instructions in this section can make your UWP app more inclusive by adding accessibility features, enabling globalization and localization, enabling users to customize their experience, and providing help when users need it.


## Accessiblity

Accessibility is about making your app usable by people who have limitations that prevent or impede the use of conventional user interfaces. For some situations, accessibility requirements are imposed by law. However, it's a good idea to address accessibility issues regardless of legal requirements so that your apps have the largest possible audience.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Accessibility overview](../accessibility/accessibility-overview.md)</b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Designing inclusive software](../accessibility/designing-inclusive-software.md)</b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Developing inclusive Windows apps](../accessibility/developing-inclusive-windows-apps.md)</b><br/> This article is a roadmap for developing accessible UWP apps.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Accessibility testing](../accessibility/accessibility-testing.md) </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Accessibility in the Store](../accessibility/accessibility-in-the-store.md)</b><br/>Describes the requirements for declaring your UWP app as accessible in the Windows Store.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Accessibility checklist](../accessibility/accessibility-checklist.md)</b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Expose basic accessibility information](../accessibility/basic-accessibility-information.md)</b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Keyboard accessibility](../accessibility/keyboard-accessibility.md)</b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[High-contrast themes](../accessibility/high-contrast-themes.md)</b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Accessible text requirements](../accessibility/accessible-text-requirements.md)</b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Accessibility practices to avoid](../accessibility/practices-to-avoid.md)</b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Custom automation peers](../accessibility/custom-automation-peers.md)</b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Control patterns and interfaces](../accessibility/control-patterns-and-interfaces.md)</b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b>   
</p>
  </div>
</div>
</div>



## Globalization and localization

Windows is used worldwide, by audiences that vary in culture, region, and language. A user may speak any language, or even multiple languages. A user may be located anywhere in the world, and may speak any language in any location. You can increase the potential market for your app by designing it to be readily adaptable using globalization and localization.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Do's and don'ts](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>Follow these best practices when globalizing your apps for a wider audience and when localizing your apps for a specific market.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Use global-ready formats](../globalizing/use-global-ready-formats.md)</b><br/>Develop a global-ready app by appropriately formatting dates, times, numbers, and currencies.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Manage language and region](../globalizing/manage-language-and-region.md)</b><br/>Control how Windows selects UI resources and formats the UI elements of the app, by using the various language and region settings provided by Windows.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Use patterns to format dates and times](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>Use the [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API with custom patterns to display dates and times in exactly the format you wish.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Adjust layout and fonts, and support RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>Develop your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Prepare your app for localization](../globalizing/prepare-your-app-for-localization.md)</b><br/>Prepare your app for localization to other markets, languages, or regions.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Put UI strings into resources](../globalizing/put-ui-strings-into-resources.md)</b><br/>Put string resources for your UI into resource files. You can then reference those strings from your code or markup.</p>
  </div>
  <div class="side-by-side-content-right">
<b></b>   
<p></p>
  </div>
</div>
</div>


## App settings

App settings let you the user customize your app, optimizing it for their individual needs and preferences. Providing the right settings and storing them properly can make a great user experience even better.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Guidelines](../app-settings/guidelines-for-app-settings.md)</b><br/>Best practices for creating and displaying app settings.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Store and retrieve app data](../app-settings/store-and-retrieve-app-data.md)</b><br/>How to store and retrieve local, roaming, and temporary app data.</p>
  </div>
</div>
</div>

## In-app help
No matter how well you’ve designed your app, some users will need a little extra help.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Guidelines for app help](../in-app-help/guidelines-for-app-help.md)</b><br/>Applications can be complex, and providing effective help for your users can greatly improve their experience.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Instructional UI](../in-app-help/instructional-ui.md)</b><br/>Sometimes it can be helpful to teach the user about functions in your app that might not be obvious to them, such as specific touch interactions. In these cases, you need to present instructions to the user through the UI so they can discover and use features they might have missed.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[In-app help](../in-app-help/in-app-help.md)</b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[External help](../in-app-help/external-help.md)</b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
  </div>
</div>
</div>



<!--HONumber=Aug16_HO5-->


