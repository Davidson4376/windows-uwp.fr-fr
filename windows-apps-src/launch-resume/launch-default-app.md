---
author: TylerMSFT
title: Lancer l’application par défaut pour un URI
description: Découvrez comment lancer l’application par défaut d’un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique fournit également une vue d’ensemble des nombreux schémas d’URI intégrés à Windows.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.author: twhitney
ms.date: 06/26/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a5a0af7b7227f149722cf34a368fc588198880ef
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5881745"
---
# <a name="launch-the-default-app-for-a-uri"></a>Lancer l’application par défaut pour un URI


**API importantes**

- [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
- [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
- [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Découvrez comment lancer l’application par défaut pour un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique donne également une vue d’ensemble des nombreux schémas d’URI intégré à Windows. Vous pouvez également lancer des URI personnalisés. Pour plus d’informations sur l’inscription d’un schéma d’URI personnalisé et la gestion de l’activation des URI, voir [Gérer l’activation des URI](handle-uri-activation.md).

Les schémas d’URI permettent d’ouvrir des applications en cliquant sur des liens hypertexte. Tout comme vous pouvez commencer à écrire un nouveau message électronique à l’aide de **mailto:**, vous pouvez ouvrir le navigateur web par défaut à l’aide de **http:**.

Cette rubrique décrit les schémas d’URI suivants qui sont intégrés dans Windows:

| Schéma d’URI | Lancement |
| ----------:|----------|
|[bingmaps:, ms-drive-to: et ms-walk-to: ](#maps-app-uri-schemes) | Application Cartes |
|[http:](#http-uri-scheme) | Navigateur web par défaut |
|[mailto:](#email-uri-scheme) | Application de courrier électronique par défaut |
|[ms-call:](#call-app-uri-scheme) |  Application d’appel |
|[ms-chat:](#messaging-app-uri-scheme) | Application de messagerie |
|[ms-people:](#people-app-uri-scheme) | Application Contacts |
|[ms-photos:](#photos-app-uri-scheme) | Application Photos |
|[ms-settings:](#settings-app-uri-scheme) | Application Paramètres |
|[ms-store:](#store-app-uri-scheme)  | Application du Windows Store |
|[ms-tonepicker:](#tone-picker-uri-scheme) | Sélecteur de tonalités |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | Application Numéros à proximité |
|[msnweather:](#weather-app-uri-scheme) | Application Météo |

<br>
Par exemple, l’URI suivant ouvre le navigateur par défaut et affiche le site web Bing.

`http://bing.com`

Vous pouvez également lancer des schémas d’URI personnalisés. Si aucune application n’est installée pour gérer cet URI, vous pouvez recommander à l’utilisateur une application à installer. Pour plus d’informations, voir [Recommander une application si aucune application n’est disponible pour gérer l’URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

En général, votre application ne peut pas sélectionner l’application à lancer. L’utilisateur détermine l’application à lancer. Plusieurs applications peuvent s’inscrire pour gérer le même schéma d’URI. Une exception à cette règle a trait aux schémas d’URI réservés. Les inscriptions de schémas d’URI réservés sont ignorées. Pour obtenir la liste complète des schémas d’URI réservés, voir [Gérer l’activation des URI](handle-uri-activation.md). Si plusieurs applications ont inscrit le même schéma d’URI, votre application peut recommander le lancement d’une application spécifique. Pour plus d’informations, voir [Recommander une application si aucune application n’est disponible pour gérer l’URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Appeler LaunchUriAsync pour lancer un URI

Utilisez la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) pour lancer un URI. Lors de l’appel de cette méthode, votre application doit être au premier plan, c’est-à-dire qu’elle doit être visible pour l’utilisateur. Cette conditions contribue à garantir que l’utilisateur conserve le contrôle. Pour pouvoir la respecter, assurez-vous que vous avez relié directement tous les lancements d’URI à l’interface utilisateur de votre application. L’utilisateur doit toujours exercer une action pour initier un lancement d’URI. Si vous tentez de lancer un URI alors que votre application n’est pas au premier plan, le lancement échoue et votre rappel d’erreur est appelé.

Commencez par créer un objet [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/system.uri.aspx) pour représenter l’URI, puis passez-le à la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476). Utilisez le résultat renvoyé pour voir si l’appel a réussi, comme illustré dans l’exemple suivant.

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

![boîte de dialogue d’avertissement sur un arrière-plan grisé de l’application. la boîte de dialogue demande à l’utilisateur s’il souhaite basculer entre les applications et a des boutons «oui» et «non» dans le coin inférieur droit. le bouton «non» est mis en surbrillance.](images/warningdialog.png)

Si vous voulez que cette invite apparaisse toujours, utilisez la propriété [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) pour indiquer que le système d’exploitation doit afficher un avertissement.

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

Pour faire une recommandation, appelez la méthode [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) avec [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) ayant pour valeur le nom de la famille de packages de l’application du Store que vous voulez recommander. Le système d’exploitation utilise cette information pour remplacer l’option générale permettant de rechercher une application dans le Windows Store par une option spécifique permettant d’acquérir l’application recommandée dans le Windows Store.

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

Les applications sources qui appellent la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) peuvent demander à rester à l’écran après le lancement d’un URI. Par défaut, Windows essaie de partager tout l’espace disponible de manière équitable entre l’application source et l’application cible qui gère l’URI. Les applications sources peuvent utiliser la propriété [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) pour indiquer au système d’exploitation qu’elles préfèrent que leur fenêtre d’application occupe une plus grande ou plus petite partie de l’espace disponible. La propriété **DesiredRemainingView** peut également servir à indiquer que l’application source n’a pas besoin de rester à l’écran après le lancement de l’URI et qu’elle peut être complètement remplacée par l’application cible. Cette propriété spécifie uniquement la taille de fenêtre par défaut de l’application appelante. Elle ne spécifie pas le comportement d’autres applications qui peuvent se trouver en même temps sur l’écran.

**Remarque**Windows tient compte de différents facteurs pour déterminer la taille de l’application source de la fenêtre finale, par exemple, la préférence de l’application source, le nombre d’applications à l’écran, l’orientation de l’écran et ainsi de suite. La définition de la propriété [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) ne garantit pas un comportement de fenêtrage spécifique pour l’application source.

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

Utilisez le schéma d’URI **ms-call:** pour lancer l’application d’appel.

| Schéma d’URI       | Résultat                   |
|------------------|--------------------------|
| ms-call:settings | Lance la page de paramètres de l’application d’appel. |

### <a name="email-uri-scheme"></a>Schéma d’URI pour le courrier électronique

Utilisez le schéma d’URI **mailto:** pour lancer l’application de courrier électronique par défaut.

| Schéma d’URI |Résultats                          |
|------------|---------------------------------|
| mailto:    | Lance l’application de courrier électronique par défaut. |
| mailto:\[email address\] | Lance l’application de courrier électronique et crée un message dont la ligne À contient l’adresse de messagerie spécifiée. Notez que le message n’est pas envoyé tant que l’utilisateur n’appuie pas sur Envoyer. |

### <a name="http-uri-scheme"></a>Schéma d’URI pour le protocole HTTP

Utilisez le schéma d’URI **http:** pour lancer le navigateur web par défaut.

| Schéma d’URI | Résultats                           |
|------------|-----------------------------------|
| http:      | Lance le navigateur web par défaut. |

### <a name="maps-app-uri-schemes"></a>Schémas d’URI pour l’application Cartes

Utilisez les schémas d’URI **bingmaps:**, **ms-drive-to:** et **ms-walk-to:** afin de [lancer l’application Cartes Windows](launch-maps-app.md) pour trouver des cartes, des itinéraires et des résultats de recherche spécifiques. Par exemple, l’URI suivant ouvre l’application Cartes Windows et affiche une carte centrée sur la ville de New York.

`bingmaps:?cp=40.726966~-74.006076`

![Exemple de l’application Cartes Windows.](images/mapnyc.png)

Pour plus d’informations, voir [Lancer l’application Cartes Windows](launch-maps-app.md). Pour utiliser le contrôle de carte dans votre propre application, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695).

### <a name="messaging-app-uri-scheme"></a>Schéma d’URI pour l’application de messagerie

Utilisez le schéma d’URI **ms-chat:** pour lancer l’application de messagerie Windows.

| Schéma d’URI |Résultats |
|------------|--------|
| ms-chat:   | Lance l’application de messagerie. |
| ms-chat:?ContactID={contacted}  |  Permet le lancement de l’application de messagerie avec les informations d’un contact spécifique.   |
| ms-chat:?Body={body} | Permet le lancement de l’application de messagerie avec une chaîne à utiliser en tant que contenu du message.|
| ms-chat:?Addresses={address}&Body={body} | Permet le lancement de l’application de messagerie avec les informations d’adresse d’une personne particulière et une chaîne à utiliser en tant que contenu du message. Remarque: les adresses peuvent être concaténées. |
| ms-chat:?TransportId={transportId}  | Permet le lancement de l’application de messagerie avec un ID de transport particulier. |

### <a name="tone-picker-uri-scheme"></a>Schéma d’URI pour le sélecteur de tonalités

Utilisez le schéma d’URI **ms-tonepicker:** pour choisir les sonneries, les alarmes et les sons système. Vous pouvez également enregistrer de nouvelles sonneries et afficher le nom complet d’une tonalité.

| Schéma d’URI | Résultats |
|------------|---------|
| ms-tonepicker: | Sélectionnez les sonneries, alarmes et sons système. |

Les paramètres sont transmis à l’API LaunchURI.à l’aide de la classe [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx). Pour en savoir plus, voir [Sélectionner et enregistrer des tonalités à l’aide du schéma d’URI ms-tonepicker](launch-ringtone-picker.md).

### <a name="nearby-numbers-app-uri-scheme"></a>Schéma d’URI pour l’application Numéros à proximité

Utilisez le schéma d’URI **ms-yellowpage:** pour lancer l’application de recherche de numéros à proximité.

| Schéma d’URI | Résultats |
|------------|---------|
| ms-yellowpage:?input=\[keyword\]&amp;method=\[String or T9\] | Lance l’application Numéros à proximité.<br>`input` fait référence au mot-clé que vous voulez rechercher.<br>`method` fait référence au type de recherche (chaîne ou T9).<br>Si `method` est `T9` (un type de clavier), alors `keyword` doit être une chaîne numérique correspondant aux touches de clavier T9 à rechercher.<br>Si `method` est `String`, alors `keyword` est le mot-clé à rechercher. |

### <a name="people-app-uri-scheme"></a>Schéma d’URI pour l’application Contacts

Utilisez le schéma d’URI **ms-people:** pour lancer l’application Contacts.
Pour en savoir plus, voir [Lancer l’application Contacts](launch-people-apps.md).

### <a name="photos-app-uri-scheme"></a>Schéma d’URI pour l’application Photos

Utilisez le schéma d’URI **ms-photos:** pour lancer l’application Photos afin d'afficher une image ou de modifier une vidéo. Exemple :  
Pour afficher une image: `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
Ou pour modifier une vidéo: `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> Les URI permettant de modifier une vidéo ou d'afficher une image sont disponibles uniquement sur le bureau.

| Schéma d’URI |Résultats |
|------------|--------|
| ms-photos:viewer?fileName={filename} | Lance l’application Photos pour afficher l’image spécifiée où {filename} est un nom de chemin d'accès complet. Exemple : `c:\users\userName\Pictures\ImageToView.jpg` |
| ms-photos:videoedit?InputToken={input token} | Lance l’application Photos en mode retouche vidéo pour le fichier représenté par le jeton de fichier. **InputToken** est requis. Utilisez le jeton de partage de fichier [SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) pour obtenir un jeton pour un fichier. |
| ms-photos:videoedit?Action={action} | Un paramètre facultatif qui ouvre l’application Photos dans le mode retouche vidéo spécifié où {action} est l'un des éléments suivants: **SlowMotion**, **FrameExtraction**, **Trim**, **View**, **Ink **. Si rien n'est spécifié, le choix par défaut est **View**. |
| ms-photos:videoedit?StartTime={timespan} | Un paramètre facultatif qui spécifie l’endroit où démarrer la lecture de la vidéo. `{timespan}` doit être au format `"hh:mm:ss.ffff"`. Si rien n'est spécifié, le choix par défaut est `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>Schéma d’URI pour l’application Paramètres

Utilisez le schéma d’URI **ms-settings:** pour [lancer l’application Paramètres Windows](launch-settings-app.md). Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Par exemple, l’URI suivant ouvre l’application Paramètres et affiche les paramètres de confidentialité de l’appareil photo.

`ms-settings:privacy-webcam`

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations, voir [Lancer l’application Paramètres Windows](launch-settings-app.md) et [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/hh768223).

### <a name="store-app-uri-scheme"></a>Schéma d’URI pour l’application du Store

Utilisez le schéma d’URI **ms-windows-store:** pour [lancer l’application UWP](launch-store-app.md). Ouvrez des pages de détails, d’avis et de recherche en rapport avec les produits. Par exemple, l’URI suivant ouvre l’application UWP et accède à la page d’accueil du Store.

`ms-windows-store://home/`

Pour en savoir plus, voir [Lancer l’application UWP](launch-store-app.md).

### <a name="weather-app-uri-scheme"></a>Schéma d’URI l’application Météo

Utilisez la **msnweather:** schéma d’URI pour lancer l’application Météo.

| Schéma d’URI | Résultats |
|------------|---------|
| msnweather://Forecast?la= \[latitude\] & lo = \ [longitude\] | Lance l’application Météo dans la page des prévisions basée sur un coordonnées d’emplacement géographique.<br>`latitude` fait référence à la latitude de la localisation.<br> `longitude` fait référence à la longitude de la localisation.<br> |
