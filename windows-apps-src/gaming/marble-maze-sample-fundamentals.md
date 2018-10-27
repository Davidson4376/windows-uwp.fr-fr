---
author: eliotcowley
title: Notions de base de l’exemple Marble Maze
description: Ce document décrit les caractéristiques fondamentales du projet Marble Maze; par exemple, comment il utilise Visual C++ dans l’environnement Windows Runtime, comment elle est créée et structuré et généré.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.author: elcowle
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, jeux, exemples, directx, principes de base
ms.localizationpriority: medium
ms.openlocfilehash: f595c8f429c93a13d6342c281a90f3b0f5741621
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5683229"
---
# <a name="marble-maze-sample-fundamentals"></a>Principes de base de l’exemple MarbleMaze




Cette rubrique présente les principales caractéristiques du projet MarbleMaze, notamment la façon dont il utilise Visual C++ dans l’environnement WindowsRuntime, mais également la façon dont il est créé, structuré et généré. Cette rubrique décrit également plusieurs des conventions utilisées dans le code.

> [!NOTE]
> L’exemple de code correspondant à ce document est disponible dans [l’exemple de jeu Marble Maze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

Ce document traite certains points importants relatifs à la planification et au développement de votre jeu de plateforme Windows universelle (UWP).

-   Utilisez le modèle Visual C++ **Application DirectX11 (Windows universel)** dans Visual Studio pour créer votre jeu UWP DirectX.
-   Windows Runtime fournit des classes et des interfaces vous permettant de développer des applications UWP grâce à une approche orientée objet plus moderne.
-   Utilisez des références d’objet comportant le symbole d’accent circonflexe (^) pour gérer la durée de vie des variables Windows Runtime, [Microsoft::WRL::ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) pour gérer la durée de vie des objetsCOM et [std::shared_ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) ou [std::unique_ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) pour gérer la durée de vie de tous les autres objetsC++ alloués par segment de mémoire.
-   Dans la majorité des cas, utilisez la gestion des exceptions plutôt que les codes de résultats pour gérer les erreurs inattendues.
-   Utilisez les [annotations SAL](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) conjointement avec les outils d’analyse du code pour aider à détecter les erreurs dans votre application.

## <a name="creating-the-visual-studio-project"></a>Création du projet Visual Studio


Si vous avez téléchargé et extrait l’exemple, vous pouvez ouvrir le fichier **MarbleMaze_VS2017.sln** (dans le dossier **C++** ) dans Visual Studio, et vous avez le code.

Nous sommes partis d’un projet existant pour la création du projet Visual Studio pour Marble Maze. Toutefois, si vous ne disposez pas déjà d’un projet offrant les fonctionnalités de base nécessaires à votre jeu pour UWP en DirectX, nous vous recommandons de créer un projet à partir du modèle VisualStudio **Application DirectX11 (Windows universel)** qui fournit une application3D avec les fonctionnalités de base en question. Pour cela, procédez comme suit:

1. Dans Visual Studio 2017, sélectionnez **fichier > Nouveau > projet …**

2. Dans la fenêtre **Nouveau projet** , dans le volet gauche, sélectionnez **installés > Modèles > Visual C++**.

3. Dans la liste du milieu, sélectionnez **Application DirectX 11 (Windows universel)**. Si vous ne voyez pas cette option, vous devrez pas les composants requis installés&mdash;voir [Modifier Visual Studio 2017 en ajoutant ou supprimant des charges de travail et les composants](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) pour plus d’informations sur l’installation des composants supplémentaires.

4. Donnez à votre projet un **nom**et un **emplacement** pour les fichiers à stocker un **nom de la Solution**, puis cliquez sur **OK**.

![Nouveau projet](images/marble-maze-sample-fundamentals-1.png)

L’un des paramètres de projet importants du modèle **Application DirectX11 (Windows universel)** est l’option **/ZW** qui permet au programme d’utiliser les extensions de langage WindowsRuntime. Cette option est activée par défaut lorsque vous utilisez le modèle Visual Studio. Voir [Définition des options du compilateur](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options) pour plus d’informations sur la définition des options du compilateur dans Visual Studio.

> **Attention**  l’option **/ZW** n’est pas compatible avec les options telles que **/clr**. Dans le cas de **/clr**, cela signifie que vous ne pouvez pas cibler .NETFramework et Windows Runtime à partir du même projet Visual C++.

 

Chaque application UWP que vous acquérez dans le Microsoft Store est fourni sous la forme d’un package d’application. Un package d’application comprend un manifeste de package qui contient des informations sur votre application. Par exemple, vous pouvez spécifier les capacités (autrement dit, l’accès requis aux ressources système ou aux données utilisateur protégées) de votre application. Si vous déterminez que votre application a besoin de certaines fonctionnalités, utilisez le manifeste du package pour les déclarer. Le manifeste vous permet également de spécifier des propriétés de projet telles que les rotations prises en charge de l’appareil, les images de la vignette et l’écran de démarrage. Vous pouvez modifier le manifeste en ouvrant **Package.appxmanifest** dans votre projet. Pour plus d’informations sur les packages d’application, voir [Création de packages d’application](https://msdn.microsoft.com/library/windows/apps/mt270969).

##  <a name="building-deploying-and-running-the-game"></a>Génération, déploiement et exécution du jeu

Dans les menus déroulants situés dans la partie supérieure de Visual Studio, à gauche du bouton vert de lecture, sélectionnez votre configuration de déploiement. Nous vous recommandons de la définir sur **Déboguer** en ciblant l’architecture de votre appareil (**x86** pour 32bits, **x64** pour 64bits) et sur votre **machine locale**. Vous pouvez également effectuer un test sur une **machine distante**, ou sur un **appareil** connecté via USB. Cliquez ensuite sur le bouton vert de lecture pour générer et effectuer le déploiement sur votre appareil.

![Débogage; x64; Ordinateur local](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>Contrôle du jeu

Vous pouvez utiliser les entrées tactiles, l’accéléromètre, la manette Xbox One ou la souris pour contrôler Marble Maze.

-   Utilisez la croix directionnelle de la manette pour modifier l’élément de menu actif.
-   Utiliser la fonctionnalité tactile, le A ou un écran de démarrage bouton sur le contrôleur, ou la souris pour sélectionner un élément de menu.
-   Utilisez les fonctions tactiles, l’accéléromètre, le stick analogique gauche ou la souris pour incliner le jeu.
-   Utiliser la fonctionnalité tactile, le A ou un écran de démarrage bouton sur le contrôleur, ou la souris pour fermer les menus, tels que le tableau des scores.
-   Utilisez le bouton Démarrer sur le contrôleur ou la touche P du clavier pour interrompre ou reprendre le jeu.
-   Utilisez le bouton Retour de la manette ou la touche Début du clavier pour redémarrer le jeu.
-   Lorsque le tableau des scores est visible, utilisez le bouton précédent sur le contrôleur ou la touche début du clavier pour effacer tous les scores.

##  <a name="code-conventions"></a>Conventions de code


Windows Runtime est une interface de programmation permettant de créer des applications pour UWP qui ne peuvent être exécutées que dans un environnement d’application particulier. Ces applications utilisent des fonctions autorisées, les types de données et les appareils et sont distribuées depuis le Microsoft Store. Au niveau le plus bas, Windows Runtime est composé d’une interface binaire d’application. L’interface binaire d’application est un contrat binaire de bas niveau qui rend les API Windows Runtime accessibles à plusieurs langages de programmation tels que les langages JavaScript, .NET et Visual C++.

Afin d’appeler les API Windows Runtime à partir des langages JavaScript et .NET, ces langages nécessitent des projections spécifiques à chaque environnement de langage. Quand vous appelez une API Windows Runtime à partir du langage JavaScript ou .NET, vous invoquez la projection, laquelle appelle à son tour la fonction ABI sous-jacente. Bien que vous puissiez également appeler les fonctions ABI directement à partir du langage C++, Microsoft fournit également des projections pour C++, car elles permettent de consommer nettement plus facilement les API Windows Runtime, tout en maintenant de hautes performances. Microsoft fournit également des extensions de langage pour Visual C++ qui prennent en charge spécifiquement les projections Windows Runtime. Plusieurs de ces extensions de langage présentent une syntaxe similaire à celle du langage C++/CLI. Toutefois, au lieu de cibler l’environnement CLR (Common Langage Runtime), les applications natives utilisent cette syntaxe pour cibler Windows Runtime. Le modificateur de référence d’objet, ou accent circonflexe (^), est un élément important de cette nouvelle syntaxe, car il permet l’effacement automatique des objets d’exécution au moyen du décompte de références. Au lieu d’appeler des méthodes telles que [AddRef](https://msdn.microsoft.com/library/windows/desktop/ms691379) et [Release](https://msdn.microsoft.com/library/windows/desktop/ms682317) pour gérer la durée de vie d’un objet WindowsRuntime, le runtime supprime l’objet quand aucun autre composant ne le référence, par exemple lorsqu’il quitte l’étendue ou que vous attribuez la valeur **nullptr** à toutes les références. Une autre part importante de l’utilisation de VisualC++ pour créer des applications UWP relève du mot-clé **ref new**. Utilisez **ref new** à la place de **new** pour créer des objets WindowsRuntime avec décompte des références. Pour plus d’informations, voir [Système de types (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822).

> [!IMPORTANT]
> Vous devez uniquement utiliser **^** et **ref new** quand vous créez des objets ou des composants Windows Runtime. La syntaxeC++ standard peut vous servir à écrire du code de l’application principale qui n’utilise pas WindowsRuntime.

Marble Maze utilise **^** et **Microsoft::WRL::ComPtr** pour gérer les objets alloués par segment de mémoire et limiter les fuites de mémoire. Nous vous recommandons d’utiliser ^ pour gérer la durée de vie des variables Windows Runtime, **ComPtr** pour gérer la durée de vie des variables COM (par exemple, lorsque vous utilisez DirectX) et **std::shared\_ptr** ou **std::unique\_ptr** pour gérer la durée de vie de tous les autres objets C++ alloués par segment de tas.

 

Pour plus d’informations sur les extensions de langage qui sont à la disposition d’une application UWP enC++, voir [Informations de référence sur le langage VisualC++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871).

###  <a name="error-handling"></a>Gestion des erreurs

Le jeu Marble Maze utilise la gestion des exceptions comme principal moyen de traiter les erreurs inattendues. Même si le code de jeu utilise généralement des codes de journalisation ou des codes d’erreur, tels que les valeurs **HRESULT** pour indiquer des erreurs, la gestion des exceptions présente deuxprincipaux avantages. En premier lieu, elle facilite la lecture et la maintenance du code. Du point de vue du code, la gestion des exceptions constitue un moyen plus efficace de propager une erreur vers une routine qui se chargera de sa gestion. L’utilisation de codes d’erreur exige généralement que chaque fonction propage explicitement les erreurs. L’autre avantage est que vous pouvez configurer le débogueur Visual Studio de façon à s’arrêter immédiatement au niveau de l’emplacement et du contexte de l’erreur quand une exception est levée. Windows Runtime a largement recours à la gestion des exceptions. Par conséquent, en utilisant la gestion des exceptions dans votre code, vous pouvez combiner toute la gestion des exceptions en un seul modèle.

Nous vous recommandons d’utiliser les conventions suivantes dans votre modèle de gestion des erreurs :

-   Utilisez des exceptions pour communiquer des erreurs inattendues.
-   N’utilisez pas les exceptions pour contrôler le flux de code.
-   Interceptez uniquement les exceptions que vous pouvez gérer sans risque et à partir desquelles une récupération est possible. Sinon, n’interceptez pas l’exception et laissez l’application s’arrêter.
-   Quand vous appelez une routine DirectX qui renvoie **HRESULT**, utilisez la fonction **DX::ThrowIfFailed**. Cette fonction est définie dans [directxhelper.h:](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h). **ThrowIfFailed** lève une exception si fourni **HRESULT** est un code d’erreur. Par exemple, **E\_POINTER** provoque la levée de [Platform::NullReferenceException](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx) par **ThrowIfFailed**.

    Quand vous utilisez **ThrowIfFailed**, placez l’appel DirectX sur une ligne distincte pour améliorer la lisibilité du code, comme illustré dans l’exemple suivant.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Bien que nous vous recommandons d’éviter l’utilisation de **HRESULT** pour les erreurs inattendues, il est plus important d’éviter l’utilisation de la gestion des exceptions pour contrôler le flux de code. Utilisez plutôt une valeur de retour **HRESULT**, si nécessaire, pour contrôler le flux de code.

###  <a name="sal-annotations"></a>Annotations SAL

Utilisez les annotations SAL et les outils d’analyse du code pour découvrir les erreurs présentes dans votre application.

Le langage SAL de Microsoft vous permet d’annoter (ou décrire) la façon dont une fonction utilise ses paramètres. Les annotations SAL décrivent également des valeurs de retour. Les annotations SAL peuvent être utilisées conjointement à l’outil d’analyse du code C/C++ pour découvrir les éventuelles erreurs du code source C ou C++. Les erreurs de codage courantes signalées par l’outil sont notamment les dépassements de mémoire tampon, une mémoire non initialisée, les déréférencements du pointeur Null et les fuites de mémoire et de ressources.

Examinons la méthode **BasicLoader::LoadMesh** , qui est déclarée dans [BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h). Cette méthode utilise `_In_` pour spécifier le *nom de fichier* est un paramètre d’entrée (et par conséquent sera uniquement être lue à partir de), `_Out_` pour spécifier que *vertexBuffer* et *indexBuffer* sont des paramètres de sortie (et par conséquent sera uniquement écrit), et `_Out_opt_` pour spécifier que *vertexCount* et *indexCount* sont facultatifs paramètres de sortie (et peuvent être écrits pour). Étant donné que *vertexCount* et *indexCount* sont des paramètres de sortie optionnels, ils peuvent avoir la valeur **nullptr**. L’outil d’analyse du code C/C++ examine les appels à cette méthode pour s’assurer que les paramètres qu’elle transmet répondent à ces critères.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Pour effectuer l’analyse du code de votre application, dans la barre de menus, choisissez **Build > exécuter l’analyse du Code sur la Solution**. Pour plus d’informations sur l’analyse de code, voir [Analyse de la qualité du codeC/C++ à l’aide de l’analyse du code](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

La liste complète des annotations disponibles est définie dans le fichier sal.h. Pour plus d’informations, voir [Annotations SAL](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Étapes suivantes


Pour plus d’informations sur la structure du code de l’application MarbleMaze, ainsi que sur les différences entre la structure d’une application UWP en DirectX et celle d’une application de bureau classique, voir [Structure de l’application MarbleMaze](marble-maze-application-structure.md).

## <a name="related-topics"></a>Rubriques connexes


* [Structure de l’application Marble Maze](marble-maze-application-structure.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




