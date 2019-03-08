---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640854"
---
# <a name="post-itemid"></a>POST ({itemID})
Indique que tout ou partie d’un article en stock consommable a été utilisée et décrémente la quantité de la consommable par le montant demandé.
Le domaine pour ces URI est `inventory.xboxlive.com`.

  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EQB)
  * [Corps de la demande](#ID4E2B)
  * [Corps de la réponse](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Notes

   * Si la quantité demandé par l’appelant pour consommer dépasse l’approvisionnement restante de l’élément, l’appel va être rejeté.
   * La quantité demandé à consommer l’appelant doit être un entier positif supérieur à 0. Appels avec une valeur de la consommation inférieure ou égale à 0 sont rejetées.
   * Si l’appelant fournit un ID de Transaction vide, la demande sera rejetée.
   * S’il est disponible est consignée la revendication de titre afin qu’il soit possible de déterminer quel titre signale la consommation.
   * Des publications supplémentaires avec la même transactionId seront ignorées pendant un certain temps.


> [!NOTE]
> Le <b>en-tête x-xbl-contrat-version</b> pour cette API est « 4 ».


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| itemID| chaîne| l’ID unique pour chaque utilisateur pour un article en stock au singulier|

<a id="ID4E2B"></a>


## <a name="request-body"></a>Corps de la requête

<a id="ID4EBC"></a>


### <a name="sample-request"></a>Exemple de demande


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


Le champ quantity remove permet à l’appelant indiquer la quantité de consommables qu’ils souhaitent supprimer à partir de la quantité restante de la consommables. Le champ ID de Transaction fournit à l’appelant une méthode pour réessayer en utilisant les opérations de contenu consommables tout en limitant le risque de comptage de deux fois la même utilisation.

<a id="ID4ENC"></a>


## <a name="response-body"></a>Corps de la réponse

La réponse à la publication, en supposant qu’elle transmet l’authentification et que vous est attribué le contexte d’autorisation appropriée est un un acknolodgement de réception avec le même transactionId transmis au service dans la requête POST, URL de l’élément consommable et de l’élément nouveau valeur de quantité.

<a id="ID4EVC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EBD"></a>


##### <a name="parent"></a>Parent

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>Informations supplémentaires

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)
