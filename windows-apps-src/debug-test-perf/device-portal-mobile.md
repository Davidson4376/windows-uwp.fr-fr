---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal pour appareils mobiles
description: Découvrez comment Windows Device Portal vous permet de configurer et de gérer à distance votre appareil mobile.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portail de périphérique
ms.localizationpriority: medium
ms.openlocfilehash: 0531cbefef509f7bc323829031b366bec3c798d8
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2796461"
---
# <a name="device-portal-for-mobile"></a>Device Portal pour appareils mobiles

À partir de Windows10 version1511, des fonctionnalités de développement supplémentaires sont disponibles pour la famille d’appareils mobiles. Ces fonctionnalités sont disponibles uniquement lorsque le mode développeur est activé sur l’appareil.

Pour en savoir plus sur la façon d’activer le mode développeur, consultez [Activer votre appareil pour le développement](../get-started/enable-your-device-for-development.md).

![Paramètres de Device Portal](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>Configurer Device Portal sur Windows Phone

### <a name="turn-on-device-discovery-and-pairing"></a>Activer la détection et le couplage d’appareils

Pour vous connecter à Device Portal, vous devez activer la détection d’appareils et Device Portal dans les paramètres de votre téléphone. Cela vous permet de coupler votre téléphone à un PC ou à un autre appareil Windows10. Les deux appareils doivent être connectés au même sous-réseau du réseau par une connexion filaire ou sans fil, ou ils doivent être connectés par USB.

La première fois que vous vous connectez à Device Portal, vous êtes invité à entrer un code de sécurité à 6 caractères (avec respect de la casse). Cela garantit votre accès au téléphone et vous préserve des attaques. Appuyez sur le bouton Coupler de votre téléphone pour générer et afficher le code, puis entrez les 6caractères dans la zone de texte du navigateur.

![Paramètres de détection d’appareils en mode développeur](images/device-portal/mob-dev-mode-pairing.png)

Vous pouvez vous connecter à Device Portal de 3façons: USB, hôte local et sur le réseau local (y compris avec VPN et fonction modem).

**Pour se connecter à Device Portal**

1. Dans votre navigateur, entrez l’adresse indiquée ici selon le type de connexion que vous utilisez.

    - USB: `http://127.0.0.1:10080`

    utilisez cette adresse lorsque le téléphone est connecté à un PC par le biais d’une connexion USB. Les deux appareils doivent disposer de Windows10 version1511 ou ultérieure.
    
    - Hôte local: `http://127.0.0.1`

    utilisez cette adresse pour afficher Device Portal localement sur le téléphone dans Microsoft Edge pour Windows10 Mobile.
    
    - Réseau local: `https://<The IP address or hostname of the phone>`

    utilisez cette adresse pour établir la connexion par le biais d’un réseau local.

    L’adresse IP du téléphone est affichée dans les paramètres Device Portal sur le téléphone. Une connexion HTTPS est requise pour l’authentification et la communication sécurisée. Le nom d’hôte (modifiable dans Paramètres> Système > À propos de) peut également être utilisé pour accéder au portail d’appareil sur le réseau local (par exemple http://Phone360), ce qui est utile pour les appareils qui peuvent changer fréquemment de réseau ou d’adresse IP, ou qui doivent être partagés). 

2. Appuyez sur le bouton Coupler de votre téléphone pour générer et afficher le code de sécurité requis.

3. Entrez le code de sécurité à 6caractères dans la zone de mot de passe de Device Portal, dans votre navigateur.

4. (Facultatif) Activez la case à cocher Mémoriser mon ordinateur dans votre navigateur afin de mémoriser ce couplage.

Voici la section Device Portal de la page des paramètres du développeur sur Windows Phone.

![Paramètres de Device Portal](images/device-portal/mob-dev-mode-portal.png)

Si vous utilisez Device Portal dans un environnement protégé, comme un laboratoire de test, où vous faites confiance à tous les utilisateurs du réseau local, si aucune information personnelle n’est présente sur votre appareil et si vous présentez des exigences uniques, vous pouvez désactiver l’authentification. Cela permet une communication non chiffrée. Toute personne connaissant l’adresse IP de votre téléphone pourra le contrôler.

## <a name="tool-notes"></a>Remarques relatives à l’outil

## <a name="device-portal-pages"></a>Pages Device Portal
### <a name="processes"></a>Processus

Windows Mobile Device Portal ne permet pas d’arrêter les processus arbitraires. 

Device Portal sur les appareils mobiles propose les pages standard. Pour obtenir une description détaillée, consultez [Vue d’ensemble de WindowsDevicePortal](device-portal.md).

- Gestionnaire d’applications
- Explorateur de fichiers de l’application (outil d’exploration des stockages isolés)
- Processus
- Graphiques de performances
- Suivi des événements pour Windows (ETW)
- Suivi des performances (WPR) 
- Appareils
- Réseau

## <a name="see-also"></a>Voir aussi

* [Vue d’ensemble du portail d’appareil Windows](device-portal.md)
* [Informations de référence sur les API principales du portail d’appareilWindows](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)