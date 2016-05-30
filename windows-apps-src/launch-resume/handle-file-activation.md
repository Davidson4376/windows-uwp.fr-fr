---
author: mcleblanc
title: Gérer l’activation des fichiers
description: Une application peut s’inscrire afin de devenir le gestionnaire par défaut pour un certain type de fichier.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
---

# Gérer l’activation des fichiers


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

Une application peut s’inscrire afin de devenir le gestionnaire par défaut pour un certain type de fichier. Tant les applications de plateforme Windows classique (CWP) que les applications de plateforme Windows universelle (UWP) peuvent s’inscrire pour devenir gestionnaire de fichiers par défaut. Si l’utilisateur choisit votre application en tant que gestionnaire par défaut pour un certain type de fichier, celle-ci sera activée lors du lancement de ce type de fichier.

Nous vous recommandons de vous inscrire uniquement pour un type de fichier si vous pensez gérer tous les lancements de fichiers pour ce type. Si votre application nécessite uniquement d’utiliser ce type de fichier en interne, vous ne devez pas vous inscrire pour devenir le gestionnaire par défaut. Si vous choisissez de vous inscrire pour un type de fichier, vous devez fournir à l’utilisateur final la fonctionnalité attendue lorsque votre application est activée pour ce type de fichier. Par exemple, une visionneuse d’images doit s’inscrire pour afficher un fichier .jpg. Pour plus d’informations sur les associations de fichiers, voir [Recommandations en matière de types de fichier et d’URI](https://msdn.microsoft.com/library/windows/apps/hh700321).

Ces étapes montrent comment s’inscrire pour un type de fichier personnalisé, alsdk, et comment activer votre application quand l’utilisateur lance un fichier alsdk.

> **Remarque** Dans les applications UWP, certains URI et certaines extensions de fichier ne peuvent être utilisés que par des applications intégrées et le système d’exploitation. Toute tentative d’inscription de votre application avec une extension de fichier ou un URI réservés sera ignorée. Pour plus d’informations, voir [Noms de schéma d’URI et de fichier réservé](reserved-uri-scheme-names.md).

## Étape 1 : spécifier le point d’extension dans le manifeste du package


L’application reçoit des événements d’activation uniquement pour les extensions de fichiers répertoriées dans le manifeste du package. Procédez comme suit pour indiquer que votre application gère les fichiers portant l’extension `.alsdk`.

1.  Dans l’**Explorateur de solutions**, double-cliquez sur package.appxmanifest pour ouvrir le concepteur de manifeste. Sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichier**, puis cliquez sur **Ajouter**. Pour plus d’informations sur les identificateurs utilisés par les associations de fichiers, voir [Identificateurs programmatiques](https://msdn.microsoft.com/library/windows/desktop/cc144152).

    Voici une brève description de chacun des champs que vous pouvez remplir dans le concepteur de manifeste :

| Champ | Description |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nom complet** | Spécifiez le nom complet d’un groupe de types de fichiers. Le nom d’affichage sert à identifier le type de fichier dans l’option [Définir les programmes par défaut](https://msdn.microsoft.com/library/windows/desktop/cc144154) du **Panneau de configuration**. |
| **Logo** | Spécifiez le logo utilisé pour identifier le type de fichier sur le Bureau et dans l’option [Définir les programmes par défaut](https://msdn.microsoft.com/library/windows/desktop/cc144154) du **Panneau de configuration**. Si aucun logo n’est spécifié, le petit logo de l’application est utilisé. |
| **Info-bulle** | Spécifiez l’[info-bulle](https://msdn.microsoft.com/library/windows/desktop/cc144152) d’un groupe de types de fichier. Cette info-bulle s’affiche quand l’utilisateur pointe sur l’icône d’un fichier de ce type avec la souris. |
| **Nom** | Choisissez un nom pour un groupe de types de fichiers partageant les mêmes nom complet, logo, info-bulle et indicateurs de modification. Choisissez un nom de groupe pouvant rester le même sur toutes les applications à mettre à jour. **Remarque** Le nom doit être entièrement en minuscules. |
| **Type de contenu** | Spécifiez le type de contenu MIME, par exemple **image/jpeg**, pour un type de fichier particulier. **Remarque importante sur les types de contenu autorisés :** voici la liste alphabétique des types de contenu MIME que vous ne pouvez pas entrer dans le manifeste du package, car ils sont réservés ou interdits : **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **Type de fichier** | Précisez le type de fichier à inscrire précédé d’un point (par exemple, « .jpeg »). **Types de fichier réservé et interdit** Pour obtenir la liste alphabétique des types de fichier pour les applications intégrées que vous ne pouvez pas inscrire pour vos applications UWP parce qu’ils sont réservés ou interdits, voir [Noms de schéma d’URI réservé et types de fichier](reserved-uri-scheme-names.md). |

2.  Entrez `alsdk` comme **Nom**.
3.  Entrez `.alsdk` comme **Type de fichier**.
4.  Entrez « images\Icon.png » comme Logo.
5.  Appuyez sur Ctrl+S pour enregistrer la modification dans package.appxmanifest.

Les étapes ci-dessus ajoutent un élément [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) tel que celui-ci dans le manifeste du package. La catégorie **windows.fileTypeAssociation** indique que l’application gère les fichiers portant l’extension `.alsdk`.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## Étape 2 : Ajouter les icônes appropriées


Les applications qui deviennent la valeur par défaut d’un type de fichier ont leurs icônes affichées à différents emplacements dans l’ensemble du système. Par exemple, ces icônes s’affichent dans :

-   la vue d’éléments de l’Explorateur Windows, les menus contextuels et le Ruban ;
-   l’applet Programmes par défaut du Panneau de configuration ;
-   le sélecteur de fichiers ;
-   les résultats de recherche sur l’écran d’accueil.

Reproduisez l’apparence du logo de la vignette de l’application et utilisez la couleur d’arrière-plan de celle-ci au lieu de rendre l’icône transparente. Faites en sorte que le logo s’étende jusqu’au bord sans remplissage. Testez vos icônes sur des arrière-plans blancs. Pour obtenir des exemples d’icônes, voir [Exemple de lancement d’association](http://go.microsoft.com/fwlink/p/?LinkID=620490).
![explorateur de solutions avec une vue des fichiers dans le dossier images. il existe des versions de 16, 32, 48 et 256 pixels de « icon.targetsize » et de « smalltile-sdk ».](images/seviewofimages.png)

## Étape 3 : Gérer l’événement activé


Le gestionnaire d’événements [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) reçoit tous les événements d’activation des fichiers.

> [!div class="tabbedCodeSnippets"]
```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```
```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
       // TODO: Handle file activation
       // The number of files received is args->Files->Size
       // The first file is args->Files->GetAt(0)->Name
}
```
```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

    > **Note**  When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

Nous recommandons que les applications créent une image XAML pour chaque événement d’activation qui ouvre une nouvelle page. De cette façon, le backstack de navigation pour la nouvelle image XAML ne contient aucune partie du contenu précédent pouvant figurer dans la fenêtre active de l’application au moment de la suspension. Les applications qui décident d’utiliser une seule image XAML pour le lancement et les contrats de fichier doivent effacer les pages du journal de navigation de l’image avant de naviguer vers une nouvelle page.

En cas de lancement via l’activation de fichier, les applications doivent envisager d’inclure une interface utilisateur permettant à l’utilisateur de revenir à la première page de l’application.

## Remarques


Les fichiers que vous recevez peuvent provenir d’une source non approuvée. Nous vous recommandons de vérifier le contenu d’un fichier avant d’entreprendre une quelconque action sur ce fichier. Pour plus d’informations sur la validation d’entrée, voir [Écriture de code sécurisé](http://go.microsoft.com/fwlink/p/?LinkID=142053).

> **Remarque** Cet article s’adresse aux développeurs Windows 10 qui développent des applications de plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes

**Exemple complet**

* [Exemple de lancement d’association](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**Concepts**

* [Programmes par défaut](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Modèle d’associations de types de fichiers et de protocoles](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**Tâches**

* [Lancer l’application par défaut d’un fichier](launch-the-default-app-for-a-file.md)
* [Gérer l’activation des URI](handle-uri-activation.md)

**Recommandations**

* [Recommandations en matière de types de fichiers et d’URI](https://msdn.microsoft.com/library/windows/apps/hh700321)

**Référence**
* [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
* [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

 

 





<!--HONumber=May16_HO2-->


