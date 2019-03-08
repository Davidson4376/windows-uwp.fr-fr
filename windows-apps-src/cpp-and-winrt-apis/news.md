---
description: Nouveautés et modifications apportées à C++ / c++ / WinRT.
title: Quelles sont les nouveautés en C / c++ / WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c ++, cpp, winrt, projection, actualités, ce qui de, les nouveaux
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639954"
---
# <a name="whats-new-in-cwinrt"></a>Quelles sont les nouveautés en C / c++ / WinRT

Le tableau ci-dessous contient des informations et les modifications apportées aux [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) dans la dernière version la disposition générale du SDK Windows, qui est 10.0.17763.0 (Windows 10, version 1809). Ces modifications peuvent également être présentes dans les versions ultérieures de kit de développement logiciel Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Actualités et les modifications, dans la version 10.0.17763.0 du SDK de Windows (Windows 10, version 1809)

| Fonctionnalités nouvelles ou modifiées | Informations supplémentaires |
| - | - |
| **Modification avec rupture**. Pour pouvoir compiler, c++ / WinRT ne dépend pas les en-têtes à partir du SDK Windows. | Consultez [Isolation à partir des fichiers d’en-tête Windows SDK](#isolation-from-windows-sdk-header-files), ci-dessous. |
| Le format de système de projet Visual Studio a changé. | Consultez [comment recibler votre C + c++ / projet WinRT vers une version ultérieure du SDK Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), ci-dessous. |
| Il existe de nouvelles fonctions et classes de base pour vous aider à passer un objet de collection à une fonction Windows Runtime, ou pour implémenter vos propres propriétés de collection et les types de collection. | Consultez [Collections avec C / c++ / WinRT](collections.md). |
| Vous pouvez utiliser la [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) extension de balisage avec votre C / c++ / WinRT des classes de runtime. | Pour plus d’informations et d’exemples de code, consultez [vue d’ensemble de liaison de données](/windows/uwp/data-binding/data-binding-quickstart). |
| Prise en charge de l’annulation d’une coroutine permet d’inscrire un rappel d’annulation. | Pour plus d’informations et d’exemples de code, consultez [annuler une opération asynchrone et les rappels d’annulation](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Lorsque vous créez un délégué qui pointe vers une fonction membre, vous pouvez établir un nom fort ou une référence faible à l’objet actif (au lieu d’un texte brut *cela* pointeur) au point où le gestionnaire est enregistré. | Pour plus d’informations et d’exemples de code, consultez le **si vous utilisez une fonction membre en tant que délégué** sous-section dans la section [en toute sécurité l’accès à la *cela* pointeur avec un délégué de gestion des événements](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bogues résolus qui ont été découverts par la conformité est améliorée de Visual Studio à la norme C++. La chaîne d’outils LLVM et Clang est également mieux utilisé pour valider C + c++ / la mise en conformité aux normes de WinRT. | Vous rencontrerez n’est plus le problème décrit dans [Pourquoi mon nouveau projet n’est pas compilé ? J’utilise Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et le SDK version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Autres modifications.

- **Modification avec rupture**. [**WinRT::get_abi(WinRT::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) retourne désormais `void*` au lieu de `HSTRING`. Vous pouvez utiliser `static_cast<HSTRING>(get_abi(my_hstring));` pour obtenir une chaîne HSTRING.
- **Modification avec rupture**. [**WinRT::put_abi(WinRT::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) retourne désormais `void**` au lieu de `HSTRING*`. Vous pouvez utiliser `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` pour obtenir un HSTRING *.
- **Modification avec rupture**. HRESULT est désormais projeté en tant que **winrt::hresult**. Si vous avez besoin d’un HRESULT (d’effectuer la vérification de type, ou pour prendre en charge les caractéristiques de type), vous pouvez `static_cast` un **winrt::hresult**. Sinon, **winrt::hresult** convertit vers HRESULT, tant que vous incluez `unknwn.h` avant d’inclure n’importe quel C + c++ / WinRT en-têtes.
- **Modification avec rupture**. GUID est désormais projeté en tant que **winrt::guid**. Pour les API que vous implémentez, vous devez utiliser **winrt::guid** pour les paramètres GUID. Sinon, **winrt::hresult** convertit en GUID, tant que vous incluez `unknwn.h` avant d’inclure n’importe quel C + c++ / WinRT en-têtes.
- **Modification avec rupture**. Le [ **winrt::handle_type constructeur** ](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) a été renforcé en le rendant explicite (il est désormais plus difficile d’écrire du code incorrect avec lui). Si vous avez besoin d’assigner une valeur de handle brut, appelez le [ **handle_type::attach fonction** ](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) à la place.
- **Modification avec rupture**. Les signatures de **WINRT_CanUnloadNow** et **WINRT_GetActivationFactory** ont été modifiés. Vous ne doit pas déclarer ces fonctions du tout. Au lieu de cela, incluez `winrt/base.h` (qui est automatiquement inclus si vous incluez tout C + c++ / fichiers d’en-tête espace de noms WinRT Windows) pour inclure les déclarations de ces fonctions.
- Pour le [ **winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** sont déconseillés en faveur du **from_file_time/to_file_time**.
- API qui attendent **IBuffer** paramètres sont simplifiées. Bien que la plupart des API préférez collections ou les tableaux, suffisamment API s’appuient sur **IBuffer** qu’il devait être plus facile à utiliser ces API à partir de C++. Cette mise à jour offre un accès direct aux données derrière un **IBuffer** implémentation, à l’aide de la même convention d’affectation de noms de données utilisée par les conteneurs de bibliothèque C++ Standard. Cela vous évite également de conflit avec les noms de métadonnées qui traditionnellement commencent par une lettre majuscule.
- Amélioration de la génération de code : diverses améliorations afin de réduire la taille du code, améliorer incorporation (inlining) et d’optimiser la mise en cache de fabrique.
- Supprimer la récursivité inutile. Lorsque la ligne de commande fait référence à un dossier, plutôt qu’à un spécifique `.winmd`, le `cppwinrt.exe` outil ne recherche plus de manière récursive pour `.winmd` fichiers. Le `cppwinrt.exe` outil également gère désormais les doublons plus intelligemment, rendant plus résistants aux erreurs des utilisateurs et à mal formée `.winmd` fichiers.
- Pointeurs intelligents renforcés. Auparavant, les revokers événement n’a pas pu révoquer lorsque move-attribuer une nouvelle valeur. Cela a permis de découvrir un problème où les classes de pointeur intelligent n’étaient pas gérer de façon fiable auto-affectation ; enraciné dans le [ **winrt::com_ptr struct modèle**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** a été résolu, et les revokers événement fixés pour gérer la sémantique de déplacement correctement afin qu’ils révoquent lors de l’affectation.

> [!IMPORTANT]
> Modifications importantes ont été apportées à la [C++ / c++ / WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix), à la fois dans la version 1.0.181002.2 et ensuite dans la version 1.0.190128.4. Pour plus d’informations de ces modifications, et comment elles affectent vos projets existants, [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) et [les versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="isolation-from-windows-sdk-header-files"></a>Isolation des fichiers d’en-tête Windows SDK

Il s’agit potentiellement d’une modification avec rupture pour votre code.

Pour pouvoir compiler, c++ / WinRT ne dépend plus des fichiers d’en-tête à partir du SDK Windows. Fichiers d’en-tête dans la bibliothèque d’exécution C (CRT) et le C++ bibliothèque STL (Standard Template) n’également incluent des en-têtes Windows SDK. Et qui améliore la conformité aux normes, évite toute dépendance par inadvertance et réduit considérablement le nombre de macros que vous avez pour vous prémunir contre.

Cette indépendance signifie que C++ / c++ / WinRT est désormais plus portable et conformes aux normes, et il améliore la possibilité qu’elles deviennent une bibliothèque de compilateur croisé et multiplateforme. Cela signifie également que le C++ / c++ / WinRT en-têtes ne sont pas affectés négativement macros.

Si vous avez précédemment le laissé à C++ / c++ / WinRT pour inclure tous les en-têtes Windows dans votre projet, vous devez maintenant pour les inclure. Il est, dans tous les cas, toujours meilleure pratique pour inclure explicitement les en-têtes dont vous dépendez et pas le laisser à une autre bibliothèque pour les inclure.

Actuellement, les seules exceptions à l’isolation de fichier d’en-tête Windows SDK concernent les fonctions intrinsèques et les valeurs numériques. Il n’existe aucun problème connu avec ces dépendances de la dernière.

Dans votre projet, vous pouvez réactiver l’interopérabilité avec les en-têtes Windows SDK si vous avez besoin. Peut, par exemple, voulez-vous implémenter une interface COM (enraciné dans [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Pour cet exemple, inclure `unknwn.h` avant d’inclure n’importe quel C + c++ / WinRT en-têtes. Ainsi, le C + c++ / WinRT les bibliothèque de base pour activer des hooks prendre en charge les interfaces du COM classique. Pour obtenir un exemple de code, consultez [composants COM de l’auteur avec C / c++ / WinRT](author-coclasses.md). De même, incluez explicitement tous les autres en-têtes Windows SDK qui déclarent des types et/ou les fonctions que vous souhaitez appeler.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment recibler votre C + c++ / projet WinRT vers une version ultérieure du SDK Windows

La méthode recibler votre projet est susceptible d’entraîner le problème de compilateur et éditeur de liens moins est également plus fastidieuse. Cette méthode implique la création d’un projet (en ciblant la version du Kit de développement logiciel Windows de votre choix), puis copier les fichiers sur votre nouveau projet à partir de votre ancien. Il y aura des sections de votre ancienne `.vcxproj` et `.vcxproj.filters` les fichiers que vous pouvez simplement copier plus à vous faire économiser de l’ajout de fichiers dans Visual Studio.

Toutefois, il existe deux autres façons de recibler votre projet dans Visual Studio.

- Accédez à la propriété de projet **général** \> **Windows SDK Version**, puis sélectionnez **toutes les Configurations** et **toutes les plateformes**. Définissez **Windows SDK Version** vers la version que vous souhaitez cibler.
- Dans **l’Explorateur de solutions**, cliquez sur le nœud de projet, cliquez sur **recibler les projets**, choisissez l’ou les versions à cibler, puis cliquez sur **OK**.

Si vous rencontrez n’importe quel compilateur ou des erreurs de l’éditeur de liens après avoir utilisé une de ces deux méthodes, vous pouvez essayer la solution de nettoyage (**Build** > **nettoyer la Solution** et/ou supprimez manuellement tous les fichiers et dossiers temporaires) avant d’essayer de générer une nouvelle fois.

Si le compilateur C++ génère «*erreur C2039 : « IUnknown » : n’est pas un membre de '\`espace de noms global''*», puis ajoutez `#include <unknwn.h>` vers le haut de votre `pch.h` fichier (avant d’inclure n’importe quel C + c++ / WinRT en-têtes).

Vous devrez peut-être également ajouter `#include <hstring.h>` après cela.

Si l’éditeur de liens C++ produit «*erreur LNK2019 : symbole externe non résolu _WINRT_CanUnloadNow@0 référencé dans la fonction _VSDesignerCanUnloadNow@0* », vous pouvez alors résoudre qui en ajoutant `#define _VSDESIGNER_DONT_LOAD_AS_DLL` à votre `pch.h` fichier.
