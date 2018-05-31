---
title: Paramètres de création de ressources de diffusion en continu
description: Il existe certaines contraintes quant au type de ressources Direct3D que vous pouvez créer en tant que ressource de diffusion en continu.
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords:
- Paramètres de création de ressources de diffusion en continu
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2a17f69ae0353bb7682a1dbfa48d5909f48d4aa
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652988"
---
# <a name="streaming-resource-creation-parameters"></a>Paramètres de création de ressources de diffusion en continu


Il existe certaines contraintes quant au type de ressources Direct3D que vous pouvez créer en tant que ressource de diffusion en continu.

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**Type de ressource pris en charge**  
Texture2D\ [Array\] (y compris TextureCube\[Array\], qui est une variante de Texture2D\[Array\]) ou d'une mémoire tampon.

**NON pris en charge: **Texture1D\ [Array\].

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**Utilisation des ressources prises en charge**  
Utilisation par défaut.

**NON pris en charge: **dynamique, intermédiaire ou non modifiable.

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**Indicateurs de diverses ressources prises en charge**  
Sous forme de vignettes; autrement dit, diffusion en continu (par définition), cube de texture, dessiner des arguments indirects, la mémoire tampon autorise les vues brutes, mémoire tampon structurée, limitation de ressource ou générer des mips.

**NON pris en charge: **partagé, mutex à clé partagée, compatible avec GDI, descripteur NT partagé, contenu restreint, limiter les ressources partagées, limiter le pilote des ressources partagées, protégés ou pool de vignettes.

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**Indicateurs de liaison pris en charge**  
Liaison en tant que ressource de nuanceur, cible de rendu, gabarit de profondeur ou accès sans ordre.

**NON pris en charge: **liaison en tant que mémoire tampon constante, mémoire tampon de vertex (la liaison en tant que tampon sous forme de vignettes SRV/UAV/RTV est prise en charge), mémoire tampon d'index, sortie de flux, décodeur ou encodeur vidéo.

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**Formats pris en charge**  
Tous les formats disponibles pour la configuration indiquée, qu'elle soit sous forme de vignettes ou non, à quelques exceptions près.

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**Description de l'échantillon pris en charge (nombre d’échantillonnages multiples, qualité)**  
Tout les éléments qui seraient pris en charge pour la configuration donnée, qu'elle soit sous forme de vignettes ou non, à quelques exceptions près.

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**Largeur/hauteur/niveaux MIP/Taille du tableau pris en charge**  
Extensions complètes prises en charge par Direct3D. Les ressources de diffusion en continu n’ont aucune restriction en ce qui concerne la taille de la mémoire totale imposée sur les ressources qui ne sont pas diffusées en continu. Les ressources de diffusion en continu sont limitées uniquement par les limites globales de l’espace d'adresse virtuelle. Voir [Espace d’adresse disponible pour les ressources de diffusion en continu](address-space-available-for-streaming-resources.md).

Le contenu initial de la mémoire du pool de vignettes n’est pas défini.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="address-space-available-for-streaming-resources.md">Espace d’adresse disponible pour les ressources de diffusion en continu</a></p></td>
<td align="left"><p>Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 




