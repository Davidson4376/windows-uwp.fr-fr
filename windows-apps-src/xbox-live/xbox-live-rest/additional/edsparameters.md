---
title: Paramètres EDS
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " Paramètres EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655414"
---
# <a name="eds-parameters"></a>Paramètres EDS

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

Ces paramètres de requête ne sont pas nécessairement acceptées par tous les [Services de découverte de divertissement (EDS) API](../uri/marketplace/atoc-reference-marketplace.md), mais tous sont acceptés par plusieurs API.

| Paramètre| Type| Description|
| --- | --- | --- |
| combinedContentRating| chaîne| Facultatif. Consultez [obtenir (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md).|
| continuationToken| chaîne| Facultatif. Le jeton de continuation est un objet blob opaque qui contient les informations dont le service a besoin pour la pagination dans certains scénarios. Si la valeur est omise, la première page de résultats est retournée (où la taille de page est déterminée par le paramètre de maxItems), ainsi que d’un jeton de liaison qui peut être utilisé pour obtenir la deuxième page de résultats. La deuxième page serait contiennent le jeton de continuation pour la troisième page de résultats et ainsi de suite.|
| Vous le souhaitez| chaîne| Facultatif. Consultez l’API de nom de champ combiné.|
| desiredMediaItemTypes| tableau de chaînes| Facultatif. Ce paramètre détermine les types d’éléments à retourner dans la réponse.|
| domaine.| chaîne| Facultatif. Le paramètre de domaine détermine le contexte de la place de marché de jeu et d’application dans le client qui appelle pour. Par défaut, le domaine est « Modern », indiquant que le client peut demander uniquement du contenu Xbox One. Si le client souhaite passer le domaine pour exposer le contenu Xbox 360, il doit spécifier le domaine en tant que « Xbox360 ». Actuellement, inter-domaines résultats ne sont pas pris en charge. Les valeurs possibles sont : <ul><li>Xbox360</li><li>Moderne</li></ul> La valeur « Xbox360 » est uniquement pris en charge pour singleMediaGroupSearch. Parcourir et détails sont pris en charge. crossMediaGroupSearch n’est pas pris en charge et renverra une erreur 400.|
| champs| chaîne| Facultatif. Consultez [obtenir (/media/ {marketplaceId} / champs)](../uri/marketplace/uri-medialocalefieldsget.md).|
| firstPartyOnly| Valeur booléenne| Paramètre de filtre facultatif. Détermine si contenu uniquement interne est retourné, ou si les deux premiers - et -tiers contenu sont retournées à partir de la requête. |
| freeOnly| Valeur booléenne| Paramètre de filtre facultatif. Limite les résultats pour libérer uniquement de contenu.|
| GroupBy| TK| Le paramètre groupBy est utilisé pour aider à classer le jeu de résultats en groupes, plutôt qu’un seul jeu de résultats. En spécifiant ce paramètre modifiera le jeu de résultats à retourner plusieurs listes d’éléments, où le nombre d’éléments dans chaque compartiment est déterminé par le paramètre de maxItems. <ul><li>MediaGroup - les résultats sont regroupés par MediaGroup.</li></ul> |
| hasTrailer| Valeur booléenne| Paramètre de filtre facultatif. Détermine si les éléments renvoyés doivent contenir un code de fin ou si l’utilisation d’un code de fin est facultative. Si la valeur est true, tous les éléments doivent avoir des codes de fin.|
| id| chaîne| Facultatif. S’il est fourni, restreint les résultats pour être uniquement les enfants de l’élément avec l’ID donné. Si ce paramètre est fourni, MediaItemType doit également être spécifié. |
| ID| tableau de chaînes| Obligatoire. Tous les ID (jusqu'à 10) pour lequel les détails seront retournés. Notez que les ID qui contient des caractères non conformes à placer dans une URL (les ID de type ProviderContentId sont des URL complètes normalement eux-mêmes et par conséquent contient des caractères non autorisés) doit être codée URL doit être correctement envoyé à EDS.|
| idType| chaîne| Facultatif. Le type de l’ID qui sont passés dans le paramètre ID. Les valeurs valides sont les suivantes : <ul><li>Canonique (Bing/place de marché)</li><li>XboxHexTitle (application de lecture sur la console)</li></ul>  Toutes fournies Qu'id doivent partager le même type. Si cette valeur est omise, tous les ID sont supposées pour être Canonical.|
| latestOnly| Valeur booléenne| Paramètre de filtre facultatif. Limite les résultats aux seules avec la dernière date de publication.|
| maxItems| entier signé 32 bits| Facultatif. Détermine le nombre maximal d’éléments qui doivent être retournées à partir d’un appel. Les valeurs valides sont des nombres compris entre 1 et 25, inclusif. Le paramètre par défaut est 25 si omise.|
| MediaGroup| chaîne| Facultatif. Le groupe de support des ID. Toutes fournies Qu'id doivent partager le même groupe de support.|
| MediaItemType| chaîne| Facultatif. Le type de l’élément dont l’ID a été spécifié dans le paramètre id. Si le paramètre id est fourni, ce paramètre doit également être spécifié.|
| orderBy| chaîne| Obligatoire. Le paramètre orderBy détermine la façon dont les éléments retournés doivent être triées. Les valeurs courantes pour ce champ sont répertoriées ici, mais certaines API peuvent prendre en charge les valeurs supplémentaires.<ul><li>playCountDaily - par nombre d’heures média lu, du jour plus récente.</li><li>freeAndPaidCountDaily - par nombre d’achats gratuites et payantes, du jour plus récente.</li><li>paidCountAllTime - par nombre d’achats de payants uniquement, pour toutes les périodes.</li><li>paidCountDaily - par nombre d’uniquement payants achats du jour plus récente.</li><li>digitalReleaseDate - par date disponible au téléchargement.</li><li>date de publication - par date disponible dans les magasins, retour à la date de publication numérique (si disponible).</li><li>userRatings - par les évaluations de l’utilisateur moyen.</li></ul> |
| preferredProvider| chaîne| Facultatif. Si l’utilisateur dispose d’un fournisseur de contenu par défaut, tels que Comcast Xfinity ou FIOS Verizon, les ID de ce fournisseur peut être passé. Lorsque l’ordre réel des éléments ne changera pas, pour chaque élément, les informations du fournisseur spécifié seront affiche en haut de la liste des fournisseurs (si le fournisseur de contenu par défaut est l’élément disponible).|
| q| chaîne| Obligatoire. Terme utilisé dans la recherche de la requête.|
| queryRefiners| tableau de chaînes| Facultatif. Afficher la liste des [EDS requête raffineurs](edsqueryrefiners.md).|
| Relation| chaîne| Facultatif. Filtre qui utilise le paramètre ID en tant que la base de recherche pour d’autres produits qui correspondent au type de relation spécifié : <ul><li>bundledWith - recherchez les produits de bundle où le paramètre ID est inclus dans le cadre de ces regroupements.</li><li>bundledProducts - rechercher les produits qui sont incluses dans le groupe spécifié par le paramètre ID.</li></ul>  Seuls les produits qui sont visibles dans la place de marché (peuvent être retournées dans un appel de parcourir) seront affichera avec ce paramètre. Si une offre groupée dispose d’un produit masqué, il fait toujours partie du regroupement, mais dans ces résultats.|
  | ScopeId | chaîne | Ce paramètre est utilisé dans les scénarios de recherche inversée pour les supports vidéo. |
  | ScopeIdType | chaîne | Ce paramètre est utilisé dans les scénarios de recherche inversée pour les supports vidéo. Valeurs possibles : Titre. |
  | skipItems | entier signé 32 bits | Facultatif. Pour autoriser la pagination dans les scénarios non-entre les groupes, le paramètre skipItems est utilisé pour déterminer combien d’éléments ont déjà été vu (et par conséquent, quel élément doit être affiché tout d’abord dans le jeu de résultats). La valeur est 0, c’est le cas skipItems = 0 (ou simplement non-spécification skipItems) commence la récupération au début de la liste. skipItems = 3 serait ignorer les trois premiers éléments dans la liste et commencer la récupération avec le quatrième élément. |
  | subscriptionLevel | tableau de chaînes | Paramètre de filtre facultatif. Le paramètre subscriptionLevel détermine que le type d’abonnement de l’utilisateur a (par exemple, si un utilisateur possède un abonnement payant ou un abonnement gratuit). Les valeurs possibles sont les suivantes. <ul><li>Gold : L’utilisateur dispose d’un abonnement payant</li><li>Silver : L’utilisateur dispose d’un abonnement gratuit.</li></ul>  |
| TargetDevices| chaîne| EDS offre la possibilité de filtrer pour les appareils cibles. Les offres (ProviderContent ou la disponibilité) retournés pour l’élément peuvent être limités à un appareil cible.|
| topRatedOnly| Valeur booléenne| Paramètre de filtre facultatif. Limite les résultats uniquement le contenu de meilleurs.|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Parent

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>Informations supplémentaires

[URI de la place de marché](../uri/marketplace/atoc-reference-marketplace.md)
