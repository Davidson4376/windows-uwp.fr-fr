---
author: stevewhims
description: Le code qui s’intègre à l’appareil proprement dit et à ses capteurs implique des entrées de l’utilisateur et des sorties vers ce dernier.
title: Portage WindowsPhone Silverlight vers UWP pour le modèle d’e/s, d’appareil et d’application»
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a0041fd154a4ce32930e10e21175706e8e7ad988
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7168481"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Portage de WindowsPhone Silverlight vers UWP pour les e/s, d’appareil et modèle d’application


Rubrique précédente : [Portage du balisage XAML et de la couche interface utilisateur](wpsl-to-uwp-porting-xaml-and-ui.md).

Le code qui s’intègre à l’appareil proprement dit et à ses capteurs implique l’entrée de l’utilisateur et la sortie vers ce dernier. Il peut également impliquer le traitement des données. Néanmoins, ce code n’est généralement pas pensé comme la couche interface utilisateur ni comme la couche de données. Ce code inclut l’intégration au contrôleur de vibrations, à l’accéléromètre, au gyroscope, au microphone et au haut-parleur (qui rejoignent la reconnaissance et la synthèse vocales), à la (géo)localisation et aux modalités d’entrée telles que l’écran tactile, la souris, le clavier et le stylet.

## <a name="application-lifecycle-process-lifetime-management"></a>Cycle de vie des applications (gestion de la durée de vie des processus)

Votre application WindowsPhone Silverlight contient du code pour enregistrer et restaurer l’état de l’application et son état d’affichage pour prendre en charge est désactivée puis réactivée. Le cycle de vie des applications de plateforme Windows universelle (UWP) a similitudes avec celui des applications WindowsPhone Silverlight, dans la mesure où ils sont tous deux conçus avec le même objectif de renforcer les ressources disponibles pour l’application que l’utilisateur a choisi de le au premier plan à tout moment. Vous constaterez que votre code s’adapte assez facilement au nouveau système.

**Remarque**  en appuyant sur le bouton matériel **précédent** automatiquement des application WindowsPhone Silverlight s’arrête. Si vous appuyez sur le bouton matériel **Précédent** d’un appareil mobile, une application UWP *ne s’arrête pas* automatiquement. Au lieu de cela, l’application est suspendue, puis peut être arrêtée par la suite. Toutefois, ces détails sont transparents pour une application qui réagit de façon appropriée aux événements touchant le cycle de vie de l’application.

Une « fenêtre de réponse » correspond au laps de temps qui s’écoule entre le moment où l’application devient inactive et le moment où le système déclenche l’événement de suspension. Dans le cas d’une application UWP, ce type de fenêtre n’existe pas : l’événement de suspension est déclenché dès que l’application devient inactive.

Pour plus d’informations, voir [Cycle de vie de l’application](https://msdn.microsoft.com/library/windows/apps/mt243287).

## <a name="camera"></a>Appareil photo

Code de capture d’appareil photo WindowsPhone Silverlight utilise les classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera**ou **Microsoft.Phone.Tasks.CameraCaptureTask** . Pour porter ce code vers la plateforme Windows universelle (UWP), vous pouvez utiliser la classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). La rubrique [**CapturePhotoToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700836) contient un exemple de code. Cette méthode vous permet de capturer une photo dans un fichier de stockage, et qu’il a besoin de **microphone** et **webcam**[**fonctionnalités de l’appareil**](https://msdn.microsoft.com/library/windows/apps/dn934747) devant être définis dans le manifeste de package d’application.

Une autre option consiste à la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) , ce qui requiert également le **microphone** et **webcam**[**fonctionnalités de l’appareil**](https://msdn.microsoft.com/library/windows/apps/dn934747).

Les applications de filtre ne sont pas prises en charge pour les applications UWP.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Détection de la plateforme d’exécution de votre application

La manière d’envisager le ciblage d’application change avec Windows 10. Selon le nouveau modèle conceptuel, une application cible la plateforme Windows universelle (UWP) et s’exécute sur tous les appareils Windows. Elle peut ensuite choisir d’activer des fonctionnalités exclusives à certaines familles d’appareils. Si nécessaire, l’application a également la possibilité de restreindre son ciblage à une ou plusieurs familles d’appareils spécifiques. Pour plus d’informations sur les familles d’appareils et savoir comment déterminer les familles d’appareils à cibler, voir le [Guide des applications UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).

**Remarque**  nous vous recommandons de pas utiliser système d’exploitation ou la famille d’appareils pour détecter la présence de fonctionnalités. En règle générale, l’identification de la famille d’appareils ou du système d’exploitation actuel ne constitue pas le meilleur moyen de déterminer si une fonctionnalité particulière du système d’exploitation ou de la famille d’appareils est présente. Plutôt que de détecter le système d’exploitation ou la famille d’appareils (et le numéro de version), vérifiez directement la présence de la fonctionnalité à l’aide d’un test (voir [Compilation conditionnelle et code adaptatif](wpsl-to-uwp-porting-to-a-uwp-project.md)). Si vous devez exiger un système d’exploitation ou une famille d’appareils spécifique, veillez à l’utiliser comme une version minimale prise en charge plutôt que de concevoir le test pour cette version particulière.

Pour adapter l’interface utilisateur à différents appareils, plusieurs techniques sont recommandées. Continuez à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans le balisage XAML, continuez à utiliser les tailles en pixels effectifs (auparavant appelés « pixels d’affichage ») afin que votre interface utilisateur s’adapte à différentes résolutions et différents facteurs d’échelle (voir [Pixels d’affichage/effectifs, distance d’affichage et facteurs d’échelle](wpsl-to-uwp-porting-xaml-and-ui.md)). Utilisez également les déclencheurs adaptatifs et les méthodes setter du Gestionnaire d’état visuel pour adapter votre interface utilisateur à la taille de la fenêtre (voir le [Guide des applications UWP](https://msdn.microsoft.com/library/windows/apps/dn894631)).

Toutefois, si l’un de vos scénarios implique nécessairement la détection de la famille d’appareils, vous pouvez mettre celle-ci en œuvre. Dans cet exemple, nous faisons appel à la classe [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) pour naviguer vers une page adaptée à la famille d’appareils mobiles lorsqu’un tel appareil est utilisé, en veillant à revenir vers une page par défaut dans le cas contraire.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Votre application peut également déterminer la famille d’appareils sur laquelle elle est exécutée à partir des facteurs de sélection de ressources en vigueur. L’exemple ci-dessous indique comment effectuer cette opération de manière impérative, et la rubrique [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) décrit le cas d’utilisation le plus courant de la classe pour le chargement de ressources propres à la famille d’appareils en fonction du facteur de cette dernière.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Voir également [Compilation conditionnelle et code adaptatif](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>État de l’appareil

Une application de WindowsPhone Silverlight peut utiliser la classe **Microsoft.Phone.Info.DeviceStatus** pour obtenir des informations sur l’appareil sur lequel l’application est en cours d’exécution. S’il n’existe pas d’équivalent UWP direct pour l’espace de noms **Microsoft.Phone.Info**, voici certains événements et propriétés que vous pouvez utiliser dans une application UWP à la place des appels aux membres de la classe **DeviceStatus**.

| Silverlight pour Windows Phone                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Propriétés **ApplicationCurrentMemoryUsage** et **ApplicationCurrentMemoryUsageLimit** | Propriétés [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/dn633832) et [**AppMemoryUsageLimit**](https://msdn.microsoft.com/library/windows/apps/dn633836)                                                                                                                                    |
| Propriété **ApplicationPeakMemoryUsage**                                                 | Utilisez les outils de profilage de mémoire de Visual Studio. Pour plus d’informations, voir [Analyser l’utilisation de la mémoire](http://msdn.microsoft.com/library/windows/apps/dn645469.aspx).                                                                                                                                                                          |
| Propriété **DeviceFirmwareVersion**                                                      | Propriété [**EasClientDeviceInformation.SystemFirmwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608144) (famille d’appareils de bureau uniquement)                                                                                                                                                                             |
| Propriété **DeviceHardwareVersion**                                                      | Propriété [**EasClientDeviceInformation.SystemHardwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608145) (famille d’appareils de bureau uniquement)                                                                                                                                                                             |
| Propriété **DeviceManufacturer**                                                         | Propriété [**EasClientDeviceInformation.SystemManufacturer**](https://msdn.microsoft.com/library/windows/apps/hh701398) (famille d’appareils de bureau uniquement)                                                                                                                                                                                |
| Propriété **DeviceName**                                                                 | Propriété [**EasClientDeviceInformation.SystemProductName**](https://msdn.microsoft.com/library/windows/apps/hh701401) (famille d’appareils de bureau uniquement)                                                                                                                                                                                 |
| Propriété **DeviceTotalMemory**                                                          | Aucun équivalent                                                                                                                                                                                                                                                                                                                      |
| Propriété **IsKeyboardDeployed**                                                         | Aucun équivalent. Cette propriété fournit des informations sur les claviers matériels pour appareils mobiles, qui sont rarement utilisés.                                                                                                                                                                                                        |
| Propriété **IsKeyboardPresent**                                                          | Aucun équivalent. Cette propriété fournit des informations sur les claviers matériels pour appareils mobiles, qui sont rarement utilisés.                                                                                                                                                                                                        |
| Événement **KeyboardDeployedChanged**                                                       | Aucun équivalent. Cette propriété fournit des informations sur les claviers matériels pour appareils mobiles, qui sont rarement utilisés.                                                                                                                                                                                                        |
| Propriété **PowerSource**                                                                | Aucun équivalent                                                                                                                                                                                                                                                                                                                      |
| Événement **PowerSourceChanged**                                                            | Gérez l’événement [**RemainingChargePercentChanged**](https://msdn.microsoft.com/library/windows/apps/jj207240) (famille d’appareils mobiles uniquement). L’événement est déclenché lorsque la valeur de la propriété [**RemainingChargePercent**](https://msdn.microsoft.com/library/windows/apps/jj207239) (famille d’appareils mobiles uniquement) diminue de 1 %. |

## <a name="location"></a>Emplacement

Lorsqu’une application déclarant la fonctionnalité de localisation dans son manifeste de package d’application s’exécute sur Windows 10, le système invite l’utilisateur final à donner son consentement. Si votre application affiche sa propre invite de consentement personnalisée ou qu’elle fournit une bascule de type activation/désactivation, vous devrez supprimer ces éléments pour que l’utilisateur final ne soit invité qu’une seule fois à autoriser cette fonctionnalité.

## <a name="orientation"></a>Orientation

L’équivalent d’application UWP des propriétés **PhoneApplicationPage.SupportedOrientations** et **Orientation** correspond à l’élément [**uap:InitialRotationPreference**](https://msdn.microsoft.com/library/windows/apps/dn934798) dans le manifeste de package d’application. Sélectionnez l’onglet **Application** s’il ne l’est pas déjà, puis cochez une ou plusieurs cases sous **Rotations prises en charge** pour enregistrer vos préférences.

Toutefois, vous êtes invité à concevoir l’interface utilisateur de votre application UWP de façon à en optimiser l’apparence indépendamment de l’orientation de l’appareil et de la taille de l’écran. Pour plus d’informations, voir la rubrique [Portage pour différents facteurs de forme et expériences utilisateur](wpsl-to-uwp-form-factors-and-ux.md).

Rubrique suivante : [Portage des couches métier et des couches de données](wpsl-to-uwp-business-and-data.md).

