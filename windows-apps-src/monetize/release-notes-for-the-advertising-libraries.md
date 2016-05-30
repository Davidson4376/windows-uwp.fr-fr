---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Passez en revue les notes de publication des bibliothèques de publicités Microsoft contenues dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft.
title: Notes de publication des bibliothèques de publicités Microsoft
---

# Notes de publication des bibliothèques de publicités Microsoft


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette section fournit les notes de publication de la version actuelle des bibliothèques de publicités Microsoft contenues dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft. Ces bibliothèques prennent en charge les applications XAML et HTML/JavaScript pour Windows 10, Windows 8.1, Windows Phone 8.1 et Windows Phone 8.

## Installation


Les bibliothèques de publicités Microsoft sont incluses dans le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk). Pour tous les types de projet autres que Silverlight Windows Phone 8.x, les assemblys de Microsoft Advertising qui étaient distribués dans les versions autonomes précédentes du Kit de développement logiciel (SDK) Microsoft Universal Ad Client et du Kit de développement logiciel (SDK) Microsoft Advertising sont désormais installés avec le SDK d’engagement et de monétisation de la Boutique Microsoft. Pour plus d’informations sur l’installation du SDK et des bibliothèques qu’il contient, voir [Installer les bibliothèques de publicités Microsoft](install-the-microsoft-advertising-libraries.md).

Pour obtenir les assemblys de publicités Microsoft pour des projets Silverlight Windows Phone 8.x, installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk), ouvrez votre projet dans Visual Studio, puis accédez à **Projet** > **Ajouter un service connecté** > **Ad Mediator** pour télécharger automatiquement les assemblys. Après quoi, vous pouvez supprimer les références au médiateur publicitaire dans votre projet si vous ne souhaitez pas utiliser la médiation publicitaire. Pour plus d’informations, voir [AdControl dans Silverlight Windows Phone](adcontrol-in-windows-phone-silverlight.md).

## Présentation des différences entre les bibliothèques de publicités Microsoft et la médiation publicitaire

Même si les bibliothèques de publicités Microsoft et les bibliothèques de médiation publicitaire sont fournies par le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft, ces bibliothèques ont des objets différents. Utilisez les classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) et [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) des bibliothèques de publicités Microsoft si vous voulez afficher des bannières publicitaires et des spots vidéo publicitaires de Microsoft dans une application XAML ou JavaScript. Utilisez la classe **AdMediatorControl** des bibliothèques de médiation publicitaire si vous voulez afficher des bannières publicitaires provenant de plusieurs réseaux publicitaires dans une application XAML (la médiation publicitaire n’est pas prise en charge pour les applications JavaScript/HTML). Pour plus d’informations, voir [Quelle est la différence entre AdMediatorControl et AdControl ?](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Désinstaller les versions précédentes

Avant d’installer le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft, il est vivement conseillé de désinstaller toutes les instances préalables du Kit de développement logiciel (SDK) Microsoft Universal Ad Client et du Kit de développement logiciel (SDK) Microsoft Advertising.

## Cibler des sorties de génération propres à une architecture

Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **Toute UC** dans votre projet. Si votre projet cible la plateforme **Toute UC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, voir [Problèmes connus](known-issues-for-the-advertising-libraries.md).

## Support C++

Les bibliothèques de publicités Microsoft (qui incluent les classes **AdControl** et **InterstitialAd**) prennent en charge les applications écrites en C++ et DirectX à l’aide de l’interopérabilité de Windows Runtime (*interop*). Pour plus d’informations et d’exemples sur la programmation en C++ et XAML, voir [Système de types](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## Aucun contrôle de boîte à outils

Dans la version actuelle des bibliothèques de publicités Microsoft contenues dans le SDK d’engagement et de monétisation de la Boutique Microsoft, il n’existe aucun contrôle de boîte à outils pour faire glisser une classe **AdControl** ou **InterstitialAd** vers une aire de conception de votre application. Pour obtenir des instructions sur l’ajout de ces contrôles dans le balisage et le code, voir [Procédures pas à pas de développement](developer-walkthroughs.md).

## Les propriétés latitude et longitude ne sont plus disponibles

La classe **AdControl** ne possède plus les propriétés **Latitude** et **Longitude** pour les applications UWP. À la place, le code intégré au contrôle de publicité détecte et envoie ces valeurs aux serveurs de publicités pour le compte de l’application.

## Remarque importante

Veillez à lire le Contrat de Licence Utilisateur Final (CLUF) dans son intégralité. Voir la rubrique [Remarque importante : CLUF](important-notice-eula.md).

 

 


<!--HONumber=May16_HO2-->


