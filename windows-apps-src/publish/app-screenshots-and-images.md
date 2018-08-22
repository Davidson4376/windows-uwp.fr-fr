---
author: jnHs
Description: You can select the screenshots, logos, and other art assets (such as trailers and promotional images) to include in your app's Store listing.
title: Captures d’écran, images et bandes-annonces de l’application
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 8/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, bande-annonce, vidéo, capture d’écran, image, icône, description dans le Store, images de description dans le Store
ms.localizationpriority: medium
ms.openlocfilehash: d787ac620f9d0114d67d85def758416afd9f5f15
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2788434"
---
# <a name="app-screenshots-images-and-trailers"></a>Captures d’écran, images et bandes-annonces de l’application

Des images bien conçues constituent l’un des principaux moyens dont vous disposez pour présenter votre application aux clients potentiels dans le Store.

Vous pouvez fournir des [captures d’écran](#screenshots), des [logos](#store-logos) et d’autres ressources d’illustration (telles que des [bandes-annonces](#trailers) et des [images promotionnelles](##additional-art-assets)) à inclure dans la description de votre application dans le Store. Certains de ces éléments sont requis, tandis que d’autres sont facultatifs (bien qu’il soit recommandé d’inclure certaines images facultatives afin d’optimiser l’affichage de votre application dans le Store). 

Lors du [processus de soumission de l’application](app-submissions.md), vous fournissez ces ressources d’illustration à l’étape [Descriptions dans le Store](create-app-store-listings.md). Notez que les images qui sont utilisées dans le Store et la façon dont elles apparaissent peuvent varier selon le système d’exploitation du client et d’autres facteurs.

Le Store peut également utiliser la vignette et d’autres images de votre application que vous incluez dans le package de votre application. Exécutez le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) pour déterminer si une image requise est manquante avant de soumettre votre application. Pour obtenir des conseils et des recommandations sur ces images, voir [Ressources de vignette et d’icône](../design/shell/tiles-and-notifications/app-assets.md).

## <a name="screenshots"></a>Captures d’écran

Les captures d’écran sont les images de votre application que voient vos clients dans la description du Store.

Vous avez la possibilité de fournir des captures d’écran pour les différentes familles d’appareils prises en charge par votre application, afin que les captures appropriées apparaissent lorsqu’un client affiche la description de votre application dans le Store sur ce type d’appareil. 

La soumission de votre application ne nécessite qu’une seule capture d’écran (pour une famille d’appareils quelle qu’elle soit), mais vous pouvez en fournir plusieurs, jusqu’à9 pour les ordinateurs de bureau et8 pour les autres familles d’appareils. Nous vous recommandons de fournir au moins quatre captures d’écran pour chaque famille d’appareils prise en charge par votre application afin que les utilisateurs puissent voir à quoi cette dernière ressemblera sur leur type d’appareil. (N’incluez pas de captures d’écran pour les familles d’appareils que votre application ne prend pas en charge.) Notez que les captures d’écran de **bureau** s’affichent également pour les clients sur les appareils SurfaceHub.

> [!NOTE]
> Microsoft VisualStudio contient un [outil vous permettant d’effectuer des captures d’écran](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Chaque capture d’écran doit être au format .png en mode paysage ou portrait, et la taille du fichier ne doit pas dépasser 50Mo.

Les exigences de taille varient en fonction de la famille d’appareils:
- Bureau: 1366x768pixels ou plus. Prend en charge les images4K (3840x2160). (Également visible pour les clients sur les appareils SurfaceHub.)
- Mobile: La taille des images doit correspondre à l’une des valeurs suivantes: 1080x1920, 1920x1080, 768x1280, 1280x768, 720x1280, 1280x720, 800x480 ou 480x800pixels.
- Xbox: 3480x2160pixels ou moins. Prend en charge les images4K (3840x2160).
- Holographique: 1268x720pixels ou plus. Prend en charge les images4K (3840x2160).

Pour optimiser l’affichage, gardez les instructions ci-après à l’esprit lorsque vous créez vos captures d’écran:
- Placez les visuels et le texte essentiels dans les trois quarts supérieurs de l’image. Un texte superposé peut apparaître dans le quart inférieur. 
- N’ajoutez pas de logos, d'icônes ou de messages marketing supplémentaires sur vos captures d’écran.
- N’utilisez pas de couleurs très claires ou très sombres, ni de bandes à fort contraste qui peuvent interférer avec la lisibilité du texte superposé.

Vous pouvez également fournir un bref sous-titre pour chaque capture d’écran (au maximum 200caractères).

> [!TIP]
> Les captures d’écran sont affichés dans votre description dans l’ordre. Une fois que vous téléchargez vos captures d’écran, vous pouvez les glisser-déplacer pour les réorganiser. 

Notez que si vous créez des descriptions dans le Store pour [plusieurs langues](supported-languages.md), vous disposez d’une page **Description dans le Store** pour chacune d’elles. Vous devez charger des images pour chaque langue séparément (même si vous utilisez les mêmes images), et fournir des sous-titres à utiliser pour chaque langue.


## <a name="store-logos"></a>Logos du Store

Vous pouvez charger les logos du Store pour personnaliser davantage l’affichage dans le Store. Nous vous recommandons de fournir ces images pour optimiser l’affichage de vos descriptions dans le Store sur l’ensemble des appareils et des versions de système d’exploitation pris en charge par votre application. Notez que si votre application est disponible pour les clients sur Xbox, certaines de ces images sont requises.

Vous pouvez fournir ces images sous forme de fichiers .png (inférieurs à 50Mo), qui doivent respecter les instructions ci-dessous.

### <a name="916-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>Affiche9:16 (720x1080 ou 1440x2160pixels)

Ce format est utilisé comme image de logo principale pour les clients sur Windows10 et les appareils Xbox. C’est pourquoi nous vous **conseillons vivement** de fournir cette image. Si vous ne l’incluez pas, l’affichage de votre description risque de ne pas être optimal et ne sera pas cohérent avec les autres descriptions que les clients verront lorsqu’ils navigueront sur le Store. Cette image peut également être utilisée dans des résultats de recherche ou dans des collections sélectionnées d’un point de vue éditorial.

Cette image doit inclure le nom de votre application, et le texte présent sur l’image doit respecter les exigences de lisibilité et d’accessibilité (taux de contraste de4,5 pour1). Notez que du texte superposé peut apparaître dans le quart inférieur de cette image, veillez donc à ne pas inclure de texte ou d’images clés à cet emplacement.

> [!NOTE]
> Si votre application est disponible pour les clients sur Xbox, cette image est **requise** et doit inclure le titre du produit. Le titre doit apparaître dans les trois quarts supérieurs de l’image, dans la mesure où du texte superposé peut apparaître dans le quart inférieur de l’image.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>Boîte1:1 (1080x1080 ou 2160x2160pixels)

Cette image peut apparaître dans différentes pages du Store pour Windows10 (y compris Xbox), et si vous ne fournissez pas l’image **Affiche 9:16**, elle sera utilisée comme votre logo principal. Cette image doit également inclure le nom de votre application. Du texte superposé peut apparaître dans le quart inférieur de cette image, veillez donc à ne pas inclure de texte ou d’images clés à cet emplacement. Assurez-vous d’inclure le nom de votre application dans cette image. 

> [!NOTE]
> Si votre application est disponible pour les clients sur Xbox, cette image est **requise** et doit inclure le titre du produit. Le titre doit apparaître dans les trois quarts supérieurs de l’image, dans la mesure où du texte superposé peut apparaître dans le quart inférieur de l’image.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>Icône de vignette d’application1:1 (300x300pixels)

Cette image est requise pour un affichage correct sur WindowsPhone8.1 et versions antérieures. Si votre application prend en charge WindowsPhone8.1 ou versions antérieures et que vous ne fournissez pas cette image, les clients concernés verront une icône vide associée à la description de votre application. (Ceci s’applique également aux clients sur Windows10, si votre application ne comporte que des packages ciblant WindowsPhone8.1 ou les versions antérieures.) Si votre soumission inclut *uniquement* des packages UWP, vous n’avez pas besoin de fournir cette image. (Notez que si votre soumission comprend à la fois des packages Windows Phone8.x et des packages UWP et que vous fournissez cette image, celle-ci peut être utilisée sur Windows10dans certaines dispositions du Store. Pour éviter cela, vous pouvez créer des [descriptions propres à la plateforme](create-platform-specific-store-listings.md) pour les versions de WindowsPhone prises en charge par votre application et inclure uniquement à cet emplacement l’icône de vignette de l’application.)

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Affichage téléchargé uniquement des images de logo dans le magasin

Vous pouvez empêcher le magasin d’avec les images de logo dans des packages de votre application lors de l’affichage de votre annonce pour les clients Windows 10 (y compris Xbox) et au lieu de cela que le magasin utilisez uniquement des images que vous téléchargez. Cette opération vous permet de mieux contrôler l’apparence de votre application sur différents affichages partout dans le Store pour les clients sur Windows10 (y compris Xbox). (Si votre produit prend en charge les versions antérieures du système d’exploitation, les clients peuvent encore voir les images de vos packages, même si vous utilisez cette option.)

Pour que le magasin utiliser uniquement les images que vous téléchargez (pour les clients Windows 10, y compris Xbox), et pas utiliser toutes les images de vos packages, indiquant **la case à cocher pour les clients Windows 10 et Xbox, affichage téléchargé les images de logo au lieu des images à partir de mes packages **.

Lorsque vous activez cette case, une nouvelle section appelée **magasin afficher des images** s’affiche. Ici, vous pouvez télécharger des 3 images, y compris la taille de **l’application de 1:1 mosaïque icône (pixels de 300 x 300)** (si la case à cocher, le champ de fournir que cette image est déplacés dans cette section). Si vous utilisez cette option, nous vous recommandons de fournir les trois tailles d’image: 71x71, 150x150 et 300x300pixels. Toutefois, seule la taille 300 x 300 est obligatoire.


<span id="promotional-images" />

## <a name="additional-art-assets"></a>Illustrations supplémentaires

Cette section vous permet de fournir une conception graphique pour améliorer l’affichage de votre produit dans le Store. Nous vous recommandons de fournir ces images pour rendre la description dans le Store plus attrayante.

> [!TIP]
> Le format **Image principale super16:9** est particulièrement recommandé si vous prévoyez d’insérer des [bandes-annonces vidéo](#trailers) dans votre description dans le Store; si vous ne l’incluez pas, vos bandes-annonces n’apparaîtront pas dans la partie supérieure de votre description.

Pour ajouter ces images, sélectionnez **Afficher les détails** dans la section **Illustrations supplémentaires**.



### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Image Windows10 et Xbox (Image principale super16:9)

L’image **Image principale super16:9 (1920x1080 ou 3840x2160pixels)** est utilisée dans diverses dispositions du MicrosoftStore, sur tous les types d’appareils Windows10 (y compris Xbox). Nous vous recommandons de fournir cette image, quels que soient les versions du système d’exploitation ou les types d’appareils ciblés par votre application.

Cette image est *requise* pour l’obtention d’un affichage correct si votre description inclut des [bandes-annonces vidéo](#trailers). Pour les clients utilisant Windows10, version1607 ou ultérieure (incluant Xbox), cette image est utilisée comme image principale dans la partie supérieure de votre description (ou apparaît après la diffusion des bandes-annonces). Elle peut également servir à recommander votre application dans les dispositions promotionnelles partout dans le Store. Notez que cette image ne doit pas inclure le titre du produit ou tout autre texte.

Voici quelques conseils à suivre lors de l’élaboration de cette image:

- Cette image doit être au format .png de 1920x1080pixels ou 3840x2160pixels.
- Sélectionnez une image dynamique en rapport avec l’application qui permette de la reconnaître et de la différencier. Évitez les photos issues de banques d’images ou les images génériques.
- N’insérez pas de texte dans l’image.
- Évitez de placer des éléments visuels clés dans le tiers inférieur de l’image (étant donné que dans certaines dispositions, nous pouvons appliquer un dégradé sur cette partie).
- Placez les informations les plus importantes dans le centre de l’image (étant donné que dans certaines dispositions, nous pouvons rogner l’image).
- Réduisez l’espace vide.
- Éviter de montrer l’interface utilisateur de votre application et n’utilisez pas d’images spécifiques à des appareils.
- Évitez les thèmes politiques et nationaux, les drapeaux ou les symboles religieux.
- N’incluez pas d’images en rapport avec des gestes irrespectueux, la nudité, le jeu, l’argent, la drogue, le tabac ou l’alcool.
- N’utilisez pas d’armes pointant vers l’utilisateur ni une violence excessive.

Si la présence de cette image nous permet de prendre en compte votre application pour les offres promotionnelles que nous recommandons, elle ne garantit pas que votre application sera recommandée. Pour plus d’informations, consultez l’article [Faciliter la promotion de votre application](make-your-app-easier-to-promote.md).


### <a name="xbox-images"></a>Images Xbox

Si vous publiez votre application sur Xbox, ces images sont requises pour un affichage correct. 

Vous pouvez charger 3tailles différentes:
- **Illustration principale avec marque, 584x800pixels**: doit inclure le titre du produit. Une barre de marque est requise sur cette image. Placez le titre et toutes les images clés dans les trois quarts supérieurs de l’image, dans la mesure où un effet de superposition peut apparaître dans le quart inférieur.
- **Image principale avec titre, 1920x1080pixels**: doit inclure le titre du produit. Placez le titre et toutes les images clés dans les trois quarts supérieurs de l’image, dans la mesure où un effet de superposition peut apparaître dans le quart inférieur.
- **Image carrée avec promotion recommandée, 1080x1080pixels**: ne doit *pas* inclure le titre du produit.

> [!NOTE]
> Pour optimiser l’affichage sur Xbox, vous devez également fournir une image **9:16 (720x1080 ou 1440x2160pixels)** dans la section [Logos Store](#store-logos).


### <a name="holographic-image"></a>Image holographique

L’image **2:1 (2400x1200)** est uniquement utilisée si votre application prend en charge la famille d’appareils Holographique. Si tel est le cas, nous vous recommandons de fournir cette image.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Images uniquement pour Windows8.x et/ou Windows Phone8.x 

Si votre application prend en charge les versions antérieures du système d’exploitation (Windows8.x et/ou Windows Phone8.x), ces images doivent être fournies dans l’ordre pour que nous puissions recommander votre application dans les présentations promotionnelles (même si la présence de ces images ne garantit pas la promotion de votre application). Si votre application ne prend pas en charge ces versions antérieures du système d’exploitation, vous pouvez ignorer cette section. (Cette section était précédemment appelée **Images promotionnelles facultatives**.)

**Pour WindowsPhone8.1 et les versions antérieures**, deux tailles d’images sont utilisables dans les présentations promotionnelles: **1000x800pixels (5:4)** et **358x358pixels (1:1)**. Si votre application s’exécute sur Windows Phone8.1 ou versions antérieures, nous recommandons de fournir des images dans ces deux tailles pour des questions de promotion.  

> [!TIP]
> Veillez à fournir une image en 300 x 300 pour l’icône de vignette d'application dans la section [Logos Store](#store-logos) pour toute soumission qui prend en charge WindowsPhone8.1 ou versions antérieures. Cela permet de garantir que votre application n’apparaît pas dans le Store avec une icône vide.  

**Pour Windows8.1 et versions antérieures**, les mises en forme promotionnelles peuvent utiliser une image au format **414x180pixels**. Si votre application s’exécute sur Windows8.1 ou versions antérieures, nous recommandons de fournir une image de cette taille pour des questions de promotion.


## <a name="trailers"></a>Bandes-annonces

Les bandes-annonces sont de courtes vidéos qui permettent aux clients de voir votre produit en action et de s’en faire ainsi une meilleure idée. Elles s’affichent dans la partie supérieure de la description de votre application dans le Store (à condition que vous ayez inclus une **image 1920x1080pixels (16:9)** dans la section [Images promotionnelles](#promotional-images)). 

Les bandes-annonces sont codées avec la fonctionnalité de diffusion en continu lisse [Smooth Streaming](http://www.iis.net/downloads/microsoft/smooth-streaming), qui adapte la qualité du flux vidéo fourni aux clients en temps réel, en fonction de la bande passante disponible et des ressources processeur.

> [!NOTE]
> Les bandes-annonces s'affichent uniquement pour les clients sur Windows10, version1607 ou ultérieure (incluant Xbox).

### <a name="upload-trailers"></a>Charger les bandes-annonces

Vous pouvez ajouter jusqu'à 15bandes-annonces à votre description dans le Store. Veillez à ce qu'elles répondent toutes aux [exigences](#trailer-requirements) répertoriées ci-dessous.

Vous devez charger un fichier vidéo (.mp4 ou .mov), une vignette et un titre pour chaque bande-annonce fournie.

> [!IMPORTANT]
> Lorsque vous utilisez des bandes-annonces, vous devez également fournir une **image 1920x1080pixels (16:9)** dans la section [Image promotionnelles](#promotional-images) pour que vos bandes-annonces apparaissent dans la partie supérieure de votre description dans le Store. Cette image s’affichera après la diffusion de vos bandes-annonces.

Pour concevoir des bandes-annonces efficaces, appliquez les recommandations suivantes:
- Les bandes-annonces doivent être de bonne qualité et de courte durée (60secondes au maximum et moins de 2Go recommandés). 
- Utilisez une vignette différente pour chaque bande-annonce afin que les clients sachent qu’elles sont uniques.
- Comme certaines dispositions du Store peuvent légèrement rogner le haut et le bas de votre bande-annonce, assurez-vous que les informations principales s’affichent au centre de l’écran.
- La résolution et la fréquence d’images doivent correspondre au document source. Par exemple, un contenu filmé à 720p60 doit être codé et chargé à 720p60. 

Vous devez également respecter la configuration requise répertoriée ci-dessous.

**Pour ajouter les bandes-annonces à votre liste:**
1. Chargez le **fichier vidéo** de votre bande-annonce dans la case indiquée. Une zone de liste déroulante s’affiche également au cas où vous souhaiteriez réutiliser une bande-annonce que vous avez déjà chargée (par exemple, pour une description dans le Store dans une autre langue).
2. Après avoir chargé la bande-annonce, vous devez charger une **image miniature** pour l’accompagner. Il doit s'agir d'un fichier .png de 1920 x 1080pixels, et généralement d'une image fixe extraite de la bande-annonce.
3. Cliquez sur l’icône de crayon pour ajouter un **titre** à votre bande-annonce (255caractères au maximum).
4. Si vous souhaitez ajouter d'autres bandes-annonces à la liste, cliquez sur **Ajouter une bande-annonce** et répétez les étapes indiquées ci-dessus.

> [!TIP]
> Si vous avez créé des descriptions dans le Store en plusieurs langues, vous pouvez sélectionner **Choisir à partir de bandes-annonces existantes** pour réutiliser les bandes-annonces que vous avez déjà chargées. Vous n’êtes pas obligé de les charger une à une pour chaque langue.

Pour supprimer une bande-annonce d'une liste, cliquez sur le **X** en regard de son nom de fichier. Vous pouvez choisir de la supprimer uniquement de la description actuelle dans le Store dans laquelle vous travaillez ou de la supprimer de toutes les descriptions de votre produit dans le Store (dans chaque langue).


### <a name="trailer-requirements"></a>Exigences pour les bandes-annonces

Lorsque vous fournissez vos bandes-annonces, veillez à respecter ces exigences:

- Le format vidéo doit être MOV ou MP4. 
- La durée de la vidéo ne doit pas dépasser 60secondes.
- La taille du fichier de la bande-annonce ne doit pas dépasser 2Go. 
- La résolution vidéo doit être 1920x1080pixels ou 3840x2160pixels.
- La vignette doit être un fichier PNG avec une résolution de 1920x1080pixels ou 3840x2160pixels.
- Le titre ne doit pas dépasser 255caractères. 
- N’incluez pas les classifications d'âge minimum dans vos bandes-annonces.

Comme les autres champs sur la page de description du Store, les bandes-annonces doivent obtenir une certification pour pouvoir être publiées sur le Microsoft Store. Assurez-vous que vos bandes-annonces respectent les [Politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

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

