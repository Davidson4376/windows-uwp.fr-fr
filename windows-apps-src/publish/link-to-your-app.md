---
Description: Vous pouvez aider les clients à découvrir votre application en liant à la publication de votre application dans le Microsoft Store.
title: Créer un lien vers votre application
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, lien, protocole windows store, lien vers une application, lien vers application
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661534"
---
# <a name="link-to-your-app"></a>Créer un lien vers votre application


Vous pouvez aider les clients à découvrir votre application en liant à la publication de votre application dans le Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtention du lien vers la description de votre application dans le Store

Pour obtenir l’URL de la description de votre application dans le Windows Store, accédez à la page [Identité de l’application](view-app-identity-details.md) de l’application dans la section **Gestion des applications**. L’URL présente le format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Lorsqu’un client clique sur ce lien, la page de description web de votre application s’affiche. Sur les appareils Windows, l’application du Windows Store lancera et affichera également la description de votre application.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Liaison à la liste de Store de votre application avec le badge Microsoft Store

Vous pouvez lier directement à la liste de votre application avec un badge personnalisé d’informer les clients de que votre application se trouve dans le Microsoft Store.

Pour créer votre badge, visitez le [Microsoft Store badges](https://go.microsoft.com/fwlink/p/?LinkID=534236) page. Pour générer le badge et le lien, vous devez disposer de **l’ID Store** de 12 caractères de votre application. Vous trouverez **l’ID Store** de votre application sur la page [Identité de l’application](view-app-identity-details.md) de la section **Gestion des applications**.

> [!NOTE]
> Consultez [application marketing instructions](app-marketing-guidelines.md) informations et les exigences liées à votre utilisation du badge Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Liaison directement à votre application dans le Microsoft Store

Vous pouvez créer un lien qui lance le Microsoft Store et accède directement à la page de liste de votre application sans ouvrir un navigateur à l’aide de la **ms-windows-store :** Schéma d’URI.

Ces liens sont utiles si vous savez que vos utilisateurs travaillent sur un appareil Windows et que vous voulez qu’ils accèdent directement à la page de description dans le Windows Store. Par exemple, vous pouvez utiliser ce lien après la vérification des chaînes de l’agent utilisateur dans un navigateur pour confirmer que le système d’exploitation de l’utilisateur prend en charge le Windows Store, ou lorsque vous communiquez déjà par le biais d’une application UWP.

Pour utiliser ce schéma d’URI pour accéder au Store de votre application répertoriant, ajoutez votre ID d’application Store sur ce lien :

`ms-windows-store://pdp/?ProductId=`

Pour plus d’informations sur l’utilisation du protocole Microsoft Store, consultez [lancer l’application Microsoft](../launch-resume/launch-store-app.md).

 

 




