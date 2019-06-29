---
Description: Si vous avez fourni des packages ciblant différents systèmes d’exploitation, vous pouvez personnaliser certaines parties de votre description dans le Windows Store pour ces différents systèmes.
title: Création d’annonces Windows Store spécifiques à la plateforme
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, personnaliser, description,version antérieure
ms.localizationpriority: medium
ms.openlocfilehash: 2d9a9e86397ca5e85697543f99481b43f438a063
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468966"
---
# <a name="create-platform-specific-store-listings"></a>Création d’annonces Windows Store spécifiques à la plateforme


Si votre application publiée précédemment comporte des packages qui ciblent différents systèmes d’exploitation, vous avez la possibilité de personnaliser les parties de votre annonce Store pour les clients sur des versions antérieures du système d’exploitation (Windows 8.x ou une version antérieure et/ou de Windows Phone 8.x ou une version antérieure). 

Les clients sur Windows 10 (y compris Xbox) seront affiche toujours la valeur par défaut [annonce de Store](create-app-store-listings.md). Vous ne voyez pas l’option de création de listes de Store spécifique à la plateforme, sauf si vous avez déjà publié votre application avec les packages qui prennent en charge une ou plusieurs versions du système d’exploitation antérieures. 

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant 8.x/Windows de Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Les listes spécifiques à la plateforme Store peuvent être utiles si vous souhaitez mentionner les fonctionnalités qui s’affichent uniquement dans une version de système d’exploitation, ou que vous souhaitez fournir des captures d’écran qui sont spécifiques à un système d’exploitation spécifique (indépendamment du type d’appareil).

> [!NOTE]
> Spécifique à la plateforme de création Store listing dans un langage ne crée pas les obtenir une liste spécifique à la plateforme Store dans d’autres langages prenant en charge votre application. Il vous faudra créer la description dans le Windows Store spécifique à la plateforme pour chacune des langues. Notez également que vous ne pouvez pas [importer et exporter Store listing données](import-and-export-store-listings.md) pour les listes spécifiques à la plateforme.


## <a name="creating-a-platform-specific-store-listing"></a>Création d’une description dans le Windows Store spécifique à la plateforme

Dans la partie supérieure de votre **annonce de Store** page, si votre application publiée précédemment inclut les packages qui prennent en charge les versions antérieures du système d’exploitation ((Windows 8.x ou une version antérieure et/ou de Windows Phone 8.x ou une version antérieure), vous pouvez sélectionner **créer un la liste spécifique à la plateforme app Store**. Une fois cette option sélectionnée, vous devrez choisir parmi les versions du système d’exploitation ciblées que votre soumission prend en charge. Une fois que vous avez déjà créé des listes de Store spécifique à la plateforme pour toutes les versions antérieures du système d’exploitation ciblée par votre application, vous ne pourrez pas effectuer une autre sélection.

Vous pouvez utiliser votre annonce de Store par défaut (Windows 10) comme point de départ, s’affiche sur le texte applicable et les images que vous avez entré pour votre Store par défaut répertoriant ; vous serez en mesure d’apporter des modifications que vous souhaitez qu’avant l’enregistrement. Vous pouvez également choisir de partir d’une description vide.

Une fois que vous aurez cliqué sur **Continuer**, votre page **Description dans le Store** inclura une section pour la description spécifique à la plateforme que vous venez de créer. Cette section inclut son propre ensemble de champs pour **Description** (obligatoire), **Nouveautés de cette version**, **Captures d’écran**, **Icône de vignette de l’application**, **Fonctionnalités de l’application** et **Configuration système supplémentaire requise**. Veillez à entrer des informations dans chaque champ où vous souhaitez afficher des informations pour la description personnalisée dans le Windows Store, même si ces informations sont identiques à celles de la description par défaut. Si vous laissez un de ces champs vide, aucune information ne s’affiche pour ce champ dans la description personnalisée dans le Windows Store.

> [!IMPORTANT]
> Les champs de la section [Informations supplémentaires](create-app-store-listings.md#additional-information) de la description dans le Store ne peuvent pas être personnalisés pour différentes versions du système d’exploitation.
> 
> En outre, étant donné que certains champs de la page [Description dans le Store](create-app-store-listings.md) par défaut s’appliquent uniquement aux clients sous Windows 10, vous ne verrez pas toutes les options lors de la création d’une description dans le Store spécifique à la plateforme. Par exemple, vous ne pouvez pas ajouter de bandes-annonces à une description dans le Store spécifique à la plateforme, car les bandes-annonces s'affichent uniquement pour les clients sur Windows 10, version 1607 ou ultérieure. 

Vous pouvez continuer à modifier les listes spécifiques à la plateforme en fonction des besoins pour apporter des modifications pour les clients sur une version spécifique du système d’exploitation.


## <a name="removing-a-platform-specific-store-listing"></a>Suppression d’une description dans le Windows Store spécifique à la plateforme

Si vous créez une description dans le Store spécifique à la plateforme, puis décidez d’afficher plutôt votre description par défaut aux clients sur ce système d’exploitation, sélectionnez le lien **Supprimer** devant cette description.

Après avoir confirmé la présentation de la description par défaut dans le Store à ces clients, sélectionnez **OK**. La description dans le Store spécifique à la plateforme sera supprimée (pour toutes les langues dans lesquelles elle existait) et les clients exécutant cette version du système d’exploitation verront désormais votre description par défaut. Si vous changez d'avis, vous pouvez créer une autre description dans le Store spécifique à la plateforme pour ce système d’exploitation en suivant les étapes indiquées ci-dessus.
 

 




