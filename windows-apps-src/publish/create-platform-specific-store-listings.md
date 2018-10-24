---
author: jnHs
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: Créer des descriptions dans le Store spécifiques à la plateforme
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 3/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, personnaliser, description,version antérieure
ms.localizationpriority: medium
ms.openlocfilehash: 6f30a825cc7aec1b6f7dbf5cff0ea1c17c43ffd7
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5478391"
---
# <a name="create-platform-specific-store-listings"></a>Créer des descriptions dans le Store spécifiques à la plateforme


Si votre application comporte des packages ciblant différents systèmes d’exploitation, vous pouvez personnaliser certaines parties de votre description dans le Store pour les clients exécutant des versions antérieures du système d’exploitation (Windows8.x ou version antérieure et/ou Windows Phone8.x ou version antérieure). 

> [!IMPORTANT]
> Les clients sur Windows 10 voient toujours la valeur par défaut [description dans le Store](create-app-store-listings.md). Vous ne verrez pas la possibilité de créer des descriptions dans le Store spécifiques à la plateforme, sauf si vous avez déjà chargé des packages qui prennent en charge une ou plusieurs versions antérieures du système d’exploitation. 

Les descriptions dans le Windows Store spécifiques à la plateforme peuvent être utiles si vous voulez mentionner des fonctionnalités qui apparaissent uniquement dans une version de système d’exploitation, ou fournir des captures d’écran spécifiques d’un système d’exploitation particulier (indépendamment du type d’appareil), au lieu que tous les clients voient la même description.

> [!NOTE]
> La création d’une description dans le Store spécifique à la plateforme dans une langue donnée n’entraîne pas la création de cette description dans toutes les autres langues prises en charge par votre application, même si vous avez saisi les informations de description dans le Store associées à ces langues. Il vous faudra créer la description dans le Windows Store spécifique à la plateforme pour chacune des langues. Notez également que vous ne pouvez pas importer et exporter des données de description dans le Store pour les annonces spécifiques à la plateforme d’annonce.


## <a name="creating-a-platform-specific-store-listing"></a>Création d’une description dans le WindowsStore spécifique à la plateforme

Dans la partie supérieure de votre page **Description dans le Store**, si votre application prend en charge des versions antérieures du système d’exploitation ((Windows8.x ou version antérieure et/ou Windows Phone8.x ou version antérieure), vous pouvez choisir **Créer une description dans le Store d'applications spécifique à la plateforme**. 

> [!TIP]
> Vous ne verrez l'option permettant de créer des descriptions dans le Store spécifiques à la plateforme qu'après avoir chargé les packages.

Une fois cette option sélectionnée, vous devrez choisir parmi les versions du système d’exploitation ciblées que votre soumission prend en charge. Puisque vous avez déjà créé des descriptions dans le Store spécifiques à la plateforme pour toutes les versions de système d’exploitation ciblées par votre application, vous ne pourrez pas effectuer d'autre sélection. (Windows 10 n’est pas inclus dans la liste des choix, car les clients sur Windows 10 voient toujours de l’application du Windows Store description par défaut.)

Vous pouvez choisir d’utiliser votre description dans le Store par défaut comme point de départ, ce qui réaffichera le texte et les images applicables que vous avez entrés pour cette description; vous serez alors en mesure de modifier ces éléments à votre convenance avant d’enregistrer ces modifications. Vous pouvez également choisir de partir d’une description vide.

Une fois que vous aurez cliqué sur **Continuer**, votre page **Description dans le Store** inclura une section pour la description spécifique à la plateforme que vous venez de créer. Cette section inclut son propre ensemble de champs pour **Description** (obligatoire), **Nouveautés de cette version**, **Captures d’écran**, **Icône de vignette de l’application**, **Fonctionnalités de l’application** et **Configuration système supplémentaire requise**. Veillez à entrer des informations dans chaque champ où vous souhaitez afficher des informations pour la description personnalisée dans le Windows Store, même si ces informations sont identiques à celles de la description par défaut. Si vous laissez un de ces champs vide, aucune information ne s’affiche pour ce champ dans la description personnalisée dans le Windows Store.


> [!IMPORTANT]
> Les champs de la section [Informations supplémentaires](create-app-store-listings.md#additional-information) de la description dans le Store ne peuvent pas être personnalisés pour différentes versions du système d’exploitation.
> 
> En outre, étant donné que certains champs de la page [Description dans le Store](create-app-store-listings.md) par défaut s’appliquent uniquement aux clients sous Windows10, vous ne verrez pas toutes les options lors de la création d’une description dans le Store spécifique à la plateforme. Par exemple, vous ne pouvez pas ajouter de bandes-annonces à une description dans le Store spécifique à la plateforme, car les bandes-annonces s'affichent uniquement pour les clients sur Windows10, version1607 ou ultérieure. 


## <a name="removing-a-platform-specific-store-listing"></a>Suppression d’une description dans le Store spécifique à la plateforme

Si vous créez une description dans le Store spécifique à la plateforme, puis décidez d’afficher plutôt votre description par défaut aux clients sur ce système d’exploitation, sélectionnez le lien **Supprimer** devant cette description.

Après avoir confirmé la présentation de la description par défaut dans le Store à ces clients, sélectionnez **OK**. La description dans le Store spécifique à la plateforme sera supprimée (pour toutes les langues dans lesquelles elle existait) et les clients exécutant cette version du système d’exploitation verront désormais votre description par défaut. Si vous changez d'avis, vous pouvez créer une autre description dans le Store spécifique à la plateforme pour ce système d’exploitation en suivant les étapes indiquées ci-dessus.

 

 




