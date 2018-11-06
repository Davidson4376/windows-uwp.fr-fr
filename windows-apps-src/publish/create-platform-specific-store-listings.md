---
author: jnHs
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: Créer des descriptions dans le Store spécifiques à la plateforme
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp, personnaliser, description,version antérieure
ms.localizationpriority: medium
ms.openlocfilehash: c2bc58540f49afb8b24eaa58150132fea1d27f84
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6050686"
---
# <a name="create-platform-specific-store-listings"></a>Créer des descriptions dans le Store spécifiques à la plateforme


Si votre application publiée précédemment comporte des packages ciblant différents systèmes d’exploitation, vous avez la possibilité de personnaliser certaines parties de votre description dans le Windows Store pour les clients utilisant des versions antérieures du système d’exploitation (Windows 8.x ou version antérieure et/ou Windows Phone 8.x ou version antérieure). 

Les clients sur Windows 10 (y compris Xbox) voient toujours la valeur par défaut [description dans le Store](create-app-store-listings.md). Vous ne verrez pas la possibilité de créer des descriptions spécifiques à la plateforme, sauf si vous avez déjà publié votre application avec des packages prenant en charge une ou plusieurs versions du système d’exploitation antérieures. 

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créés ne peuvent pas inclure des packages ciblant 8.x/Windows Windows Phone 8.x ou version antérieure. Pour plus d’informations, consultez le [billet de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Descriptions du Windows Store spécifique à la plateforme peut être utile si vous voulez mentionner des fonctionnalités qui apparaissent uniquement dans une version de système d’exploitation, ou souhaitez proposer des captures d’écran spécifiques à un système d’exploitation particulier (indépendamment du type de périphérique).

> [!NOTE]
> La création d’une description dans le Store spécifique à la plateforme dans une langue donnée n’entraîne pas la création de cette description dans toutes les autres langues prises en charge par votre application, même si vous avez saisi les informations de description dans le Store associées à ces langues. Il vous faudra créer la description dans le Windows Store spécifique à la plateforme pour chacune des langues. Notez également que vous ne pouvez pas [Importer et exporter des données de description dans le Windows Store](import-and-export-store-listings.md) pour les annonces spécifiques à la plateforme.


## <a name="creating-a-platform-specific-store-listing"></a>Création d’une description dans le WindowsStore spécifique à la plateforme

En haut de votre page de **description dans le Store** , si votre application publiée précédemment comprend des packages qui prennent en charge les versions antérieures du système d’exploitation ((Windows 8.x ou version antérieure et/ou Windows Phone 8.x ou version antérieure), vous pouvez sélectionner **créer une description spécifique à la plateforme **. Une fois cette option sélectionnée, vous devrez choisir parmi les versions du système d’exploitation ciblées que votre soumission prend en charge. Une fois que vous avez déjà créé des descriptions spécifiques à la plateforme pour toutes les versions antérieures du système d’exploitation ciblées par votre application, il se peut que vous ne pourrez pas effectuer une autre sélection.

Vous pouvez utiliser votre description par défaut (Windows 10) en tant que point de départ, ce qui réaffichera le texte applicable et les images que vous avez saisi pour votre banque par défaut de description; vous serez alors en mesure d’apporter des modifications que vous souhaitez obtenir avant d’enregistrer. Vous pouvez également choisir de partir d’une description vide.

Une fois que vous aurez cliqué sur **Continuer**, votre page **Description dans le Store** inclura une section pour la description spécifique à la plateforme que vous venez de créer. Cette section inclut son propre ensemble de champs pour **Description** (obligatoire), **Nouveautés de cette version**, **Captures d’écran**, **Icône de vignette de l’application**, **Fonctionnalités de l’application** et **Configuration système supplémentaire requise**. Veillez à entrer des informations dans chaque champ où vous souhaitez afficher des informations pour la description personnalisée dans le Windows Store, même si ces informations sont identiques à celles de la description par défaut. Si vous laissez un de ces champs vide, aucune information ne s’affiche pour ce champ dans la description personnalisée dans le Windows Store.

> [!IMPORTANT]
> Les champs de la section [Informations supplémentaires](create-app-store-listings.md#additional-information) de la description dans le Store ne peuvent pas être personnalisés pour différentes versions du système d’exploitation.
> 
> En outre, étant donné que certains champs de la page [Description dans le Store](create-app-store-listings.md) par défaut s’appliquent uniquement aux clients sous Windows10, vous ne verrez pas toutes les options lors de la création d’une description dans le Store spécifique à la plateforme. Par exemple, vous ne pouvez pas ajouter de bandes-annonces à une description dans le Store spécifique à la plateforme, car les bandes-annonces s'affichent uniquement pour les clients sur Windows10, version1607 ou ultérieure. 

Vous pouvez continuer à modifier des descriptions spécifiques à la plateforme selon les besoins pour apporter des modifications pour les clients utilisant une version particulière du système d’exploitation.


## <a name="removing-a-platform-specific-store-listing"></a>Suppression d’une description dans le Store spécifique à la plateforme

Si vous créez une description dans le Store spécifique à la plateforme, puis décidez d’afficher plutôt votre description par défaut aux clients sur ce système d’exploitation, sélectionnez le lien **Supprimer** devant cette description.

Après avoir confirmé la présentation de la description par défaut dans le Store à ces clients, sélectionnez **OK**. La description dans le Store spécifique à la plateforme sera supprimée (pour toutes les langues dans lesquelles elle existait) et les clients exécutant cette version du système d’exploitation verront désormais votre description par défaut. Si vous changez d'avis, vous pouvez créer une autre description dans le Store spécifique à la plateforme pour ce système d’exploitation en suivant les étapes indiquées ci-dessus.
 

 




