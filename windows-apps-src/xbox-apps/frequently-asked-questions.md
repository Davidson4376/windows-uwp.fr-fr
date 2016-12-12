---
author: Mtoepke
title: Frequently asked questions
description: FAQ about UWP on Xbox.
translationtype: Human Translation
ms.sourcegitcommit: 1ee75d73cc42455677bc2c0df08b41f33fc4f7b0
ms.openlocfilehash: a6d410e3a4873bb96fba46789b28185c392a7492

---

# <a name="frequently-asked-questions"></a>Frequently asked questions

Things not working the way you expected? Look through this page of frequently asked questions. Also check out the [Known issues](known-issues.md) topic and the [Developing Universal Windows apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) forum. 

### <a name="why-are-my-games-and-apps-not-working"></a>Why are my games and apps not working?

If your games and apps are not working, or if you don’t have access to the store or to Live services, you are probably running in Developer Mode. You can tell you’re running in Developer Mode if you select Home and you see a big Dev Home tile on the right side of your screen, instead of the usual Gold/Live content. If you want to play games, you can open Dev Home and switch back to Retail Mode by using the **Leave developer mode** button.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Why can’t I connect to my Xbox One using Visual Studio?

Start by verifying that you are running in Developer Mode, and not in Retail Mode. You cannot connect to your Xbox One when it is in Retail Mode. You can simply check this by pressing the **Home** button and looking for the Dev Home tile on the right side of your screen. If the tile is not there, but instead you see Gold/Live content, you are in Retail Mode. You need to run the Dev Mode Activation app to switch to Developer Mode.

> [!NOTE]
> You must have a user signed in to deploy an app.

For more information, see [Fixing deployment failures](#fixing-deployment-failures) later on this page.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>How do I switch between Retail Mode and Developer Mode?

Follow the [Xbox One Developer Mode Activation](devkit-activation.md) instructions to understand more about these states.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>How do I know if I am in Retail Mode or Developer Mode?

Follow the [Xbox One Developer Mode Activation](devkit-activation.md) instructions to understand more about these states. 

You can simply check this by pressing the **Home** button and looking at the right side of the screen. If you are in Developer Mode, you will see the Dev Home tile on the right side. If you are in Retail Mode, you will see the usual Gold/Live content.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Will my games and apps still work if I activate Developer Mode?

Yes, you can switch from Developer Mode to Retail Mode, where you can play your games. For more information, see the [Xbox One Developer Mode Activation](devkit-activation.md) page. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Will I lose my games and apps or saved changes?

If you decide to leave the Developer Program, you won't lose your installed games and apps. In addition, as long as you were online when you played them, your saved games are all saved on your Live account cloud profile, so you won’t lose them.

### <a name="how-do-i-leave-the-developer-program"></a>How do I leave the Developer Program?

See the [Xbox One Developer Mode Deactivation](devkit-deactivation.md) topic for details about how to leave the Developer Program.

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>I sold my Xbox One and left it in Developer Mode. How do I deactivate Developer Mode?

If you no longer have access to your Xbox One, you can deactivate it in Windows Dev Center. For details, see the **Deactivate your console using Windows Dev Center** section in the [Xbox One Developer Mode Deactivation](devkit-deactivation.md#deactivate-your-console-using-windows-dev-center) topic. 

### <a name="i-left-the-developer-program-using-windows-dev-center-but-im-in-still-developer-mode-what-do-i-do"></a>I left the Developer Program using Windows Dev Center but I’m in still Developer Mode. What do I do?

Start Dev Home and select the **Leave developer mode** button. This will restart your console in Retail Mode. 

### <a name="can-i-publish-my-app"></a>Can I publish my app?

You can [publish apps](../publish/index.md) through Dev Center if you have a [developer account](https://developer.microsoft.com/store/register). UWP apps created and tested on a retail Xbox One console will go through the same ingestion, review, and publication process that Windows conducts today, with additional reviews to meet today’s Xbox One standards.

### <a name="can-i-publish-my-game"></a>Can I publish my game?

You can use UWP and your Xbox One in Developer Mode to build and test your games on Xbox One. To publish UWP games, you must register with [ID@XBOX](http://www.xbox.com/Developers/id). 
[ID@XBOX](http://www.xbox.com/Developers/id) provides developers full access to Xbox Live APIs for their games, including Gamerscore and Achievements, as well as the ability to take advantage of multiplayer between devices, cloud saves, and all the features of Xbox Live on Xbox One. 
[ID@XBOX](http://www.xbox.com/Developers/id) can also provide access to Xbox One development kits for games that require access to the maximum potential of the Xbox One hardware.

### <a name="will-the-standard-game-engines-work"></a>Will the standard Game engines work?

Check out the [Known issues](known-issues.md) page for this release.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>What capabilities and system resources are available to UWP games on Xbox One? 

For information, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>If I create a DirectX 12 UWP game, will it run on my Xbox One in Developer Mode?

For information, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>Will the entire UWP API surface be available on Xbox?

Check out the [Known issues](known-issues.md) page for this release.

### <a name="fixing-deployment-failures"></a>Fixing deployment failures

If you can’t deploy your app from Visual Studio, these steps may help you fix the problem. If you get stuck, ask for help on the forum.

> [!NOTE]
> You must have a user signed in to deploy an app. If you receive a 0x87e10008 error message, make sure you have a user signed in and try again.

If Visual Studio cannot connect to your Xbox One:

1. Make sure that you are in Developer Mode (discussed earlier on this page).
2. Make sure that you have set up your development PC correctly. Did you follow *all* of the directions in [Getting started with UWP app development on Xbox One](getting-started.md)? 

3. If you haven’t yet, read through the [Development environment setup](development-environment-setup.md) topic and the [Introduction to Xbox One tools](introduction-to-xbox-tools.md) topic.

4. Make sure that you can “ping” your console IP address from your development PC.
  > [!NOTE]
  > In order to get the best deployment performance, we recommend that you use a wired connection to your console.

5. Make sure that you are using the Universal (Unencrypted Protocol) in the Authentication drop-down list on the **Debug** tab. For more details, see [Development environment setup](development-environment-setup.md).

<!--6. Make sure you are not hitting a PIN pairing issue; see "Visual Studio/Xbox PIN pairing failures" in the [Known Issues](known-issues.md) topic.-->

<!--
If Visual Studio can connect, but deployment is failing (for example you get this error message: "DEP0700 : Registration of the app failed.(0x80073cf9)"):

1. Make sure that your app is not installed by uninstalling it from the Collections app in the Xbox One shell. 

> **Note**&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

2. If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode, and then switch back to Developer Mode. 
This will clear Dev Storage.

3. If your issues persist, follow the steps above and then use **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content and deactivate Developer Mode.
-->

### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>If I’m building an app using HTML/JavaScript, how do I enable Gamepad navigation?

TVHelpers is a set of JavaScript and XAML/C# samples and libraries to help you build great Xbox One and TV experiences in JavaScript and C#. TVJS is a library that helps you build premium UWP apps for Xbox One. TVJS includes support for automatic controller navigation, rich media playback, search, and more. You can use TVJS with your hosted web app just as easily as with a packaged web UWP app with full access to the Windows Runtime APIs.

For more information, see the [TVHelpers](https://github.com/Microsoft/TVHelpers) project and the project [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>See also
- [Known issues with UWP on Xbox One](known-issues.md)
- [UWP on Xbox One](index.md)



<!--HONumber=Dec16_HO1-->


