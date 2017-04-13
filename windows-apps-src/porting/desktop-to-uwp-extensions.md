---
author: normesta
Description: "En plus des API normales disponibles pour toutes les applications UWP, il existe certaines extensions et API disponibles uniquement pour les applications de bureau converties. Cet article décrit ces extensions et la manière de les utiliser."
Search.Product: eADQiWindows 10XVcnh
title: "Extensions d’application du pont du bureau vers UWP"
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: a3c29f0d36cec9a5816d5c20eb43a6634260cc43
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-app-extensions"></a>Pont du bureau vers UWP: extensions d’application

Vous pouvez améliorer votre application de bureau convertie avec un large éventail d’API de la plateforme Windows universelle (UWP). Toutefois, en plus des API normales disponibles pour toutes les applications UWP, il existe certaines extensions et API disponibles uniquement pour les applications de bureau converties. Ces fonctionnalités sont axées sur des scénarios tels que le lancement d’un processus lors de la connexion de l’utilisateur et l’intégration dans l’Explorateur de fichiers, et sont conçues pour faciliter la transition entre l’application de bureau d’origine et le package de l’application convertie.

Cet article décrit ces extensions et la manière de les utiliser. La plupart nécessitent la modification manuelle du fichier manifeste de votre application convertie, lequel contient des déclarations sur les extensions utilisées par votre application. Pour modifier le manifeste, cliquez avec le bouton droit sur le fichier **Package.appxmanifest** dans votre solution VisualStudio, puis sélectionnez *Afficher le code*.

## <a name="manifest-xml-namespaces"></a>Espaces de noms XML du manifeste

Les extensions d’application Pont du bureau nécessitent plusieurs espaces de noms XML différents. Ces espaces de noms doivent être définis dans l’élément `<Package>` à la racine de votre fichier manifeste, comme suit:

```xml
<Package
  ...
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  ...
  IgnorableNamespaces="uap uap2 uap3 mp rescap desktop">
```

Si votre fichier manifeste génère des erreurs de compilateur, vérifiez que vous avez inclus tous les espaces de noms requis et que l’espace de nom approprié est indiqué en tant que préfixe pour les éléments XML enfants. Vous pouvez également consulter un fichier manifeste de travail complet dans le [référentiel d’exemples Pont du bureau sur GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

## <a name="startup-tasks"></a>Tâches de démarrage

Les tâches de démarrage permettent à votre application d’exécuter un fichier exécutable automatiquement dès qu’un utilisateur se connecte.

Pour déclarer une tâche de démarrage, ajoutez le code suivant au manifeste de votre application:

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *Extension Category* doit toujours avoir la valeur «windows.startupTask».
- *Extension Executable* correspond au chemin d’accès relatif du fichier .exe à démarrer.
- *Extension EntryPoint* doit toujours avoir la valeur «Windows.FullTrustApplication».
- *StartupTask TaskId* est un identificateur unique pour votre tâche. À l’aide de cet identificateur, votre application peut appeler les API de la classe [**Windows.ApplicationModel.StartupTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) pour activer ou désactiver une tâche de démarrage par programmation.
- *StartupTask Enabled* indique si la tâche est activée ou désactivée au démarrage. Les tâches activées seront exécutées la prochaine fois que l’utilisateur se connecte (sauf si celui-ci les désactive).
- *StartupTask DisplayName* est le nom de la tâche qui apparaît dans le Gestionnaire des tâches. Cette chaîne est localisable à l’aide de ```ms-resource```.

Les applications peuvent déclarer plusieurs tâches de démarrage; chacune d’elles est déclenchée et exécutée de manière indépendante. Toutes les tâches de démarrage apparaissent dans le Gestionnaire des tâches sous l’onglet **Démarrage** avec le nom spécifié dans le manifeste de votre application et l’icône de votre application. Le Gestionnaire des tâches analyse automatiquement l’impact de vos tâches sur le démarrage. Les utilisateurs peuvent choisir de désactiver manuellement la tâche de démarrage de votre application à l’aide du Gestionnaire des tâches; si un utilisateur désactive une tâche, vous ne pouvez pas la réactiver par programmation.

## <a name="app-execution-alias"></a>Alias d’exécution d’application

Un alias d’exécution d’application vous permet de spécifier un nom de mot clé pour votre application. Les utilisateurs ou les autres processus peuvent utiliser ce mot clé pour lancer facilement votre application, comme si elle se trouvait dans la variable PATH (à l’aide de la boîte de dialogue Exécuter ou d’une invite de commande, par exemple), sans fournir le chemin d’accès complet. Par exemple, si vous déclarez l’alias «Foo», un utilisateur peut taper «Foo Bar.txt» à partir de cmd.exe et votre application sera activée avec le chemin d’accès à «Bar.txt» inclus dans les arguments d’événement d’activation.

Pour spécifier un alias d’exécution d’application, ajoutez le code suivant au manifeste de votre application:

```XML
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *Extension Category* doit toujours avoir la valeur «windows.appExecutionAlias».
- *Extension Executable* correspond au chemin d’accès relatif de l’exécutable à lancer lorsque l’alias est appelé.
- *Extension EntryPoint* doit toujours avoir la valeur «Windows.FullTrustApplication».
- *ExecutionAlias Alias* est le nom court de votre application. Il doit toujours se terminer par l’extension «.exe».

Vous ne pouvez spécifier qu’un seul alias d’exécution d’application pour chaque application du package. Si plusieurs applications présentent le même alias, le système appellera la dernière inscrite. Prenez donc soin de choisir un alias peu susceptible d’être remplacé par d’autres applications.

## <a name="protocol-associations"></a>Associations de protocole

Les associations de protocole permettent l’interopérabilité entre votre application convertie et les autres programmes ou les composants système. Lorsque votre application convertie est lancée à l’aide d’un protocole, vous pouvez spécifier des paramètres spécifiques à transmettre à ses arguments d’événement d’activation afin qu’elle puisse se comporter en conséquence. Notez que les paramètres sont uniquement pris en charge pour les applications de confiance totale converties; les applications UWP ne peuvent pas les utiliser.  

Pour déclarer une association de protocole, ajoutez le code suivant au manifeste de votre application:

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *Extension Category* doit toujours avoir la valeur «windows.protocol».
- *Protocol Name* correspond au nom du protocole.
- *Protocol Parameters* est la liste des paramètres et des valeurs à transmettre en tant qu’arguments d’événement à votre application lorsqu’elle est activée. Notez que si une variable peut contenir un chemin d’accès à un fichier, vous devez placer la valeur entre guillemets afin qu’elle ne soit pas rompue en cas de transmission d’un chemin d’accès comprenant des espaces.

## <a name="files-and-file-explorer-integration"></a>Gestion de fichiers et intégration dans l’Explorateur de fichiers

Différentes options sont disponibles pour inscrire les applications converties en vue de la gestion de certains types de fichier et de l’intégration dans l’Explorateur de fichiers. Cela permet aux utilisateurs d’accéder facilement à votre application dans le cadre de leur flux de travail normal.

Pour commencer, ajoutez le code suivant au manifeste de votre application:

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *Extension Category* doit toujours avoir la valeur «windows.fileTypeAssociation».
- *FileTypeAssociation Name* est un ID unique. Cet ID est utilisé de façon interne pour générer un ProgID haché associé à votre association de type de fichier. Vous pouvez utiliser cet ID pour gérer les modifications dans les futures versions de votre application. Par exemple, si vous voulez modifier l’icône associée à une extension de fichier, vous pouvez la déplacer dans un nouvel élément FileTypeAssociation avec un autre nom.  

Ajoutez ensuite des éléments enfants supplémentaires à cette entrée en fonction des fonctionnalités spécifiques dont vous avez besoin. Les options disponibles sont décrites ci-dessous.

### <a name="supported-file-types"></a>Types de fichiers pris en charge

Votre application peut indiquer qu’elle prend en charge l’ouverture de types de fichiers spécifiques. Si un utilisateur clique avec le bouton droit sur un fichier et sélectionne Ouvrir avec, votre application apparaît dans la liste de suggestions.

Exemple:

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType* est l’extension prise en charge par votre application.

### <a name="file-context-menu-verbs"></a>Verbes du menu contextuel du fichier

Pour ouvrir des fichiers, les utilisateurs double-cliquent généralement dessus. Cependant, lorsqu’un utilisateur clique avec le bouton droit sur un fichier, le menu contextuel présente différentes options (appelées «verbes») qui précisent le type d’interaction souhaité avec le fichier: Ouvrir, Modifier, Aperçu, Imprimer, etc.

La spécification d’un type de fichier pris en charge ajoute automatiquement le verbe «Ouvrir». Cependant, les applications peuvent également ajouter des verbes personnalisés supplémentaires au menu contextuel de l’Explorateur de fichiers. Ceux-ci permettent à l’application de se lancer d’une manière spécifique selon la sélection de l’utilisateur lors de l’ouverture d’un fichier.

Exemple:

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
</uap2:SupportedVerbs>
```

- *Verb Id* est un ID unique du verbe. Si votre application est une application UWP, cet ID est transmis à votre application en tant qu’argument d’événement d’activation afin qu’elle puisse gérer la sélection de l’utilisateur de façon appropriée. Si votre application est une application convertie de confiance totale, elle reçoit des paramètres à la place (voir la puce suivante).
- *Verb Parameters* correspond à la liste des paramètres et des valeurs d’arguments associés au verbe. Si votre application est une application convertie de confiance totale, ceux-ci sont transmis en tant qu’arguments d’événements lorsqu’elle est activée pour vous permettre de personnaliser son comportement en fonction du verbe d’activation. Si une variable peut contenir un chemin d’accès à un fichier, vous devez placer la valeur entre guillemets afin qu’elle ne soit pas rompue en cas de transmission d’un chemin d’accès comprenant des espaces. Notez que si votre application est une application UWP, vous ne pouvez pas transmettre de paramètres; elle reçoit l’ID à la place (voir la puce précédente).
- *Verb Extended* indique que le verbe doit seulement apparaître si l’utilisateur maintient enfoncée la touche **Maj** avant de cliquer avec le bouton droit sur le fichier pour afficher le menu contextuel. Cet attribut est facultatif et est défini par défaut sur *False* (c’est-à-dire que le verbe est toujours affiché) s’il n’est pas répertorié. Vous devez spécifier ce comportement individuellement pour chaque verbe (à l’exception de «Ouvrir», qui est toujours défini sur *False*).
- *Verb* correspond au nom à afficher dans le menu contextuel de l’Explorateur de fichiers. Cette chaîne est localisable à l’aide de ```ms-resource```.

### <a name="shell-context-menu-verbs"></a>Verbes du menu contextuel de l’interpréteur de commande

L’ajout d’éléments au menu contextuel du dossier de l’interpréteur de commandes n’est pas pris en charge actuellement.

### <a name="multiple-selection-model"></a>Modèle de sélection multiple

Les sélections multiples vous permettent de spécifier le comportement de votre application lorsqu’elle est utilisée pour ouvrir plusieurs fichiers simultanément (par exemple, en sélectionnant 10fichiers dans l’Explorateur de fichiers et en appuyant sur Ouvrir).

Les applications de bureau converties présentent les trois mêmes options que les applications de bureau standard.
- *Player*: votre application est activée une fois, tous les fichiers sélectionnés étant transmis en tant que paramètres d’arguments.
- *Single*: votre application est activée une fois pour le premier fichier sélectionné. Les autres fichiers sont ignorés.
- *Document*: une nouvelle instance distincte de votre application est activée pour chaque fichier sélectionné.

Vous pouvez définir des préférences spécifiques pour les différents types de fichiers et d’actions. Par exemple, il se peut que vous souhaitiez ouvrir les *documents* en mode *Document* et les *images* en mode *Player*.

Pour définir le comportement de votre application, ajoutez l’attribut *MultiSelectModel* aux éléments de votre manifeste liés aux types de fichiers et au lancement de fichiers.

Définition d’un modèle pour un type de fichier pris en charge:

```XML
<uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
    <uap:SupportedFileTypes>
        <uap:FileType>.txt</uap:FileType>
</uap:SupportedFileTypes>
```

Définition d’un modèle pour des verbes:

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
```

Si votre application ne spécifie pas d’option pour la sélection multiple, la valeur par défaut est *Player* si l’utilisateur ouvre jusqu’à 15fichiers. Autrement, si votre application est une application convertie, la valeur par défaut est *Document*. Les applications UWP sont toujours lancées en mode *Player*.

### <a name="complete-example"></a>Exemple complet

Voici un exemple complet qui intègre un grand nombre des éléments liés aux fichiers et à l’Explorateur de fichiers décrits ci-dessus:

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
        <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## <a name="see-also"></a>Voir également

- [Manifeste du package de l’application](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)
