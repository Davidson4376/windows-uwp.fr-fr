---
author: muhsinking
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: Énumérer les appareils sur un réseau
description: Outre la détection d’appareils connectés localement, vous pouvez utiliser les API Windows.Devices.Enumeration pour énumérer les appareils sur protocoles sans fil et réseau.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 00f8d4314d67828fa30007d3b8af4c4e1d06c154
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6652829"
---
# <a name="enumerate-devices-over-a-network"></a>Énumérer les appareils sur un réseau



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Outre la découverte d’appareils connectés localement, vous pouvez utiliser les API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) pour énumérer les appareils sur protocoles sans fil et réseau.

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>Énumération d’appareils sur protocoles réseau ou sans fil

Parfois, vous devez énumérer des appareils qui ne sont pas connectés localement et peuvent uniquement être détectés via des protocoles sans fil ou réseau. Pour ce faire, les API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) possèdent trois genres différents d’objets appareil : **AssociationEndpoint** (AEP), **AssociationEndpointContainer** (conteneur AEP) et **AssociationEndpointService** (service AEP). En tant que groupe, ils sont appelés AEP ou objets AEP.

Certaines API d’appareils fournissent une chaîne de sélecteur que vous pouvez utiliser pour énumérer les objets AEP disponibles. Cela peut inclure à la fois les appareils associés et non associés avec le système. Certains appareils peuvent ne pas nécessiter d’association. Ces API d’appareil peuvent essayer de s’associer avec l’appareil si c’est nécessaire avant d’interagir avec celui-ci. Wi-Fi Direct est un exemple d’API qui suit ce modèle. Si ces API d’appareil ne s’associent pas automatiquement avec l’appareil, vous pouvez les associer à l’aide de l’objet [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396) disponible dans [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960).

Toutefois, dans certains cas, vous pouvez vouloir détecter manuellement les appareils par vous-même, sans utiliser une chaîne de sélecteur prédéfinie. Par exemple, vous devrez peut-être simplement collecter des informations sur des appareils AEP sans interagir avec eux, ou vous souhaiterez peut-être trouver plus d’objets AEP que ceux qui seront détectés avec la chaîne de sélecteur prédéfinie. Dans ce cas, vous créerez votre propre chaîne de sélecteur et vous l’utiliserez suivant les instructions disponibles sous [Créer un sélecteur d’appareil](build-a-device-selector.md).

Lorsque vous créez votre propre sélecteur, il est fortement recommandé de limiter la portée d’énumération aux protocoles qui vous intéressent. Par exemple, vous ne voulez pas que l’option Wi-Fi recherche des appareils Wi-Fi Direct si vous vous intéressez particulièrement aux appareils UPnP. Windows a défini une identité pour chaque protocole, que vous pouvez utiliser pour déterminer la portée de votre énumération. Le tableau suivant répertorie les types de protocoles et leurs identificateurs.

| Type de protocole ou d’appareil réseau              | Id                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP (incluant DIAL et DLNA)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| Web Services on Devices (WSD)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-Fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| Détection de service DNS (DNS-SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| Point de service                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| Imprimantes réseau (imprimantes Active Directory) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Windows Connect Now (WCN)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| Stations d’accueil WiGig                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| Approvisionnement Wi-Fi pour imprimantes HP           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| Bluetooth                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| Bluetooth LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## <a name="aqs-examples"></a>Exemples de requêtes AQS

Chaque genre d’AEP possède une propriété que vous pouvez utiliser pour limiter votre énumération à un protocole spécifique. Gardez à l’esprit que vous pouvez utiliser l’opérateur OR dans un filtre AQS pour combiner plusieurs protocoles. Voici quelques exemples de chaînes de filtrage AQS qui montrent comment effectuer une requête pour les appareils AEP.

Cette requête AQS interroge tous les objets **AssociationEndpoint** UPnP lorsque [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) est défini sur **AsssociationEndpoint**.

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Cette requête AQS interroge tous les objets **AssociationEndpoint** UPnP et WSD lorsque [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) est défini sur **AsssociationEndpoint**.

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Cette requête AQS interroge tous les objets **AssociationEndpointService** UPnP si [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) est défini sur **AsssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Cette requête AQS interroge les objets **AssociationEndpointContainer** lorsque [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) est défini sur **AssociationEndpointContainer**, mais les trouve uniquement en énumérant le protocole UPnP. En règle générale, il n’est pas utile d’énumérer les conteneurs qui proviennent d’un seul protocole. Toutefois, cela permet de limiter votre filtre aux protocoles pour lesquels vous savez que votre appareil peut être détecté.

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 
