---
author: normesta
description: Entrées et propriétés d’une feuille de style de carte
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Référence de feuille de style de carte
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, cartes, feuille de style de carte
ms.localizationpriority: medium
ms.openlocfilehash: 984741de5be585f7d6d726ec4c736e6ebce78830
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862086"
---
# <a name="map-style-sheet-reference"></a>Référence de feuille de style de carte

Utilisation des technologies de mappage Microsoft mapper des feuilles de style pour définir l’apparence de cartes.  Une feuille de style de carte est définie à l’aide de la Notation JSON (JavaScript Object) et peut être utilisée dans différentes manières, y compris dans d’une application Windows Store [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) par le biais de la méthode [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Par exemple, le code JSON ci-après vous permet de faire apparaître les plans d’eau en rouge, les légendes des plans d’eau en vert et les zones continentales en bleu:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```
Vous pouvez également utiliser un code JSON pour supprimer la totalité des légendes et des objets ponctuels d’une carte.

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

Parfois, la valeur d’une propriété est transformée pour produire le résultat final.  Par exemple, végétation fillColor a des nuances légèrement différentes selon le type de l’entité affichée.  Ce comportement peut être désactivé, ce qui permet d'utiliser la valeur précise fournie, à l’aide de la propriété ignoreTransform.

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

Cet article indique les entrées et [propriétés](#properties) JSON que vous pouvez utiliser pour personnaliser l’apparence de vos cartes.

<a id="entries" />

## <a name="entries"></a>Entrées
Ce tableau utilise des caractères «>» pour représenter les différents niveaux de la hiérarchie des entrées.  Il indique également les versions de Windows prend en charge de chaque entrée et qui l’ignorer.

| Build | Nom de version de Windows |
|-------|----------------------|
| 1506  | CreatorsUpdate      |
| 1629  | FallCreatorsUpdate |
| 1713  | Mise à jour d’avril2018    |

| Nom                         | Groupe de propriétés            | 1506 | 1629 | 1713 | Next | Description    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | Version de feuille de style que vous souhaitez utiliser. |
| settings                     | [Settings](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | Paramètres qui s’appliquent à la totalité de la feuille de style. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées de la carte. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées non utilisateur. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones décrivant le terrain utilisent.  Il ne doivent ne pas pour être confondu avec les bâtiments physiques qui sont sous l’entrée de structure. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les aéroports. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones dans lesquelles il y a une forte concentration d'entreprises ou de points intéressants. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent CIMETIERES. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone continent. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones incluant des écoles et autres ressources de formation. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Domaines relatifs aux personnes autochtones se réserve. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones qui sont utilisés à des fins industrielles. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone (îles). |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui sont utilisées à des fins médicales (par exemple: un site hospitalier). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les bases militaires ou ont des utilisations militaires. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui sont utilisées pour nautique connexes. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone de cercle. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui est utilisé comme une piste avion. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones sablonneuses, telles que des plages. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Surfaces au sol allouées à des galeries marchandes ou à d’autres centres commerciaux. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Domaines relatifs aux stades. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones souterraines (par exemple: emplacement d'une station de métro). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Forêts, zones herbeuses, etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Terres forestières. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Domaines relatifs aux golfs. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent parcs. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Terrains de sport, tels qu’un terrain de base-ball ou un court de tennis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Domaines relatifs aux réserves. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Point de toutes les fonctionnalités sont dessinées avec une icône quelconque. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Étiquettes de nombres d’adresse. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent les éléments naturels. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des pics montagneux. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sommets de volcan. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des points d’eau, tels qu’une cascade. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent n’importe quel emplacement intéressant. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent les locaiton business. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les sites touristiques musées, zoos, etc.. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les emplacements d’utilisation générale à la Communauté. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Les icônes qui représentent des écoles et autres études associées emplacements. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les salles de divertissement telles que des salles, cinémas, etc.. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des services essentiels tels que stationnement, banques, gaz, etc.. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent les restaurants, cafés, etc.. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les hôtels et autres entreprises de dépôt. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des entreprises en matière d’immobilier. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les hôtels et autres entreprises de dépôt. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la superficie d’une agglomération (par exemple, une ville ou un village). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’une agglomération. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’un département ou d’une province. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la capitale d’un pays ou d’une région. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Caractères représentant le nom abrégé d’une route. (Par exemple: A6). Si vous définissez la propriété **ImageFamily** de l’entrée de paramètres sur la valeur *Palette*, utilisez uniquement des valeurs de palette. |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sorties, appartenant généralement à un réseau autoroutier. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des arrêts d’autobus, des gares, des aéroports, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones administratives telles que les pays, les régions et les départements. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bordures de région pays et étiquettes. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, États, provinces, etc., les bordures et étiquettes. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, les départements, etc., les bordures et étiquettes. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments et autres constructions. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour l’éducation. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés à des fins médicales telles que hôpitaux. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour le transit tels que les aéroports. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies appartenant au réseau de transport (par exemple, routes, lignes de chemin de fer et lignes de ferry). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant toutes les routes. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les bus access volumineux, contrôlé. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les rampes haut débit qui se connectent généralement à contrôlés bus access. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les bus. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les routes principales. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les routes secondaires. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les rampes généralement se connectent à bus. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes unpaved. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les routes qui coûtent de l’argent à utiliser. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies ferrées. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Sentiers de promenade au sein des parcs ou sentiers de randonnée pédestre. |
| >>> passerelle                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Passerelle avec élévation de privilèges. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes de ferry. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Tout type de plan d’eau. Comprend notamment les océans et les ruisseaux. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rivières, ruisseaux ou autres passages d’eau.  Notez que cet élément peut prendre la forme d’une ligne ou d’un polygone et qu’il peut être relié à des plans d’eau autres que des rivières. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les entrées associées de routage. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Router ligne entrées liées. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les itinéraires d’origine. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Lignes qui représentent les itinéraires conduites panorama. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent le parcours des itinéraires. |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les entrées de l’utilisateur. |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Style des instances [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) par défaut. |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Style des instances [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) par défaut. |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | Style des instances [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) par défaut.  Cela permet principalement de configurer renderAsSurface. |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Style des instances [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) par défaut. |

<a id="properties" />

## <a name="properties"></a>Propriétés

Cette section décrit les propriétés que vous pouvez utiliser pour chaque entrée.

<a id="version" />

### <a name="version-properties"></a>Propriétés de version

| Propriété                     | Type    | Description                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | Chaîne  | Version de feuille de style ciblée. Utilisée à des fins d’applicabilité. «1.0» par défaut, «1.*» pour les mises à jour de fonctionnalités mineures supplémentaires. |

<a id="settings" />

### <a name="settings-properties"></a>Propriétés de paramètres

| Propriété                     | Type    | 1506 | 1629 | 1713 | Next | Description |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si l’atmosphère apparaît dans le contrôle3D. |
| buildingTexturesVisible      | Booléen    |      |      |  ✔   |  ✔   | Indicateur indiquant s’il faut ou non afficher des textures sur des bâtiments symboliques en 3D qui ont des textures. |
| fogColor                     | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB du brouillard de distance qui apparaît dans le contrôle3D. |
| glowColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB pouvant être appliquée à l’éclat des légendes et des icônes. |
| imageFamily                  | Chaîne  |  ✔   |  ✔   |  ✔   |  ✔   | Nom de l’ensemble d’images à utiliser pour ce style. Définissez cette propriété sur la valeur *Default* pour les signes qui utilisent des couleurs fixes basées sur le signe réel. Définissez cette propriété sur la valeur *Palette* pour les signes qui utilisent des couleurs de palette configurables. |
| landColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB d’une zone continentale avant qu’un élément quelconque soit dessiné sur cette dernière. |
| logosVisible                 | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété **Organization** doivent dessiner les logos appropriés ou utiliser une icône générique. |
| officialColorVisible         | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété de couleur officielle (telle que celle des lignes de transport en commun en Chine) doivent dessiner cette couleur. Par exemple, désactivez cette valeur dans le cas d’une carte en noir et blanc. |
| rasterRegionsVisible         | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur qui indique s’il faut dessiner trame régions où ils ont une meilleure représentation à vecteurs (Japon et Corée). |
| shadedReliefVisible          | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant s’il convient ou non de dessiner l’ombrage des hauteurs sur la carte. |
| shadedReliefDarkColor        | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté foncé d’un relief par ombres portées.  Canal alpha représente la valeur alpha maximale. |
| shadedReliefLightColor       | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté clair d’un relief par ombres portées.  Canal alpha représente la valeur alpha maximale. |
| shadowColor                  | Couleur   |      |      |      |  ✔️   | La couleur de l’ombre derrière les icônes qui utilisent des ombres. |
| spaceColor                   | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB de la zone entourant la carte. |
| useDefaultImageColors        | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur qui indique si les couleurs d’origine dans le SVG doivent être utilisé, plutôt que recherche de l’entrée de la palette de couleurs d’une image. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriétés MapElement

| Propriété                     | Type    | 1506 | 1629 | 1713 | Next | Description |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de l’élément en arrière-plan d’une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| fillColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur utilisée pour le remplissage des polygones, pour l’arrière-plan des icônes d’objet ponctuel et pour le centre des lignes si elles sont fractionnées. |
| fontFamily                   | Chaîne  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du glyphe affiché au milieu d’une icône d’objet ponctuel. |
| iconScale                    | Flottant   |      |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du glyphe d’une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| labelColor                   | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle des tailles de légende par défaut. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| labelVisible                 | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Fait en sorte que la valeur alpha de l’élément **FillColor** remplace l’élément **StrokeColor** au lieu de fusionner avec ce dernier. |
| scale                        | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la taille de l’objet ponctuel entier. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| strokeColor                  | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur à utiliser pour le contour des polygones, pour le contour des icônes d’objet ponctuel et pour la couleur des lignes. |
| strokeWidthScale             | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du trait des lignes. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| visible                      | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1506 | 1629 | 1713 | Next | Description |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne secondaire ou de contour de la bordure d’un polygone rempli. |
| borderStrokeColor            | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne principale de la bordure d’un polygone rempli. |
| borderVisible                | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Le montant par lequel le trait des bordures sont mis à l’échelle. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriétés PointStyle

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1506 | 1629 | 1713 | Next | Description |
|------------------------------|---------|------|------|------|------|-------------|
| Arrière-plan de la forme             | Flottant   |      |      |      |  ✔️   | Forme à utiliser comme arrière-plan de l’icône--en remplaçant toutes les formes qui existe. |
| stemAnchorRadiusScale        | Flottant   |      |      |  ✔   |  ✔   | Valeur de mise à l’échelle du point d'ancrage du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| stemColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de la ligne verticale sortant du bas de l’icône en mode3D. |
| stemHeightScale              | Flottant   |      |      |  ✔   |  ✔   | Valeur de mise à l’échelle de la longueur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la largeur par défaut et la valeur *2* pour obtenir une largeur deux fois plus grande. |
| stemOutlineColor             | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du contour de la ligne verticale sortant du bas de l’icône en mode3D. |
| stemWidthScale               | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la largeur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la largeur par défaut et la valeur *2* pour obtenir une largeur deux fois plus grande. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1506 | 1629 | 1713 | Next | Description |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Booléen    |      |      |  ✔   |  ✔   | Indicateur indiquant qu’un modèle 3D doit être affiché comme un bâtiment, sans atténuation de profondeur par rapport au sol. |
