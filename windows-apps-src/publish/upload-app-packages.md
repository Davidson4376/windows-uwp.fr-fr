---
Description: La page de Packages est quand vous chargez tous les fichiers de package (.appxupload, .appx, .appxbundle et/ou .xap) pour l’application que vous envoyez.
title: Chargement des packages d’application
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, packages, téléchargement, chargement du package
ms.localizationpriority: medium
ms.openlocfilehash: 97735a8e860f7c941cc35d77a21496696683640f
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468878"
---
# <a name="upload-app-packages"></a>Chargement des packages d’application

Le **Packages** page est quand vous chargez tous les fichiers de package (.msix, .msixupload, .msixbundle, .appx, .appxupload et/ou .appxbundle) pour l’application que vous envoyez. Vous pouvez charger tous vos packages pour la même application sur cette page, et quand un client télécharge votre application, le Store fournira automatiquement chaque client avec le package qui convient mieux à leur appareil. Une fois vos packages chargés, vous verrez un tableau indiquant [les packages offerts aux familles d’appareils Windows 10 spécifiques](#device-family-availability) (et aux systèmes d’exploitation plus anciens, le cas échéant), classés par ordre.

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant 8.x/Windows de Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Pour plus d’informations sur le contenu et sur la structure d’un package, voir [Exigences relatives au package de l’application](app-package-requirements.md). Vous souhaiterez également en savoir plus sur [comment numéros de version impact sur les packages qui sont remis à des clients spécifiques](package-version-numbering.md) et [comment gérer les packages pour différents scénarios](guidance-for-app-package-management.md).


## <a name="uploading-packages-to-your-submission"></a>Chargement de packages pour votre soumission

Pour charger des packages, faites-les glisser dans le champ de chargement, ou cliquez pour parcourir vos fichiers. Le **Packages** page vous permettra de télécharger des fichiers .msix, .msixupload, .msixbundle, .appx, .appxupload et/ou .appxbundle.

> [!IMPORTANT]
> Pour Windows 10, nous vous recommandons de télécharger le fichier .msixupload ou .appxupload ici plutôt que de .msix, .appx, .msixbundle ou .appxbundle.  Pour plus d’informations sur la création de packages d’applications UWP pour le Windows Store, consultez l’article [Créer un package d’application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md).

Si vous avez créé des [versions d’évaluation de package](package-flights.md) pour votre application, une liste déroulante apparaît avec l’option de copie des packages de l’une des versions d’évaluation de package. Sélectionnez la version d’évaluation de package comportant les packages que vous souhaitez intégrer. Vous pouvez transférer la totalité ou uniquement une partie des packages dans cette soumission.

Si nous détectons des erreurs avec un package lors de la validation de cela, nous affichons un message pour vous permettre de savoir quel est le problème. Vous devez supprimer le package, résoudre le problème, essayez de télécharger à nouveau. Il se peut également que vous receviez des avertissements concernant des problèmes potentiels, sans que cela vous empêche de poursuivre votre soumission.


## <a name="device-family-availability"></a>Disponibilité de la famille d’appareils

Une fois vos packages correctement chargés, la section **Disponibilité de la famille d’appareils** affiche un tableau identifiant les packages offerts aux familles d’appareils Windows 10 spécifiques (et aux versions antérieures du système d’exploitation), classés par ordre. Dans cette section, vous déterminez également si vous souhaitez offrir ou non la soumission aux clients sur des familles d’appareils Windows 10 spécifiques.

Pour plus d’informations, consultez la section [Disponibilité de la famille d’appareils](device-family-availability.md).


## <a name="package-details"></a>Détails du package

Vos packages téléchargés sont répertoriés ici, regroupés par système d’exploitation cible. Nous affichons le nom, la version et l’architecture du package. Cliquez sur **Afficher les détails** pour visualiser des informations complémentaires comme les langues prises en charge, les fonctionnalités de l’application et la taille de fichier de chaque package.

Si vous voulez supprimer un package de votre soumission, cliquez sur le lien **Supprimer** au bas de la section **Détails** de chaque package.


## <a name="removing-redundant-packages"></a>Suppression des packages redondants

Si nous détectons qu’un ou plusieurs de vos packages sont redondants, nous affichons un avertissement vous recommandant de supprimer ces packages de votre soumission. Cette situation survient généralement quand, après avoir chargé des packages spécifiques, fournissez des packages de version supérieure prenant en charge le même ensemble de clients. Dans ce cas, aucun client n’obtient le package redondant, car vous proposez maintenant d’un meilleur package (version supérieure).

Si nous détectons la présence de packages redondants, nous vous offrons la possibilité de tous les supprimer automatiquement de cette soumission. Vous pouvez également supprimer des packages de la soumission individuellement.


## <a name="gradual-package-rollout"></a>Lancement de package progressif

Si votre soumission est une mise à jour d’une application publiée précédemment, une case à cocher **Déployer progressivement la mise à jour après la publication de cette soumission (pour les clients Windows 10 uniquement)** s’affiche. Vous pouvez choisir un pourcentage de clients qui récupèrent les packages de la soumission, de manière à pouvoir surveiller les commentaires et les données d’analyse, et ainsi vérifier le contenu de la mise à jour avant de la déployer plus largement. Vous pouvez augmenter le pourcentage (ou arrêter la mise à jour) à tout moment sans avoir à créer une nouvelle soumission. 

Pour plus d’informations, voir [Lancement de package progressif](gradual-package-rollout.md).


## <a name="mandatory-update"></a>Mise à jour obligatoire

Si votre soumission est une mise à jour d’une application publiée précédemment, une case à cocher **Rendre obligatoire cette mise à jour** s’affiche. Vous pouvez ainsi définir la date et l’heure d’une mise à jour obligatoire, en supposant que vous ayez utilisé les API Windows.Services.Store pour permettre à votre application de vérifier par programme les mises à jour de packages, et de télécharger et d’installer les packages mis à jour. Pour utiliser cette option, votre application doit cibler Windows 10, version 1607 ou une version ultérieure.

Pour plus d’informations, consultez la section [Télécharger et installer des mises à jour de package pour votre application](../packaging/self-install-package-updates.md).

 




