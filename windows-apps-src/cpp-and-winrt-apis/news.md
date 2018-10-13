---
author: stevewhims
description: Actualités et les modifications apportées à C++ / WinRT.
title: Quelles sont les nouveautés en C++ / WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, actualités, ce qui de, les nouveaux
ms.localizationpriority: medium
ms.openlocfilehash: bc6be28e112dfdd14b3585bd88ba066fbeae382d
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4569938"
---
# <a name="whats-new-in-cwinrt"></a>Quelles sont les nouveautés en C++ / WinRT

Le tableau ci-dessous contient les actualités et devient [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) dans la dernière version disponible généralement du SDK Windows, qui est 10.0.17763.0 (Windows 10, version 1809). Ces modifications peuvent également être présentes dans les versions ultérieures de SDK Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Actualités et les modifications, dans le SDK Windows version 10.0.17763.0 (Windows 10, version 1809)

| Fonctionnalité nouvelle ou modifiée | Informations supplémentaires |
| - | - |
| **Nouveauté**. Pour pouvoir compiler, C++ / WinRT ne dépend pas les en-têtes du SDK Windows. | Une [Isolation des fichiers d’en-tête du SDK Windows](#isolation-from-windows-sdk-header-files), voir ci-dessous. |
| Le format de système de projet Visual Studio a changé. | Voir [comment recibler votre C + / projet WinRT vers une version ultérieure du SDK Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)ci-dessous. |
| Il existe de nouvelles fonctions et des classes de base pour vous aider à passer un objet de collection à une fonction Windows Runtime, ou pour implémenter vos propres propriétés de collection et les types de collection. | Voir [Collections avec C++ / WinRT](collections.md). |
| Vous pouvez utiliser l’extension de balisage [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) avec votre C++ / WinRT classes d’exécution. | Pour plus d’informations et des exemples de code, consultez la [vue d’ensemble de liaison de données](/windows/uwp/data-binding/data-binding-quickstart). |
| Prise en charge de l’annulation d’une coroutine vous permet d’inscrire un rappel d’annulation. | Pour plus d’informations et des exemples de code, voir [l’annulation d’une opération asynchrone et les rappels de l’annulation](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Lorsque vous créez un délégué pointant vers une fonction membre, vous pouvez établir une référence forte ou faible à l’objet actif (au lieu d’un pointeur brut *cette* ) au niveau du point où le gestionnaire est enregistré. | Pour plus d’informations et des exemples de code, voir la sous-section **Si vous utilisez une fonction membre en tant que délégué** dans la section [en toute sécurité accéder à *ce* pointeur avec un délégué de gestion des événements](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bogues corrigés qui ont été découvert par la conformité d’améliorée de Visual Studio à la norme C++. La chaîne d’outils LLVM et Clang est également mieux utilisé pour valider C++ / conformité aux normes de WinRT. | Vous rencontrerez n’est plus le problème décrit dans [Pourquoi ne mon projet compilation? Je suis à l’aide de Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et du SDK version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Autres modifications.

- **Nouveauté**. [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) retourne désormais `void*` à la place de `HSTRING`. Vous pouvez utiliser `static_cast<HSTRING>(get_abi(my_hstring));` pour obtenir une HSTRING.
- **Nouveauté**. [**WinRT::put_abi(WinRT::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) maintenant renvoie `void**` à la place de `HSTRING*`. Vous pouvez utiliser `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` pour obtenir une HSTRING *.
- **Nouveauté**. HRESULT est désormais projetée en tant que **winrt::hresult**. Si vous avez besoin HRESULT (d’un contrôle de type ou de prendre en charge les caractéristiques de type), vous pouvez `static_cast` un **winrt::hresult**. Dans le cas contraire, **winrt::hresult** convertit en HRESULT, tant que vous incluez `unknwn.h` avant que vous incluez des C++ / WinRT en-têtes.
- **Nouveauté**. GUID est désormais projeté en tant que **winrt::guid**. Pour les API que vous implémentez, vous devez utiliser **winrt::guid** pour les paramètres GUID. Dans le cas contraire, **winrt::hresult** convertit en GUID, tant que vous incluez `unknwn.h` avant que vous incluez des C++ / WinRT en-têtes.
- **Nouveauté**. Le [**constructeur de winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) a été renforcée par ce qui rend explicite (il est désormais plus difficile à écrire du code incorrect avec celui-ci). Si vous avez besoin d’affecter une valeur de handle brut, appelez la [**fonction de handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) à la place.
- **Nouveauté**. Les signatures de **WINRT_CanUnloadNow** et **WINRT_GetActivationFactory** ont été modifiés. Vous ne déclarez ces fonctions du tout. Au lieu de cela, incluez `winrt/base.h` (qui est automatiquement inclus si vous incluez des C / fichiers d’en-tête espace de noms WinRT Windows) à inclure les déclarations de ces fonctions.
- Pour la [**structure de winrt::clock**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** sont déconseillées par **from_file_time/to_file_time**.
- API qui attendent **IBuffer** paramètres est simplifiés. Bien que la plupart des API préférez collections ou des tableaux, suffisamment API s’appuient sur **IBuffer** qu’il est nécessaire pour être plus facile à utiliser ces API à partir de C++. Cette mise à jour fournit un accès direct aux données derrière une implémentation **IBuffer** , à l’aide de la même convention d’affectation de noms données utilisée par les conteneurs de la bibliothèque C++ Standard. Cela évite également collision avec des noms de métadonnées qui traditionnellement commencent par une lettre majuscule.
- Amélioration de la génération de code: diverses améliorations afin de réduire la taille du code, améliorer inline et optimiser la mise en cache en usine.
- Supprimé récurrence inutile. Lorsque la ligne de commande fait référence à un dossier, plutôt qu’à un spécifique `.winmd`, `cppwinrt.exe` outil recherche n’est plus de manière récursive pour `.winmd` fichiers. Le `cppwinrt.exe` outil également gère désormais doublons plus intelligemment, rendant plus résistants à une erreur de l’utilisateur et à mal formé `.winmd` fichiers.
- Pointeurs intelligents renforcés. Auparavant, les revokers événement n’a pas pu révoquer lorsque déplacement attribué une nouvelle valeur. Cela a permis de découvrir un problème dans lequel les classes de pointeur intelligent n’ont pas été gérer de façon fiable attribution automatique; racine dans le [**modèle de structure winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** a été corrigée et les revokers événement fixés pour gérer sémantique de déplacement correctement afin qu’ils révoquer lors de l’attribution.

> [!NOTE]
> Avec la version 1.0.181002.2 (ou version ultérieure) de la [C++ / Extension WinRT Visual Studio (VSIX)](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix) installé, la création d’un nouveau C++ / WinRT projet installe automatiquement le [package NuGet de Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour ce projet. Le package NuGet de Microsoft.Windows.CppWinRT fournit améliorée C++ / WinRT projet build prise en charge, rendre votre projet portable entre un ordinateur de développement et d’un agent de build (sur lequel seul le package NuGet et pas VSIX, est installé).
>
> Pour un projet existant&mdash;une fois que vous avez installé la version 1.0.181002.2 (ou version ultérieure) du projet VSIX&mdash;nous recommandons que vous ouvrez le projet dans Visual Studio, cliquez sur **le projet** \> **Gérer les Packages NuGet …**  \>  **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package de ce projet.


## <a name="isolation-from-windows-sdk-header-files"></a>Isolation des fichiers d’en-tête du SDK Windows

Il s’agit d’une modification avec rupture de votre code potentiellement.

Pour pouvoir compiler, C++ / WinRT dépend ne sont plus des fichiers d’en-tête du SDK Windows. Fichiers d’en-tête dans la bibliothèque d’exécution C (CRT) et la bibliothèque STL (C++ Standard Template Library) également n’incluent pas les en-têtes du SDK Windows. Et qui améliore la conformité aux normes, qui permet d’éviter les dépendances par inadvertance et réduit considérablement le nombre de macros dont vous disposez pour vous protéger contre.

Cette autonomie signifie que C++ / WinRT est désormais plus portable et conforme aux normes, et il va sens de la possibilité de qu’elle devienne une bibliothèque inter-compilateur et inter-plateforme. Cela signifie également que le C++ / WinRT en-têtes ne sont pas des macros attaqués.

Si vous l’avez précédemment laissé vers C++ / WinRT à inclure les en-têtes de Windows dans votre projet, vous devez maintenant inclure vous-même. Il s’agit, dans tous les cas, toujours conseillée pour inclure explicitement les en-têtes que vous dépendez et ne laissez pas à une autre bibliothèque pour les inclure pour vous.

Actuellement, les seules exceptions à l’isolation de fichier d’en-tête du SDK Windows sont pour intrinsèques et les valeurs numériques. Il n’existe aucun problème connu avec ces derniers dépendances restants.

Dans votre projet, vous pouvez réactiver l’interopérabilité avec les en-têtes du SDK Windows si vous avez besoin. Par exemple, vous souhaiterez implémenter une interface COM (ancrées au cœur [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Dans cet exemple, incluent `unknwn.h` avant que vous incluez des C++ / WinRT en-têtes. Ainsi, C++ / WinRT les bibliothèque de base pour activer les raccordements différentes prendre en charge des interfaces COM classiques. Pour obtenir un exemple de code, voir [composants auteur COM avec C++ / WinRT](author-coclasses.md). De même, inclure explicitement tous les autres en-têtes du SDK Windows qui déclarent les types et fonctions que vous voulez appeler.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment recibler votre C + / projet WinRT vers une version ultérieure du SDK Windows

La méthode recibler votre projet qui est susceptible d’entraîner le problème de compilateur et l’éditeur de liens tours est également le plus fastidieux. Cette méthode implique la création d’un nouveau projet (ciblant la version du SDK Windows de votre choix), puis copier les fichiers à votre nouveau projet à partir de votre ancien. Il y aura des sections de votre ancien `.vcxproj` et `.vcxproj.filters` fichiers que vous pouvez simplement copier plus pour enregistrer vous Ajout des fichiers dans Visual Studio.

Toutefois, il existe deux autres façons de recibler votre projet dans Visual Studio.

- Accédez à la propriété **générale**du projet \> **Version du SDK Windows**et sélectionnez **Toutes les Configurations** et **Toutes les plateformes**. Définissez la **Version du SDK Windows** à la version que vous souhaitez cibler.
- Dans l' **Explorateur de solutions**, cliquez sur le nœud de projet et cliquez sur **Recibler les projets**, choisir les ou les versions que vous voulez cibler, puis cliquez sur **OK**.

Si vous rencontrez n’importe quel compilateur ou des erreurs de l’éditeur de liens après l’utilisation d’une de ces deux méthodes, vous pouvez essayer de nettoyage de la solution (**Build** > **Nettoyer la Solution** et/ou supprimer manuellement tous les fichiers et dossiers temporaires) avant d’essayer de générer une nouvelle fois.

Si le compilateur C++ génère»*erreur C2039: 'IUnknown': n’est pas un membre de ' espace de noms \'global''*», puis ajoutez `#include <unknwn.h>` vers le haut de votre `pch.h` fichier (avant que vous incluez des C / WinRT en-têtes).

Vous devrez également ajouter `#include <hstring.h>` après cela.

Si l’éditeur de liens C++ génère «*erreur LNK2019: symbole externe non résolu _WINRT_CanUnloadNow@0 référencé dans la fonction _VSDesignerCanUnloadNow@0 *», vous pouvez alors résoudre qui en ajoutant `#define _VSDESIGNER_DONT_LOAD_AS_DLL` à votre `pch.h` fichier.
