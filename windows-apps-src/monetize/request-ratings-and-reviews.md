---
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: Demander des évaluations et des avis pour votre app
ms.date: 01/22/2019
ms.topic: article
keywords: windows10, uwp, évaluations et avis
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: 7a1899358cd5ce9d2f9fa1bd174a123740f98e7a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042635"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Demander des évaluations et des avis pour votre app

Vous pouvez ajouter du code à votre app de plateforme Windows universelle (UWP) pour inviter par programmation vos clients à évaluer ou à donner un avis sur votre app. Il existe plusieurs façons de procéder:
* Vous pouvez afficher une boîte de dialogue d'avis et d'évaluation directement dans le contexte de votre app.
* Vous pouvez ouvrir par programmation la page d’évaluation et d'avis de votre app dans le MicrosoftStore.

Lorsque vous êtes prêt à analyser vos données d’évaluations et avis, vous pouvez afficher les données dans l’espace partenaires ou utiliser l’API d’analytique Microsoft Store pour récupérer ces données par programme.

> [!IMPORTANT]
> Lorsque vous ajoutez une fonction d’évaluation au sein de votre application, tous les avis doivent renvoyer l’utilisateur mécanismes d’évaluation du magasin, quel que soit l’étoiles choisie. Si vous collectez des commentaires des utilisateurs, il doit être clair qu’il n’est pas lié à l’évaluation de l’application ou un avis dans le Windows Store, mais est envoyée directement au développeur de l’application. Voir le développeur de Code de conduite pour plus d’informations relatives à [Fraudulent ou activités malveillants](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Afficher une boîte de dialogue d'évaluation et d'avis dans votre app

Pour lancer par programme une boîte de dialogue à partir de votre application afin d’inviter vos clients à évaluer votre application et à soumettre un avis, appelez la méthode [RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) dans l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) . 

> [!IMPORTANT]
> La demande d'affichage de la boîte de dialogue d'évaluations et d'avis peut être appelée sur le thread d'interface utilisateur de votre app.

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

La méthode **RequestRateAndReviewAppAsync** a été introduite dans Windows 10, version 1809, et peut être utilisé uniquement dans les projets qui ciblent Windows **10 octobre 2018 Update (version 10.0; Build 17763)** ou une version ultérieure dans Visual Studio.

### <a name="response-data-for-the-rating-and-review-request"></a>Données de réponse pour les requêtes d’évaluations et d'avis

Une fois que vous envoyez la demande pour afficher l’évaluation et passez en revue la boîte de dialogue, la propriété [ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) de la classe [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) contient une chaîne au format JSON qui indique si la demande a réussi.

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

| Champ          | Description                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *status*       | Une chaîne qui indique si le client a soumis avec succès une évaluation ou un avis. Les valeurs prises en charge sont  **success** et **aborted**. |
| *data*         | Un objet qui contient une valeur booléenne unique nommée *updated*. Cette valeur indique si le client a mis à jour une évaluation ou un avis existant(e). L'objet *data* est uniquement inclus dans les réponses de réussite. |
| *errorDetails* | Une chaîne qui contient les détails de l’erreur relative à la demande.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Lancer la page d'évaluations et d'avis pour votre app dans le MicrosoftStore

Si vous souhaitez programmer l'ouverture de la page d'évaluations et d'avis pour votre app dans le MicrosoftStore, vous pouvez utiliser la méthode [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) avec le schéma d'URI ```ms-windows-store://review```, comme indiqué dans cet exemple de code.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Pour plus d’informations, voir [Lancer l'app MicrosoftStore](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analyser vos données d'évaluations et avis

Vous pouvez analyser les données d'évaluations et d’avis de vos clients de plusieurs manières:
* Vous pouvez utiliser le rapport [avis](../publish/reviews-report.md) dans l’espace partenaires pour voir les évaluations et avis de vos clients. Vous pouvez également télécharger ce rapport pour le consulter hors ligne.
* Vous pouvez utiliser les méthodes [Obtenir des évaluations de l’app](get-app-ratings.md) et [Obtenir les avis sur les apps](get-app-reviews.md) dans l’API d’analytique du MicrosoftStore pour programmer la récupération des évaluations et avis de vos clients au format JSON.

## <a name="related-topics"></a>Rubriquesassociées

* [Envoyer des demandes au MicrosoftStore](send-requests-to-the-store.md)
* [Lancer l’app MicrosoftStore](../launch-resume/launch-store-app.md)
* [Rapport sur les avis](../publish/reviews-report.md)
