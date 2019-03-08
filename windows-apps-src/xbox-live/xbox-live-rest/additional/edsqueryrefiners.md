---
title: Affinements de requête EDS
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " Affinements de requête EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611464"
---
# <a name="eds-query-refiners"></a>Affinements de requête EDS
 
<a id="ID4EO"></a>

  
 
Les paramètres suivants peuvent être utilisés pour affiner une requête de Services de découverte de divertissement (EDS) à un ensemble plus ciblée d’éléments. Aucun de ces paramètres sont requis dans n’importe quelle API, mais ils sont acceptés dans n’importe quelle API acceptant raffineurs de requête.
 
Les noms de paramètre peuvent être passés en tant que valeurs dans n’importe quel paramètre « queryRefiners ». Ce sera ensuite le retour appliqué par le nombre d’éléments qui serait retournée si la demande ont été répétée avec affinement de la requête, ventilées par chaque valeur de l’affinement de la requête.
 
Voici comment cela pourrait fonctionner dans la pratique :
 
   * Un appel à l’API de navigation est effectué, y compris le paramètre « queryRefiners = genre ».
   * L’API retourne des jeux de huit. Outre les éléments, une liste de chaque genre ayant des éléments s’affichera, ainsi que le nombre d’éléments appartiennent à ce genre s’affiche. Pour un jeu, cela peut être « Shooter : 3, puzzle : 5".
   * Une deuxième requête est effectuée. Il est identique à la première, sauf que « genre = Shooter » est ajouté.
   * La réponse contient désormais uniquement trois jeux, qui appartiennent à la catégorie « Shooter ».
  
| Paramètre| Type de données| Description| 
| --- | --- | --- | 
| <b>decade</b>| chaîne| La décennie dans lequel tous les éléments doivent ont été publiées.| 
| <b>genre</b>| tableau de chaînes| La liste des genres tous les éléments doivent avoir.| 
| <b>labelOwner</b>| chaîne| L’étiquette de musique associée de l’artiste, album ou piste.| 
| <b>network</b>| tableau de chaînes| Le réseau qui a créé les éléments.| 
| <b>studio</b>| tableau de chaînes| Studio qui a créé les éléments.| 
| <b>xboxAppCategories</b>| tableau de chaînes| La liste des catégories que toutes les applications Xbox doit avoir.| 
| <b>xboxAvatarClothes</b>| tableau de chaînes| La liste des types de vêtements tous les éléments de Xbox Avatar doivent avoir.| 
| <b>xboxAvatarStores</b>| tableau de chaînes| La liste des magasins qui Xbox tous les éléments de l’avatar doivent appartenir.| 
| <b>xboxGamePublisherBits</b>| tableau de chaînes| La liste des bits de jeu de serveur de publication doit être définie sur tous les éléments de type de partie ou des éléments de type d’application.| 
| <b>xboxIsBrowsable</b>| Valeur booléenne| Si <b>true</b>, retournera des jeux complètes, qui ne sont pas directement exploitables en plus du contenu pertinent. Valeur par défaut est <b>false</b>.| 
| <b>xboxHasChildMediaItemTypes</b>| tableau de chaînes| Tous les éléments renvoyés avec un groupe de support de jeu doivent comporter aucun enfant dont le type média est une des valeurs fournies.| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[URI de la place de marché](../uri/marketplace/atoc-reference-marketplace.md)

   