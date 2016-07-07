---
author: jnHs
Description: "Votre application doit inclure différents logos, différentes captures d’écran et différentes images."
title: "Images et captures d’écran de l’application"
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.sourcegitcommit: ecb030b7c529f765eded46e4e3e9db99ad0c27e8
ms.openlocfilehash: 9eac5658e2ac04b2abc1bf06abf5c73b16260bc7

---

# Images et captures d’écran de l’application


Votre application doit inclure différents logos, différentes captures d&#39;écran et différentes images. Certains de ces éléments sont obligatoires et d’autres facultatifs. Gardez à l’esprit que vos images sont l’un des principaux moyens de représenter votre application. Des images bien conçues ont leur utilité pour rendre votre application attrayante pour les clients.

Pendant le [processus de soumission de l'application](app-submissions.md), vous fournissez des [captures d'écran](#screenshots) et des [illustrations publicitaires](#promotional-artwork) à l'étape [Descriptions](create-app-descriptions.md). Ces images aident à afficher votre application dans le Windows Store.

Le Store utilise également la vignette et d’autres images de votre application que vous incluez dans le package de votre application. Exécutez le [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) pour déterminer si une image requise est manquante. Pour obtenir des conseils et des recommandations sur ces images, voir [Ressources de vignette et d’icône](../controls-and-patterns/tiles-and-notifications-app-assets.md).

> **Remarque** La façon dont les images sont utilisées dans le Windows Store, sur l’écran d’accueil du client, ainsi que dans l’application elle-même, peut varier en fonction du système d’exploitation du client et d’autres facteurs.


## Images fournies pendant le processus de soumission

Lorsque vous entrez les informations de description de votre application, vous pouvez fournir plusieurs captures d'écran (au minimum une) et des conceptions graphiques promotionnelles. Ces images ne proviennent pas du package de votre application. Vous devez les fournir à l'étape **Description** pour chaque langue prise en charge.

Le tableau suivant répertorie les différentes images que vous pouvez charger et explique comment les utiliser. Plus de détails sont fournis dans les sections ci-dessous.

| Image                                                       | Taille de pixel                           | Utilisation                                                                                                                                                                           | Quand l’inclure                                                                                                                                            |
|-------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Captures d’écran de bureau](#screenshots)                         | 1366x768 ou plus                 | Affichées dans votre description de l’application dans le Windows Store lorsqu’elles sont consultées sur un appareil de bureau.                                                                                                          | Recommandées pour toutes les applications, en particulier si votre application est conçue pour une utilisation sur des appareils de bureau. Au moins une capture d’écran (pour toute famille d’appareils) est requise. |
| [Captures d’écran pour appareils mobiles](#screenshots)                          | 768 x 1 280, 720 x 1 280, ou 480 x 800 | Affichées dans votre description de l’application dans le Windows Store lorsqu’elles sont consultées sur un appareil mobile.                                                                                                           | Recommandées pour toutes les applications, en particulier si votre application est conçue pour une utilisation sur des appareils mobiles. Au moins une capture d’écran (pour toute famille d’appareils) est requise. |
| [Captures d’écrans holographiques](#screenshots)                          | 1268x720 ou plus                 | Affichées dans votre description de l’application dans le Windows Store lorsqu’elles sont consultées sur un appareil holographique.                                                                                                           | Recommandées si votre application est conçue pour une utilisation sur Microsoft HoloLens. Au moins une capture d’écran (pour toute famille d’appareils) est requise. |
| [Icône de vignette d’application](#app-tile-icon)                             | 300 x 300                            | Affichée comme icône de votre application dans le Windows Store pour Windows Phone 8.1 et versions antérieures (et, si vous disposez uniquement de packages ciblant Windows Phone 8.1 ou versions antérieures, dans le Windows Store sur Windows 10) | Requise pour un affichage correct dans le Windows Store si votre application cible Windows Phone 8.1 ou versions antérieures.                                                                 |
| [Image promotionnelle : Windows 10 Spotlight](#promotional-artwork) | 2 400 x 1 200                          | Utilisée pour des mises en forme promotionnelles dans le Windows Store sur Windows 10.                                                                                                                        | Recommandée pour toutes les applications, notamment celles dotées de packages UWP ciblant les clients Windows 10                                                               |
| [Images promotionnelles : Windows Phone 8.1 et versions antérieures](#promotional-artwork) | 1 000 x 800, 358 x 358                | Utilisées pour des mises en forme promotionnelles dans le Windows Store sur Windows 8.1.et versions antérieures.                                                                                                     | Recommandées pour toutes les applications ciblant Windows Phone 8.1 ou versions antérieures.                                                                                           |
| [Image promotionnelle : Windows 8.1 et versions antérieures](#promotional-artwork)        | 414 x 180                            | Utilisée pour des mises en forme promotionnelles dans le Windows Store sur Windows 8.1.                                                                                                                       | Recommandée pour toutes les applications ciblant Windows 8.1 ou versions antérieures.                                                                                                 |
 

## Captures d’écran

Les captures d’écran sont les images de votre application que voient vos clients dans la description du Windows Store.

Vous verrez plusieurs champs sur la page **Description**, où vous avez la possibilité de fournir des captures d’écran pour différentes familles d’appareils (qui seront affichées lorsqu’un client consulte la description de votre application dans le Windows Store sur ce type d’appareil).

La soumission de votre application ne nécessite qu’une seule capture d’écran, mais vous pouvez en fournir plusieurs, jusqu’à 9 pour les ordinateurs de bureau et 8 pour les appareils mobiles et holographiques). Vous n’êtes pas tenu de proposer des captures d’écran distinctes pour chaque famille d’appareils, mais nous recommandons de fournir des captures d’écran pour celles prises en charge par votre application afin que les clients sachent à quoi cette dernière ressemblera sur leur appareil.

> **Remarque** Microsoft Visual Studio contient un [outil vous permettant d’effectuer des captures d’écran](http://go.microsoft.com/fwlink/p/?LinkId=221135).

Chaque capture d’écran doit être au format .png en mode paysage ou portrait, et la taille du fichier ne doit pas dépasser 2Mo.

Les exigences de taille varient en fonction de la famille d’appareils:
- Mobile: 768x1280, 720x1280 ou 480x800pixels
- Bureau: 1366x768 pixels ou plus
- Holographique: 1268x720 pixels ou plus

Vous pouvez fournir un bref sous-titre pour chaque capture d’écran (au maximum 200caractères).

> **Remarque** Si vous créez des descriptions pour [plusieurs langues](supported-languages.md), vous disposez d’une page **Description** pour chacune d’elles. Vous devez charger des images pour chaque langue séparément (même si vous utilisez les mêmes images), et fournir des sous-titres à utiliser pour chaque langue.


## Icône de vignette d’application

Elle n’est pas requise pour toutes les soumissions, mais est fortement recommandée si votre application s’exécute sur Windows Phone 8.1 ou versions antérieures. Cette icône est utilisée lors de l’affichage de la description de votre application aux clients sur Windows Phone 8.1 et versions antérieures. Si vous ne fournissez pas cette image, les clients sur Windows Phone 8.1 ou versions antérieures verront une icône vide associée à la description de votre application. (Cela s’applique également aux clients sur Windows 10, si votre application est dotée uniquement de packages ciblant Windows Phone 8.1 ou versions antérieures.)

Cette icône doit être un fichier .png de 300 x 300 pixels.

## Conception graphique promotionnelle

L’équipe de rédaction du Windows Store utilise différentes images pour faire la promotion des applications. Le fait de soumettre une conception graphique promotionnelle permet à l’équipe de rédaction du Windows Store de prendre en compte votre application dans les présentations publicitaires.

> **Important** La présence d’images promotionnelles pour votre application ne garantit pas sa promotion dans le Windows Store. En revanche, si aucune image promotionnelle n’est disponible, votre application ne pourra pas être incluse dans toutes les activités promotionnelles. (Voir [Faciliter la promotion de votre application](make-your-app-easier-to-promote.md) pour plus d’informations.)

Vous voudrez peut-être soumettre votre conception graphique promotionnelle en différentes tailles selon les versions de systèmes d’exploitation ciblées par votre application. Les images doivent être au format .png pour toutes les tailles.

Voici quelques conseils à suivre lors de l’élaboration de votre conception graphique promotionnelle :

-   Sélectionnez des images dynamiques en rapport avec l’application qui permettent de la reconnaître et de la différencier. Évitez les photos issues de banques d’images ou les images génériques.
-   N’incluez pas de texte (excepté celui de votre marque).
-   Réduisez l’espace vide dans votre image.
-   Éviter de montrer l’interface utilisateur de votre application et n’utilisez pas d’images spécifiques à des appareils.
-   Évitez les thèmes politiques et nationaux, les drapeaux ou les symboles religieux.
-   N’incluez pas d’images en rapport avec des gestes irrespectueux, la nudité, le jeu, l’argent, la drogue, le tabac ou l’alcool.
-   N’utilisez pas d’armes pointant vers l’utilisateur ou une violence excessive.

### Pour Windows 10 : 2 400 x 1 200

Dans le Windows Store sur Windows 10, une image de une rotative visant à promouvoir le contenu figure en haut des pages de la catégorie Applications et jeux. Pour que votre application puisse prétendre à ce positionnement, veillez à soumettre une image 2 400 x 1 200.

Lors de la conception de votre image, gardez à l’esprit que si nous l’utilisons pour la une, nous appliquerons un dégradé sur le tiers inférieur de manière à pouvoir afficher lisiblement du texte marketing sur l’image. Par conséquent, évitez de placer du texte et des éléments visuels dans le tiers inférieur. Nous pouvons en outre rogner votre image. Veillez donc à placer la marque de votre application et les informations les plus importantes au centre.

L’image ci-dessous illustre les proportions essentielles à respecter. La « zone sécurisée » au centre demeure intacte même si l’image est rognée. La «zone de texte dynamique» se situe là où un texte et un dégradé peuvent apparaître.

![instructions relatives à l’image de une](images/spotlight1.jpg) L’exemple ci-dessous montre une image de une respectant ces proportions. (Les lignes figurant sur l’image montrant comment la conception s’intègre parfaitement dans les zones désignées ne figureront pas dans l’image finale.)

![image de une bien conçue](images/spotlight2.jpg)
### Pour Windows Phone8.1 et versions antérieures: 1000x800, 358x358

Dans le Windows Store sur Windows Phone 8.1 et versions antérieures, deux tailles d’image peuvent être utilisées dans les images promotionnelles : 1 000 x 800 pixels et 358 x 358 pixels. Si votre application s’exécute sur Windows Phone8.1 ou versions antérieures, nous recommandons de fournir des images dans ces deux tailles pour des questions de promotion.

> **Conseil** Veillez donc à fournir une image d’[icône de vignette d’application](#app-tile-icon) 300 x 300 pour vous assurer qu’une icône vide ne sera pas associée à votre application dans le Windows Store.

### Pour Windows8.1 et versions antérieures: 414x180

Dans le Windows Store sur Windows8.1 et versions antérieures, les mises en forme promotionnelles peuvent utiliser une image au format 414x180pixels. Si votre application s’exécute sur Windows Phone8.1 ou versions antérieures, nous recommandons de fournir une image de cette taille pour des questions de promotion.



<!--HONumber=Jun16_HO4-->


