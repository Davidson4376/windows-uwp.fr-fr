---
title: Lancer l’application par défaut pour un URI
description: Découvrez comment lancer l’application par défaut d’un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique fournit également une vue d’ensemble des nombreux schémas d’URI intégrés à Windows.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.date: 06/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 48b4df662b990f64adcbfe2e7e03dc5303b2962e
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340427"
---
# <a name="launch-the-default-app-for-a-uri"></a>Lancer l’application par défaut pour un URI


**API importantes**

- [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
- [**PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
- [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview)

Découvrez comment lancer l’application par défaut d’un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique fournit également une vue d’ensemble des nombreux schémas d’URI intégrés à Windows. Vous pouvez également lancer des URI personnalisés. Pour plus d’informations sur l’inscription d’un schéma d’URI personnalisé et la gestion de l’activation des URI, voir [Gérer l’activation des URI](handle-uri-activation.md).

Les schémas d’URI permettent d’ouvrir des applications en cliquant sur des liens hypertexte. Tout comme vous pouvez commencer à écrire un nouveau message électronique à l’aide de **mailto:** , vous pouvez ouvrir le navigateur web par défaut à l’aide de **http:** .

Cette rubrique décrit les schémas d’URI suivants qui sont intégrés dans Windows :

| Schéma d’URI | Lancement |
| ----------:|----------|
|[BingMaps :, MS-Drive-to : et MS-Walk-to :](#maps-app-uri-schemes) | Application Cartes |
|[protocoles](#http-uri-scheme) | Navigateur web par défaut |
|[écrire](#email-uri-scheme) | Application de courrier électronique par défaut |
|[MS-Call :](#call-app-uri-scheme) |  Application d’appel |
|[MS-conversation :](#messaging-app-uri-scheme) | Application de messagerie |
|[MS-People :](#people-app-uri-scheme) | Application Contacts |
|[MS-photos :](#photos-app-uri-scheme) | Application Photos |
|[MS-paramètres :](#settings-app-uri-scheme) | Application Paramètres |
|[MS-Store :](#store-app-uri-scheme)  | Application du Windows Store |
|[MS-tonepicker :](#tone-picker-uri-scheme) | Sélecteur de tonalités |
|[MS-Yellowpage :](#nearby-numbers-app-uri-scheme) | Application Numéros à proximité |
|[msnweather:](#weather-app-uri-scheme) | Application météo |

<br>
Par exemple, l’URI suivant ouvre le navigateur par défaut et affiche le site web Bing.

`https://bing.com`

Vous pouvez également lancer des schémas d’URI personnalisés. Si aucune application n’est installée pour gérer cet URI, vous pouvez recommander à l’utilisateur une application à installer. Pour plus d’informations, voir [Recommander une application si aucune application n’est disponible pour gérer l’URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

En général, votre application ne peut pas sélectionner l’application à lancer. L’utilisateur détermine l’application à lancer. Plusieurs applications peuvent s’inscrire pour gérer le même schéma d’URI. Une exception à cette règle a trait aux schémas d’URI réservés. Les inscriptions de schémas d’URI réservés sont ignorées. Pour obtenir la liste complète des schémas d’URI réservés, voir [Gérer l’activation des URI](handle-uri-activation.md). Si plusieurs applications ont inscrit le même schéma d’URI, votre application peut recommander le lancement d’une application spécifique. Pour plus d’informations, voir [Recommander une application si aucune application n’est disponible pour gérer l’URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Appeler LaunchUriAsync pour lancer un URI

Utilisez la méthode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) pour lancer un URI. Lors de l’appel de cette méthode, votre application doit être au premier plan, c’est-à-dire qu’elle doit être visible pour l’utilisateur. Cette conditions contribue à garantir que l’utilisateur conserve le contrôle. Pour pouvoir la respecter, assurez-vous que vous avez relié directement tous les lancements d’URI à l’interface utilisateur de votre application. L’utilisateur doit toujours exercer une action pour initier un lancement d’URI. Si vous tentez de lancer un URI alors que votre application n’est pas au premier plan, le lancement échoue et votre rappel d’erreur est appelé.

Commencez par créer un objet [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri) pour représenter l’URI, puis passez-le à la méthode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync). Utilisez le résultat renvoyé pour voir si l’appel a réussi, comme illustré dans l’exemple suivant.

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

Dans certains cas, le système d’exploitation invite l’utilisateur à confirmer qu’il veut réellement changer d’application.

![boîte de dialogue d’avertissement sur un arrière-plan grisé de l’application. la boîte de dialogue demande à l’utilisateur s’il souhaite basculer entre les applications et a des boutons « oui » et « non » dans le coin inférieur droit. le bouton « non » est mis en surbrillance.](images/warningdialog.png)

Si vous voulez que cette invite apparaisse toujours, utilisez la propriété [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.treatasuntrusted) pour indiquer que le système d’exploitation doit afficher un avertissement.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>Recommander une application si aucune application n’est disponible pour gérer l’URI

L’utilisateur ne dispose pas toujours d’une application capable de gérer l’URI que vous lancez. Par défaut, le système d’exploitation gère ces cas en fournissant à l’utilisateur un lien pour rechercher une application appropriée dans le Windows Store. Si vous voulez recommander à l’utilisateur l’acquisition d’une application spécifique dans ce scénario, vous pouvez passer cette recommandation avec l’URI que vous lancez.

Les recommandations sont également utiles quand plusieurs applications sont inscrites pour gérer un schéma d’URI. Si vous recommandez une application spécifique, Windows ouvre celle-ci si elle est installée.

Pour faire une recommandation, appelez la méthode [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)** ](https://docs.microsoft.com/en-us/uwp/api/windows.system.launcher.launchuriasync#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_) avec [**LauncherOptions.preferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) ayant pour valeur le nom de la famille de packages de l’application du Store que vous voulez recommander. Le système d’exploitation utilise cette information pour remplacer l’option générale permettant de rechercher une application dans le Windows Store par une option spécifique permettant d’acquérir l’application recommandée dans le Windows Store.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>Définir une préférence d’affichage persistant

Les applications sources qui appellent la méthode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) peuvent demander à rester à l’écran après le lancement d’un URI. Par défaut, Windows essaie de partager tout l’espace disponible de manière équitable entre l’application source et l’application cible qui gère l’URI. Les applications sources peuvent utiliser la propriété [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) pour indiquer au système d’exploitation qu’elles préfèrent que leur fenêtre d’application occupe une plus grande ou plus petite partie de l’espace disponible. La propriété **DesiredRemainingView** peut également servir à indiquer que l’application source n’a pas besoin de rester à l’écran après le lancement de l’URI et qu’elle peut être complètement remplacée par l’application cible. Cette propriété spécifie uniquement la taille de fenêtre par défaut de l’application appelante. Elle ne spécifie pas le comportement d’autres applications qui peuvent se trouver en même temps sur l’écran.

**Remarque**  Windows prend en compte plusieurs facteurs différents lorsqu’il détermine la taille finale de la fenêtre de l’application source, par exemple, la préférence de l’application source, le nombre d’applications à l’écran, l’orientation de l’écran, et ainsi de suite. La définition de la propriété [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) ne garantit pas un comportement de fenêtrage spécifique pour l’application source.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>Schémas d’URI ##

Les différents schémas d’URI sont décrits ci-dessous.

### <a name="call-app-uri-scheme"></a>Schéma d’URI pour l’application d’appel

Utilisez l' **appel MS-Call :** Schéma d’URI pour lancer l’application d’appel.

| Schéma d’URI       | Résultat                   |
|------------------|--------------------------|
| ms-call:settings | Lance la page de paramètres de l’application d’appel. |

### <a name="email-uri-scheme"></a>Schéma d’URI pour le courrier électronique

Utilisez **mailto :** Schéma d’URI pour lancer l’application de messagerie par défaut.

| Schéma d’URI |Résultats                          |
|------------|---------------------------------|
| mailto:    | Lance l’application de courrier électronique par défaut. |
| mailto : \[email-no__t-1 | Lance l’application de courrier électronique et crée un message dont la ligne À contient l’adresse de messagerie spécifiée. Notez que le message n’est pas envoyé tant que l’utilisateur n’appuie pas sur Envoyer. |

### <a name="http-uri-scheme"></a>Schéma d’URI pour le protocole HTTP

Utilisez le **protocole http :** Schéma d’URI permettant de lancer le navigateur Web par défaut.

| Schéma d’URI | Résultats                           |
|------------|-----------------------------------|
| http:      | Lance le navigateur web par défaut. |

### <a name="maps-app-uri-schemes"></a>Schémas d’URI pour l’application Cartes

Utilisez **BingMaps :** , **MS-Drive-to :** et **MS-Walk-to :** Schémas d’URI permettant de [lancer l’application Windows Maps](launch-maps-app.md) sur des mappages, des directions et des résultats de recherche spécifiques. Par exemple, l’URI suivant ouvre l’application Cartes Windows et affiche une carte centrée sur la ville de New York.

`bingmaps:?cp=40.726966~-74.006076`

![Exemple de l’application Cartes Windows.](images/mapnyc.png)

Pour plus d’informations, voir [Lancer l’application Cartes Windows](launch-maps-app.md). Pour utiliser le contrôle de carte dans votre propre application, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps).

### <a name="messaging-app-uri-scheme"></a>Schéma d’URI pour l’application de messagerie

Utilisez **MS-chat :** Schéma d’URI pour lancer l’application de messagerie Windows.

| Schéma d’URI |Résultats |
|------------|--------|
| ms-chat:   | Lance l’application de messagerie. |
| ms-chat:?ContactID={contacted}  |  Permet le lancement de l’application de messagerie avec les informations d’un contact spécifique.   |
| ms-chat:?Body={body} | Permet le lancement de l’application de messagerie avec une chaîne à utiliser en tant que contenu du message.|
| ms-chat:?Addresses={address}&Body={body} | Permet le lancement de l’application de messagerie avec les informations d’adresse d’une personne particulière et une chaîne à utiliser en tant que contenu du message. Remarque : Les adresses peuvent être concaténées. |
| ms-chat:?TransportId={transportId}  | Permet le lancement de l’application de messagerie avec un ID de transport particulier. |

### <a name="tone-picker-uri-scheme"></a>Schéma d’URI pour le sélecteur de tonalités

Utilisez le **MS-tonepicker :** Schéma d’URI pour choisir des sonneries, des alarmes et des tonalités système. Vous pouvez également enregistrer de nouvelles sonneries et afficher le nom complet d’une tonalité.

| Schéma d’URI | Résultats |
|------------|---------|
| ms-tonepicker: | Sélectionnez les sonneries, alarmes et sons système. |

Les paramètres sont transmis à l’API LaunchURI.à l’aide de la classe [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset). Pour en savoir plus, voir [Sélectionner et enregistrer des tonalités à l’aide du schéma d’URI ms-tonepicker](launch-ringtone-picker.md).

### <a name="nearby-numbers-app-uri-scheme"></a>Schéma d’URI pour l’application Numéros à proximité

Utilisez le **MS-Yellowpage :** Schéma d’URI pour lancer l’application de nombres proches.

| Schéma d’URI | Résultats |
|------------|---------|
| MS-Yellowpage :? Input = \[keyword @ no__t-1 & méthode = \[String ou T9 @ no__t-3 | Lance l’application Numéros à proximité.<br>`input` fait référence au mot clé dans lequel vous souhaitez effectuer la recherche.<br>`method` fait référence au type de recherche (chaîne ou recherche T9).<br>Si `method` est `T9` (un type de clavier), alors `keyword` doit être une chaîne numérique correspondant aux touches de clavier T9 à rechercher.<br>Si `method` est `String`, alors `keyword` est le mot-clé à rechercher. |

### <a name="people-app-uri-scheme"></a>Schéma d’URI pour l’application Contacts

Utilisez le Schéma d’URI pour lancer l’application People.
Pour plus d’informations, voir [Lancer l’application Contacts](launch-people-apps.md).

### <a name="photos-app-uri-scheme"></a>Schéma d’URI pour l’application Photos

Utilisez le Schéma d’URI pour lancer l’application photos pour afficher une image ou modifier une vidéo. Exemple :  
Pour afficher une image : `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
Ou pour modifier une vidéo : `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> Les URI permettant de modifier une vidéo ou d'afficher une image sont disponibles uniquement sur le bureau.

| Schéma d’URI |Résultats |
|------------|--------|
| ms-photos:viewer?fileName={filename} | Lance l’application Photos pour afficher l’image spécifiée où {filename} est un nom de chemin d'accès complet. Par exemple : `c:\users\userName\Pictures\ImageToView.jpg` |
| ms-photos:videoedit?InputToken={input token} | Lance l’application Photos en mode retouche vidéo pour le fichier représenté par le jeton de fichier. **InputToken** est requis. Utilisez le jeton de partage de fichier [SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) pour obtenir un jeton pour un fichier. |
| ms-photos:videoedit?Action={action} | Paramètre facultatif qui ouvre l’application photos dans le mode d’édition vidéo spécifié, où {action} est l’un des éléments suivants : **SlowMotion**, **FrameExtraction**, **Trim**, **View**, **Ink**. Si rien n'est spécifié, le choix par défaut est **View**. |
| ms-photos:videoedit?StartTime={timespan} | Un paramètre facultatif qui spécifie l’endroit où démarrer la lecture de la vidéo. `{timespan}` doit être au format `"hh:mm:ss.ffff"`. S’il n’est pas spécifié, la valeur par défaut est `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>Schéma d’URI pour l’application Paramètres

Utilisez la **section MS-Settings :** Schéma d’URI pour [lancer l’application Paramètres Windows](launch-settings-app.md). Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Par exemple, l’URI suivant ouvre l’application Paramètres et affiche les paramètres de confidentialité de l’appareil photo.

`ms-settings:privacy-webcam`

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations, voir [Lancer l’application Paramètres Windows](launch-settings-app.md) et [Recommandations en matière d’applications prenant en charge la confidentialité](https://docs.microsoft.com/windows/uwp/security/index).

### <a name="store-app-uri-scheme"></a>Schéma d’URI pour l’application du Windows Store

Utilisez l' **emplacement MS-Windows-Store :** Schéma d’URI pour [lancer l’application UWP](launch-store-app.md). Ouvrir les pages de détails du produit, les pages d’évaluation des produits et les pages de recherche, etc. Par exemple, l’URI suivant ouvre l’application UWP et lance la page d’hébergement du Store.

`ms-windows-store://home/`

Pour en savoir plus, voir [Lancer l’application UWP](launch-store-app.md).

### <a name="weather-app-uri-scheme"></a>Schéma d’URI de l’application météo

Utilisez **msnweather :** Schéma d’URI pour lancer l’application météo.

| Schéma d’URI | Résultats |
|------------|---------|
| msnweather://Forecast ? la = \[latitude @ no__t-1 & Lo = \[longitude @ no__t-3 | Lance l’application météo dans la page prévision en fonction des coordonnées géographiques de l’emplacement.<br>`latitude` fait référence à la latitude de l’emplacement.<br> `longitude` fait référence à la longitude de l’emplacement.<br> |
