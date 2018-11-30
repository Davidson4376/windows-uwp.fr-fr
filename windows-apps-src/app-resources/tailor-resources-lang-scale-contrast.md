---
Description: This topic explains the general concept of qualifiers, how to use them, and the purpose of each of the qualifier names.
title: Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: ac61d57a965e3a35c6eb7cfaf17d0f4ef2a02501
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8211093"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs

Cette rubrique explique le concept général des qualificateurs de ressource, leur utilisation et le rôle de chacun des noms de qualificateur. Consultez [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) pour un tableau de référence de toutes les valeurs de qualificateur possibles.

Votre application peut charger des ressources et actifs adaptés à des contextes d’exécution tels que la langue d’affichage, le contraste élevé, le [facteur d’échelle de l'affichage](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor) et bien d’autres. La façon de procéder consiste à donner à vos dossiers ou fichiers de ressources un nom correspondant au nom et à la valeur de qualificateur correspondant à ces contextes. Par exemple, vous souhaitez peut-être que votre application charge un autre ensemble de ressources d’image en mode de contraste élevé.

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, voir [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Nom de qualificateur, valeur de qualificateur et qualificateur

Un nom de qualificateur est une clé qui correspond à un ensemble de valeurs de qualificateur. Voici le nom de qualificateur et les valeurs de qualificateur correspondant au contraste.

| Contexte | Nom de qualificateur | Valeurs de qualificateur |
| :--------------- | :--------------- | :--------------- |
| Paramètre de contraste élevé | contraste | standard, élevé, noir, blanc |

Vous combinez un nom de qualificateur avec une valeur de qualificateur pour former un qualificateur. `<qualifier name>-<qualifier value>` est le format d’un qualificateur. `contrast-standard` est un exemple de qualificateur.

Par conséquent, pour le contraste élevé, le jeu de qualificateurs est `contrast-standard`, `contrast-high`, `contrast-black` et `contrast-white`. Les noms de qualificateur et les valeurs de qualificateur ne tiennent pas compte de la casse. Par exemple, les qualificateurs `contrast-standard` et `Contrast-Standard` sont identiques.

## <a name="use-qualifiers-in-folder-names"></a>Utiliser des qualificateurs dans les noms de dossier

Voici un exemple d’utilisation des qualificateurs pour nommer des dossiers contenant des fichiers de ressources. Utilisez des qualificateurs dans les noms de dossier si vous disposez de plusieurs fichiers de ressources par qualificateur. De cette façon, vous définissez le qualificateur une seule fois au niveau du dossier, et le qualificateur s’applique à tous les éléments qui se trouvent à l’intérieur du dossier.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Si vous nommez vos dossiers comme dans l’exemple ci-dessus, votre application utilise le paramètre de contraste élevé pour charger des fichiers de ressources à partir du dossier nommé par le qualificateur approprié. Par conséquent, si le paramètre est Contraste noir élevé, les fichiers de ressources du dossier `\Assets\Images\contrast-black` sont chargés. Si le paramètre est Aucun (autrement dit, l’ordinateur n’est pas en mode de contraste élevé), les fichiers de ressource du dossier `\Assets\Images\contrast-standard` sont chargés.

## <a name="use-qualifiers-in-file-names"></a>Utiliser des qualificateurs dans les noms de fichier

Au lieu de créer et de nommer des dossiers, vous pouvez utiliser un qualificateur pour nommer les fichiers de ressources eux-mêmes. Cette méthode peut être préférable si vous n'avez qu'un seul fichier de ressources par qualificateur. Voici un exemple.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

Le fichier dont le nom contient le qualificateur le plus approprié pour le paramètre est celui qui est chargé. Cette logique de correspondance fonctionne aussi bien pour les noms de fichier que pour les noms de dossier.

## <a name="reference-a-string-or-image-resource-by-name"></a>Faire référence à une ressource de chaîne ou d’image à l’aide de son nom

Voir [Faire référence à un identificateur de ressource de chaîne à partir du balisage XAML](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml-markup), [Faire référence à un identificateur de ressource de chaîne à partir du code](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code) et [Faire référence à une image ou une autre ressource à partir du code et du balisage XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Le qualificateur réel et neutre correspondent
Vous n’avez pas besoin de fournir un fichier de ressources pour *chaque* valeur de qualificateur. Par exemple, si vous constatez que vous n'avez besoin que d'une ressource visuelle pour le contraste élevé et que d'une pour le contraste standard, vous pouvez nommer ces ressources comme suit.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

Le premier nom de fichier contient le qualificateur `contrast-high`. Ce qualificateur est une correspondance *réelle* pour n’importe quel paramètre de contraste élevé lorsque le contraste élevé est *activé*. En d’autres termes, il est privilégié parce qu'il offre la plus proche correspondance. Une correspondance *réelle* peut se produire uniquement si le qualificateur contient une valeur *réelle*, comme dans cet exemple. Dans ce cas, `high` est une valeur *réelle* pour `contrast`.

Le fichier nommé `logo.png` ne contient aucun qualificateur de contraste. L’absence d’un qualificateur est une valeur *neutre*. Si aucune correspondance privilégiée ne peut être trouvée, la valeur neutre sert alors de correspondance de secours. Dans cet exemple, si le contraste élevé est *désactivé*, il y a aucune correspondance réelle. La correspondance *neutre* est la meilleure qui puisse être trouvée, donc la ressource `logo.png` est chargée.

Si vous renommiez `logo.png` en `logo.contrast-standard.png`, le nom du fichier contiendrait une valeur de qualificateur réelle. Avec un contraste élevé désactivé, il y aurait une correspondance réelle avec `logo.contrast-standard.png` et c'est ce fichier de ressources qui serait chargé. Par conséquent, les mêmes fichiers seraient chargés, dans les mêmes conditions, mais en raison de correspondances différentes.

Si vous avez uniquement besoin d'un ensemble de ressources pour le contraste élevé et d'un autre pour le contraste standard, vous pouvez utiliser des noms de dossier au lieu des noms de fichiers. Dans ce cas, l’omission complète du nom du dossier vous donne la correspondance neutre.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Pour plus d’informations sur le fonctionnement de la correspondance de qualificateur, consultez [Système de gestion des ressources](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Plusieurs qualificateurs

Vous pouvez combiner des qualificateurs dans les noms de dossier et de fichier. Par exemple, vous souhaitez peut-être que votre application charge des ressources d’image lorsque le mode contraste élevé est activé *et* que le facteur d’échelle de l’affichage est de400. L’une des méthodes possibles consiste à utiliser des dossiers imbriqués.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Pour `logo.png` et les autres fichiers à charger, les paramètres doivent correspondre *aux deux* qualificateurs.

Une autre option consiste à associer plusieurs qualificateurs dans le nom d’un dossier.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

Dans un nom de dossier, vous combinez plusieurs qualificateurs séparés par un trait de soulignement. `<qualifier1>[_<qualifier2>...]` est le format.

Vous pouvez combiner plusieurs qualificateurs dans un nom de fichier dans le même format.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

Selon les outils et le flux de travail utilisés pour la création de ressources, ou ce que vous trouvez plus facile à lire et/ou gérer, vous pouvez choisir une seule stratégie d’attribution de noms pour tous les qualificateurs ou les combiner pour des qualificateurs différents.

## <a name="alternateform"></a>AlternateForm

Le qualificateur `alternateform` sert à fournir une ressource sous une autre forme à des fins spéciales. En général, cela sert uniquement aux développeurs d’applications japonais pour fournir une chaîne furigana à laquelle la valeur `msft-phonetic` est réservée (voir la section «Prenez en charge les furigana pour les chaînes japonaises qui peuvent être triées» dans [Comment se préparer à la localisation](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)).

Votre système cible ou votre application doit fournir une valeur à laquelle les qualificateurs `alternateform` correspondent. N’utilisez pas le préfixe `msft-` pour vos propres valeurs de qualificateur `alternateform` personnalisées.

## <a name="configuration"></a>Configuration

Il est peu probable que vous ayez besoin du nom de qualificateur `configuration`. Il peut être utilisé pour spécifier les ressources qui s'appliquent uniquement à un environnement donné au moment de la création, telles que les ressources de test uniquement.

Le qualificateur `configuration` permet de charger la ressource qui correspond le mieux à la valeur de la variable d’environnement `MS_CONFIGURATION_ATTRIBUTE_VALUE`. Vous pouvez donc définir la variable à la valeur de chaîne qui a été attribuée aux ressources pertinentes, par exemple `designer` ou `test`.

## <a name="contrast"></a>Contraste

Le qualificateur `contrast` permet de fournir les ressources qui correspondent le mieux aux paramètres de contraste élevé.

## <a name="custom"></a>Personnalisé

Votre application peut définir une valeur pour le qualificateur `custom`. Les ressources qui correspondent le mieux à cette valeur sont alors chargées. Par exemple, vous pouvez souhaiter charger des ressources en fonction de la licence de votre application. Lorsque votre application se lance, elle vérifie sa licence et utilise celle-ci comme valeur du qualificateur `custom` en appelant [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_), comme illustré dans l’exemple de code.

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

Dans ce scénario, vous devez attribuer à vos ressources des noms qui incluent les qualificateurs `custom-premium`, `custom-standard` et `custom-trial`.

## <a name="devicefamily"></a>DeviceFamily

Il est peu probable que vous ayez besoin du nom de qualificateur `devicefamily`. Vous pouvez et devez éviter de l’utiliser autant que possible, car vous pouvez utiliser d'autres techniques beaucoup plus pratiques et fiables. Ces techniques sont décrites dans [Détection de la plateforme d’exécution de votre application](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) et [Code adaptatif de version](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Mais en dernier recours, il est possible d’utiliser des qualificateurs devicefamily pour nommer des dossiers qui contiennent vos vues XAML (une vue XAML est un fichier XAML qui contient une disposition de l’interface utilisateur et des contrôles).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

Vous pouvez également nommer des fichiers.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

Dans tous les cas, chaque copie de `MainPage.[<qualifier>].xaml` partage un `MainPage.xaml.cs` commun, qui reste inchangé dans votre projet en termes de nom, emplacement et contenu.

Vous pouvez également utiliser un qualificateur devicefamily pour nommer un fichier (`.resw`) ou un dossier de ressources. Par exemple, lorsque votre application s'exécute sur la famille d’appareils mobiles, l’élément d’interface utilisateur `<TextBlock x:Uid="DeviceFriendlyName"/>` utilisera les ressources de texte et de premier plan définies dans votre fichier `Resources.devicefamily-mobile.resw` s’il contient

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Pour plus d’informations sur l’utilisation d’un Fichier de ressources, voir [Localiser vos chaînes d’interface utilisateur](localize-strings-ui-manifest.md).

## <a name="dxfeaturelevel"></a>DXFeatureLevel

Il est peu probable que vous ayez besoin du nom de qualificateur `dxfeaturelevel`. Il a été conçu pour être utilisé avec les composants du jeu Direct3D, pour que les ressources de bas niveau soient chargées en fonction de leur correspondance avec une configuration matérielle de bas niveau particulière du moment. Mais la prévalence de cette configuration matérielle est désormais si faible que nous recommandons de ne pas utiliser ce qualificateur.

## <a name="homeregion"></a>HomeRegion

Le qualificateur `homeregion` correspond au paramètre de pays ou de région de l’utilisateur. Il représente le lieu de résidence de l’utilisateur. Les valeurs incluent toute [balise de région BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) valide. Autrement dit, tout code de région de deux lettres au format **ISO3166-1 alpha-2**, ainsi que le jeu de codes géographiques de trois chiffres **ISO3166-1 numériques** pour les régions composées (voir [Classification M49 des codes de région de la Division de statistique de l'ONU](http://go.microsoft.com/fwlink/p/?linkid=247929)). Les codes de «groupements sélectionnés économiques et autres» ne sont pas valides.

## <a name="language"></a>Language

Un qualificateur `language` correspond au paramètre de langue d’affichage. Les valeurs incluent toute [balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) valide. Pour une liste des langues, voir le [registre des sous-balises de langues IANA](http://go.microsoft.com/fwlink/p/?linkid=227303).

Si vous souhaitez que votre application prenne en charge différentes langues d’affichage et si des opérateurs de chaîne figurent dans votre code ou dans votre balisage XAML, retirez ces chaînes du code/balisage et placez-les dans un Fichier de ressources (`.resw`). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application.

Vous utilisez généralement un qualificateur `language` pour nommer les dossiers qui contiennent vos fichiers de ressources (`.resw`).

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

Vous pouvez omettre la partie `language-` d’un qualificateur `language` (autrement dit, le nom du qualificateur). Vous ne pouvez pas le faire avec les autres types de qualificateurs; et cela n'est possible que dans un nom de dossier.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

Au lieu de nommer des dossiers, vous pouvez utiliser des qualificateurs `language` pour nommer les fichiers de ressources eux-mêmes.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Voir [Localiser vos chaînes d’interface utilisateur](localize-strings-ui-manifest.md) pour savoir comment rendre votre application localisable à l’aide de ressources de chaîne et comment faire référence à une ressource de chaîne dans votre application.

## <a name="layoutdirection"></a>LayoutDirection

Un qualificateur `layoutdirection` correspond au paramètre de sens de la disposition de la langue d’affichage. Par exemple, une image a peut-être besoin d'être mise en miroir pour une langue qui s’écrit et se lit de droite à gauche, comme l’arabe ou l'hébreu. Les panneaux de disposition et les images de votre interface utilisateur répondront correctement au sens de la disposition si vous définissez leur propriété [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (voir [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). Toutefois, le qualificateur `layoutdirection` est destiné aux cas où un simple retournement n’est pas suffisant. Il permet de répondre au sens d'un ordre de lecture et d'un alignement de texte spécifiques de manière plus générale.

## <a name="scale"></a>Échelle

Windows sélectionne automatiquement un facteur d’échelle pour chaque affichage en fonction de son nombre de PPP (points par pouce) et de la distance de visualisation de l’appareil. Voir [Pixels effectifs et facteur d’échelle](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Vous devez créer vos images selon plusieurs tailles recommandées (au moins 100,200 et400) afin que Windows puisse choisir la taille idéale ou utiliser la taille le plus proche et la mettre à l’échelle. Pour que Windows soit en mesure d’identifier le fichier physique contenant la taille d’image appropriée pour le facteur d’échelle de l’affichage, vous utilisez un qualificateur `scale`. L’échelle d’une ressource correspond à la valeur de [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale), ou à la ressource avec la plus grande échelle suivante.

Voici un exemple de définition du qualificateur au niveau du dossier.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Et cet exemple le définit au niveau du fichier.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Pour plus d’informations sur la qualification d’une ressource pour `scale` et `targetsize`, voir [Appliquer le qualificateur targetsize à une ressource d’image](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>TargetSize

Le qualificateur `targetsize` sert principalement à spécifier des [icônes d’association de types de fichier](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh127427) ou des [icônes du protocole](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/bb266530) à afficher dans l’Explorateur de fichiers. La valeur du qualificateur représente la longueur du côté d’une image carrée en pixels bruts (physiques). La ressource dont la valeur correspond au paramètre Affichage de l’Explorateur de fichiers est chargée ou, en l’absence d'une correspondance exacte, la ressource avec la plus grande valeur suivante.

Vous pouvez définir des ressources qui représentent plusieurs tailles de la valeur du qualificateur `targetsize` pour l’icône d’application (`/Assets/Square44x44Logo.png`) dans l’onglet Ressources visuelles du concepteur de manifeste du package d’application.

Pour plus d’informations sur la qualification d’une ressource pour `scale` et `targetsize`, voir [Appliquer le qualificateur targetsize à une ressource d’image](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Theme

Le qualificateur `theme` est utilisé pour fournir les ressources qui correspondent le mieux au paramètre de mode d’application par défaut, ou au remplacement de votre application à l’aide de [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application?branch=master.RequestedTheme).

## <a name="important-apis"></a>API importantes

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)

## <a name="related-topics"></a>Rubriques associées

* [Pixels effectifs et facteur d’échelle](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Système de gestion des ressources](resource-management-system.md)
* [Comment se préparer à la localisation](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)
* [Détection de la plateforme d’exécution de votre application](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Présentation des familles de périphériques](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [Localiser vos chaînes d’interface utilisateur](localize-strings-ui-manifest.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Classification M49 des codes de région de la Division de statistique de l'ONU](http://go.microsoft.com/fwlink/p/?linkid=247929)
* [Registre des sous-balises de langues IANA](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
