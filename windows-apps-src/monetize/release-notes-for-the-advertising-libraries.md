---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "Passez en revue les notes de publication des bibliothèques de publicités Microsoft contenues dans le Microsoft Store Services SDK"
title: "Notes de publication des bibliothèques de publicités Microsoft"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: b82c4385b0e7089bdddbe094f47f0766f90aa21b

---

# Notes de publication des bibliothèques de publicités Microsoft




Cette section fournit des notes de publication de la version actuelle des bibliothèques de publicités de Microsoft dans le Microsoft Store Services SDK (pour les applications UWP) et dans le Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x (pour les applications Windows8.1 et Windows Phone8.x). Ces bibliothèques prennent en charge les applications XAML et HTML/JavaScript pour Windows10, Windows8.1, Windows Phone8.1 et Windows Phone8.

## Installation


Les bibliothèques de publicités de Microsoft sont disponibles dans le cadre du [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (pour les applications UWP) et du [Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) (pour les applications Windows8.1 et Windows Phone8.x). Pour plus d’informations sur l’installation des SDK et des bibliothèques qu’ils contiennent, voir [Installer les bibliothèques de publicités Microsoft](install-the-microsoft-advertising-libraries.md).

Pour obtenir les assemblys de publicités Microsoft pour des projets Silverlight Windows Phone8.x, installez le [Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk), ouvrez votre projet dans Visual Studio, puis accédez à **Projet** > **Ajouter un service connecté** > **AdMediator** pour télécharger automatiquement les assemblys. Après quoi, vous pouvez supprimer les références au médiateur publicitaire de votre projet si vous ne souhaitez pas utiliser la médiation publicitaire. Pour plus d’informations, voir [AdControl dans Silverlight WindowsPhone](adcontrol-in-windows-phone-silverlight.md).


## Désinstaller les versions précédentes

Avant d’installer le Microsoft Store Services SDK (pour les applications UWP) ou le Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x (pour les applications Windows8.1 et Windows Phone8.x), il est vivement conseillé de désinstaller toutes les instances précédentes du Kit de développement logiciel (SDK) Microsoft Universal AdClient ou du Kit SDK Microsoft Advertising.

## Cibler des sorties de génération propres à une architecture

Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **TouteUC** dans votre projet. Si votre projet cible la plateforme **TouteUC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, voir [Problèmes connus](known-issues-for-the-advertising-libraries.md).

## Support C++

Les bibliothèques de publicités Microsoft (qui incluent les classes **AdControl** et **InterstitialAd**) prennent en charge les applications écrites en C++ et DirectX à l’aide de l’interopérabilité de Windows Runtime (*interop*). Pour plus d’informations et d’exemples sur la programmation en C++ et XAML, voir [Système de types](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## Aucun contrôle de boîte à outils

Dans la version actuelle des bibliothèques de publicités Microsoft contenues dans le Microsoft Store Services SDK et dans le Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x, il n’existe aucun contrôle de boîte à outils pour faire glisser une classe **AdControl** ou **InterstitialAd** vers une aire de conception de votre application. Pour obtenir des instructions sur l’ajout de ces contrôles dans votre balisage et votre code, voir [Procédures pas à pas de développement](developer-walkthroughs.md).

## Les propriétés latitude et longitude ne sont plus disponibles

La classe **AdControl** ne possède plus les propriétés **Latitude** et **Longitude** pour les applicationsUWP. À la place, le code intégré au contrôle de publicité détecte et envoie ces valeurs aux serveurs de publicités pour le compte de l’application.

## Remarque importante

Veillez à lire le Contrat de Licence Utilisateur Final (CLUF) dans son intégralité. Voir la rubrique [Remarque importante: CLUF](important-notice-eula.md).

 

 



<!--HONumber=Sep16_HO2-->


