---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: Gestion des noms d’application
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, noms d’application, modifiez le nom de l’application, nom de l’application mise à jour, jeu, nom de produit
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4060189"
---
# <a name="manage-app-names"></a>Gestion des noms d’application

Le permet de **gérer des noms d’application** vous permet d’afficher tous les noms que vous avez réservés pour votre application, réserver des noms supplémentaires (pour d’autres langues ou pour modifier le nom de votre application) et supprimer des noms que vous n’avez pas besoin. Vous trouverez cette page du tableau de [bord du centre de développement Windows](https://partner.microsoft.com/dashboard) , développez la section **Gestion des applications** dans le menu de navigation de gauche pour vos applications.


## <a name="reserve-additional-names-for-your-app"></a>Réserver des noms supplémentaires pour votre application

Vous pouvez réserver plusieurs noms pour une même application. Cette fonctionnalité est particulièrement utile si vous proposez une application en différentes langues pour lesquelles vous souhaitez utiliser des noms d'application distincts. Vous pouvez également réserver un nouveau nom afin de modifier le nom d’une application, comme décrit ci-dessous.

Pour réserver un nouveau nom de l’application, recherchez la zone de texte dans la section **réserver d’autres noms** de la page **gérer des noms d’application** . Entrez dans cette zone le nom que vous souhaitez réserver, puis cliquez sur **Vérifier la disponibilité**. Si le nom indiqué est disponible, cliquez sur **Reserve product name**. Vous pouvez réserver plusieurs noms d’application en répétant ces étapes, si vous le souhaitez.

> [!NOTE]
> Pour plus d’informations sur la réservation de noms d’application et sur les raisons pour lesquelles un nom peut ne pas être disponible, consultez l’article [Créer votre application en réservant un nom](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Supprimer des noms d'application

Si vous ne voulez plus utiliser un nom que vous avez réservé précédemment, vous pouvez le libérer en le supprimant depuis cette page. Procédez avec précaution, car le nom que vous supprimez deviendra immédiatement utilisable par toute autre personne.

Pour supprimer l’un des noms réservés de votre application, recherchez ce dernier, puis cliquez sur **Supprimer**. Dans la boîte de dialogue de confirmation, cliquez de nouveau sur **Supprimer** pour confirmer l'opération.

Notez que votre application doit disposer d’au moins un nom réservé. Pour supprimer complètement une application à partir de votre tableau de bord, (et libérer tous les noms que vous avez réservés pour cette application), cliquez sur **Supprimer cette application** à partir de la page de **Présentation de l’application** . Si une soumission de l’application est en cours, vous devez commencer par supprimer cette soumission. Notez que si vous avez déjà publié l’application au Windows Store, vous ne pouvez pas la supprimer de votre tableau de bord (Toutefois, vous pouvez utiliser la fonctionnalité **Afficher/masquer les produits** sur votre page de **présentation** pour masquer l’application). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renommer une application qui a déjà été publiée

Si votre application figure déjà dans le WindowsStore et que vous voulez la renommer, vous devez lui réserver un nouveau nom (en suivant la procédure décrite ci-dessus), puis créer une autre soumission pour l’application. 

Vous devez mettre à jour ou les packages de votre application pour remplacer l’ancien nom par le nouveau et de charger l’ou les packages mis à jour à votre soumission.
- Tout d’abord, mettre à jour le fichier Package.StoreAssociation.xml pour utiliser le nouveau nom, soit manuellement, soit à l’aide de Visual Studio (**projet > Store > associer l’application avec le Windows Store …**). Pour plus d’informations, voir le [Package d’une application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md).
- Vous devez également mettre à jour l’élément [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) dans le manifeste de votre application, puis mettre à jour l’ensemble des graphiques ou textes contenant le nom de l’application. 
  > [!IMPORTANT]
  > Veillez à mettre à jour le fichier Package.StoreAssociation.xml avant de modifier l’élément **Package/Properties/DisplayName** dans le manifeste de l’application; dans le cas contraire, vous risquez d’obtenir une erreur.

Pour mettre à jour une description du Windows Store afin qu’il utilise le nouveau nom, accédez à la [page de description du Windows Store](create-app-store-listings.md) pour cette langue et sélectionnez le nom dans la liste déroulante **nom du produit** . Veillez à passer en revue votre description et les autres parties de la description pour toute mention du nom et de mettre à jour si nécessaire.

> [!NOTE]
> Si votre application comporte des packages et/ou les descriptions du Windows Store en plusieurs langues, vous devez mettre à jour les packages et/ou de descriptions pour chaque langue dans laquelle le nom doit être mis à jour du Windows Store.

Une fois que votre application a été publiée avec le nouveau nom, vous pouvez supprimer tous les noms plus anciens que vous n’avez plus besoin d’utiliser.

> [!TIP]
> Chaque application s’affiche dans votre tableau de bord à l’aide du premier nom que vous avez réservé pour elle. Si vous avez suivi les étapes ci-dessus pour renommer une application et que vous souhaitez voir apparaître dans votre tableau de bord en utilisant le nouveau nom, vous devez supprimer le nom d’origine (en cliquant sur **Supprimer** sur la page **gérer des noms d’application** ). 

 

 




