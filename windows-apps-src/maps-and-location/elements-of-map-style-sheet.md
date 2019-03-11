---
description: Entrées et propriétés d’une feuille de style de carte
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Référence de feuille de style de carte
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, cartes, feuille de style de carte
ms.localizationpriority: medium
ms.openlocfilehash: f199e08f74ace4e6c8b123a701af19469b029aed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608414"
---
# <a name="map-style-sheet-reference"></a>Référence de feuille de style de carte

Utilisent des technologies de mappage Microsoft _mapper des feuilles de style_ pour définir l’apparence des mappages.  Une feuille de style de mappage est définie à l’aide de JavaScript Objet Notation (JSON) et peut être utilisée de différentes façons, par exemple dans une application Windows Store [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) via la [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) méthode.

Feuilles de style peuvent être créés à l’aide de manière interactive la [éditeur de feuilles de Style de carte](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) application.

Le code JSON suivant peut être utilisé pour rendre des zones de l’eau apparaissent en rouge, les étiquettes de l’eau apparaissent en vert et terrestre apparaître en bleu :

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Ce JSON peut être utilisé pour supprimer toutes les étiquettes et les points d’une carte.

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

Cet article indique les entrées et [propriétés](#properties) JSON que vous pouvez utiliser pour personnaliser l’apparence de vos cartes.  Ces propriétés peuvent également être appliquées aux éléments de mappage utilisateur via le [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) propriété.

<a id="entries" />

## <a name="entries"></a>Entrées
Ce tableau utilise des caractères « > » pour représenter les différents niveaux de la hiérarchie des entrées.  Il montre également les versions de Windows prend en charge chaque entrée et qui l’ignorer.

| Version | Nom de la version Windows |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | Mise à jour d’avril 2018    |
|  1809   | Mise à jour d’octobre 2018  |

| Nom                         | Groupe de propriétés            | 1703 | 1709 | 1803 | 1809 | Description    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | Version de feuille de style que vous souhaitez utiliser. |
| paramètres                     | [Paramètres](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | Paramètres qui s’appliquent à la totalité de la feuille de style. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées de la carte. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées non utilisateur. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones décrivant land utilisent.  Il ne doivent ne pas pour être confondue avec les bâtiments physiques qui sont sous l’entrée de la structure. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les aéroports. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones dans lesquelles il y a une forte concentration d'entreprises ou de points intéressants. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent CIMETIERES. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone continent. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les écoles et les autres installations d’enseignement. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui comprennent les réserves de ressources autochtones. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones qui sont utilisés à des fins industrielles. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone (île). |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Les zones qui sont utilisés à des fins médicales (par exemple : un campus hôpital). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les bases militaires ou ont des utilisations militaires. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui sont utilisés à des fins de connexes nautiques. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone de voisinage. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui est utilisé comme une piste d’avion. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones sablonneuses, telles que des plages. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Surfaces au sol allouées à des galeries marchandes ou à d’autres centres commerciaux. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les stades. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones souterraines (par exemple : emplacement d'une station de métro). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Forêts, zones herbeuses, etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Terres forestières. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent le parcours de golf. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les parcs. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Terrains de sport, tels qu’un terrain de base-ball ou un court de tennis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui comprennent les réserves de ressources de nature. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les fonctionnalités de point qui sont dessinées avec une icône quelconque. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Adresse numéros des étiquettes. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent des fonctionnalités naturelles. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des pics montagneux. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sommets de volcan. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des points d’eau, tels qu’une cascade. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent n’importe quel emplacement intéressant. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent n’importe quel emplacement de l’entreprise. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des attractions touristes musées, zoos, etc. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des emplacements de l’utilisation générale à la Communauté. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Les icônes qui représentent les écoles et les autres éducation liés emplacements. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les lieux de divertissement comme théâtres, cinémas, etc. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des services essentiels tels que stationnement, les banques, les hydrocarbures, etc. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes qui représentent des restaurants, des cafés, etc. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des hôtels et autres entreprises de dépôt. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent les entreprises immobilier. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes qui représentent des hôtels et autres entreprises de dépôt. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la superficie d’une agglomération (par exemple, une ville ou un village). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’une agglomération. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’un département ou d’une province. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la capitale d’un pays ou d’une région. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Caractères représentant le nom abrégé d’une route. (Par exemple : I-5). Si vous définissez la propriété **ImageFamily** de l’entrée de paramètres sur la valeur *Palette*, utilisez uniquement des valeurs de palette. |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sorties, appartenant généralement à un réseau autoroutier. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des arrêts d’autobus, des gares, des aéroports, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones administratives telles que les pays, les régions et les départements. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bordures de région de pays et les étiquettes. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, États, provinces, etc., les bordures et des étiquettes. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Un administrateur 2, comtés, etc., les bordures et des étiquettes. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments et autres constructions. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour l’éducation. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés à des fins médicales telles que les hôpitaux. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour le transit tels que les aéroports. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies appartenant au réseau de transport (par exemple, routes, lignes de chemin de fer et lignes de ferry). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant toutes les routes. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les autoroutes grand accès contrôlé. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Les lignes qui représentent les chemins d’entrée haute vitesse qui se connectent généralement à contrôlé les autoroutes accès. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les autoroutes. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes principales. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes secondaires. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des rues. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des chemins d’entrée qui se connectent généralement aux autoroutes. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des rues unpaved. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes qui coûtent à utiliser. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies ferrées. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Sentiers de promenade au sein des parcs ou sentiers de randonnée pédestre. |
| >>> passerelle                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Passerelle avec élévation de privilèges. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes de ferry. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Tout type de plan d’eau. Comprend notamment les océans et les ruisseaux. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rivières, ruisseaux ou autres passages d’eau.  Notez que cet élément peut prendre la forme d’une ligne ou d’un polygone et qu’il peut être relié à des plans d’eau autres que des rivières. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les entrées associées de routage. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Acheminer ligne entrées consignées. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des itinéraires de conduite. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Lignes qui représentent des paysages itinéraires de conduite. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Les lignes qui représentent les parcours des itinéraires. |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les entrées utilisateur. |
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
| version                      | Chaîne  | Version de feuille de style ciblée. Utilisée à des fins d’applicabilité. « 1.0 » par défaut, « 1.* » pour les mises à jour de fonctionnalités mineures supplémentaires. |

<a id="settings" />

### <a name="settings-properties"></a>Propriétés de paramètres

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si l’atmosphère apparaît dans le contrôle 3D. |
| buildingTexturesVisible      | Booléen    |      |      |  ✔   |  ✔   | Indicateur indiquant s’il faut ou non afficher des textures sur des bâtiments symboliques en 3D qui ont des textures. |
| fogColor                     | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB du brouillard de distance qui apparaît dans le contrôle 3D. |
| glowColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB pouvant être appliquée à l’éclat des légendes et des icônes. |
| imageFamily                  | Chaîne  |  ✔   |  ✔   |  ✔   |  ✔   | Nom de l’ensemble d’images à utiliser pour ce style. Définissez cette propriété sur la valeur *Default* pour les signes qui utilisent des couleurs fixes basées sur le signe réel. Définissez cette propriété sur la valeur *Palette* pour les signes qui utilisent des couleurs de palette configurables. |
| landColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB d’une zone continentale avant qu’un élément quelconque soit dessiné sur cette dernière. |
| logosVisible                 | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété **Organization** doivent dessiner les logos appropriés ou utiliser une icône générique. |
| officialColorVisible         | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété de couleur officielle (telle que celle des lignes de transport en commun en Chine) doivent dessiner cette couleur. Par exemple, désactivez cette valeur dans le cas d’une carte en noir et blanc. |
| rasterRegionsVisible         | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Un indicateur qui indique s’il faut dessiner raster les régions où ils ont une meilleure représentation de vecteurs (Japon et Corée). |
| shadedReliefVisible          | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant s’il convient ou non de dessiner l’ombrage des hauteurs sur la carte. |
| shadedReliefDarkColor        | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté foncé d’un relief par ombres portées.  Canal alpha représente la valeur alpha maximale. |
| shadedReliefLightColor       | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté clair d’un relief par ombres portées.  Canal alpha représente la valeur alpha maximale. |
| shadowColor                  | Color   |      |      |      |  ✔   | La couleur de l’ombre derrière les icônes qui utilisent des ombres. |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB de la zone entourant la carte. |
| useDefaultImageColors        | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Un indicateur qui indique si les couleurs d’origine dans le format SVG doivent être utilisé plutôt que recherche de l’entrée de la palette de couleurs dans une image. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriétés MapElement

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Flottante   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de l’élément en arrière-plan d’une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| fillColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur utilisée pour le remplissage des polygones, pour l’arrière-plan des icônes d’objet ponctuel et pour le centre des lignes si elles sont fractionnées. |
| fontFamily                   | Chaîne  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du glyphe affiché au milieu d’une icône d’objet ponctuel. |
| iconScale                    | Flottante   |      |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du glyphe d’une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| labelColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Flottante   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle des tailles de légende par défaut. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| labelVisible                 | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Fait en sorte que la valeur alpha de l’élément **FillColor** remplace l’élément **StrokeColor** au lieu de fusionner avec ce dernier. |
| scale                        | Flottante   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la taille de l’objet ponctuel entier. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| strokeColor                  | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur à utiliser pour le contour des polygones, pour le contour des icônes d’objet ponctuel et pour la couleur des lignes. |
| strokeWidthScale             | Flottante   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du trait des lignes. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| visible                      | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne secondaire ou de contour de la bordure d’un polygone rempli. |
| borderStrokeColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne principale de la bordure d’un polygone rempli. |
| borderVisible                | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Flottante   |  ✔   |  ✔   |  ✔   |  ✔   | Le montant par lequel le trait des bordures sont mis à l’échelle. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriétés PointStyle

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| Arrière-plan de la forme             | Flottante   |      |      |      |  ✔   | Forme à utiliser en tant que l’arrière-plan de l’icône--Remplacer n’importe quelle forme existe. |
| stemAnchorRadiusScale        | Flottante   |      |      |  ✔   |  ✔   | Valeur de mise à l’échelle du point d'ancrage du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| stemColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de la ligne verticale sortant du bas de l’icône en mode 3D. |
| stemHeightScale              | Flottante   |      |      |  ✔   |  ✔   | Valeur de mise à l’échelle de la longueur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la longueur par défaut et la valeur *2* pour obtenir une longueur deux fois plus grande. |
| stemOutlineColor             | Color   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du contour de la ligne verticale sortant du bas de l’icône en mode 3D. |
| stemWidthScale               | Flottante   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la largeur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la longueur par défaut et la valeur *2* pour obtenir une longueur deux fois plus grande. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Booléen    |      |      |  ✔   |  ✔   | Indicateur indiquant qu’un modèle 3D doit être affiché comme un bâtiment, sans atténuation de profondeur par rapport au sol. |
