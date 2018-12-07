---
title: Enregistrer et charger des paramètres dans une application UWP
description: Découvrez comment enregistrer et charger des paramètres d’application dans les applications de plateforme Windows universelle.
ms.date: 05/07/2018
ms.topic: article
keywords: prise en main, uwp, Windows10, piste d'apprentissage, paramètres, enregistrer des paramètres, charger des paramètres
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4aa56bf24d2dfa1fd4ae1947a5b0edf7f312ea2f
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796075"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>Enregistrer et charger des paramètres dans une application UWP

Cette rubrique décrit ce que vous devez savoir pour commencer l'enregistrement et le chargement de paramètres dans une application de plateforme Windows universelle (UWP). Les API principales sont présentées et des liens sont fournis pour vous aider à en savoir plus.

Utilisez des paramètres pour retenir les aspects personnalisables par l’utilisateur de votre application. Par exemple, un lecteur de flux d’actualités peut utiliser des paramètres d’application pour enregistrer les sources d’actualités à afficher et la police à utiliser pour la lecture des articles.

Nous allons examiner le code pour enregistrer et charger les paramètres d’application, notamment les paramètres locaux et itinérants.

## <a name="what-do-you-need-to-know"></a>Ce que vous devez savoir

Utilisez les paramètres d’application pour stocker les données de configuration telles que les préférences de l’utilisateur et l’état de l’application.  Les paramètres spécifiques à l’appareil sont stockés localement. Les paramètres qui s’appliquent à tous les appareils sur lesquels votre application est installée sont stockés dans le magasin de données itinérantes. Les paramètres sont itinérants entre les différents appareils auxquels l’utilisateur est connecté avec le même compte Microsoft et qui disposent de la même version de l’application.

Les types de données suivants peuvent être utilisés avec des paramètres: entiers, doubles, flottants, caractères, chaînes, points, DateTime, etc. Vous pouvez également stocker des instances de la classe [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) qui est utile lorsque plusieurs paramètres doivent être traités en tant qu’unité. Par exemple, un nom de police et une taille de point pour afficher du texte dans le volet de lecture de votre application doivent être enregistrés/restaurés comme une seule unité. Cela empêche la désynchronisation d'une paramètre avec les autres à cause de retards lors de l'itinérance d'un paramètre avant l'autre.

Voici les principales API que vous devez connaître pour enregistrer ou charger les paramètres d’application:

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) obtient le conteneur de paramètres d’application dans le magasin de données d’application locales. Stockez ici des paramètres qu'il n'est pas approprié de déplacer d'un appareil à l'autre, car ils représentent un état spécifique à cet appareil, ou sont trop grands.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) obtient le conteneur de paramètres d’application dans le magasin de données d’application itinérantes. Ces données se déplace d'un appareil à l'autre.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) est un conteneur qui représente les paramètres d’application sous forme de paires clé/valeur. Utilisez cette classe pour créer et récupérer des valeurs de paramètre.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) représente plusieurs paramètres d’application qui doivent être sérialisés en tant qu’unité. Cela est utile lorsqu’un paramètre ne doit pas être mis à jour indépendamment d’un autre.

## <a name="save-app-settings"></a>Enregistrer les paramètres d'application

Pour cette présentation, nous allons nous concentrer sur deux scénarios simples: enregistrer et charger un paramètre d'application en local et déplacer un paramètre de police composite/taille de police entre des appareils.

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

Enregistrez un paramètre sur l’appareil local, en obtenant d'abord un **ApplicationDataContainer** pour le magasin de données de paramètres locaux avec `Windows.Storage.ApplicationData.Current.LocalSettings`. Les paires de dictionnaire clé/valeur que vous affectez à cette instance sont enregistrées dans le magasin de données de paramètres d'appareil locaux.

Enregistrez un paramètre itinérant à l’aide d’un modèle semblable. Tout d’abord obtenez un **ApplicationDataContainer** pour le magasin de données de paramètres itinérants avec `Windows.Storage.ApplicationData.Current.RoamingSettings`. Affectez ensuite des paires clé/valeur pour cette instance.  Ces paires clé/valeur se déplaceront automatiquement entre des appareils.

Dans l’extrait de code ci-dessus, une **ApplicationDataCompositeValue** stocke plusieurs paires clé/valeur. Les valeurs composites sont utiles lorsque vous disposez de plusieurs paramètres qui ne doivent pas être désynchronisés entre eux. Lorsque vous enregistrez une **ApplicationDataCompositeValue**, les valeurs sont enregistrées et chargées en tant qu’unité, ou de façon atomique. Ainsi, les paramètres qui sont associés ne sont pas désynchronisés, car ils sont rendus itinérantes en tant qu’unité au lieu d’individuellement.

## <a name="load-app-settings"></a>Charger les paramètres d'application

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

Chargez un paramètre à partir de l’appareil local, en obtenant d'abord une instance **ApplicationDataContainer** pour le magasin de données de paramètres locaux avec `Windows.Storage.ApplicationData.Current.LocalSettings`. Utilisez-la ensuite pour récupérer les paires clé/valeur.

Chargez un paramètre itinérant en suivant un modèle semblable. Tout d’abord, obtenez une instance **ApplicationDataContainer** pour le magasin de données de paramètres itinérants avec `Windows.Storage.ApplicationData.Current.RoamingSettings`. Accédez aux paires clé/valeur à partir de cette instance. Si les données n’ont pas encore déplacées vers l’appareil dont vous accédez aux paramètres, vous obtenez un **ApplicationDataContainer** de valeur null. C’est pour cela qu'une vérification `if (composite != null)` est présente dans l’exemple de code ci-dessus.

## <a name="useful-apis-and-docs"></a>API et documents utiles

Voici un résumé rapide des API et d'autres documents utiles pour vous aider à commencer à enregistrer et à charger des paramètres d'application.

### <a name="useful-apis"></a>API utiles

| API | Description |
|------|---------------|
| [ApplicationData.LocalSettings](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.temporaryfolder) | Obtient le conteneur de paramètres d'application du magasin de données d'application locales. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | Obtient le conteneur de paramètres d'application du magasin de données d'application itinérantes. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | Conteneur de paramètres d’application qui prend en charge la création, la suppression, l'énumération et le parcours de la hiérarchie du conteneur. |
| [Espace de noms Windows.UI.ApplicationSettings](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Fournit des classes que vous utiliserez pour définir les paramètres d’application qui s’affichent dans le volet des paramètres de Windows Shell. |

### <a name="useful-docs"></a>Documents utiles

| Sujet | Description |
|-------|----------------|
| [Recommandations en matière de paramètres d’application](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | Décrit les bonnes pratiques pour créer et afficher des paramètres d’application. |
| [Stocker et récupérer des paramètres et autres données d’application](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | Procédure d’enregistrement et de récupération de paramètres, notamment les paramètres itinérants. |

## <a name="useful-code-samples"></a>Exemples de code utiles

| Exemple de code | Description |
|-----------------|---------------|
| [Exemple de données d'application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | Focus sur les scénarios2 à 4 relatifs aux paramètres |
