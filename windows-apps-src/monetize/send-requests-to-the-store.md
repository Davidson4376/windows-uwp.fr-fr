---
author: Xansky
Description: You can use the SendRequestAsync method to send requests to the Microsoft Store for operations that do not yet have an API available in the Windows SDK.
title: Envoyer des requêtes au MicrosoftStore
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: 6463f6eee6d3f5ec82122cef532db8d0e9a26dc6
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5520547"
---
# <a name="send-requests-to-the-microsoft-store"></a>Envoyer des requêtes au MicrosoftStore

Depuis la version1607 de Windows10, le SDK Windows fournit des API pour les opérations liées au MicrosoftStore (comme les achats dans l’application) dans l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Toutefois, bien que les services prenant en charge le WindowsStore soient constamment mis à jour, développés et améliorés entre les versions du système d’exploitation, les nouvelles API ne sont généralement ajoutées au SDK Windows qu’au moment de la publication des versions majeures du système d’exploitation.

Nous fournissons la méthode [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) car elle dispose d’une grande souplesse pour effectuer des opérations de WindowsStore disponibles pour les applications de plateforme Windows universelle (UWP) avant la publication d’une nouvelle version du SDK Windows. Vous pouvez utiliser cette méthode pour envoyer des requêtes au WindowsStore pour les nouvelles opérations ne disposant pas encore d’une API correspondante dans la dernière version du SDK Windows.

> [!NOTE]
> La méthode **SendRequestAsync** est disponible uniquement pour les applications ciblant la version1607 ou ultérieure de Windows10. Certaines requêtes prises en charge par cette méthode le sont uniquement dans les versions postérieures à la version1607 de Windows10.

**SendRequestAsync** est une méthode statique de la classe [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper). Pour appeler cette méthode, vous devez lui transmettre les informations suivantes:
* Un objet [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) fournissant des informations sur l’utilisateur pour lequel vous souhaitez effectuer l’opération. Pour plus d’informations sur cet objet, consultez [Démarrer avec la classe StoreContext](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Un entier identifiant la requête que vous souhaitez transmettre au WindowsStore.
* Si la requête gère des arguments, vous pouvez également passer une chaîne au format JSON contenant les arguments à transmettre avec la requête.

L’exemple qui suit montre comment appeler cette méthode. Cet exemple implique l’utilisation d’instructions pour les espaces de noms **Windows.Services.Store** et **System.Threading.Tasks**.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

Consultez les sections suivantes pour plus d’informations sur les requêtes actuellement disponibles via la méthode **SendRequestAsync**. Nous mettrons à jour cet article lorsque de nouveaux types de requêtes seront ajoutés.

## <a name="request-for-in-app-ratings-and-reviews"></a>Requête pour les évaluations et avis in-app

Vous pouvez lancer par programme une boîte de dialogue à partir de votre app, pour inviter vos clients à évaluer votre app et à soumettre un avis en transmettant le nombre entier de requête 16 pour la méthode **SendRequestAsync**. Pour plus d’informations, voir [Afficher une boîte de dialogue d'évaluation et d'avis dans votre app](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app).

## <a name="requests-for-flight-group-scenarios"></a>Requêtes de scénarios de groupes de versions d’évaluation

> [!IMPORTANT]
> Actuellement, les requêtes de groupes de versions d’évaluation décrites dans cette section ne sont pas toutes disponibles pour la majorité des comptes de développeur. Ces requêtes échoueront tant que votre compte de développeur ne sera pas spécialement approvisionné par Microsoft.

La méthode **SendRequestAsync** prend en charge un ensemble de requêtes pour les scénarios de groupes de versions d’évaluation, telles que l’ajout d’un utilisateur ou d’un périphérique à un groupe de versions d’évaluation. Pour nous envoyer ces requêtes, transmettez la valeur 7 ou 8 au paramètre *requestKind* ainsi qu’une chaîne au format JSON au paramètre *parametersAsJson* indiquant la requête que vous souhaitez soumettre ainsi que tous les arguments liés. Ces valeurs *requestKind* diffèrent de plusieurs façons.

|  Valeur de type de requête  |  Description  |
|----------------------|---------------|
|  7                   |  Les demandes sont effectuées dans le contexte de l’appareil actuel. Cette valeur ne peut être utilisée que sur une version1703 ou ultérieure de Windows10.  |
|  8                   |  Les requêtes sont effectuées dans le contexte de l’utilisateur actuellement connecté au WindowsStore. Cette valeur peut être utilisée sur une version1607 ou ultérieure de Windows10.  |

Les requêtes de groupes de versions d’évaluation suivantes sont actuellement en place.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Récupérer des variables distantes pour le groupe de versions d’évaluation le plus élevé

> [!IMPORTANT]
> Cette requête est actuellement indisponible pour la plupart des comptes de développeur. Cette requête échouera tant que votre compte de développeur ne sera pas spécialement approvisionné par Microsoft.

Cette requête récupère les variables distantes pour le groupe de versions d’évaluation le plus élevé de l’utilisateur ou du périphérique actuel. Pour envoyer cette requête, fournissez les informations suivantes pour les paramètres *requestKind* et *parametersAsJson* de la méthode **SendRequestAsync**.

|  Paramètre  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  Spécifiez 7 pour retourner le groupe de versions d’évaluation le plus élevé de l’appareil, ou spécifiez 8 pour retourner le groupe de versions d’évaluation le plus élevé de l’utilisateur actuel et du périphérique. Nous vous recommandons d’utiliser la valeur 8 pour le paramètre *requestKind*, dans la mesure où cette valeur renvoie le groupe de versions d’évaluation le plus élevé parmi tous les membres pour l’appareil et l’utilisateur en cours.  |
|  *parametersAsJson*                   |  Transmettez une chaîne au format JSON contenant les données montrées dans l’exemple ci-dessous.  |

L’exemple qui suit illustre le format des données JSON à transmettre à *parametersAsJson*. Le champ *type* doit être affecté à la chaîne *GetRemoteVariables*. Affectez le champ *projectId* à l’ID du projet pour lequel vous avez défini les variables distantes dans le tableau de bord du centre de développement Windows.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Une fois cette requête soumise, la propriété [réponse](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) de [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) renvoie une valeur contenant une chaîne au format JSON avec les champs suivants.

|  Champ  |  Description  |
|----------------------|---------------|
|  *anonyme*                   |  Valeur booléenne, où **true** (vrai) indique que l’identité de l’utilisateur ou du périphérique n’était pas présente dans la demande, et **false** (faux) indique que l’identité de l’utilisateur ou du périphérique était contenue dans la demande.  |
|  *name*                   |  Une chaîne contenant le nom du groupe de versions d’évaluation le plus élevé auquel appartient l’appareil ou l’utilisateur.  |
|  *settings*                   |  Un dictionnaire de paires clé/valeur contenant le nom et la valeur des variables distantes que le développeur a configurées pour le groupe de versions d’évaluation.  |

L’exemple qui suit montre une valeur de retour pour cette demande.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Ajouter l’appareil ou l’utilisateur en cours à un groupe de versions d’évaluation

> [!IMPORTANT]
> Cette requête est actuellement indisponible pour la plupart des comptes de développeur. Cette requête échouera tant que votre compte de développeur ne sera pas spécialement approvisionné par Microsoft.

Pour envoyer cette requête, fournissez les informations suivantes pour les paramètres *requestKind* et *parametersAsJson* de la méthode **SendRequestAsync**.

|  Paramètre  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  Spécifiez 7 pour ajouter le périphérique à un groupe de versions d’évaluation, ou indiquez 8 pour ajouter l’utilisateur actuellement connecté au WindowsStore à un groupe de versions d’évaluation.  |
|  *parametersAsJson*                   |  Transmettez une chaîne au format JSON contenant les données montrées dans l’exemple ci-dessous.  |

L’exemple qui suit illustre le format des données JSON à transmettre à *parametersAsJson*. Le champ *type* doit être affecté à la chaîne *AddToFlightGroup*. Affectez le champ *flightGroupId* à l’ID du groupe de versions d’évaluation à auquel vous souhaitez ajouter l’appareil ou l’utilisateur.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

En cas d’erreur lors de la requête, la valeur retournée par la propriété [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) de [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contient le code de réponse.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Retirer l’appareil ou l’utilisateur en cours d’un groupe de versions d’évaluation

> [!IMPORTANT]
> Cette requête est actuellement indisponible pour la plupart des comptes de développeur. Cette requête échouera tant que votre compte de développeur ne sera pas spécialement approvisionné par Microsoft.

Pour envoyer cette requête, fournissez les informations suivantes pour les paramètres *requestKind* et *parametersAsJson* de la méthode **SendRequestAsync**.

|  Paramètre  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  Spécifiez 7 pour retirer le périphérique d’un groupe de versions d’évaluation, ou indiquez 8 pour retirer l’utilisateur actuellement connecté au WindowsStore d’un groupe de versions d’évaluation.  |
|  *parametersAsJson*                   |  Transmettez une chaîne au format JSON contenant les données montrées dans l’exemple ci-dessous.  |

L’exemple qui suit illustre le format des données JSON à transmettre à *parametersAsJson*. Le champ *type* doit être affecté à la chaîne *RemoveFromFlightGroup*. Affectez le champ *flightGroupId* à l’ID du groupe de versions d’évaluation duquel vous souhaitez retirer l’appareil ou l’utilisateur.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

En cas d’erreur lors de la requête, la valeur retournée par la propriété [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) de [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contient le code de réponse.

## <a name="related-topics"></a>Rubriquesassociées

* [Afficher une boîte de dialogue d'évaluation et d'avis dans votre app](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)
