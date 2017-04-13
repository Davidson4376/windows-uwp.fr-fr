---
author: jnHs
Description: "Sur la page Packages, vous pouvez charger tous les fichiers de package (.xap, .appx, .appxupload et/ou .appxbundle) pour l’application que vous soumettez. Vous pouvez charger des packages pour tous les systèmes d’exploitation ciblés par votre application."
title: "Chargement des packages d’application"
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 1bc2ce82688db20315113efc9b080b449a850f05
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="upload-app-packages"></a>Chargement des packages d’application


Sur la page **Packages**, vous pouvez charger tous les fichiers de package (.appx, .appxupload, .appxbundle, et/ou .xap) pour l’application que vous soumettez. Vous pouvez charger des packages pour tous les systèmes d’exploitation ciblés par votre application. Quand un client télécharge votre application, le WindowsStore propose automatiquement au client le package le mieux adapté à son appareil. Une fois vos packages chargés, vous verrez un tableau indiquant [les packages offerts aux familles d’appareilsWindows10 spécifiques](#device-family-availability) (et aux systèmes d’exploitation plus anciens, le cas échéant), classés par ordre.

Pour plus d’informations sur le contenu et sur la structure d’un package, voir [Exigences relatives au package de l’application](app-package-requirements.md). Vous devez également découvrir [comment les numéros de version peuvent avoir un impact sur les packages livrés à des clients spécifiques](package-version-numbering.md) et [comment les packages sont distribués à différents systèmes d’exploitation](guidance-for-app-package-management.md).

## <a name="uploading-packages-to-your-submission"></a>Chargement de packages pour votre soumission

Pour charger des packages, faites-les glisser dans le champ de chargement, ou cliquez pour parcourir vos fichiers. La page **Packages** permet de charger des fichiers .xap, .appx, .appxupload et/ou .appxbundle.

Si vous avez créé des [versions d’évaluation de package](package-flights.md) pour votre application, une liste déroulante apparaît avec l’option de copie des packages de l’une des versions d’évaluation de package. Sélectionnez la version d’évaluation de package comportant les packages que vous souhaitez intégrer. Vous pouvez transférer la totalité ou uniquement une partie des packages dans cette soumission.

> **Important**  Pour Windows10, vous devez toujours charger le fichier .appxupload, et non le fichier .appx ou .appxbundle. Pour plus d’informations sur l’empaquetage d’applications pour UWP pour le Windows Store, consultez [Empaquetage d’applications Windows universelles pour Windows 10](../packaging/packaging-uwp-apps.md).

Si nous détectons des problèmes liés à vos packages lors de leur validation, vous devrez supprimer le package et résoudre le problème avant d’essayer de le charger à nouveau. Pour plus d’informations, voir [Résolution des erreurs de chargement de package](resolve-package-upload-errors.md).

Il se peut également que vous receviez des avertissements concernant des problèmes potentiels, sans que cela vous empêche de poursuivre votre soumission.

## <a name="device-family-availability"></a>Disponibilité de la famille d’appareils

Une fois vos packages correctement chargés, la section **Disponibilité de la famille d’appareils** affiche un tableau identifiant les packages offerts aux familles d’appareilsWindows10 spécifiques (et aux versions antérieures du système d’exploitation), classés par ordre. Dans cette section, vous déterminez également si vous souhaitez offrir la soumission aux clients sur des familles d’appareils Windows10 spécifiques.

> **Remarque** Si vous n’avez pas encore chargé les packages, la section **Disponibilité de la famille d’appareils** affiche les familles d’appareils Windows10 spécifiques, avec des cases à cocher vous permettant de configurer la soumission pour les clients sur ces familles d’appareils. Le tableau apparaît uniquement une fois que les packages sont chargés.

Vous verrez également une case à cocher permettant d’indiquer si vous souhaitez permettre à Microsoft de rendre l’application disponible pour de futures familles d’appareils Windows 10. Nous recommandons de conserver cette case à cocher activée pour que votre application puisse être disponible pour davantage de clients potentiels à mesure que de nouvelles familles d’appareils seront introduites.

### <a name="choosing-which-device-families-to-support"></a>Sélection des familles d’appareils prises en charge

Vous pouvez décocher la case en regard d’une famille d’appareilsWindows10 si vous ne souhaitez pas proposer la soumission aux clients sur ce type d’appareils. Si la case associée à une famille d’appareils est décochée, les nouveaux clients sur ce type d’appareils ne pourront pas acquérir l’application (bien que les clients qui possèdent déjà l’application puissent toujours l’utiliser, et bénéficient des mises à jour soumises). 

> **Remarque** Aucune case à cocher associée à **Windows8/8.1** et à **WindowsPhone8.x et aux versions antérieures** n’existe. Si votre soumission comprend des packages pouvant s’exécuter sur ces versions de système d’exploitation, ces packages seront rendus disponibles pour les clients. Pour arrêter de proposer votre application aux clients sur les versions antérieures du système d’exploitation, il vous faudra supprimer les packages correspondants de votre soumission.

Si votre application prend en charge les familles d’appareils mobiles et de bureau, nous vous recommandons de conserver cochées les cases **Windows 10 Mobile** et **Windows 10 Desktop**, sauf si vous avez une raison spécifique de limiter les types d’appareilsWindows10 pouvant acquérir votre application. Par exemple, vous avez peut-être créé des packages Windows universels, mais savez que vous devez encore tester certaines fonctionnalités de l’application sur des appareils mobiles. Pour empêcher de nouveaux clients de télécharger l’application sur des appareils mobiles Windows10, vous pouvez désactiver la case à cocher **Windows 10 Mobile**. Si vous estimez ultérieurement être prêt à proposer l’application aux clients sur appareils mobiles Windows10, vous pouvez créer une soumission en activant la case à cocher **Windows 10 Mobile**.

Si votre application n’est pas un jeu (ou s’il s’agit d’un jeu et que vous êtes passé par le processus d’[approbation de concept](../gaming/concept-approval.md)), et que votre soumission comprend des packagesUWP neutres et/ou x64 compilés à l’aide de la version14393 du SDKWindows10 ou une version ultérieure, vous pouvez cocher la case **Xbox Windows10** afin de proposer l’application aux clients sur Xbox. 

> **Important** Pour que votre application se lance sur les appareilsXbox, vous devez inclure un package neutre ou x64 compilé avec la version14393du SDKWindows ou une version ultérieure. Toutefois, si vous cochez la caseXbox Windows10, votre package présentant la version la plus élevée applicable à Xbox (un package neutre ou x64 ciblant la famille d’appareilsuniverselle ou Xbox) sera toujours proposé aux clients sur Xbox, même s’il est compilé avec une versionantérieure du SDK. Pour cette raison, il est essentiel de vous assurer que le package présentant la version la plus élevée applicable à Xbox soit compilé avec la version14393 du SDKWindows ou une version supérieure. Si ce n’est pas le cas, vous verrez un message d’erreur indiquant que les clientsXbox ne seront pas en mesure de lancer votre application. 
> 
> Pour résoudre ce problème, procédez comme suit:
> -    Remplacez les packages applicables par les instances compilées à l’aide de la version14393 du SDKWindows ou une version ultérieure.
> -    Si vous disposez déjà d’un package qui prend en charge Xbox et qui est compilé à l’aide de la version14393 du SDKWindows ou une version ultérieure, augmentez son numéro de version, de manière à en faire le package présentant la version la plus élevée de la soumission.
> -    Désélectionnez la case à cocher **Xbox Windows 10**.
>     
> SI vous ne parvenez toujours pas à résoudre le problème, contactez le support technique.

Si vous avez testé votre application pour vous assurer qu’elle s’exécute correctement sur Microsoft HoloLens, vous pouvez également cocher la case **Windows 10 Holographique** pour proposer l’application aux clients HoloLens. Pour plus d’informations sur la création, le test et la publication d’applications holographiques, voir la [Vue d’ensemble du développement holographique Windows](http://dev.windows.com/holographic/development_overview).

> **Important** Pour empêcher complètement une certaine famille d’appareils Windows 10 d’obtenir votre application, vous devez mettre à jour l’élément [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) dans votre manifeste appx pour cibler uniquement la famille d’appareils que vous voulez prendre en charge (c’est-à-dire, Windows.Mobile ou Windows.Desktop), au lieu de laisser la valeur Windows.Universal (pour la famille d’appareils universelle) que Microsoft Visual Studio inclut dans le manifeste appx par défaut.

Il est important de savoir que les sélections que vous opérez ici s’appliquent uniquement aux nouvelles acquisitions. Quiconque disposant déjà de votre application peut continuer à l’utiliser et obtenir les mises à jour que vous soumettez, même si vous supprimez cette famille d’appareils ici. Cela s’applique même aux clients ayant acquis votre application avant la mise à niveau vers Windows 10. Par exemple, si vous avez une application publiée avec des packages Windows Phone 8.1, puis ajoutez un package Windows 10 (UWP) à la même application, qui cible la famille d’appareils universelle, les clients mobiles Windows 10 qui disposaient de votre package Windows Phone 8.1 recevront une mise à jour vers ce package Windows 10 (UWP), même si vous avez désactivé la case à cocher **Windows 10 Mobile** (car il ne s’agit pas d’une nouvelle acquisition, mais d’une mise à jour). En revanche, si vous ne fournissez pas de package Windows 10 (UWP) ciblant la famille d’appareils universelle ou d’appareils mobiles, vos clients mobiles Windows 10 resteront avec le package Windows Phone 8.1.

Pour plus d’informations sur les familles d’appareils, voir le [Guide des applications pour la plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631) et [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

### <a name="understanding-ranking"></a>Compréhension du classement

Cette section, qui vous permet d’identifier les familles d’appareils Windows10 qui peuvent télécharger votre soumission, vous indique également les packages spécifiques qui seront rendus disponibles aux familles d’appareils considérées. Si vous possédez plusieurs packages pouvant s’exécuter sur une famille d’appareils spécifique, le tableau indique l’ordre de proposition des packages, en fonction de leur numéro de version. Pour plus d’informations sur le classement des packages en fonction de leur numéro de version, consultez la section [Numérotation des versions de packages](package-version-numbering.md). 

Par exemple, imaginons que vous disposez de deuxpackages: Package_A.appxupload et Package_B.appxupload. Pour une famille d’appareils donnée, si Package_A.appxupload est classé au premier rang et que Package_B.appxupload est classé au second rang, lorsqu’un client sur ce type d’appareil acquiert votre application, le WindowsStore tentera dans un premier temps d’offrir Package_A.appxupload. Si l’appareil du client n’est pas en mesure d’exécuter Package_A.appxupload, le WindowsStore propose Package_B.appxupload. Si l’appareil du client ne peut exécuter aucun des packages associés à cette famille d’appareils, par exemple, si l’instance **MinVersion** prise en charge par votre application est supérieure à la version de l’appareil du client, le client ne pourra pas télécharger l’appareil sur cet appareil.

> **Remarque** Les numéros de version des packages.xap ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour cette raison, si vous disposez de plusieurs packages de rang égal, vous verrez un astérisque en lieu et place d’un numéro; les clients peuvent recevoir les deuxpackages. Pour mettre à jour les clients d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.



## <a name="package-details"></a>Détails du package

Une fois vos packages correctement chargés, nous en dressons la liste en les groupant par système d’exploitation cible. Nous affichons le nom, la version et l’architecture du package. Cliquez sur **Afficher les détails** pour visualiser des informations complémentaires comme les langues prises en charge, les fonctionnalités de l’application et la taille de fichier de chaque package.

Si vous utilisez la [Médiation publicitaire Windows](../monetize/use-ad-mediation-to-maximize-revenue.md), vous disposez également d’un lien pour configurer la médiation publicitaire pour chaque package.

Si vous voulez supprimer un package de votre soumission, cliquez sur le lien **Supprimer** au bas de la section **Détails** de chaque package.

## <a name="removing-redundant-packages"></a>Suppression des packages redondants

Si nous détectons qu’un ou plusieurs de vos packages sont redondants, nous affichons un avertissement vous recommandant de supprimer ces packages de votre soumission. Cette situation survient généralement quand, après avoir chargé des packages spécifiques, fournissez des packages de version supérieure prenant en charge le même ensemble de clients. Dans ce cas, aucun client n’obtient le package redondant, car vous proposez maintenant d’un meilleur package (version supérieure).

Si nous détectons la présence de packages redondants, nous vous offrons la possibilité de tous les supprimer automatiquement de cette soumission. Vous pouvez également supprimer des packages de la soumission individuellement.

## <a name="gradual-package-rollout"></a>Lancement de package progressif

Si votre soumission est une mise à jour d’une application publiée précédemment, une case à cocher **Déployer progressivement la mise à jour après la publication de cette soumission (pour les clientsWindows10 uniquement)** s’affiche. Vous pouvez choisir un pourcentage de clients qui récupèrent les packages de la soumission, de manière à pouvoir surveiller les commentaires et les données d’analyse, et ainsi vérifier le contenu de la mise à jour avant de la déployer plus largement. Vous pouvez augmenter le pourcentage (ou arrêter la mise à jour) à tout moment sans avoir à créer une nouvelle soumission. 

Pour plus d’informations, voir [Lancement de package progressif](gradual-package-rollout.md).

## <a name="mandatory-update"></a>Mise à jour obligatoire

Si votre soumission est une mise à jour d’une application publiée précédemment, une case à cocher **Rendre obligatoire cette mise à jour** s’affiche. Vous pouvez ainsi définir la date et l’heure d’une mise à jour obligatoire, en supposant que vous ayez utilisé les APIWindows.Services.Store pour permettre à votre application de vérifier par programme les mises à jour de packages, et de télécharger et d’installer les packages mis à jour. Pour utiliser cette option, votre application doit cibler Windows10, version1607 ou une version ultérieure.

Pour plus d’informations, consultez la section [Télécharger et installer des mises à jour de package pour votre application](../packaging/self-install-package-updates.md).

 




