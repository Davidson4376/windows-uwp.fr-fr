---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: Créer un lien vers votre application
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, lien, protocole windows store, lien vers une application, lien vers application
ms.localizationpriority: medium
ms.openlocfilehash: 0025321aa73a66cc0a976bd347e613de3c3c4765
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2788054"
---
# <a name="link-to-your-app"></a>Créer un lien vers votre application


Vous pouvez aider les clients découvrir votre application par une liaison à la liste de votre application dans Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtention du lien vers la description de votre application dans le Store

Pour obtenir l’URL de la description de votre application dans le WindowsStore, accédez à la page [Identité de l’application](view-app-identity-details.md) de l’application dans la section **Gestion des applications**. L’URL présente le format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Lorsqu’un client clique sur ce lien, la page de description web de votre application s’affiche. Sur les appareils Windows, l’application du WindowsStore lancera et affichera également la description de votre application.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Liaison à une liste de banque de votre application avec le logo Microsoft Store

Vous pouvez lier directement à la liste de votre application avec une étiquette personnalisée pour informer les clients de que votre application est dans Microsoft Store.

Pour créer votre badge, visitez la page [Microsoft Store badges](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Pour générer le badge et le lien, vous devez disposer de **l’ID Store** de 12caractères de votre application. Vous trouverez **l’ID Store** de votre application sur la page [Identité de l’application](view-app-identity-details.md) de la section **Gestion des applications**.

> [!NOTE]
> Voir [instructions marketing d’application](app-marketing-guidelines.md) pour les informations et les obligations liées à l’utilisation du logo Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Liaison directe à votre application dans le magasin de Microsoft

Vous pouvez créer un lien qui lance Microsoft Store et accède directement à la page de votre application annonce sans ouvrir un navigateur à l’aide de la **ms-windows-store:** modèle URI.

Ces liens sont utiles si vous savez que vos utilisateurs travaillent sur un appareil Windows et que vous voulez qu’ils accèdent directement à la page de description dans le WindowsStore. Par exemple, vous pouvez utiliser ce lien après la vérification des chaînes de l’agent utilisateur dans un navigateur pour confirmer que le système d’exploitation de l’utilisateur prend en charge le WindowsStore, ou lorsque vous communiquez déjà par le biais d’une applicationUWP.

Pour utiliser ce modèle URI pour lier directement à la liste des magasins de votre application, ajoutez les ID de la base de votre application à ce lien:

`ms-windows-store://pdp/?ProductId=`

Pour plus d’informations sur l’utilisation du protocole Microsoft Store, voir [lancer l’application Microsoft](../launch-resume/launch-store-app.md).

 

 




