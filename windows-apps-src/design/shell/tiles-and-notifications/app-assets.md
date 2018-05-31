---
author: mijacobs
Description: App icon assets, which appear in a variety of forms throughout the Windows 10 operating system, are the calling cards for your Universal Windows Platform (UWP) app.
title: Ressources de vignette et d’icône
ms.assetid: D6CE21E5-2CFA-404F-8679-36AA522206C7
label: Tile and icon assets
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc614f7803e7546add8ecccb7cc20dc0250d0d22
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2018
ms.locfileid: "1522788"
---
# <a name="guidelines-for-tile-and-icon-assets"></a>Recommandations en matière de ressources de vignette et d’icône

 


Les ressources d’icône d’application, qui s’affichent sous différentes formes dans le système d’exploitation Windows10, sont les cartes de visite de votre application de plateforme Windows universelle (UWP). Ces recommandations précisent où apparaissent les ressources d’icône d’application dans le système et fournissent des conseils de conception détaillés pour vous aider à créer les plus belles icônes.

![Écran d’accueil et vignettes de Windows10](images/assetguidance01.jpg)

## <a name="adaptive-scaling"></a>Mise à l’échelle adaptative


Tout d’abord, voici une brève vue d’ensemble de la mise à l’échelle adaptative pour mieux comprendre comment fonctionne la mise à l’échelle avec des ressources. Windows 10 introduit une évolution du modèle de mise à l’échelle existant. En plus de la mise à l’échelle du contenu vectoriel, il existe un ensemble unifié de facteurs d’échelle qui assurent la cohérence de taille des éléments d’interface utilisateur en fonction de diverses tailles d’écran et résolutions d’affichage. Ces facteurs d’échelle sont également compatibles avec les facteurs d’échelle d’autres systèmes d’exploitation tels qu’iOS et Android. Cela facilite le partage de ressources entre ces plateformes.

Le Store sélectionne les ressources à télécharger notamment en fonction de la résolution de l’appareil. Seules les ressources correspondant au mieux à l’appareil sont téléchargées.

## <a name="tile-elements"></a>Éléments de vignette


Les composants de base d’une vignette de démarrage comprennent une plaque de fond, une icône, une barre de marque, des marges et un titre d’application:

![Décomposition des éléments d’une vignette](images/assetguidance02.png)

La barre de marque au bas d’une vignette comprend le nom de l’application, le badge et le compteur (le cas échéant):

![Barre de marque de la vignette](images/assetguidance03.png)

La hauteur de la barre de marque dépend du facteur d’échelle de l’appareil sur lequel la vignette s’affiche:

| Facteur d’échelle | Pixels |
|--------------|--------|
| 100%         | 32     |
| 125 %         | 40     |
| 150 %         | 48     |
| 200 %         | 64     |
| 400 %         | 128    |

 

Le système définit les marges de vignette. Elles ne peuvent pas être modifiées. La majorité du contenu s’affiche entre les marges, comme illustré dans cet exemple:

![Marges de vignette](images/assetguidance04.png)

La largeur des marges dépend du facteur d’échelle de l’appareil sur lequel la vignette s’affiche:

| Facteur d’échelle | Pixels |
|--------------|--------|
| 100%         | 8      |
| 125 %         | 10     |
| 150 %         | 12     |
| 200 %         | 16     |
| 400 %         | 32     |

 

## <a name="tile-assets"></a>Ressources de vignette


Chaque ressource de vignette a la même taille que la vignette sur laquelle elle est positionnée. Vous pouvez personnaliser les vignettes de votre application avec deux représentations différentes d’une ressource:

1. Une icône ou un logo centrés avec remplissage. La couleur de la plaque de fond apparaît ainsi sur les éléments suivants:

![Vignette et plaque de fond](images/assetguidance05.png)

2. Une vignette personnalisée en pleine page sans remplissage:

![Vignette en pleine page](images/assetguidance06.png)

Par souci de cohérence entre les appareils, chaque taille de vignette (petite, moyenne, large et grande) a son propre ratio de dimensionnement. Pour obtenir un positionnement homogène de l’icône sur l’ensemble des vignettes, nous vous donnons quelques recommandations de base en matière de remplissage pour les tailles de vignette suivantes. L’emplacement idéal d’une icône est à l’intersection des deux superpositions violettes. Même si les icônes ne tiennent pas toujours à l’intérieur de l’emplacement, le volume visuel d’une icône devrait plus ou moins correspondre aux exemples fournis.

Dimensionnement d’une petite vignette:

![Exemple de dimensionnement d’une petite vignette](images/assetguidance07a.png)

Dimensionnement d’une vignette moyenne:

![Exemple de dimensionnement d’une vignette moyenne](images/assetguidance07b.png)

Dimensionnement d’une vignette large:

![Exemple de dimensionnement d’une vignette large](images/assetguidance07c.png)

Dimensionnement d’une grande vignette:

![Exemple de dimensionnement d’une grande vignette](images/assetguidance07d.png)

Dans cet exemple, l’icône est trop grande pour la vignette:

![Icône trop grande pour la vignette](images/assetguidance08a.png)

Dans cet exemple, l’icône est trop petite pour la vignette:

![Icône trop petite pour la vignette](images/assetguidance08b.png)

Les taux de remplissage suivants sont particulièrement adaptés aux icônes positionnées à l’horizontale ou à la verticale.

Pour les petites vignettes, limitez la largeur et la hauteur de l’icône à 66% de la taille de la vignette:

![Ratios de dimensionnement d’une petite vignette](images/assetguidance09.png)

Pour les vignettes moyennes, limitez la largeur de l’icône à 66% et la hauteur de l’icône à 50% de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque:

![Ratios de dimensionnement d’une vignette moyenne](images/assetguidance10.png)

Pour les vignettes larges, limitez la largeur de l’icône à 66% et la hauteur de l’icône à 50% de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque:

![Ratios de dimensionnement d’une vignette large](images/assetguidance11.png)

Pour les grandes vignettes, limitez la largeur de l’icône à 66% et la hauteur de l’icône à 50% de la taille de la vignette:

![Ratios de dimensionnement d’une grande vignette](images/assetguidance12.png)

Certaines icônes sont conçues pour être positionnées à l’horizontale ou à la verticale, tandis que certaines ont des formes plus complexes qui les empêchent de respecter les dimensions cibles. Les icônes qui semblent centrées peuvent déborder d’un côté. Dans ce cas, certaines parties de l’icône peuvent se retrouver hors de l’emplacement recommandé, à condition que l’icône offre le même attrait visuel qu’une icône aux bonnes dimensions:

![Trois icônes centrées](images/assetguidance13.png)

Pour les ressources pleine page, prenez en compte les éléments qui interagissent avec les marges et les bords des vignettes. Conservez des marges équivalentes à au moins 16 % de la hauteur ou de la largeur de la vignette. Ce pourcentage représente le double de la largeur des marges pour la plus petite taille de vignette:

![Vignette pleine page avec marges](images/assetguidance14.png)

Dans cet exemple, les marges sont trop étroites:

![Vignette pleine page avec marges trop étroites](images/assetguidance15.png)

## <a name="tile-assets-in-list-views"></a>Ressources de vignette dans les affichages Liste


Les vignettes peuvent également apparaître dans un affichage Liste. Les recommandations de dimensionnement des ressources de vignette qui apparaissent dans les affichages Liste diffèrent légèrement de celles des ressources de vignette décrites précédemment. Cette section détaille ces caractéristiques de dimensionnement spécifiques.

![Ressources de vignette dans un affichage Liste](images/assetguidance16.png)

Limitez la largeur et la hauteur de l’icône à 75% de la taille de la vignette:

![Dimensionnement d’une icône de vignette en affichage Liste](images/assetguidance17.png)

Pour les icônes horizontales et verticales, limitez la largeur et la hauteur de l’icône à 75% de la taille de la vignette:

![Dimensionnement d’une icône de vignette en affichage Liste](images/assetguidance18.png)

Pour des illustrations pleine page d’éléments de marque importants, conservez des marges d’au moins 12,5%:

![Illustration pleine page d’une vignette en affichage Liste](images/assetguidance19.png)

Dans cet exemple, l’icône est trop grande pour la vignette:

![Icône trop grande pour la vignette](images/assetguidance20a.png)

Dans cet exemple, l’icône est trop petite pour la vignette:

![Icône trop petite pour la vignette](images/assetguidance20b.png)

## <a name="target-based-assets"></a>Ressources basées sur la taille de la cible avec plaque


Les ressources basées sur la cible sont destinées aux icônes et vignettes qui apparaissent dans la barre des tâches Windows, dans les applications actives, dans l’affichage Alt+Tab, dans l’alignement automatique et dans le coin inférieur droit des vignettes de démarrage. Vous n’avez pas besoin d’ajouter du remplissage à ces ressources ; Windows s’en charge le cas échéant. Ces ressources doivent présenter un encombrement minimal de 16 pixels. Voici un exemple de ces ressources telles qu’elles apparaissent dans les icônes de la barre des tâches Windows:

![Ressources dans la barre des tâches Windows](images/assetguidance21.png)

Même si ces interfaces utilisateur positionnent par défaut une ressource basée sur la cible sur une plaque de fond en couleur, vous pouvez également utiliser une ressource basée sur la cible sans plaque. Lorsque vous créez des ressources sans plaque, pensez au fait qu’elles pourront apparaître sur différentes couleurs d’arrière-plan.

![Ressources avec et sans plaque](images/assetguidance22.png)

Voici des recommandations de dimensionnement pour les ressources basées sur la cible, à l’échelle 100%:

![Dimensionnement des ressources basées sur la cible à l’échelle 100%](images/assetguidance23.png)

## <a name="splash-screen-assets"></a>Ressources d’écran de démarrage


L’image d’écran de démarrage peut être fournie sous la forme d’un chemin d’accès direct à un fichier image ou en tant que ressource. En utilisant une référence à une ressource, vous pouvez fournir des images d’échelles différentes afin que Windows puisse choisir la taille la mieux adaptée à l’appareil et à la résolution d’écran. Vous pouvez également fournir des images à contraste élevé pour l’accessibilité et des images localisées correspondant aux différentes langues d’interface utilisateur.

Si vous ouvrez le fichier « Package.appxmanifest » dans un éditeur de texte, l’élément [**SplashScreen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) s’affiche en tant qu’enfant de l’élément [**VisualElements**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements). Le balisage de l’écran de démarrage par défaut dans le fichier manifeste ressemble à ceci dans un éditeur de texte :

```XML
<uap:SplashScreen Image="Assets\SplashScreen.png" /></code></pre></td>
</tr>
</tbody>
</table>
```

Les ressources d’écran de démarrage sont centrées sur chaque appareil sur lequel elles apparaissent:

![Dimensionnement des ressources d’écran de démarrage](images/assetguidance27.png)

## <a name="high-contrast-assets"></a>Ressources à contraste élevé


Le mode de contraste élevé utilise des jeux de ressources distincts pour le blanc à contraste élevé (arrière-plan blanc avec texte noir) et le noir à contraste élevé (arrière-plan noir avec texte blanc). Si vous ne fournissez pas de ressources à contraste élevé pour votre application, les ressources standard seront utilisées.

Si les ressources standard de votre application offrent un affichage acceptable sur un arrière-plan noir et blanc, alors votre application pourra au moins offrir un affichage satisfaisant en mode de contraste élevé. Si vos ressources standard n’offrent pas un affichage acceptable sur un arrière-plan noir et blanc, envisagez d’inclure des ressources à contraste élevé. Ces exemples illustrent les deux types de ressources à contraste élevé:

![Exemples de ressources à contraste élevé](images/assetguidance28.png)

Si vous décidez de fournir des ressources à contraste élevé, vous devez inclure les deux types de ressources: blanc sur noir et noir sur blanc. Au moment d’ajouter ces ressources à votre package, vous pouvez créer un dossier «contraste noir» pour les ressources blanc sur noir et un dossier «contraste blanc» pour les ressources noir sur blanc.

## <a name="asset-size-tables"></a>Tableaux des tailles de ressource


Nous vous recommandons vivement de fournir au moins des ressources pour les facteurs d’échelle 100, 200 et 400. Si vous fournissez des ressources pour tous les facteurs d’échelle, l’expérience utilisateur sera optimale.

<br/>

<table>
<thead>
<tr><th colspan="3">Petite vignette (Square71x71Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Échelle 100%</td>
    <td width="20%">71x71</td>
    <td>Square71x71Logo.Scale-100.png</td>
</tr>
<tr>
    <td>Échelle 125%</td>
    <td>89x89</td>
    <td>Square71x71Logo.scale-125.png</td>
</tr>
<tr>
    <td>Échelle 150%</td>
    <td>107x107</td>
    <td>Square71x71Logo.scale-150.png</td>
</tr>
<tr>
    <td>Échelle 200%</td>
    <td>142x142</td>
    <td>Square71x71Logo.scale-200.png</td>
</tr>
<tr>
    <td>Échelle 400%</td>
    <td>284x284</td>
    <td>Square71x71Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Vignette moyenne (Square150x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Échelle 100%</td>
    <td width="20%">150x150</td>
    <td>Square150x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>Échelle 125%</td>
    <td>188x188</td>
    <td>Square150x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>Échelle 150%</td>
    <td>225x225</td>
    <td>Square150x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>Échelle 200%</td>
    <td>300x300</td>
    <td>Square150x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>Échelle 400%</td>
    <td>600x600</td>
    <td>Square150x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Vignette large (Wide310x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Échelle 100%</td>
    <td width="20%">310x150</td>
    <td>Wide310x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>Échelle 125%</td>
    <td>388x188</td>
    <td>Wide310x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>Échelle 150%</td>
    <td>465 x 225</td>
    <td>Wide310x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>Échelle 200%</td>
    <td>620x300</td>
    <td>Wide310x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>Échelle 400%</td>
    <td>1240x600</td>
    <td>Wide310x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Grande vignette (Square310x310Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Échelle 100%</td>
    <td width="20%">310x310</td>
    <td>Square310x310Logo.scale-100.png</td>
</tr>
<tr>
    <td>Échelle 125%</td>
    <td>388x388</td>
    <td>Square310x310Logo.scale-125.png</td>
</tr>
<tr>
    <td>Échelle 150%</td>
    <td>465x465</td>
    <td>Square310x310Logo.scale-150.png</td>
</tr>
<tr>
    <td>Échelle 200%</td>
    <td>620x620</td>
    <td>Square310x310Logo.scale-200.png</td>
</tr>
<tr>
    <td>Échelle 400%</td>
    <td>1240x 1240</td>
    <td>Square310x310Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Icône de la liste d’applications (Square44x44Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Échelle 100%</td>
    <td width="20%">44x44</td>
    <td>Square44x44Logo.scale-100.png</td>
</tr>
<tr>
    <td>Échelle 125%</td>
    <td>55x55</td>
    <td>Square44x44Logo.scale-125.png</td>
</tr>
<tr>
    <td>Échelle 150%</td>
    <td>66x66</td>
    <td>Square44x44Logo.scale-150.png</td>
</tr>
<tr>
    <td>Échelle 200%</td>
    <td>88x88</td>
    <td>Square44x44Logo.scale-200.png</td>
</tr>
<tr>
    <td>Échelle 400%</td>
    <td>176x176</td>
    <td>Square44x44Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Écran de démarrage (SplashScreen)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Échelle 100%</td>
    <td width="20%">620x300</td>
    <td>SplashScreen.scale-100.png</td>
</tr>
<tr>
    <td>Échelle 125%</td>
    <td>775x375</td>
    <td>SplashScreen.scale-125.png</td>
</tr>
<tr>
    <td>Échelle 150%</td>
    <td>930x450</td>
    <td>SplashScreen.scale-150.png</td>
</tr>
<tr>
    <td>Échelle 200%</td>
    <td>1240x600</td>
    <td>SplashScreen.scale-200.png</td>
</tr>
<tr>
    <td>Échelle 400%</td>
    <td>2480x1200</td>
    <td>SplashScreen.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>
 

**Ressources basées sur la taille de la cible avec plaque**

Les ressources basées sur la cible sont utilisées dans plusieurs facteurs d’échelle. Le nom d’élément pour les ressources basées sur la cible est **Square44x44Logo**. Nous vous recommandons vivement de fournir au moins les ressources suivantes :

16 x 16, 24 x 24, 32 x 32, 48 x 48, 256 x 256

Le tableau suivant répertorie toutes les tailles de ressources basées sur la cible et les exemples de nom de fichier correspondants :

| Taille de la ressource | Exemple de nom de fichier                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

 

\* Fournissez ces tailles de ressource à titre de référence

## <a name="asset-types"></a>Types de ressources


Voici tous les types de ressources, leur utilisation et les noms de fichier recommandés.

**Ressources de vignette**

-   Les ressources centrées sont généralement utilisées sur l’écran de démarrage pour présenter votre application.
-   Format du nom de fichier: [Square\Wide]\*x\*Logo.scale-\*.png
-   Applications concernées: toutes les applications UWP
-   Utilisations :
    -   Vignettes d’écran de démarrage par défaut (bureau et appareil mobile)
    -   Centre de maintenance (bureau et appareil mobile)
    -   Sélecteur de tâches (appareil mobile)
    -   Sélecteur Partager (appareil mobile)
    -   Sélecteur (appareil mobile)
    -   Windows Store

**Ressources évolutives avec plaque**

-   Ces ressources sont utilisées sur les surfaces qui requièrent des facteurs d’échelle. Les ressources sont équipées d’une plaque par le système ou sont fournies avec leur propre couleur d’arrière-plan si l’application inclut cette fonctionnalité.
-   Format de nom de fichier: Square44x44Logo.scale-\*.png
-   Applications concernées: toutes les applications UWP
-   Utilisations :
    -   Liste Toutes les applications du menu Démarrer (bureau)
    -   Liste Les plus utilisées du menu Démarrer (bureau)
    -   Gestionnaire des tâches (bureau)
    -   Résultats de recherche Cortana
    -   Liste Toutes les applications du menu Démarrer (appareil mobile)
    -   Paramètres

**Ressources basées sur la taille de la cible avec plaque**

-   Ces tailles de ressource fixes ne sont pas mises à l’échelle avec les plaques. Principalement utilisées pour les expériences héritées. Les ressources sont vérifiées par le système.
-   Format de nom de fichier: Square44x44Logo.targetsize-\*.png
-   Applications concernées: toutes les applications UWP
-   Utilisations :
    -   Liste des raccourcis du menu Démarrer (bureau)
    -   Coin inférieur de la vignette du menu Démarrer (bureau)
    -   Raccourcis (bureau)
    -   Panneau de configuration (bureau)

**Ressources basées sur la taille de la cible sans plaque**

-   Ces ressources ne sont pas équipées de plaque ni mises à l’échelle par le système.
-   Format de nom de fichier: Square44x44Logo.targetsize-\*\_altform-unplated.png
-   Applications concernées: toutes les applications UWP
-   Utilisations :
    -   Barre des tâches et miniature de la barre des tâches (bureau)
    -   Liste des raccourcis de la barre des tâches
    -   Applications actives
    -   Alt+Tab

**Ressources d’extension de fichier**

-   Ces ressources sont spécifiques aux extensions de fichier. Elles s’affichent en regard des icônes d’association de fichier de type Win32 dans l’Explorateur de fichiers et doivent être indépendantes du thème défini. Le dimensionnement diffère selon les plateformes mobiles et de bureau.
-   Format de nom de fichier: \*LogoExtensions.targetsize-\*.png
-   Applications concernées: Musique, Vidéo, Photos, MicrosoftEdge, Microsoft Office
-   Utilisations :
    -   Explorateur de fichiers
    -   Cortana
    -   Différentes surfaces d’interface utilisateur (bureau)

**Écran de démarrage**

-   La ressource qui s’affiche sur l’écran de démarrage de votre application. Elle est automatiquement mise à l’échelle sur les plateformes mobiles et de bureau.
-   Format de nom de fichier: SplashScreen.scale-*.png
-   Applications concernées: toutes les applications UWP
-   Utilisations :
    -   Écran de démarrage de l’application