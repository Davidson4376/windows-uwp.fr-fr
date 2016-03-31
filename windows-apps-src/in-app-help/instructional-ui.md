---
Design an instructional user interface (UI) that teaches users how to work with your Windows Store app.
Guidelines for designing instructional UI
ms.assetid: D082D4FF-CA09-48C8-9E02-2ECA457B1B37
Instructional UI
template: detail.hbs
---

# Guidelines for designing instructional UI


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Design an instructional user interface (UI) that teaches users how to work with your Universal Windows Platform (UWP) app.

## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Recommendations


-   Use instructional UI to introduce a new user to what your app can do.
-   Use for tips on new features or specific details about how your app has changed after an update.
-   Integrate instructional UI with specific tasks.
-   Don't block interaction with the application UI.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Additional usage guidance


In some circumstances, the best way to help users interact with your app is to teach them from within your app's UI. We use the term *instructional UI* to refer to this type of guidance. A great example is using a UI element, like inline text or a flyout, to tell a user when they need to use a touch interaction to complete a task.

Keep in mind that instructional UI is not a replacement for thoughtful design. If you use it too often or out of context, you can disrupt the flow of your app and diminish its effectiveness. Before adding instructional UI, explore other ways to introduce users to your app. See Related Topics for more info about layouts, navigation, and controls.

Here are a few instances in which instructional UI can help your users learn:

-   **Helping users discover touch interactions.** The following screen shot shows instructional UI teaching a player how to use touch gestures in the game, Cut the Rope.

    ![screen shot from game showing instructional ui message, "slide acress to cut the rope"](images/in-game-controls-3.png)

-   **Making a great first impression.** Consider using instructional UI to introduce a new user to what your app can do. For example, when Movie Moments launches for the first time, instructional UI prompts the user to begin creating movies.

    ![launch screen for movie moments app](images/instructional-ui-movie.png)

-   **Guiding users to take the next step in a complicated task.** In the Windows Mail app, a hint at the bottom of the Inbox directs users to **Settings** to access older messages.

    ![cropped screen shot of windows mail app showing instructional ui message](images/instructional-ui-mail-inbox.png)

    When the user clicks the message, the app's **Settings** flyout appears on the right side of the screen, allowing the user to complete the task. These screen shots show the Mail app before and after a user clicks the instructional UI message.

    | Before                                                               | After                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![screen shot of windows mail app](images/instructional-ui-mail.png) | ![screen shot of windows mail app with an extended settings flyout](images/instructional-ui-mail-flyout.png) |

     

-   **Introducing UI changes.** If you made significant UI changes in the latest version of your app, users might like tips on new features or specific details about how your app has changed.

**Principles of instructional UI design**

-   **Keep it simple.** Introduce one basic concept at a time and use images when possible. Consider adding a Help section to your UWP app to address complex features.
-   **Teach in context.** Integrate instructional UI with the task it helps a user complete. A user is more likely to retain a concept when it's introduced when they need it most.
-   **Don't block interaction.** Make sure users can still interact with your app while instructional UI is present. Instructional UI should help users, not annoy them by getting in the way.
-   **Teach, then disappear.** Remove instructional UI as soon as it's no longer relevant or allow the user to dismiss it. Also, in most cases, users only need instructional UI displayed once. Avoid repeatedly displaying the same instructional UI.
-   **Use sparingly.** Thoughtful design and a Help section can often teach users everything they need to know to enjoy your app. Consider the breadth of design options before adding instructional UI to your app.

## <span id="related_topics"></span>Related articles

* [Guidelines for app help](guidelines-for-app-help.md)
 

 




<!--HONumber=Mar16_HO1-->
