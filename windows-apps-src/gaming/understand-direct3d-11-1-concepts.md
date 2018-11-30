---
title: Modifications importantes de Direct3D9 à Direct3D11
description: Cette rubrique décrit les principales différences entre DirectX9 et DirectX11.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx,, direct3d9, direct3d11, changements
ms.localizationpriority: medium
ms.openlocfilehash: ecdd8591efb3920d2cfe333aa8ec02c65c1a1465
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8216020"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Modifications importantes de Direct3D9 à Direct3D11



**Résumé**

-   [Planifier votre portage DirectX](plan-your-directx-port.md)
-   Modifications importantes de Direct3D 9 à Direct3D 11
-   [Mappage des fonctionnalités](feature-mapping.md)


Cette rubrique décrit les principales différences entre DirectX9 et DirectX11.

Direct3D 11 correspond fondamentalement au même type d’API que Direct3D 9 : une interface virtualisée de bas niveau dans du matériel vidéo. Il vous permet encore d’effectuer des opérations de dessin graphique sur une variété d’implémentations matérielles. La disposition de l’API graphique a changé depuis Direct3D 9 ; le concept de contexte de périphérique a été étendu et une API a été ajoutée particulièrement pour l’infrastructure graphique. Les ressources stockées sur le périphérique Direct3D disposent d’un mécanisme original pour le polymorphisme des données appelé affichage des ressources.

## <a name="core-api-functions"></a>Principales fonctions d’API


Dans Direct3D 9, vous deviez créer une interface vers l’API Direct3D avant de pouvoir commencer à l’utiliser. Dans votre jeu de plateforme Windows universelle (UWP) Direct3D 11, vous appelez une fonction statique appelée [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) pour créer le périphérique et le contexte de périphérique.

## <a name="devices-and-device-context"></a>Périphériques et contexte de périphérique


Un périphérique Direct3D 11 représente une carte graphique virtualisée. Il sert à créer des ressources dans la mémoire vidéo, par exemple, à télécharger des textures vers le GPU, à créer des affichages sur les ressources de textures et les chaînes d’échange et à créer des échantillons de textures. Pour obtenir la liste complète des utilisations d’une interface de périphérique Direct3D 11, voir [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) et [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575).

Un contexte de périphérique Direct3D 11 sert à définir l’état du pipeline et à générer des commandes de rendu. Par exemple, une chaîne de rendu Direct3D 11 utilise un contexte de périphérique pour configurer la chaîne de rendu et dessiner la scène (voir ci-dessous). Le contexte de périphérique est utilisé pour accéder (mapper) la mémoire vidéo utilisée par les ressources de périphérique Direct3D ; il sert également à mettre à jour les données de sous-ressources, par exemple les données de tampons constants. Pour obtenir la liste complète des utilisations du contexte de périphérique Direct3D 11, voir [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) et [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). Notez que la plupart de nos exemples utilisent un contexte immédiat pour générer directement le rendu sur le périphérique, mais que Direct3D 11 prend également en charge des contextes de périphérique différés, lesquels sont principalement utilisés pour le multithreading.

Dans Direct3D 11, le handle de périphérique et le handle de contexte de périphérique sont tous les deux obtenus en appelant la fonction [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Cette méthode permet également de demander un ensemble spécifique de caractéristiques matérielles et de récupérer des informations sur les niveaux de fonctionnalité Direct3D pris en charge par la carte graphique. Voir [Présentation d’un périphérique dans Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476880) pour plus d’informations sur les périphériques, les contextes de périphérique et les considérations liées au thread.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>Infrastructure de périphérique, tampons de trame et affichages de cibles de rendu


Dans Direct3D 11, la carte de périphérique et la configuration matérielle sont définies avec l’API DXGI (DirectX Graphics Infrastructure) à l’aide des interfaces COM [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) et [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543). Les tampons et autres ressources de fenêtre (visibles ou hors écran) sont créés et configurés par des interfaces DXGI spécifiques ; l’implémentation du modèle de fabrique [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) acquiert les ressources DXGI telles que le tampon de trame. Étant donné que DXGI possède la chaîne d’échange, une interface DXGI est utilisée pour présenter les trames à l’écran ; voir [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631).

Utilisez [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) pour créer une chaîne d’échange compatible avec votre jeu. Vous devez créer une chaîne de permutation pour une fenêtre principale, ou pour une composition (technologie interop XAML), plutôt que de créer une chaîne de permutation pour un HWND.

## <a name="device-resources-and-resource-views"></a>Ressources de périphérique et affichages des ressources


Direct3D 11 prend en charge un niveau supplémentaire de polymorphisme sur les ressources de mémoire vidéo appelées vues. En gros, quand vous aviez un objet Direct3D 9 unique pour une texture, vous avez maintenant deux objets : la ressource de texture, qui contient les données et l’affichage des ressources, qui indique comment la vue est utilisée pour le rendu. Une vue basée sur une ressource permet à cette ressource d’être utilisée dans un but spécifique. Par exemple, une ressource de texture en 2D est créée en tant que [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635), puis un affichage des ressources de nuanceur ([**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)) est créé par-dessus pour être utilisable en tant que texture dans un nuanceur. Une vue de cible de rendu ([**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)) peut également être créée sur la même ressource de texture en 2D afin d’être utilisée en tant que surface de dessin. Dans un autre exemple, les mêmes données de pixel sont représentées dans 2formats de pixel différents à l’aide de 2affichages distincts sur une seule ressource de texture.

La ressource sous-jacente doit être créée avec des propriétés compatibles avec les types d’affichages qui seront créés à partir de cette ressource. Par exemple, si un élément [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) est appliqué à une surface, cette surface est créée avec l’indicateur [**D3D11\_BIND\_RENDER\_TARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476085). La surface doit également présenter un format de surface DXGI compatible avec le rendu (voir [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)).

La plupart des ressources que vous utilisez pour le rendu héritent de l’interface [**ID3D11Resource**](https://msdn.microsoft.com/library/windows/desktop/ff476584), laquelle hérite de [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380). Les tampons vertex, les tampons d’index, les mémoires tampons constantes et les nuanceurs sont tous des ressources Direct3D11. Les dispositions d’entrée et les états d’échantillonneur héritent directement de [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380).

Les affichages des ressources utilisent une valeur d’énumération DXGI\_FORMAT pour indiquer le format de pixel. Certains D3DFMT ne sont pas pris en charge en tant que DXGI_FORMAT. Par exemple, il n’existe pas de format RVB 24bpp dans DXGI équivalent à D3DFMT_R8G8B8. Il n’existe pas non plus d’équivalents BGR à chaque format RVB (DXGI\_FORMAT\_R10G10B10A2\_UNORM équivaut à D3DFMT\_A2B10G10R10, mais il n’existe aucun équivalent direct à D3DFMT\_A2R10G10B10). Tout contenu présentant l’un de ces formats hérités doit être converti dans un format pris en charge au moment de la création. Pour obtenir la liste complète des formats DXGI, voir l’énumération [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059).

Les ressources de périphérique Direct3D (et les affichages des ressources) sont créés avant la génération du rendu de la scène. Les contextes de périphérique sont utilisés pour configurer la chaîne de rendu, comme expliqué ci-dessous.

## <a name="device-context-and-the-rendering-chain"></a>Contexte de périphérique et chaîne de rendu


Dans Direct3D 9 et Direct3D 10.x, il existait un seul objet de périphérique Direct3D qui gérait la création des ressources, l’état et le dessin. Dans Direct3D 11, l’interface de périphérique Direct3D gère encore la création des ressources, mais toutes les opérations d’état et de dessin sont gérées à l’aide d’un contexte de périphérique Direct3D. Voici un exemple d’utilisation du contexte de périphérique (interface [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) pour configurer la chaîne de rendu :

-   Définir et effacer les vues de cibles de rendu (et la vue de profondeur/gabarit)
-   Définir le tampon de vertex, le tampon d’index et le schéma d’entrée pour le stade d’assembleur d’entrée (stade IA)
-   Lier les nuanceurs de vertex et de pixels au pipeline
-   Lier les tampons constants aux nuanceurs
-   Lier les vues de textures et les échantillons au nuanceur de pixels
-   Dessiner la scène

Quand l’une des méthodes [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) est appelée, la scène est dessinée sur l’affichage de la cible de rendu. Quand vous avez terminé tout le dessin, la carte DXGI est utilisée pour présenter la trame complète en appelant la méthode [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).

## <a name="state-management"></a>Gestion de l’état


Direct3D 9 gérait les paramètres d’état avec un large ensemble de bascules individuelles défini à l’aide des méthodes SetRenderState, SetSamplerState et SetTextureStageState. Étant donné que Direct3D 11 ne prend pas en charge le pipeline à fonction fixe hérité, la méthode SetTextureStageState est remplacée par l’écriture de nuanceurs de pixels. Il n’existe pas d’équivalent à un bloc d’état Direct3D 9. Direct3D11 gère l’état par le biais de l’utilisation de 4types d’objets d’état qui offrent un moyen plus rationalisé de regrouper l’état de rendu.

Par exemple, au lieu d’utiliser SetRenderState avec D3DRS\_ZENABLE, vous créez un objet DepthStencilState avec ce paramètre et d’autres paramètres d’état associés, et vous l’utilisez pour modifier l’état pendant le rendu.

Quand vous portez des applications Direct3D9 vers des objets d’état, sachez que vos diverses combinaisons d’état sont représentées sous forme d’objets d’état inaltérables. Ils doivent être créés une seule fois et réutilisés tant qu’ils sont valides.

## <a name="direct3d-feature-levels"></a>Niveaux de fonctionnalités Direct3D


Direct3D possède un nouveau mécanisme pour déterminer la prise en charge matérielle, appelé niveaux de fonctionnalités. Les niveaux de fonctionnalités simplifient la tâche de détermination de ce que peut faire la carte graphique en vous permettant de demander un ensemble bien défini de fonctionnalités GPU. Par exemple, le niveau de fonctionnalité 9\_1 implémente la fonctionnalité fournie par les cartes graphiques Direct3D9, notamment le modèle de nuanceur2.x. Étant donné que 9\_1 correspond au niveau de fonctionnalité le plus bas, vous pouvez vous attendre à ce que tous les périphériques prennent en charge un nuanceur de vertex et un nuanceur de pixels, ce qui correspondait aux mêmes stades pris en charge par le modèle de nuanceur programmable de Direct3D9.

Votre jeu utilise la fonction [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) pour créer le périphérique Direct3D et le contexte de périphérique. Quand vous appelez cette fonction, vous fournissez la liste des niveaux de fonctionnalités que votre jeu peut prendre en charge. Elle renvoie le niveau de fonctionnalité pris en charge le plus élevé à partir de cette liste. Par exemple, si votre jeu peut utiliser des textures BC4/BC5 (une caractéristique du matériel DirectX10), vous incluez au moins 9\_1 et 10\_0 dans la liste des niveaux de fonctionnalités pris en charge. Si le jeu s’exécute sur du matériel DirectX 9 et que les textures BC4/BC5 ne peuvent pas être utilisées, [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) renvoie 9\_1. Votre jeu peut alors revenir à un format de texture différent (et à des textures plus petites).

Si vous décidez d’étendre votre jeu Direct3D 9 pour prendre en charge des niveaux de fonctionnalités Direct3D supérieurs, alors il est préférable de d’abord terminer le portage de votre code graphique Direct3D 9 existant. Une fois que votre jeu fonctionne dans Direct3D 11, il est plus facile d’ajouter des chemins de rendu supplémentaires avec des graphiques améliorés.

Pour obtenir une explication détaillée de la prise en charge des niveaux de fonctionnalités, voir [Niveaux de fonctionnalités Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876). Voir [Fonctionnalités de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476342) et [Fonctionnalités de Direct3D 11.1](https://msdn.microsoft.com/library/windows/desktop/hh404562) pour obtenir la liste complète des fonctionnalités de Direct3D 11.

## <a name="feature-levels-and-the-programmable-pipeline"></a>Niveaux de fonctionnalités et pipeline programmable


Le matériel a continué à évoluer depuis Direct3D 9 et plusieurs nouveaux stades facultatifs ont été ajoutés au pipeline graphique programmable. L’ensemble d’options dont vous disposez pour le pipeline graphique varie avec le niveau de fonctionnalité Direct3D. Le niveau de fonctionnalité 10.0 inclut le stade de nuanceur de géométrie avec flux facultatif pour le rendu multipasse sur le GPU. Le niveau de fonctionnalité 11\_0 inclut le nuanceur de coque et le nuanceur de domaine à utiliser pour le pavage matériel. Le niveau de fonctionnalité 11\_0 inclut également une prise en charge complète des nuanceurs DirectCompute, tandis que les niveaux de fonctionnalités 10.x incluent uniquement la prise en charge d’une forme limitée de DirectCompute.

Tous les nuanceurs sont écrits en HLSL à l’aide d’un profil de nuanceur qui correspond à un niveau de fonctionnalité Direct3D. Les profils de nuanceur ont une compatibilité ascendante, si bien qu’un nuanceur HLSL qui compile avec vs\_4\_0\_level\_9\_1 ou ps\_4\_0\_level\_9\_1 fonctionne sur tous les périphériques. Les profils de nuanceur n’ont pas de compatibilité descendante, de sorte qu’un nuanceur compilé avec vs\_4\_1 fonctionne uniquement sur les périphériques 10\_1, 11\_0 ou 11\_1.

Direct3D9 gérait les constantes des nuanceurs en utilisant un tableau partagé avec SetVertexShaderConstant et SetPixelShaderConstant. Direct3D 11 utilise des tampons constants, lesquels sont des ressources comme un tampon de vertex ou un tampon d’index. Les tampons constants sont conçus pour être mis à jour de manière efficace. Au lieu d’organiser toutes les constantes de nuanceurs dans un seul tableau global, vous les organisez dans des groupes logiques et les gérez via un ou plusieurs tampons constants. Quand vous portez votre jeu Direct3D 9 vers Direct3D 11, envisagez d’organiser vos tampons constants afin de pouvoir les mettre à jour de manière appropriée. Par exemple, regroupez les constantes de nuanceurs dont toutes les trames ne sont pas mises à jour dans une mémoire tampon constante distincte, afin de ne pas avoir à charger constamment ces données sur la carte graphique avec vos constantes de nuanceurs plus dynamiques.

> **Remarque**  plus de Direct3D 9 applications mises à une utilisation intensive des nuanceurs, mais parfois mixte en cours d’utilisation du comportement de fonction fixe hérité. Notez que Direct3D11 utilise uniquement un modèle de nuanceur programmable. Les fonctionnalités de fonction fixe héritées de Direct3D 9 sont obsolètes.

 

 

 




