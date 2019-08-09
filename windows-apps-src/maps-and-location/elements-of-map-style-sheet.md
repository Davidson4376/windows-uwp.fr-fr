---
description: Entrées et propriétés d’une feuille de style de carte
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Référence de feuille de style de carte
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, cartes, feuille de style de carte
ms.localizationpriority: medium
ms.openlocfilehash: 723426b4affec4251f26485ac7ecc1fca307c102
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867701"
---
# <a name="map-style-sheet-reference"></a>Référence de feuille de style de carte

Les technologies de mappage Microsoft utilisent des _feuilles de style de carte_ pour définir l’apparence des cartes.  Une feuille de style cartographique est définie à l’aide de JavaScript Object Notation (JSON) et peut être utilisée de différentes façons, notamment dans les [collection MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) d’une application Windows Store via la méthode [MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Les feuilles de style peuvent être créées de manière interactive à l’aide de l’application [éditeur de feuille de style de carte](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .

Le code JSON suivant peut être utilisé pour faire apparaître les zones d’eau en rouge, les étiquettes d’eau s’affichent en vert et les zones terrestres en bleu:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Ce JSON peut être utilisé pour supprimer toutes les étiquettes et tous les points d’une carte.

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

Cet article indique les entrées et [propriétés](#properties) JSON que vous pouvez utiliser pour personnaliser l’apparence de vos cartes.  Ces propriétés peuvent également être appliquées aux éléments de mappage d’utilisateur par le biais de la propriété [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) .

<a id="entries" />

## <a name="entries"></a>Entrées
Ce tableau utilise des caractères « > » pour représenter les différents niveaux de la hiérarchie des entrées.  Il montre également quelles versions de Windows prennent en charge chaque entrée et qui l’ignore.

| Version | Nom de la version Windows |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | Mise à jour d’avril 2018    |
|  1809   | Mise à jour d’octobre 2018  |
|  1903   | Mise à jour 2019 mai      |

| Nom                               | Groupe de propriétés            | 1703 | 1709 | 1803 | 1809 | 1903 | Description    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| version                            | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Version de feuille de style que vous souhaitez utiliser. |
| paramètres                           | [Réglages](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Paramètres qui s’appliquent à la totalité de la feuille de style. |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées de la carte. |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Entrée parente de toutes les entrées non utilisateur. |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones décrivant l’utilisation des terrains.  Celles-ci ne doivent pas être confondues avec les bâtiments physiques qui se trouvent sous l’entrée de structure. |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les aéroports. |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Zones dans lesquelles il y a une forte concentration d'entreprises ou de points intéressants. |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent Cemeteries. |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de la zone de continent. |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent des écoles et d’autres sites éducatifs. |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent des réserves de populations indigènes. |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Zones utilisées à des fins industrielles. |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone d’îlot. |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones utilisées à des fins médicales (par exemple, un Campus hospitalier). |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Les zones qui englobent les bases militaires ou qui ont des utilisations militaires. |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones utilisées à des fins relatives à la nautique. |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Étiquettes de zone de voisinage. |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones utilisées comme piste d’avion. |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones sablonneuses, telles que des plages. |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Surfaces au sol allouées à des galeries marchandes ou à d’autres centres commerciaux. |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les stades. |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Zones souterraines (par exemple : emplacement d'une station de métro). |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Forêts, zones herbeuses, etc. |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Terres forestières. |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent les cours golf. |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent des parcs. |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Terrains de sport, tels qu’un terrain de base-ball ou un court de tennis. |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones qui englobent des réserves de nature. |
| > > frozenWater                     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Eau figée, telle que glacier. |
| >> point                           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les fonctionnalités de point qui sont dessinées avec une icône d’un certain type. |
| >>> address                        | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | Étiquettes des numéros d’adresse. |
| >>> naturalPoint                   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes qui représentent des fonctionnalités naturelles. |
| >>>> peak                          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des pics montagneux. |
| >>>>> volcanicPeak                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sommets de volcan. |
| >>>> waterPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des points d’eau, tels qu’une cascade. |
| >>> pointOfInterest                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant n’importe quel emplacement intéressant. |
| >>>> business                      | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant n’importe quel emplacement d’entreprise. |
| > > > > > attractionPoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes qui représentent des attractions touristiques comme les musées, les jardins zoologiques, etc. |
| > > > > > > amusementPlacePoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de la place de divertissement. |
| > > > > > > aquariumPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône d’aquarium. |
| > > > > > > artGalleryPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de la Galerie art. |
| > > > > > > campPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône camp. |
| > > > > > > fishingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de pêche. |
| > > > > > > gardenPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de jardin. |
| > > > > > > hikingPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de randonnée. |
| > > > > > > libraryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de bibliothèque. |
| > > > > > > museumPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône Musée. |
| > > > > > > naturalPlacePoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de place naturelle. |
| > > > > > > parkPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de parc. |
| > > > > > > restAreaPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de la zone Rest. |
| > > > > > > touristAttractionPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône touristiques attractions. |
| > > > > > > zooPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône Zoo. |
| > > > > > communityPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes qui représentent les emplacements d’utilisation générale pour la communauté. |
| > > > > > > conventionCenterPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône du centre de Convention. |
| > > > > > > financialPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône financier. |
| > > > > > > governmentPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône gouvernement. |
| > > > > > > informationTechnologyPoint  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône technologies de l’information. |
| > > > > > > palacePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Palace icône. |
| > > > > > > pollingStationPoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de la station d’interrogation. |
| > > > > > > researchPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de recherche. |
| > > > > > educationPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes qui représentent des écoles et d’autres emplacements liés à l’enseignement. |
| > > > > > entertainmentPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des systèmes de divertissement tels que des théâtres, des cinémas, etc. |
| > > > > > > artsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône Arts. |
| > > > > > > bowlingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de bowling. |
| > > > > > > casinoPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône casino. |
| > > > > > > golfCoursePoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de cours golf. |
| > > > > > > gymPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Sport icône. |
| > > > > > > marinaPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Marina icône. |
| > > > > > > movieTheaterPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône cinéma. |
| > > > > > > nightClubPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de club de nuit. |
| > > > > > > recreationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de recréation. |
| > > > > > > skatingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de patin. |
| > > > > > > skiAreaPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de zone de ski. |
| > > > > > > stadiumPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de piscine. |
| > > > > > > swimmingPoolPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de piscine. |
| > > > > > > theaterPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de théâtre. |
| > > > > > > wineryPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de vin. |
| > > > > > essentialServicePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes qui représentent des services essentiels tels que le parking, les banques, le gaz, etc. |
| > > > > > > aTMPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône ATM. |
| > > > > > > automobileRentalPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de location d’automobile. |
| > > > > > > automobileRepairPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de réparation automobile. |
| > > > > > > bankPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de la Banque. |
| > > > > > > clinicPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône Clinic. |
| > > > > > > electricChargingStationPoint| [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de station de chargement électrique. |
| > > > > > > fireStationPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | FireStation icône. |
| > > > > > > gasStationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | GasStation icône. |
| > > > > > > groceryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône d’épicerie. |
| > > > > > > hospitalPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de l’hôpital. |
| > > > > > > laundryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de blanchisserie. |
| > > > > > > liquorAndWineStorePoint     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de liqueur et de vin. |
| > > > > > > mailPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône courrier. |
| > > > > > > marketPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de marché. |
| > > > > > > parkingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de parking. |
| > > > > > > petsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône animaux. |
| > > > > > > pharmacyPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône pharmacie. |
| > > > > > > policePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de police. |
| > > > > > > postalServicePoint          | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône service postal. |
| > > > > > > professionalPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de service professionnel. |
| > > > > > > toiletPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de toilette. |
| > > > > > > veterinarianPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône VETERINAIRE. |
| >>>>> foodPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes qui représentent des restaurants, des cafés, etc. |
| > > > > > > barAndGrillPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de barre et de grille. |
| > > > > > > barPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de barre. |
| > > > > > > breweryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de brasserie. |
| > > > > > > cafePoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de café. |
| > > > > > > iceCreamShopPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de magasin de glace. |
| > > > > > > restaurantPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de restaurant. |
| > > > > > > teaShopPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | TeaShop icône. |
| > > > > > lodgingPoint                 | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes représentant les hôtels et les autres entreprises de dépôt. |
| > > > > > > gotelPoint                  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône d’hôtel. |
| > > > > > realEstatePoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes représentant des entreprises immobilières. |
| > > > > > shoppingPoint                | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Les icônes représentant les hôtels et les autres entreprises de dépôt. |
| > > > > > > automobileDealerPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de concessionnaire d’automobile. |
| > > > > > > beautyAndSpaPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de beauté et de Spa. |
| > > > > > > bookstorePoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône librairie. |
| > > > > > > departmentStorePoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de service Store. |
| > > > > > > electronicsShopPoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de magasin électronique. |
| > > > > > > golfShopPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône Golf Shop. |
| > > > > > > homeApplianceShopPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de la boutique du matériel. |
| > > > > > > mallPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de centre commercial. |
| > > > > > > phoneShopPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icône de magasin téléphonique. |
| >>> populatedPlace                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la superficie d’une agglomération (par exemple, une ville ou un village). |
| >>>> capital                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’une agglomération. |
| >>>>> adminDistrictCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant le chef-lieu d’un département ou d’une province. |
| > > > > > > majorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icônes représentant le capital principal d’un État ou d’une province. |
| > > > > > > minorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icônes représentant le capital mineur d’un État ou d’une province. |
| >>>>> countryRegionCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant la capitale d’un pays ou d’une région. |
| > > > > majorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Les icônes qui représentent la taille de l’emplacement rempli par l’essentiel. |
| > > > > minorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Les icônes qui représentent la taille de l’emplacement de remplissage mineur. |
| >>> roadShield                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Caractères représentant le nom abrégé d’une route. (Par exemple: I-5). Si vous définissez la propriété **ImageFamily** de l’entrée de paramètres sur la valeur *Palette*, utilisez uniquement des valeurs de palette. |
| >>> roadExit                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des sorties, appartenant généralement à un réseau autoroutier. |
| >>> transit                        | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Icônes représentant des arrêts d’autobus, des gares, des aéroports, etc. |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zones administratives telles que les pays, les régions et les départements. |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bordures et étiquettes de la région du pays. |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, États, provinces, etc., bordures et étiquettes. |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Un administrateur 2, comtés, etc., bordures et étiquettes. |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments et autres constructions. |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Locaux. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés pour l’éducation. |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bâtiments utilisés à des fins médicales, comme des hôpitaux. |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Constructions utilisées pour le transit, telles que les aéroports. |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Voies appartenant au réseau de transport (par exemple, routes, lignes de chemin de fer et lignes de ferry). |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Voies représentant toutes les routes. |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent les grandes routes d’accès contrôlé. |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des rampes haute vitesse qui se connectent généralement à des routes d’accès contrôlé. |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes représentant des routes. |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes majeures. |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes représentant des routes artérielles. |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des rues. |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des rampes qui se connectent généralement aux routes. |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des rues à la une. |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes représentant des routes qui coûtent l’utilisation. |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Voies ferrées. |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Sentiers de promenade au sein des parcs ou sentiers de randonnée pédestre. |
| > > > de la passerelle                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Passerelle avec élévation de privilèges. |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes de ferry. |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Tout type de plan d’eau. Comprend notamment les océans et les ruisseaux. |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Rivières, ruisseaux ou autres passages d’eau.  Notez que cet élément peut prendre la forme d’une ligne ou d’un polygone et qu’il peut être relié à des plans d’eau autres que des rivières. |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les entrées associées au routage. |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Entrées liées à la ligne de routage. |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes de conduite. |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes de conduite paysages. |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Lignes qui représentent des routes de parcours. |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Toutes les entrées utilisateur. |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Style des instances [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) par défaut. |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Style des instances [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) par défaut. |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | Style des instances [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) par défaut.  Cela permet principalement de configurer renderAsSurface. |
| >> userPoint                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Style des instances [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) par défaut. |

<a id="properties" />

## <a name="properties"></a>Properties

Cette section décrit les propriétés que vous pouvez utiliser pour chaque entrée.

<a id="version" />

### <a name="version-properties"></a>Propriétés de version

| Propriété                     | Type    | Description                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | Version de feuille de style ciblée. Utilisée à des fins d’applicabilité. « 1.0 » par défaut, « 1.* » pour les mises à jour de fonctionnalités mineures supplémentaires. |

<a id="settings" />

### <a name="settings-properties"></a>Propriétés de paramètres

| Propriété                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | Description |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si l’atmosphère apparaît dans le contrôle 3D. |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | Indicateur indiquant s’il faut ou non afficher des textures sur des bâtiments symboliques en 3D qui ont des textures. |
| fogColor                     | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB du brouillard de distance qui apparaît dans le contrôle 3D. |
| glowColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB pouvant être appliquée à l’éclat des légendes et des icônes. |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Nom de l’ensemble d’images à utiliser pour ce style. Définissez cette propriété sur la valeur *Default* pour les signes qui utilisent des couleurs fixes basées sur le signe réel. Définissez cette propriété sur la valeur *Palette* pour les signes qui utilisent des couleurs de palette configurables. |
| landColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB d’une zone continentale avant qu’un élément quelconque soit dessiné sur cette dernière. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété **Organization** doivent dessiner les logos appropriés ou utiliser une icône générique. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant si les éléments dotés d’une propriété de couleur officielle (telle que celle des lignes de transport en commun en Chine) doivent dessiner cette couleur. Par exemple, désactivez cette valeur dans le cas d’une carte en noir et blanc. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur qui spécifie s’il faut ou non dessiner des régions raster dans lesquelles elles ont une meilleure représentation que les vecteurs (Japon et Corée). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur précisant s’il convient ou non de dessiner l’ombrage des hauteurs sur la carte. |
| shadedReliefDarkColor        | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté foncé d’un relief par ombres portées.  Le canal alpha représente la valeur alpha maximale. |
| shadedReliefLightColor       | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du côté clair d’un relief par ombres portées.  Le canal alpha représente la valeur alpha maximale. |
| shadowColor                  | Color   |      |      |      |  ✔   |  ✔   | Couleur de l’ombre derrière les icônes qui utilisent des ombres. |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de couleur ARVB de la zone entourant la carte. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indicateur qui signale si les couleurs d’origine dans le SVG doivent être utilisées au lieu de rechercher dans l’entrée de la palette les couleurs d’une image. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriétés MapElement

| Propriété                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | Description |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de l’élément en arrière-plan d’une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| fillColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur utilisée pour le remplissage des polygones, pour l’arrière-plan des icônes d’objet ponctuel et pour le centre des lignes si elles sont fractionnées. |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | Densité d’un caractère, en termes de clarté ou d’épaisseur des traits. Les attributs «**Light**», «**normal**», «**SemiBold**» et «**Bold**» peuvent être définis |
| iconColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du glyphe affiché au milieu d’une icône d’objet ponctuel. |
| iconScale                    | Float   |      |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du glyphe d’une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| labelColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle des tailles de légende par défaut. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Fait en sorte que la valeur alpha de l’élément **FillColor** remplace l’élément **StrokeColor** au lieu de fusionner avec ce dernier. |
| scale                        | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la taille de l’objet ponctuel entier. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| strokeColor                  | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur à utiliser pour le contour des polygones, pour le contour des icônes d’objet ponctuel et pour la couleur des lignes. |
| strokeWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du trait des lignes. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | Description |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne secondaire ou de contour de la bordure d’un polygone rempli. |
| borderStrokeColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de ligne principale de la bordure d’un polygone rempli. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du trait des bordures. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriétés PointStyle

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | Description |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | Indicateur qui spécifie si l’ombre de l’icône doit être visible ou non |
| forme-arrière-plan             | String  |      |      |      |      |  ✔   | Forme à utiliser comme arrière-plan de l’icône, en remplaçant toute forme qui existe. |
| forme-icône                   | String  |      |      |      |      |  ✔   | Forme à utiliser comme glyphe de premier plan de l’icône, en remplaçant toute forme qui existe. |
| stemAnchorRadiusScale        | Float   |      |      |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle du point d'ancrage du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| stemColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur de la ligne verticale sortant du bas de l’icône en mode 3D. |
| stemHeightScale              | Float   |      |      |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la longueur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la longueur par défaut et la valeur *2* pour obtenir une longueur deux fois plus grande. |
| stemOutlineColor             | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Couleur du contour de la ligne verticale sortant du bas de l’icône en mode 3D. |
| stemWidthScale               | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Valeur de mise à l’échelle de la largeur du pied d'une icône.  Par exemple, utilisez la valeur *1* pour obtenir la longueur par défaut et la valeur *2* pour obtenir une longueur deux fois plus grande. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | Description |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | Indicateur indiquant qu’un modèle 3D doit être affiché comme un bâtiment, sans atténuation de profondeur par rapport au sol. |
