---
author: mtoepke
title: "Notions de base de l’exemple Marble Maze"
description: "Ce document présente les principales caractéristiques du projet MarbleMaze, notamment la façon dont il utilise Visual C++ dans l’environnement WindowsRuntime, mais également la façon dont il est créé, structuré et généré."
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a9df995078763df73542a4101e73e147517b1eb

---

# Notions de base de l’exemple MarbleMaze


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Ce document présente les principales caractéristiques du projet Marble Maze, notamment, la façon dont il utilise Visual C++ dans l’environnement Windows Runtime, mais également la façon dont il est créé, structuré et généré. Ce document décrit également plusieurs des conventions utilisées dans le code.

> **Remarque** L’exemple de code correspondant à ce document est disponible dans l’[exemple de jeu MarbleMaze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Ce document traite certains points importants relatifs à la planification et au développement de votre jeu de plateforme Windows universelle (UWP).

-   Utilisez le modèle **Application DirectX11 (Windows universel)** dans une applicationC++ pour créer votre jeu UWP en DirectX. Utilisez Visual Studio pour générer un projet d’application pour UWP, comme vous le feriez pour générer un projet standard.
-   Windows Runtime fournit des classes et des interfaces vous permettant de développer des applications UWP grâce à une approche orientée objet plus moderne.
-   Utilisez des références d’objet comportant le symbole d’accent circonflexe (^) pour gérer la durée de vie des variables Windows Runtime, [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) pour gérer la durée de vie des objetsCOM et [**std::shared_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026.aspx) ou [**std::unique_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601.aspx) pour gérer la durée de vie de tous les autres objetsC++ alloués par segment de mémoire.
-   Dans la majorité des cas, utilisez la gestion des exceptions plutôt que les codes de résultats pour gérer les erreurs inattendues.
-   Utilisez les annotations SAL et les outils d’analyse du code pour découvrir les erreurs présentes dans votre application.

## Création du projet Visual Studio


Si vous avez téléchargé et extrait l’exemple, ouvrez le fichier solution MarbleMaze.sln dans Visual Studio pour visualiser le code. Vous pouvez également afficher la source sur la page de la galerie MSDN de l’[exemple de jeu MarbleMaze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011) dans l’onglet **Parcourir le code**.

Nous sommes partis d’un projet existant pour la création du projet Visual Studio pour Marble Maze. Toutefois, si vous ne disposez pas déjà d’un projet offrant les fonctionnalités de base nécessaires à votre jeu pour UWP en DirectX, nous vous recommandons de créer un projet à partir du modèle VisualStudio **Application DirectX11 (Windows universel)** qui fournit une application3D avec les fonctionnalités de base en question.

L’un des paramètres de projet importants du modèle **Application DirectX11 (Windows universel)** est l’option **/ZW** qui permet au programme d’utiliser les extensions de langage WindowsRuntime. Cette option est activée par défaut lorsque vous utilisez le modèle Visual Studio.

> **Attention** L’option **/ZW** n’est pas compatible avec les options telles que **/clr**. Dans le cas de l’option **/clr**, cela signifie qu’il n’est pas possible de cibler à la fois .NET Framework et WindowsRuntime s’ils appartiennent à un même projet VisualC++.

 

Chaque application UWP que vous achetez dans le Windows Store est fournie sous la forme d’un package d’application. Un package d’application comprend un manifeste de package qui contient des informations sur votre application. Par exemple, vous pouvez spécifier les capacités (autrement dit, l’accès requis aux ressources système ou aux données utilisateur protégées) de votre application. Si vous déterminez que votre application a besoin de certaines fonctionnalités, utilisez le manifeste du package pour les déclarer. Le manifeste vous permet également de spécifier des propriétés de projet telles que les rotations prises en charge de l’appareil, les images de la vignette et l’écran de démarrage. Pour plus d’informations sur les packages d’application, voir [Création de packages d’application](https://msdn.microsoft.com/library/windows/apps/mt270969).

##  Génération, déploiement et exécution du jeu


Générez un projet d’application UWP à la manière d’un projet standard. (Dans la barre de menus, sélectionnez **Générer, Générer la solution**.) Au cours de l’étape de génération, le code est compilé et ajouté dans un package prêt à être utilisé comme une application UWP.

Après avoir généré le projet, vous devez le déployer. (Dans la barre de menus, choisissez **Générer, Déployer la solution**.) VisualStudio déploie aussi le projet lorsque vous exécutez le jeu à partir du débogueur.

Après avoir déployé le projet, choisissez la vignette Marble Maze pour exécuter le jeu. Vous pouvez également choisir dans VisualStudio, dans la barre de menus, **Déboguer, Démarrer le débogage**.

###  Contrôle du jeu

Vous pouvez utiliser les fonctions tactiles, l’accéléromètre, la manette Xbox 360 ou la souris pour contrôler le jeu Marble Maze.

-   Utilisez la croix directionnelle de la manette pour modifier l’élément de menu actif.
-   Utilisez les fonctions tactiles, le bouton A, le bouton Démarrer ou la souris pour sélectionner un élément de menu.
-   Utilisez les fonctions tactiles, l’accéléromètre, le stick analogique gauche ou la souris pour incliner le jeu.
-   Utilisez les fonctions tactiles, le bouton A, le bouton Démarrer ou la souris pour fermer les menus, tels que le tableau des scores.
-   Utilisez le bouton Démarrer ou la touche P pour suspendre ou redémarrer le jeu.
-   Utilisez le bouton Retour de la manette ou la touche Début du clavier pour redémarrer le jeu.
-   Quand le tableau des scores est visible, utilisez le bouton Retour ou la touche Début pour effacer tous les scores.

##  Conventions de code


Windows Runtime est une interface de programmation permettant de créer des applications pour UWP qui ne peuvent être exécutées que dans un environnement d’application particulier. Ces applications utilisent des fonctionnalités, des types de données et des appareils autorisés, et sont distribuées par le biais du Windows Store. Au niveau le plus bas, Windows Runtime est composé d’une interface binaire d’application. L’interface binaire d’application est un contrat binaire de bas niveau qui rend les API Windows Runtime accessibles à plusieurs langages de programmation tels que les langages JavaScript, .NET et Visual C++.

Afin d’appeler les API Windows Runtime à partir des langages JavaScript et .NET, ces langages nécessitent des projections spécifiques à chaque environnement de langage. Quand vous appelez une API Windows Runtime à partir du langage JavaScript ou .NET, vous invoquez la projection, laquelle appelle à son tour la fonction ABI sous-jacente. Bien que vous puissiez également appeler les fonctions ABI directement à partir du langage C++, Microsoft fournit également des projections pour C++, car elles permettent de consommer nettement plus facilement les API Windows Runtime, tout en maintenant de hautes performances. Microsoft fournit également des extensions de langage pour Visual C++ qui prennent en charge spécifiquement les projections Windows Runtime. Plusieurs de ces extensions de langage présentent une syntaxe similaire à celle du langage C++/CLI. Toutefois, au lieu de cibler l’environnement CLR (Common Langage Runtime), les applications natives utilisent cette syntaxe pour cibler Windows Runtime. Le modificateur de référence d’objet, ou accent circonflexe (^), est un élément important de cette nouvelle syntaxe, car il permet l’effacement automatique des objets d’exécution au moyen du décompte de références. Au lieu d’appeler des méthodes telles que **AddRef** et **Release** pour gérer la durée de vie d’un objet WindowsRuntime, le runtime supprime l’objet quand aucun autre composant ne le référence, par exemple lorsqu’il quitte l’étendue ou que vous attribuez la valeur **nullptr** à toutes les références. Une autre part importante de l’utilisation de VisualC++ pour créer des applications UWP relève du mot-clé **ref new**. Utilisez **ref new** à la place de **new** pour créer des objets WindowsRuntime avec décompte des références. Pour plus d’informations, voir [Système de types (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822).

> **Important**  
Vous devez uniquement utiliser **^** et **ref new** quand vous créez des objets ou des composants Windows Runtime. La syntaxeC++ standard peut vous servir à écrire du code de l’application principale qui n’utilise pas WindowsRuntime.

Marble Maze utilise **^** et [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) pour gérer les objets alloués par segment de mémoire et limiter les fuites de mémoire. Nous vous conseillons d’utiliser ^ pour gérer la durée de vie des variables WindowsRuntime, **ComPtr** pour gérer la durée de vie des variables COM (comme quand vous utilisez DirectX) et std::[**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026) ou [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601) pour gérer la durée de vie de tous les autres objetsC++ alloués par segment de mémoire.

 

Pour plus d’informations sur les extensions de langage qui sont à la disposition d’une application UWP enC++, voir [Informations de référence sur le langage VisualC++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871).

###  Gestion des erreurs

Le jeu Marble Maze utilise la gestion des exceptions comme principal moyen de traiter les erreurs inattendues. Même si le code de jeu utilise généralement des codes de journalisation ou des codes d’erreur, tels que les valeurs **HRESULT** pour indiquer des erreurs, la gestion des exceptions présente deuxprincipaux avantages. En premier lieu, elle facilite la lecture et la maintenance du code. Du point de vue du code, la gestion des exceptions constitue un moyen plus efficace de propager une erreur vers une routine qui se chargera de sa gestion. L’utilisation de codes d’erreur exige généralement que chaque fonction propage explicitement les erreurs. L’autre avantage est que vous pouvez configurer le débogueur Visual Studio de façon à s’arrêter immédiatement au niveau de l’emplacement et du contexte de l’erreur quand une exception est levée. Windows Runtime a largement recours à la gestion des exceptions. Par conséquent, en utilisant la gestion des exceptions dans votre code, vous pouvez combiner toute la gestion des exceptions en un seul modèle.

Nous vous recommandons d’utiliser les conventions suivantes dans votre modèle de gestion des erreurs :

-   Utilisez des exceptions pour communiquer des erreurs inattendues.
-   N’utilisez pas les exceptions pour contrôler le flux de code.
-   Interceptez uniquement les exceptions que vous pouvez gérer sans risque et à partir desquelles une récupération est possible. Sinon, n’interceptez pas l’exception et laissez l’application s’arrêter.
-   Quand vous appelez une routine DirectX qui renvoie **HRESULT**, utilisez la fonction **DX::ThrowIfFailed**. Cette fonction est définie dans DirectXSample.h. **ThrowIfFailed** lève une exception si la valeur **HRESULT** fournie est un code d’erreur. Par exemple, **E\_POINTER** provoque la levée de [**Platform::NullReferenceException**](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx) par **ThrowIfFailed**.

    Quand vous utilisez **ThrowIfFailed**, placez l’appel DirectX sur une ligne distincte pour améliorer la lisibilité du code, comme illustré dans l’exemple suivant.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Même si l’utilisation de **HRESULT** pour les erreurs inattendues est à éviter, il est encore plus important d’éviter celle de la gestion des exceptions pour contrôler le flux de code. Utilisez plutôt une valeur de retour **HRESULT**, si nécessaire, pour contrôler le flux de code.

###  Annotations SAL

Utilisez les annotations SAL et les outils d’analyse du code pour découvrir les erreurs présentes dans votre application.

Le langage SAL de Microsoft vous permet d’annoter (ou décrire) la façon dont une fonction utilise ses paramètres. Les annotations SAL décrivent également des valeurs de retour. Les annotations SAL peuvent être utilisées conjointement à l’outil d’analyse du code C/C++ pour découvrir les éventuelles erreurs du code source C ou C++. Les erreurs de codage courantes signalées par l’outil sont notamment les dépassements de mémoire tampon, une mémoire non initialisée, les déréférencements du pointeur Null et les fuites de mémoire et de ressources.

Utilisez plutôt la méthode **BasicLoader::LoadMesh** qui est déclarée dans BasicLoader.h. Cette méthode utilise \_In\_ pour spécifier que *filename* est un paramètre d’entrée (et qu’il ne peut donc faire l’objet que d’une lecture), \_Out\_ pour indiquer que *vertexBuffer* et *indexBuffer* sont des paramètres de sortie (et qu’ils feront donc uniquement l’objet d’une écriture) et \_Out\_opt\_ pour spécifier que *vertexCount* et *indexCount* sont des paramètres de sortie facultatifs (pouvant faire l’objet d’une écriture). Étant donné que *vertexCount* et *indexCount* sont des paramètres de sortie optionnels, ils peuvent avoir la valeur **nullptr**. L’outil d’analyse du code C/C++ examine les appels à cette méthode pour s’assurer que les paramètres qu’elle transmet répondent à ces critères.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Pour effectuer l’analyse du code de votre application, dans la barre de menus, sélectionnez **Générer, Exécuter l’analyse du code sur la solution**. Pour plus d’informations sur l’analyse de code, voir [Analyse de la qualité du codeC/C++ à l’aide de l’analyse du code](https://msdn.microsoft.com/library/windows/apps/ms182025.aspx).

La liste complète des annotations disponibles est définie dans le fichier sal.h. Pour plus d’informations, voir [Annotations SAL](https://msdn.microsoft.com/library/windows/apps/ms235402.aspx).

## Étapes suivantes


Pour plus d’informations sur la structure du code de l’application MarbleMaze, ainsi que sur les différences entre la structure d’une application UWP en DirectX et celle d’une application de bureau classique, voir [Structure de l’application MarbleMaze](marble-maze-application-structure.md).

## Rubriques connexes


* [Structure de l’application Marble Maze](marble-maze-application-structure.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


