---
author: jnHs
Description: Vous pouvez aider les clients à découvrir votre application en créant un lien vers la description de votre application dans le Windows Store.
title: Créer un lien vers votre application
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
---

# Créer un lien vers votre application


Vous pouvez aider les clients à découvrir votre application en créant un lien vers la description de votre application dans le Store.

## Obtention du lien vers la description de votre application dans le Store


Vous pouvez trouver le lien vers la description de votre application dans le Windows Store sur la page [Identité de l’application](view-app-identity-details.md), dans la section **Gestion de l’application** pour chaque application répertoriée dans votre tableau de bord.

Ce lien est au format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Lorsqu’un client clique sur ce lien, il accède automatiquement à la page de description web de votre application. Si l’appareil du client prend en charge votre application, l’application du Windows Store lancera et affichera également la description de votre application.

> **Remarque** Selon les versions de système d’exploitation que vous ciblez, vous pouvez voir plusieurs liens ici. Toutes les applications présentent l’URL pour Windows 10, qui fonctionne pour tout système d’exploitation. Vous pouvez voir des liens supplémentaires pour Windows 8.1 et versions antérieures et/ou Windows Phone 8.1 et versions antérieures, qui ne fonctionnent que sur les versions de système d’exploitation spécifiées.

 

## Créer un lien vers la description de votre application sur le Store avec le badge Windows Store


Vous pouvez créer un lien direct vers la description de votre application grâce à un badge Windows Store personnalisé qui permet d’informer les clients de la disponibilité de votre application dans le Windows Store.

Pour créer votre badge, visitez la page [Badges Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=534236). Pour générer le badge et le lien, vous devez renseigner l’ID Windows Store de votre application dans ce formulaire. Cet ID correspond aux 12 derniers caractères de l’**URL pour Windows 10** affichée sur la page [Identité de l’application](view-app-identity-details.md) dans la section **Gestion des applications**.

> **Remarque** Pour plus d’informations sur l’utilisation du badge Windows Store, voir [Recommandations pour le marketing adapté aux applications](app-marketing-guidelines.md).

 

## Création d’un lien direct vers votre application dans le Windows Store


Vous pouvez créer un lien permettant d’ouvrir le Windows Store et d’accéder directement à la page de description de votre application sans avoir à ouvrir un navigateur à l’aide du schéma d’URI **ms-windows-store:**.

Ces liens sont utiles si vous savez que vos clients utilisent un appareil Windows et que vous souhaitez les rediriger immédiatement vers la page de description dans le Windows Store, par exemple, après avoir vérifié les chaînes d’agent utilisateur dans un navigateur pour confirmer le système d’exploitation de l’utilisateur, ou lorsque vous communiquez déjà via une application UWP et que vous souhaitez appliquer ce protocole.

Pour créer un lien d’accès direct à la description de votre application à l’aide du protocole Windows Store, il vous suffit d’ajouter l’ID Windows Store de votre application au lien suivant :

`ms-windows-store://pdp/?ProductId=`

Pour plus d’informations sur l’utilisation du protocole Windows Store, voir [Lancement de l’application Windows Store](../launch-resume/launch-store-app.md).

 

 






<!--HONumber=May16_HO2-->


