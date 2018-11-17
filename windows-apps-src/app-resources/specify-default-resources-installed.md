---
author: stevewhims
Description: If your app doesn't have resources that match the particular settings of a customer device, then the app's default resources are used. This topic explains how to specify what those default resources are.
title: Préciser les ressources par défaut que votre application utilise
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: daa40656c72812e19c7f6f5fa71e50c2206670af
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7148449"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Préciser les ressources par défaut que votre application utilise

Si votre application n’a pas les ressources qui correspondent aux paramètres particuliers d’un périphérique client, les ressources de l’application par défaut sont utilisées. Cette rubrique explique comment spécifier ce que sont ces ressources par défaut.

Lorsqu’un client installe votre application à partir de Microsoft Store, les paramètres sur l’appareil du client sont comparés aux ressources disponibles de l’application. Cette association est réalisée afin que seules les ressources appropriées soient téléchargées et installées pour cet utilisateur. Par exemple, les chaînes et les images les mieux adaptées pour les préférences linguistiques de l’utilisateur, ainsi que la résolution et les paramètres PPP du périphérique, sont utilisés. Par exemple, `200` est la valeur par défaut pour `scale`, mais vous pouvez remplacer cette valeur par défaut si vous le souhaitez.

Même pour les ressources qui n’entrent pas dans leurs propres packs de ressources (par exemple, des images personnalisées pour les paramètres de contraste élevé), vous pouvez spécifier les ressources par défaut que l’application doit utiliser au moment de l’exécution si une ressource qui correspond aux paramètres de l’utilisateur est introuvable. Par exemple, `standard` est la valeur par défaut pour `contrast`, mais vous pouvez remplacer cette valeur par défaut si vous le souhaitez.

Ces valeurs par défaut sont spécifiées sous la forme de valeurs de qualificateurs de ressources par défaut. Pour obtenir une explication de ce que sont les qualificateurs de ressources, de leur utilisation et de leur finalité, consultez [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md).

Vous pouvez configurer ce que sont ces valeurs par défaut de deux manières. Vous pouvez soit ajouter un fichier de configuration à votre projet, soit modifier votre fichier de projet directement. Utilisez l’option avec laquelle vous êtes le plus à l’aise, ou celle qui fonctionne le mieux avec votre système de génération.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Option1. Utilisez priconfig.default.xml pour spécifier des valeurs de qualificateur par défaut

1. Dans Visual Studio, ajoutez un nouvel élément à votre projet. Choisissez le fichier XML et nommez-le `priconfig.default.xml`.
2. Dans l’Explorateur de solutions, sélectionnez `priconfig.default.xml`et vérifiez la fenêtre Propriétés. L’option Action de génération du fichier doit être définie sur Aucun, et Copier dans le répertoire de sortie sur Ne pas copier.
3. Remplacez le contenu du fichier par ce code XML.
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **Remarque** la valeur `LANGUAGE-TAG(S)`doit être synchronisée avec la langue par défaut de votre application. S’il s’agit d’une [balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) unique, la langue par défaut de votre application doit être la même balise. Si c’est une liste séparée par des virgules de balises de langue, la langue par défaut de votre application doit être la première balise dans la liste. Vous définissez la langue par défaut dans le champ **Langue par défaut** dans l’onglet **Application** du fichier source du manifeste du package d’application (`Package.appxmanifest`).

4. Chaque élément `<qualifier>`indique à Visual Studio quelle valeur utiliser comme valeur par défaut pour chaque nom de qualificateur. Avec le contenu du fichier que vous avez actuellement, vous n’avez pas véritablement modifié le comportement de Visual Studio. En d’autres termes, Visual Studio *se comportait déjà comme si* ce fichier était présent avec ce contenu, car c’est le comportement par défaut. Pour remplacer une valeur par défaut par votre propre valeur par défaut, vous devez donc modifier une valeur dans le fichier. Voici un exemple de l’aspect que peut avoir le fichier si vous avez modifié les trois premières valeurs.
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. Enregistrez le fichier et fermez-le, puis générez une nouvelle fois votre projet.

Pour vérifier que vos valeurs par défaut de remplacement sont pris en compte, recherchez le fichier `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`et confirmez que son contenu correspond à vos choix. Si c’est le cas, vous avez correctement configuré les valeurs de qualificateur des ressources que votre application utilisera par défaut. Si aucune correspondance n’est trouvée pour les paramètres de l’utilisateur, les ressources dont le nom de fichier ou de dossier contient les valeurs de qualificateurs par défaut que vous avez définies ici seront utilisées.

### <a name="how-does-this-work"></a>Comment cela fonctionne-t-il?

En coulisse, Visual Studio lance un outil nommé `MakePri.exe` pour générer un fichier appelé un index de ressource de package (IRP), qui décrit toutes les ressources de votre application et qui indique quelles sont les ressources par défaut. Pour plus d’informations sur cet outil, consultez [Compiler des ressources manuellement avec MakePri.exe ](compile-resources-manually-with-makepri.md). Visual Studio envoie un fichier de configuration à `MakePri.exe`. Le contenu de votre fichier `priconfig.default.xml` est utilisé en tant qu’élément `<default>` de ce fichier de configuration, qui est la partie qui spécifie l’ensemble des valeurs de qualificateur qui sont considérées comme des valeurs par défaut. Ainsi, si vous ajoutez et modifiez `priconfig.default.xml`, vous influencez en fin de compte le contenu du fichier d’index de ressource de package que Visual Studio génère pour votre application et l’ajoute à son package d’application.

**Remarque** chaque fois que vous modifiez la valeur de l’élément `<qualifier name="Language" ... />`, vous devez synchroniser ce changement avec la langue par défaut de votre application. Ainsi, les ressources linguistiques indexées dans le fichier PRI de votre application correspondent à la langue par défaut du manifeste de votre application. La valeur dans l’élément `<qualifier name="Language" ... />` remplace la valeur dans le manifeste en ce qui concerne le contenu de `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`, mais ce fichier et le manifeste de votre application doivent correspondre.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Utilisation d’un nom de fichier différent de `priconfig.default.xml`

Si vous nommez votre fichier `priconfig.default.xml`, Visual Studio le reconnaît et l’utilise automatiquement. Si vous lui donnez un nom différent, vous devez l’indiquer à Visual Studio. Dans votre fichier de projet, entre les balises d’ouverture et de fermeture du premier élément `<PropertyGroup>`, ajoutez ce fichier XML.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Remplacez `FILE-PATH-AND-NAME` par le chemin d’accès à votre fichier et son nom.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Option2. Utilisez votre fichier de projet pour spécifier des valeurs de qualificateur par défaut

Ceci est une alternative à l’option1. Une fois que vous comprenez le fonctionnement de l’option1, vous pouvez choisir d’utiliser l’option2 à la place, si elle correspond mieux à votre flux de travail de développement et/ou de génération.

Dans votre fichier de projet, entre les balises d’ouverture et de fermeture du premier élément `<PropertyGroup>`, ajoutez ce fichier XML.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Voici un exemple de l’aspect que peut avoir le fichier une fois que vous aurez modifié les trois premières valeurs.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Enregistrez et fermez-le, puis générez une nouvelle fois votre projet.

**Remarque** chaque fois que vous modifiez la valeur `Language=`, vous devez synchroniser ce changement avec la langue par défaut de votre application dans le concepteur de manifeste (en ouvrant `Package.appxmanifest`).

## <a name="related-topics"></a>Rubriquesassociées

* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Balise de langueBCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
