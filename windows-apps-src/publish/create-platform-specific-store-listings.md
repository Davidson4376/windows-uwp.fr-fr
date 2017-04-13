---
author: jnHs
Description: "Si vous avez fourni des packages ciblant différents systèmes d’exploitation, vous pouvez personnaliser certaines parties de votre description dans le Windows Store pour ces différents systèmes."
title: "Créer des descriptions dans le WindowsStore spécifiques à la plateforme"
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 4a4aac5c5990807f2988be1ba818992b1bccbea3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-platform-specific-store-listings"></a>Créer des descriptions dans le WindowsStore spécifiques à la plateforme


Si vous avez fourni des packages ciblant différents systèmes d’exploitation, vous pouvez personnaliser certaines parties de votre description dans le Windows Store pour ces différents systèmes. Vous ne voyez cette option que si vous avez déjà chargé les packages et intégré des packages pour les versions antérieures du système d’exploitation (Windows8.x ou version antérieure et/ou Windows Phone8.x ou version antérieure).

> **Important** Les clients sur Windows10 voient toujours votre description dans le Windows Store par défaut.

Les descriptions dans le Windows Store spécifiques à la plateforme peuvent être utiles si vous voulez mentionner des fonctionnalités qui apparaissent uniquement dans une version de système d’exploitation, ou fournir des captures d’écran spécifiques d’un système d’exploitation particulier (indépendamment du type d’appareil), au lieu que tous les clients voient la même description.

> **Remarque** La création d’une description dans le Windows Store spécifique à la plateforme dans une langue donnée n’entraîne pas la création de cette description dans toutes les autres langues prises en charge par votre application, même si vous avez [saisi les informations de description dans le Windows Store](create-app-store-listings.md) associées à ces langues. Il vous faudra créer la description dans le Windows Store spécifique à la plateforme pour chacune des langues.

## <a name="creating-a-platform-specific-store-listing"></a>Création d’une description dans le WindowsStore spécifique à la plateforme

Dans la partie supérieure de votre page **Description dans le Windows Store**, vous pouvez voir un lien vers **créer une description dans le Windows Store spécifique à la plateforme** si vous avez chargé des packages prenant en charge des versions antérieures du système d’exploitation ((Windows8.x ou version antérieure et/ou Windows Phone8.x ou version antérieure).

> **Conseil** Vous ne pouvez pas créer des descriptions dans le Windows Store spécifiques à la plateforme avant d’avoir chargé les packages.

Lorsque vous cliquez sur ce lien pour **créer un description de l’application dans le Windows Store spécifique à la plateforme**, une fenêtre contextuelle s’affiche, vous demandant de choisir parmi les systèmes d’exploitation ciblés pris en charge. Windows10 n’est pas inclus dans la liste des choix, car les clients sur Windows10 voient toujours la description par défaut de l’application dans le Windows Store.

> **Remarque** Si vous avez déjà créé des descriptions dans le Windows Store spécifiques à la plateforme pour toutes les versions de système d’exploitation ciblées par votre application, vous ne pourrez pas effectuer de sélection.

Vous pouvez choisir d’utiliser votre description par défaut dans le Windows Store comme point de départ, ce qui réaffichera l’ensemble du texte et des images que vous avez entrés pour cette description; vous serez alors en mesure de modifier ces éléments à votre convenance avant d’enregistrer ces modifications. Vous pouvez également choisir de partir d’une description vide.

Une fois que vous aurez cliqué sur **Continuer**, votre page **Description dans le Windows Store** inclura une section pour la description spécifique à la plateforme que vous venez de créer (sous la section [Description par défaut dans le Windows Store](create-app-store-listings.md#default-store-listing-fields)). Cette section inclut son propre ensemble de champs pour **Description** (obligatoire), **Notes de publication**, **Captures d’écran**, **Icône de vignette de l’application**, **Fonctionnalités de l’application** et **Configuration système supplémentaire requise**. Veillez à entrer des informations dans chaque champ où vous souhaitez afficher des informations pour la description personnalisée dans le Windows Store, même si ces informations sont identiques à celles de la description par défaut. Si vous laissez un de ces champs vide, aucune information ne s’affiche pour ce champ dans la description personnalisée dans le Windows Store.

> **Remarque** Vous ne pouvez pas personnaliser tous les champs de la description de votre application dans le Windows Store pour un système d’exploitation ciblé. Pour plus d’informations, voir l’article [Champs partagés](create-app-store-listings.md#shared-fields).

## <a name="removing-a-platform-specific-store-listing"></a>Suppression d’une description dans le WindowsStore spécifique à la plateforme

Si vous créez une description dans le Windows Store spécifique à la plateforme, puis décidez d’afficher plutôt votre description par défaut aux clients sur ce système d’exploitation, cliquez sur le lien **Supprimer**. Après avoir confirmé la présentation de la description par défaut dans le Windows Store à ces clients, cliquez sur **OK**. La description dans le Windows Store spécifique à la plateforme sera supprimée. Si vous décidez de l’afficher à nouveau, vous pouvez toujours créer une nouvelle description dans le Windows Store spécifique à la plateforme pour ce système d’exploitation en suivant les étapes indiquées ci-dessus.

 

 




