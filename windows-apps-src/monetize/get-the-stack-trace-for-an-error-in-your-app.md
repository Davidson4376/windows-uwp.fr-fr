---
author: mcleanbyron
ms.assetid: 
description: "Utilisez cette méthode dans l’API d’analyse du Windows&nbsp;Store pour obtenir la trace de pile concernant une erreur dans votre application."
title: Obtenir la trace de pile concernant une erreur dans votre application
translationtype: Human Translation
ms.sourcegitcommit: 767097f068630e5ec171415c05d6dc395c8b26b3
ms.openlocfilehash: 90481b5f85d010a142e86ca67ac94c3ec25d89c6

---

# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Obtenir la trace de pile concernant une erreur dans votre application

Utilisez cette méthode dans l’API d’analyse du Windows&nbsp;Store pour obtenir la trace de pile concernant une erreur dans votre application. Cette méthode ne peut télécharger que la trace de pile concernant une erreur d’application survenue dans les 30&nbsp;derniers jours. Les traces de pile sont également disponibles dans la section **Échecs** du [rapport d’intégrité](../publish/health-report.md) dans le tableau de bord du Centre de développement Windows.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les détails sur une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer l’ID du fichier&nbsp;CAB associé à l’erreur dont vous voulez récupérer la trace de pile.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes&nbsp;:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60&nbsp;minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier&nbsp;CAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | ID Windows&nbsp;Store de l’application dont vous souhaitez obtenir la trace de pile. L’ID Windows&nbsp;Store est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows&nbsp;Store&nbsp;: 9WZDNCRFJ3Q8. |  Oui  |
| cabId | chaîne | ID unique du fichier&nbsp;CAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse. |  Oui  |

<span/>
 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir la trace de pile avec cette méthode. Remplacez la valeur *applicationId* par l’ID Windows&nbsp;Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description                  |
|------------|---------|--------------------------------|
| Valeur      | array   | Tableau d’objets qui contiennent chacun une image de données de trace de pile. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs de la trace de pile](#stack-trace-values) ci-dessous. |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur&nbsp;10, mais que plus de 10&nbsp;lignes d’erreur sont associées à la requête. |
| TotalCount | nombre entier | Nombre total de lignes dans les résultats de la requête.          |

<span/>

### <a name="stack-trace-values"></a>Valeurs de la trace de pile

Les éléments du tableau *Value* ont les valeurs suivantes&nbsp;:

| Valeur           | Type    | Description      |
|-----------------|---------|----------------|
| level            | chaîne  |  Numéro de l’image représentant cet élément dans la pile d’appels.  |
| image   | chaîne  |   Nom du fichier exécutable ou de l’image de bibliothèque qui contient la fonction appelée dans cette image de pile.           |
| function | chaîne  |  Nom de la fonction appelée dans cette image de pile. Cette fonctionnalité n’est disponible que si votre application contient des symboles pour le fichier exécutable ou la bibliothèque.              |
| offset     | chaîne  |  Décalage d’octet de l’instruction actuelle par rapport au début de la fonction.      |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les informations sur une erreur de votre application](get-details-for-an-error-in-your-app.md)



<!--HONumber=Dec16_HO1-->


