---
author: stevewhims
description: Actualités et les modifications apportées à C++ / WinRT.
title: Quelles sont les nouveautés en C++ / WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, actualités, ce qui de, les nouveaux
ms.localizationpriority: medium
ms.openlocfilehash: 1ada059dc2acfa96dd61b6f3460e25736d96ff68
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6982248"
---
# <a name="whats-new-in-cwinrt"></a>Quelles sont les nouveautés en C++ / WinRT

Le tableau ci-dessous contient les actualités et les modifications apportées à [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) dans la dernière version disponible en général du SDK Windows, qui est 10.0.17763.0 (Windows 10, version 1809). Ces modifications peuvent également être présentes dans les versions ultérieures de SDK Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Actualités et les modifications, dans le SDK Windows version 10.0.17763.0 (Windows 10, version 1809)

| Fonctionnalité nouvelle ou modifiée | Informations supplémentaires |
| - | - |
| **Nouveauté**. Pour qu’il se compile, C++ / WinRT ne dépend pas les en-têtes du SDK Windows. | Une [Isolation des fichiers d’en-tête du SDK Windows](#isolation-from-windows-sdk-header-files), voir ci-dessous. |
| Le format de système de projet Visual Studio a changé. | Voir [comment recibler votre C++ / WinRT les projet vers une version ultérieure du SDK Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)ci-dessous. |
| Il existe de nouvelles fonctions et des classes de base pour vous aider à passer un objet de collection à une fonction Windows Runtime, ou pour implémenter vos propres propriétés de collection et les types de collection. | Voir [Collections avec C++ / WinRT](collections.md). |
| Vous pouvez utiliser l’extension de balisage [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) avec votre C++ / WinRT classes d’exécution. | Pour plus d’informations et des exemples de code, consultez la [vue d’ensemble de liaison de données](/windows/uwp/data-binding/data-binding-quickstart). |
| Prise en charge de l’annulation d’une coroutine vous permet d’inscrire un rappel d’annulation. | Pour plus d’informations et d’exemples de code, voir [l’annulation d’une opération asynchrone et les rappels de l’annulation](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Lorsque vous créez un délégué pointant vers une fonction membre, vous pouvez établir une référence forte ou faible à l’objet actif (au lieu d’un pointeur brut *cette* ) au niveau du point où le gestionnaire est enregistré. | Pour plus d’informations et des exemples de code, consultez la section **Si vous utilisez une fonction membre en tant que délégué** dans la section [en toute sécurité l’accès à *ce* pointeur avec un délégué de gestion des événements](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bogues corrigés qui ont été découvert par la mise en conformité améliorée de Visual Studio à la norme C++. La chaîne d’outils LLVM et Clang est également mieux utilisé pour valider C++ / conformité aux normes de WinRT. | Vous rencontrerez n’est plus le problème décrit dans [Pourquoi ne mon projet compilation? J’utilise Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et du SDK version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Autres modifications.

- **Nouveauté**. [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) retourne désormais `void*` à la place de `HSTRING`. Vous pouvez utiliser `static_cast<HSTRING>(get_abi(my_hstring));` pour obtenir une HSTRING.
- **Nouveauté**. [**WinRT::put_abi(WinRT::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) maintenant renvoie `void**` à la place de `HSTRING*`. Vous pouvez utiliser `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` pour obtenir une HSTRING *.
- **Nouveauté**. HRESULT est désormais projetée en tant que **winrt::hresult**. Si vous avez besoin HRESULT (d’un contrôle de type ou de prendre en charge les caractéristiques de type), vous pouvez `static_cast` un **winrt::hresult**. Dans le cas contraire, **winrt::hresult** convertit en HRESULT, tant que vous incluez `unknwn.h` avant que vous incluez des C++ / WinRT en-têtes.
- **Nouveauté**. GUID est désormais projeté en tant que **winrt::guid**. Pour les API que vous implémentez, vous devez utiliser **winrt::guid** pour les paramètres GUID. Dans le cas contraire, **winrt::hresult** convertit en GUID, tant que vous incluez `unknwn.h` avant que vous incluez des C++ / WinRT en-têtes.
- **Nouveauté**. Le [**constructeur de winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) a été renforcée en la rendant explicite (il est désormais plus difficile à écrire du code incorrect avec celui-ci). Si vous avez besoin d’affecter une valeur de handle brut, appelez la [**fonction de handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) à la place.
- **Nouveauté**. Les signatures de **WINRT_CanUnloadNow** et **WINRT_GetActivationFactory** ont été modifiés. Vous up ne doit pas déclarer ces fonctions du tout. Au lieu de cela, incluez `winrt/base.h` (qui est automatiquement inclus si vous incluez des C / fichiers d’en-tête espace de noms WinRT Windows) à inclure les déclarations de ces fonctions.
- Pour la [**structure de winrt::clock**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** sont déconseillées en faveur de **from_file_time/to_file_time**.
- API qui attendent **IBuffer** paramètres est simplifiés. Bien que la plupart des API préférez collections ou des tableaux, suffisamment API s’appuient sur **IBuffer** qu’il est nécessaire pour être plus facile à utiliser ces API à partir de C++. Cette mise à jour fournit un accès direct aux données derrière une implémentation **IBuffer** , à l’aide de la même convention d’affectation de noms des données utilisée par les conteneurs de la bibliothèque C++ Standard. Cela évite également collision avec des noms de métadonnées qui commencent par convention avec une lettre majuscule.
- Génération de code améliorée: plusieurs améliorations afin de réduire la taille du code, améliorer inline et optimiser la mise en cache en usine.
- Supprimé récurrence inutile. Lorsque la ligne de commande fait référence à un dossier, plutôt qu’à un spécifique `.winmd`, la `cppwinrt.exe` outil recherche n’est plus de manière récursive pour `.winmd` fichiers. Le `cppwinrt.exe` outil également gère désormais doublons plus intelligemment, rendant plus résistants à une erreur de l’utilisateur et de mal formé `.winmd` fichiers.
- Sécurité renforcée des pointeurs intelligents. Auparavant, les revokers événement n’a pas pu révoquer quand déplacement attribué une nouvelle valeur. Cela a permis de découvrir un problème dans lequel les classes de pointeur intelligent n’étaient pas gérer de façon fiable attribution automatique; racine dans le [**modèle de structure winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** a été résolu, et les revokers événement fixés pour gérer sémantique de déplacement correctement afin qu’ils révoquer lors de l’attribution.

> [!NOTE]
> Avec la version 1.0.181002.2 (ou version ultérieure) de la [C++ / Extension WinRT Visual Studio (VSIX)](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix) installé, la création d’un nouveau C++ / WinRT projet installe automatiquement le [package NuGet de Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour ce projet. Le package NuGet de Microsoft.Windows.CppWinRT fournit améliorée C++ / WinRT projet build prise en charge, qui devient votre projet portable entre un ordinateur de développement et d’un agent de build (sur lequel uniquement le package NuGet et pas VSIX, est installé).
>
> Pour un projet existant&mdash;une fois que vous avez installé la version 1.0.181002.2 (ou version ultérieure) du projet VSIX&mdash;nous recommandons que vous ouvrez le projet dans Visual Studio, cliquez sur **le projet** \> **Gérer les Packages NuGet …**  \>  **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package de ce projet.


## <a name="isolation-from-windows-sdk-header-files"></a>Isolation des fichiers d’en-tête du SDK Windows

Il s’agit potentiellement un changement majeur pour votre code.

Pour qu’il se compile, C++ / WinRT dépend n’est plus de fichiers d’en-tête du SDK Windows. Fichiers d’en-tête dans la bibliothèque d’exécution C (CRT) et la bibliothèque STL (C++ Standard Template Library) n’également incluent tous les en-têtes du SDK Windows. Et qui améliore la conformité aux normes, qui permet d’éviter les dépendances par inadvertance et réduit considérablement le nombre de macros dont vous disposez pour vous protéger contre.

Cette autonomie signifie que C++ / WinRT est désormais plus portable et conforme aux normes, et il va sens de la possibilité de qu’elle devienne une bibliothèque inter-compilateur et inter-plateforme. Cela signifie également que le C++ / WinRT en-têtes ne sont pas des macros attaqués.

Si vous l’avez précédemment laissé vers C++ / WinRT pour inclure tous les en-têtes Windows dans votre projet, vous devez maintenant pour les inclure. Il est, dans tous les cas, toujours conseillée pour inclure explicitement les en-têtes qui vous dépendez et en l’absence d’une autre bibliothèque pour les inclure pour vous.

Actuellement, les seules exceptions à l’isolation de fichier d’en-tête SDK Windows sont pour les intrinsèques et les valeurs numériques. Il n’existe aucun problème connu avec ces derniers dépendances restants.

Dans votre projet, vous pouvez réactiver l’interopérabilité avec les en-têtes du SDK Windows si vous avez besoin. Par exemple, vous souhaiterez implémenter une interface COM (émanant de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Dans cet exemple, incluent `unknwn.h` avant que vous incluez des C++ / WinRT en-têtes. Ainsi, C++ / WinRT les bibliothèque de base pour activer les raccordements différentes prendre en charge des interfaces COM classiques. Pour obtenir un exemple de code, voir [composants auteur COM avec C++ / WinRT](author-coclasses.md). De même, inclure explicitement tous les en-têtes du SDK Windows qui déclarent des types et fonctions que vous voulez appeler.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment recibler votre C++ / WinRT les projet vers une version ultérieure du SDK Windows

La méthode recibler votre projet qui est susceptible d’entraîner le problème de compilateur et l’éditeur de liens tours est également aussi la plus longue. Cette méthode implique la création d’un nouveau projet (ciblant la version du SDK Windows de votre choix) et puis copier les fichiers sur votre nouveau projet à partir de votre ancien. Il y aura des sections de votre ancien `.vcxproj` et `.vcxproj.filters` fichiers que vous pouvez simplement copier plus de vous faire gagner Ajout de fichiers dans Visual Studio.

Toutefois, il existe deux autres façons de recibler votre projet dans Visual Studio.

- Accédez à **l’onglet Général**des propriétés de projet \> **Version du SDK Windows**et sélectionnez **Toutes les Configurations** et **Toutes les plateformes**. Définissez la **Version du SDK Windows** à la version que vous voulez cibler.
- Dans l' **Explorateur de solutions**, cliquez sur le nœud de projet et cliquez sur **Recibler les projets**, choisir les ou les versions que vous voulez cibler, puis cliquez sur **OK**.

Si vous rencontrez n’importe quel compilateur ou des erreurs de l’éditeur de liens après l’utilisation d’une de ces deux méthodes, vous pouvez essayer de nettoyage de la solution (**Build** > **Nettoyer la Solution** et/ou supprimer manuellement tous les fichiers et dossiers temporaires) avant d’essayer de générer une nouvelle fois.

Si le compilateur C++ génère «*erreur C2039: 'IUnknown': n’est pas un membre de ' espace de noms \'global''*», puis ajoutez `#include <unknwn.h>` vers le haut de votre `pch.h` fichier (avant que vous incluez des C / WinRT en-têtes).

Vous devrez également ajouter `#include <hstring.h>` après cela.

Si l’éditeur de liens C++ génère «*erreur LNK2019: symbole externe non résolu _WINRT_CanUnloadNow@0 référencé dans la fonction _VSDesignerCanUnloadNow@0 *», vous pouvez alors résoudre qui en ajoutant `#define _VSDESIGNER_DONT_LOAD_AS_DLL` à votre `pch.h` fichier.
