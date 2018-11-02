---
title: Types de périphériques
description: Les types de périphériques Direct3D comprennent les périphériques de couche d’abstraction matérielle (HAL) et le module de rastérisation de référence.
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- Types de périphériques
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cbf7d984226984391da340c74791dad4a6c0d8fb
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5925909"
---
# <a name="device-types"></a>Types de périphériques


Les types de périphériques Direct3D comprennent les périphériques de couche d’abstraction matérielle (HAL) et le module de rastérisation de référence.

## <a name="span-idhaldevicespanspan-idhaldevicespanspan-idhaldevicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>Périphérique HAL


Le type de périphérique principal est le périphérique HAL, qui prend en charge la rastérisation accélérée matérielle, ainsi que les traitements de vertex matériel et logiciel. Si l’ordinateur sur lequel votre application s’exécute est équipé d’une carte vidéo prenant en charge Direct3D, votre application doit utiliser cette carte pour les opérations Direct3D. Les périphériques HAL Direct3D implémentent une partie ou la totalité des modules de transformation, d’éclairage et de rastérisation dans le matériel.

Les applications n’accèdent pas directement aux cartes graphiques. Elles appellent des fonctions et méthodes Direct3D. Direct3D accède au matériel par le biais de la couche d’abstraction matérielle. Si l’ordinateur sur lequel s’exécutent vos applications prend en charge la couche HAL, il bénéficiera de performances optimales si vous utilisez un périphérique HAL.

## <a name="span-idreferencedevicespanspan-idreferencedevicespanspan-idreferencedevicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>Périphérique de référence


Direct3D prend en charge un type de périphérique supplémentaire appelé périphérique de référence ou module de rastérisation de référence. Contrairement à un périphérique logiciel, le module de rastérisation de référence prend en charge chacune des fonctionnalités Direct3D. Ce périphérique est destiné à être utilisé à des fins de débogage et n’est donc disponible que sur les ordinateurs équipés du Kit de développement logiciel (SDK) DirectX. Étant donné que ces fonctionnalités sont axées sur la précision plutôt que sur la vitesse et qu’elles sont implémentées dans le logiciel, elles n’offrent pas des résultats très rapides. Le module de rastérisation de référence utilise des instructions de processeur spéciales chaque fois qu’il le peut, mais il n’est pas conçu pour les applications vendues au détail. N’utilisez le module de rastérisation de référence qu’à des fins de test ou de démonstration de fonctionnalités.

## <a name="span-idhalvsrefspanspan-idhalvsrefspanspan-idhalvsrefspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>Comparatif des périphériques HAL et REF


Les périphériques HAL (couche d’abstraction matérielle) et REF (module de rastérisation de référence) sont les deux principaux types de périphériques Direct3D. Le premier type s’articule autour de la prise en charge matérielle et se révèle d’une grande rapidité, mais risque de ne pas prendre en charge certaines fonctions. Le second type n’utilise aucune accélération matérielle et est donc particulièrement lent, mais prend correctement en charge l’ensemble complet de fonctionnalités Direct3D. Dans la plupart des cas, vous pourrez vous contenter d’utiliser des périphériques HAL, mais si vous souhaitez exécuter une fonctionnalité avancée non prise en charge par votre carte graphique, vous pourriez avoir besoin de recourir à un périphérique REF.

L’autre cas dans lequel l’utilisation d’un périphérique REF peut se révéler utile est celui où le périphérique HAL produit des résultats inattendus (lorsque vous avez la certitude que votre code est correct, mais que vous n’obtenez pas le résultat escompté). Le comportement du périphérique REF étant d’une grande fiabilité, vous pouvez tester votre application sur ce dernier pour voir si vous continuez à obtenir des résultats inattendus. Si tel n’est pas le cas, cela signifie (a) que votre application suppose que la carte graphique prend en charge une fonctionnalité non gérée en réalité, ou (b) qu’il s’agit d’un bogue de pilote. Si le problème persiste avec le périphérique REF, il découle d’un bogue de l’application.

## <a name="span-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>Traitement de vertex matériel ou logiciel


Le choix entre un traitement de vertex matériel et un traitement logiciel ne s’applique réellement qu’aux périphériques HAL. Lorsque vous transmettez des vertex par le biais du pipeline, ces vertex doivent être transformés (par les matrices de modèle, de vue et de projection successivement) et éclairés (par des éclairagesD3D intégrés). Cette étape est désignée sous l’acronyme T&L (Transformation & Lighting, transformation et éclairage). Le traitement de vertex matériel signifie que cette opération est effectuée dans le matériel, si ce dernier la prend en charge; de la même façon, le traitement de vertex logiciel se produit dans le logiciel. L’approche générale consiste à commencer par essayer de créer un périphérique T&L matériel, puis si cette opération échoue, à créer un périphérique mixte, et en cas de nouvel échec, à créer un périphérique logiciel. (Si le traitement logiciel échoue, l’opération est abandonnée et prend fin avec un message d’erreur.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Appareils](devices.md)

 

 




