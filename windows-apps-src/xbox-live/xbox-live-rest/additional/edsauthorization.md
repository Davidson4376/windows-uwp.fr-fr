---
title: Autorisation EDS
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " Autorisation EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607604"
---
# <a name="eds-authorization"></a>Autorisation EDS
 
  * [Introduction](#ID4EN)
  * [Processus d’autorisation](#ID4EFB)
  * [3.0 jetons de : Vs multi-utilisateur. Utilisateur unique](#ID4EEC)
  * [EDS prend en charge plusieurs utilisateurs ?](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>Introduction
 
Services de découverte de divertissement (EDS) 3.1 ne prendra pas en charge le trafic anonyme. L’authentification est requise pour toutes les demandes à EDS. EDS nécessitera un XToken provenant d’appelants pour authentifier correctement les clients. Ces jetons sont produits par XSTS et peuvent être obtenus via divers Services d’authentification Xbox (XAS). Il existe des services d’authentification distinct pour les appareils, les utilisateurs et les titres qui tous définit l’identité du jeton.
 
XSTS est l’opérateur de contrôle pour Xbox LIVE. Il est la première ligne de défense pour déterminer si un utilisateur ou un appareil est autorisé à se connecter à l’un des services Xbox LIVE. Après avoir authentifié l’utilisateur, le XSTS génère un XToken qu’ils peuvent utiliser pour s’identifier en toute sécurité à n’importe quel composant sur le service. Cette XToken est votre passeport de vie.
 
Personnes et les choses, à utiliser nos services. Et nous voulons la plupart de ces éléments et les personnes pour pouvoir utiliser nos services. Mais comment nous assurer que les choses ne sont pas prétendant être des personnes, et que les personnes sont réellement qui prétendent le qu'être ? Nous fournissons les jetons qui peut les utiliser pour s’identifier auprès d’autres.
 
Ces jetons sont produits par le XSTS et sont généralement appelés XTokens. XToken est un terme général qui est utilisé pour couvrir les jetons qui contiennent une variété de choses différentes et peuvent provenir de nombreuses formes différentes, mais elles sont toutes créées, signés et éventuellement chiffrés par le serveur XSTS. En interne, XSTS utilise MXAN pour créer et formater les jetons. MXAN est le seul composant qui extrait des informations à partir d’un XToken de jamais. Services de consommation les jetons de passer les en-têtes de demande à MXAN à décoder. Les jetons sont traitées et validées, et les revendications exprimées dans le jeton sont retournées au service. Le service peut ensuite l’utiliser que ces valeurs pour identifier l’utilisateur appelant ou l’appareil et effectuer des actions en fonction de ces informations de revendication.
 
Les jetons d’identité de base - pour le titre -, l’appareil et utilisateur sont fournis par les Services d’authentification Xbox (XAS). Chaque XAS est chargé de générer un jeton d’identité qui spécifie les valeurs pour les différentes revendications qui sont responsables.
 
   * XASD (XAS pour les appareils) : crée un DToken qui fournit une identité d’appareil
   * XASU (XAS pour les utilisateurs) : crée un UToken qui fournit une identité d’utilisateur
   * XAST (XAS pour les titres) : crée un TToken qui fournit une identité de titre
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>Processus d’autorisation
 
   * Obtenir un ou plusieurs jetons d’identité. Vous pouvez demander un XToken avec au maximum un chaque jetons D, U et T. Vous devez fournir au moins une des D ou U. 
     * Demandez un DToken XASD en fournissant les détails de l’authentification d’appareil
     * Demander un UToken XASU avec une certaine forme d’authentification de l’utilisateur. L’authentification des utilisateurs est probablement fournie sous la forme d’un jeton de compte de service administré (RPS).
     * Demander un TToken XAST. Les titres qui sont disponibles varient selon la plateforme en cours d’exécution afin de vous devez fournir un DToken à XAST également.
  
   * Créer une demande XSTS.
 
     * Définir la partie de confiance que vous demandez un jeton pour.
     * Remplir les propriétés de la demande avec les jetons D, U ou T.
    * Exécuter la demande de XSTS et mettre en cache le XToken résultant. Le XToken retourné contient (au plus) toutes les informations d’identité de titre, l’utilisateur et appareil à partir des jetons d’identité et les éventuelles revendications supplémentaires (état actuel de l’abonnement, groupes d’utilisateurs, etc.).
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 jetons de : Vs multi-utilisateur. Utilisateur unique
 
Il s’agit de la forme du jeton 3.0 : `XBL3.0 x=&lt;hash>;&lt;token>`
 
Selon le &lt;hachage >, le jeton est traité différemment :
 
   * Si &lt;hachage > est égal à * (astérisque), aucun utilisateur particulier n’est effectuant la demande et tous les utilisateurs dans le jeton sont présents dans l’entité de sécurité désérialisée. Il s’agit de la forme multi-utilisateur true.
   * Si &lt;hachage > est égal à - (tiret), aucun utilisateur n’est effectuant la demande. Tous les utilisateurs dans l’entité de sécurité désérialisée sont supprimés.
   * Si &lt;hachage > n’est pas égal à * ou - il est un identificateur indiquant à quel utilisateur dans le jeton effectue la demande. Seul l’utilisateur indiqué sera présent dans l’entité de sécurité désérialisée. Tous les autres utilisateurs sont supprimés. Il s’agit le jeton d’utilisateur unique 3.0.
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS prend en charge plusieurs utilisateurs ?
 * La réponse est aucune. Dans le cas a été décrite, la console sera toujours envoyer des jetons d’utilisateur unique. Même s’il existe plusieurs utilisateurs connectés, il doit être un indiqué « appelant », où toutes les autres identités sont supprimées.
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[URI de la place de marché](../uri/marketplace/atoc-reference-marketplace.md)

   