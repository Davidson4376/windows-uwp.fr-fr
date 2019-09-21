---
title: Lancer l’application Cartes Windows
description: Découvrez comment lancer l’application Cartes Windows à partir de votre application.
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c85eaacd62de9a2efe380197ba467c5009cd0c5
ms.sourcegitcommit: f0588a086cf2499968bf03b10c6bce5f518e90cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "68757435"
---
# <a name="launch-the-windows-maps-app"></a>Lancer l’application Cartes Windows




Découvrez comment lancer l’application Cartes Windows à partir de votre application. Cette rubrique décrit les **BingMaps :** , **MS-Drive-to :** , **MS-Walk-to :** et **MS-Settings :** Schémas de Uniform Resource Identifier (URI). Utilisez ces schémas d’URI afin de lancer l’application Cartes Windows pour des cartes, itinéraires et résultats de recherche spécifiques ou pour télécharger des cartes de l’application Cartes Windows hors connexion à partir de l’application Paramètres.

**Conseil** Pour plus d’informations sur le lancement de l’application Cartes Windows à partir de votre application, téléchargez l’[Exemple de carte pour la plateforme Windows universelle (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619977) à partir du [référentiel Windows-universal-samples](https://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.

## <a name="introducing-uris"></a>Présentation des URI

Les schémas d’URI vous permettent d’ouvrir des applications en cliquant sur des liens hypertexte (ou par programme dans votre application). Tout comme vous pouvez commencer un nouveau message électronique à l’aide de **mailto:** , ou ouvrir un navigateur web à l’aide de **http:** , vous pouvez accéder à l’application Cartes Windows à l’aide de **bingmaps:** , **ms-drive-to:** et **ms-walk-to:** .

-   **BingMaps :** L’URI fournit des mappages pour les emplacements, les résultats de recherche, les directions et le trafic.
-   Le **lecteur ms-to :** L’URI fournit des directions de marche par tour à partir de votre emplacement actuel.
-   Le **MS-Walk-to :** L’URI fournit des directions de parcours à la main à partir de votre emplacement actuel.

Par exemple, l’URI suivant ouvre l’application Cartes Windows et affiche une carte centrée sur la ville de New York.

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![carte centrée sur la ville de New York.](images/mapnyc.png)

Voici une description du schéma d’URI :

**BingMaps :? Query**

Dans ce schéma d’URI, l’élément *query* est une série de paires nom/valeur de paramètre :

**& param1 = value1 & param2 = value2...**

Pour obtenir la liste complète des paramètres disponibles, voir les références des paramètres [bingmaps:](#bingmaps-param-reference), [ms-drive-to:](#ms-drive-to-param-reference) et [ms-walk-to:](#ms-walk-to-param-reference). Des exemples sont également fournis plus loin dans cette rubrique.

## <a name="launch-a-uri-from-your-app"></a>Lancer un URI à partir de votre application


Pour lancer l’application Windows Maps à partir de votre application, appelez la méthode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) avec un **BingMaps :** , **MS-Drive-to :** ou **MS-Walk-to :** URI. L’exemple suivant lance le même URI à partir de l’exemple précédent. Pour plus d’informations sur le lancement d’une application via un URI, voir [Lancer l’application par défaut pour un URI](launch-default-app.md).

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

Dans cet exemple, la classe [**LauncherOptions**](https://docs.microsoft.com/uwp/api/Windows.System.LauncherOptions) est utilisée pour s’assurer du lancement de l’application Cartes Windows.

## <a name="display-known-locations"></a>Afficher des emplacements connus

Il existe de nombreuses options permettant de contrôler la partie de la carte à afficher. Vous pouvez utiliser le paramètre *cp* (point central) avec les paramètres *rad* (rayon) ou *lvl* (niveau de zoom) pour afficher un emplacement et choisir comment zoomer dessus. Lorsque vous utilisez le paramètre *cp*, vous pouvez également définir un paramètre *hdg* (en-tête) et *pit* (inclinaison) pour indiquer dans quelle direction effectuer la recherche. Une autre méthode consiste à utiliser le paramètre *bb* (zone de délimitation) pour indiquer le maximum de coordonnées (sud, est, nord et ouest) de la zone que vous souhaitez afficher.

Pour contrôler le type d’affichage, utilisez les paramètres *sty* (style) et *ss* (Streetside). Le paramètre *sty* vous permet de basculer entre les vues aérienne et routière. Le paramètre *ss* place la carte dans une vue Streetside. Pour plus d’informations sur ces paramètres et d’autres, voir la [référence de paramètre bingmaps:](#bingmaps-param-reference).


| Exemple d’URI                                                                 | Résultats                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | Ouvre l’application Cartes.                                                                                                                                                                            |
| bingmaps:?cp=40.726966~-74.006076                                          | Affiche une carte centrée sur la ville de New York.                                                                                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=10                                   | Affiche une carte centrée sur la ville de New York avec le niveau de zoom 10.                                                                                                                            |
| BingMaps :? BB = 39.719\_-74.52 ~ 41.71\_-73,5                                   | Affiche une carte de la ville de New York, qui est la zone spécifiée dans l’argument **bb**.                                                                                                           |
| BingMaps :? BB = 39.719\_-74.52 ~ 41.71\_-73.5 & CP = 47 ~-122                        | Affiche une carte de la ville de New York, qui est la zone spécifiée dans l’argument du cadre englobant. Le point central pour Seattle spécifié dans l’argument **cp** est ignoré parce que *bb* est spécifié. |
| BingMaps :? collection = point. 36.116584\_-115,176753\_Caesars% 20Palace & NIV = 16 | Affiche une carte avec un point nommé Caesars Palace (à Las Vegas) et affecte la valeur 16 au niveau de zoom.                                                                                                 |
| BingMaps :? collection = point. 40.726966\_-74,006076\_% 255FBusiness        | Affiche une carte avec un point nommé\_une entreprise (à Las Vegas).                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&amp;trfc=1&amp;sty=a                             | Affiche une carte de la ville de New York avec le trafic sur une vue de carte aérienne.                                                                                                                          |
| bingmaps:?cp=47.6204~-122.3491&amp;sty=3d                                      | Affiche une vue 3D de la Space Needle.                                                                                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&amp;sty=3d&amp;rad=200&amp;pit=75&amp;hdg=165               | Affiche une vue 3D de la Space Needle avec un rayon de 200 m, une inclinaison de 75 degrés et une orientation de 165 degrés.                                                                             |
| bingmaps:?cp=47.6204~-122.3491&amp;ss=1                                        | Affiche une vue Streetside de la Space Needle.                                                                                                                                                |


## <a name="display-search-results"></a>Afficher les résultats de la recherche

Lorsque vous recherchez des lieux à l’aide du paramètre *q*, nous recommandons de définir des termes aussi spécifiques que possible et ce à l’aide des paramètres *cp*, *bb* ou *where* pour spécifier un emplacement de recherche. Si vous ne spécifiez aucun emplacement de recherche et si l’emplacement actuel de l’utilisateur n’est pas disponible, la recherche peut ne pas renvoyer de résultats pertinents. Les résultats de recherche sont affichés dans la vue de carte la plus appropriée. Pour plus d’informations sur ces paramètres et d’autres, voir la [référence de paramètre bingmaps:](#bingmaps-param-reference).


| Exemple d’URI                                                    | Résultats                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:?q=1600%20Pennsylvania%20Ave,%20Washington,%20DC     | Affiche une carte et recherche l’adresse de la Maison Blanche à Washington. |
| bingmaps:?q=coffee&amp;where=Seattle                              | Recherche un café à Seattle.                                                    |
| bingmaps:?cp=40.726966~-74.006076&where=New%20York            | Recherche New York près du point central indiqué.                             |
| BingMaps :? BB = 39.719\_-74.52 ~ 41.71\_-73.5 & q = pizza              | Recherche une pizzeria dans la zone de délimitation (autrement dit, à New York).      |

 
## <a name="display-multiple-points"></a>Afficher plusieurs points


Utilisez le paramètre *collection* pour afficher un ensemble personnalisé de points sur la carte. S’il existe plusieurs points, une liste de points s’affiche. Une collection peut inclure jusqu’à 25 points, qui sont répertoriés dans l’ordre indiqué. La collection est prioritaire sur les demandes d’itinéraires et de recherche. Pour plus d’informations sur ce paramètre et d’autres, voir la [référence de paramètre bingmaps:](#bingmaps-param-reference).

| Exemple d’URI | Résultats                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| BingMaps :? collection = point. 36.116584\_-115,176753\_Caesars% 20Palace                                                                                                | Recherche Caesars Palace à Las Vegas et affiche les résultats sur une carte dans la meilleure vue de carte.                         |
| BingMaps :? collection = point. 36.116584\_-115,176753\_Caesars% 20Palace & NIV = 16                                                                                         | Affiche une punaise nommée « Caesars Palace à Las Vegas », avec un niveau de zoom de 16.                                               |
| BingMaps :? collection = point. 36.116584\_-115,176753\_Caesars% 20Palace ~ point. 36.113126\_-115,175188\_% 20Bellagio & NIV = 16 & CP = 36.114902 ~-115,176669                   | Affiche une punaise nommée « Caesars Palace à Las Vegas » et une autre appelée « Hôtel Bellagio à Las Vegas », avec un niveau de zoom de 16.              |
| BingMaps :? collection = point. 40.726966\_-74,006076\_factice% 255FBusiness% 255Fwith% 255FUnderscore                                                                        | Affiche New York avec un clic-infos\_nommé\_«\_fausses affaires » avec un trait de soulignement.                                                  |
| BingMaps :? collection = nom. Hôtel% 20List ~ point. 36.116584\_-115,176753\_Caesars% 20Palace ~ point. 36.113126\_-115,175188\_% 20Bellagio & NIV = 16 & CP = 36.114902 ~-115,176669 | Affiche une liste nommée « Liste d’hôtels » et deux punaises correspondant aux hôtels Caesars Palace et Bellagio à Las Vegas, avec un niveau de zoom de 16. |

 

## <a name="display-directions-and-traffic"></a>Afficher un itinéraire et le trafic


Vous pouvez afficher un itinéraire entre deux points à l’aide du paramètre *rtp*. Ces points peuvent être des adresses ou des coordonnées en latitude et longitude. Utilisez le paramètre *trfc* pour afficher des informations sur le trafic. Pour spécifier le type d’itinéraire (en voiture, à pied ou en transport public), utilisez le paramètre *mode*. Si le paramètre *mode* n’est pas spécifié, un itinéraire est fourni sur la base du mode de transport préféré de l’utilisateur. Pour plus d’informations sur ces paramètres et d’autres, voir la [référence de paramètre bingmaps:](#bingmaps-param-reference).

![exemple d’itinéraire](images/windowsmapgcdirections.png)

| Exemple d’URI                                                                                                              | Résultats                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BingMaps :? RTP = pos. 44.9160\_-110.4158 ~ pos. 45.0475\_-109,4187                                                             | Affiche une carte avec un itinéraire de point à point. Le paramètre *mode* n’étant pas spécifié, un itinéraire est fourni sur la base du mode de transport préféré de l’utilisateur. |
| bingmaps:?cp=43.0332~-87.9167&amp;trfc=1                                                                                    | Affiche une carte centrée sur Milwaukee, Wisconsin, avec le trafic.                                                                                                        |
| BingMaps :? RTP = ADR. One Microsoft Way, Redmond, WA 98052 ~ pos. 39.0731\_-108,7238                                           | Affiche une carte avec un itinéraire de l’adresse spécifiée à l’emplacement indiqué.                                                                            |
| BingMaps :? RTP = ADR. 1% 20Microsoft% 20Way,% 20Redmond,% 20WA,% 2098052 ~ pos. 36.1223\_-111,9495\_grand% 20Canyon% 20northern% 20rim | Affiche un itinéraire de 1 Microsoft Way, Redmond, WA, 98052, au rebord nord du Grand Canyon.                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | Affiche une carte avec un itinéraire en voiture de l’emplacement indiqué au point de repère spécifié.                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=d                      | Affiche un itinéraire en voiture de Mountain View à l’aéroport international de San Francisco en Californie.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=w                      | Affiche un itinéraire à pied de Mountain View à l’aéroport international de San Francisco en Californie.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=t                      | Affiche un itinéraire de transit de Mountain View à l’aéroport international de San Francisco en Californie.                                                                  |

## <a name="display-turn-by-turn-directions"></a>Afficher un itinéraire détaillé


**MS-Drive-to :** et **MS-Walk-to :** Les schémas d’URI vous permettent de lancer directement une vue tour à tour d’un itinéraire. Ces schémas d’URI peuvent uniquement fournir un itinéraire à partir de la localisation actuelle de l’utilisateur. Si vous devez fournir des instructions entre des points qui n’incluent pas l’emplacement actuel de l’utilisateur, utilisez **BingMaps :** Schéma d’URI comme décrit dans la section précédente. Pour plus d’informations sur ces schémas d’URI, voir les références de paramètres [ms-drive-to:](#ms-drive-to-param-reference) et [ms-walk-to:](#ms-walk-to-param-reference).

> **Important**  Quand le **lecteur ms-to :** ou **MS-Walk-to :** Les schémas d’URI sont lancés, l’application Maps vérifie si l’appareil a déjà subi un correctif de l’emplacement GPS. Si tel est le cas, l’application Cartes continue de fournir un itinéraire détaillé. Dans le cas contraire, l’application affiche la vue d’ensemble de l’itinéraire, comme décrit dans la section [Afficher un itinéraire et le trafic](#display-directions-and-traffic).

![exemple d’itinéraire détaillé](images/windowsmapsappdirections.png)

| Exemple d’URI                                                                                                | Résultats                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake | Affiche un itinéraire étape par étape en voiture de votre emplacement actuel à Green Lake. |
| ms-walk-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake  | Affiche un itinéraire étape par étape à pied de votre emplacement actuel à Green Lake. |


## <a name="download-offline-maps"></a>Télécharger des cartes hors connexion

**MS-Settings :** Le modèle d’URI vous permet de lancer directement dans une page particulière de l’application paramètres. Pendant la **Configuration MS-Settings :** Le schéma d’URI ne démarre pas dans l’application Maps, il vous permet de lancer directement sur la page Maps hors connexion dans l’application paramètres et affiche une boîte de dialogue de confirmation pour télécharger les cartes hors connexion utilisées par l’application Maps. Le schéma d’URI accepte un point spécifié par une latitude et une longitude et détermine automatiquement si des cartes hors connexion sont disponibles pour une région contenant ce point.  Si la latitude et la longitude transmises appartiennent à plusieurs régions de téléchargement, la boîte de dialogue de confirmation permet à l’utilisateur de sélectionner la région à télécharger. Si des cartes hors connexion ne sont pas disponibles pour une région contenant ce point, la page Cartes hors connexion de l’application Paramètres s’affiche avec une boîte de dialogue d’erreur.

| Exemple d’URI  | Résultats |
|-------------|---------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | Ouvre l’application Paramètres au niveau de la page Cartes hors connexion en affichant une boîte de dialogue de confirmation pour vous permettre de télécharger des cartes pour la région contenant le point de latitude et de longitude spécifié. |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>Référence de paramètre bingmaps:

Vous pouvez afficher la syntaxe de chaque paramètre de ce tableau à l’aide de l’extension ABNF (Augmented Backus–Naur Form).

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Définition</th>
<th align="left">Exemple et définition de l’extension ABNF</th>
<th align="left">Détails</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><b>cp</b></p></td>
<td align="left"><p>Point central</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3DIGIT ["." 1*7DIGIT]</p>
<p>degreeslon = ["-"] 1*2DIGIT ["." 1*7DIGIT]</p>
<p>Exemple :</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>Ces deux valeurs doivent être exprimées en degrés et séparées par un tilde (<b>~</b>).</p>
<p>Les valeurs de longitude valides sont comprises entre -180 et +180 (ces deux valeurs étant incluses).</p>
<p>Les valeurs de latitude valables sont comprises entre -90 et +90 (ces deux valeurs étant incluses).</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>BB</b></p></td>
<td align="left"><p>Cadre englobant</p></td>
<td align="left"><p>bb = "bb=" southlatitude " _" westlongitude "~" northlatitude "_ " eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>Exemple :</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>Zone rectangulaire qui spécifie le cadre englobant exprimé en degrés, en utilisant un tilde (<b>~</b>) pour séparer l’angle inférieur gauche de l’angle supérieur droit. Les valeurs de latitude et de longitude de chaque point sont séparées par un trait de soulignement (<b>_</b>).</p>
<p>Les valeurs de longitude valides sont comprises entre -180 et +180 (ces deux valeurs étant incluses).</p>
<p>Les valeurs de latitude valables sont comprises entre -90 et +90 (ces deux valeurs étant incluses).</p><p>Les paramètres cp et lvl sont ignorés lorsqu’un cadre englobant est fourni.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>where</b></p></td>
<td align="left"><p>Location</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1 *( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "* " / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>Exemple :</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>Terme de recherche correspondant à un emplacement, un élément géographique ou un lieu spécifiques.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>question</b></p></td>
<td align="left"><p>Terme de requête</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>Exemple :</p>
<p>q=restaurants%20mexicains</p></td>
<td align="left"><p>Terme de recherche pour les entreprises locales ou la catégorie des entreprises.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mono</b></p></td>
<td align="left"><p>Niveau de zoom</p></td>
<td align="left"><p>NIV = "NIV =" 1<i>2DIGIT ["." 1</i>2DIGIT]</p>
<p>Exemple :</p>
<p>lvl=10.50</p></td>
<td align="left"><p>Définit le niveau de zoom de la vue de carte. Les valeurs valables sont incluses entre 1-20, la valeur 1 correspondant au zoom maximal.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>sty</b></p></td>
<td align="left"><p>Style</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>Exemple :</p>
<p>sty=a</p></td>
<td align="left"><p>Définit le style de carte. Les valeurs valables pour ce paramètre sont les suivantes :</p>
<ul>
<li><b>r</b>: Affichez une vue aérienne de la carte.</li>
<li><b>r</b>: Affichez une vue routière de la carte.</li>
<li><b>3D</b>: Affichez une vue 3D de la carte. Utilisez cette valeur conjointement avec le paramètre <b>cp</b> et éventuellement avec le paramètre <b>rad</b>.</li>
</ul>
<p>Dans Windows 10, les styles vue aérienne et affichage 3D sont identiques.</p>
<div class="alert">
<b>Remarque</b>l'  omission du paramètre <b>sty</b> produit les mêmes résultats que sty = r.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>rad</b></p></td>
<td align="left"><p>Rayon</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>Exemple :</p>
<p>rad=1000</p></td>
<td align="left"><p>Zone circulaire qui spécifie la vue de carte souhaitée. La valeur du rayon se mesure en mètres.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>compose</b></p></td>
<td align="left"><p>Inclinaison</p></td>
<td align="left"><p>pit = "pit=" pitch (inclinaison)</p>
<p>Exemple :</p>
<p>pit=60</p></td>
<td align="left"><p>Indique l’angle sous lequel la carte est visualisée, où 90 revient à regarder l’horizon (inclinaison maximale) et 0 à regarder le sol verticalement (inclinaison minimale).</p><p>Les valeurs d’inclinaison valables sont comprises entre 0 et 90 (ces deux valeurs étant incluses).</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>Orientation</p></td>
<td align="left"><p>hdg = "hdg=" heading (orientation)</p>
<p>Exemple :</p>
<p>hdg=180</p></td>
<td align="left"><p>Indique l’orientation (ou cap) de la carte exprimée en degrés, où 0 ou 360 = Nord, 90 = Est, 180 = Sud et 270 = Ouest.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>sécurité</b></p></td>
<td align="left"><p>Streetside</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>Exemple :</p>
<p>ss=1</p></td>
<td align="left"><p>Spécifie l’affichage des images au niveau de la rue quand <code>ss=1</code>. Si vous omettez le paramètre <b>ss</b>, vous obtenez le même résultat qu’avec la commande <code>ss=0</code>. Utilisez ce paramètre conjointement avec le paramètre <b>cp</b> pour spécifier l’emplacement de la vue au niveau de la rue.</p>
<div class="alert">
<b></b>Remarque  : l’image au niveau de la rue n’est pas disponible dans toutes les régions.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>trfc</b></p></td>
<td align="left"><p>Trafic</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>Exemple :</p>
<p>trfc=1</p></td>
<td align="left"><p>Spécifie si les informations sur le trafic sont incluses sur la carte. Si vous omettez le paramètre trfc, vous obtenez le même résultat qu’avec la commande <code>trfc=0</code>.</p>
<div class="alert">
<b></b>Notez  que les données de trafic ne sont pas disponibles dans toutes les régions.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>RTP</b></p></td>
<td align="left"><p>Itinéraire</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>


<p>Exemples :</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>Définit les valeurs de début et de fin d’un itinéraire à dessiner sur la carte, séparées par un tilde (<b>~</b>). Chacun des points de navigation est défini par une position, reposant sur une latitude et une longitude, ou par un identificateur d’adresse.</p>
<p>Un itinéraire complet contient exactement deux points de navigation. Par exemple, un itinéraire avec deux points de navigation est défini par <code>rtp="A"~"B"</code>.</p>
<p>Il est également acceptable de spécifier un itinéraire incomplet. Ainsi, vous pouvez définir uniquement le début d’un itinéraire avec <code>rtp="A"~</code>. Dans ce cas, le panneau de saisie d’itinéraire s’affiche avec le point de navigation fourni inséré dans le champ <b>De</b>, et le focus positionné sur le champ <b>À</b>.</p>
<p>Si seule la fin d’un itinéraire est spécifiée, comme avec <code>rtp=~"B"</code>, le panneau d’itinéraire affiche le point de navigation fourni dans le champ <b>À</b>. Si la localisation actuelle est disponible, elle figure dans le champ <b>De</b> sur lequel le focus est positionné.</p>
<p>Aucune ligne d’itinéraire n’est dessinée lorsque l’itinéraire fourni est incomplet.</p>
<p>Utilisez ces paramètres conjointement avec le paramètre <b>mode</b> servant à spécifier le mode de transport (en voiture, en transport public ou à pied). Si le paramètre <b>mode</b> n’est pas spécifié, un itinéraire est fourni sur la base du mode de transport préféré de l’utilisateur.</p>
<div class="alert">
<b>Remarque</b>un  titre peut être utilisé pour un emplacement si l’emplacement est spécifié par la valeur du paramètre <b>pos</b> . Le titre s’affichera à la place de la latitude et de la longitude.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mode</b></p></td>
<td align="left"><p>Mode de transport</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>Exemple :</p>
<p>mode=d</p></td>
<td align="left"><p>Définit le mode de transport. Les valeurs valables pour ce paramètre sont les suivantes :</p>
<ul>
<li><b>d</b>: Affiche la vue d’ensemble de l’itinéraire pour les directions routières</li>
<li><b>t</b>: Affiche la vue d’ensemble de l’itinéraire pour les directions de transit</li>
<li><b>w</b>: Affiche la vue d’ensemble de l’itinéraire pour les directions de marche</li>
</ul>
<p>Utilisez ce paramètre conjointement avec le paramètre <b>rtp</b> pour la définition de l’itinéraire de transport. Si le paramètre <b>mode</b> n’est pas spécifié, un itinéraire est fourni sur la base du mode de transport préféré de l’utilisateur. Il est possible d’indiquer un paramètre <b>mode</b> sans aucun paramètre d’itinéraire afin de fournir une saisie d’itinéraire pour ce mode à partir de la localisation actuelle.</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>collection</b></p></td>
<td align="left"><p>Collection</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>Exemple :</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>Collection de points à ajouter à la carte et à la liste. Il est possible de nommer la collection de points en utilisant le paramètre name. Un point est spécifié à l’aide d’une latitude, d’une longitude et d’un titre facultatif.</p>
<p>Le nom doit être séparé des différents points par un caractère tilde (<b>~</b>).</p>
<p>Si l’élément spécifié contient un tilde, assurez-vous qu’il est codé comme suit : <code>%7E</code>. En l’absence de paramètres de niveau de zoom et de point central, la collection propose la meilleure vue de carte possible.</p>

<p><b>Important</b> Si l’élément que vous spécifiez contient un trait de soulignement, assurez-vous qu’il est double et encodé comme suit : %255F.</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>Référence de paramètre ms-drive-to:


L’URI permettant de lancer une demande d’itinéraire détaillé en voiture n’a pas besoin d’encodage et présente le format suivant.

> **Remarque**  Dans ce schéma d’URI, vous ne spécifiez pas le point de départ. Le point de départ est toujours supposé être la localisation actuelle. Si vous devez spécifier un point de départ différent de l’emplacement actuel, voir [Afficher un itinéraire et le trafic](#display-directions-and-traffic).

 

| Paramètre | Définition | Exemple | Détails |
|------------|-----------|---------|---------|
| **destination. Latitude** | Latitude de destination | Exemple : destination.latitude=47.6451413797194 | Latitude de la destination. Les valeurs de latitude valables sont comprises entre -90 et +90 (ces deux valeurs étant incluses). |
| **destination. Longitude** | Longitude de destination | Exemple : destination.longitude=-122.141964733601 | Longitude de la destination. Les valeurs de longitude valides sont comprises entre -180 et +180 (ces deux valeurs étant incluses). |
| **destination.name** | Nom de la destination | Exemple : destination.name=Redmond, WA | Nom de la destination. Vous n’avez pas besoin d’encoder la valeur **destination.name**. |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>Référence de paramètre ms-walk-to:


L’URI permettant de lancer une demande d’itinéraire détaillé à pied n’a pas besoin d’encodage et présente le format suivant.

> **Remarque**  Dans ce schéma d’URI, vous ne spécifiez pas le point de départ. Le point de départ est toujours supposé être la localisation actuelle. Si vous devez spécifier un point de départ différent de l’emplacement actuel, voir [Afficher un itinéraire et le trafic](#display-directions-and-traffic).
 

| Paramètre | Définition | Exemple | Détails |
|-----------|------------|---------|----------|
| **destination. Latitude** | Latitude de destination | Exemple : destination.latitude=47.6451413797194 | Latitude de la destination. Les valeurs de latitude valables sont comprises entre -90 et +90 (ces deux valeurs étant incluses). |
| **destination. Longitude** | Longitude de destination | Exemple : destination.longitude=-122.141964733601 | Longitude de la destination. Les valeurs de longitude valides sont comprises entre -180 et +180 (ces deux valeurs étant incluses). |
| **destination.name** | Nom de la destination | Exemple : destination.name=Redmond, WA | Nom de la destination. Vous n’avez pas besoin d’encoder la valeur **destination.name**. |

## <a name="ms-settings-parameter-reference"></a>Référence de paramètre ms-settings:

La syntaxe pour mappe les paramètres spécifiques à l’application pour **MS-Settings :** Le schéma d’URI est défini ci-dessous. **Maps-downloadmaps** est spécifié avec les **Paramètres MS-Settings :** Sous la forme **MS-Settings : Maps-downloadmaps ?** pour indiquer la page de paramètres cartes hors connexion. 

| Paramètre | Définition | Exemple | Détails |
|-----------|------------|---------|----------|
| **LatLong** | Point définissant une région de carte hors connexion. | Exemple : latlong=47.6,-122.3 | Le point géographique est spécifié par une latitude et une longitude séparées par une virgule. Les valeurs de latitude valables sont comprises entre -90 et +90 (ces deux valeurs étant incluses). Les valeurs de longitude valides sont comprises entre -180 et +180 (ces deux valeurs étant incluses). |
