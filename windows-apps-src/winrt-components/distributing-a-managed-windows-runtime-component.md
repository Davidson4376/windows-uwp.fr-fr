---
title: Distribution d’un composant Windows Runtime managé
description: Vous pouvez distribuer votre composant Windows Runtime par copie des fichiers.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4af684fb8352cef154f83ad30723ff3f9bd6aa42
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393700"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribution d’un composant Windows Runtime managé

Vous pouvez distribuer votre composant Windows Runtime par copie des fichiers. Toutefois, si votre composant comporte de nombreux fichiers, l’installation peut être fastidieuse pour vos utilisateurs. En outre, des erreurs de placement des fichiers ou l’impossibilité de définir les références peuvent leur occasionner des problèmes. Vous pouvez empaqueter un composant complexe sous la forme d’un kit de développement logiciel (SDK) d’extension Visual Studio pour en faciliter l’installation et l’utilisation. Les utilisateurs doivent uniquement définir une référence pour le package entier. Ils peuvent facilement localiser et installer votre composant à l’aide de la boîte de dialogue **Extensions et mises à jour**, comme indiqué dans l’article [Recherche et utilisation des extensions Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015) de MSDN Library.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planification d’un composant Windows Runtime distribuable

Choisissez des noms uniques pour les fichiers binaires, tels que vos fichiers .winmd. Nous recommandons le format suivant pour en garantir l’unicité :

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Vos fichiers binaires seront installés dans des packages d’application, éventuellement avec les fichiers binaires d’autres développeurs. Pour plus d’informations, consultez [« SDK d’extension » dans How to : Créez un kit](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)de développement logiciel (SDK) dans MSDN Library.

Pour décider du mode de distribution de votre composant, tenez compte de sa complexité. Un SDK d’extension ou un gestionnaire de package similaire est recommandé lorsque :

-   Votre composant comporte plusieurs fichiers.
-   Vous fournissez des versions de votre composant pour plusieurs plateformes (x86 et ARM, par exemple).
-   Vous fournissez à la fois une version de débogage et une version commerciale de votre composant.
-   Votre composant comprend des fichiers et des assemblys utilisés uniquement au moment de la conception.

Un SDK d’extension est particulièrement utile si plusieurs des conditions ci-dessus s’appliquent.

> **Remarque pour les**composants complexes, le système de gestion des packages NuGet offre une alternative open source aux kits de développement logiciel (SDK) d’extension.   Tout comme les SDK d’extension, NuGet permet de créer des packages qui simplifient l’installation des composants complexes. Pour une comparaison des packages NuGet et des SDK d’extension Visual Studio, consultez l’article [Comparaison de l’ajout de références à l’aide de NuGet et à l’aide d’un kit de développement logiciel (SDK) d’extension](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015) de MSDN Library.

## <a name="distribution-by-file-copy"></a>Distribution par copie des fichiers

Si votre composant comporte un seul fichier .winmd ou un fichier .winmd et un fichier d’index de ressource (.pri), vous pouvez simplement mettre le fichier .winmd à la disposition des utilisateurs à des fins de copie. Les utilisateurs pourront placer le fichier où ils souhaitent dans un projet, utiliser la boîte de dialogue **Ajouter un élément existant** pour ajouter le fichier .winmd au projet, puis utiliser la boîte de dialogue Gestionnaire de références pour créer une référence. Si vous incluez un fichier .pri ou un fichier .xml, demandez aux utilisateurs de placer ces fichiers avec le fichier .winmd.

> **Remarque Visual Studio**produit toujours un fichier. pri lorsque vous générez votre composant Windows Runtime, même si votre projet n’inclut aucune ressource.   Si vous disposez d’une application de test pour votre composant, vous pouvez déterminer si le fichier. pri est utilisé en examinant le contenu du package d’application\\dans\\le dossier AppX de débogage bin. Si le fichier .pri de votre composant n’apparaît pas à cet emplacement, vous n’avez pas besoin de le distribuer. Vous pouvez également utiliser l’outil [MakePRI.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) pour vider le fichier de ressources de votre projet de composant Windows Runtime. Par exemple, dans la fenêtre d’invite de commandes Visual Studio, tapez : makepri dump /if MyComponent.pri /of MyComponent.pri.xml Pour en savoir plus sur les fichiers .pri, consultez l’article [Système de gestion des ressources (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Distribution par kit de développement logiciel (SDK) d’extension

Un composant complexe inclut généralement des ressources Windows, mais reportez-vous à la remarque concernant la détection des fichiers .pri vides à la section précédente.

**Pour créer un kit de développement logiciel (SDK) d’extension**

1.  Vérifiez que le SDK Visual Studio est installé. Vous pouvez télécharger le SDK Visual Studio depuis la page [Téléchargements Visual Studio](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs).
2.  Créez un projet à l’aide du modèle Projet VSIX, qui se trouve sous Visual C# ou Visual Basic dans la catégorie Extensibilité. Ce modèle est installé en même temps que le SDK Visual Studio. ([Procédure pas à pas : Création d’un kit C# de développement](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) logiciel [à l’aide de ou Visual Basic ou procédure pas à pas : La création d’un C++ ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)Kit de développement logiciel (SDK) à l’aide de, illustre l’utilisation de ce modèle dans un scénario très simple. )
3.  Déterminez la structure de dossiers de votre SDK. La structure de dossiers commence au niveau racine de votre projet VSIX, avec les dossiers **Références**, **Package redistribuable** et **DesignTime**.

    -   **Références** correspond à l’emplacement des fichiers binaires que les utilisateurs peuvent programmer. Le SDK d’extension crée des références à ces fichiers dans les projets Visual Studio de vos utilisateurs.
    -   **Package redistribuable** correspond à l’emplacement des autres fichiers qui doivent être distribuées avec vos fichiers binaires, dans les applications qui sont créées à l’aide de votre composant.
    -   **DesignTime** correspond à l’emplacement des fichiers utilisés uniquement lorsque les développeurs créent des applications qui utilisent votre composant.

    Dans chacun de ces dossiers, vous pouvez créer des dossiers de configuration. Les noms autorisés sont debug, retail et CommonConfiguration. Le dossier CommonConfiguration est réservé aux fichiers qui sont identiques pour les versions commerciales ou de débogage. Si vous distribuez uniquement des versions commerciales de votre composant, vous pouvez tout placer dans CommonConfiguration et omettre les deux autres dossiers.

    Dans chaque dossier de configuration, vous pouvez fournir des dossiers d’architecture pour les fichiers spécifiques d’une plateforme. Si vous utilisez les mêmes fichiers pour toutes les plateformes, vous pouvez fournir un dossier unique nommé neutral. Vous pouvez trouver des informations sur la structure des dossiers, notamment d’autres noms de [dossiers d’architecture, dans Procédure : Créez un kit](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)de développement logiciel (SDK) dans MSDN Library. (Cet article présente à la fois les SDK de plateforme et les SDK d’extension. Il peut s’avérer utile de réduire la section sur les SDK de plateforme afin d’éviter toute confusion. )

4.  Créez un fichier de manifeste SDK. Le manifeste spécifie les informations de nom et de version, les architectures prises en charge par votre kit de développement logiciel (SDK), les versions .NET et d’autres informations sur la façon dont Visual Studio utilise votre SDK. Vous trouverez plus d’informations et un exemple [dans Procédure : Créer un kit](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)de développement logiciel (SDK).
5.  Générez et distribuez le kit de développement logiciel de l’extension. Pour des informations plus détaillées, notamment sur la recherche et la signature du package VSIX, consultez l’article Déploiement VSIX de MSDN Library.

## <a name="related-topics"></a>Rubriques connexes

* [Création d’un Kit de développement logiciel (SDK)](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [système de gestion des packages NuGet](https://github.com/NuGet/Home)
* [Système de gestion des ressources (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Recherche et utilisation des extensions Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Options de commande MakePRI. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
