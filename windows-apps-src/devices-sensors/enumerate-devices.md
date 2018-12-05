---
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: Énumérer les appareils
description: L’espace de noms d’énumération vous permet de rechercher des appareils connectés au système, en interne, en externe ou détectables sur les protocoles sans fil ou réseau.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f6348cc713d4fb93dfed9310eea9d3fd1025a2de
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8713979"
---
# <a name="enumerate-devices"></a>Énumérer les appareils


## <a name="samples"></a>Exemples

Le moyen le plus simple d’énumérer tous les appareils disponibles consiste à prendre un instantané avec la commande [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) (expliquée plus en détail dans la section ci-dessous).

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

Pour télécharger un exemple illustrant des utilisations plus avancées des API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), cliquez [ici](http://go.microsoft.com/fwlink/?LinkID=620536).

## <a name="enumeration-apis"></a>API d’énumération

L’espace de noms d’énumération vous permet de rechercher des appareils connectés au système, en interne, en externe ou détectables sur les protocoles sans fil ou réseau. Les API que vous utilisez pour énumérer les appareils possibles sont l’espace de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459). Voici quelques raisons d’utiliser ces API.

-   Recherche d’un appareil à connecter à votre application.
-   Obtention d’informations sur les appareils connectés au système ou détectables par celui-ci.
-   Réception par une application de notifications en cas d’ajout, de connexion, de déconnexion, de changement de statut de connexion ou de modification d’autres propriétés d’appareils.
-   Réception par une application de déclencheurs d’arrière-plan en cas de connexion, de déconnexion, de changement de statut de connexion ou de modification d’autres propriétés d’appareils.

Ces API peuvent énumérer les appareils sur l’un des protocoles et bus suivants, à condition que l’appareil et le système exécutant l’application prennent en charge cette technologie. Cette liste n’est pas exhaustive. D’autres protocoles peuvent être pris en charge par un appareil spécifique.

-   Bus connectés physiquement. Cela inclut les bus PCI et USB. Par exemple, tout ce que vous pouvez voir dans le **Gestionnaire de périphériques**.
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   Digital Living Network Alliance (DLNA)
-   [**Discovery and Launch (DIAL)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**Détection de service DNS (DNS-SD)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [Web Services on Devices (WSD)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [Bluetooth](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**Wi-Fi Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**Point de service**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Dans de nombreux cas, vous ne devez pas vous soucier de l’utilisation des API d’énumération. En effet, de nombreuses API qui utilisent des appareils sélectionnent automatiquement l’appareil par défaut approprié ou fournissent une API d’énumération plus simplifiée. Par exemple, [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) utilise automatiquement le récepteur audio par défaut. Tant que votre application peut utiliser l’appareil par défaut, il est inutile d’utiliser les API d’énumération dans l’application. Les API d’énumération constituent une solution générale et souple pour détecter les appareils disponibles et s’y connecter. Cette rubrique fournit des informations sur l’énumération des appareils et décrit les quatre méthodes courantes pour énumérer les appareils.

-   Utilisation de l’interface utilisateur [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)
-   Énumération d’une capture instantanée d’appareils actuellement détectables par le système
-   Énumération d’appareils actuellement détectables et observation des modifications
-   Énumération d’appareils actuellement détectables et observation des modifications dans une tâche en arrière-plan

## <a name="deviceinformation-objects"></a>Objets DeviceInformation


Lorsque vous utilisez les API d’énumération, vous devez fréquemment utiliser des objets [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Ces objets contiennent la plupart des informations disponibles sur l’appareil. Le tableau suivant présente certaines des propriétés **DeviceInformation** intéressantes. Pour obtenir la liste complète, consultez la page de référence pour **DeviceInformation**.

| Propriété                         | Commentaires                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | Ceci est l’identificateur unique de l’appareil, fourni en tant que variable de chaîne. Dans la plupart des cas, il s’agit d’une valeur opaque que vous allez simplement transmettre d’une méthode à l’autre pour indiquer l’appareil spécifique qui vous intéresse. Vous pouvez également utiliser la propriété **DeviceInformation.Kind** après la fermeture puis la réouverture de votre application. Cela garantit la possibilité de récupérer et réutiliser le même objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). |
| **DeviceInformation.Kind**       | Cela indique le genre d’objet appareil représenté par l’objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Il ne s’agit pas de la catégorie d’appareil ou du type d’appareil. Un seul appareil peut être représenté par plusieurs objets **DeviceInformation** de genres différents. Les valeurs possibles pour cette propriété sont répertoriées dans [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx), de même que la manière dont elles sont associées.                           |
| **DeviceInformation.Properties** | Ce conteneur des propriétés contient des informations demandées pour l’objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Les propriétés les plus communes sont facilement référencées en tant que propriétés de l’objet **DeviceInformation**, comme avec [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name). Pour plus d’informations, voir [Propriétés d’informations d’appareil](device-information-properties.md).                                                                |

 

## <a name="devicepicker-ui"></a>Interface utilisateur DevicePicker


[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) est un contrôle fourni par Windows, qui crée une petite interface permettant à l’utilisateur de sélectionner un appareil dans une liste. Vous pouvez personnaliser la fenêtre **DevicePicker** de plusieurs façons.

-   Vous pouvez contrôler les appareils répertoriés dans l’interface utilisateur en ajoutant une valeur [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors.aspx) ou [**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses.aspx), ou les deux à [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter). Dans la plupart des cas, vous devez ajouter uniquement un sélecteur ou une classe, mais s’il vous en faut davantage, vous pouvez en ajouter plusieurs. Si vous ajoutez plusieurs classes ou sélecteurs, ils sont unis à l’aide d’une fonction logique OR.
-   Vous pouvez spécifier les propriétés que vous souhaitez récupérer pour les appareils. Pour ce faire, ajoutez des propriétés à [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties).
-   Vous pouvez modifier l’apparence de [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) à l’aide de [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance).
-   Vous pouvez spécifier la taille et l’emplacement d’affichage de [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841).

Quand [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) est affiché, le contenu de l’interface utilisateur est automatiquement mis à jour si des appareils sont ajoutés, supprimés ou mis à jour.

**Remarque**vous ne pouvez pas spécifier la [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) à l’aide de la [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841). Si vous souhaitez disposer d’appareils avec un **DeviceInformationKind** spécifique, vous devez créer un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) et fournir votre propre interface utilisateur.

 

La diffusion de contenu multimédia et DIAL fournissent également leurs propres sélecteurs si vous souhaitez les utiliser. Il s’agit respectivement de [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) et [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783).

## <a name="enumerate-a-snapshot-of-devices"></a>Énumérer une capture instantanée d’appareils


Dans certains scénarios, le [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) ne sera pas adapté à vos besoins et vous aurez besoin d’une solution plus flexible. Peut-être souhaiterez-vous créer votre propre interface utilisateur ou devrez-vous énumérer les appareils sans afficher d’interface à l’utilisateur. Dans ces situations, vous pouvez énumérer une capture instantanée d’appareils. Cette opération implique de parcourir les appareils actuellement connectés ou associés au système. Toutefois, gardez à l’esprit que cette méthode examine uniquement une capture instantanée d’appareils disponibles. Vous ne trouverez donc pas les appareils qui se connectent après l’énumération de la liste. Par ailleurs, vous ne serez pas averti si un appareil est mis à jour ou supprimé. Un autre inconvénient potentiel est que cette méthode retient tous les résultats jusqu’à ce que l’énumération soit terminée. C’est pourquoi vous ne devez pas utiliser cette méthode quand vous vous intéressez à des objets **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, dans la mesure où ils se trouvent sur un protocole réseau ou sans fil. L’opération peut prendre jusqu’à 30 secondes. Dans ce scénario, vous devez utiliser un objet [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) pour énumérer les appareils possibles.

Pour énumérer une capture instantanée d’appareils, utilisez la méthode [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx). Cette méthode attend que le processus d’énumération soit terminé et renvoie tous les résultats sous la forme d’un seul objet [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx). Cette méthode est également surchargée pour vous offrir plusieurs options de filtrage des résultats et limiter ceux-ci aux appareils qui vous intéressent. Pour ce faire, fournissez une [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) ou transmettez un sélecteur d’appareil. Le sélecteur d’appareil est une chaîne AQS qui spécifie les appareils que vous voulez énumérer. Pour plus d’informations, voir [Créer un sélecteur d’appareil](build-a-device-selector.md).

Vous trouverez ci-dessous l’exemple d’un instantané d’énumération d’appareil:



En plus de limiter les résultats, vous pouvez spécifier les propriétés à récupérer pour les appareils. Dans ce cas, les propriétés spécifiées seront disponibles dans le conteneur des propriétés pour chacun des objets [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) renvoyés dans la collection. Il est important de noter que toutes les propriétés ne sont pas disponibles pour tous les genres d’appareils. Pour voir les propriétés disponibles selon les genres d’appareils, voir [Propriétés d’informations d’appareil](device-information-properties.md).



## <a name="enumerate-and-watch-devices"></a>Énumérer et observer des appareils


Une méthode plus puissante et plus flexible d’énumération des appareils consiste à créer un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446). Cette option offre davantage de souplesse lors de l’énumération d’appareils. Elle vous permet d’énumérer les appareils actuellement présents, ainsi que de recevoir des notifications lorsque des appareils correspondant à votre sélecteur d’appareil sont ajoutés, supprimés, ou lorsque des propriétés sont modifiées. Lorsque vous créez un **DeviceWatcher**, vous fournissez un sélecteur d’appareil. Pour plus d’informations sur les sélecteurs d’appareil, voir [Créer un sélecteur d’appareil](build-a-device-selector.md). Après avoir créé l’observateur, vous recevrez les notifications suivantes pour tout appareil correspondant aux critères fournis.

-   Notification d’ajout lorsqu’un nouvel appareil est ajouté.
-   Notification de mise à jour lorsqu’une propriété qui vous intéresse est modifiée.
-   Notification de suppression lorsqu’un appareil n’est plus disponible ou ne correspond plus à votre filtre.

Dans la plupart des cas où vous utilisez un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), vous maintenez une liste des appareils, dans laquelle des éléments sont ajoutés, supprimés ou mis à jour à mesure que l’observateur reçoit des mises à jour des appareils que vous observez. Lorsque vous recevez une notification de mise à jour, les informations mises à jour sont disponibles sous la forme d’un objet [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationupdate.aspx). Pour mettre à jour votre liste d’appareils, commencez par rechercher les [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) appropriées qui ont changé. Ensuite, appelez la méthode [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update) pour cet objet, en fournissant l’objet **DeviceInformationUpdate**. Il s’agit d’une fonction pratique qui met automatiquement à jour votre objet **DeviceInformation**.

Étant donné qu’un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) envoie des notifications à mesure que des appareils arrivent et lorsqu’ils sont modifiés, vous devez utiliser cette méthode d’énumération des appareils lorsque vous vous intéressez à des objets **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService** car ils sont énumérés sur les protocoles réseau ou sans fil.

Pour créer un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), utilisez l’une des méthodes [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx). Ces méthodes sont surchargées pour vous permettre de spécifier les appareils qui vous intéressent. Pour ce faire, fournissez une [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) ou transmettez un sélecteur d’appareil. Le sélecteur d’appareil est une chaîne AQS qui spécifie les appareils que vous voulez énumérer. Pour plus d’informations, voir [Créer un sélecteur d’appareil](build-a-device-selector.md). Vous pouvez également spécifier les propriétés à récupérer pour les appareils et qui vous intéressent. Dans ce cas, les propriétés spécifiées seront disponibles dans le conteneur des propriétés pour chacun des objets [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) renvoyés dans la collection. Il est important de noter que toutes les propriétés ne sont pas disponibles pour tous les genres d’appareils. Pour voir les propriétés disponibles selon les genres d’appareils, voir [Propriétés d’informations d’appareil](device-information-properties.md)

## <a name="watch-devices-as-a-background-task"></a>Observer des appareils en tant que tâche en arrière-plan


Observer des appareils en tant que tâche en arrière-plan est très similaire à la création d’un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) comme décrit ci-dessus. En fait, vous devrez quand même en premier lieu créer un objet **DeviceWatcher** normal comme décrit dans la section précédente. Une fois la création effectuée, vous appelez [**GetBackgroundTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) à la place de [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start). Lorsque vous appelez **GetBackgroundTrigger**, vous devez spécifier les notifications qui vous intéressent : ajout, suppression ou mise à jour. Vous ne pouvez pas demander la mise à jour ou la suppression sans demander l’ajout. Une fois que vous inscrivez le déclencheur, le **DeviceWatcher** commence immédiatement à s’exécuter en arrière-plan. À partir de là, chaque fois qu’il reçoit une nouvelle notification pour votre application qui correspond à vos critères, la tâche en arrière-plan se déclenche et vous recevez les dernières modifications apportées depuis le dernier déclenchement de votre application.

**Important**la première fois qu’un [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) déclenche votre application survient lorsque l’observateur est atteint l’état **EnumerationCompleted** . Cela signifie qu’il contient tous les résultats initiaux. Les fois suivantes où il déclenche votre application, il ne contient que les notifications d’ajout, de mise à jour et de suppression qui ont eu lieu depuis le dernier déclencheur. C’est légèrement différent d’un objet [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) de premier plan car les résultats initiaux n’arrivent pas un à un et sont uniquement livrés de manière groupée une fois que l’état **EnumerationCompleted** est atteint.

 

Certains protocoles sans fil se comportent différemment selon qu’ils analysent en arrière-plan ou au premier plan, ou ils peuvent ne pas du tout prendre en charge l’analyse en arrière-plan. L’analyse en arrière-plan présente trois possibilités. Le tableau suivant répertorie les possibilités et les effets qu’elles peuvent produire sur votre application. Par exemple, Bluetooth et Wi-Fi Direct ne pas prennent en charge les analyses en arrière-plan, et par conséquent, ne gèrent pas l’objet [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838).

| Comportement                                  | Impact                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Même comportement en arrière-plan               | Aucun                                                                                                                                    |
| Seules les analyses passives sont possibles en arrière-plan | La détection d’appareils peut être plus longue pendant l’attente d’une analyse passive.                                                           |
| Les analyses en arrière-plan ne sont pas prises en charge.            | Aucun appareil ne sera détecté par le [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838), et aucune mise à jour ne sera signalée. |

 

Si votre [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) inclut un protocole qui ne prend pas en charge l’analyse en tant que tâche en arrière-plan, votre déclencheur continuera à fonctionner. Toutefois, vous ne serez pas en mesure d’obtenir les mises à jour ou les résultats sur ce protocole. Les mises à jour pour d’autres protocoles ou appareils seront toujours détectées normalement.

## <a name="using-deviceinformationkind"></a>Utilisation de DeviceInformationKind


Dans la plupart des scénarios, vous n’aurez pas à vous soucier du [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) d’un objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). En effet, le sélecteur d’appareil renvoyé par l’API d’appareil que vous utilisez garantit souvent les genres d’objets appareil à utiliser avec leur API. Toutefois, dans certains scénarios, vous souhaiterez obtenir les **DeviceInformation** des appareils, mais une API d’appareil correspondante ne sera pas disponible pour fournir un sélecteur d’appareil. Dans ces cas, vous devrez créer votre propre sélecteur. Par exemple, Web Services on Devices ne dispose pas d’une API dédiée, mais vous pouvez détecter ces appareils et obtenir des informations les concernant à l’aide des API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), puis les utiliser à l’aide des API de socket.

Si vous créez votre propre sélecteur d’appareil pour énumérer des objets appareil, il est primordial pour vous de comprendre [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx). Tous les genres possibles, de même que la manière dont ils sont associés, sont décrits sur la page de référence de **DeviceInformationKind**. L’une des utilisations les plus courantes de **DeviceInformationKind** consiste à spécifier quels genres d’appareils vous recherchez lors de l’envoi d’une requête en conjonction avec un sélecteur d’appareil. Cela garantit que vous énumérez uniquement sur les appareils correspondant à la valeur **DeviceInformationKind** fournie. Par exemple, vous pouvez trouver un objet **DeviceInterface**, puis exécuter une requête afin d’obtenir des informations relatives à l’objet **Device** parent. Cet objet parent peut contenir des informations supplémentaires.

Il est important de noter que les propriétés disponibles dans le conteneur des propriétés pour un objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) varient selon le [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) de l’appareil. Certaines propriétés sont disponibles uniquement avec certains genres. Pour plus d’informations sur les propriétés disponibles pour les différents genres, voir [Propriétés d’informations d’appareil](device-information-properties.md). Par conséquent, dans l’exemple ci-dessus, la recherche du parent **Device** vous donnera accès à des informations supplémentaires qui n’étaient pas disponibles à partir de l’objet appareil **DeviceInterface**. Pour cette raison, lorsque vous créez vos chaînes de filtre AQS, il est important de vérifier que les propriétés demandées sont disponibles pour les objets **DeviceInformationKind** que vous énumérez. Pour plus d’informations sur la création d’un filtre, voir [Créer un sélecteur d’appareil](build-a-device-selector.md).

Lors de l’énumération d’objets **AssociationEndpoint**, **AssociationEndpointContainer** ou **AssociationEndpointService**, vous énumérez sur un protocole sans fil ou réseau. Dans ces situations, nous vous recommandons de ne pas utiliser [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) mais plutôt [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx). En effet, effectuer des recherches sur un réseau entraîne souvent des opérations de recherche qui n’expirent pas pendant au moins 10 secondes avant de générer [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx). **FindAllAsync** n’achève pas son opération tant que **EnumerationCompleted** n’est pas déclenché. Si vous utilisez un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), vous obtenez des résultats plus proches du temps réel, quel que soit le moment d’appel de **EnumerationCompleted**.

## <a name="save-a-device-for-later-use"></a>Enregistrer un appareil en vue d’une utilisation ultérieure


Tout objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) est identifié de façon unique par une combinaison de deux éléments d’information : [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) et [**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx). Si vous conservez ces deux éléments d’information, vous pouvez recréer un objet **DeviceInformation** après sa perte en fournissant ces informations à [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/br225425.aspx). Dans ce cas, vous pouvez enregistrer les préférences utilisateur pour un appareil qui s’intègre à votre application.


 

 




