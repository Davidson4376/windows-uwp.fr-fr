---
author: Xansky
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir la trace de pile concernant une erreur dans votre application de bureau.
title: Obtenir la trace de pile concernant une erreur dans votre application de bureau
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, services du Store, API d’analyse du MicrosoftStore, trace de pile, erreur, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 60f4ed7251fa934190a96e8c7d6f0edd21520980
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4680545"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Obtenir la trace de pile concernant une erreur dans votre application de bureau

Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir la trace de pile d'une erreur dans une application de bureau que vous avez ajoutée au [Programme pour applications de bureau Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Cette méthode ne peut télécharger que la trace de pile concernant une erreur survenue dans les 30derniers jours. Les traces de pile sont également disponibles dans le [rapport d'intégrité](https://msdn.microsoft.com/library/windows/desktop/mt826504) pour les applications de bureau dans le tableau de bord du Centre de développement Windows.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les détails sur une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer le hachage d'ID du fichierCAB associé à l’erreur dont vous voulez récupérer la trace de pile.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez le hachage d’ID du fichierCAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cette valeur, utilisez la méthode [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre app, en spécifiant la valeur **cabIdHash** dans le corps de la réponse.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | L'ID produit de l’application de bureau dont vous souhaitez obtenir une trace de pile. Pour obtenir l’ID produit d’une application de bureau, ouvrez le [rapport analytique du centre de développement de votre application de bureau](https://msdn.microsoft.com/library/windows/desktop/mt826504) (par exemple, le **Rapport d’intégrité**) et récupérez l’ID produit à partir de l’URL. |  Oui  |
| cabIdHash | chaîne | Le hachage d'ID unique du fichierCAB associé à l’erreur dont vous souhaitez récupérer la trace de pile. Pour obtenir cette valeur, utilisez la méthode [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre app, en spécifiant la valeur **cabIdHash** dans le corps de la réponse. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir la trace de pile avec cette méthode. Remplacez les paramètres *applicationId* et *cabIdHash* par les valeurs qui correspondent à votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description                  |
|------------|---------|--------------------------------|
| Valeur      | array   | Tableau d’objets qui contiennent chacun une image de données de trace de pile. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs de la trace de pile](#stack-trace-values) ci-dessous. |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10, mais que plus de 10lignes d’erreur sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de données de la requête.          |


### <a name="stack-trace-values"></a>Valeurs de la trace de pile

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur           | Type    | Description      |
|-----------------|---------|----------------|
| level            | chaîne  |  Numéro de l’image représentant cet élément dans la pile d’appels.  |
| image   | chaîne  |   Nom du fichier exécutable ou de l’image de bibliothèque qui contient la fonction appelée dans cette image de pile.           |
| function | chaîne  |  Nom de la fonction appelée dans cette image de pile. Cette fonctionnalité n’est disponible que si votre application contient des symboles pour le fichier exécutable ou la bibliothèque.              |
| offset     | chaîne  |  Décalage d’octet de l’instruction actuelle par rapport au début de la fonction.      |


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
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de signalement d’erreurs pour votre application de bureau](get-desktop-application-error-reporting-data.md)
* [Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)
* [Télécharger le fichier CAB concernant une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)
