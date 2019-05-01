---
Description: Affichez les noms réservés pour l’app, réservez d’autres noms (pour d’autres langues ou pour changer le nom de l’app) et supprimez les noms réservés inutiles.
title: Gestion des noms d’application
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, les noms d’application, modifiez le nom de l’application, nom de l’application mise à jour, nom de la partie nom de produit
ms.localizationpriority: medium
ms.openlocfilehash: 786bfd7cbb4129274a2ff19bc33084824d296bcd
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63827526"
---
# <a name="manage-app-names"></a>Gestion des noms d’application

Le **gérer les noms de l’application** vous permet d’afficher tous les noms que vous avez réservés pour votre application, de réserver des noms supplémentaires (pour les autres langages ou pour modifier le nom de votre application) et supprimer des noms que vous n’avez pas besoin. Vous pouvez trouver cette page dans [partenaires](https://partner.microsoft.com/dashboard) en développant le **gestion des applications** section dans le menu de navigation de gauche pour toutes vos applications.

> [!IMPORTANT]
> Vous pouvez réserver des noms supplémentaires pour une application, et vous pouvez choisir d’utiliser une de celles de la version publiée de votre application au lieu de celle que vous réservé lorsque vous avez créé votre application dans le centre de partenaires. Toutefois, n’oubliez pas que le premier nom que vous réservez pour votre produit servira dans certaines de votre service informatique de [détails de l’identité](view-app-identity-details.md), telles que la **nom de famille de packages (NFP)**. Ces valeurs peuvent être visibles à certains utilisateurs et ne peut pas être modifié, par conséquent, assurez-vous que le nom que vous réserver tout d’abord est approprié pour cette utilisation.


## <a name="reserve-additional-names-for-your-app"></a>Réserver des noms supplémentaires pour votre application

Vous pouvez réserver plusieurs noms pour une même application. Cette fonctionnalité est particulièrement utile si vous proposez une application en différentes langues pour lesquelles vous souhaitez utiliser des noms d'application distincts. Vous pouvez également réserver un nouveau nom afin de modifier le nom d’une application, comme décrit ci-dessous.

Pour réserver un nouveau nom d’application, recherchez la zone de texte dans le **réserver plusieurs noms** section de la **gérer les noms de l’application** page. Entrez dans cette zone le nom que vous souhaitez réserver, puis cliquez sur **Vérifier la disponibilité**. Si le nom indiqué est disponible, cliquez sur **Reserve product name**. Vous pouvez réserver plusieurs noms d’application en répétant cette procédure, si vous le souhaitez.

> [!NOTE]
> Pour plus d’informations sur la réservation de noms d’application et sur les raisons pour lesquelles un nom peut ne pas être disponible, consultez l’article [Créer votre application en réservant un nom](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Supprimer des noms d'application

Si vous ne voulez plus utiliser un nom que vous avez réservé précédemment, vous pouvez le libérer en le supprimant depuis cette page. Procédez avec précaution, car le nom que vous supprimez deviendra immédiatement utilisable par toute autre personne.

Pour supprimer l’un des noms réservés de votre application, recherchez ce dernier, puis cliquez sur **Supprimer**. Dans la boîte de dialogue de confirmation, cliquez de nouveau sur **Supprimer** pour confirmer l'opération.

Notez que votre application doit avoir au moins un nom réservé. Pour supprimer complètement une application à partir du centre de partenaires (et libérer tous les noms que vous avez réservés pour cette application), cliquez sur **supprimer cette application** à partir de la **vue d’ensemble de l’application** page. Si une soumission de l’application est en cours, vous devez commencer par supprimer cette soumission. Notez que si vous avez déjà publié l’application sur le Store, vous ne pouvez pas le supprimer à partir du centre de partenaires (bien que vous pouvez utiliser la **afficher/masquer les produits** fonctionnalité sur votre **vue d’ensemble** page pour la masquer). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renommer une application qui a déjà été publiée

Si votre application figure déjà dans le Windows Store et que vous voulez la renommer, vous devez lui réserver un nouveau nom (en suivant la procédure décrite ci-dessus), puis créer une autre soumission pour l’application. 

Vous devez mettre à jour de package (s) de votre application pour remplacer l’ancien nom par le nouveau et de charger l’ou les packages mis à jour dans votre envoi.
- Commencez par mettre à jour le fichier Package.StoreAssociation.xml en vue d’utiliser le nouveau nom, soit manuellement, soit à l’aide de Visual Studio (**Projet > Store > Associer l’application au Windows Store...**). Pour plus d’informations, consultez [empaqueter une application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md).
- Vous devez également mettre à jour l’élément [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) dans le manifeste de votre application, puis mettre à jour l’ensemble des graphiques ou textes contenant le nom de l’application. 
  > [!IMPORTANT]
  > Veillez à mettre à jour le fichier Package.StoreAssociation.xml avant de modifier l’élément **Package/Properties/DisplayName** dans le manifeste de l’application ; dans le cas contraire, vous risquez d’obtenir une erreur.

Pour mettre à jour une liste de Store afin qu’elle utilise le nouveau nom, accédez à la [page d’annonce Store](create-app-store-listings.md) pour ce langage et sélectionnez le nom de la **Product name** liste déroulante. Veillez à consulter votre description et autres parties de la liste pour les mentions du nom et de mettre à jour si nécessaire.

> [!NOTE]
> Si votre application possède des packages et/ou d’annonces de Store dans plusieurs langues, vous devez mettre à jour les packages et/ou Store annonces pour chaque langue dans laquelle le nom doit être mis à jour.

Une fois que votre application a été publiée avec le nouveau nom, vous pouvez supprimer tous les noms plus anciens que vous n’avez plus besoin d’utiliser.

> [!TIP]
> Chaque application s’affiche dans les partenaires en utilisant le nom de la première qui vous réservé pour celui-ci. Si vous avez suivi les étapes ci-dessus pour renommer une application, et qu’il apparaisse dans l’espace partenaires en utilisant le nouveau nom, vous devez supprimer le nom d’origine (en cliquant sur **supprimer** sur le **gérer les noms de l’application** page). 

 

 




