---
author: TylerMSFT
title: "Lancer l’application par défaut pour un URI"
description: "Découvrez comment lancer l’application par défaut d’un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique donne également une vue d’ensemble des nombreux schémas d’URI intégrés à Windows."
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.sourcegitcommit: 9011d2e2e1e51edc89851e815d31e13390c24f96
ms.openlocfilehash: d454317d135e2b2b952c16fb00685e34b489865c

---

# Lancer l’application par défaut pour un URI


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Découvrez comment lancer l’application par défaut pour un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique donne également une vue d’ensemble des nombreux schémas d’URI intégré à Windows. Vous pouvez également lancer des URI personnalisés. Pour plus d’informations sur l’inscription d’un schéma d’URI personnalisé et la gestion de l’activation des URI, voir [Gérer l’activation des URI](handle-uri-activation.md).

## Comment lancer un URI


Les schémas d’URI permettent d’ouvrir des applications en cliquant sur des liens hypertexte. Tout comme vous pouvez commencer un nouveau message électronique à l’aide de **mailto:**, vous pouvez ouvrir le navigateur web par défaut à l’aide de **http:**. Cette rubrique décrit certains des schémas d’URI intégrés dans Windows :

-   Le [schéma d’URI ms-settings:](#settings) lance l’application Paramètres Windows
-   Le [schéma d’URI ms-store:](#store) lance l’application Windows Store
-   Le [schéma d’URI http:](#browser) lance le navigateur web par défaut
-   Le [schéma d’URI mailto:](#email) lance l’application de messagerie par défaut
-   Les [schémas d’URI bingmaps:, ms-drive-to: et ms-walk-to:](#maps) lancent l’application Cartes Windows

Par exemple, l’URI suivant ouvre le navigateur par défaut et affiche le site web Bing.

`http://bing.com`

Vous pouvez également lancer des schémas d’URI personnalisés. Si aucune application n’est installée pour gérer cet URI, vous pouvez recommander à l’utilisateur une application à installer. Pour plus d’informations, voir [Recommander une application](#recommend).

En général, votre application ne peut pas sélectionner l’application à lancer. L’utilisateur détermine l’application à lancer. Plusieurs applications peuvent s’inscrire pour gérer le même schéma d’URI. Une exception à cette règle a trait aux schémas d’URI réservés. Les inscriptions de schémas d’URI réservés sont ignorées. Pour obtenir la liste complète des schémas d’URI réservés, voir [Gérer l’activation des URI](handle-uri-activation.md). Si plusieurs applications ont inscrit le même schéma d’URI, votre application peut recommander le lancement d’une application spécifique. Pour plus d’informations, voir [Recommander une application](#recommend).

### Appeler LaunchUriAsync

Utilisez la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) pour lancer un URI. Lors de l’appel de cette méthode, votre application doit être au premier plan, c’est-à-dire qu’elle doit être visible pour l’utilisateur. Cette conditions contribue à garantir que l’utilisateur conserve le contrôle. Pour pouvoir la respecter, assurez-vous que vous avez relié directement tous les lancements d’URI à l’interface utilisateur de votre application. L’utilisateur doit toujours exercer une action pour initier un lancement d’URI. Si vous tentez de lancer un URI alors que votre application n’est pas au premier plan, le lancement échoue et votre rappel d’erreur est appelé.

Commencez par créer un objet [**System.Uri**](https://msdn.microsoft.com/en-us/library/windows/apps/system.uri.aspx) pour représenter l’URI, puis passez-le à la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476). Utilisez le résultat renvoyé pour voir si l’appel a réussi, comme illustré dans l’exemple suivant.

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

### Recommander une application

Dans certains cas, l’utilisateur ne dispose pas forcément d’une application installée pour gérer l’URI que vous lancez. Par défaut, le système d’exploitation gère ces cas en fournissant à l’utilisateur un lien pour rechercher une application appropriée dans le Windows Store. Si vous voulez recommander à l’utilisateur l’acquisition d’une application spécifique dans ce scénario, vous pouvez passer cette recommandation avec l’URI que vous lancez.

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

### Définir une préférence d’affichage persistant

Les applications sources qui appellent la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) peuvent demander à rester à l’écran après le lancement d’un URI. Par défaut, Windows essaie de partager tout l’espace disponible de manière équitable entre l’application source et l’application cible qui gère l’URI. Les applications sources peuvent utiliser la propriété [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) pour indiquer au système d’exploitation qu’elles préfèrent que leur fenêtre d’application occupe une plus grande ou plus petite partie de l’espace disponible. La propriété **DesiredRemainingView** peut également servir à indiquer que l’application source n’a pas besoin de rester à l’écran après le lancement de l’URI et qu’elle peut être complètement remplacée par l’application cible. Cette propriété spécifie uniquement la taille de fenêtre par défaut de l’application appelante. Elle ne spécifie pas le comportement d’autres applications qui peuvent se trouver en même temps sur l’écran.

**Remarque** Windows tient compte de plusieurs facteurs différents pour déterminer la taille finale de la fenêtre de l’application source, par exemple, la préférence de l’application source, le nombre d’applications à l’écran, l’orientation de l’écran, etc. La définition de la propriété [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) ne garantit pas un comportement de fenêtrage spécifique pour l’application source.

 

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## Schémas d’URI pour l’application Cartes Windows


Votre application peut utiliser les schémas d’URI **bingmaps:**, **ms-drive-to:** et **ms-walk-to:** afin de [lancer l’application Cartes Windows](launch-maps-app.md) pour des cartes, itinéraires et résultats de recherche spécifiques. Par exemple, l’URI suivant ouvre l’application Cartes Windows et affiche une carte centrée sur la ville de New York.

`bingmaps:?cp=40.726966~-74.006076`

![Exemple de l’application Cartes Windows.](images/mapnyc.png)

Pour plus d’informations, voir [Lancer l’application Cartes Windows](launch-maps-app.md). Pour utiliser le contrôle de carte dans votre propre application, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695).

## Schéma d’URI pour l’application Paramètres Windows


Votre application peut utiliser le schéma d’URI **ms-settings:** pour [lancer l’application Paramètres Windows](launch-settings-app.md). Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Par exemple, l’URI suivant ouvre l’application Paramètres et affiche les paramètres de confidentialité de l’appareil photo.

`ms-settings:privacy-webcam`

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations, voir [Lancer l’application Paramètres Windows](launch-settings-app.md) et [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/hh768223).

## Schéma d’URI pour l’application du Windows Store


Votre application peut utiliser le schéma d’URI **ms-windows-store:** pour [lancer l’application du Windows Store](launch-store-app.md). Ouvrez des pages de détails, d’avis et de recherche en rapport avec les produits. Par exemple, l’URI suivant ouvre l’application du Windows Store et accède à la page d’accueil du Windows Store.

`ms-windows-store://home/`

Pour plus d’informations, voir [Lancer l’application Windows Store](launch-store-app.md).

## Schéma d’URI pour l’application d’appel


Votre application peut utiliser le schéma d’URI **ms-call:** pour lancer l’application d’appel.

| Schéma d’URI       | Résultats                               |
|------------------|---------------------------------------|
| ms-call:settings | Lance la page de paramètres de l’application Appel. |

 

## Schéma d’URI pour l’application de conversation


Votre application peut utiliser le schéma d’URI **ms-chat:** pour lancer l’application Messages.

| Schéma d’URI                               | Résultats                                                                                                                                                                                |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ms-chat:                                 | Lance l’application de messagerie.                                                                                                                                                            |
| ms-chat:?ContactID={contacted}           | Permet le lancement de l’application de messagerie avec les informations d’un contact spécifique.                                                                                               |
| ms-chat:?Body={body}                     | Permet le lancement de l’application de messagerie avec une chaîne à utiliser en tant que contenu du message.                                                                                    |
| ms-chat:?Addresses={address}&amp;Body={body} | Permet le lancement de l’application de messagerie avec les informations d’adresse d’une personne particulière et une chaîne à utiliser en tant que contenu du message. Remarque : les adresses peuvent être concaténées. |
| ms-chat:?TransportId={transportId}       | Permet le lancement de l’application de messagerie avec un ID de transport particulier.                                                                                                        |

 

## Schéma d’URI pour le courrier électronique


Votre application peut utiliser le schéma d’URI **mailto:** pour lancer l’application de courrier électronique par défaut.

| Schéma d’URI               | Résultats                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mailto:                  | Lance l’application de courrier électronique par défaut.                                                                                                                             |
| mailto:\[email address\] | Lance l’application de courrier électronique et crée un message dont la ligne À contient l’adresse de messagerie spécifiée. Notez que le message n’est pas envoyé tant que l’utilisateur n’appuie pas sur Envoyer. |

 

## Schéma d’URI pour le protocole HTTP


Votre application peut utiliser le schéma d’URI **http:** pour lancer le navigateur web par défaut.

| Schéma d’URI | Résultats                           |
|------------|-----------------------------------|
| http:      | Lance le navigateur web par défaut. |

 

## Schéma d’URI pour l’application de recherche de numéros à proximité


Votre application peut utiliser le schéma d’URI **ms-yellowpage:** pour lancer l’application de recherche de numéros à proximité.

| Schéma d’URI                                            | Résultats                                                                               |
|-------------------------------------------------------|---------------------------------------------------------------------------------------|
| ms-yellowpage:?input=\[keyword\]&amp;method=\[String|T9\] | Lance l’application de recherche de points d’intérêt installée qui prend en charge ce nouvel URI. |

 

## Schéma d’URI pour l’application Contacts


Votre application peut utiliser le schéma d’URI **ms-people:** pour lancer l’application Contacts.

Pour plus d’informations, voir [Lancer l’application Contacts](launch-people-apps.md).

 

 



<!--HONumber=Jun16_HO4-->


