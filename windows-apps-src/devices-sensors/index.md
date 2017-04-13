---
author: DBirtolo
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: Appareils, capteurs et alimentation
description: "Pour offrir une expérience riche à vos utilisateurs, vous serez peut-être amené à intégrer des appareils ou capteurs externes dans votre application."
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 7915f626d5e86aed0c47f4d2f1efc61348385ba8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="devices-sensors-and-power"></a>Appareils, capteurs et alimentation

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Pour offrir une expérience riche à vos utilisateurs, vous serez peut-être amené à intégrer des appareils ou capteurs externes dans votre application. Voici quelques exemples des fonctionnalités que vous pouvez ajouter à votre application à l’aide de la technologie décrite dans cette section.

-   Fourniture d’une expérience d’impression améliorée
-   Intégration de capteurs de mouvement et d’orientation dans votre jeu
-   Connexion directe à un appareil ou via un protocole réseau

| Rubrique | Description |
|-------|-------------|
| [Activer les fonctionnalités d’un appareil](enable-device-capabilities.md) | Ce didacticiel décrit comment déclarer des fonctionnalités d’appareil dans Microsoft Visual Studio. Votre application peut ainsi utiliser des appareils photo, des microphones, des capteurs d’emplacement et d’autres appareils. | 
| [Activer l’accès en mode utilisateur sur WindowsIoT](enable-usermode-access.md) | Ce didacticiel décrit comment activer l’accès en mode utilisateur à GPIO, I2C, SPI et UART sur Windows10IoTStandard. |
| [Énumérer les appareils](enumerate-devices.md) | L’espace de noms d’énumération vous permet de rechercher des appareils connectés au système, en interne, en externe ou détectables sur les protocoles sans fil ou réseau. |
| [Jumeler des appareils](pair-devices.md) | Pour pouvoir être utilisés, certains appareils doivent être jumelés. L’espace de noms [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459) prend en charge trois méthodes différentes de jumelage des appareils. |
| [Couplage hors-bande](out-of-band-pairing.md) | Cette section explique comment utiliser le couplage hors-bande pour permettre aux applications de se connecter à certains appareils sans qu’une découverte soit requise. | 
| [Capteurs](sensors.md) | Les capteurs permettent à votre application de connaître la relation entre un appareil et le monde physique qui l’entoure. Ils peuvent indiquer à votre application la direction, l’orientation et le mouvement de l’appareil. |
| [Bluetooth](bluetooth.md) | Cette section contient des articles sur l’intégration du Bluetooth aux applications de la plateforme Windows universelle (UWP), notamment sur l’utilisation des API RFCOMM, GATT et des publications Bluetooth Low Energy (LE). | 
| [Impression et numérisation](printing-and-scanning.md) | Cette section décrit comment imprimer et numériser à partir de votre application Windows universelle. | 
| [Impression3D](3d-printing.md) | Cette section explique comment utiliser la fonctionnalité d’impression 3D dans votre application Windows universelle. |
| [Créer une application de carte à puce NFC](host-card-emulation.md) | Auparavant, Windows Phone 8.1 prenait en charge les applications d’émulation de carte NFC à l’aide d’un élément sécurisé sur carte SIM, mais ce modèle nécessitait le couplage fort d’applications de paiement sécurisé avec les opérateurs de réseau mobile. Cette configuration éliminait de facto le recours aux solutions de paiement proposées par d’autres négociants ou développeurs ne présentant aucun couplage avec les opérateurs de réseau mobile. Dans Windows 10 Mobile, nous avons introduit une nouvelle technologie d’émulation de carte, appelée HCE (Host Card Emulation, émulation de carte hôte). Grâce à la technologie HCE, votre application peut directement interagir avec un lecteur de cartes NFC. Cette rubrique illustre le fonctionnement de la technologie HCE sur les appareils Windows 10 Mobile et vous explique comment développer une application HCE permettant à vos clients d’accéder à vos services sur leur téléphone, plutôt que via une carte physique, sans aucune collaboration avec un opérateur de réseau mobile. |
| [Obtenir des informations sur la batterie](get-battery-info.md) | Découvrez comment obtenir des informations détaillées sur la batterie à l’aide des API de l’espace de noms [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017). |

