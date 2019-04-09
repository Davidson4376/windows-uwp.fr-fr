---
description: Nouveautés et modifications apportées à C++/WinRT.
title: Quelles sont les nouveautés en C / c++ / WinRT
ms.date: 04/02/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c ++, cpp, winrt, projection, actualités, ce qui de, les nouveaux
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8ee10450a7a346c1ae032240aaecc65e7f87822d
ms.sourcegitcommit: 940645c705865ba9635ccae2da9d917420faf608
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58812608"
---
# <a name="whats-new-in-cwinrt"></a>Quelles sont les nouveautés en C / c++ / WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>Actualités et les changements dans C++WinRT 2.0

Pour plus d’informations sur la [ C++Extension WinRT Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix), le [Microsoft.Windows.CppWinRT NuGet package](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)et le `cppwinrt.exe` outil&mdash;, y compris comment acquérir et installez-les&mdash;consultez [prise en charge de Visual Studio pour C++/WinRT, XAML, l’extension VSIX et le package NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Modifications apportées à la C++WinRT Extension Visual Studio (VSIX) pour la version 2.0

- Le visualiseur de débogage prend désormais en charge Visual Studio 2019 ; ainsi que de continuer à prendre en charge de Visual Studio 2017.
- Nombreux correctifs de bogues ont été apportées.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Modifications apportées au package NuGet de Microsoft.Windows.CppWinRT pour la version 2.0

- Le `cppwinrt.exe` outil est désormais inclus dans le package NuGet de Microsoft.Windows.CppWinRT, et l’outil génère des en-têtes de projection de plateforme pour chaque projet à la demande. Par conséquent, le `cppwinrt.exe` outil ne dépend plus le SDK Windows (même si l’outil est toujours fournis avec le Kit de développement logiciel pour des raisons de compatibilité).
- `cppwinrt.exe` génère maintenant des en-têtes de projection sous chaque dossier intermédiaire spécifique à la plateforme/configuration ($IntDir) pour activer les builds parallèles.
- Le C++/WinRT build prise en charge (propriétés/cibles) est maintenant entièrement documentée, au cas où vous souhaiteriez personnaliser manuellement vos fichiers projet. Consultez [Microsoft.Windows.CppWinRT NuGet Package](https://github.com/Microsoft/xlang/tree/user/sjones/cppwinrt_nuget/src/package/nuget).
- Nombreux correctifs de bogues ont été apportées.

### <a name="changes-to-cwinrt-for-version-20"></a>Modifications apportées aux C++/WinRT pour la version 2.0

#### <a name="open-source"></a>Open source

Le `cppwinrt.exe` outil prend une métadonnées Windows Runtime (`.winmd`) de fichiers et génère, à partir de celui-ci, une norme en-tête basés C++ bibliothèque qui *projets* les API décrites dans les métadonnées. De cette façon, vous pouvez utiliser ces API à partir de votre C++/WinRT code.

Cet outil est désormais un projet d’entièrement open source, disponible sur GitHub. Visitez [Microsoft\/xlang](https://github.com/Microsoft/xlang), puis cliquez sur dans à **src** > **outil** > **cppwinrt**.

#### <a name="xlang-libraries"></a>bibliothèques de XLANG

Une bibliothèque à en-tête uniquement entièrement portable (pour l’analyse le format de métadonnées ECMA-335 utilisé par le Runtime Windows) constitue la base de tous les Windows Runtime et outils à l’avenir de xlang. En particulier, nous avons également réécrit le `cppwinrt.exe` outil à partir de zéro en utilisant les bibliothèques xlang. Cela fournit des requêtes de métadonnées beaucoup plus précises, résolution des problèmes de longue date quelques avec le C++projection de langage /WinRT.

#### <a name="fewer-dependencies"></a>Moins de dépendances

En raison du lecteur de métadonnées xlang, les `cppwinrt.exe` outil lui-même a moins de dépendances. Il est ainsi beaucoup plus souple, ainsi que d’être utilisée dans d’autres scénarios&mdash;en particulier dans la contrainte, créer des environnements. En particulier, il ne dépend plus `RoMetadata.dll`.
 
Il s’agit des dépendances pour `cppwinrt.exe` 2.0.
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite.dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

Par opposition à ces dépendances, lequel `cppwinrt.exe` 1.0 a.

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

#### <a name="the-windows-runtime-noexcept-attribute"></a>Le Runtime Windows `noexcept` attribut

Le Runtime Windows dispose d’un nouveau `[noexcept]` attribut, vous pouvez utiliser pour décorer vos méthodes et les propriétés dans [MIDL 3.0](/uwp/midl-3/predefined-attributes). La présence de l’attribut indique à la prise en charge des outils que votre implémentation ne lève une exception (ni retourner un échec HRESULT). Ainsi, les projections de langage optimiser la génération de code en évitant la surcharge de gestion des exceptions qui est requis pour prendre en charge les appels de binaire ABI (interface) d’application qui peuvent potentiellement être défaillants.

C++/ WinRT tire parti de cette en produisant C++ `noexcept` implémentations de la consommation et de création de code. Si vous avez des méthodes de l’API ou propriétés qui sont sans échec et vous êtes préoccupées par la taille du code, vous pouvez rechercher cet attribut.

#### <a name="optimized-code-generation"></a>Génération de code optimisée

C++/ WinRT génère désormais encore plus efficace C++ source code (en arrière-plan) pour que le C++ compilateur peut produire le plus petit et plus efficace code binaire possible. De nombreuses améliorations sont destinées à réduire les coûts de gestion des exceptions en évitant inutiles des informations de déroulement. Les fichiers binaires qui utilisent de grandes quantités de C++/code de WinRT verrez environ une réduction de 4 % de la taille du code. Le code est également plus efficace (elle s’exécute plus rapidement) en raison du nombre d’instructions réduites.

Ces améliorations s’appuient sur une nouvelle fonctionnalité d’interopérabilité qui est également disponible pour vous. Tous les C++/types WinRT sont désormais des propriétaires de ressources, notamment un constructeur pour en prendre possession directement, en évitant l’approche en deux étapes précédentes.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Gestion des exceptions (EH) code-génération optimisée

Cette modification complète le travail effectué par Microsoft C++ équipe optimiseur afin de réduire le coût de gestion des exceptions. Si vous utilisez application binary interface (ABI) (par exemple, COM) lourdement dans votre code, vous pouvez observer un grand nombre de code qui suit ce modèle.

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

C++/ WinRT lui-même génère ce modèle pour toutes les API qui est implémentée. Avec des milliers de fonctions d’API, n’importe quel optimisation ici peut être significative. Dans le passé, l’optimiseur n’aurait pas détecter que les blocs catch sont toutes identiques, afin qu’il a été dupliquer beaucoup de code autour de chaque ABI (qui à son tour contribué à la croyance que l’utilisation d’exceptions dans le code système génère des binaires volumineux). Toutefois, à partir de Visual Studio 2019, le C++ compilateur insère toutes celles catch funclets et stocke uniquement ceux qui sont uniques. Il en résulte une réduction de 18 % supplémentaire et global de la taille du code pour les fichiers binaires qui s’appuient sur ce modèle. Ce n’est pas seulement code EH désormais plus efficace que l’utilisation de codes de retour, mais également le problème sur les fichiers binaires volumineux est désormais un partie du passé.

#### <a name="incremental-build-improvements"></a>Améliorations de la génération incrémentielle

Le `cppwinrt.exe` outil compare désormais la sortie d’un fichier d’en-tête/source généré sur le contenu de tout fichier existant sur le disque, et il n’écrit le fichier si le fichier a été modifié en fait. Cette opération enregistre beaucoup de temps avec des e/s disque, et elle garantit que les fichiers ne sont pas considérés comme « modifiées » par le C++ compilateur. Le résultat est que la recompilation est évitée ou réduite, dans de nombreux cas.

#### <a name="generic-interfaces-are-now-all-generated"></a>Interfaces génériques sont maintenant tous générés

En raison du lecteur de métadonnées xlang, C++/WinRT génère désormais toutes les interfaces paramétrables ou génériques, à partir des métadonnées. Interfaces telles que [Windows::Foundation::Collections::IVector\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_) sont désormais générés à partir des métadonnées non écrites `winrt/base.h`. Le résultat est que la taille de `winrt/base.h` a été réduit de moitié, et que les optimisations sont générées avec le bouton droit dans le code (qui était difficile à faire avec l’approche manuel).

> [!IMPORTANT]
> Interfaces telles que l’exemple donné apparaissent désormais dans leurs en-têtes d’espace de noms respectifs, plutôt que dans `winrt/base.h`. Par conséquent, si vous ne le n'avez pas déjà fait, vous devrez inclure l’en-tête de l’espace de noms approprié afin d’utiliser l’interface.

#### <a name="component-optimizations"></a>Optimisations de composant

Cette mise à jour ajoute la prise en charge de plusieurs participer des optimisations supplémentaires pour C++/WinRT, décrites dans les sections ci-dessous. Étant donné que ces optimisations sont apportées aux (ce qui vous devrez peut-être apporter des modifications mineures pour prendre en charge), vous devez les activer explicitement à l’aide de la `cppwinrt.exe` l’outil `-opt` indicateur.

Un nouveau projet (à partir d’un modèle de projet) utilisera `-opt` par défaut.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Construction et l’accès de l’implémentation directe de largeurs uniformes

Ces deux optimisations autoriser votre composant un accès direct à ses propres types d’implémentation, même quand il utilise uniquement des types projetés. Il est inutile d’utiliser [ **rendre**](/uwp/cpp-ref-for-winrt/make), [ **make_self**](/uwp/cpp-ref-for-winrt/make-self), ni [ **get_self** ](/uwp/cpp-ref-for-winrt/get-self) si vous souhaitez simplement utiliser la surface d’API publique. Vos appels compilera jusqu'à l’implémentation des appels directs, et ceux peuvent même être entièrement inline.

##### <a name="type-erased-factories"></a>Type-erased les fabriques

Cette optimisation évite le #include les dépendances dans `module.g.cpp` afin qu’il ne doive pas recompilé chaque fois que n’importe quelle classe de mise en œuvre unique qui se produit à modifier. Il en résulte des performances de génération améliorée.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>Plus intelligemment et plus efficace `module.g.cpp` pour les grands projets avec plusieurs bibliothèques

Le `module.g.cpp` fichier maintenant contient également deux autres composable « Helpers », nommé **winrt_can_unload_now**, et **winrt_get_activation_factory**. Ils ont été conçus pour les grands projets où une DLL est composée d’un nombre de bibliothèques, chacun avec ses propres classes de runtime. Dans ce cas, vous devez assembler manuellement la DLL **DllGetActivationFactory** et **DllCanUnloadNow**. Ces programmes d’assistance rendent beaucoup plus facile pour vous pour ce faire, en évitant les erreurs fausses d’origine. Le `cppwinrt.exe` l’outil `-lib` indicateur peut également être utilisé pour donner à chaque lib individuel son propre Préambule (plutôt que `winrt_xxx`) afin que les fonctions de chaque lib peuvent être nommées individuellement et par conséquent combinées sans ambiguïté.

#### <a name="new-winrtcoroutineh-header"></a>Nouvelle `winrt/coroutine.h` en-tête

Le `winrt/coroutine.h` en-tête est la nouvelle maison pour tous les C++prise en charge de la coroutine de /WinRT. Auparavant, cette prise en charge se trouvait à plusieurs endroits, ce qui nous avons pensé était trop limitée. Étant donné que les interfaces asynchrones Windows Runtime sont maintenant générés, au lieu d’écrit manuellement, ils se trouvent désormais dans `winrt/Windows.Foundation.h`. En plus d’être plus facile à gérer et plus facilement prises en charge, cela signifie que cette coroutine helpers comme [ **resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) n’avez plus à être ajouté à la fin d’un en-tête de l’espace de noms spécifique. Au lieu de cela, elles peuvent inclure plus naturellement leurs dépendances. Cela permet davantage **resume_foreground** pour prendre en charge non seulement la reprise sur une donnée [ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher), mais il peut désormais également prendre en charge la reprise sur un donné [ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Auparavant, seul peut être pris en charge ; mais pas les deux, dans la mesure où la définition ne peut résider que dans un espace de noms.

Voici un exemple de la **DispatcherQueue** prennent en charge.

```cppwinrt
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

Les programmes d’assistance coroutine sont désormais également affublés `[[nodiscard]]`, ce qui permet leur convivialité. Si vous oubliez de (ou ne réalisent pas avoir à) `co_await` leur pour qu’elle fonctionne, puis, en raison `[[nodiscard]]`, ces erreurs produisent maintenant un avertissement du compilateur.

#### <a name="help-with-diagnosing-stack-allocations"></a>Faciliter le diagnostic des allocations de la pile

Étant donné que les noms de classe prévues et d’implémentation sont (par défaut), les mêmes et ne diffèrent que par l’espace de noms, il est possible de confondre un à l’autre et de créer accidentellement une implémentation sur la pile, plutôt que d’utiliser le [ **rendre** ](/uwp/cpp-ref-for-winrt/make) famille de programmes d’assistance. Cela peut être difficile de diagnostiquer dans certains cas, étant donné que l’objet peut être détruit alors que les références en suspens sont toujours en cours. Une assertion maintenant récupère ce, pour les versions debug. Tandis que l’assertion ne détecte pas l’allocation de pile à l’intérieur d’une coroutine, il est néanmoins utile pour intercepter la plupart des erreurs de ce type.

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Programmes d’assistance capture améliorée et des délégués de variadiques

Cette mise à jour corrige la limitation avec les programmes d’assistance capture en prenant en charge également des types projetés. Cela s’affiche maintenant, puis avec les API d’interopérabilité de Windows Runtime, lorsqu’ils retournent un type projeté.

Cette mise à jour ajoute également la prise en charge de [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) et [ **get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) lors de la création d’un délégué variadic (non - Windows Runtime).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Prise en charge de destruction différée et QI sans échec au cours de destruction

Une application XAML peut passer lui-même à des difficultés en raison de son besoin d’effectuer une [ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) dans un destructeur, afin d’appeler une implémentation de nettoyage vers le haut ou vers le bas de la hiérarchie. Toutefois, qu’appel implique un QI après que le décompte de références de l’objet a déjà atteint la valeur zéro. Cette mise à jour ajoute la prise en charge de debouncing le décompte de références, veiller à ce que lorsqu’il atteint zéro, il ne peut jamais être réactivé ; tout en continuant à QI temporaires qui est nécessaire au cours de destruction. Cette procédure est inévitable dans certaines applications XAML/contrôles, et C++/WinRT est désormais résilient à ce dernier.

Destruction peut être différée en fournissant un statique **final_release** de fonction, et le déplacement de la propriété de la **unique_ptr** à un autre contexte.

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

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

Dans l’exemple ci-dessous, une fois le **MainPage** est publié (pour la dernière fois), **final_release** est appelée. Que la fonction consacre à cinq secondes en attente (sur le pool de threads), et puis elle reprend à l’aide de la page **répartiteur** (qui nécessite QI/AddRef/Release travailler). Il efface ensuite le **unique_ptr**, ce qui conduit le **MainPage** destructeur réellement appelée. Même dans ce cas, **DataContext** est appelée, ce qui nécessite un QI pour **IFrameworkElement**. Bien sûr, vous n’êtes pas obligé d’implémenter votre **final_release** comme une coroutine. Mais qui ne fonctionne pas et il est très simple déplacer la destruction vers un autre thread.

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Prise en charge améliorée pour l’héritage de style COM interface unique

Ainsi que pour la programmation Windows Runtime, C++/WinRT est également utilisé pour créer et consommer des API COM uniquement. Cette mise à jour permet d’implémenter un serveur COM dans lequel il existe une hiérarchie d’interface. Ce n’est pas requis pour l’exécution de Windows ; mais il est requis pour certaines implémentations COM.

#### <a name="correct-handling-of-out-params"></a>Correct de gestion des `out` params

Il peut être difficile de travailler avec `out` params ; en particulier les tableaux Windows Runtime. Avec cette mise à jour, C++/WinRT est considérablement plus robuste et plus résistant aux erreurs lorsqu’il s’agit de `out` params et tableaux ; si ces paramètres arrivent via une projection de langage, ou à partir d’un développeur COM qui utilise l’ABI brutes, et qui effectue Erreur d’initialisation ne pas variables de manière cohérente. Dans les deux cas, C++/WinRT prend désormais la bonne décision lorsqu’il s’agit de remise de types projetés à l’ABI (par la mémorisation de libérer les ressources), et lorsqu’il s’agit de réinitialisation ou d’effacement des paramètres qui arrivent travers l’ABI.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Événements désormais gérer des jetons non valides de façon fiable

Le [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) implémentation maintenant gère normalement le cas où sa **supprimer** méthode est appelée avec une valeur de jeton non valide (une valeur qui n’est pas présente dans le tableau).

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>Variables locales de coroutine sont détruits désormais avant le retour de la coroutine

La façon traditionnelle de l’implémentation d’un type de la coroutine peut permettre de variables locales au sein de la coroutine le point d’être détruit *après* la coroutine retourne/se termine (au lieu d’avant la suspension final). La reprise de n’importe quel waiter est désormais différée jusqu'à ce que la dernière suspension, afin d’éviter ce problème et à accumuler des autres avantages.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Actualités et les modifications, dans la version 10.0.17763.0 du SDK de Windows (Windows 10, version 1809)

Le tableau ci-dessous contient des informations et les modifications apportées aux [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) dans la dernière version la disposition générale du SDK Windows, qui est 10.0.17763.0 (Windows 10, version 1809). Ces modifications peuvent également être présentes dans les versions ultérieures de kit de développement logiciel Insider Preview.

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
- **Modification avec rupture**. GUID est désormais projeté en tant que **winrt::guid**. Pour les API que vous implémentez, vous devez utiliser **winrt::guid** pour les paramètres GUID. Sinon, **winrt::guid** convertit en GUID, tant que vous incluez `unknwn.h` avant d’inclure les C++/WinRT en-têtes.
- **Modification avec rupture**. Le [ **winrt::handle_type constructeur** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) a été renforcé en le rendant explicite (il est désormais plus difficile d’écrire du code incorrect avec lui). Si vous avez besoin d’assigner une valeur de handle brut, appelez le [ **handle_type::attach fonction** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) à la place.
- **Modification avec rupture**. Les signatures de **WINRT_CanUnloadNow** et **WINRT_GetActivationFactory** ont été modifiés. Vous ne doit pas déclarer ces fonctions du tout. Au lieu de cela, incluez `winrt/base.h` (qui est automatiquement inclus si vous incluez tout C + c++ / fichiers d’en-tête espace de noms WinRT Windows) pour inclure les déclarations de ces fonctions.
- Pour le [ **winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** sont déconseillés en faveur du **from_file_time/to_file_time**.
- API qui attendent **IBuffer** paramètres sont simplifiées. Bien que la plupart des API préférez collections ou les tableaux, suffisamment API s’appuient sur **IBuffer** qu’il devait être plus facile à utiliser ces API à partir de C++. Cette mise à jour offre un accès direct aux données derrière un **IBuffer** implémentation, à l’aide de la même convention d’affectation de noms de données utilisée par les conteneurs de bibliothèque C++ Standard. Cela vous évite également de conflit avec les noms de métadonnées qui traditionnellement commencent par une lettre majuscule.
- Amélioration de la génération de code : diverses améliorations afin de réduire la taille du code, améliorer incorporation (inlining) et d’optimiser la mise en cache de fabrique.
- Supprimer la récursivité inutile. Lorsque la ligne de commande fait référence à un dossier, plutôt qu’à un spécifique `.winmd`, le `cppwinrt.exe` outil ne recherche plus de manière récursive pour `.winmd` fichiers. Le `cppwinrt.exe` outil également gère désormais les doublons plus intelligemment, rendant plus résistants aux erreurs des utilisateurs et à mal formée `.winmd` fichiers.
- Pointeurs intelligents renforcés. Auparavant, les revokers événement n’a pas pu révoquer lorsque move-attribuer une nouvelle valeur. Cela a permis de découvrir un problème où les classes de pointeur intelligent n’étaient pas gérer de façon fiable auto-affectation ; enraciné dans le [ **winrt::com_ptr struct modèle**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** a été résolu, et les revokers événement fixés pour gérer la sémantique de déplacement correctement afin qu’ils révoquent lors de l’affectation.

> [!IMPORTANT]
> Modifications importantes ont été apportées à la [C++ / c++ / WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix), à la fois dans la version 1.0.181002.2 et ensuite dans la version 1.0.190128.4. Pour plus d’informations de ces modifications, et comment elles affectent vos projets existants, [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) et [les versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Isolation des fichiers d’en-tête Windows SDK

Il s’agit potentiellement d’une modification avec rupture pour votre code.

Pour pouvoir compiler, c++ / WinRT ne dépend plus des fichiers d’en-tête à partir du SDK Windows. Fichiers d’en-tête dans la bibliothèque d’exécution C (CRT) et le C++ bibliothèque STL (Standard Template) n’également incluent des en-têtes Windows SDK. Et qui améliore la conformité aux normes, évite toute dépendance par inadvertance et réduit considérablement le nombre de macros que vous avez pour vous prémunir contre.

Cette indépendance signifie que C++ / c++ / WinRT est désormais plus portable et conformes aux normes, et il améliore la possibilité qu’elles deviennent une bibliothèque de compilateur croisé et multiplateforme. Cela signifie également que le C++ / c++ / WinRT en-têtes ne sont pas affectés négativement macros.

Si vous avez précédemment le laissé à C++ / c++ / WinRT pour inclure tous les en-têtes Windows dans votre projet, vous devez maintenant pour les inclure. Il est, dans tous les cas, toujours meilleure pratique pour inclure explicitement les en-têtes dont vous dépendez et pas le laisser à une autre bibliothèque pour les inclure.

Actuellement, les seules exceptions à l’isolation de fichier d’en-tête Windows SDK concernent les fonctions intrinsèques et les valeurs numériques. Il n’existe aucun problème connu avec ces dépendances de la dernière.

Dans votre projet, vous pouvez réactiver l’interopérabilité avec les en-têtes Windows SDK si vous avez besoin. Peut, par exemple, voulez-vous implémenter une interface COM (enraciné dans [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Pour cet exemple, inclure `unknwn.h` avant d’inclure n’importe quel C + c++ / WinRT en-têtes. Ainsi, le C + c++ / WinRT les bibliothèque de base pour activer des hooks prendre en charge les interfaces du COM classique. Pour obtenir un exemple de code, consultez [composants COM de l’auteur avec C / c++ / WinRT](author-coclasses.md). De même, incluez explicitement tous les autres en-têtes Windows SDK qui déclarent des types et/ou les fonctions que vous souhaitez appeler.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment recibler votre C + c++ / projet WinRT vers une version ultérieure du SDK Windows

La méthode recibler votre projet est susceptible d’entraîner le problème de compilateur et éditeur de liens moins est également plus fastidieuse. Cette méthode implique la création d’un projet (en ciblant la version du Kit de développement logiciel Windows de votre choix), puis copier les fichiers sur votre nouveau projet à partir de votre ancien. Il y aura des sections de votre ancienne `.vcxproj` et `.vcxproj.filters` les fichiers que vous pouvez simplement copier plus à vous faire économiser de l’ajout de fichiers dans Visual Studio.

Toutefois, il existe deux autres façons de recibler votre projet dans Visual Studio.

- Accédez à la propriété de projet **général** \> **Windows SDK Version**, puis sélectionnez **toutes les Configurations** et **toutes les plateformes**. Définissez **Windows SDK Version** vers la version que vous souhaitez cibler.
- Dans **l’Explorateur de solutions**, cliquez sur le nœud de projet, cliquez sur **recibler les projets**, choisissez l’ou les versions à cibler, puis cliquez sur **OK**.

Si vous rencontrez n’importe quel compilateur ou des erreurs de l’éditeur de liens après avoir utilisé une de ces deux méthodes, vous pouvez essayer la solution de nettoyage (**Build** > **nettoyer la Solution** et/ou supprimez manuellement tous les fichiers et dossiers temporaires) avant d’essayer de générer une nouvelle fois.

Si le compilateur C++ génère «*erreur C2039 : « IUnknown » : n’est pas un membre de '\`espace de noms global''*», puis ajoutez `#include <unknwn.h>` vers le haut de votre `pch.h` fichier (avant d’inclure n’importe quel C + c++ / WinRT en-têtes).

Vous devrez peut-être également ajouter `#include <hstring.h>` après cela.

Si l’éditeur de liens C++ produit «*erreur LNK2019 : symbole externe non résolu _WINRT_CanUnloadNow@0 référencé dans la fonction _VSDesignerCanUnloadNow@0* », vous pouvez alors résoudre qui en ajoutant `#define _VSDESIGNER_DONT_LOAD_AS_DLL` à votre `pch.h` fichier.
