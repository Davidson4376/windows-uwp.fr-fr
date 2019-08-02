---
description: Nouveautés et modifications apportées à C++/WinRT.
title: Nouveautés de C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, actualités, nouveautés, nouveau
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e1fd738435b8622a2db2e849abf1c4984bb7ae64
ms.sourcegitcommit: fccefde61a155a4a5a866acd1c4c9de42a14ddfd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68507726"
---
# <a name="whats-new-in-cwinrt"></a>Nouveautés de C++/WinRT

Au fur et à mesure que de nouvelles versions de C++/WinRT sont publiées, cette rubrique décrit les nouveautés et les modifications apportées.

## <a name="news-and-changes-in-cwinrt-20"></a>Nouveautés et modifications dans C++/WinRT 2.0

Pour plus d’informations sur l’[ extension Visual Studio C++/WinRT (VSIX)](https://aka.ms/cppwinrt/vsix), le [package NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) et l’outil `cppwinrt.exe` (notamment sur la manière de les acquérir et de les installer), consultez [Prise en charge Visual Studio pour C++/WinRT, XAML, l’extension VSIX et le package NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Modifications apportées à l’extension Visual Studio C++/WinRT (VSIX) pour la version 2.0

- Le visualiseur de débogage prend désormais en charge Visual Studio 2019 et continue de prendre en charge Visual Studio 2017.
- De nombreuses corrections de bogues ont été effectuées.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Modifications apportées au package NuGet Microsoft.Windows.CppWinRT pour la version 2.0

- L’outil `cppwinrt.exe` est désormais inclus dans le package NuGet Microsoft.Windows.CppWinRT et il génère des en-têtes de projection de plateforme pour chaque projet à la demande. Par conséquent, l’outil `cppwinrt.exe` ne dépend plus du SDK Windows (même s’il est toujours fourni avec le SDK pour des raisons de compatibilité).
- `cppwinrt.exe` génère maintenant des en-têtes de projection sous chaque dossier intermédiaire spécifique de la plateforme/configuration ($IntDir) pour activer des builds parallèles.
- La prise en charge des builds C++/WinRT (propriétés/cibles) est maintenant entièrement documentée, au cas où vous souhaiteriez personnaliser manuellement vos fichiers projet. Consultez le fichier [Lisez-moi](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) du package NuGet Microsoft.Windows.CppWinRT.
- De nombreuses corrections de bogues ont été effectuées.

### <a name="changes-to-cwinrt-for-version-20"></a>Modifications apportées à C++/WinRT pour la version 2.0

#### <a name="open-source"></a>Open source

L’outil `cppwinrt.exe` prend un fichier de métadonnées Windows Runtime (`.winmd`) et génère, à partir de celui-ci, une bibliothèque C++ standard basée sur un fichier d’en-tête qui *projette* les API décrites dans les métadonnées. Ainsi, vous pouvez utiliser ces API à partir de votre code C++/WinRT.

Cet outil est désormais un projet entièrement open source, disponible sur GitHub. Visitez [Microsoft\/xlang](https://github.com/Microsoft/xlang), puis cliquez sur **src** > **tool** > **cppwinrt**.

#### <a name="xlang-libraries"></a>Bibliothèques xlang

Une bibliothèque à en-tête uniquement entièrement portable (pour l’analyse du format de métadonnées ECMA-335 utilisé par Windows Runtime) constitue la base de tous les outils Windows Runtime et xlang à venir. Nous avons notamment aussi réécrit l’outil `cppwinrt.exe` ex nihilo en utilisant les bibliothèques xlang. Les requêtes de métadonnées sont ainsi beaucoup plus précises, ce qui permet de résoudre certains problèmes de longue date liés à la projection du langage C++/WinRT.

#### <a name="fewer-dependencies"></a>Moins de dépendances

À cause du lecteur de métadonnées xlang, l’outil `cppwinrt.exe` lui-même a moins de dépendances. Il est ainsi beaucoup plus flexible et utilisable dans plus de scénarios, en particulier dans les environnements de build contraints. Il ne s’appuie notamment plus sur `RoMetadata.dll`.
 
Voici les dépendances pour `cppwinrt.exe` 2.0.
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

Toutes ces DLL sont disponibles sur Windows 10, mais également sur les versions antérieures, jusqu’à Windows 7, et même Windows Vista. Si vous le souhaitez, votre ancien serveur de builds exécutant Windows 7 peut désormais exécuter `cppwinrt.exe` pour générer des en-têtes C++ pour votre projet. Moyennant quelques manipulations, vous pouvez même [exécuter C++/WinRT sur Windows 7](https://github.com/kennykerr/win7), si cela vous intéresse.

À l’inverse, `cppwinrt.exe` 1.0 comporte ces dépendances.

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>Attribut `noexcept` de Windows Runtime

Le Windows Runtime a un nouvel attribut `[noexcept]`, que vous pouvez utiliser pour décorer vos méthodes et propriétés dans [MIDL 3.0](/uwp/midl-3/predefined-attributes). La présence de l’attribut indique aux outils associés que votre implémentation ne lève pas d’exceptions (et ne retourne pas de HRESULT ayant échoué). Ainsi, les projections de langage optimisent la génération de code en évitant la surcharge de la gestion des exceptions nécessaire pour prendre en charge les appels d’interface binaire d’application (ABI) en risque d’échec.

C++/WinRT tire parti de cette optimisation en produisant des implémentations `noexcept` C++ à la fois du code de consommation et du code de création. Si vous avez des méthodes ou propriétés d’API sans échec et que la taille du code vous préoccupe, vous pouvez examiner cet attribut.

#### <a name="optimized-code-generation"></a>Génération de code optimisée

C++/WinRT génère désormais du code source C++ encore plus efficace (en arrière-plan) pour que le compilateur C++ puisse produire le code binaire le plus petit et le plus efficace possible. De nombreuses améliorations ont vocation à réduire le coût de la gestion des exceptions en évitant les informations de déroulement inutiles. Les binaires qui utilisent de grandes quantités de code C++/WinRT observent une réduction d’environ 4 % de la taille du code. Le code est également plus efficace (il s’exécute plus rapidement) en raison du nombre réduit d’instructions.

Ces améliorations s’appuient sur une nouvelle fonctionnalité d’interopérabilité également à votre disposition. Tous les types C++/WinRT propriétaires de ressources incluent désormais un constructeur à des fins de prise de possession directe, ce qui permet d’éviter l’approche antérieure en deux étapes.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Génération du code de gestion des exceptions optimisée

Cette modification complète le travail effectué par l’équipe en charge de l’optimiseur Microsoft C++ pour réduire le coût de la gestion des exceptions. Si vous utilisez massivement des interfaces binaires d’applications (ABI) (par exemple, COM) dans votre code, vous pouvez observer qu’une grande quantité de code suit ce modèle.

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/WinRT lui-même génère ce modèle pour toutes les API implémentées. Avec des milliers de fonctions d’API, toute optimisation peut ici s’avérer significative. Par le passé, l’optimiseur n’aurait pas détecté que ces blocs catch sont tous identiques, si bien qu’il dupliquait beaucoup de code autour de chaque ABI (qui à leur tour contribuaient à la croyance que l’utilisation d’exceptions dans le code système générait des binaires volumineux). Toutefois, à partir de Visual Studio 2019, le compilateur C++ incorpore tous ces funclets catch et stocke uniquement ceux qui sont uniques. Il en résulte une réduction globale supplémentaire de 18 % de la taille du code pour les binaires qui s’appuient lourdement sur ce modèle. Non seulement le code de gestion des exeptions est maintenant plus efficace qu’avec l’utilisation de codes de retour, mais la préoccupation liée à la taille plus importante des binaires relève à présent du passé.

#### <a name="incremental-build-improvements"></a>Améliorations des builds incrémentielles

L’outil `cppwinrt.exe` compare désormais la sortie d’un fichier d’en-tête/source généré au contenu de tout fichier existant sur le disque, puis il écrit dans le fichier uniquement si le fichier a été réellement modifié. Cela fait gagner un temps considérable avec les E/S de disque et garantit que les fichiers ne sont pas considérés comme « modifiés » par le compilateur C++. En conséquence, la recompilation est évitée ou réduite, dans de nombreux cas.

#### <a name="generic-interfaces-are-now-all-generated"></a>Interfaces génériques maintenant toutes générées

En raison du lecteur de métadonnées xlang, C++/WinRT génère désormais toutes les interfaces paramétrables, ou génériques, à partir des métadonnées. Les interfaces comme [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) sont désormais générées à partir des métadonnées plutôt que manuscrites dans `winrt/base.h`. Le résultat est que la taille de `winrt/base.h` a été réduite de moitié et que les optimisations sont générées directement dans le code (ce qui était difficile à faire avec l’approche manuelle).

> [!IMPORTANT]
> Les interfaces comme l’exemple donné apparaissent désormais dans leurs en-têtes d’espace de noms respectifs, plutôt que dans `winrt/base.h`. Ainsi, si vous ne l’avez pas déjà fait, vous devez inclure l’en-tête d’espace de noms approprié afin d’utiliser l’interface.

#### <a name="component-optimizations"></a>Optimisations du composant

Cette mise à jour ajoute une prise en charge de plusieurs optimisations supplémentaires volontaires pour C++/WinRT, décrites dans les sections ci-dessous. Étant donné que ces optimisations sont des changements cassants (pour lesquels vous risquez de devoir apporter des changements mineurs pour les prendre en charge), vous devez les activer explicitement. Dans Visual Studio, définissez la propriété du projet **Propriétés communes** > **C++/WinRT** > **Optimisé** sur *Oui*. `<CppWinRTOptimized>true</CppWinRTOptimized>` est alors ajouté à votre fichier projet. Cela revient à ajouter le commutateur `-opt[imize]` lors de l’appel de `cppwinrt.exe` à partir de la ligne de commande.

Un nouveau projet (provenant d’un modèle de projet) utilisera `-opt` par défaut.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Construction uniforme et accès à l’implémentation direct

Ces deux optimisations permettent à votre composant d’accéder directement à ses propres types d’implémentation, même quand il utilise uniquement les types projetés. Il est inutile d’utiliser [**make**](/uwp/cpp-ref-for-winrt/make), [**make_self**](/uwp/cpp-ref-for-winrt/make-self) ou [**get_self**](/uwp/cpp-ref-for-winrt/get-self) si vous voulez simplement utiliser la surface d’API publique. Vos appels se compileront jusqu’aux appels directs dans l’implémentation, et ceux-ci peuvent même être entièrement inline.

Pour plus d’informations et des exemples de code, consultez [Accepter la onstruction uniforme et l'accès à l'implémentation direct](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

##### <a name="type-erased-factories"></a>Fabriques avec types effacés

Cette optimisation évite les dépendances #include dans `module.g.cpp` afin qu’il n’ait pas besoin d’être recompilé à chaque modification d’une seule classe d’implémentation. Les performances des builds s’en retrouvent alors améliorées.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>`module.g.cpp` plus intelligent et plus efficace pour les projets volumineux avec plusieurs bibliothèques

Le fichier `module.g.cpp` contient à présent deux autres assistances composables nommées **winrt_can_unload_now** et **winrt_get_activation_factory**. Celles-ci ont été conçues pour les projets volumineux dans lesquels une DLL comprend plusieurs bibliothèques, chacune avec ses propres classes d’exécution. Dans ce cas, vous devez assembler manuellement les **DllGetActivationFactory** et **DllCanUnloadNow** de la DLL. Ces assistances vous facilitent grandement la tâche, en vous évitant les fausses erreurs d’origine. L’indicateur `-lib` de l’outil `cppwinrt.exe` peut également être utilisé pour donner à chaque bibliothèque individuelle son propre préambule (au lieu de `winrt_xxx`) afin que les fonctions de chaque bibliothèque puissent être nommées individuellement et par conséquent combinées sans ambiguïté.

#### <a name="coroutine-support"></a>Prise en charge des coroutines

La prise en charge des coroutines est automatiquement incluse. Auparavant, la prise en charge se trouvait à plusieurs endroits, ce qui était trop contraignant à notre avis. Puis, de façon temporaire pour v2.0, un fichier d’en-tête `winrt/coroutine.h` était nécessaire, mais il ne l’est plus. Étant donné que les interfaces asynchrones Windows Runtime sont maintenant générées, au lieu d’être manuscrites, elles se trouvent désormais dans `winrt/Windows.Foundation.h`. Outre une plus grande facilité de maintenance et de prise en charge, cela signifie que les assistances de coroutine comme [**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) n’ont plus besoin d’être ajoutées à la fin de l’en-tête d’un espace de noms spécifique. En effet, elles peuvent inclure plus naturellement leurs dépendances. Ainsi, **resume_foreground** peut prendre en charge non seulement la reprise sur un [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) donné, mais également la reprise sur un [ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) donné. Auparavant, un seul pouvait être pris en charge, mais pas les deux, dans la mesure où la définition ne pouvait résider que dans un seul espace de noms.

Voici un exemple de prise en charge de **DispatcherQueue**.

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

Les assistances de coroutine sont désormais également décorées avec `[[nodiscard]]`, ce qui en améliore l’utilisabilité. Si vous oubliez de (ou ne savez pas pas que vous devez) les `co_await` pour qu’elles fonctionnent, alors, en raison de `[[nodiscard]]`, ces erreurs produisent maintenant un avertissement du compilateur.

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>Aide au diagnostic des allocations directes (pile)

Étant donné que les noms de classe projetés et d’implémentation sont (par défaut) les mêmes et ne diffèrent que par l’espace de noms, il est possible de les confondre et de créer accidentellement une implémentation sur la pile, plutôt que d’utiliser la famille [**make**](/uwp/cpp-ref-for-winrt/make) d’assistances. Cela peut être difficile à diagnostiquer dans certains cas, étant donné que l’objet peut être détruit alors que des références en attente sont toujours en version d’évaluation. Une assertion améliore maintenant cela pour les versions de débogage. Tandis que l’assertion ne détecte pas l’allocation de la pile à l’intérieur d’une coroutine, elle s’avère néanmoins utile pour intercepter la plupart des erreurs de ce type.

Pour plus d'informations, consultez [Diagnostic des allocations directes](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Assistances de capture améliorées et délégués variadiques

Cette mise à jour corrige la limitation avec les assistances de capture en prenant également en charge les types projetés. Cela se produit de temps en temps avec les API d’interopérabilité Windows Runtime, quand elles retournent un type projeté.

Cette mise à jour ajoute également une prise en charge de [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) et [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) lors de la création d’un délégué variadique (non-Windows Runtime).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Prise en charge de la destruction différée et méthode QI sans échec pendant la destruction

Il n’est pas rare dans le destructeur d’un objet de classe runtime d’appeler une méthode qui perturbe temporairement le compteur de références. Quand le compteur de références revient à zéro, l’objet est détruit une deuxième fois. Dans une application XAML, il peut être nécessaire d’exécuter une méthode [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) dans un destructeur, afin d’appeler une implémentation du nettoyage vers le haut ou le bas de la hiérarchie. Le compteur de références de l’objet ayant cependant déjà atteint zéro, cette méthode QI entraîne donc également un rebond du compteur de références.

Cette mise à jour ajoute la prise en charge d’un antirebond du compteur de références, garantissant que quand il atteint zéro, il ne peut jamais repartir, tout en vous permettant d’exécuter une méthode QI pour tous les éléments temporaires nécessaires lors de la destruction. Cette procédure est inévitable dans certaines applications/certains contrôles XAML et C++/WinRT y est désormais résilient.

Vous pouvez différer la destruction en fournissant une fonction **final_release** statique sur votre type d’implémentation. Le dernier pointeur restant vers l’objet, sous la forme d’un **std::unique_ptr**, est passé à votre **final_release**. Vous pouvez ensuite choisir de déplacer la propriété de ce pointeur à un autre contexte. Vous pouvez sans problème appliquer une méthode QI au pointeur sans déclencher une double destruction. Mais la modification nette du compteur de références doit être égale à zéro au moment où vous détruisez l’objet.

La valeur de retour de **final_release** peut être `void`, un objet de l’opération asynchrone comme [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) ou **winrt::fire_and_forget**.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

Dans l’exemple ci-dessous, une fois que **MainPage** est publié (pour la dernière fois), **final_release** est appelé. Cette fonction passe cinq secondes en attente (sur le pool de threads), puis elle reprend à l’aide du **Dispatcher** de la page (qui nécessite une méthode QI/AddRef/Release pour fonctionner). Elle nettoie ensuite une ressource sur ce thread d’interface utilisateur. Enfin, elle efface le **unique_ptr**, ce qui provoque l’appel du destructeur **MainPage**. Même dans ce destructeur, **DataContext** est appelé, ce qui nécessite une méthode QI pour **IFrameworkElement**.

Vous n’êtes pas obligé d’implémenter votre **final_release** en tant que coroutine. Cela fonctionne cependant et simplifie considérablement le déplacement de la destruction vers un autre thread, qui est ce qui se produit dans cet exemple.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

Pour plus d’informations, [Détails sur les destructeurs](/windows/uwp/cpp-and-winrt-apis/details-about-destructors).

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Prise en charge améliorée de l’héritage d’interface unique de style COM

Comme dans la programmation Windows Runtime, C++/WinRT est également utilisé pour créer et consommer des API uniquement COM. Cette mise à jour permet d’implémenter un serveur COM dans lequel il existe une hiérarchie d’interfaces. Elle n’est pas obligatoire pour Windows Runtime, mais elle l’est pour certaines implémentations COM.

#### <a name="correct-handling-of-out-params"></a>Gestion correcte des paramètres `out`

Il peut s’avérer difficile d’utiliser des paramètres `out`, notamment des tableaux Windows Runtime. Avec cette mise à jour, C++/WinRT est considérablement plus robuste et plus résistant aux erreurs lorsqu’il s’agit de paramètres et tableaux `out`, que ces paramètres arrivent par le biais d’une projection de langage ou par le biais d’un développeur COM qui utilise l’ABI brute et qui commet l’erreur de ne pas lancer des variables de manière cohérente. Dans les deux cas, C++/WinRT remet désormais correctement les types projetés à l’ABI (en n’oubliant pas de libérer les ressources), et réinitialise ou efface correctement les paramètres qui arrivent par l’intermédiaire de l’ABI.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Gestion désormais fiable des jetons non valides par les événements

L’implémentation [**winrt::event**](/uwp/cpp-ref-for-winrt/event) gère maintenant le cas où sa méthode **remove** est appelée avec une valeur de jeton non valide (une valeur qui n’est pas présente dans le tableau).

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>Destruction des variables locales de coroutine avant le retour de la coroutine

La façon traditionnelle d’implémenter un type de coroutine peut permettre à des variables locales au sein de la coroutine d’être détruites *après* le retour/la fin de la coroutine (plutôt qu’avant la suspension finale). La reprise d’objets waiter est désormais différée jusqu’à la suspension finale, afin d’éviter ce problème et de bénéficier d’autres avantages.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Actualités et modifications du SDK Windows version 10.0.17763.0 (Windows 10, version 1809)

Le tableau ci-dessous contient les nouveautés et les modifications concernant C++/WinRT dans le SDK Windows version 10.0.17763.0 (Windows 10, version 1809).

| Fonctionnalité nouvelle ou modifiée | Informations supplémentaires |
| - | - |
| **Changement cassant**. Pour permettre sa compilation, C++/WinRT ne dépend pas des en-têtes du SDK Windows. | Consultez [Isolation des fichiers d’en-tête du SDK Windows](#isolation-from-windows-sdk-header-files), ci-dessous. |
| Le format du système de projet Visual Studio a changé. | Consultez [Guide pratique pour recibler votre projet C++/WinRT vers une version ultérieure du SDK Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), ci-dessous. |
| Il existe de nouvelles fonctions et classes de base pour vous aider à passer un objet de collection à une fonction Windows Runtime ou pour implémenter vos propres propriétés et types de collection. | Consultez [Collections avec C++/WinRT](collections.md). |
| Vous pouvez utiliser l’extension de balisage [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) avec vos classes d’exécution C++/WinRT. | Pour plus d’informations et des exemples de code, consultez [Vue d’ensemble de la liaison de données](/windows/uwp/data-binding/data-binding-quickstart). |
| La prise en charge de l’annulation d’une coroutine vous permet d’inscrire un rappel d’annulation. | Pour plus d’informations et des exemples de code, consultez [Annulation d’une opération asynchrone et rappels d’annulation](concurrency-2.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Quand vous créez un délégué qui pointe vers une fonction membre, vous pouvez établir une référence forte ou faible à l’objet actuel (au lieu d’un pointeur *this* brut) au point où le gestionnaire est inscrit. | Pour plus d’informations et des exemples de code, consultez la sous-section **Si vous utilisez une fonction membre en tant que délégué** dans la section [Accès sécurisé au pointeur *this* avec un délégué de gestion des événements](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Des bogues découverts par la conformité améliorée de Visual Studio à la norme C++ sont corrigés. La chaîne d’outils LLVM et Clang est également mieux exploitée pour valider la conformité aux normes C++/WinRT. | Vous ne rencontrerez plus le problème décrit dans [Pourquoi la compilation de mon nouveau projet échoue-t-elle ? J’utilise Visual Studio 2017 (version 15.8.0 ou ultérieure) et le SDK version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Autres modifications.

- **Changement cassant**. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) retourne désormais `void*` au lieu de `HSTRING`. Vous pouvez utiliser `static_cast<HSTRING>(get_abi(my_hstring));` pour obtenir un HSTRING. Consultez [Interopération avec le HSTRING d’ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Changement cassant**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) retourne désormais `void**` au lieu de `HSTRING*`. Vous pouvez utiliser `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` pour obtenir un HSTRING*. Consultez [Interopération avec le HSTRING d’ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Changement cassant**. HRESULT est désormais projeté en tant que **winrt::hresult**. Si vous avez besoin d’un HRESULT (pour effectuer une vérification de type ou prendre en charge des caractéristiques de type), alors vous pouvez faire un `static_cast` sur un **winrt::hresult**. Sinon, **winrt::hresult** se convertit en HRESULT, tant que vous incluez `unknwn.h` avant d’inclure des en-têtes C++/WinRT.
- **Changement cassant**. GUID est désormais projeté en tant que **winrt::guid**. Pour les API que vous implémentez, vous devez utiliser **winrt::guid** pour les paramètres GUID. Sinon, **winrt::guid** se convertit en GUID, tant que vous incluez `unknwn.h` avant d’inclure des en-têtes C++/WinRT. Consultez [Interopération avec le struct GUID d’ABI](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct).
- **Changement cassant**. Le [**constructeur winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) a été renforcé en devenant plus explicite (il est désormais plus difficile d’écrire du code incorrect avec lui). Si vous avez besoin d’assigner une valeur de handle brute, appelez la [**fonction handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) à la place.
- **Changement cassant**. Les signatures de **WINRT_CanUnloadNow** et **WINRT_GetActivationFactory** ont changé. Vous ne devez pas du tout déclarer ces fonctions. Incluez plutôt `winrt/base.h` (qui est automatiquement inclus si vous incluez des fichiers d’en-tête d’espace de noms Windows C++/WinRT) pour inclure les déclarations de ces fonctions.
- Pour le [**struct winrt::clock**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** sont dépréciés en faveur de **from_file_time/to_file_time**.
- Les API qui attendent des paramètres **IBuffer** sont simplifiées. Bien que la plupart des API préfèrent les collections ou les tableaux, il en existe suffisamment qui s’appuient sur **IBuffer** pour qu’il soit nécessaire de simplifier leur utilisation à partir de C++. Cette mise à jour offre un accès direct aux données situées derrière une implémentation **IBuffer**, en utilisant la même convention de nommage de données que celle utilisée par les conteneurs des bibliothèques standard C++. Les conflits avec les noms de métadonnées qui commencent traditionnellement par une lettre majuscule sont ainsi évités.
- Génération de code améliorée : diverses améliorations permettent de réduire la taille du code, d’améliorer l’incorporation (inlining) et d’optimiser la mise en cache de la fabrique.
- Récursivité inutile supprimée. Quand la ligne de commande fait référence à un dossier, plutôt qu’à un `.winmd` spécifique, l’outil `cppwinrt.exe` ne recherche plus des fichiers `.winmd` de manière récursive. L’outil `cppwinrt.exe` gère à présent aussi les doublons plus intelligemment, en renforçant leur résilience face aux erreurs des utilisateurs et aux fichiers `.winmd` mal formés.
- Pointeurs intelligents renforcés. Auparavant, les révocateurs d’événements échouaient quand une nouvelle valeur était déplacée ou affectée. Cela a permis de découvrir un problème où les classes de pointeur intelligent ne géraient pas de façon fiable l’auto-affectation ; rootée dans le [**modèle de struct winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). **winrt::com_ptr** a été corrigé, ainsi que les révocateurs d’événements, pour gérer la sémantique de déplacement correctement afin que la révocation s’effectue au moment de l’affectation.

> [!IMPORTANT]
> Des modifications importantes ont été apportées à la l’[extension Visual Studio C++/WinRT (VSIX)](https://aka.ms/cppwinrt/vsix), à la fois dans la version 1.0.181002.2, puis plus tard dans la version 1.0.190128.4. Pour plus d’informations sur ces modifications et la manière dont elles affectent vos projets existants, consultez [Prise en charge Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) et [Versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Isolation des fichiers d’en-tête du SDK Windows

Il s’agit d’un changement cassant potentiel pour votre code.

Pour permettre sa compilation, C++/WinRT ne dépend plus des fichiers d’en-tête du SDK Windows. Les fichiers d’en-tête de la bibliothèque du runtime C (CRT) et la bibliothèque des modèles standard C++ (STL) n’incluent pas non plus d’en-têtes du SDK Windows. Cela permet d’améliorer la conformité aux normes, d’éviter les dépendances par inadvertance et de réduire considérablement le nombre de macros contre lesquelles se protéger.

Cette indépendance signifie que C++/WinRT est désormais plus portable et conforme aux normes, ce qui améliore son potentiel de devenir une bibliothèque multicompilateur et multiplateforme. Cela signifie également que les en-têtes C++/WinRT ne sont pas des macros affectées avec un effet défavorable.

Si vous laissiez auparavant le soin à C++/WinRT d’inclure des en-têtes Windows dans votre projet, vous devez maintenant les inclure vous-même. Il est, dans tous les cas, toujours préférable d’inclure explicitement les en-têtes dont vous dépendez et de ne pas laisser le soin à une autre bibliothèque de les inclure pour vous.

Actuellement, les seules exceptions à l’isolation des fichiers d’en-tête du SDK Windows concernent les intrinsèques et les numériques. Il n’existe aucun problème connu avec ces dernières dépendances restantes.

Dans votre projet, vous pouvez réactiver l’interopérabilité avec les en-têtes du SDK Windows si nécessaire. Vous pouvez, par exemple, implémenter une interface COM (rootée dans [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)). Pour cet exemple, incluez `unknwn.h` avant d’inclure des en-têtes C++/WinRT. Ainsi, la bibliothèque de base C++/WinRT permet à divers hooks de prendre en charge des interfaces COM classiques. Pour obtenir un exemple de code, consultez [Créer des composants COM avec C++/WinRT](author-coclasses.md). De même, incluez explicitement tous les autres en-têtes du SDK Windows qui déclarent des types et/ou des fonctions que vous voulez appeler.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Guide pratique pour recibler votre projet C++/WinRT vers une version ultérieure du SDK Windows

La méthode qui permet de recibler votre projet susceptible d’entraîner le moins de problèmes de compilateur et d’éditeur de liens est également la plus fastidieuse. Cette méthode implique de créer un projet (ciblant la version du SDK Windows de votre choix), puis de copier des fichiers vers votre nouveau projet à partir de l’ancien. Vous pouvez tout simplement copier des sections de vos anciens fichiers `.vcxproj` et `.vcxproj.filters` pour éviter d’ajouter des fichiers dans Visual Studio.

Toutefois, il existe deux autres façons de recibler votre projet dans Visual Studio.

- Accédez à la propriété de projet **Général** \> **Version du SDK Windows**, puis sélectionnez **Toutes les configurations** et **Toutes les plateformes**. Définissez **Version du SDK Windows** sur la version à cibler.
- Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud du projet, cliquez sur **Recibler les projets**, choisissez les versions à cibler, puis cliquez sur **OK**.

Si vous rencontrez des erreurs de compilateur ou d’éditeur de liens après avoir utilisé l’une de ces deux méthodes, vous pouvez essayer de nettoyer la solution (**Build** > **Nettoyer la solution** et/ou supprimez manuellement tous les fichiers et dossiers temporaires) avant de réessayer la génération.

Si le compilateur C++ génère l’« *erreur C2039 : IUnknown : n’est pas membre de l’\`espace de noms global* », alors ajoutez `#include <unknwn.h>` au début de votre fichier `pch.h` (avant d’inclure des en-têtes C++/WinRT).

Vous devrez peut-être également ajouter `#include <hstring.h>` après.

Si l’éditeur de liens C++ génère l’« *erreur LNK2019 : symbole externe non résolu _WINRT_CanUnloadNow@0 référencé dans la fonction _VSDesignerCanUnloadNow@0* », alors vous pouvez résoudre ce problème en ajoutant `#define _VSDESIGNER_DONT_LOAD_AS_DLL` à votre fichier `pch.h`.
