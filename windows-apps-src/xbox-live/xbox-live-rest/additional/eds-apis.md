---
title: API EDS auxiliaires
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " API EDS auxiliaires"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603234"
---
# <a name="auxiliary-eds-apis"></a>API EDS auxiliaires

Il existe plusieurs divertissement découverte de Services (EDS) API qui ne fournissent directement des informations sur le contenu, mais fournissent des informations générales sur la façon d’utiliser le service ou l’aide de modèles d’interface utilisateur courants de lecteur.

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>API auxiliaire

| API| URI| Description|
| --- | --- | --- |
| Valeurs de paramètre de l’API| /{locale}/metadata| Énumération des valeurs possibles de paramètres qui peuvent être utilisés dans les appels au service|
| Combinées de générateur de classification du contenu| /{locale}/contentRating| Crée une valeur qui peut être utilisée dans d’autres API pour le filtrage de contenu potentiellement répréhensible ou explicit. Pour plus d’informations, voir ci-dessous.|
| Générateur de noms de champ combiné| /{locale}/fields| Crée une valeur qui peut être utilisée dans le détail API pour contrôler quels champs sont retournés. Pour plus d’informations, voir ci-dessous.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>Valeurs de paramètre de l’API

Cette API décrit les paramètres qui peuvent être utilisés avec le service. Les informations retournées sont utilisables par l’interface utilisateur du client, car le texte localisé accompagne chaque paramètre.

Aucun des API ci-dessous acceptent les paramètres de requête.

| API| URI| Description|
| --- | --- | --- | --- | --- | --- |
| Types| /{locale}/metadata/mediaGroups| La liste complète des groupes de supports|
| Types par Media group d’éléments multimédia| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| Types contenus dans le groupe de support donné d’éléments de la liste des médias.|
| Tous les types d’éléments multimédia| /{locale}/metadata/mediaItemTypes| La liste complète des types d’éléments multimédia|
| Champs par type d’élément multimédia| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| La liste des champs dans un type d’élément multimédia unique|
| Requête raffineurs| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| La liste des raffineurs de requête pris en charge pour le type d’élément multimédia donné|
| Toutes les valeurs d’affinement de requêtes| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| Type d’élément les valeurs pour l’affinement de la requête spécifiée pour le média donné|
| Toutes les sous-valeurs affinement de requêtes| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| La liste des valeurs secondaire pour une valeur d’affinement de requête donnée (par exemple, « subgenres d’un genre donné »). La valeur d’affinement de requête est transmise comme un paramètre de chaîne de requête nommé « queryRefinerValue », ce qui est effectué pour autoriser des valeurs d’affinement de requête avec des caractères interdits dans les ressources URI à passer.|
| Tris| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| La liste des ordres de tri pour le type d’élément multimédia donné|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>Combinées de générateur de classification du contenu

Appliquer des contrôles parentaux sur le contenu enfants sont autorisés à voir est une tâche compliquée. Non seulement chaque type d’élément média possède son propre système d’évaluation, mais ces systèmes de contrôle d’accès peuvent varier d’un pays à l’autre. Cela signifie qu’il n’y a plusieurs éléments différents de données qui doivent être utilisés pour filtrer correctement tous les éléments.

Au lieu de spécifier tous les paramètres dans tous les appels d’API, cette API génère une valeur à passer dans les paramètres combinedContentRating dans d’autres API et communiquer toujours les mêmes informations. Il est conçu pour faciliter les API à utiliser et à gérer, car plusieurs paramètres passés à cette API sont réduites en une valeur unique et réutilisable pour les autres API.

Bien que les valeurs exactes retournés par cette API peuvent changer par la suite, ils ne doivent changer très rarement (par exemple, entre les versions de EDS) et par conséquent, peut être mis en cache pendant de longues périodes de temps. N’importe quelle API acceptant qu'un paramètre combinedContentRating donnera un message d’erreur explicite si la valeur passée n’est pas valide, ce qui signifie que l’appelant simplement doit appeler cette API pour obtenir une valeur mise à jour. Si une API accepte un paramètre combinedContentRating, mais un n’est pas fourni, aucun filtrage de contenu n’aura lieu en fonction de contrôle parental

> [!NOTE]
> Cela ne signifie pas que le contenu uniquement « sécurisée » est retournée--cela signifie que tout le contenu est retourné, y compris le contenu potentiellement explicite).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>Nom du champ combiné

Les API d’EDS, par défaut, retourne un très petit ensemble minimal de champs pour chaque élément :

   * Type d’élément multimédia
   * Groupe de support
   * Id
   * Nom

Pour obtenir plus d’informations, les API acceptent un paramètre « champs » qui spécifie les éléments de données supplémentaires doivent être renvoyées. Comme il existe de nombreux champs possibles, en spécifiant leur nom dans sa totalité pour chaque appel d’API serait encombrements considérablement la demande. Au lieu de cela, les noms peuvent être passés dans cette API qui génère beaucoup plus petite valeur qui peut être passée dans les autres API.

Pour n’importe quelle API qui accepte ce paramètre, la valeur fournie doit être le sur-ensemble de tous les champs dans tous les types d’élément multimédia spécifié. Il n’est pas possible de spécifier différents ensembles de champs pour les types d’éléments multimédias différents. Toutefois, si un champ s’applique à un élément type de support, mais pas une autre, elle apparaît uniquement dans le support des types d’éléments où figurent des données (par exemple, si « AvatarBodyType » est inclus dans l’appel à l’API de nom de champ combinées, uniquement AvatarItems contient le champ).

Les valeurs retournées à partir de cette API sont hautement mis en cache--en fait, ils ne doivent pas changer entre les déploiements d’EDS, à l’exception. Il est recommandé que, si la mise en cache est souhaité, le cache pas durer plus la session de l’utilisateur.

Outre les noms de champ réels, cette API accepte « all » comme valeur valide. Cette opération génère une valeur qui contient chaque champ, qu'il est possible de spécifier. La valeur « all » est susceptible de servir uniquement pour le développement, débogage et à des fins de test.

<a id="ID4ERG"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ETG"></a>


##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>Informations supplémentaires

[URI de la place de marché](../uri/marketplace/atoc-reference-marketplace.md)
