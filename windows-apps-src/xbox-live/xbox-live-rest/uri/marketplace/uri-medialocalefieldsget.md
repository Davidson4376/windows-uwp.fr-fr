---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606674"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
Obtient le jeton de champs. Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EGC)
  * [Paramètres de chaîne de requête](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Divertissement découverte de Services (EDS) API, par défaut, retourne un très petit ensemble minimal de champs pour chaque élément :
 
   * Type d’élément multimédia
   * Groupe de support
   * Id
   * Nom
  
Pour obtenir plus d’informations, les API acceptent un **champs** paramètre qui spécifie les éléments de données supplémentaires doivent être retournées. Comme il existe de nombreux champs possibles, en spécifiant leur nom dans sa totalité pour chaque appel d’API serait encombrements considérablement la demande. Au lieu de cela, les noms peuvent être passés dans cette API qui génère beaucoup plus petite valeur qui peut être passée dans les autres API.
 
Pour n’importe quelle API qui accepte ce paramètre, la valeur fournie doit être le sur-ensemble de tous les champs dans tous les types d’élément multimédia spécifié. Il n’est pas possible de spécifier différents ensembles de champs pour les types d’éléments multimédias différents. Toutefois, si un champ s’applique à un élément type de support, mais pas une autre, elle apparaît uniquement dans le support des types d’éléments où figurent des données (par exemple, si « AvatarBodyType » est inclus dans l’appel à [obtenir (/media/ {marketplaceId} / champs)](uri-medialocalefields.md), uniquement AvatarItems contient le champ).
 
Les valeurs retournées à partir de cette API sont hautement mis en cache--en fait, ils ne doivent pas changer entre les déploiements d’EDS, à l’exception. Il est recommandé que, si la mise en cache est souhaité, le cache pas durer plus la session de l’utilisateur.
 
Outre les noms de champ réels, cette API accepte « all » comme valeur valide. Cette opération génère une valeur qui contient chaque champ, qu'il est possible de spécifier. Utilisation de la valeur « all » est recommandée uniquement pour le développement, débogage et à des fins de test.
 
Vous pouvez également envoyer `desired={list of fields separated by a '.'}` à une API qui accepte le **champs** jeton.
 
Vous ne pouvez pas passer à la fois dans **souhaitée** et **champs** ensemble.
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| Vous le souhaitez| chaîne| Obligatoire. Le «. » -liste séparée par des champs qui doivent être retournées en plus de l’ensemble minimal. Pas tous les champs spécifiés sont garanties devant être retournées pour chaque objet (données simplement existe ne peut-être pas pour certains éléments), mais aucun champ en dehors de l’ensemble minimal qui ne sont pas spécifiés ici ne s’affichera. | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   