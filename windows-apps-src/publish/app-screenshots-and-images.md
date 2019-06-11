---
Description: Vous pouvez sélectionner les captures d’écran, les logos et d'autres ressources d'illustration (par exemple, des bandes-annonces et des images publicitaires) à inclure dans votre description de l’application dans le Windows Store.
title: Captures d’écran, images et bandes-annonces de l’application
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 03/07/2019
ms.topic: article
keywords: windows 10, uwp, bande-annonce, vidéo, capture d’écran, image, icône, description dans le Store, images de description dans le Store
ms.localizationpriority: medium
ms.openlocfilehash: 3f1931a15b5517264cd11dca8d8086dda7094b93
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826158"
---
# <a name="app-screenshots-images-and-trailers"></a>Captures d’écran, images et bandes-annonces de l’application

Des images bien conçues constituent l’un des principaux moyens dont vous disposez pour présenter votre application aux clients potentiels dans le Windows Store.

Vous pouvez fournir [des captures d’écran](#screenshots), [logos](#store-logos), [remorques](#trailers)et autres ressources de l’illustration à inclure dans la liste de Store de votre application. Certains de ces éléments sont requis, tandis que d’autres sont facultatifs (bien qu’il soit recommandé d’inclure certaines images facultatives afin d’optimiser l’affichage de votre application dans le Store).

Lors du [processus de soumission de l’application](app-submissions.md), vous fournissez ces ressources d’illustration à l’étape [Descriptions dans le Store](create-app-store-listings.md). Notez que les images qui sont utilisées dans le Store et la façon dont elles apparaissent peuvent varier selon le système d’exploitation du client et d’autres facteurs.

Le Store peut également utiliser les icône de votre application et autres images que vous incluez dans votre package de l’application. Exécutez le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) pour déterminer si une image requise est manquante avant de soumettre votre application. Pour des conseils et des recommandations sur ces images, consultez [icônes d’application et les logos](../design/style/app-icons-and-logos.md).

## <a name="screenshots"></a>Les captures d'écran

Les captures d’écran sont les images de votre application que voient vos clients dans la description du Windows Store.

Vous avez la possibilité de fournir des captures d’écran pour les différentes familles d’appareils prises en charge par votre application, afin que les captures appropriées apparaissent lorsqu’un client affiche la description de votre application dans le Store sur ce type d’appareil. 

La soumission de votre application ne nécessite qu’une seule capture d’écran (pour une famille d’appareils quelle qu’elle soit), mais vous pouvez en fournir plusieurs, jusqu’à 9 pour les ordinateurs de bureau et 8 pour les autres familles d’appareils. Nous vous recommandons de fournir au moins quatre captures d’écran pour chaque famille d’appareils prise en charge par votre application afin que les utilisateurs puissent voir à quoi cette dernière ressemblera sur leur type d’appareil. (N’incluez pas de captures d’écran pour les familles d’appareils que votre application ne prend pas en charge.) Notez que **Desktop** des captures d’écran s’affichera également pour les clients sur les appareils Surface Hub.

> [!NOTE]
> Microsoft Visual Studio contient un [outil vous permettant d’effectuer des captures d’écran](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Chaque capture d’écran doit être au format .png en mode paysage ou portrait, et la taille du fichier ne doit pas dépasser 50 Mo.

Les exigences de taille varient en fonction de la famille d’appareils :
- Bureau : 1366 x 768 pixels ou plus. Prend en charge les images 4K (3840 x 2160). (Également visible pour les clients sur les appareils Surface Hub.)
- Mobile : Images doivent être une des opérations suivantes : 1080 x 1920, 1920 x 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480 ou 480 x 800 pixels.
- Xbox : 3480 x 2160 pixels ou plus petits. Prend en charge les images 4K (3840 x 2160).
- HOLOGRAPHIQUE : 1268 x 720 pixels ou plus. Prend en charge les images 4K (3840 x 2160).

Pour optimiser l’affichage, gardez les instructions ci-après à l’esprit lorsque vous créez vos captures d’écran :
- Placez les visuels et le texte essentiels dans les trois quarts supérieurs de l’image. Un texte superposé peut apparaître dans le quart inférieur. 
- N’ajoutez pas de logos, d'icônes ou de messages marketing supplémentaires sur vos captures d’écran.
- N’utilisez pas de couleurs très claires ou très sombres, ni de bandes à fort contraste qui peuvent interférer avec la lisibilité du texte superposé.

Vous pouvez également fournir un bref sous-titre pour chaque capture d’écran (au maximum 200 caractères).

> [!TIP]
> Les captures d’écran sont affichés dans votre description dans l’ordre. Une fois que vous téléchargez vos captures d’écran, vous pouvez les glisser-déplacer pour les réorganiser. 

Notez que si vous créez des descriptions dans le Store pour [plusieurs langues](supported-languages.md), vous disposez d’une page **Description dans le Store** pour chacune d’elles. Vous devez charger des images pour chaque langue séparément (même si vous utilisez les mêmes images), et fournir des sous-titres à utiliser pour chaque langue. (Si vous avez des annonces de Store dans un grand nombre de langues, il peut s’avérer plus facile de les mettre à jour par [exportation de votre liste de données et de travail hors connexion](import-and-export-store-listings.md).)


## <a name="store-logos"></a>Logos Store

Vous pouvez charger les logos du Store pour personnaliser davantage l’affichage dans le Store. Nous vous recommandons de fournir ces images pour optimiser l’affichage de vos descriptions dans le Store sur l’ensemble des appareils et des versions de système d’exploitation pris en charge par votre application. Notez que si votre application est disponible pour les clients sur Xbox, certaines de ces images sont requises.

Vous pouvez fournir ces images sous forme de fichiers .png (inférieurs à 50 Mo), qui doivent respecter les instructions ci-dessous.

### <a name="916-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>Affiche 9:16 (720 x 1080 ou 1440 x 2160 pixels)

Ce format est utilisé comme image de logo principale pour les clients sur Windows 10 et les appareils Xbox. C’est pourquoi nous vous **conseillons vivement** de fournir cette image. Votre annonce n’a pas se prêtent parfaitement si vous n’incluez pas et ne sera pas cohérente avec les autres listes que les clients voient quand vous parcourez le Store. Cette image peut également être utilisée dans des résultats de recherche ou dans des collections sélectionnées d’un point de vue éditorial.

Cette image doit inclure le nom de votre application, et le texte présent sur l’image doit respecter les exigences de lisibilité et d’accessibilité (taux de contraste de 4,5 pour 1). Notez que du texte superposé peut apparaître dans le quart inférieur de cette image, veillez donc à ne pas inclure de texte ou d’images clés à cet emplacement.

> [!NOTE]
> Si votre application est disponible pour les clients sur Xbox, cette image est **requise** et doit inclure le titre du produit. Le titre doit apparaître dans les trois quarts supérieurs de l’image, dans la mesure où du texte superposé peut apparaître dans le quart inférieur de l’image.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>Boîte 1:1 (1080 x 1080 ou 2160 x 2160 pixels)

Cette image peut apparaître dans différentes pages du Store pour Windows 10 (y compris Xbox), et si vous ne fournissez pas l’image **Affiche 9:16**, elle sera utilisée comme votre logo principal. Cette image doit également inclure le nom de votre application. Du texte superposé peut apparaître dans le quart inférieur de cette image, veillez donc à ne pas inclure de texte ou d’images clés à cet emplacement. Assurez-vous d’inclure le nom de votre application dans cette image. 

> [!NOTE]
> Si votre application est disponible pour les clients sur Xbox, cette image est **requise** et doit inclure le titre du produit. Le titre doit apparaître dans les trois quarts supérieurs de l’image, dans la mesure où du texte superposé peut apparaître dans le quart inférieur de l’image.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>Icône de vignette d’application 1:1 (300 x 300 pixels)

Cette image est requise pour un affichage correct sur Windows Phone 8.1 et versions antérieures. Si votre application publiée précédemment prend en charge Windows Phone 8.1 ou version antérieure et que vous ne fournissez pas cette image, les clients seront affiche une icône vide avec la liste de votre application. (Cela s’applique également aux clients sur Windows 10 si votre application a uniquement les packages ciblant Windows Phone 8.1 ou version antérieure.)

Si votre soumission *uniquement* inclut des packages UWP, vous n’avez pas besoin de fournir cette image (sauf si vous cochez la case **pour les clients sur Windows 10 et Xbox, afficher des images de logo chargé au lieu des images à partir de mes packages** , comme décrit dans la section suivante).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Affichage seulement téléchargé des images de logo dans le Store

Vous avez la possibilité peuvent empêcher le Store d’utiliser les images de logo dans les packages de votre application lors de l’affichage de votre annonce aux clients sur Windows 10 (y compris Xbox) et à la place le Store utiliser uniquement les images que vous téléchargez. Cette opération vous permet de mieux contrôler l’apparence de votre application sur différents affichages partout dans le Store pour les clients sur Windows 10 (y compris Xbox). (Si votre application publiée précédemment prend en charge les versions antérieures du système d’exploitation, les clients peuvent toujours voir les images à partir de vos packages.)

Pour que le Store utiliser uniquement les images que vous téléchargez (pour les clients sur Windows 10, notamment la Xbox) et n’utilisez pas les images à partir de vos packages, cochez la case indiquant que **pour les clients sur Windows 10 et Xbox, afficher des images de logo chargé au lieu des images à partir de Mes packages**.

Lorsque vous activez cette case, une nouvelle section appelée **afficher des images Store** s’affiche. Ici, vous pouvez charger des 3 images, y compris le **icône de vignette 1:1 application (300 x 300 pixels)** taille (si vous cochez la case, le champ pour fournir cette image est déplacés dans cette section). Nous vous recommandons de fournir toutes les tailles d’image trois si vous utilisez cette option : 300 x 300, 150 x 150 et 71 x 71 pixels. Toutefois, seule la taille 300 x 300 est obligatoire.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Ressources supplémentaires et des codes de fin

Cette section vous permet de fournir une conception graphique pour améliorer l’affichage de votre produit dans le Store. Nous vous recommandons de fournir ces images pour rendre la description dans le Store plus attrayante.

> [!TIP]
> Le [art Super héros de 16:9](#windows-10-and-xbox-image-169-super-hero-art) image est particulièrement recommandée si vous prévoyez d’inclure [remorques vidéo](#trailers) dans votre liste de Store ; si vous ne l’incluez, vos codes de fin ne s’affiche en haut de votre annonce.


### <a name="trailers"></a>Bandes-annonces

Les bandes-annonces sont de courtes vidéos qui permettent aux clients de voir votre produit en action et de s’en faire ainsi une meilleure idée. Elles sont affichées en haut de la liste de votre application Store (tant que vous incluez un [art Super héros de 16:9](#windows-10-and-xbox-image-169-super-hero-art) image). 

Les bandes-annonces sont codées avec la fonctionnalité de diffusion en continu lisse [Smooth Streaming](https://www.iis.net/downloads/microsoft/smooth-streaming), qui adapte la qualité du flux vidéo fourni aux clients en temps réel, en fonction de la bande passante disponible et des ressources processeur.

> [!NOTE]
> Les bandes-annonces s'affichent uniquement pour les clients sur Windows 10, version 1607 ou ultérieure (incluant Xbox).

### <a name="upload-trailers"></a>Charger les bandes-annonces

Vous pouvez ajouter jusqu'à 15 bandes-annonces à votre description dans le Store. Veillez à ce qu'elles répondent toutes aux [exigences](#trailer-requirements) répertoriées ci-dessous.

Vous devez charger un fichier vidéo (.mp4 ou .mov), une vignette et un titre pour chaque bande-annonce fournie.

> [!IMPORTANT]
> Lorsque vous utilisez des codes de fin, vous devez également fournir un [art Super héros de 16:9](#windows-10-and-xbox-image-169-super-hero-art) section dans l’ordre pour vos codes de fin apparaissent en haut de votre Store liste l’image. Cette image s’affichera après la diffusion de vos bandes-annonces.

Pour concevoir des bandes-annonces efficaces, appliquez les recommandations suivantes :
- Les bandes-annonces doivent être de bonne qualité et de courte durée (60 secondes au maximum et moins de 2 Go recommandés). 
- Utilisez une vignette différente pour chaque bande-annonce afin que les clients sachent qu’elles sont uniques.
- Comme certaines dispositions du Store peuvent légèrement rogner le haut et le bas de votre bande-annonce, assurez-vous que les informations principales s’affichent au centre de l’écran.
- La résolution et la fréquence d’images doivent correspondre au document source. Par exemple, un contenu filmé à 720p60 doit être codé et chargé à 720p60. 

Vous devez également respecter la configuration requise répertoriée ci-dessous.

**Pour ajouter des codes de fin à votre annonce :**
1. Chargez le **fichier vidéo** de votre bande-annonce dans la case indiquée. Une zone de liste déroulante s’affiche également au cas où vous souhaiteriez réutiliser une bande-annonce que vous avez déjà chargée (par exemple, pour une description dans le Store dans une autre langue).
2. Après avoir chargé la bande-annonce, vous devez charger une **image miniature** pour l’accompagner. Il doit s'agir d'un fichier .png de 1920 x 1080 pixels, et généralement d'une image fixe extraite de la bande-annonce.
3. Cliquez sur l’icône de crayon pour ajouter un **titre** à votre bande-annonce (255 caractères au maximum).
4. Si vous souhaitez ajouter d'autres bandes-annonces à la liste, cliquez sur **Ajouter une bande-annonce** et répétez les étapes indiquées ci-dessus.

> [!TIP]
> Si vous avez créé des descriptions dans le Store en plusieurs langues, vous pouvez sélectionner **Choisir à partir de bandes-annonces existantes** pour réutiliser les bandes-annonces que vous avez déjà chargées. Vous n’êtes pas obligé de les charger une à une pour chaque langue.

Pour supprimer une bande-annonce d'une liste, cliquez sur le **X** en regard de son nom de fichier. Vous pouvez choisir s’il faut supprimer uniquement la Store la liste dans laquelle vous travaillez, ou supprimez-le de toutes les annonces de Store de votre produit (dans tous les langages).


### <a name="trailer-requirements"></a>Exigences pour les bandes-annonces

Lorsque vous fournissez vos bandes-annonces, veillez à respecter ces exigences :

- Le format vidéo doit être MOV ou MP4. Si vous téléchargez des vidéo 4K, MP4 uniquement est pris en charge.
- La durée de la vidéo ne doit pas dépasser 60 secondes.
- La taille du fichier de la bande-annonce ne doit pas dépasser 2 Go. 
- La résolution vidéo doit être 1920 x 1080 pixels ou 3840 x 2160 pixels.
- La vignette doit être un fichier PNG avec une résolution de 1920 x 1080 pixels ou 3840 x 2160 pixels.
- Le titre ne doit pas dépasser 255 caractères. 
- N’incluez pas les classifications d'âge minimum dans vos bandes-annonces.

Comme les autres champs sur la page de description du Store, les bandes-annonces doivent obtenir une certification pour pouvoir être publiées sur le Microsoft Store. Assurez-vous que vos bandes-annonces respectent les [Politiques du Microsoft Store](store-policies.md).

Il existe des exigences supplémentaires en fonction du type de fichier.

#### <a name="mov"></a>MOV

| Vidéo | Audio | 
| --- | --- | 
| <ul><li>ProRes 1080p (HQ le cas échéant)</li><li>Fréquence d’images native ; 29,97 ips de préférence</li></ul> | <ul><li>Stéréo obligatoire</li><li>Niveau Audio recommandé : -16 LKFS/LUFS</li></ul> |


#### <a name="mp4"></a>MP4

| Vidéo | Audio |
| --- | --- |
| <ul><li>Codec : [H.264](https://docs.microsoft.com/en-us/windows/desktop/DirectShow/h-264-video-types) (AVC1)  </li><li>Balayage progressif (pas d'entrelacement)</li><li>High Profile</li><li>2 images B consécutives</li><li>GOP fermé. GOP de la moitié de la fréquence d’images</li><li>CABAC</li><li>50 Mo/s </li><li>Espace de couleurs : 4.2.0</li></ul> | <ul><li>Codec : AAC-LC</li><li>Canaux : Stéréo ou son à effet surround</li><li>Taux d’échantillonnage : 48 kHz</li><li>Vitesse de transmission audio : 384 Ko/s pour la stéréo, 512 Ko/s pour son à effet surround</li></ul> |

> [!WARNING]
> Les clients ne peuvent pas entendre l’audio pour les fichiers MP4 encodés avec des codecs autre que AVC1.

Pour les fichiers Mezzanine H.264, nous recommandons les éléments suivants :
- Conteneur : MP4
- Pas de listes de modifications (ou vous risquez de perdre la synchronisation AV)
- moov atom en tête du fichier (démarrage rapide)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Image Windows 10 et Xbox (Image principale super 16:9)

Dans le **image Windows 10 et Xbox** section, le **art Super héros de 16:9 (pixels de 1920 x 1080 ou 3840 x 2160)** image est utilisée dans les différentes dispositions sous la forme dans le Microsoft Store sur tous les types d’appareils Windows 10 (y compris Xbox). Nous vous recommandons de fournir cette image, quels que soient les versions du système d’exploitation ou les types d’appareils ciblés par votre application.

Cette image est *requise* pour l’obtention d’un affichage correct si votre description inclut des [bandes-annonces vidéo](#trailers). Pour les clients utilisant Windows 10, version 1607 ou ultérieure (incluant Xbox), cette image est utilisée comme image principale dans la partie supérieure de votre description (ou apparaît après la diffusion des bandes-annonces). Elle peut également servir à recommander votre application dans les dispositions promotionnelles partout dans le Store. Notez que cette image ne doit pas inclure le titre du produit ou tout autre texte.

Voici quelques conseils à suivre lors de l’élaboration de cette image :

- Cette image doit être au format .png de 1920 x 1080 pixels ou 3840 x 2160 pixels.
- Sélectionnez une image dynamique en rapport avec l’application qui permette de la reconnaître et de la différencier. Évitez les photos issues de banques d’images ou les images génériques.
- N’insérez pas de texte dans l’image.
- Évitez de placer des éléments visuels clés dans le tiers inférieur de l’image (étant donné que dans certaines dispositions, nous pouvons appliquer un dégradé sur cette partie).
- Placez les informations les plus importantes dans le centre de l’image (étant donné que dans certaines dispositions, nous pouvons rogner l’image).
- Réduisez l’espace vide.
- Éviter de montrer l’interface utilisateur de votre application et n’utilisez pas d’images spécifiques à des appareils.
- Évitez les thèmes politiques et nationaux, les drapeaux ou les symboles religieux.
- N’incluez pas d’images en rapport avec des gestes irrespectueux, la nudité, le jeu, l’argent, la drogue, le tabac ou l’alcool.
- N’utilisez pas d’armes pointant vers l’utilisateur ou une violence excessive.

Si la présence de cette image nous permet de prendre en compte votre application pour les offres promotionnelles que nous recommandons, elle ne garantit pas que votre application sera recommandée. Pour plus d’informations, consultez l’article [Faciliter la promotion de votre application](make-your-app-easier-to-promote.md).


### <a name="xbox-images"></a>Images Xbox

Si vous publiez votre application sur Xbox, ces images sont requises pour un affichage correct. 

Vous pouvez charger 3 tailles différentes :
- **Marque la clé art, 584 x 800 pixels**: Doit inclure le titre du produit. Une barre de marque est requise sur cette image. Placez le titre et toutes les images clés dans les trois quarts supérieurs de l’image, dans la mesure où un effet de superposition peut apparaître dans le quart inférieur.
- **Intitulée art héros, 1920 x 1080 pixels**: Doit inclure le titre du produit. Placez le titre et toutes les images clés dans les trois quarts supérieurs de l’image, dans la mesure où un effet de superposition peut apparaître dans le quart inférieur.
- **En vedette art promotionnelle carré, 1080 x 1080 pixels**: Doit *pas* inclure le titre du produit.

> [!NOTE]
> Pour optimiser l’affichage sur Xbox, vous devez également fournir une image **9:16 (720 x 1 080 ou 1 440 x 2 160 pixels)** dans la section [Logos Windows Store](#store-logos).


### <a name="holographic-image"></a>Image holographique

L’image **2:1 (2400 x 1200)** est uniquement utilisée si votre application prend en charge la famille d’appareils Holographique. Si tel est le cas, nous vous recommandons de fournir cette image.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Images uniquement pour Windows 8.x et/ou Windows Phone 8.x 

Si votre application précédemment soumis prend en charge les versions antérieures du système d’exploitation (Windows 8.x et/ou de Windows Phone 8.x), ces images doivent être fournis dans l’ordre pour nous permettre de prendre en compte présentant votre application dans des présentations promotionnelles (même si elles ne garantissent pas que votre application est proposée). Si votre application ne prend pas en charge ces versions de système d’exploitation, ignorez cette section. (Cette section était précédemment appelée **Images promotionnelles facultatives**.)

**Pour Windows Phone 8.1 et versions antérieures**, deux tailles d’image peuvent être utilisées dans les dispositions de promotions : **1 000 x 800 pixels (5:4)** et **358 x 358 pixels (1:1)** . Si votre application s’exécute sur Windows Phone 8.1 ou version antérieure, nous vous recommandons de fournir des images dans les deux de ces tailles.  

> [!TIP]
> Veillez à fournir une image en 300 x 300 pour l’icône de vignette d'application dans la section [Logos Store](#store-logos) pour toute soumission qui prend en charge Windows Phone 8.1 ou versions antérieures. Cela permet de garantir que votre application n’apparaît pas dans le Store avec une icône vide.  

**Pour Windows 8.1 et versions antérieures**, les mises en forme promotionnelles peuvent utiliser une image au format **414 x 180 pixels**. Si votre application s’exécute sur Windows 8.1 ou version antérieure, nous vous recommandons de fournir une image dans cette sizen.




