---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Découvrez comment ajouter des valeurs d’ID d’application et d’ID d’unité publicitaire du tableau de bord du Centre de développement Windows à votre application avant de la soumettre au Windows Store.
title: Configurer des unités publicitaires dans votre application

---

# Configurer des unités publicitaires dans votre application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Lorsque vous utilisez une classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour afficher des publicités dans votre application, vous devez spécifier un ID d’application et un ID d’unité publicitaire. Lors du développement de votre application, utilisez les [valeurs de test d’ID d’application et d’ID d’unité publicitaire](test-mode-values.md) pour voir comment votre application restitue les publicités au cours du test.

Après avoir testé votre application, et une fois que vous êtes prêt à la soumettre au Centre de développement Windows, vous devez mettre à jour son code pour qu’il utilise les valeurs des ID d’application et d’unité publicitaire du [tableau de bord du Centre de développement Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Si vous essayez d’utiliser des valeurs de test dans votre application dynamique, votre application ne recevra pas de publicités dynamiques.

Pour configurer l’ID d’application et les unités publicitaires pour votre application dynamique :

1.  Dans le tableau de bord du Centre de développement Windows, sélectionnez votre application, puis cliquez sur **Monétisation &gt; Monétiser avec des publicités**.
2.  Dans la section **Publicités Microsoft Advertising** de cette page, créez une unité publicitaire. Pour le type d’unité publicitaire, sélectionnez **Bannière** si vous utilisez un **AdControl**, ou **Spot vidéo** si vous utilisez un **InterstitialAd**. Pour en savoir plus sur cette page, voir [Monétiser à l’aide des publicités](../publish/monetize-with-ads.md).

3.  Pour chaque unité publicitaire générée, vous voyez l’**ID de l’application** et l’**ID d’unité publicitaire** sur cette page. Pour afficher les publicités dans votre application, vous devez utiliser ces valeurs dans le code de votre application :

    * Si votre application affiche des bannières, affectez ces valeurs aux propriétés **ApplicationId** et **AdUnitId** de votre objet **AdControl**.

    * Si votre application affiche des spots publicitaires, transmettez ces valeurs à la méthode **RequestAd** de votre objet **InterstitialAd**.

> **Important** Si votre application utilise la médiation publicitaire pour afficher les bannières publicitaires de Microsoft (elle utilise un objet **AdMediatorControl**), il n’est pas nécessaire de demander des unités publicitaires. Dans ce scénario, les unités publicitaires sont automatiquement générées pour vous. Pour plus d’informations, voir [Quelle est la différence entre AdMediatorControl et AdControl ?](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

 

## Rubriques connexes

[Valeurs du mode test](test-mode-values.md)


 

 


<!--HONumber=May16_HO2-->


