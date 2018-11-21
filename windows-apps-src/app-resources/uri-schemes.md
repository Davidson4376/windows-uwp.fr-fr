---
author: stevewhims
Description: There are several URI (Uniform Resource Identifier) schemes that you can use to refer to files that come from your app's package, your app's data folders, or the cloud. You can also use a URI scheme to refer to strings loaded from your app's Resources Files (.resw).
title: Schémas d’URI
template: detail.hbs
ms.author: stwhi
ms.date: 10/16/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 75ba42674ca1ea460698fcce6e67bb3528589797
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7423375"
---
# <a name="uri-schemes"></a>Schémas d’URI

Vous pouvez utiliser plusieurs schémas d’URI (Uniform Resource Identifier) pour faire référence à des fichiers qui proviennent de votre package d’application, des dossiers de données de votre application ou du cloud. Vous pouvez également utiliser un schéma d’URI pour faire référence à des chaînes chargées à partir des fichiers de ressources (.resw) de votre application. Vous pouvez utiliser ces schémas d’URI dans votre code, votre balisage XAML, le manifeste de votre package d’application ou dans vos modèles de vignette et de notification toast.

## <a name="common-features-of-the-uri-schemes"></a>Fonctionnalités courantes des schémas d’URI

Tous les schémas décrits dans cette rubrique suivent les règles de schéma d’URI standard pour la normalisation et l’extraction des ressources. Voir [RFC3986](http://go.microsoft.com/fwlink/p/?LinkId=263444) pour la syntaxe générale d’un URI.

Tous les schémas d’URI définissent la partie hiérarchique conformément à [RFC3986](http://go.microsoft.com/fwlink/p/?LinkId=263444) en tant que composants d’autorité et de chemin d’accès de l’URI.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Cela signifie qu’un URI se compose essentiellement de trois éléments. Immédiatement après les deux barres obliques de l’URI, *schéma* est un composant (qui peut être vide) appelé l’*autorité*. Tout de suite après figure le *chemin d’accès*. Dans l’exemple d’URI `http://www.contoso.com/welcome.png`, le schéma est «`http://`», l’autorité est «`www.contoso.com`» et le chemin d’accès est «`/welcome.png`». Un autre exemple est l’URI `ms-appx:///logo.png`, où le composant d’autorité est vide et prend une valeur par défaut.

Le composant fragment est ignoré par le traitement spécifique au schéma des URI mentionnés dans cette rubrique. Lors de la récupération et de la comparaison des ressources, le composant fragment n’a aucune incidence. Toutefois, les couches situées au-dessus de l’implémentation spécifique peuvent interpréter le fragment pour récupérer une ressource secondaire.

La comparaison est effectuée octet par octet après la normalisation de tous les composants IRI.

## <a name="case-insensitivity-and-normalization"></a>Non-respect de la casse et normalisation

Tous les schémas d’URI décrits dans cette rubrique suivent les règles de schéma d’URI standard (RFC3986) pour la normalisation et la récupération des ressources pour les schémas. La forme normalisée de ces URI conserve la casse et décode à l’aide du percent encoding les caractères RFC3986 non réservés.

Pour tous les schémas d’URI décrits dans cette rubrique, *schéma*, *autorité*, et *chemin d’accès* ne sont, conformément à la norme, pas sensibles à la casse ou sont traités par le système comme ne respectant pas la casse. **Remarque:** la seule exception à cette règle est l’*autorité* de `ms-resource`, qui respecte la casse.

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx et ms-appx-web

Utilisez le schéma d’URI `ms-appx` ou `ms-appx-web` pour faire référence à un fichier qui provient de votre package d’application (voir [Création de packages d’application](../packaging/index.md)). Les fichiers contenus dans votre package d’application sont généralement des images statiques, des données, du code ou des fichiers de disposition. Le schéma `ms-appx-web` accède aux mêmes fichiers que `ms-appx`, mais dans le compartiment web. Pour en savoir plus et voir des exemples, voir [Faire référence à une image ou une autre ressource à partir du code et du balisage XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Nom de schéma (ms-appx et ms-appx-web)

Le nom du schéma d’URI est la chaîne «ms-appx» ou «ms-appx-web».

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autorité (ms-appx et ms-appx-web)

L’autorité est le nom de l’identité du package qui est défini dans le manifeste du package. Pour les URI et IRI (Internationalized resource identifier), il est donc limité au jeu de caractères autorisé dans un nom d’identité de package. Le nom du package doit être le nom d’un des packages du graphique de dépendances du package de l’application en cours d’exécution.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Si un autre caractère figure dans l’autorité, la récupération et la comparaison échouent. La valeur par défaut de l’autorité est le package de l’application en cours d’exécution.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Informations d’utilisateur et de port (ms-appx et ms-appx-web)

Contrairement à d’autres schémas courants, le schéma `ms-appx` ne définit pas de composant pour les informations d’utilisateur ou de port. Dans la mesure où «@" and ":» ne sont pas autorisés en tant que valeurs d’autorité valides, la recherche échoue en cas d’utilisation. Chaque URI suivante échoue.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Chemin d’accès (ms-appx et ms-appx-web)

Le composant de chemin d’accès correspond à la syntaxe RFC3986 générique et prend en charge les caractères non-ASCII dans les IRI. Le composant de chemin d’accès définit le chemin d’accès physique ou logique d’un fichier. Ce fichier se trouve dans un dossier associé à l’emplacement d’installation du package d’application, pour l’application spécifiée par l’autorité.

Si le chemin d’accès fait référence à un nom de fichier et chemin d’accès physiques, cette ressource de fichier physique est récupérée. Toutefois, si aucun fichier physique n’est trouvé, la ressource réelle renvoyée lors de la récupération est déterminée à l’aide de la négociation de contenu lors de l’exécution. Cette détermination est basée sur les paramètres de l’application, du système d’exploitation et de l’utilisateur tels que la langue, le facteur d’échelle de l’affichage, le thème, le contraste élevé et d’autres contextes d’exécution. Par exemple, une combinaison des langues de l’application, des paramètres d’affichage du système et des paramètres de contraste élevé de l’utilisateur peut être prise en considération lors de la détermination de la valeur de ressource réelle à récupérer.

```xml
ms-appx:///images/logo.png
```

L’URI ci-dessus peut en fait récupérer un fichier dans le package de l’application en cours avec le nom de fichier physique suivant.

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

Vous pouvez bien entendu récupérer également ce même fichier physique en y faisant référence directement à l’aide de son nom complet.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

À l’instar des URI génériques, le composant de chemin d’accès de `ms-appx(-web)` respecte la casse. Toutefois, lorsque le système de fichiers sous-jacent via lequel la ressource est accessible ne respecte pas la casse (par exemple, NTFS), la ressource est récupérée sans respect de la casse.

La forme normalisée de l’URI conserve la casse et décode les caractères RFC3986 non réservés à l’aide du percent encoding (symbole «%» suivi d’une représentation hexadécimale à deux chiffres). Les caractères «?», «#», «/», «*» et «”» (le caractère de guillemet double) doivent être codés à l’aide du percent encoding dans un chemin d’accès pour représenter des données telles que les noms de fichier ou de dossier. Tous les caractères codés à l’aide du percent encoding sont décodés avant la récupération. Par conséquent, pour récupérer un fichier nommé Hello#World.html, utilisez cet URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Requête (ms-appx et ms-appx-web)

Les paramètres de requête sont ignorés lors de la récupération des ressources. La forme normalisée des paramètres de requête respecte la casse. Les paramètres de requête ne sont pas ignorés lors de la comparaison.

## <a name="ms-appdata"></a>ms-appdata

Utilisez le schéma d’URI `ms-appdata` pour faire référence aux fichiers qui proviennent des dossiers de données locales, itinérantes et temporaires de l’application. Pour plus d’informations sur ces dossiers de données d’application, voir [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md).

Le schéma d’URI `ms-appdata` n’effectue pas la négociation de contenu lors de l’exécution effectuée par [ms-appx et ms-appx-web](#ms-appx-and-ms-appx-web). Toutefois, vous pouvez répondre au contenu de [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) et charger les ressources appropriées à partir des données d’application en utilisant leur nom de fichier physique complet dans l’URI.

### <a name="scheme-name-ms-appdata"></a>Nom de schéma (ms-appdata)

Le nom du schéma d’URI est la chaîne «ms-appdata».

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autorité (ms-appdata)

L’autorité est le nom de l’identité du package qui est défini dans le manifeste du package. Pour les URI et IRI (Internationalized resource identifier), il est donc limité au jeu de caractères autorisé dans un nom d’identité de package. Le nom du package doit être le nom du package de l’application en cours d’exécution.

```xml
ms-appdata://Contoso.MyApp/
```

Si un autre caractère figure dans l’autorité, la récupération et la comparaison échouent. La valeur par défaut de l’autorité est le package de l’application en cours d’exécution.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Informations d’utilisateur et de port (ms-appdata)

Contrairement à d’autres schémas courants, le schéma `ms-appdata` ne définit pas de composant pour les informations d’utilisateur ou de port. Dans la mesure où «@" and ":» ne sont pas autorisés en tant que valeurs d’autorité valides, la recherche échoue en cas d’utilisation. Chaque URI suivante échoue.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Chemin d’accès (ms-appdata)

Le composant de chemin d’accès correspond à la syntaxe RFC3986 générique et prend en charge les caractères non-ASCII dans les IRI. À l’emplacement [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live), troisdossiers sont réservés pour le stockage de l’état local, itinérant et temporaire. Le schéma `ms-appdata` permet d’accéder aux fichiers et dossiers à ces emplacements. Le premier segment du composant de chemin d’accès doit spécifier le dossier spécifique comme suit. Par conséquent, «hier-part» sous la forme «path-empty» n’est pas légal.

Dossier local.

```xml
ms-appdata:///local/
```

Dossier temporaire.

```xml
ms-appdata:///temp/
```

Dossier itinérant.

```xml
ms-appdata:///roaming/
```

À l’instar des URI génériques, le composant de chemin d’accès de `ms-appdata` respecte la casse. Toutefois, lorsque le système de fichiers sous-jacent via lequel la ressource est accessible ne respecte pas la casse (par exemple, NTFS), la ressource est récupérée sans respect de la casse.

La forme normalisée de l’URI conserve la casse et décode les caractères RFC3986 non réservés à l’aide du percent encoding (symbole «%» suivi d’une représentation hexadécimale à deux chiffres). Les caractères «?», «#», «/», «*» et «”» (le caractère de guillemet double) doivent être codés à l’aide du percent encoding dans un chemin d’accès pour représenter des données telles que les noms de fichier ou de dossier. Tous les caractères codés à l’aide du percent encoding sont décodés avant la récupération. Par conséquent, pour récupérer un fichier local nommé Hello#World.html, utilisez cet URI.

```xml
ms-appdata://local/Hello%23World.html
```

La récupération de la ressource et l’identification du segment de chemin d’accès de niveau supérieur sont gérées après la normalisation des points («.././b/c»). Par conséquent, les URI ne peuvent pas utiliser de points pour l’un des dossiers réservés et l’URI suivant n’est pas autorisé.

```xml
ms-appdata:///local/../hello/logo.png
```

En revanche, cet URI est autorisé (bien que redondant).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Requête (ms-appdata)

Les paramètres de requête sont ignorés lors de la récupération des ressources. La forme normalisée des paramètres de requête respecte la casse. Les paramètres de requête ne sont pas ignorés lors de la comparaison.

## <a name="ms-resource"></a>ms-resource

Utilisez le schéma d’URI `ms-resource` pour faire référence à des chaînes chargées à partir des fichiers de ressources (.resw) de votre application. Pour en savoir plus sur les fichiers de ressources (.resw) et voir des exemples, voir [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Nom de schéma (ms-resource)

Le nom du schéma d’URI est la chaîne «ms-resource».

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autorité (ms-resource)

L’autorité est le mappage de ressources de niveau supérieur défini dans l’index de ressource de package (PRI), qui correspond généralement au nom d’identité du package qui est défini dans le manifeste du package. Voir [Création de packages d’application](../packaging/index.md) Pour les URI et IRI (Internationalized resource identifier), il est donc limité au jeu de caractères autorisé dans un nom d’identité de package. Le nom du package doit être le nom d’un des packages du graphique de dépendances du package de l’application en cours d’exécution.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Si un autre caractère figure dans l’autorité, la récupération et la comparaison échouent. La valeur par défaut de l’autorité est le nom du package (qui respecte la casse) de l’application en cours d’exécution.

```xml
ms-resource:///
```

L’autorité respecte la casse et la forme normalisée conserve la casse. Toutefois, lors de la recherche d’une ressource, la casse n’est pas respectée.

### <a name="user-info-and-port-ms-resource"></a>Informations d’utilisateur et de port (ms-resource)

Contrairement à d’autres schémas courants, le schéma `ms-resource` ne définit pas de composant pour les informations d’utilisateur ou de port. Dans la mesure où «@" and ":» ne sont pas autorisés en tant que valeurs d’autorité valides, la recherche échoue en cas d’utilisation. Chaque URI suivante échoue.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Chemin d’accès (ms-resource)

Le chemin d’accès identifie l’emplacement hiérarchique de la sous-arborescence [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) (voir [Système de gestion des ressources](https://msdn.microsoft.com/library/windows/apps/jj552947)) et la [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) qu’elle contient. En règle générale, cela correspond au nom de fichier (sans extension) d’un fichier de ressources (.resw) et à l’identificateur d’une ressource de chaîne figurant dans celui-ci.

Pour en savoir plus et voir des exemples, voir [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md) et [Prise en charge des vignettes et notifications toast pour la langue, l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

À l’instar des URI génériques, le composant de chemin d’accès de `ms-resource` respecte la casse. Toutefois, la récupération sous-jacent n’un [CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628) avec *ignoreCase* définie sur `true`.

La forme normalisée de l’URI conserve la casse et décode les caractères RFC3986 non réservés à l’aide du percent encoding (symbole «%» suivi d’une représentation hexadécimale à deux chiffres). Les caractères «?», «#», «/», «*» et «”» (le caractère de guillemet double) doivent être codés à l’aide du percent encoding dans un chemin d’accès pour représenter des données telles que les noms de fichier ou de dossier. Tous les caractères codés à l’aide du percent encoding sont décodés avant la récupération. Par conséquent, pour récupérer une ressource de chaîne à partir d’un fichier de ressources nommé `Hello#World.resw`, utilisez cet URI.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Requête (ms-resource)

Les paramètres de requête sont ignorés lors de la récupération des ressources. La forme normalisée des paramètres de requête respecte la casse. Les paramètres de requête ne sont pas ignorés lors de la comparaison. Les paramètres de requête sont comparés en tenant compte de la casse.

Les développeurs de composants spécifiques situés dans une couche au-dessus de cette analyse d’URI peuvent choisir d’utiliser les paramètres de requête comme ils le souhaitent.

## <a name="related-topics"></a>Rubriques associées

* [URI (Uniform Resource Identifier): syntaxe générique](http://go.microsoft.com/fwlink/p/?LinkId=263444)
* [Création de packages d’application](../packaging/index.md)
* [Faire référence à une image ou une autre ressource à partir du code et du balisage XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md)
* [Système de gestion des ressources](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [Prise en charge des vignettes et notifications toast pour la langue, l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)