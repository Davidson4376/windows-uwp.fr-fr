---
author: jnHs
Description: "Affichez les noms réservés pour l’app, réservez d’autres noms (pour d’autres langues ou pour changer le nom de l’app) et supprimez les noms réservés inutiles."
title: "Gestion des noms d’application"
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 539ccc671ae3f66c6c17a077ac1c09d989d86fdc
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2017
---
# <a name="manage-app-names"></a>Gestion des noms d’application


Vous pouvez afficher les noms que vous avez réservés pour votre application, réserver des noms supplémentaires (pour d’autres langues ou pour modifier le nom de votre application) et supprimer les noms dont vous n’avez pas besoin. Pour ce faire, accédez à la page **Gérer les noms d’application** dans la section **Gestion des applications** pour chaque application figurant dans le tableau de bord du Centre de développement Windows.

## <a name="reserve-additional-names-for-your-app"></a>Réserver des noms supplémentaires pour votre application

Vous pouvez réserver plusieurs noms pour une même application. Cette fonctionnalité est particulièrement utile si vous proposez une application en différentes langues pour lesquelles vous souhaitez utiliser des noms d'application distincts. Vous pouvez également réserver un nouveau nom afin de modifier le nom d’une application.

La section **Réserver d’autres noms** de la page **Gérer les noms d’application** comporte une zone de texte. Entrez dans cette zone le nom que vous souhaitez réserver, puis cliquez sur **Vérifier la disponibilité**. Si le nom indiqué est disponible, cliquez sur **Reserve product name**.

> [!NOTE]
> Pour plus d’informations sur la réservation de noms d’application et sur les raisons pour lesquelles un nom peut ne pas être disponible, consultez l’article [Créer votre application en réservant un nom](create-your-app-by-reserving-a-name.md).

Si vous le souhaitez, vous pouvez continuer à réserver des noms d’application supplémentaires sur cette page.

## <a name="delete-app-names"></a>Supprimer des noms d'application

Si vous ne voulez plus utiliser un nom que vous avez réservé précédemment, vous pouvez le libérer en le supprimant depuis cette page. Procédez avec précaution, car le nom que vous supprimez deviendra immédiatement utilisable par toute autre personne.

Pour supprimer l’un des noms réservés de votre application, recherchez ce dernier, puis cliquez sur **Supprimer**. Dans la boîte de dialogue de confirmation, cliquez de nouveau sur **Supprimer** pour confirmer l'opération.

Notez que votre application doit comporter au moins un nom réservé. Si vous souhaitez supprimer définitivement une application de votre tableau de bord (et libérer tous les noms que vous avez réservés pour cette dernière), cliquez sur **Supprimer cette application** sur la page **Vue d’ensemble de l’application**. Si une soumission de l’application est en cours, vous devez commencer par supprimer cette soumission. Si vous avez déjà publié l’application sur le WindowsStore, vous ne pouvez pas la supprimer de votre tableau de bord (toutefois, vous pouvez utiliser la fonctionnalité **Afficher/Masquer les produits** sur votre page **Vue d’ensemble** pour masquer l’application). 

## <a name="rename-an-app-that-has-already-been-published"></a>Renommer une application qui a déjà été publiée

Si votre application figure déjà dans le WindowsStore et que vous voulez la renommer, vous devez lui réserver un nouveau nom (en suivant la procédure décrite ci-dessus), puis créer une autre soumission pour l’application.

Vous devez mettre à jour le ou les packages de votre application en y incluant le nouveau nom afin que le WindowsStore affiche l’application sous ce nom. Commencez par mettre à jour le fichier Package.StoreAssociation.xml en vue d’utiliser le nouveau nom, soit manuellement, soit à l’aide de VisualStudio (**Projet > Store > Associer l’application au WindowsStore...**). Pour plus d’informations, consultez l’article [Création de packages d’application UWP](../packaging/packaging-uwp-apps.md).

Vous devez également mettre à jour l’élément [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-displayname) dans le manifeste de votre application, puis mettre à jour l’ensemble des graphiques ou textes contenant le nom de l’application. 

> [!IMPORTANT]
> Veillez à mettre à jour le fichier Package.StoreAssociation.xml avant de modifier l’élément **Package/Properties/DisplayName** dans le manifeste de l’application; dans le cas contraire, vous risquez d’obtenir une erreur.

Vous devrez également examiner la description de l’application dans le WindowsStore et modifier le nom de l’application si vous la mentionnez à cet emplacement. Une fois votre application publiée sous le nouveau nom, vous pourrez supprimer l’ancien nom que vous n’utilisez plus.

 

 




