---
title: Configurer la console de développement Xbox
description: Découvrez comment configurer votre console de développement Xbox pour prendre en charge le développement Xbox Live.
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 479be2401e0c54801645ad1c0d91b11b7ffb6869
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649014"
---
# <a name="configure-your-xbox-development-console"></a>Configurer la console de développement Xbox

À la configuration de votre console de développement :
- Obtenir votre ID
- Définir votre bac à sable sur vos kits de développement
- Connectez-vous avec un compte de développement

## <a name="get-your-ids"></a>Obtenir votre ID
Pour activer les bacs à sable et les services Xbox Live, vous devrez obtenir plusieurs ID pour configurer votre kit de développement et votre titre. Il est possible avec le même processus.

Suivez [configuration du service Xbox Live](../xbox-live-service-configuration.md) pour obtenir votre ID

## <a name="set-your-sandbox-on-your-development-kits"></a>Définir votre bac à sable sur vos kits de développement
Vous ne serez pas en mesure de démarrer votre kit de développement sans avoir à définir votre ID de bac à sable. Pour ce faire, vous pouvez utiliser le « Xbox un responsable » qui est installé sur votre PC par le XDK, ou vous pouvez ouvrir une fenêtre de commande XDK et utilisez la commande de Configuration (xbconfig.exe) comme suit :

Vérifiez votre bac à sable en cours. Tapez xbconfig sandboxid à l’invite de commandes.

Si elle n’est pas ce que vous attendez, modifier votre id de bac à sable. Tapez xbconfig sandboxid =<your sandbox id> à l’invite de commandes.

Redémarrez votre console à l’aide de redémarrage (xbreboot.exe) à l’invite de commandes.

Vérifiez que votre bac à sable a été correctement réinitialisé. Tapez xbconfig sandboxid à l’invite de commandes.

## <a name="sign-in-with-a-development-account"></a>Connectez-vous avec un compte de développement

Vous pouvez créer des comptes de développement utilisés pour se connecter [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) ou [Partner Center](https://partner.microsoft.com/dashboard)
