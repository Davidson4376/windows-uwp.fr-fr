---
author: TylerMSFT
title: Gérer l’activation des URI
description: Découvrez comment inscrire une application afin qu’elle devienne le gestionnaire par défaut pour un nom de schéma d’URI (Uniform Resource Identifier).
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41c7286493e08fd62ad4b207d0e014dd4fbd5318
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3961023"
---
# <a name="handle-uri-activation"></a>Gérer l’activation des URI

**API importantes**

-   [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
-   [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

Découvrez comment inscrire une application afin qu’elle devienne le gestionnaire par défaut pour un nom de schéma d’URI (Uniform Resource Identifier). Tant les applications de bureau Windows que les applications de plateforme Windows universelle (UWP) peuvent s’inscrire pour devenir gestionnaire par défaut pour un nom de schéma d’URI. Si l’utilisateur choisit votre application en tant que gestionnaire par défaut pour un nom de schéma d’URI, celle-ci sera activée à chaque lancement de ce type d’URI.

Nous vous recommandons de vous inscrire pour un nom de schéma d’URI uniquement si vous pensez gérer tous les lancements d’URI pour ce type de schéma d’URI. Si vous choisissez de vous inscrire pour un nom de schéma d’URI, vous devez fournir à l’utilisateur final la fonctionnalité attendue lorsque votre application est activée pour ce schéma d’URI. Par exemple, une application qui s’inscrit pour le nom de schéma d’URI mailto: doit ouvrir un nouveau message électronique de sorte que l’utilisateur puisse composer un nouveau message électronique. Pour plus d’informations sur les associations d’URI, voir [Recommandations et liste de vérification des types de fichier et des URI](https://msdn.microsoft.com/library/windows/apps/hh700321).

Ces étapes montrent comment s’inscrire pour un nom de schéma d’URI personnalisé, `alsdk://`, et comment activer votre application quand l’utilisateur lance un URI `alsdk://`.

> [!NOTE]
> Dans les applications UWP, certaines extensions de fichier et les URI sont réservées pour une utilisation par les applications intégrées et le système d’exploitation. Toute tentative d’inscription de votre application avec une extension de fichier ou un URI réservés sera ignorée. Pour obtenir la liste alphabétique des schémas d’URI que vous ne pouvez pas inscrire pour vos applications UWP parce qu’ils sont réservés ou interdits, voir [Noms de schéma d’URI réservé et types de fichier](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Étape 1 : spécifier le point d’extension dans le manifeste du package

L’application reçoit des événements d’activation uniquement pour les noms de schémas d’URI répertoriés dans le manifeste du package. Procédez comme suit pour indiquer que votre application gère le nom de schéma d’URI `alsdk`.

1. Dans l’**Explorateur de solutions**, double-cliquez sur package.appxmanifest pour ouvrir le concepteur de manifeste. Sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Protocole**, puis cliquez sur **Ajouter**.

    Voici une brève description de chacun des champs que vous pouvez remplir dans le concepteur de manifeste pour le protocole (voir [**Manifeste du package AppX**](https://msdn.microsoft.com/library/windows/apps/dn934791) pour plus de détails):

| Champ | Description |
|-------|-------------|
| **Logo** | Spécifiez le logo qui est utilisé pour identifier le nom de schéma d’URI dans [Définir les programmes par défaut](https://msdn.microsoft.com/library/windows/desktop/cc144154) du **Panneau de configuration**. Si aucune valeur n’est spécifiée pour Logo, le petit logo de l’application est utilisé. |
| **Nom d’affichage** | Spécifiez le nom d’affichage pour identifier le nom de schéma d’URI dans [Définir les programmes par défaut](https://msdn.microsoft.com/library/windows/desktop/cc144154) du **Panneau de configuration**. |
| **Nom** | Choisissez le nom du schéma d’URI. |
|  | **Remarque**  Le nom doit être indiqué entièrement en minuscules. |
|  | **Types de fichier réservé et interdit** Pour obtenir la liste alphabétique des schémas d’URI que vous ne pouvez pas inscrire pour vos applications UWP parce qu’ils sont réservés ou interdits, voir [Noms de schéma d’URI réservé et types de fichier](reserved-uri-scheme-names.md). |
| **Exécutable** | Spécifie l’exécutable de lancement par défaut pour le protocole. En l’absence de spécification, le fichier exécutable de l’application est utilisé. En cas de spécification, la chaîne doit contenir 1 à 256 caractères et se terminer par « .exe ». Elle ne peut pas contenir les caractères suivants : &gt;, &lt;, :, ", &#124;, ? ou \*. En cas de spécification, le **Point d’entrée** est également utilisé. Si le **Point d’entrée** n’est pas spécifié, le point d’entrée défini pour l’application est utilisé. |
| **Point d’entrée** | Spécifie la tâche qui gère l’extension de protocole. Il s’agit généralement du nom complet de l’espace de noms d’un type Windows Runtime. En l’absence de spécification, le point d’entrée de l’application est utilisé. |
| **Page de démarrage** | La page web qui gère le point d’extensibilité. |
| **Groupe de ressources** | Une balise que vous pouvez utiliser pour regrouper les activations d’extensions à des fins de gestion des ressources. |
| **Affichage souhaité** (Windows uniquement) | Spécifiez le champ **Affichage souhaité** pour indiquer la quantité d’espace nécessaire à la fenêtre de l’application quand elle est lancée pour le nom de schéma d’URI. Les valeurs possibles du champ **Affichage souhaité** sont **Default**, **UseLess**, **UseHalf**, **UseMore** ou **UseMinimum**.<br/>**Remarque**  Windows tient compte de différents facteurs pour déterminer la taille finale de la fenêtre de l’application cible, par exemple, la préférence de l’application source, le nombre d’applications à l’écran, l’orientation de l’écran, etc. La définition du champ **Affichage souhaité** ne garantit pas un comportement de fenêtrage spécifique pour l’application cible.<br/>**Famille d’appareils mobiles: Affichage souhaité** n’est pas pris en charge dans la famille d’appareils mobiles. |

2. Entrez `images\Icon.png` comme **Logo**.
3. Entrez `SDK Sample URI Scheme` comme **Nom d’affichage**.
4. Entrez `alsdk` comme **Nom**.
5. Appuyez sur Ctrl+S pour enregistrer la modification dans package.appxmanifest.

    Cette opération ajoute un élément [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) tel que celui-ci dans le manifeste du package. La catégorie **windows.protocol** indique que l’application gère le nom de schéma d’URI `alsdk`.

```xml
    <Applications>
        <Application Id= ... >
            <Extensions>
                <uap:Extension Category="windows.protocol">
                  <uap:Protocol Name="alsdk">
                    <uap:Logo>images\icon.png</uap:Logo>
                    <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
                  </uap:Protocol>
                </uap:Extension>
          </Extensions>
          ...
        </Application>
   <Applications>
```

## <a name="step-2-add-the-proper-icons"></a>Étape 2: Ajouter les icônes appropriées

Les applications qui deviennent la valeur par défaut d’un nom de schéma d’URI ont leurs icônes affichées à différents emplacements dans l’ensemble du système, par exemple dans l’applet Programmes par défaut du Panneau de configuration. Incluez une icône 44x44 dans votre projet à cet effet. Reproduisez l’apparence du logo de la vignette de l’application et utilisez la couleur d’arrière-plan de celle-ci au lieu de rendre l’icône transparente. Faites en sorte que le logo s’étende jusqu’au bord sans remplissage. Testez vos icônes sur des arrière-plans blancs. Voir [Recommandations en matière de ressources de vignette et d’icône](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets) pour plus d’informations sur les icônes.

## <a name="step-3-handle-the-activated-event"></a>Étape3: Gérer l’événement activé

Le gestionnaire d’événements [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) reçoit tous les événements d’activation. La propriété **Kind** indique le type d’événement d’activation. Cet exemple est défini pour gérer les événements d’activation [**Protocol**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.activation.activationkind.aspx#Protocol).

```csharp
public partial class App
{
   protected override void OnActivated(IActivatedEventArgs args)
  {
      if (args.Kind == ActivationKind.Protocol)
      {
         ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
         // TODO: Handle URI activation
         // The received URI is eventArgs.Uri.AbsoluteUri
      }
   }
}
```

```vb
Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
   If args.Kind = ActivationKind.Protocol Then
      ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
      
      ' TODO: Handle URI activation
      ' The received URI is eventArgs.Uri.AbsoluteUri
 End If
End Sub
```

```cppwinrt
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
    {
        auto protocolActivatedEventArgs{ args.as<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs>() };
        // TODO: Handle URI activation  
        auto receivedURI{ protocolActivatedEventArgs.Uri().RawUri() };
    }
}
```

```cpp
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
{
   if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
   {
      Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs =
          dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
      
      // TODO: Handle URI activation  
      // The received URI is eventArgs->Uri->RawUri
   }
}
```

> [!NOTE]
> En cas de lancement via un contrat de protocole, assurez-vous que le bouton précédent fait revenir l’utilisateur à l’écran qui a lancé l’application et non au contenu précédent de l’application.

Le code suivant lance par programmation l’application via son URI:

```csharp
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

Pour plus de détails sur le lancement d’une application via un URI, voir [Lancer l’application par défaut pour unURI](launch-default-app.md).

Nous recommandons que les applications créent un [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) XAML pour chaque événement d’activation qui ouvre une nouvelle page. De cette façon, la pile arrière («backstack») de navigation pour le nouveau **Frame** XAML ne contient aucune partie du contenu précédent pouvant figurer dans la fenêtre active de l’application au moment de la suspension. Les applications qui décident d’utiliser un seul **Frame** XAML pour le lancement et les contrats de fichier doivent effacer les pages du journal de navigation du **Frame** avant de naviguer vers une nouvelle page.

En cas de lancement via l’activation de protocole, les applications doivent envisager d’inclure une interface utilisateur permettant à l’utilisateur de revenir à la première page de l’application.

## <a name="remarks"></a>Remarques

N’importe quelle application ou n’importe quel site web peut utiliser votre nom de schéma d’URI, y compris des applications et sites malveillants. Par conséquent, toute donnée reçue dans cet URI peut provenir d’une source non approuvée. Nous vous recommandons de ne jamais effectuer une action permanente en fonction des paramètres que vous recevez dans un URI. Par exemple, les paramètres d’URI peuvent être utilisés pour lancer l’application sur la page de compte d’un utilisateur, mais nous vous recommandons de ne jamais les utiliser pour modifier directement le compte de l’utilisateur.

> [!NOTE]
> Si vous créez un nouveau nom de schéma d’URI pour votre application, veillez à suivre les recommandations de [RFC 4395](http://go.microsoft.com/fwlink/p/?LinkID=266550). Cela permet de garantir que votre nom respecte les normes applicables aux schémas d’URI.

> [!NOTE]
> En cas de lancement via un contrat de protocole, assurez-vous que le bouton précédent fait revenir l’utilisateur à l’écran qui a lancé l’application et non au contenu précédent de l’application.

Nous recommandons que les applications créent un [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) XAML pour chaque événement d’activation qui ouvre une nouvelle cible d’URI. De cette façon, la pile arrière («backstack») de navigation pour le nouveau **Frame** XAML ne contient aucune partie du contenu précédent pouvant figurer dans la fenêtre active de l’application au moment de la suspension.

Si vous décidez que vos applications doivent utiliser un seul [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) XAML pour le lancement et les contrats de protocole, effacez les pages du journal de navigation du **Frame** avant de naviguer vers une nouvelle page. En cas de lancement via le contrat de protocole, envisagez d’inclure une interface utilisateur permettant à l’utilisateur de revenir en haut de l’application.

## <a name="related-topics"></a>Rubriquesassociées

### <a name="complete-sample-app"></a>Exemple complet d’application

- [Exemple de lancement d’association](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>Concepts

- [Programmes par défaut](https://msdn.microsoft.com/library/windows/desktop/cc144154)
- [Modèle d’associations de types de fichiers et d’URI](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>Tâches

- [Lancer l’application par défaut pour un URI](launch-default-app.md)
- [Gérer l’activation des fichiers](handle-file-activation.md)

### <a name="guidelines"></a>Recommandations

- [Recommandations en matière de types de fichiers et d’URI](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Référence

- [Manifeste du package AppX](https://msdn.microsoft.com/library/windows/apps/dn934791)
- [Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224742)
- [Windows.UI.Xaml.Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330)
