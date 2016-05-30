---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: En savoir plus sur le processus de bout en bout de développement et de publication d’une application avec des publicités.
title: Flux de travail de création d’applications contenant des publicités

---

# Flux de travail de création d’applications contenant des publicités


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Pour afficher des publicités dans vos applications, votre application doit être en mesure de recevoir des publicités à partir d’un réseau publicitaire. Microsoft fournit un service web permettant aux développeurs d’applications Windows de recevoir des publicités. Lorsque l’utilisateur clique sur la publicité dans votre application, vous (en tant qu’*éditeur* de la publicité) touchez de l’argent de la part du créateur de la publicité (l’*annonceur*). L’argent ainsi gagné auprès des publicitaires vous est payé à l’aide de votre compte.

Les principales étapes suivantes décrivent le développement et la publication d’une application contenant des publicités.

1.  Étape de développement :

    * Configurez votre compte du Centre de développement Windows.
    * Développez votre application à l’aide des valeurs du mode test.

2.  Prêt à la diffusion :

    * Configurez votre application pour recevoir des publicités dynamiques.
    * Soumettez votre application au Centre de développement Windows et prenez connaissances des données de performances.

Pour plus d’informations sur les différentes étapes, lisez la section correspondante ci-dessous.

## Configurez votre compte du Centre de développement Windows

Vous devez posséder un compte auprès du Centre de développement Windows pour pouvoir publier votre application et recevoir des publicités. Cela est valable que vous fassiez appel ou non à la médiation publicitaire. La gestion des applications publicitaires s’effectue également dans le Centre de développement Windows. Si vous avez utilisé Microsoft pubCenter pour gérer la publicité dans vos applications, cela a été remplacé par des fonctionnalités dans le Centre de développement Windows.

Pour configurer votre compte avec le Centre de développement Windows, visitez la [page d’accueil](https://dev.windows.com/windows-apps). Des informations supplémentaires sont disponibles sur les [pages d’aide](https://dev.windows.com/develop) du Centre de développement Windows.

## Développez votre application à l’aide des valeurs du mode test

Suivez les instructions décrites dans les procédures détaillées suivantes pour ajouter un contrôle [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) afin d’afficher des publicités dans votre application :

-   [Spots publicitaires](interstitial-ads.md)
-   [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
-   [AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
-   [AdControl dans Silverlight Windows Phone](adcontrol-in-windows-phone-silverlight.md)

Lorsque vous utilisez un contrôle **AdControl** ou **InterstitialAd** pour afficher des publicités dans votre application, vous devez spécifier un ID d’application et un ID d’unité publicitaire dans votre code pour lier votre application à votre compte du Centre de développement Windows et proposer des publicités. Lors du développement de votre application, utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire pour voir comment votre application restitue les publicités au cours du test. Vous pourrez ainsi voir comment l’application reçoit et restitue les publicités au cours du test. Pour plus d’informations, voir [Valeurs du mode test](test-mode-values.md).

Pour obtenir des exemples complets de projet qui montrent comment ajouter des bannières et des spots vidéo publicitaires à des applications HTML/JavaScript et XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

## Configurez votre application pour recevoir des publicités dynamiques

Après avoir testé votre application, et une fois que vous êtes prêt à la soumettre au Centre de développement Windows, vous devez mettre à jour son code pour qu’il utilise les valeurs des ID d’application et d’unité publicitaire du [tableau de bord du Centre de développement Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Si vous essayez d’utiliser des valeurs de test dans votre application dynamique, votre application ne recevra pas de publicités dynamiques. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

## Soumettre votre application

Après avoir terminé le développement de votre application, vous pouvez la publier dans le Windows Store à l’aide du tableau de bord du Centre de développement Windows. Outre le respect des exigences relatives à l’ensemble des applications dans le Windows Store, les applications affichant des publicités doivent respecter certaines exigences supplémentaires. Pour plus d’informations, voir [Soumettre une application contenant des publicités au Windows Store](submit-an-app-with-ads-to-the-windows-store.md).

Une fois votre application publiée et disponible dans le Windows Store, vous pouvez passer en revue vos [rapports sur les performances publicitaires](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

 

 


<!--HONumber=May16_HO2-->


