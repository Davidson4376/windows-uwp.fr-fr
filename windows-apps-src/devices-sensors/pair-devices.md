---
author: muhsinking
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: Jumeler des appareils
description: Pour pouvoir être utilisés, certains appareils doivent être jumelés. L’espace de noms Windows.Devices.Enumeration prend en charge troisméthodes différentes de jumelage des appareils.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 64f4756df37cbfaf041e432b7e4a890123f52d2f
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5945752"
---
# <a name="pair-devices"></a>Coupler des appareils



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Pour pouvoir être utilisés, certains appareils doivent être jumelés. L’espace de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) prend en charge troisméthodes différentes de jumelage des appareils.

-   Jumelage automatique
-   Jumelage de base
-   Jumelage personnalisé

**Conseil**certains appareils n’avez pas besoin d’être jumelés pour être utilisé. Ce sujet est abordé dans la section sur le jumelage automatique.

 

## <a name="automatic-pairing"></a>Jumelage automatique


Parfois, vous souhaitez utiliser un appareil dans votre application, sans pour autant chercher à le jumeler. Vous souhaitez simplement être en mesure d’utiliser la fonctionnalité associée à l’appareil. Par exemple, si votre application est dédiée à la capture d’images d’une webcam, ce n’est pas nécessairement l’appareil qui vous intéresse, mais plutôt la capture d’image. Si des API dédiées à l’appareil qui vous intéresse sont disponibles, vous êtes soumis au jumelage automatique.

Le cas échéant, il vous suffit d’utiliser les API associées à l’appareil afin de transmettre les appels nécessaires, et de vous appuyer sur le système pour l’exécution des jumelages nécessaires. Pour utiliser la fonctionnalité de certains appareils, il n’est pas forcément nécessaire de procéder un jumelage. Si aucun jumelage n’est nécessaire, les API d’appareil exécutent en arrière-plan l’action de jumelage. Ainsi, vous n’êtes pas tenu d’intégrer cette fonctionnalité dans votre application. Votre application ne recevra aucune information relative au jumelage des appareils, ce qui ne vous empêchera pas d’accéder à ces derniers et d’utiliser leur fonctionnalité.

## <a name="basic-pairing"></a>Jumelage de base


Lors d’un jumelage de base, votre application utilise les API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) pour tenter de jumeler l’appareil. Dans ce scénario, vous laissez Windows exécuter et gérer le processus de jumelage. Si une interaction utilisateur est nécessaire, elle est gérée par Windows. Vous devez utiliser le jumelage de base si vous devez effectuer un jumelage avec un appareil, et qu’il n’existe pas d’API d’appareil dédiée aux tentatives de jumelage automatique. Vous souhaitez seulement utiliser l’appareil et devez d’abord effectuer un jumelage avec ce dernier.

Pour tenter de procéder à un jumelage de base, vous devez d’abord obtenir l’objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) associé à l’appareil qui vous intéresse. Une fois que vous recevez cet objet, vous interagissez avec la propriété [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx), qui est un objet [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx). Pour procéder à un jumelage, il vous suffit d’appeler [**DeviceInformationPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/mt608800). Il vous faudra **await** le résultat, ceci pour octroyer à votre application le temps nécessaire à la tentative de jumelage. Le résultat de l’action de jumelage est alors renvoyé. Si aucune erreur n’est identifiée, l’appareil est jumelé.

Si vous avez recours au jumelage de base, vous disposez également d’un accès à des informations supplémentaires sur l’état de l’appareil. Par exemple, vous connaissez l’état de jumelage ([**IsPaired**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) et savez si l’appareil peut être jumelé ([**CanPair**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair)). Ces deux données sont des propriétés de l’objet [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx). Si vous utilisez le jumelage automatique, vous n’aurez pas forcément accès à ces informations, sauf si vous obtenez les objets [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) appropriés.

## <a name="custom-pairing"></a>Jumelage personnalisé


Grâce au jumelage personnalisé, votre application peut prendre part au processus de jumelage. Dès lors, votre application peut spécifier les éléments [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) qui sont pris en charge pour le processus de jumelage. Vous êtes également chargé de créer votre propre interface utilisateur pour interagir avec l’utilisateur au besoin. Utilisez le jumelage personnalisé lorsque vous souhaitez que votre application possède un peu plus d’influence sur l’exécution du processus de jumelage ou pour afficher votre propre interface utilisateur de jumelage.

Pour implémenter le jumelage personnalisé, vous devez obtenir l’objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) associé à l’appareil qui vous intéresse, tout comme avec le jumelage de base. Toutefois, la propriété spécifique qui vous intéresse est [**DeviceInformation.Pairing.Custom**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.custom.aspx). Cela vous octroie un objet [**DeviceInformationCustomPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.aspx). L’ensemble des méthodes [**DeviceInformationCustomPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairasync.aspx) vous demandent d’inclure un paramètre [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808). Il indique les actions que l’utilisateur devra effectuer pour tenter de jumeler l’appareil. Consultez la page de référence **DevicePairingKinds** pour en savoir plus sur les différents types d’action que l’utilisateur devra effectuer. Tout comme avec le jumelage de base, il vous faudra **await** le résultat afin d’octroyer à votre application le temps nécessaire à la procédure de jumelage. Le résultat de l’action de jumelage est alors renvoyé. Si aucune erreur n’est identifiée, l’appareil est jumelé.

Pour prendre en charge le jumelage personnalisé, il vous faudra créer un gestionnaire pour l’événement [**PairingRequested**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested.aspx). Ce gestionnaire doit vérifier qu’il prend en compte l’ensemble des éléments [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) pouvant être utilisés dans un scénario de jumelage personnalisé. L’action appropriée à entreprendre dépend de l’élément **DevicePairingKinds** fourni avec les arguments de l’événement.

Il est important de garder à l’esprit que le jumelage personnalisé est toujours une opération de niveau système. Pour cette raison, lorsque vous utilisez un ordinateur de bureau ou un appareil Windows Phone, une boîte de dialogue système apparaît juste avant l’opération de jumelage. En effet, ces deux plateformes valorisent une expérience utilisateur qui nécessite le consentement de l’utilisateur. Dans la mesure où cette boîte de dialogue est automatiquement générée, vous n’aurez pas besoin de créer votre propre boîte de dialogue lorsque vous sélectionnez un élément [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) de **ConfirmOnly** avec ces plateformes. Pour les autres **DevicePairingKinds**, vous devrez exécuter certaines opérations spécifiques de gestion, en fonction de la valeur **DevicePairingKinds** spécifique. Pour savoir comment gérer le jumelage personnalisé associé à différentes valeurs **DevicePairingKinds**, voir les exemples.

## <a name="unpairing"></a>Annulation du jumelage


L’opération d’annulation s’applique uniquement aux scénarios de jumelage de base ou personnalisé décrits plus haut. Si vous utilisez le jumelage automatique, les informations d’état de jumelage de l’appareil ne sont pas transmises à l’application ; aucune annulation de jumelage n’est nécessaire. Les processus d’annulation des jumelages de base et personnalisé sont identiques. Pour quelle raison ? Il n’est pas nécessaire de fournir d’informations supplémentaires ou d’interagir dans le processus d’annulation de jumelage.

Si vous souhaitez annuler le jumelage d’un appareil, vous devez commencer par obtenir l’objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) qui lui est associé. Vous devez ensuite récupérer la propriété [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) et appeler [**DeviceInformationPairing.UnpairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.unpairasync). Comme c’était le cas avec le jumelage, il vous faudra **await** le résultat. Le résultat de l’action d’annulation de jumelage est renvoyé. Si aucune erreur n’est identifiée, le jumelage de l’appareil est annulé.

## <a name="sample"></a>Exemple


Pour télécharger un exemple illustrant comment utiliser les API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), cliquez [ici](http://go.microsoft.com/fwlink/?LinkID=620536).

 

 
