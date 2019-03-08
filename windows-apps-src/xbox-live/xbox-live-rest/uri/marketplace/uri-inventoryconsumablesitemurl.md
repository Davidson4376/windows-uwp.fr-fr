---
title: /users/me/consumables/{itemID}
assetID: 45724827-5e35-326f-3f17-f49e606d9e08
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurl.html
description: Point de terminaison rESTful pour Xbox des éléments consommables pour un utilisateur.
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4822180f11879417be67351f901474a38f89d82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614084"
---
# <a name="usersmeconsumablesitemid"></a>/users/me/consumables/{itemID}
Accède à l’ensemble complet des détails d’un élément spécifique de stock consommable.
Le domaine pour ces URI est `inventory.xboxlive.com`.

  * [Paramètres d’URI](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| itemID| chaîne| l’ID unique pour chaque utilisateur pour un article en stock au singulier|

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>Méthodes valides

[POST ({itemID})](uri-inventoryconsumablesitemurlpost.md)

&nbsp;&nbsp;Indique que tout ou partie d’un article en stock consommable a été utilisée et décrémente la quantité de la consommable par le montant demandé.

<a id="ID4E4B"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent

[URI de la place de marché](atoc-reference-marketplace.md)


<a id="ID4EJC"></a>


##### <a name="further-information"></a>Informations supplémentaires

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)
