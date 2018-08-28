---
author: stevewhims
Description: If you want your app to support different display languages, and you have string literals in your code or XAML markup or app package manifest, then move those strings into a Resources File (.resw). You can then make a translated copy of that Resources File for each language that your app supports.
title: Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.author: stwhi
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: c9db9f3ce4397bec6fb0b6b339875c206d17c3fd
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2886121"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application
Pour plus d’informations sur la proposition de valeur de la localisation de votre application, voir [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

Si vous souhaitez que votre application prenne en charge différentes langues d’affichage et si des opérateurs de chaîne figurent dans votre code, dans votre balisage XAML ou dans le manifeste du package d’application, déplacez ces chaînes dans un fichier de ressources (.resw). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application.

Des opérateurs de chaîne codés en dur peuvent apparaître dans le code impératif ou dans le balisage XAML, par exemple en tant que propriété **Text** d’un **TextBlock**. Ils peuvent également apparaître dans le fichier source du manifeste de votre package d’application (fichier `Package.appxmanifest`), par exemple, en tant que valeur de nom d’affichage sur l’onglet Application du Concepteur de manifeste de VisualStudio. Déplacez ces chaînes dans un fichier de ressources (.resw) et remplacez les opérateurs de chaîne codés en dur dans votre application et dans votre manifeste par des références à des identificateurs de ressources.

Contrairement aux ressources d’image, où un fichier de ressource d’image contient une seule ressource d’image, un fichier de ressources de chaîne contient *plusieurs* ressources de type chaîne. Un fichier de ressources de chaîne est un fichier de ressources (.resw), et vous créez généralement ce type de fichier de ressources dans un dossier \Strings dans votre projet. Pour obtenir des informations générales sur l’utilisation de qualificateurs dans les noms de vos fichiers de ressources (.resw), voir [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md).

## <a name="create-a-resources-file-resw-and-put-your-strings-in-it"></a>Créer un fichier de ressources (.resw) et placer vos chaînes dans ce fichier
1. Définissez la langue par défaut de votre application.
    1. Votre solution de jeu étant ouverte dans VisualStudio, ouvrez `Package.appxmanifest`.
    2. Sous l’onglet Application, vérifiez que la langue par défaut est correctement définie (par exemple, «en» ou «en-US»). Les étapes restantes supposent que vous avez défini la langue par défaut sur «en-US».
    <br>**Remarque:** vous devez au minimum fournir des ressources de chaîne localisées pour cette langue par défaut. Il s’agit des ressources qui seront chargées en l’absence d’une meilleure correspondance pour les paramètres de langue ou de langue d’affichage par défaut de l’utilisateur.
2. Créez un fichier de ressources (.resw) pour la langue par défaut.
    1. Sous le nœud de votre projet, créez un dossier et nommez-le «Strings».
    2. Sous `Strings`, créez un sous-dossier et nommez-le «en-US».
    3. Sous `en-US`, créez un fichier de ressources (.resw) et vérifiez qu’il est nommé «Resources.resw».
    <br>**Remarque:** si vous voulez porter des fichiers de ressources.NET (.resx), consultez [Portage du balisage XAML et de la couche interface utilisateur](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3.  Ouvrez `Resources.resw` et ajoutez ces ressources de chaîne.

    `Strings/en-US/Resources.resw`

    ![ajouter une ressource, anglais](images/addresource-en-us.png)

    Dans cet exemple, «Greeting» est un identificateur de ressource de chaîne auquel vous pouvez faire référence depuis votre balisage, comme nous allons le montrer. Pour l’identificateur «Greeting», une chaîne est fournie pour une propriété Text, et une autre chaîne est également fournie pour une propriété Width. «Greeting.Text» est un exemple d’identificateur de propriété, car il correspond à une propriété d’un élément d’interface utilisateur. Vous pourriez également, par exemple, ajouter «Greeting.Foreground» dans la colonne Nom et définir sa valeur sur «Red». L’identificateur «Farewell» est un identificateur de ressource de chaîne simple; il ne possède aucune sous-propriété et peut être chargé à partir du code impératif, comme nous allons le montrer. La colonne Commentaires est idéale pour fournir des instructions particulières aux traducteurs.

    Dans cet exemple, dans la mesure où nous avons une entrée d’identificateur de ressource de chaîne simple nommée «Farewell», nous ne pouvons pas avoir *également* d’identificateurs de propriété basés sur ce même identificateur. Par conséquent, l’ajout de «Farewell.Text» provoquerait une erreur Entrée dupliquée lors de la génération de `Resources.resw`.

    Les identificateurs de ressources ne respectent pas la casse et doivent être uniques pour chaque fichier de ressources. Veillez à utiliser des identificateurs de ressources significatifs pour fournir un contexte supplémentaire aux traducteurs. De plus, ne modifiez pas les identificateurs de ressources après l’envoi aux traducteurs des ressources de type chaîne. Les équipes de localisation utilisent l’identificateur de ressource pour effectuer le suivi des ajouts, des suppressions et des mises à jour dans les ressources. Les modifications apportées dans les identificateurs de ressources (appelées également «transition des identificateurs de ressources») nécessitent la retraduction des chaînes, car il semble que des chaînes aient été supprimées et d’autres ajoutées.

## <a name="refer-to-a-string-resource-identifier-from-xaml-markup"></a>Faire référence à un identificateur de ressource de chaîne à partir du balisage XAML
Vous utilisez une [directive x:Uid](../xaml-platform/x-uid-directive.md) pour associer un contrôle ou un autre élément de votre balisage à un identificateur de ressource de chaîne.

```xaml
<TextBlock x:Uid="Greeting"/>
```

Lors de l’exécution, `\Strings\en-US\Resources.resw` est chargé (étant donné qu’il est le seul fichier de ressources dans le projet pour le moment). La **directive x:Uid** sur **TextBlock** génère une recherche des identificateurs de propriété à l’intérieur du `Resources.resw` qui contient l’identificateur de ressource de chaîne «Greeting». Les identificateurs de propriété «Greeting.Text» et «Greeting.Width» sont trouvés et leurs valeurs sont appliquées au **TextBlock**, en remplaçant les valeurs définies localement dans le balisage. La valeur «Greeting.Foreground» serait également appliquée, si nous l’avions ajoutée. Mais seuls les identificateurs de propriété sont utilisés pour définir les propriétés des éléments de balisage XAML. Par conséquent, la définition de la directive **x:Uid** sur «Farewell» sur ce TextBlock n’aurait aucun effet. `Resources.resw` contient *bien* l’identificateur de ressource de chaîne «Farewell», mais il ne contient aucun identificateur de propriété pour celui-ci.

Lorsque vous affectez un identificateur de ressource de chaîne à un élément XAML, assurez-vous que *tous* les identificateurs de propriété de cet identificateur sont corrects pour l’élément XAML. Par exemple, si vous définissez `x:Uid="Greeting"` sur un **TextBlock**, «Greeting.Text» sera résolu, car le type **TextBlock** a une propriété Text. Mais si vous définissez `x:Uid="Greeting"` sur un **Button**, alors «Greeting.Text» provoquera une erreur d’exécution, car le type **Button** n’a pas de propriété Text. Dans ce cas, une solution consiste à créer un identificateur de propriété nommé «ButtonGreeting.Content» et à définir `x:Uid="ButtonGreeting"` sur le **Button**.

Au lieu de définir **Width** à partir d’un fichier de ressources, vous souhaiterez probablement autoriser les contrôles à se redimensionner de manière dynamique en fonction du contenu.

**Remarque:** pour les [propriétés jointes](../xaml-platform/attached-properties-overview.md), une syntaxe spéciale est requise dans la colonne Nom d’un fichier .resw. Par exemple, pour définir une valeur pour la propriété jointe [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) de l’identificateur «Greeting», voici ce que vous devez entrer dans la colonne Nom.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Faire référence à un identificateur de ressource de chaîne à partir du code
Vous pouvez charger de manière explicite une ressource de chaîne basée sur un identificateur de ressource de chaîne simple.

> [!NOTE]
> Si vous disposez d’un appel à une quelconque méthode **GetForCurrentView** qui *pourrait* être exécutée dans un thread d’arrière-plan/de travail, protégez cet appel par un test `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`. Appeler **GetForCurrentView** à partir des résultats d’un thread d’arrière-plan/de travail dans l’exception «*&lt;typename&gt; ne peut pas être créé dans des threads dépourvus d’un objet CoreWindow. *»

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

Vous pouvez utiliser ce même code à partir d’un projet de bibliothèque de classes (Windows universel) ou de [bibliothèque Windows Runtime (Windows universel)](../winrt-components/index.md). Lors de l’exécution, les ressources de l’application qui héberge la bibliothèque sont chargées. Il est recommandé qu’une bibliothèque charge les ressources à partir de l’application qui l’héberge, car l’application est susceptible d’avoir un niveau plus élevé de localisation. Si une bibliothèque doit fournir des ressources, elle doit donner à l’application qui l’héberge l’option de remplacer ces ressources en tant qu’entrée.

Si un nom de la ressource est segmenté (il contient «.» caractères), puis replace points avec une barre oblique («/») en caractères dans le nom de la ressource. Identificateurs de propriété, par exemple, contient des points; Ainsi, vous devez effectuer cette substitution afin de charger un de ceux à partir du code.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

En cas de doute, vous pouvez utiliser [MakePri.exe](makepri-exe-command-options.md) pour vider le fichier PRI de votre application. Chaque ressource `uri` est indiqué dans le fichier dumping.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Faire référence à un identificateur de ressource de chaîne à partir du manifeste du package d’application
1. Ouvrez le fichier source du manifeste de votre package d’application (fichier `Package.appxmanifest`), dans lequel le nom d’affichage de votre application est exprimé sous forme de chaîne littérale par défaut.

   ![ajouter une ressource, anglais](images/display-name-before.png)

2. Pour obtenir une version localisable de cette chaîne, ouvrez `Resources.resw` et ajoutez une nouvelle ressource de chaîne avec le nom «AppDisplayName» et la valeur «Adventure Works Cycles».

3. Remplacez la chaîne littérale du nom d’affichage par une référence à l’identificateur de ressource de chaîne que vous venez de créer («AppDisplayName»). Vous utilisez le schéma d’URI (Uniform Resource Identifier) `ms-resource` pour effectuer cette opération.

   ![ajouter une ressource, anglais](images/display-name-after.png)

4. Répétez la procédure pour chaque chaîne à localiser dans votre manifeste. Par exemple, le nom court de votre application (que vous pouvez configurer afin qu’il s’affiche sur la vignette de votre application dans le menu Démarrer). Pour obtenir une liste de tous les éléments du manifeste de package d’application que vous pouvez localiser, voir [Éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Localiser les ressources de chaîne
1. Faites une copie de votre fichier de ressources (.resw) pour une autre langue.
    1. Sous «Strings», créez un sous-dossier et nommez-le «de-DE» pour Allemand (Allemagne).
   <br>**Remarque:** vous pouvez utiliser n’importe quelle [balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) pour le nom du dossier. Voir [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md) pour plus d’informations sur le qualificateur de langue et une liste des balises de langue courantes.
   2. Faites une copie de `Strings/en-US/Resources.resw` dans le dossier `Strings/de-DE`.
2. Traduisez les chaînes.
    1. Ouvrez `Strings/de-DE/Resources.resw` et traduisez les valeurs dans la colonne Valeur. Il n’est pas nécessaire de traduire les commentaires.

    `Strings/de-DE/Resources.resw`

    ![ajouter une ressource, allemand](images/addresource-de-de.png)

Si vous le souhaitez, vous pouvez répéter les étapes1 et2 pour une autre langue.

`Strings/fr-FR/Resources.resw`

![ajouter une ressource, français](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Tester votre application
Testez l’application dans votre langue d’affichage par défaut. Vous pouvez ensuite modifier la langue d’affichage sous **Paramètres** > **Heure et langue** > **Région et langue** > **Langues** et tester une nouvelle fois votre application. Examinez les chaînes dans votre interface utilisateur et dans le shell (par exemple, votre barre de titre, qui est votre nom d’affichage, et le nom court dans vos vignettes).

**Remarque:** si vous trouvez un nom de dossier correspondant au paramètre de langue d’affichage, cela signifie que le fichier de ressources figurant dans ce dossier est chargé. Dans le cas contraire, un basculement a lieu, finissant avec les ressources de la langue par défaut de votre application.

## <a name="factoring-strings-into-multiple-resources-files"></a>Factorisation de chaînes dans plusieurs fichiers de ressources
Vous pouvez soit conserver toutes vos chaînes dans un seul fichier de ressources (resw), soit les factoriser dans plusieurs fichiers de ressources. Par exemple, vous voudrez peut-être conserver vos messages d’erreur dans un fichier de ressources, les chaînes du manifeste de votre package d’application dans un autre et vos chaînes d’interface utilisateur dans un troisième. Dans ce cas, voici comment se présenterait votre structure de dossiers.

![ajouter une ressource, anglais](images/manifest-resources.png)

Pour définir l’étendue d’une référence d’identificateur de ressource chaîne à un fichier spécifique, ajoutez simplement `/<resources-file-name>/` avant l’identificateur. L’exemple de balisage suivant suppose que `ErrorMessages.resw` contient une ressource nommée «PasswordTooWeak.Text» dont la valeur décrit l’erreur.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Il vous suffit d’ajouter `/<resources-file-name>/` avant l’identificateur de ressource de chaîne pour les fichiers de ressources *autres que* `Resources.resw`. En effet, «Resources.resw» est le nom de fichier par défaut et c’est donc ce qui est supposé si vous omettez le nom de fichier (comme nous l’avons fait dans les exemples précédents de cette rubrique).

L’exemple de code suivant suppose que `ErrorMessages.resw` contient une ressource nommée «MismatchedPasswords» dont la valeur décrit l’erreur.

> [!NOTE]
> Si vous disposez d’un appel à une quelconque méthode **GetForCurrentView** qui *pourrait* être exécutée dans un thread d’arrière-plan/de travail, protégez cet appel par un test `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`. Appeler **GetForCurrentView** à partir des résultats d’un thread d’arrière-plan/de travail dans l’exception «*&lt;typename&gt; ne peut pas être créé dans des threads dépourvus d’un objet CoreWindow. *»

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

Si vous déplacez la ressource «AppDisplayName» de `Resources.resw` vers `ManifestResources.resw`, puis dans le manifeste de votre package d’application, vous devez remplacer `ms-resource:AppDisplayName` par `ms-resource:/ManifestResources/AppDisplayName`.

Si un nom de fichier de ressources est segmenté (il contient «.» caractères), puis laisser les points dans le nom lorsque vous la référencez. **Ne** remplacez points caractères barre oblique («/»), comme vous le feriez pour un nom de ressource.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

En cas de doute, vous pouvez utiliser [MakePri.exe](makepri-exe-command-options.md) pour vider le fichier PRI de votre application. Chaque ressource `uri` est indiqué dans le fichier dumping.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Charger une chaîne pour une langue spécifique ou un autre contexte
Le [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) par défaut (obtenu à partir de [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contient une valeur de qualificateur pour chaque nom de qualificateur, représentant le contexte d’exécution par défaut (en d’autres termes, les paramètres de l’utilisateur et de l’ordinateur actuels). Les fichiers de ressources (.resw) sont mis en correspondance, en fonction des qualificateurs figurant dans leurs noms, avec les valeurs de qualificateur dans ce contexte d’exécution.

Toutefois, dans certains cas, vous souhaiterez que votre application remplace les paramètres système et indique de manière explicite les valeurs des qualificateurs de langue, d’échelle ou autre à utiliser lors de la recherche d’un fichier de ressources correspondant à charger. Par exemple, vous voudrez peut-être que vos utilisateurs soient en mesure de sélectionner une autre langue pour les info-bulles et les messages d’erreur.

Pour ce faire, vous pouvez créer un nouveau **ResourceContext** (au lieu d’utiliser le contexte par défaut), remplacer ses valeurs, puis utiliser cet objet de contexte dans vos recherches de chaînes.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

L’utilisation de **QualifierValues**, comme dans l’exemple de code ci-dessus, fonctionne pour tout qualificateur. Dans le cas spécial de Langue, vous pouvez procéder de cette autre façon.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Pour obtenir le même effet à un niveau global, vous *pouvez* remplacer les valeurs de qualificateur dans le **ResourceContext** par défaut. Mais nous vous conseillons plutôt d’appeler [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Vous définissez les valeurs une fois avec l’appel de **SetGlobalQualifierValue**, puis ces valeurs sont appliquées pour le **ResourceContext** par défaut chaque fois que vous l’utiliser pour les recherches.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Quelques qualificateurs ont un fournisseur de données système. Ainsi, au lieu d’appeler **SetGlobalQualifierValue**, vous pouvez ajuster le fournisseur par le biais de sa propre API. Par exemple, ce code montre comment définir [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride).

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Mise à jour de chaînes en réponse à des événements de modification de valeur de qualificateur
Votre application en cours d’exécution peut répondre à des modifications de paramètres système qui affectent les valeurs de qualificateur dans le **ResourceContext** par défaut. Ces paramètres système appellent l’événement [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) sur [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En réponse à cet événement, vous pouvez recharger vos chaînes à partir du **ResourceContext** par défaut.

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
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="loading-strings-from-a-class-library-or-a-windows-runtime-library"></a>Chargement de chaînes à partir d’une bibliothèque de classes ou d’une bibliothèque Windows Runtime
Les ressources de chaîne d’une bibliothèque de classes (Windows universel) ou d’une [bibliothèque Windows Runtime (Windows universel)](../winrt-components/index.md) référencée sont généralement ajoutées dans un sous-dossier du package dans lequel elles sont incluses pendant le processus de génération. L’identificateur de ressource d’une telle chaîne prend généralement la forme *NomDeBibliothèque/NomFichierRessources/IdentificateurRessource*.

Une bibliothèque peut obtenir un ResourceLoader pour ses propres ressources. Par exemple, le code suivant illustre comment une bibliothèque ou une application qui la référence peut obtenir un ResourceLoader pour les ressources de chaîne de la bibliothèque.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Pour une bibliothèque d’exécution Windows (Universal Windows), si l’espace de noms par défaut est segmenté (qu’il contient «.» caractères), puis utiliser les points dans le nom de plan de ressources.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

Vous n’avez pas besoin de le faire pour une bibliothèque de classes (Universal Windows). En cas de doute, vous pouvez utiliser [MakePri.exe](makepri-exe-command-options.md) pour vider votre composant ou fichier PRI de la bibliothèque. Chaque ressource `uri` est indiqué dans le fichier dumping.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Chargement de chaînes à partir d’autres packages
Les ressources d’un package d’application sont gérés et accessibles par le biais du package propriétaire de niveau supérieur [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) qui est accessible à partir de [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)en cours. Dans chaque package, différents composants peuvent avoir leurs propres sous-arborescences ResourceMap, accessibles via [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live).

Un package d’infrastructure peut accéder à ses propres ressources avec un URI d’identificateur de ressource absolu. Voir également [Schémas d’URI](uri-schemes.md).

## <a name="important-apis"></a>API importantes
* [ApplicationModel.Resources.ResourceLoader](https://msdn.microsoft.com/library/windows/apps/br206014)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Rubriques associées
* [Portage du balisage XAML et de la couche interface utilisateur](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [Directive x:Uid](../xaml-platform/x-uid-directive.md)
* [propriétés jointes](../xaml-platform/attached-properties-overview.md)
* [Éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Balise de langueBCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Comment charger des ressources de chaîne](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)