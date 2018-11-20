---
author: normesta
description: Entrées et propriétés d’une feuille de style de carte
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Référence de feuille de style de carte
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
keywords: windows10, uwp, cartes, feuille de style de carte
ms.localizationpriority: medium
ms.openlocfilehash: eace82801b2e3d1423eeec9e9da7cf56db043666
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7302713"
---
# <a name="map-style-sheet-reference"></a>Référence de feuille de style de carte

Technologies de mise en correspondance de Microsoft utilisent des _feuilles de style de carte_ pour définir l’apparence de cartes.  Une feuille de style de carte est définie à l’aide de JavaScript Object Notation (JSON) et peut être utilisée dans différentes manières, y compris dans le d’une application du Windows Store [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) par le biais de la méthode [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Feuilles de style peuvent être créés de manière interactive à l’aide de l’application de [l’Éditeur de feuilles de Style de carte](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .

Le code JSON ci-après peut être utilisé pour faire d’eau en rouge, des plans d’eau en vert et apparaissent de zones continentales en bleu:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Ce JSON peut servir à supprimer la totalité des légendes et des points d’une carte.

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

Cet article indique les entrées et [propriétés](#properties) JSON que vous pouvez utiliser pour personnaliser l’apparence de vos cartes.  Ces propriétés peuvent également être appliquées aux éléments de carte utilisateur par le biais de la propriété [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) .

<a id="entries" />

## <a name="entries"></a>Entrées
Ce tableau utilise des caractères «>» pour représenter les différents niveaux de la hiérarchie des entrées.  Il indique également quelles versions de Windows prend en charge de chaque entrée et dont l’ignorer.

| Version | Nom de la version Windows |
|---------|----------------------|
|  1703   | CreatorsUpdate      |
|  1709   | FallCreatorsUpdate |
|  1803   | Mise à jour d’avril2018    |
|  1809   | Mise à jour octobre 2018  |

| Nom                         | Groupe de propriétés            | 1703 | 1709 | 1803 | 1809 | Description    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | Version de feuille de style que vous souhaitez utiliser. |
| settings                     | [Settings](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | Paramètres qui s’appliquent à la totalité de la feuille de style. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées de la carte. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées non utilisateur. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Utilisation de zones décrivant continentales.  Il ne doivent ne pas pour confondre avec les bâtiments physiques qui sont sous l’entrée de la structure. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent aéroports. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones dans lesquelles il y a une forte concentration d'entreprises ou de points intéressants. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent cimetières. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone continent. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les établissements scolaires et les autres infrastructures éducatifs. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Qui englobent autochtones réserves. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Domaines qui sont utilisés à des fins industrielles. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone (île). |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones utilisées à des fins médicales (par exemple: un campus hospitalier). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent des bases militaires ou ont des usages militaires. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Domaines qui sont utilisés pour nautiques connexes. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone de voisinage. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui est utilisé comme une piste de décollage avion. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones sablonneuses, telles que des plages. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Surfaces au sol allouées à des galeries marchandes ou à d’autres centres commerciaux. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent stades. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zones souterraines (par exemple: emplacement d'une station de métro). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Forêts, zones herbeuses, etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Terres forestières. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent golfs. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent parcs. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Terrains de sport, tels qu’un terrain de base-ball ou un court de tennis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent réserves naturelles. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les fonctionnalités de point qui sont tracées avec une icône quelconque. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Étiquettes de nombres d’adresse. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des fonctionnalités naturelles. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des pics montagneux. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sommets de volcan. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des points d’eau, tels qu’une cascade. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant n’importe quel emplacement intéressante. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant n’importe quel locaiton d’entreprise. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des sites touristiques tels que musées, zoos, etc.. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des emplacements d’utilisation générale de la Communauté. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des établissements scolaires et les autres éducation liées à des emplacements. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des lieux de divertissement telles que des salles, cinémas, etc.. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des services essentiels tels que stationnement, banques, pédale, etc.. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des restaurants, cafés, etc.. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des hôtels ou autres sociétés de dépôt. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des entreprises de l’espace de l’écran. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Icônes représentant des hôtels ou autres sociétés de dépôt. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la superficie d’une agglomération (par exemple, une ville ou un village). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’une agglomération. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’un département ou d’une province. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la capitale d’un pays ou d’une région. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Caractères représentant le nom abrégé d’une route. (Par exemple: A6). Si vous définissez la propriété **ImageFamily** de l’entrée de paramètres sur la valeur *Palette*, utilisez uniquement des valeurs de palette. |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sorties, appartenant généralement à un réseau autoroutier. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des arrêts d’autobus, des gares, des aéroports, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zones administratives telles que les pays, les régions et les départements. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Les bordures de la région de pays et des étiquettes. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, départements, provinces, etc., les bordures et des étiquettes. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, sous-préfectures, etc., les bordures et des étiquettes. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments et autres constructions. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour l’éducation. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés à des fins médicales par exemple, hôpitaux. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour le transit, tels que des aéroports. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies appartenant au réseau de transport (par exemple, routes, lignes de chemin de fer et lignes de ferry). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant toutes les routes. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant réseau autoroutier volumineux et contrôlé par l’accès. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant les bretelles haut débit qui se connectent généralement à contrôlée réseau autoroutier accès. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant réseau autoroutier. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant les routes principales. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant les routes secondaires. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant une autoroute. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant les bretelles généralement se connectent au réseau autoroutier. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant unpaved AutoRoute. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant les routes à péage. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies ferrées. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Sentiers de promenade au sein des parcs ou sentiers de randonnée pédestre. |
| >>> passerelle                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Passerelle avec élévation de privilèges. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Lignes de ferry. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Tout type de plan d’eau. Comprend notamment les océans et les ruisseaux. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rivières, ruisseaux ou autres passages d’eau.  Notez que cet élément peut prendre la forme d’une ligne ou d’un polygone et qu’il peut être relié à des plans d’eau autres que des rivières. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les écritures connexes de routage. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | La ligne de l’itinéraire entrées liées. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant des itinéraires en voiture. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Voies représentant les itinéraires de conduite plus pittoresques. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Les lignes qui représentent à pied itinéraires. |
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
| version                      | Chaîne  | Version de feuille de style ciblée. Utilisée à des fins d’applicabilité. «1.0» par défaut, «1.*» pour les mises à jour de fonctionnalités mineures supplémentaires. |

<a id="settings" />

### <a name="settings-properties"></a>Propriétés de paramètres

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si l’atmosphère apparaît dans le contrôle3D. |
| buildingTexturesVisible      | Booléen    |      |      |  ✔   |  ✔   | Indicateur indiquant s’il faut ou non afficher des textures sur des bâtiments symboliques en 3D qui ont des textures. |
| fogColor                     | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB du brouillard de distance qui apparaît dans le contrôle3D. |
| glowColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB pouvant être appliquée à l’éclat des légendes et des icônes. |
| imageFamily                  | Chaîne  |  ✔   |  ✔   |  ✔   |  ✔   | Nom de l’ensemble d’images à utiliser pour ce style. Définissez cette propriété sur la valeur *Default* pour les signes qui utilisent des couleurs fixes basées sur le signe réel. Définissez cette propriété sur la valeur *Palette* pour les signes qui utilisent des couleurs de palette configurables. |
| landColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB d’une zone continentale avant qu’un élément quelconque soit dessiné sur cette dernière. |
| logosVisible                 | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété **Organization** doivent dessiner les logos appropriés ou utiliser une icône générique. |
| officialColorVisible         | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété de couleur officielle (telle que celle des lignes de transport en commun en Chine) doivent dessiner cette couleur. Par exemple, désactivez cette valeur dans le cas d’une carte en noir et blanc. |
| rasterRegionsVisible         | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant s’il convient ou non de dessiner les régions raster où ils ont une représentation mieux que les vecteurs (Japon et Corée). |
| shadedReliefVisible          | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant s’il convient ou non de dessiner l’ombrage des hauteurs sur la carte. |
| shadedReliefDarkColor        | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté foncé d’un relief par ombres portées.  Le canal alpha représente la valeur alpha maximale. |
| shadedReliefLightColor       | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté clair d’un relief par ombres portées.  Le canal alpha représente la valeur alpha maximale. |
| shadowColor                  | Couleur   |      |      |      |  ✔️   | La couleur de l’ombre derrière les icônes qui utilisent des ombres. |
| spaceColor                   | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB de la zone entourant la carte. |
| useDefaultImageColors        | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   | Un indicateur qui indique si les couleurs d’origine dans le format SVG doivent être utilisés plutôt que recherche de l’entrée de palette de couleurs d’une image. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriétés MapElement

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
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

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne secondaire ou de contour de la bordure d’un polygone rempli. |
| borderStrokeColor            | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne principale de la bordure d’un polygone rempli. |
| borderVisible                | Booléen    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | À l’échelle qui le trait de bordures sont. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriétés PointStyle

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|-------------|
| Arrière-plan de la forme             | Flottant   |      |      |      |  ✔️   | Forme à utiliser en tant que l’arrière-plan de l’icône--en remplaçant toute forme qui existe à cet endroit. |
| stemAnchorRadiusScale        | Flottant   |      |      |  ✔   |  ✔   | Valeur de mise à l’échelle du point d'ancrage du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| stemColor                    | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de la ligne verticale sortant du bas de l’icône en mode3D. |
| stemHeightScale              | Flottant   |      |      |  ✔   |  ✔   | Valeur de mise à l’échelle de la longueur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la largeur par défaut et la valeur *2* pour obtenir une largeur deux fois plus grande. |
| stemOutlineColor             | Couleur   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du contour de la ligne verticale sortant du bas de l’icône en mode3D. |
| stemWidthScale               | Flottant   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la largeur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la largeur par défaut et la valeur *2* pour obtenir une largeur deux fois plus grande. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | 1703 | 1709 | 1803 | 1809 | Description |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Booléen    |      |      |  ✔   |  ✔   | Indicateur indiquant qu’un modèle 3D doit être affiché comme un bâtiment, sans atténuation de profondeur par rapport au sol. |
