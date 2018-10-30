---
author: Xansky
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: Demander des évaluations et des avis pour votre app
ms.author: mhopkins
ms.date: 06/15/2018
ms.topic: article
keywords: windows10, uwp, évaluations et avis
ms.localizationpriority: medium
ms.openlocfilehash: d736fa47251c85491a29b324a3ed59181a5060c8
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762274"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Demander des évaluations et des avis pour votre app

Vous pouvez ajouter du code à votre app de plateforme Windows universelle (UWP) pour inviter par programmation vos clients à évaluer ou à donner un avis sur votre app. Il existe plusieurs façons de procéder:
* Vous pouvez afficher une boîte de dialogue d'avis et d'évaluation directement dans le contexte de votre app.
* Vous pouvez ouvrir par programmation la page d’évaluation et d'avis de votre app dans le MicrosoftStore.

Une fois que vous êtes prêt à analyser les données d'évaluations et d'avis, vous pourrez afficher les données dans le tableau de bord du centre de développement Windows ou utiliser l'API d'analyse du MicrosoftStore pour récupérer ces données via un programme.

> [!IMPORTANT]
> Lorsque vous ajoutez une fonction de contrôle d’accès au sein de votre application, tous les avis doivent envoyer à l’utilisateur aux mécanismes d’évaluation du magasin, quel que soit le d’étoiles choisie. Si vous collectez des commentaires des utilisateurs, il doit être clair qu’il n’est pas lié à la classification de l’application ou un avis dans le Windows Store, mais est envoyée directement au développeur de l’application. Consultez le développeur de Code de conduite pour plus d’informations relatives à [Fraudulent ou activités malveillants](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Afficher une boîte de dialogue d'évaluation et d'avis dans votre app

Pour lancer par programme une boîte de dialogue à partir de votre app, afin d'inviter vos clients à évaluer votre app et à soumettre un avis, appelez la méthode [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) dans l'espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Communiquez le chiffre entier 16 pour le paramètre *requestKind* et une chaîne vide pour le paramètre *parametersAsJson* comme indiqué dans cet exemple de code. Cet exemple requiert la bibliothèque [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft, et doit utiliser des instructions pour les espaces de noms **Windows.Services.Store**, **System.Threading.Tasks** et **Newtonsoft.Json.Linq **.

> [!IMPORTANT]
> La demande d'affichage de la boîte de dialogue d'évaluations et d'avis peut être appelée sur le thread d'interface utilisateur de votre app.

```csharp
public async Task<bool> ShowRatingReviewDialog()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 16, String.Empty);

    if (result.ExtendedError == null)
    {
        JObject jsonObject = JObject.Parse(result.Response);
        if (jsonObject.SelectToken("status").ToString() == "success")
        {
            // The customer rated or reviewed the app.
            return true;
        }
    }

    // There was an error with the request, or the customer chose not to
    // rate or review the app.
    return false;
}
```

La méthode **SendRequestAsync** utilise un système de requête basée sur un nombre entier simple ainsi que les paramètres de données JSON pour exposer les divers opérations Store sur les apps. Lorsque vous communiquez le chiffre entier 16 au paramètre *requestKind*, vous émettez une requête d'affichage de la boîte de dialogue d’évaluations et d'avis et envoyez les données associées au Store. Cette méthode a été introduite dans Windows10, version1607 et peut être utilisée uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build 14393)** ou une version ultérieure dans Visual Studio. Pour obtenir une vue d’ensemble de cette méthode, voir [Envoyer des demandes au MicrosoftStore](send-requests-to-the-store.md).

### <a name="response-data-for-the-rating-and-review-request"></a>Données de réponse pour les requêtes d’évaluations et d'avis

Une fois que vous aurez soumis la requête d'affichage de la boîte de dialogue d'évaluations et d'avis, la propriété [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) de la valeur de retour [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) contiendra une chaîne au format JSON qui indiquera que la requête a abouti.

L’exemple suivant montre la valeur de retour pour cette requête après qu'un client a soumis avec succès une évaluation ou un avis.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

L’exemple suivant montre la valeur de retour pour cette requête après qu'un client a décidé de ne pas soumettre une évaluation ou un avis.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

Le tableau ci-après décrit les champs qui figurent dans la chaîne de données au format JSON.

|  Champ  |  Description  |
|----------------------|---------------|
|  *status*                   |  Une chaîne qui indique si le client a soumis avec succès une évaluation ou un avis. Les valeurs prises en charge sont  **success** et **aborted**.   |
|  *data*                   |  Un objet qui contient une valeur booléenne unique nommée *updated*. Cette valeur indique si le client a mis à jour une évaluation ou un avis existant(e). L'objet *data* est uniquement inclus dans les réponses de réussite.   |
|  *errorDetails*                   |  Une chaîne qui contient les détails de l’erreur relative à la demande. |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Lancer la page d'évaluations et d'avis pour votre app dans le MicrosoftStore

Si vous souhaitez programmer l'ouverture de la page d'évaluations et d'avis pour votre app dans le MicrosoftStore, vous pouvez utiliser la méthode [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) avec le schéma d'URI ```ms-windows-store://review```, comme indiqué dans cet exemple de code.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Pour plus d’informations, voir [Lancer l'app MicrosoftStore](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analyser vos données d'évaluations et avis

Vous pouvez analyser les données d'évaluations et d’avis de vos clients de plusieurs manières:
* Vous pouvez utiliser le rapport [Évaluations](../publish/reviews-report.md) dans le tableau de bord du centre de développement Windows pour voir les évaluations et les avis de vos clients. Vous pouvez également télécharger ce rapport pour le consulter hors ligne.
* Vous pouvez utiliser les méthodes [Obtenir des évaluations de l’app](get-app-ratings.md) et [Obtenir les avis sur les apps](get-app-reviews.md) dans l’API d’analytique du MicrosoftStore pour programmer la récupération des évaluations et avis de vos clients au format JSON.

## <a name="related-topics"></a>Rubriquesassociées

* [Envoyer des demandes au MicrosoftStore](send-requests-to-the-store.md)
* [Lancer l’app MicrosoftStore](../launch-resume/launch-store-app.md)
* [Rapport sur les avis](../publish/reviews-report.md)
