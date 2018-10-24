---
author: jnHs
Description: The Packages page is where you upload all of the package files (.appxupload, .appx, .appxbundle, and/or .xap) for the app that you're submitting.
title: Chargement des packages d’application
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, packages, téléchargement, le chargement de package
ms.localizationpriority: medium
ms.openlocfilehash: d966688110870b669bdd296ec14e145a5d77b74e
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5431566"
---
# <a name="upload-app-packages"></a>Chargement des packages d’application

La page **Packages** est, vous pouvez charger tous les fichiers de package (.msix, .msixupload, .msixbundle, .appx, .appxupload, .appxbundle, et/ou .xap) pour l’application que vous soumettez. Vous pouvez charger des packages pour tous les systèmes d’exploitation ciblés par votre application. Quand un client télécharge votre application, le WindowsStore propose automatiquement au client le package le mieux adapté à son appareil. Une fois vos packages chargés, vous verrez un tableau indiquant [les packages offerts aux familles d’appareilsWindows10 spécifiques](#device-family-availability) (et aux systèmes d’exploitation plus anciens, le cas échéant), classés par ordre.

Pour plus d’informations sur le contenu et sur la structure d’un package, voir [Exigences relatives au package de l’application](app-package-requirements.md). Vous devrez également Découvrez [comment les numéros de version peuvent avoir un impact sur les packages livrés à des clients spécifiques](package-version-numbering.md) et [comment les packages sont distribués à différents systèmes d’exploitation](guidance-for-app-package-management.md).

## <a name="uploading-packages-to-your-submission"></a>Chargement de packages pour votre soumission

Pour charger des packages, faites-les glisser dans le champ de chargement, ou cliquez pour parcourir vos fichiers. La page **Packages** vous permet de télécharger des fichiers .msix, .msixupload, .msixbundle, .appx, .appxupload, .appxbundle, et/ou .xap.

> [!IMPORTANT]
> Pour Windows 10, nous vous recommandons de charger le fichier .msixupload ou .appxupload ici plutôt que .msix, .appx, .msixbundle ou .appxbundle.  Pour plus d’informations sur la création de packages d’applicationsUWP pour le WindowsStore, consultez l’article [Créer un package d’application UWP avec VisualStudio](../packaging/packaging-uwp-apps.md).

Si vous avez créé des [versions d’évaluation de package](package-flights.md) pour votre application, une liste déroulante apparaît avec l’option de copie des packages de l’une des versions d’évaluation de package. Sélectionnez la version d’évaluation de package comportant les packages que vous souhaitez intégrer. Vous pouvez transférer une partie ou la totalité des packages dans cette soumission.

Si nous détectons des erreurs avec un package lors de la validation il, nous affichons un message pour vous indiquer quel est le problème. Vous devez supprimer le package et résoudre le problème puis essayez de le charger à nouveau. Il se peut également que vous receviez des avertissements concernant des problèmes potentiels, sans que cela vous empêche de poursuivre votre soumission.


## <a name="device-family-availability"></a>Disponibilité de la famille d’appareils

Une fois vos packages correctement chargés, la section **Disponibilité de la famille d’appareils** affiche un tableau identifiant les packages offerts aux familles d’appareilsWindows10 spécifiques (et aux versions antérieures du système d’exploitation), classés par ordre. Dans cette section, vous déterminez également si vous souhaitez offrir ou non la soumission aux clients sur des familles d’appareils Windows10 spécifiques.

Pour plus d’informations, consultez la section [Disponibilité de la famille d’appareils](device-family-availability.md).


## <a name="package-details"></a>Détails du package

Vos packages chargés sont répertoriées ici, regroupés par système d’exploitation cible. Nous affichons le nom, la version et l’architecture du package. Cliquez sur **Afficher les détails** pour visualiser des informations complémentaires comme les langues prises en charge, les fonctionnalités de l’application et la taille de fichier de chaque package.

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

 




