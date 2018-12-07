---
title: Distribution d’un composant Windows Runtime managé
description: Vous pouvez distribuer votre composant WindowsRuntime par copie de fichiers.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef51e2235d8ac5c46af6093809d241d5c137d57d
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8784987"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribution d’un composant Windows Runtime managé



Vous pouvez distribuer votre composant Windows Runtime par copie des fichiers. Toutefois, si votre composant comporte de nombreux fichiers, l’installation peut être fastidieuse pour vos utilisateurs. En outre, des erreurs de placement des fichiers ou l’impossibilité de définir les références peuvent leur occasionner des problèmes. Vous pouvez empaqueter un composant complexe sous la forme d’un kit de développement logiciel (SDK) d’extension Visual Studio pour en faciliter l’installation et l’utilisation. Les utilisateurs doivent uniquement définir une référence pour le package entier. Ils peuvent facilement localiser et installer votre composant à l’aide de la boîte de dialogue **Extensions et mises à jour**, comme indiqué dans l’article [Recherche et utilisation des extensions Visual Studio](https://msdn.microsoft.com/library/vstudio/dd293638.aspx) de MSDN Library.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planification d’un composant Windows Runtime distribuable

Choisissez des noms uniques pour les fichiers binaires, tels que vos fichiers .winmd. Nous recommandons le format suivant pour en garantir l’unicité :

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Vos fichiers binaires seront installés dans des packages d’application, éventuellement avec les fichiers binaires d’autres développeurs. Consultez la rubrique Kits de développement logiciel (SDK) d’extension dans l’article [Création d’un kit de développement logiciel (SDK)](https://msdn.microsoft.com/library/hh768146.aspx)de MSDN Library.

Pour décider du mode de distribution de votre composant, tenez compte de sa complexité. Un SDK d’extension ou un gestionnaire de package similaire est recommandé lorsque :

-   Votre composant comporte plusieurs fichiers.
-   Vous fournissez des versions de votre composant pour plusieurs plateformes (x86 et ARM, par exemple).
-   Vous fournissez à la fois une version de débogage et une version commerciale de votre composant.
-   Votre composant comprend des fichiers et des assemblys utilisés uniquement au moment de la conception.

Un SDK d’extension est particulièrement utile si plusieurs des conditions ci-dessus s’appliquent.

> **Remarque**pour les composants complexes, le système de gestion de packages NuGet offre une alternative open source aux SDK d’extension. Tout comme les SDK d’extension, NuGet permet de créer des packages qui simplifient l’installation des composants complexes. Pour une comparaison des packages NuGet et des SDK d’extension Visual Studio, consultez l’article [Comparaison de l’ajout de références à l’aide de NuGet et à l’aide d’un kit de développement logiciel (SDK) d’extension](https://msdn.microsoft.com/library/jj161096.aspx) de MSDN Library.

## <a name="distribution-by-file-copy"></a>Distribution par copie des fichiers

Si votre composant comporte un seul fichier .winmd ou un fichier .winmd et un fichier d’index de ressource (.pri), vous pouvez simplement mettre le fichier .winmd à la disposition des utilisateurs à des fins de copie. Les utilisateurs pourront placer le fichier où ils souhaitent dans un projet, utiliser la boîte de dialogue **Ajouter un élément existant** pour ajouter le fichier .winmd au projet, puis utiliser la boîte de dialogue Gestionnaire de références pour créer une référence. Si vous incluez un fichier .pri ou un fichier .xml, demandez aux utilisateurs de placer ces fichiers avec le fichier .winmd.

> **Remarque**Visual Studio produit toujours un fichier .pri lorsque vous générez votre composant Windows Runtime, même si votre projet ne comprend pas de ressources. Si vous avez une application test pour votre composant, vous pouvez déterminer si le fichier .pri est utilisé en examinant le contenu du package d’application dans le dossier bin\debug\AppX. Si le fichier .pri de votre composant n’apparaît pas à cet emplacement, vous n’avez pas besoin de le distribuer. Vous pouvez également utiliser l’outil [MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx) pour vider le fichier de ressources de votre projet de composant Windows Runtime. Par exemple, dans la fenêtre d’invite de commandes Visual Studio, tapez : makepri dump /if MyComponent.pri /of MyComponent.pri.xml Pour en savoir plus sur les fichiers .pri, consultez l’article [Système de gestion des ressources (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx).

## <a name="distribution-by-extension-sdk"></a>Distribution par kit de développement logiciel (SDK) d’extension

Un composant complexe inclut généralement des ressources Windows, mais reportez-vous à la remarque concernant la détection des fichiers .pri vides à la section précédente.

**Pour créer un SDK d’extension**

1.  Vérifiez que le SDK Visual Studio est installé. Vous pouvez télécharger le SDK Visual Studio depuis la page [Téléchargements Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).
2.  Créez un projet à l’aide du modèle Projet VSIX, qui se trouve sous Visual C# ou Visual Basic dans la catégorie Extensibilité. Ce modèle est installé en même temps que le SDK Visual Studio. (L’article [Procédure pas à pas : création d’un kit de développement logiciel (SDK) en C# ou Visual Basic](https://msdn.microsoft.com/library/jj127119.aspx) ou [Procédure pas à pas : création d’un kit de développement logiciel (SDK) en C++](https://msdn.microsoft.com/library/jj127117.aspx) illustre l’utilisation de ce modèle dans un scénario très simple. )
3.  Déterminez la structure de dossiers de votre SDK. La structure de dossiers commence au niveau racine de votre projet VSIX, avec les dossiers **Références**, **Package redistribuable** et **DesignTime**.

    -   **Références** correspond à l’emplacement des fichiers binaires que les utilisateurs peuvent programmer. Le SDK d’extension crée des références à ces fichiers dans les projets Visual Studio de vos utilisateurs.
    -   **Package redistribuable** correspond à l’emplacement des autres fichiers qui doivent être distribuées avec vos fichiers binaires, dans les applications qui sont créées à l’aide de votre composant.
    -   **DesignTime** correspond à l’emplacement des fichiers utilisés uniquement lorsque les développeurs créent des applications qui utilisent votre composant.

    Dans chacun de ces dossiers, vous pouvez créer des dossiers de configuration. Les noms autorisés sont debug, retail et CommonConfiguration. Le dossier CommonConfiguration est réservé aux fichiers qui sont identiques pour les versions commerciales ou de débogage. Si vous distribuez uniquement des versions commerciales de votre composant, vous pouvez tout placer dans CommonConfiguration et omettre les deux autres dossiers.

    Dans chaque dossier de configuration, vous pouvez fournir des dossiers d’architecture pour les fichiers spécifiques d’une plateforme. Si vous utilisez les mêmes fichiers pour toutes les plateformes, vous pouvez fournir un dossier unique nommé neutral. Vous trouverez plus d’informations sur la structure de dossiers, notamment d’autres noms de dossiers d’architecture, dans l’article [Création d’un kit de développement logiciel (SDK)](https://msdn.microsoft.com/library/hh768146.aspx) de MSDN Library. (Cet article présente à la fois les SDK de plateforme et les SDK d’extension. Il peut s’avérer utile de réduire la section sur les SDK de plateforme afin d’éviter toute confusion. )

4.  Créez un fichier de manifeste SDK. Le manifeste spécifie les informations sur le nom et la version, les architectures prises en charge par votre SDK, les versions de .NET Framework et d’autres informations sur le mode d’utilisation de votre SDK par Visual Studio. Vous trouverez plus d’informations et un exemple dans l’article [Création d’un kit de développement logiciel (SDK)](https://msdn.microsoft.com/library/hh768146.aspx).
5.  Générez et distribuez le kit de développement logiciel de l’extension. Pour des informations plus détaillées, notamment sur la recherche et la signature du package VSIX, consultez l’article Déploiement VSIX de MSDN Library.

## <a name="related-topics"></a>Rubriques connexes

* [Création d’un kit de développement logiciel (SDK)](https://msdn.microsoft.com/library/hh768146.aspx)
* [Système de gestion des packages NuGet](https://github.com/NuGet/Home)
* [Système de gestion des ressources (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)
* [Recherche et utilisation des extensions Visual Studio](https://msdn.microsoft.com/library/dd293638.aspx)
* [Options de commande de MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx)
