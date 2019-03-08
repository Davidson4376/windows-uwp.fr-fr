---
title: URI du gamerpic
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " URI du gamerpic"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618384"
---
# <a name="gamerpic-uris"></a>URI du gamerpic
 
Cette section fournit des détails sur les adresses de Gamerpic identificateur URI (Universal Resource) et les méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour *gamerpics*.
 
Le domaine pour ces URI est `gamerpics.xboxlive.com`.
 
Le Gamerpic Service a été conçu pour permettre aux utilisateurs plusieurs options de personnalisation, en lui accordant un titre de la possibilité d’autoriser l’utilisateur Générer un gamerpic de leur caractère du jeu (un caractère du jeu dans ce scénario fait référence à un postule dans le jeu ; il peut être une personne une voiture, un vaisseau spatial ou toute autre entité contrôlé par l’utilisateur dans le titre).
 
Le flux de base de générer un gamerpic title est comme suit :
 
   * Le titre fournit à l’utilisateur avec la possibilité de créer une image de leur caractère dans le jeu. 
     * Si ce n’est pas le cas, le titre du message peut ensuite l’utilisateur qu’ils n’ont pas les privilèges appropriés.
     * Si l’utilisateur ne dispose pas du privilège, l’utilisateur peut continuer à créer leur gamerpic caractère.
  
   * L’utilisateur crée l’image et le titre envoie le fichier .png 1080 x 1080 au service gamerpic.
   * Le service stocke l’image et définit l’image en tant que gamerpic de nouveau l’utilisateur.
   * N’importe quel appel pour gamerpic de l’utilisateur des expériences obtiendra l’image mise à jour.
  
La possibilité de définir un gamerpic title est contrôlée par un privilège de la mise en œuvre uniquement (211). Si la mise en œuvre révoque le privilège, l’utilisateur ne peuvent pas enregistrer un gamerpic titre et le service renverra 403. Titres doivent appeler CheckPrivilege pour vérifier que les utilisateurs sont autorisés à partager du contenu (priv 211).
 
Actuellement, pour pouvoir utiliser ce service, votre titre doit être dans la liste verte. Pour demander l’approbation, envoyer par courrier électronique `slsgamerpics@microsoft.com`.
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;Accède à un gamerpic 1080 x 1080.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   