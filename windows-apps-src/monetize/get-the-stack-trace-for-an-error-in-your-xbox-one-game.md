---
author: Xansky
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir la trace de pile concernant une erreur dans votre console Xbox One du jeu.
title: Obtenir la trace de pile concernant une erreur dans votre console Xbox One du jeu
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: windows10, uwp, services du Store, API d’analyse du MicrosoftStore, trace de pile, erreur
ms.localizationpriority: medium
ms.openlocfilehash: df3af90bda9d972a891dce67730f8f320b7607c1
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6835176"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>Obtenir la trace de pile concernant une erreur dans votre console Xbox One du jeu

Utilisez cette méthode dans le Microsoft Store analytique API pour obtenir la trace de pile concernant une erreur dans votre console Xbox One jeu intégré via le portail de développement Xbox (XDP) et disponible dans le tableau de bord du centre de développement Analytique XDP. Cette méthode ne peut télécharger que la trace de pile concernant une erreur survenue dans les 30derniers jours.

Avant de pouvoir utiliser cette méthode, vous devez d’abord utiliser la méthode [d’obtenir des détails sur une erreur dans votre jeu Xbox One](get-details-for-an-error-in-your-xbox-one-game.md) pour récupérer l’ID du fichier CAB associé à l’erreur dont vous voulez récupérer la trace de pile.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichierCAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cet ID, utilisez la méthode de [obtenir plus d’informations concernant une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) pour récupérer les détails d’une erreur spécifique dans votre application et utiliser la valeur de **cabId** dans le corps de réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit du jeu Xbox One pour laquelle vous récupérez une trace de pile. Pour obtenir l’ID produit de votre jeu, accédez à votre jeu dans le portail de développement Xbox (XDP) et récupérez l’ID produit à partir de l’URL. Par ailleurs, si vous téléchargez vos données d’intégrité de l’état d’analytique du centre de développement Windows, l’ID de produit est inclus dans le fichier .tsv. |  Oui  |
| cabId | chaîne | ID unique du fichierCAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cet ID, utilisez la méthode de [obtenir plus d’informations concernant une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) pour récupérer les détails d’une erreur spécifique dans votre application et utiliser la valeur de **cabId** dans le corps de réponse de cette méthode. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir une trace de pile d’une console Xbox One, le jeu à l’aide de cette méthode. Remplacez la valeur *applicationId* par l’ID de produit pour votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
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
| function | chaîne  |  Nom de la fonction appelée dans cette image de pile. Cela est disponible uniquement si votre jeu contient des symboles pour le fichier exécutable ou de la bibliothèque.              |
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

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données rapport d’erreurs pour votre console Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtenir les détails d’une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md)
* [Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
