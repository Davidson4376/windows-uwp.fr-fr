---
Description: Some kinds of apps (multilingual dictionaries, translation tools, etc.) need to override the default behavior of an app bundle, and build resources into the app package instead of having them in separate resource packages. This topic explains how to do that.
title: Générer des ressources dans votre package d’application, plutôt que dans un pack de ressources
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 8bf2d34bc3dae20750f66c9116499a17444b798c
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8346090"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Générer des ressources dans votre package d’application, plutôt que dans un pack de ressources

Certains types d’applications (dictionnaires multilingues, outils de traduction, etc.) doivent remplacer le comportement par défaut d’un ensemble d’applications et générer des ressources dans le package d’application, plutôt que dans des packages de ressources distincts (ou des packs de ressources). Cette rubrique explique la procédure à suivre.

Par défaut, lorsque vous générez un [ensemble d’applications (.appxbundle)](../packaging/packaging-uwp-apps.md), seules vos ressources par défaut pour la langue, la mise à l’échelle et le niveau de fonctionnalité DirectX sont intégrées dans le package d’application. Vos ressources traduites&mdash;et vos ressources adaptées aux échelles autres que celles par défaut et/ou aux niveaux de fonctionnalités DirectX&mdash;sont intégrées dans les packages de ressources et sont téléchargées uniquement sur les périphériques qui en ont besoin. Si un client achète votre application dans Microsoft Store à l’aide d’un périphérique dont la préférence linguistique a la valeur espagnol, seule votre application ainsi que le package de ressources en espagnol sont téléchargés et installés. Si ce même utilisateur change par la suite ses préférences linguistiques en français dans les **paramètres**, le package de ressources en français de votre application est téléchargé et installé. La même chose se produit avec vos ressources qualifiées pour la mise à l’échelle et le niveau de fonctionnalité DirectX. Pour la plupart des applications, ce comportement est très efficace: c’est exactement ce que le client et vous-même *souhaitez* voir se produire.

Mais si votre application autorise l’utilisateur à modifier la langue à la volée à partir de l’application (plutôt que via les **paramètres**), ce comportement par défaut n’est pas approprié. Il est en effet préférable que toutes vos ressources linguistiques soit téléchargées sans condition et installées avec l’application une fois, puis qu’elles restent sur le périphérique. Toutes ces ressources doivent alors être générées dans votre package d’application, plutôt que dans des packages de ressources distincts.

**Remarque** Le fait d’inclure des ressources dans un package d’application augmente substantiellement la taille de l’application. C’est pourquoi cette décision ne doit être prise que si la nature de l’application l’exige. Si ce n’est pas le cas, vous n’avez rien à faire, hormis générer un ensemble d’applications standard comme d’habitude.

Vous pouvez configurer Visual Studio pour qu’il génère des ressources dans votre package d’application de l’une ou l’autre des deux manières. Vous pouvez soit ajouter un fichier de configuration à votre projet, soit modifier votre fichier de projet directement. Utilisez l’option avec laquelle vous êtes le plus à l’aise, ou celle qui fonctionne le mieux avec votre système de génération.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Option1. Utilisez priconfig.packaging.xml pour générer des ressources dans votre package d’application

1. Dans Visual Studio, ajoutez un nouvel élément à votre projet. Choisissez le fichier XML et nommez-le `priconfig.packaging.xml`.
2. Dans l’Explorateur de solutions, sélectionnez `priconfig.packaging.xml`et vérifiez la fenêtre Propriétés. L’option Action de génération du fichier doit être définie sur Aucun, et Copier dans le répertoire de sortie sur Ne pas copier.
3. Remplacez le contenu du fichier par ce code XML.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Chaque élément `<autoResourcePackage>` indique à Visual Studio qu’il doit fractionner automatiquement les ressources pour le nom de qualificateur donné en packages de ressources distincts. C’est ce que l’on appelle le *fractionnement automatique *. Avec le contenu du fichier que vous avez actuellement, vous n’avez pas véritablement modifié le comportement de Visual Studio. En d’autres termes, Visual Studio *se comportait déjà comme si* ce fichier était présent avec ce contenu, car c’est le comportement par défaut. Si vous ne voulez pas que Visual Studio effectue un fractionnement automatique sur un nom de qualificateur, supprimez cet élément `<autoResourcePackage>` du fichier. Voici à quoi ressemblerait le fichier si vous vouliez que toutes vos ressources linguistiques soient intégrées dans le package d’application au lieu d’être fractionnées automatiquement en packages de ressources distincts.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Enregistrez le fichier et fermez-le, puis générez une nouvelle fois votre projet.

Pour vérifier que vos choix de fractionnement automatique sont pris en compte, recherchez le fichier `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml`et confirmez que son contenu correspond à vos choix. Si tel est le cas, vous avez correctement configuré Visual Studio pour générer les ressources de votre choix dans le package d’application.

Vous devez effectuer une étape finale. **Ceci est valable uniquement si vous avez supprimé le nom de qualificateur `Language`**. Vous devez spécifier l’union de toutes les langues prises en charge par votre application comme langue par défaut de votre application. Pour plus d’informations, consultez [Préciser les ressources par défaut que votre application utilise](specify-default-resources-installed.md). Voici ce que contiendrait votre fichier `priconfig.default.xml` si vous englobiez des ressources pour l’anglais, l’espagnol et le français dans votre package d’application.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>Comment cela fonctionne-t-il?

En coulisse, Visual Studio lance un outil nommé `MakePri.exe` pour générer un fichier appelé un index de ressource de package, qui décrit toutes les ressources de votre application et qui indique notamment sur quels noms de qualificateurs de ressources effectuer le fractionnement automatique. Pour plus d’informations sur cet outil, consultez [Compiler des ressources manuellement avec MakePri.exe ](compile-resources-manually-with-makepri.md). Visual Studio envoie un fichier de configuration à `MakePri.exe`. Le contenu de votre fichier `priconfig.packaging.xml` est utilisé en tant qu’élément `<packaging>` de ce fichier de configuration, qui correspond à la partie qui détermine le fractionnement automatique. Ainsi, si vous ajoutez et modifiez `priconfig.packaging.xml`, vous influencez en fin de compte le contenu du fichier d’index de ressource de package que Visual Studio génère pour votre application, ainsi que le contenu des packages dans votre ensemble d’application.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Utilisation d’un nom de fichier différent de `priconfig.packaging.xml`

Si vous nommez votre fichier `priconfig.packaging.xml`, Visual Studio le reconnaît et l’utilise automatiquement. Si vous lui donnez un nom différent, vous devez l’indiquer à Visual Studio. Dans votre fichier de projet, entre les balises d’ouverture et de fermeture du premier élément `<PropertyGroup>`, ajoutez ce fichier XML.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Remplacez `FILE-PATH-AND-NAME` par le chemin d’accès à votre fichier et son nom.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Option2. Utilisez votre fichier de projet pour générer des ressources dans votre package d’application

Ceci est une alternative à l’option1. Une fois que vous comprenez le fonctionnement de l’option1, vous pouvez choisir d’utiliser l’option2 à la place, si elle correspond mieux à votre flux de travail de développement et/ou de génération.

Dans votre fichier de projet, entre les balises d’ouverture et de fermeture du premier élément `<PropertyGroup>`, ajoutez ce fichier XML.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Voici à quoi ressemble le fichier une fois que vous avez supprimé le premier nom de qualificateur.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Enregistrez et fermez-le, puis générez une nouvelle fois votre projet.

Vous devez effectuer une étape finale. **Ceci est valable uniquement si vous avez supprimé le nom de qualificateur `Language`**. Vous devez spécifier l’union de toutes les langues prises en charge par votre application comme langue par défaut de votre application. Pour plus d’informations, consultez [Préciser les ressources par défaut que votre application utilise](specify-default-resources-installed.md). Voici ce que contiendrait votre fichier de projet si vous englobiez des ressources pour l’anglais, l’espagnol et le français dans votre package d’application.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Rubriquesassociées

* [Créer un package d’application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md)
* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
* [Préciser les ressources par défaut que votre application utilise](specify-default-resources-installed.md)