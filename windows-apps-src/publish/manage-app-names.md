---
Description: Dans le tableau de bord du Centre de développement Windows, vous pouvez, pour chacune de vos applications, visualiser tous les noms que vous avez réservés pour votre application, réserver des noms supplémentaires et supprimer les noms inutiles à partir de la page Gérer les noms d’application de la section Gestion des applications.
title: Gérer les noms d’application
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
---

# Gérer les noms d’application


Dans le tableau de bord du Centre de développement Windows, vous pouvez, pour chacune de vos applications, visualiser tous les noms que vous avez réservés pour votre application, réserver des noms supplémentaires et supprimer les noms inutiles à partir de la page **Gérer les noms d'application** de la section **Gestion des applications**.

## Réserver des noms supplémentaires pour votre application


Vous pouvez réserver plusieurs noms pour une même application. Cette fonctionnalité est particulièrement utile si vous proposez une application en différentes langues pour lesquelles vous souhaitez utiliser des noms d'application distincts. Vous pouvez également utiliser cette fonction pour modifier le nom d'une application que vous n'avez pas encore publiée.

La section **Réserver d’autres noms** de la page **Gérer les noms d’application** comporte une zone de texte. Entrez dans cette zone le nom que vous souhaitez réserver, puis cliquez sur **Vérifier la disponibilité**. Si le nom indiqué est disponible, cliquez sur **Réserver le nom**.

> **Remarque** Pour plus d’informations sur la réservation de noms d’application et sur les raisons pour lesquelles un nom peut ne pas être disponible, voir [Créer votre application en réservant un nom](create-your-app-by-reserving-a-name.md).

Si vous le souhaitez, vous pouvez continuer à réserver des noms d’application supplémentaires sur cette page.

## Supprimer des noms d'application


Si vous ne voulez plus utiliser un nom que vous avez réservé précédemment, vous pouvez le libérer en le supprimant depuis cette page. Procédez avec précaution, car le nom que vous supprimez deviendra immédiatement utilisable par toute autre personne.

Pour supprimer l’un des noms réservés de votre application, recherchez ce dernier, puis cliquez sur **Supprimer**. Dans la boîte de dialogue de confirmation, cliquez de nouveau sur **Supprimer** pour confirmer l'opération.

Notez que votre application doit comporter au moins un nom réservé. Si vous souhaitez supprimer définitivement une application de votre tableau de bord (et libérer ainsi tous les noms que vous avez réservés pour cette dernière), cliquez sur **Supprimer cette application** sur la page **Vue d'ensemble**.

## Renommer une application qui a déjà été publiée


Si votre application figure déjà dans le Windows Store et que vous voulez la renommer, vous devez lui réserver un nouveau nom (en suivant la procédure décrite ci-dessus), puis créer une autre soumission pour l'application. Notez que vous devrez mettre à jour votre package en y incluant le nouveau nom afin que le Windows Store affiche l'application sous ce nom. Veillez à utiliser le nouveau nom dans l'élément [**Package/Properties/DisplayName**](https://msdn.microsoft.com/library/windows/apps/dn423240) du manifeste de l'application et à mettre à jour l'ensemble des graphiques ou textes contenant le nom de l'application. Vous devrez également revoir la description de l'application et modifier les éventuelles occurrences de l'ancien nom dans le texte descriptif.

Une fois votre application publiée sous le nouveau nom, vous pourrez supprimer l'ancien nom que vous n'utilisez plus.

 

 






<!--HONumber=Mar16_HO1-->


