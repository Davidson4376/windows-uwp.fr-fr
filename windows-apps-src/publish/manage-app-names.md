---
Description: Affichez les noms réservés pour l’app, réservez d’autres noms (pour d’autres langues ou pour changer le nom de l’app) et supprimez les noms réservés inutiles.
title: Gestion des noms d’application
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, noms des applications, changer le nom de l’application, mettre à jour le nom de l’application, nom du jeu, nom du produit
ms.localizationpriority: medium
ms.openlocfilehash: 0022c53dc3afc7e710495900898d3fc5c81ea45a
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682686"
---
# <a name="manage-app-names"></a>Gestion des noms d’application

L’option **gérer les noms des applications** vous permet d’afficher tous les noms que vous avez réservés pour votre application, de réserver des noms supplémentaires (pour d’autres langues ou de modifier le nom de votre application) et de supprimer les noms dont vous n’avez pas besoin. Vous pouvez trouver cette page dans l' [espace partenaires](https://partner.microsoft.com/dashboard) en développant la section **gestion des applications** dans le menu de navigation gauche pour l’une de vos applications.

> [!IMPORTANT]
> Vous pouvez réserver des noms supplémentaires pour une application, et vous pouvez choisir d’utiliser l’un de ces noms dans la version publiée de votre application au lieu de celle que vous avez réservée lors de la création de votre application dans l’espace partenaires. Toutefois, sachez que le prénom que vous réservez pour votre produit sera utilisé dans certains de vos [Détails d’identité](view-app-identity-details.md), tels que le nom de la **famille de packages (PFN)** . Ces valeurs peuvent être visibles par certains utilisateurs et ne peuvent pas être modifiées. Assurez-vous que le nom que vous réservez en premier est approprié pour cette utilisation.


## <a name="reserve-additional-names-for-your-app"></a>Réserver des noms supplémentaires pour votre application

Vous pouvez réserver plusieurs noms pour une même application. Cette fonctionnalité est particulièrement utile si vous proposez une application en différentes langues pour lesquelles vous souhaitez utiliser des noms d'application distincts. Vous pouvez également réserver un nouveau nom afin de modifier le nom d’une application, comme décrit ci-dessous.

Pour réserver un nouveau nom d’application, recherchez la zone de texte dans la section **Réserver des noms supplémentaires** de la page **gérer les noms d’application** . Entrez dans cette zone le nom que vous souhaitez réserver, puis cliquez sur **Vérifier la disponibilité**. Si le nom indiqué est disponible, cliquez sur **Reserve product name**. Vous pouvez réserver plusieurs noms d’applications en répétant ces étapes, si vous le souhaitez.

> [!NOTE]
> Pour plus d’informations sur la réservation de noms d’application et sur les raisons pour lesquelles un nom peut ne pas être disponible, consultez l’article [Créer votre application en réservant un nom](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Supprimer des noms d'application

Si vous ne voulez plus utiliser un nom que vous avez réservé précédemment, vous pouvez le libérer en le supprimant depuis cette page. Procédez avec précaution, car le nom que vous supprimez deviendra immédiatement utilisable par toute autre personne.

Pour supprimer l’un des noms réservés de votre application, recherchez ce dernier, puis cliquez sur **Supprimer**. Dans la boîte de dialogue de confirmation, cliquez de nouveau sur **Supprimer** pour confirmer l'opération.

Notez que votre application doit avoir au moins un nom réservé. Pour supprimer complètement une application de l’espace partenaires (et libérer tous les noms que vous avez réservés pour cette application), cliquez sur **supprimer cette application** dans la page **vue d’ensemble** de l’application. Si une soumission de l’application est en cours, vous devez commencer par supprimer cette soumission. Notez que si vous avez déjà publié l’application dans le Windows Store, vous ne pouvez pas la supprimer de l’espace partenaires (même si vous pouvez utiliser la fonctionnalité **Afficher/masquer les produits** sur votre page de **vue d’ensemble** pour la masquer). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renommer une application qui a déjà été publiée

Si votre application figure déjà dans le Windows Store et que vous voulez la renommer, vous devez lui réserver un nouveau nom (en suivant la procédure décrite ci-dessus), puis créer une autre soumission pour l’application. 

Vous devez mettre à jour le ou les packages de votre application pour remplacer l’ancien nom par le nouveau et charger le ou les packages mis à jour dans votre envoi.
- Commencez par mettre à jour le fichier Package.StoreAssociation.xml en vue d’utiliser le nouveau nom, soit manuellement, soit à l’aide de Visual Studio (**Projet > Store > Associer l’application au Windows Store...** ). Pour plus d’informations, consultez empaqueter [une application UWP avec Visual Studio](/windows/msix/package/packaging-uwp-apps).
- Vous devez également mettre à jour l’élément [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) dans le manifeste de votre application, puis mettre à jour l’ensemble des graphiques ou textes contenant le nom de l’application. 
  > [!IMPORTANT]
  > Veillez à mettre à jour le fichier Package.StoreAssociation.xml avant de modifier l’élément **Package/Properties/DisplayName** dans le manifeste de l’application ; dans le cas contraire, vous risquez d’obtenir une erreur.

Pour mettre à jour un listing de magasin afin qu’il utilise le nouveau nom, accédez à la [page de liste](create-app-store-listings.md) de la boutique pour cette langue et sélectionnez le nom dans la liste déroulante **nom du produit** . Veillez à passer en revue votre description et les autres parties de la liste pour connaître les mentions du nom et effectuer les mises à jour si nécessaire.

> [!NOTE]
> Si votre application comporte des packages et/ou des listes de magasins dans plusieurs langues, vous devez mettre à jour les packages et/ou stocker les listes pour chaque langue dans laquelle le nom doit être mis à jour.

Une fois votre application publiée avec le nouveau nom, vous pouvez supprimer tous les anciens noms que vous n’avez plus besoin d’utiliser.

> [!TIP]
> Chaque application s’affiche dans l’espace partenaires à l’aide du premier nom que vous avez réservé. Si vous avez suivi les étapes ci-dessus pour renommer une application et que vous souhaitez qu’elle s’affiche dans l’espace partenaires à l’aide du nouveau nom, vous devez supprimer le nom d’origine (en cliquant sur **supprimer** dans la page **gérer les noms des applications** ). 

 

 




