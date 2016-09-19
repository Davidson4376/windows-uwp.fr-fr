---
author: QuinnRadich
Description: These guidelines describe how to design effective Help content for your app.
title: Guidelines for app help
label: Guidelines for app help
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0aa2db498ab7ec25839da259dd0026b0a7cd2b13
ms.openlocfilehash: 0ca099b0acfcba16d62c6158c7a829adaf475891

---

# Guidelines for app help

\[ Updated for Universal Windows Platform (UWP) apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Applications can be complex, and providing effective help for your users can greatly improve their experience. Not all applications need to provide help for their users, and the type of help provided can vary greatly, depending on the application and how people will use it.

If you decide to provide help, consider the guidelines outlined here. And remember, help that isn't helpful can be worse than no help at all.

## <span id="intuitive_design"></span><span id="INTUITIVE_DESIGN"></span>Intuitive design

As useful as help content can be, your app cannot rely on it to provide a good experience for the user. If someone is unable to immediately discover and use the critical functions of your app, that person won't use your app. No help content will change that first impression.

An intuitive and user-friendly design is the first step to writing useful help. Not only does it keep the user engaged long enough for them to use more advanced features, but it also provides them with the knowledge of an app's core functions, which they can build upon as they continue on.

## <span id="general_instructions"></span><span id="GENERAL_INSTRUCTIONS"></span>General instructions

A user will not look for help content unless they already have a problem, so help needs to provide a quick and effective answer to that problem. If help is not immediately useful, or if help is too complicated, then people are more likely to ignore it.

All help, no matter what kind, should follow these basic principles:

-   **Easy to understand:** Help that confuses the user is worse than no help at all.

-   **Straightforward:** Users looking for help want clear answers presented directly to them.

-   **Relevant:** Users do not want to have to search for their specific issue. They want the most relevant help presented first (this is called "contextual help"), or they want an easily navigated interface.

-   **Direct:** When a user looks for help, they want to see help. If your app includes pages for reporting bugs, giving feedback, viewing term of service, or similar functions, they should be included as links or footnotes on the main help page, and not as items of importance.

-   **Consistent:** No matter the type, help is still a part of your app, and should be treated as any other part of the UI. The same design principles of usability, accessibility, and style that are used throughout the rest of your app should also be present in the help you offer.

## <span id="types_of_help"></span><span id="TYPES_OF_HELP"></span>Types of help

There are three primary categories of help content, each with varying strengths and suitable for different purposes. You can use any combination of them in your app, depending on your needs.

### <span id="instructional_ui"></span><span id="INSTRUCTIONAL_UI"></span>Instructional UI

Ideally, users should be able to use all the core functions of your app without instruction, but occasionally your app may depend on the use of a specific gesture, or there may be secondary features of your app that are not immediately obvious. In these cases, instructional UI should be used to educate users with directions on how to perform specific tasks.

[See guidelines for instructional UI](instructional-ui.md)

### <span id="in_app_help"></span><span id="IN_APP_HELP"></span>In-app help

The standard method of presenting help is to display it within the application at the user's request. There are several ways in which this can be implemented, such as help pages or informative descriptions. This method is ideal for general-purpose help that directly answers a user's questions without complexity.

[See guidelines for in-app help](in-app-help.md)

### <span id="external_help"></span><span id="EXTERNAL_HELP"></span>External help

For detailed tutorials, advanced functions, or libraries of help topics too large to fit within your application, links to external web pages are ideal. These links should be used sparingly if possible, as they remove the user from the application experience.

[See guidelines for external help](external-help.md)

\[This article contains information that is specific to Universal Windows Platform (UWP) apps and Windows 10. For Windows 8.1 guidance, please download the [Windows 8.1 guidelines PDF](https://go.microsoft.com/fwlink/p/?linkid=258743).\]



<!--HONumber=Aug16_HO3-->


