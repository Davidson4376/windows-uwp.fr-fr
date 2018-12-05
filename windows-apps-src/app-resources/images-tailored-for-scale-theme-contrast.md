---
Description: Your app can load image resource files containing images tailored for display scale factor, theme, high contrast, and other runtime contexts.
title: Charger des images et des ressources adaptées pour la mise à l’échelle, le thème, le contraste élevé et autres
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 6f4749b8560624ed58f43b33fe3373d909919347
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692592"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Charger des images et des ressources adaptées à l’échelle, au thème, au contraste élevé et à d’autres contextes
Votre application peut charger des fichiers de ressources d’image (ou d’autres fichiers de ressources) adaptés pour le [facteur d’échelle de l’affichage](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), le thème, le contraste élevé et d’autres contextes d’exécution. Ces images peuvent être référencées à partir du code impératif ou à partir du balisage XAML, par exemple en tant que propriété **Source** d’une **Image**. Elles peuvent également apparaître dans le fichier source de votre manifeste de votre package d’application (fichier `Package.appxmanifest`) &mdash; par exemple, en tant que la valeur de l’icône Application sur l’onglet Actifs visuels du Concepteur de manifeste de VisualStudio &mdash; ou sur vos vignettes et toasts. En utilisant des qualificateurs pour les noms de fichiers de vos images et, si nécessaire, en les chargeant de manière dynamique à l’aide d’un [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live), il est possible de charger le fichier image le plus approprié et correspondant le mieux aux paramètres d’exécution de l’utilisateur pour l’échelle de l’affichage, le thème, le contraste élevé, la langue et d’autres contextes.

Une ressource d’image est contenue dans un fichier de ressources d’image. Vous pouvez également imaginer l’image comme une ressource, et le fichier qui la contient comme un fichier de ressources. Ces types de fichiers de ressources se trouvent dans le dossier \Assets de votre projet. Pour obtenir des informations générales sur l’utilisation des qualificateurs dans les noms de vos fichiers de ressources d’image, voir [Personnaliser vos ressources pour la langue, la mise à l'échelle et d'autres qualificateurs](tailor-resources-lang-scale-contrast.md).

Les qualificateurs courants utilisés pour les images sont notamment [scale](tailor-resources-lang-scale-contrast.md#scale), [theme](tailor-resources-lang-scale-contrast.md#theme), [contrast](tailor-resources-lang-scale-contrast.md#contrast) et [targetsize](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Appliquer des qualificateurs de mise à l’échelle, de thème et de contraste à une image
La valeur par défaut du qualificateur `scale` est `scale-100`. Par conséquent, ces deux variantes sont équivalentes (elles fournissent toutes les deux une image à l’échelle100, ou avec le facteur d’échelle1).

```
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
```

Vous pouvez utiliser des qualificateurs dans les noms de dossiers au lieu des noms de fichiers. Cette stratégie est recommandée si vous disposez de plusieurs fichiers de ressources par qualificateur. À des fins d’exemple, ces deux variantes sont équivalentes aux deux variantes ci-dessus.

```
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
```

Voici maintenant une illustration de la façon dont vous pouvez fournir des variantes d’une ressource d’image &mdash; nommée `/Assets/Images/logo.png` &mdash; pour différents paramètres d’échelle de l’affichage, de thème et de contraste élevé. Cet exemple utilise des noms de dossiers.

```
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
```

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Faire référence à une image ou une autre ressource à partir du code et du balisage XAML
Le nom &mdash;ou identificateur&mdash; d'une ressource d’image correspond à son chemin d'accès et son nom de fichier avec tous les qualificateurs supprimés. Si vous nommez les dossiers et/ou fichiers comme dans l’un des exemples de la section précédente, vous disposez d’une ressource d’image unique et son nom (en tant que chemin d’accès absolu) est `/Assets/Images/logo.png`. Voici comment utiliser ce nom dans le balisage XAML.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Notez que vous utilisez le schéma d’URI `ms-appx`, car vous référencez un fichier qui provient de votre package d’application. Voir [Schémas d’URI](uri-schemes.md). Et voici comment vous référencez cette même ressource d’image en code impératif.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Vous pouvez utiliser `ms-appx` pour charger n’importe quel fichier arbitraire à partir de votre package d’application.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

Le schéma `ms-appx-web` accède aux mêmes fichiers que `ms-appx`, mais dans le compartiment web.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Pour tous les scénarios illustrés dans ces exemples, utilisez la surcharge de [constructeur Uri](https://docs.microsoft.com/en-us/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) qui déduit le [UriKind](https://docs.microsoft.com/en-us/dotnet/api/system.urikind). Spécifiez un URI absolu valide, y compris le schéma et l’autorité, ou laissez l’autorité accéder par défaut au package de l’application comme dans l’exemple ci-dessus.

Notez comment dans ces exemples d’URI, le schéma («`ms-appx`» ou «`ms-appx-web`») est suivi de «`://`», lui-même suivi d’un chemin d’accès absolu. Dans un chemin d’accès absolu, le caractère «`/`» de début indique que le chemin d’accès doit être interprété à partir de la racine du package.

**Remarque:** Les schémas d’URI `ms-resource` (pour les [ressources de chaînes](localize-strings-ui-manifest.md)) et `ms-appx(-web)` (pour les images et autres ressources) effectuent une mise en correspondance automatique des schémas d’URI pour trouver la ressource la plus appropriée pour le contexte actuel. Le schéma d’URI `ms-appdata` (qui est utilisé pour charger les données de l’application) n’effectue pas cette mise en correspondance automatique, mais vous pouvez répondre au contenu de [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) et charger explicitement les ressources appropriées à partir des données de l’application à l’aide de leur nom de fichier physique complet dans l’URI. Pour plus d’informations sur les données d’application, voir [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md). Les schémas d’URI web (par exemple, `http`, `https` et `ftp`) n’effectuent pas de mise en correspondance automatique non plus. Pour savoir comment procéder dans ce cas, voir [Hébergement et chargement d’images dans le cloud](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Les chemins d’accès absolus constituent un bon choix si vos fichiers d’image restent où ils se trouvent dans la structure du projet. Si vous voulez être en mesure de déplacer un fichier image, mais souhaitez qu’il reste dans le même emplacement par rapport à son fichier de balisageXAML de référence, alors au lieu d’utiliser un chemin d’accès absolu, vous pouvez envisager d’utiliser un chemin d’accès relatif au fichier de balisage le contenant. Si vous procédez ainsi, il n’est pas nécessaire d’utiliser un schéma d’URI. Vous pourrez toujours bénéficier de la mise en correspondance automatique des qualificateurs dans ce cas, mais uniquement parce que vous utilisez le chemin d’accès relatif dans le balisage XAML.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Voir aussi [Prise en charge des vignettes et toasts pour la langue, la mise à l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Appliquer le qualificateur targetsize à une ressource d’image
Vous pouvez utiliser les qualificateurs `scale` et `targetsize` sur différentes variantes de la même ressource d’image, mais vous ne pouvez pas les utiliser tous les deux sur une seule variante d’une ressource. Vous devez également définir au moins une variante sans qualificateur `TargetSize`. Cette variante doit définir une valeur pour `scale`, ou lui laisser la valeur par défaut `scale-100`. Par conséquent, ces deux variantes de la ressource `/Assets/Square44x44Logo.png` sont valides.

```
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
```

Et ces deux variantes sont également valides. 

```
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
```

Mais cette variante n’est pas valide.

```
\Assets\Square44x44Logo.scale-200_targetsize-24.png
```

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Référencer un fichier image à partir du manifeste de votre package d’application
Si vous nommez les dossiers et/ou fichiers comme dans l’un des deux exemples valides de la section précédente, vous disposez d’une ressource d’image unique d’icône d’application et son nom (en tant que chemin relatif) est `Assets\Square44x44Logo.png`. Dans le manifeste de votre package d’application, référencez simplement la ressource à l’aide de son nom. Il est inutile d’utiliser un schéma d’URI.

![ajouter une ressource, anglais](images/app-icon.png)

C’est tout ce que vous avez à faire et le système d’exploitation effectue une mise en correspondance automatique des qualificateurs pour trouver la ressource la plus appropriée pour le contexte actuel. Pour obtenir une liste de tous les éléments dans le manifeste du package d’application que vous pouvez localiser ou pour lesquels sinon vous pouvez appliquer un qualificateur de cette façon, voir [Éléments localisables du manifeste](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Appliquer le qualificateur layoutdirection à une ressource d’image
Voir [Mise en miroir des images](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Charger une image pour une langue spécifique ou un autre contexte
Pour plus d’informations sur la proposition de valeur de la localisation de votre application, voir [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

Le [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) par défaut (obtenu à partir de [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contient une valeur de qualificateur pour chaque nom de qualificateur, représentant le contexte d’exécution par défaut (en d’autres termes, les paramètres de l’utilisateur et de l’ordinateur actuels). Les fichiers image sont mis en correspondance &mdash; en fonction des qualificateurs figurant dans leurs noms &mdash; avec les valeurs de qualificateur dans ce contexte d’exécution.

Mais, dans certains cas, vous souhaiterez que votre application remplace les paramètres système et indique de manière explicite les valeurs des qualificateurs de langue, de mise à l’échelle ou autre à utiliser lors de la recherche d’une image correspondante à charger. Par exemple, vous souhaiterez peut-être contrôler avec précision quelles images à contraste élevé sont chargées et le moment où le chargement est effectué.

Pour ce faire, vous pouvez créer un nouveau **ResourceContext** (au lieu d’utiliser celui par défaut), remplacer ses valeurs, puis utiliser cet objet de contexte dans vos recherches d’images.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

Pour obtenir le même effet à un niveau global, vous *pouvez* remplacer les valeurs de qualificateur dans le **ResourceContext** par défaut. Mais nous vous conseillons plutôt d’appeler [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Vous définissez les valeurs une fois avec l’appel de **SetGlobalQualifierValue**, puis ces valeurs sont appliquées pour le **ResourceContext** par défaut chaque fois que vous l’utiliser pour les recherches. Par défaut, la classe [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) utilise le **ResourceContext** par défaut.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Mise à jour d’images suite à des événements de modification de valeur de qualificateur
Votre application en cours d’exécution peut répondre à des modifications de paramètres système qui affectent les valeurs de qualificateur dans le contexte de ressources par défaut. Ces paramètres système appellent l’événement [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) sur [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

Suite à cet événement, vous pouvez recharger vos images à l’aide du **ResourceContext** par défaut, utilisé par défaut par [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live).

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>API importantes
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Rubriques associées
* [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md)
* [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md)
* [Prise en charge des vignettes et toasts pour la langue, la mise à l’échelle et le contraste élevé](tile-toast-language-scale-contrast.md)
* [Éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Mise en miroir des images](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Internationalisation et localisation](../design/globalizing/globalizing-portal.md)
