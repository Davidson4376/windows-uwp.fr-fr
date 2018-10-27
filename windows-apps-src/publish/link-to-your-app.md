---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: Créer un lien vers votre application
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp, lien, protocole windows store, lien vers une application, lien vers application
ms.localizationpriority: medium
ms.openlocfilehash: 5028a3902e62d188db5283ec6186fe5364b53de0
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5699681"
---
# <a name="link-to-your-app"></a>Créer un lien vers votre application


Vous pouvez aider les clients à découvrir votre application en créant un lien vers la description de votre application dans le Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtention du lien vers la description de votre application dans le Store

Pour obtenir l’URL de la description de votre application dans le WindowsStore, accédez à la page [Identité de l’application](view-app-identity-details.md) de l’application dans la section **Gestion des applications**. L’URL présente le format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Lorsqu’un client clique sur ce lien, la page de description web de votre application s’affiche. Sur les appareils Windows, l’application du WindowsStore lancera et affichera également la description de votre application.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Créer un lien vers la description de votre application avec le badge Microsoft Store

Vous pouvez lier directement à la description de votre application avec un badge personnalisé pour permettre aux clients de votre application est dans le Microsoft Store.

Pour créer votre badge, visitez la page [Microsoft Store badges](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Pour générer le badge et le lien, vous devez disposer de **l’ID Store** de 12caractères de votre application. Vous trouverez **l’ID Store** de votre application sur la page [Identité de l’application](view-app-identity-details.md) de la section **Gestion des applications**.

> [!NOTE]
> Consultez les [recommandations en matière de marketing application](app-marketing-guidelines.md) d’informations et les exigences relatives à l’utilisation du badge Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Créer un lien direct vers votre application dans le Microsoft Store

Vous pouvez créer un lien qui lance le Microsoft Store et accéder directement à la page de description de votre application sans ouvrir un navigateur à l’aide de la **ms-windows-store:** schéma d’URI.

Ces liens sont utiles si vous savez que vos utilisateurs travaillent sur un appareil Windows et que vous voulez qu’ils accèdent directement à la page de description dans le WindowsStore. Par exemple, vous pouvez utiliser ce lien après la vérification des chaînes de l’agent utilisateur dans un navigateur pour confirmer que le système d’exploitation de l’utilisateur prend en charge le WindowsStore, ou lorsque vous communiquez déjà par le biais d’une applicationUWP.

Pour utiliser ce schéma d’URI pour lier directement à la description de votre application, ajoutez ID du Windows Store de votre application à ce lien:

`ms-windows-store://pdp/?ProductId=`

Pour plus d’informations sur l’utilisation du protocole de Microsoft Store, voir [lancer l’application de Microsoft](../launch-resume/launch-store-app.md).

 

 




