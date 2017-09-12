---
author: jnHs
Description: "Vous pouvez aider les clients à découvrir votre application en créant un lien vers la description de votre application dans le Windows Store."
title: "Créer un lien vers votre application"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, lien, protocole windows store, lien vers une application, lien vers application
ms.openlocfilehash: 2d0750493926937a6326c5f72f568d4294b137c5
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2017
---
# <a name="link-to-your-app"></a>Créer un lien vers votre application


Vous pouvez aider les clients à découvrir votre application en créant un lien vers la description de votre application dans le Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtention du lien vers la description de votre application dans le Store

Pour obtenir l’URL de la description de votre application dans le WindowsStore, accédez à la page [Identité de l’application](view-app-identity-details.md) de l’application dans la section **Gestion des applications**. L’URL présente le format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Lorsqu’un client clique sur ce lien, la page de description web de votre application s’affiche. Sur les appareils Windows, l’application du WindowsStore lancera et affichera également la description de votre application.


## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>Création d’un lien vers la description de votre application dans le WindowsStore avec le badge WindowsStore

Vous pouvez créer un lien direct vers la description de votre application grâce à un badge Windows Store personnalisé qui permet d’informer les clients de la disponibilité de votre application dans le Windows Store.

Pour créer votre badge, consultez la page [Badges WindowsStore](http://go.microsoft.com/fwlink/p/?LinkID=534236). Pour générer le badge et le lien, vous devez disposer de **l’ID Store** de 12caractères de votre application. Vous trouverez **l’ID Store** de votre application sur la page [Identité de l’application](view-app-identity-details.md) de la section **Gestion des applications**.

> [!NOTE]
> Pour plus d’informations sur les exigences relatives à l’utilisation du badge WindowsStore, consultez l’article [Recommandations pour le marketing adapté aux applications](app-marketing-guidelines.md).


## <a name="linking-directly-to-your-app-in-the-windows-store"></a>Création d’un lien direct vers votre application dans le WindowsStore

Vous pouvez créer un lien permettant d’ouvrir le Windows Store et d’accéder directement à la page de description de votre application sans avoir à ouvrir un navigateur à l’aide du schéma d’URI **ms-windows-store:**.

Ces liens sont utiles si vous savez que vos utilisateurs travaillent sur un appareil Windows et que vous voulez qu’ils accèdent directement à la page de description dans le WindowsStore. Par exemple, vous pouvez utiliser ce lien après la vérification des chaînes de l’agent utilisateur dans un navigateur pour confirmer que le système d’exploitation de l’utilisateur prend en charge le WindowsStore, ou lorsque vous communiquez déjà par le biais d’une applicationUWP.

Pour créer un lien d’accès direct à la description de votre application dans le WindowsStore à l’aide du protocole WindowsStore, il vous suffit d’ajouter l’IDStore de votre application à ce lien:

`ms-windows-store://pdp/?ProductId=`

Pour plus d’informations sur l’utilisation du protocole Windows Store, voir [Lancement de l’application Windows Store](../launch-resume/launch-store-app.md).

 

 




