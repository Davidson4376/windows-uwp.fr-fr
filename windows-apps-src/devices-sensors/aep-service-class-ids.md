---
author: muhsinking
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: ID de classe de service AEP
description: Les services de point de terminaison d’association (AEP) offrent un contrat de programmation pour les services qu’un appareil prend en charge sur un protocole donné. Plusieurs de ces services ont des identificateurs établis qui doivent être utilisés lors de leur référencement.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5f103ee3c281ca95abcaee76cdc6f88b74a49eb1
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5563224"
---
# <a name="aep-service-class-ids"></a>ID de classe de service AEP



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Les services de point de terminaison d’association (AEP) offrent un contrat de programmation pour les services qu’un appareil prend en charge sur un protocole donné. Plusieurs de ces services ont des identificateurs établis qui doivent être utilisés lors de leur référencement. Ces contrats sont identifiés à l’aide de la propriété **System.Devices.AepService.ServiceClassId**. Cette rubrique répertorie plusieurs ID de classe de service AEP bien connus. L’ID de classe de service AEP est également applicable aux protocoles dont l’ID de classe est personnalisé.

Un développeur d’application doit utiliser des filtres de syntaxe de recherche avancée (AQS) basés sur les ID de classe afin de limiter les requêtes aux services AEP qu’ils prévoient d’utiliser. Cela limite les résultats de requête aux services pertinents et augmente considérablement les performances, l’autonomie de la batterie et la qualité de service de l’appareil. Par exemple, une application peut se servir de ces ID de classe de service pour utiliser un appareil tel qu’un synchroniseur Miracast ou un appareil multimédia de rendu numérique (DMR) DLNA. Pour plus d’informations sur l’interaction entre les appareils et les services, voir [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991).

## <a name="bluetooth-and-bluetooth-le-services"></a>Services Bluetooth et Bluetooth LE

Les services Bluetooth utilisent le protocole Bluetooth ou le protocole Bluetooth LE. Les identificateurs de ces protocoles sont les suivants:

-   ID de protocole Bluetooth: {e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
-   ID de protocole Bluetooth LE: {bb7bb05e-5972-42b5-94fc-76eaa7084d49}

Le protocole Bluetooth prend en charge plusieurs services présentant tous le même format de base. Les quatre premiers chiffres du GUID varient en fonction du service, mais tous les GUID Bluetooth se terminent par **0000-0000-1000-8000-00805F9B34FB**. Par exemple, le service RFCOMM a le précurseur 0x0003. L’ID complet est donc **00030000-0000-1000-8000-00805F9B34FB**. Le tableau suivant répertorie certains services Bluetooth courants.

| Nom du service                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT - Service de notification des alertes    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT - E/S d’automatisation                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT - Service de batterie               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT - Tension artérielle                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT - Composition de corps              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT - Gestion de papier de luxe               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT - Contrôle glycémique continu | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT - Temps de service actuel          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT - Puissance de pédalage                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT - Vitesse et cadence de pédalage     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT - Informations sur l’appareil            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT - Détection environnementale         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT - Accès générique                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT - Attribut générique             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT - Glucose                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT - Thermomètre médical            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT - Fréquence cardiaque                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT - Appareil d’interface utilisateur        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT - Alerte immédiate               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT - Positionnement intérieur            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT - Prise en charge de protocole Internet     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT - Perte de lien                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT - Localisation et navigation       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT - Service de changement d’heure d’été suivant       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT - Service d’état d’alerte téléphonique    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT - Oxymètre de pouls                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT - Service de mise à jour d’heure de référence | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT - Vitesse et cadence de course     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT - Paramètres de numérisation               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT - Alimentation TX                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT - Données utilisateur                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT - Échelle de poids                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

Pour une liste plus complète des services Bluetooth disponibles, voir les pages relatives au protocole et au service Bluetooth [ici](http://go.microsoft.com/fwlink/p/?LinkID=619586) et [ici](http://go.microsoft.com/fwlink/p/?LinkID=619587). Vous pouvez également utiliser l’API [**GattServiceUuids**](https://msdn.microsoft.com/library/windows/apps/Dn297571) pour obtenir des services GATT communs.

## <a name="custom-bluetooth-le-services"></a>Services Bluetooth LE personnalisés

Les services Bluetooth LE personnalisés utilisent l’identificateur de protocole suivant : {bb7bb05e-5972-42b5-94fc76eaa7084d49}

Les profils personnalisés sont définis avec leurs propres GUID. Ce GUID personnalisé doit être utilisé pour **System.Devices.AepService.ServiceClassId**.

## <a name="upnp-services"></a>Services UPnP

Les services UPnP utilisent l’identificateur de protocole suivant: {0e261de4-12f0-46e6-91ba-428607ccef64}

En règle générale, tous les services UPnP ont leur nom haché dans un GUID à l’aide de l’algorithme défini dans RFC 4122. Le tableau suivant répertorie certains services UPnP communs définis dans Windows.

| Nom du service                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| Gestionnaire des connexions                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| Transport AV                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| Contrôle de rendu                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| Transfert de couche 3                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| Configuration d’interface commune WAN | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| Connexion IP WAP                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| Configuration de WLAN WFA             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| Imprimante améliorée                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| Imprimante de base                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| Registre du récepteur multimédia           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| Répertoire de contenu                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| NUMÉROTER                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

 

## <a name="wsd-services"></a>Services WSD

Les services WSD utilisent l’identificateur de protocole suivant: {782232aa-a2f9-4993-971b-aedc551346b0}

En règle générale, tous les services WSD ont leur nom haché dans un GUID à l’aide de l’algorithme défini dans RFC 4122. Le tableau suivant répertorie certains services WSD communs définis dans Windows.

| Nom du service | GUID                                     |
|--------------|------------------------------------------|
| Imprimante      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| Scanneur      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

 

## <a name="aqs-sample"></a>Exemple AQS

Cette AQS filtre tous les objets **AssociationEndpointService** UPnP qui prennent en charge la commande NUMÉROTER. Dans ce cas, le paramètre [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) est défini sur **AsssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```

 

 
