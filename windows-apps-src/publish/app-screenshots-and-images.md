---
author: jnHs
Description: "Vous pouvez sélectionner les captures d’écran, les logos et d'autres ressources d'illustration (par exemple, des bandes-annonces et des images publicitaires) à inclure dans votre description de l’application dans le Windows Store."
title: "Captures d’écran, images et bandes-annonces de l’application"
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 7d5da868a93f958a9432e7f8360129a8be8ca486
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2017
---
# <a name="app-screenshots-images-and-trailers"></a>Captures d’écran, images et bandes-annonces de l’application

Des images bien conçues constituent l’un des principaux moyens dont vous disposez pour présenter votre application aux clients potentiels dans le WindowsStore.

Vous pouvez fournir des [captures d’écran](#screenshots), des [logos](#store-logos) et d’autres ressources d’illustration (telles que des [bandes-annonces](#trailers) et des [images promotionnelles](##additional-art-assets)) à inclure dans la description de votre application dans le WindowsStore. Certains de ces éléments sont requis, tandis que d’autres sont facultatifs (bien qu’il soit recommandé d’inclure certaines images facultatives afin d’optimiser l’affichage de votre application dans le WindowsStore). 

Lors du [processus de soumission de l’application](app-submissions.md), vous fournissez ces ressources d’illustration à l’étape [Descriptions dans le WindowsStore](create-app-store-listings.md). Notez que les images qui sont utilisées dans le WindowsStore et la façon dont elles apparaissent peuvent varier selon le système d’exploitation du client et d’autres facteurs.

Le Windows Store peut également utiliser la vignette et d’autres images de votre application que vous incluez dans le package de votre application. Exécutez le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) pour déterminer si une image requise est manquante avant de soumettre votre application. Pour obtenir des conseils et des recommandations sur ces images, voir [Ressources de vignette et d’icône](../controls-and-patterns/tiles-and-notifications-app-assets.md).

## <a name="screenshots"></a>Captures d’écran

Les captures d’écran sont les images de votre application que voient vos clients dans la description du Windows Store.

Vous avez la possibilité de fournir des captures d’écran pour différentes familles d’appareils, afin que les captures appropriées apparaissent lorsqu’un client affiche la description de votre application dans le WindowsStore sur ce type d’appareil. 

La soumission de votre application ne nécessite qu’une seule capture d’écran (pour une famille d’appareils quelle qu’elle soit), mais vous pouvez en fournir plusieurs, jusqu’à9 pour les ordinateurs de bureau et8 pour les autres familles d’appareils. Nous vous recommandons de fournir au moins quatre captures d’écran pour chaque famille d’appareils prise en charge par votre application afin que les utilisateurs puissent voir à quoi cette dernière ressemblera sur leur type d’appareil.

> [!NOTE]
> Microsoft VisualStudio contient un [outil vous permettant d’effectuer des captures d’écran](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Chaque capture d’écran doit être au format .png en mode paysage ou portrait, et la taille du fichier ne doit pas dépasser 5Mo.

Les exigences de taille varient en fonction de la famille d’appareils:
- Mobile: 768x1280, 720x1280 ou 480x800pixels
- Bureau: 1366x768 pixels ou plus
- Holographique: 1268x720pixels ou plus
- Xbox: 3480x2160pixels ou moins

Pour optimiser l’affichage, gardez les instructions ci-après à l’esprit lorsque vous créez vos captures d’écran:
- Placez les visuels et le texte essentiels dans les trois quarts supérieurs de l’image. Un texte superposé peut apparaître dans le quart inférieur. 
- N’ajoutez pas de logos, d'icônes ou de messages marketing supplémentaires sur vos captures d’écran.
- N’utilisez pas de couleurs très claires ou très sombres, ni de bandes à fort contraste qui peuvent interférer avec la lisibilité du texte superposé.

Vous pouvez également fournir un bref sous-titre pour chaque capture d’écran (au maximum 200caractères).

> [!TIP]
> Les captures d’écran sont affichés dans votre description dans l’ordre. Une fois que vous téléchargez vos captures d’écran, vous pouvez les glisser-déplacer pour les réorganiser. 

Notez que si vous créez des descriptions dans le Windows Store pour [plusieurs langues](supported-languages.md), vous disposez d’une page **Description dans le Windows Store** pour chacune d’elles. Vous devez charger des images pour chaque langue séparément (même si vous utilisez les mêmes images), et fournir des sous-titres à utiliser pour chaque langue.


## <a name="store-logos"></a>Logos Windows Store

Les logos WindowsStore sont des images facultatives que vous pouvez charger pour personnaliser l’affichage dans le WindowsStore. Nous vous recommandons de fournir ces images pour optimiser l’affichage de vos descriptions dans le WindowsStore sur l’ensemble des appareils et des versions de système d’exploitation pris en charge par votre application.

Vous pouvez fournir ces images sous la forme de fichiers.png qui doivent appliquer les instructions ci-dessous:

- **9:16 (720x1080 ou 1440x2160pixels)**: ce format est utilisé comme image principale sur les descriptions du WindowsStore pour les clients qui utilisent Windows10 et les appareils Xbox. Nous vous conseillons donc vivement de fournir cette image pour optimiser l’affichage de votre description. L’image doit inclure le nom de votre application, et le texte présent sur l’image doit respecter les exigences de lisibilité et d’accessibilité (taux de contraste de4,5 pour1).  
- **1:1 (1080x1080 ou 2160x2160pixels)**: cette image peut apparaître dans certaines dispositions. Si vous la fournissez, veillez à y inclure le nom de votre application.
- **1:1 (300x300pixels)**: cette image, parfois appelée **Icône de vignette de l’application**, est utilisée lors de l’affichage de la description de votre application dans le WindowsStore pour les clients qui utilisent WindowsPhone8.1 ou les versions antérieures. Si vous ne fournissez pas cette image, les clients sur Windows Phone8.1 ou versions antérieures verront une icône vide associée à la description de votre application. (Ceci s’applique également aux clients sur Windows10, si votre application ne comporte que des packages ciblant WindowsPhone8.1 ou les versions antérieures.) Si votre soumission inclut *uniquement* des packages UWP, vous n’avez pas besoin de fournir cette image. (Notez que si votre soumission comprend à la fois des packages Windows Phone8.x et des packages UWP et que vous fournissez cette image, celle-ci peut être utilisée sur Windows10dans certaines dispositions du Windows Store. Pour éviter cela, vous pouvez créer une [description propre à la plateforme](create-platform-specific-store-listings.md) pour les versions de WindowsPhone OS et n’inclure à cet emplacement que l’icône de vignette de l’application.)

Lorsque vous présentez votre description aux clients Windows10, vous avez également la possibilité d’empêcher le WindowsStore d’utiliser les images de logo disponibles dans les packages de votre application et de faire en sorte qu’il utilise uniquement les images que vous chargez. Cette opération vous permet de mieux contrôler l’apparence de votre application sur différents affichages partout dans le WindowsStore sur Windows10.

Pour utiliser uniquement des images chargées pour l’affichage dans le WindowsStore sur Windows10, cochez la case indiquant **Pour les clients Windows10, afficher les images de logo chargées au lieu des images de mes packages**.

Lorsque vous cochez cette case, une nouvelle section intitulée **Logos WindowsStore téléchargés** s’affiche. Dans cette section, vous pouvez charger 3images, y compris «l’icône de vignette de l’application» au format 300x300 (si vous cochez cette case, le champ qui permet de fournir cette image se déplace dans cette section). Si vous utilisez cette option, nous vous recommandons de fournir les trois tailles d’image: 71x71, 150x150 et 300x300pixels. Toutefois, seule la taille 300 x 300 est obligatoire.


## <a name="additional-art-assets"></a>Illustrations supplémentaires

Cette section vous permet de fournir une conception graphique pour améliorer l’affichage de votre produit dans le WindowsStore: images promotionnelles, images Xbox, images promotionnelles facultatives et bandes-annonces. Nous vous recommandons de fournir ces images pour rendre la description dans le WindowsStore plus attrayante. Le format **bannière 16:9 (1920x1080 ou 3840x2160pixels)** est particulièrement recommandé si vous prévoyez d’inclure des [bandes-annonces vidéo](#trailers) dans votre description dans le WindowsStore; si vous n’incluez pas ce format, vos bandes annonces n’apparaîtront pas dans la partie supérieure de votre description.

Pour ajouter ces images, sélectionnez **Afficher les détails** dans la section **Illustrations supplémentaires**.


## <a name="promotional-images"></a>Images promotionnelles

Les images de cette section peuvent améliorer l’apparence de la description de votre application, et nous permettent également de prendre en compte votre application pour les offres promotionnelles que nous recommandons. Notez que la présence de ces images ne garantit pas la promotion de votre application. Pour plus d’informations, consultez l’article [Faciliter la promotion de votre application](make-your-app-easier-to-promote.md).

Voici quelques conseils à suivre lors de l’élaboration de votre conception graphique promotionnelle:

- Les images doivent être au format .png.
- Sélectionnez des images dynamiques en rapport avec l’application qui permettent de la reconnaître et de la différencier. Évitez les photos issues de banques d’images ou les images génériques.
- N’incluez pas de texte (excepté celui de votre marque).
- Réduisez l’espace vide dans votre image.
- Éviter de montrer l’interface utilisateur de votre application et n’utilisez pas d’images spécifiques à des appareils.
- Évitez les thèmes politiques et nationaux, les drapeaux ou les symboles religieux.
- N’incluez pas d’images en rapport avec des gestes irrespectueux, la nudité, le jeu, l’argent, la drogue, le tabac ou l’alcool.
- N’utilisez pas d’armes pointant vers l’utilisateur ni une violence excessive.

Le format **bannière 16:9 (1920x1080pixels ou 3840x2160pixels** est utilisé dans diverses présentations du WindowsStore sur tous les types d’appareils Windows10. Nous vous recommandons de fournir cette image, quels que soient les versions du système d’exploitation ou les types d’appareils ciblés par votre application. Cette image est *requise* pour l’obtention d’un affichage correct si votre description inclut des [bandes-annonces vidéo](#trailers). Pour les clients qui utilisent Windows10, version1607 ou ultérieure, cette image est utilisée comme image principale dans la partie supérieure de votre description (ou apparaît après la diffusion des bandes-annonces). 

La taille d’image **2:1 (2400x1200)** est uniquement utilisée si votre application prend en charge la famille d’appareils Holographique.

Lors de la conception de votre image, gardez à l’esprit que dans certaines présentations, nous appliquerons un dégradé sur le tiers inférieur de manière à pouvoir afficher lisiblement du texte marketing sur l’image. Par conséquent, évitez de placer du texte et des éléments visuels dans le tiers inférieur. Nous pouvons en outre rogner votre image. Veillez donc à placer la marque de votre application et les informations les plus importantes au centre.  

<!-- update? ![guidelines for spotlight image](images/spotlight1.jpg) 
![well-designed spotlight image](images/spotlight2.jpg)

The image below shows the key proportions to keep in mind. The "safe zone" in the center will be prominent even if we crop the image. The "dynamic text area" is where text and a gradient may appear.  -->

## <a name="xbox-images"></a>Images Xbox

Si vous publiez votre application sur Xbox, ces images sont recommandées pour un affichage correct. 

Vous pouvez charger 3tailles différentes:
- **Illustration principale avec marque, 584 x 800pixels**: utilisée dans le MarchéXbox.com. Doit inclure titre du produit. 
- **Image principale avec titre, 1920 x 1080pixels**: doit inclure titre du produit.
- **Image promotionnelle, de 1080x1080pixels**: utilisée dans le Marché Xbox. Doit inclure le titre du produit.

> [!NOTE]
> Pour optimiser l’affichage sur Xbox, vous devez également fournir une image **9:16 (720x1080 ou 1440x2160pixels)** dans la section [Logos WindowsStore](#store-logos).


## <a name="optional-promotional-images"></a>Images promotionnelles facultatives

Si votre application prend en charge les versions antérieures du système d’exploitation (Windows8.x et/ou Windows Phone8.x), ces images doivent être fournies dans l’ordre pour que nous puissions recommander votre application dans les présentations promotionnelles (même si la présence de ces images ne garantit pas la promotion de votre application). Si votre application ne prend pas en charge ces versions antérieures du système d’exploitation, vous pouvez ignorer cette section.

**Pour WindowsPhone8.1 et les versions antérieures**, deux tailles d’images sont utilisables dans les présentations promotionnelles: **1000x800pixels (5:4)** et **358x358pixels (1:1)**. Si votre application s’exécute sur Windows Phone8.1 ou versions antérieures, nous recommandons de fournir des images dans ces deux tailles pour des questions de promotion.  

> [!TIP]
> Veillez à fournir une image en 300 x 300 pour l’icône de vignette d'application dans la section [Logos Windows Store](#store-logos) pour toute soumission qui prend en charge WindowsPhone8.1 ou versions antérieures. Cela permet de garantir que votre application n’apparaît pas dans le Windows Store avec une icône vide.  

**Pour Windows8.1 et versions antérieures**, les mises en forme promotionnelles peuvent utiliser une image au format **414x180pixels**. Si votre application s’exécute sur Windows8.1 ou versions antérieures, nous recommandons de fournir une image de cette taille pour des questions de promotion.


## <a name="trailers"></a>Bandes-annonces

Les bandes-annonces sont de courtes vidéos qui permettent aux clients de voir votre produit en action et de s’en faire ainsi une meilleure idée. Elles s’affichent dans la partie supérieure de la description de votre application dans le WindowsStore (à condition que vous ayez inclus une **image 1920x1080pixels (16:9)** dans la section [Images promotionnelles](#promotional-images)). 

Les bandes-annonces sont codées avec la fonctionnalité de diffusion en continu lisse [Smooth Streaming](http://www.iis.net/downloads/microsoft/smooth-streaming), qui adapte la qualité du flux vidéo fourni aux clients en temps réel, en fonction de la bande passante disponible et des ressources processeur.

> [!NOTE]
> Les bandes-annonces s'affichent uniquement pour les clients sur Windows10, version1607 ou ultérieure.

### <a name="upload-trailers"></a>Charger les bandes-annonces

Vous pouvez ajouter jusqu'à 15bandes-annonces à votre description dans le Windows Store. Veillez à ce que chaque bande-annonce réponde aux exigences répertoriées ci-dessous.

Vous devez fournir un fichier vidéo (.mp4 ou .mov), une image miniature et un titre pour chaque bande-annonce.

> [!IMPORTANT]
> Lorsque vous utilisez des bandes-annonces, vous devez également fournir une **image 1920x1080pixels (16:9)** dans la section [Image promotionnelles](#promotional-images) pour que vos bandes-annonces apparaissent dans la partie supérieure de votre description dans le WindowsStore. Cette image s’affichera après la diffusion de vos bandes-annonces.

Pour concevoir des bandes-annonces efficaces, appliquez les recommandations suivantes:
- Les bandes-annonces doivent être de bonne qualité et de courte durée (2minutes au maximum recommandé). 
- La résolution et la fréquence d’images doivent correspondre au document source. Par exemple, un contenu filmé à 720p60 doit être codé et chargé à 720p60. 
- Utilisez une miniature différente pour chaque bande-annonce afin que les clients sachent qu’elles sont uniques.
- Comme certaines dispositions peuvent légèrement rogner le haut et le bas de votre bande-annonce, assurez-vous que les informations principales s’affichent au centre de l’écran.

Vous devez également respecter la configuration requise indiquée ci-dessous.

**Pour ajouter les bandes-annonces à votre liste:**
1. Chargez le **fichier vidéo** de votre bande-annonce dans la case indiquée. Une zone de liste déroulante s’affiche également au cas où vous souhaiteriez réutiliser une bande-annonce que vous avez déjà chargée (par exemple, pour une description dans le Windows Store dans une autre langue).
2. Après avoir chargé la bande-annonce, vous devez charger une **image miniature** pour l’accompagner. Il doit s'agir d'un fichier .png de 1920 x 1080pixels, et généralement d'une image fixe extraite de la bande-annonce.
3. Cliquez sur l’icône de crayon pour ajouter un **titre** à votre bande-annonce (255caractères au maximum).
4. Si vous souhaitez ajouter d'autres bandes-annonces à la liste, cliquez sur **Ajouter une bande-annonce** et répétez les étapes indiquées ci-dessus.

Pour supprimer une bande-annonce, cliquez sur le **X** en regard de son nom de fichier. Vous pouvez choisir de supprimer uniquement les descriptions du Windows Store en cours ou toutes les descriptions du Windows Store pour votre produit (autrement dit, pour cette langue seulement ou pour toutes les langues).

### <a name="trailer-requirements"></a>Exigences pour les bandes-annonces

Lorsque vous fournissez vos bandes-annonces, veillez à respecter ces exigences:

- Le format vidéo doit être MOV ou MP4. 
- La durée de la vidéo doit être inférieure à 30minutes. 
- La taille du fichier de la bande-annonce ne doit pas dépasser 10Go. 
- La miniature doit être un fichier PNG avec une résolution de 1920 x 1080pixels. 
- Le titre ne doit pas dépasser 255caractères. 

Comme les autres champs sur la page de description du Windows Store, les bandes-annonces doivent obtenir une certification pour pouvoir être publiées sur le Windows Store. Assurez-vous que vos bandes-annonces respectent les [Politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx).

Il existe des exigences supplémentaires en fonction du type de fichier.

#### <a name="mov"></a>MOV

<table>
<tr>
<td>
**Vidéo:**
<ul>
<li>ProRes1080p (HQ le cas échéant)</li>
<li>Fréquence d’images native; 29,97ips de préférence</li>
</ul>
</td>
<td>
**Audio:**
<ul>
<li>Stéréo obligatoire</li>
<li>Niveau Audio recommandé: -16LKFS/LUFS</li>
</ul> 
</td>
</tr>
</table>

#### <a name="mp4"></a>MP4

<table>
<tr>
<td>
**Vidéo:**
<ul>
<li>Codec: H.264</li>
<li>Balayage progressif (pas d'entrelacement)</li>
<li>High Profile</li>
<li>2images B consécutives</li>
<li>GOP fermé. GOP de la moitié de la fréquence d’images</li>
<li>CABAC</li>
<li>50Mo/s </li>
<li>Espace de couleurs: 4.2.0</li>
</ul>
</td>
<td>
**Audio:**
<ul>
<li>Codec: AAC-DS</li>
<li>Canaux: stéréo ou son surround</li>
<li>Taux d’échantillonnage: 48KHz</li>
<li>Vitesse de transmission audio: 384Ko/s pour stéréo, 512Ko/s pour son surround</li>
</ul>
</td>
</tr>
</table>

Pour les fichiers Mezzanine H.264, nous recommandons les éléments suivants:
- Conteneur: MP4
- Pas de listes de modifications (ou vous risquez de perdre la synchronisation AV)
- moov atom en tête du fichier (démarrage rapide)

