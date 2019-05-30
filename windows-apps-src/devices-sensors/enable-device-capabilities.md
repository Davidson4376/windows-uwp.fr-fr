---
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: Activer les fonctionnalités d’un appareil
description: Ce didacticiel décrit comment déclarer des fonctionnalités d’appareil dans Microsoft Visual Studio. Votre application peut ainsi utiliser des caméras, des microphones, des capteurs de localisation et d’autres appareils.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cd8c493333c2c35dee5ead064f9d002701cc1148
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370223"
---
# <a name="enable-device-capabilities"></a>Activer les fonctionnalités d’un appareil



Ce didacticiel décrit comment déclarer des fonctionnalités d’appareil dans Microsoft Visual Studio. Votre application peut ainsi utiliser des caméras, des microphones, des capteurs de localisation et d’autres appareils.

## <a name="specify-the-device-capabilities-your-app-will-use"></a>Spécifier les fonctionnalités d’appareil que votre application utilisera


Avec des applications Windows , vous êtes tenu de spécifier dans le manifeste du package d’application quand vous utilisez certains types d’appareils. Dans Visual Studio, vous pouvez déclarer la plupart des fonctionnalités à l’aide du [concepteur du manifeste](https://docs.microsoft.com/previous-versions/br230259(v=vs.140)) ou vous pouvez les ajouter manuellement comme indiqué dans [Comment spécifier des fonctionnalités de périphérique dans un manifeste de package (manuellement)](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest). Ce didacticiel suppose que vous utilisez le concepteur du manifeste.

**Remarque**    certains types de périphériques, tels que des imprimantes, scanneurs et les capteurs, n’avez pas besoin d’être déclaré dans le manifeste de package d’application.

-   Dans l’Explorateur de solutions de Visual Studio, double-cliquez sur le fichier manifeste de package **Package.appxmanifest**.
-   Ouvrez l’onglet **Capacités**.
-   Sélectionnez les fonctionnalités de périphérique utilisées par votre application. Si vous ne trouvez pas la fonctionnalité recherchée dans le concepteur du manifeste, ajoutez-la manuellement. Pour plus d’informations, voir [Comment spécifier des fonctionnalités de périphérique dans un manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest).

| Fonctionnalité du périphérique | Concepteur du manifeste | Description |
|-------------------|-------------------|-------------|    
| AllJoyn | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications et appareils compatibles AllJoyn de se détecter mutuellement et d’interagir sur un réseau. Toutes les applications qui accèdent à des API de l’espace de noms [**Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) doivent utiliser cette fonctionnalité. |
| Messages de conversation bloqués | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications de lire les messages SMS et MMS bloqués par l’application de filtrage anti-spam. |
| Accès aux messages de conversation | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications de lire et de supprimer des messages texte. Permet également aux applications de stocker des messages de conversation dans le magasin de données système. |
| Génération de code | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications de générer du code de façon dynamique. |
| Authentification en entreprise | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Cette fonctionnalité est régie par la politique du Microsoft Store. Elle offre la possibilité de se connecter à des ressources intranet d’entreprise qui nécessitent des informations d’identification de domaine. Cette fonctionnalité n’est généralement pas nécessaire pour la plupart des applications. | 
| Internet (client) | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Fournit un accès sortant à Internet et aux réseaux dans des lieux publics tels que des aéroports et des cafés. Par exemple, des réseaux intranet où l’utilisateur a désigné le réseau comme étant public. La plupart des applications qui nécessitent un accès Internet doivent utiliser cette fonctionnalité. |
| Internet (client &amp; serveur) | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Fournit un accès bidirectionnel à Internet et aux réseaux dans des lieux publics tels que des aéroports et des cafés. Cette fonctionnalité est un sur-ensemble de **Internet (Client)** . **Internet (Client)** n’a pas besoin d’être activé si cette fonctionnalité est activée. L’accès entrant aux ports critiques est toujours bloqué. |
| Location| ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Donne accès à la localisation actuelle. Celle-ci est obtenue à partir d’un matériel dédié, tel que le capteur GPS du PC, ou est tirée des informations disponibles sur le réseau. | 
| Microphone | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Donne accès au flux audio du microphone. Cela permet à l’application d’enregistrer à partir des microphones connectés. | 
| Médiathèque | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet d’ajouter, de modifier ou de supprimer des fichiers dans la **médiathèque** pour le PC local et des PC de **groupe résidentiel**. | 
| Objets 3D | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Fournit un accès par programme aux **objets 3D** de l’utilisateur, permettant à l’application d’énumérer tous les fichiers dans la bibliothèque et d’y accéder sans interaction de l’utilisateur. Cette fonctionnalité est généralement utilisée dans les applications et les jeux 3D qui ont besoin d’accéder à l’intégralité de la bibliothèque d’**objets 3D**. | 
| Appel téléphonique | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications d’accéder à toutes les lignes téléphoniques sur l’appareil, et d’exécuter les fonctions suivantes : passer un appel sur le téléphone et afficher le numéroteur système sans intervention de l’utilisateur ; accéder aux métadonnées liées à la ligne ; accéder à des déclencheurs liés à la ligne. Autoriser l’application de filtre antispam sélectionnée par l’utilisateur à définir et à vérifier la liste rouge et les informations sur l’origine des appels. | 
| Bibliothèque d’images | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet d’ajouter, de modifier ou de supprimer des fichiers dans la **bibliothèque d’images** pour le PC local et des PC de **groupe résidentiel**. | 
| Point de service | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Fournit l’accès aux périphériques de point de service. Cette fonctionnalité est requise pour accéder aux API de l’espace de noms [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService). | 
| Réseaux privés (client &amp; serveur) | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Fournit l’accès entrant et sortant aux réseaux intranet qui ont un contrôleur de domaine authentifié, ou que l’utilisateur a désignés comme réseaux domestiques ou professionnels. L’accès entrant aux ports critiques est toujours bloqué. | 
| Proximité | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet de se connecter à des périphériques proches du PC via la communication en champ proche (NFC, Near-Field Communication). La proximité en champ proche peut être utilisée pour envoyer des fichiers ou communiquer avec une application sur l’appareil proche. | 
| Stockage amovible | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Offre la possibilité d’ajouter, de modifier ou de supprimer des fichiers sur des appareils de stockage amovibles. L’application peut accéder uniquement aux types de fichiers sur stockage amovible qui sont définis dans le manifeste à l’aide de la déclaration **Associations de types de fichier**. L’application ne peut pas accéder au stockage amovible sur des PC de **groupe résidentiel**. | 
| Certificats utilisateur partagés | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Cette fonctionnalité est régie par la politique du Microsoft Store. Elle offre la possibilité d’accéder à des certificats logiciels et matériels, tels que des certificats de carte à puce, pour valider l’identité d’un utilisateur. Quand des API associées sont appelées en cours d’exécution, l’utilisateur doit agir (insérer une carte, sélectionner un certificat, etc.). Cette fonctionnalité n’est pas nécessaire si votre application inclut un certificat privé via une déclaration **Certificates**. | 
| Informations sur le compte d’utilisateur | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications d’accéder au nom et à l’image de l’utilisateur. Cette fonctionnalité est nécessaire pour accéder à certaines API dans l’espace de noms [**Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile). | 
| Vidéothèque | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet d’ajouter, de modifier ou de supprimer des fichiers dans la **vidéothèque** pour le PC local et des PC de **groupe résidentiel**. | 
| Appel VOIP | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Permet aux applications d’accéder aux API d’appel VoIP dans l’espace de noms [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls). | 
| Webcam | ![Disponible dans le concepteur de manifeste](images/ap-tools.png) | Fournit l’accès à l’appareil photo intégré ou au flux vidéo de la webcam connectée. Cela permet à l’application de capturer des instantanés et des films. | 
| USB | | Donne accès aux périphériques USB personnalisés. Cette fonctionnalité nécessite des éléments enfants. Cette fonctionnalité n’est pas prise en charge sur Windows Phone. | 
| Périphérique d’interface utilisateur (HID) | | Donne accès aux périphériques d’interface utilisateur (HID). Cette fonctionnalité nécessite des éléments enfants. Pour plus d’informations, voir [Comment spécifier des fonctionnalités de périphérique pour un périphérique d’interface utilisateur (HID)](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid). | 
| Bluetooth GATT | | Donne accès aux périphériques Bluetooth LE via un ensemble de services principaux, de services inclus, de caractéristiques et de descripteurs. Cette fonctionnalité nécessite des éléments enfants. Pour plus d’informations, voir [Comment spécifier des fonctionnalités de périphérique pour Bluetooth](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth). | 
| Bluetooth RFCOMM |  | Donne accès à des API qui prennent en charge le transport BR/EDR (Basic Rate/Extended Data Rate) et permet également à votre application UWP d’accéder à un périphérique qui implémente SPP (Serial Port Profile). Cette fonctionnalité nécessite des éléments enfants. Pour plus d’informations, voir [Comment spécifier des fonctionnalités de périphérique pour Bluetooth](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth). |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>Utiliser l’API Windows Runtime pour communiquer avec votre appareil

Le tableau suivant connecte certaines fonctionnalités à des API de Windows Runtime.

| Fonctionnalité du périphérique        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) | 
| Messages de conversation bloqués    | [**Windows.ApplicationModel.CommunicationBlocking**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.CommunicationBlocking) | 
| Location                 | Pour plus d’informations, voir [Vue d’ensemble des cartes et de la localisation](https://docs.microsoft.com/windows/uwp/maps-and-location/index). | 
| Appel téléphonique               | [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) | 
| Informations sur le compte d’utilisateur | [**Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile) | 
| Appel VOIP             | [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) | 
| USB                      | [**Windows.Devices.Usb**](https://docs.microsoft.com/uwp/api/Windows.Devices.Usb) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.HumanInterfaceDevice) | 
| Bluetooth GATT           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) | 
| Bluetooth RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm) | 
| Point de service         | [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) |

