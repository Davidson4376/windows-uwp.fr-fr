---
author: QuinnRadich
Description: Design an instructional UI that teaches users how to work with your Windows app.
title: Guidelines for designing instructional UI
label: Instructional UI
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 08b0b88e8ef17c2a8f264df5db4f971c8c49ab2e
ms.openlocfilehash: 6fc4840ce37d05bbaf0e423049d79b4e0c4febfc

---

# Instructional UI guidelines

\[ Updated for Universal Windows Platform (UWP) apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Sometimes it can be helpful to teach the user about functions in your app that might not be obvious to them, such as specific touch interactions. In these cases, you need to present instructions to the user through the UI so they can discover and use features they might have missed.

## <span id="when_to_use_instructional_ui"></span><span id="WHEN_TO_USE_INSTRUCTIONAL_UI"></span>When to use instructional UI

Instructional UI has to be used carefully. When overused it can be easily ignored by, or annoy, the user, making it ineffective.

Instructional UI should be used to help the user discover important and non-obvious features of your app, such as touch gestures or settings they may be interested in. It can also be used to inform users about new features or changes in your app that they might have otherwise overlooked.

Unless your app is dependent on touch gestures, instructional UI should not be used to teach users the fundamental features of your app.

## <span id="writing_instructional_ui"></span><span id="WRITING_INSTRUCTIONAL_UI"></span>Principles of writing instructional UI

Good instructional UI is relevant and educational to the user, and enhances the user experience. It should be:

-   **Simple:** Users don't want their experience to be interrupted with complicated information.
-   **Memorable:** Users don't want to see the same instructions every time they attempt a task, so instructions need to be something they'll remember.
-   **Immediately relevant:** If the instructional UI doesn't teach a user about something that they immediately want to do, they won't have a reason to pay attention to it.

Avoid overusing instructional UI, and be sure to choose the right topics. Do not teach:

-   **Fundamental features:** If a user needs instructions to use your app, consider making the app design more intuitive.
-   **Obvious features:** If a user can figure out a feature on their own without instruction, then the instructional UI will just get in the way.
-   **Complex features:** Instructional UI needs to be concise. Users interested in complex features are usually willing to seek out instructions.

Avoid inconveniencing the user with your instructional UI. Do not:

-   **Obscure important information:** Instructional UI should never get in the way of other features of your app.
-   **Force users to participate:** Users should be able to ignore instructional UI and still progress through the app.
-   **Displaying repeat information:** Don't harass the user with instructional UI, even if they ignore it the first time. Adding a setting to display instructional UI again is a better solution.

## <span id="examples_of_instructional_ui"></span><span id="EXAMPLES_OF_INSTRUCTIONAL_UI"></span>Examples of instructional UI

Here are a few examples of instructional UI that can help your users:

-   **Help users discover touch interactions.** The following screen shot shows instructional UI teaching a player how to use touch gestures in the game *Cut the Rope*.

    ![screen shot from game showing instructional ui message, "slide acress to cut the rope"](images/in-game-controls-3.png)

-   **Make a great first impression.** When Movie Moments launches for the first time, instructional UI prompts the user to begin creating movies without obstructing their experience.

    ![launch screen for movie moments app](images/instructional-ui-movie.png)

-   **Guide users to take the next step in a complicated task.** In the Windows Mail app, a hint at the bottom of the Inbox directs users to **Settings** to access older messages.

    ![cropped screen shot of windows mail app showing instructional ui message](images/instructional-ui-mail-inbox.png)

    When the user clicks the message, the app's **Settings** flyout appears on the right side of the screen, allowing the user to complete the task. These screen shots show the Mail app before and after a user selects the instructional UI message.

    | Before                                                               | After                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![screen shot of windows mail app](images/instructional-ui-mail.png) | ![screen shot of windows mail app with an extended settings flyout](images/instructional-ui-mail-flyout.png) |

## <span id="related_topics"></span>Related articles

* [Guidelines for app help](guidelines-for-app-help.md)



<!--HONumber=Aug16_HO3-->


