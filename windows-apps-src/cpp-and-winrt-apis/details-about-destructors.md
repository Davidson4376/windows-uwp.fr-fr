---
description: C++/WinRT 2.0 vous permet de différer la destruction de vos types implémentation et de créer des requêtes en toute sécurité pendant la destruction. Cette rubrique décrit ces fonctionnalités et explique quand les utiliser.
title: Détails sur les destructeurs
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, destruction différée, requêtes sécurisées
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387796"
---
# <a name="details-about-destructors"></a>Détails sur les destructeurs

C++/WinRT 2.0 vous permet de différer la destruction de vos types implémentation et de créer des requêtes en toute sécurité pendant la destruction. Cette rubrique décrit ces fonctionnalités et explique quand les utiliser.

## <a name="deferred-destruction"></a>Destruction différée

Dans la rubrique [Diagnostic des allocations directes](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc), nous avons souligné le fait que votre type d’implémentation ne peut avoir de destructeur privé.

Un destructeur public présente l'avantage de permettre une destruction différée, à savoir la possibilité de détecter le dernier appel [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) sur votre objet, puis de s'approprier cet objet pour différer indéfiniment sa destruction.

Rappelez-vous que les objets COM classiques sont soumis à un décompte intrinsèque des références ; ce décompte de références est géré via les fonctions [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) et **IUnknown::Release**. Dans une implémentation traditionnelle de **Release**, le destructeur C++ de l'objet COM classique est appelé lorsque le décompte de références atteint 0.

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` appelle le destructeur de l’objet avant de libérer la mémoire occupée par l’objet. Cela fonctionne plutôt bien , sous réserve que n’ayez rien à faire d'intéressant dans votre destructeur.

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

Qu'entendez-vous par *intéressant* ? En fait, un destructeur est intrinsèquement synchrone. Vous ne pouvez pas basculer de threads&mdash; pour détruire des ressources spécifiques aux threads dans un contexte différent. Vous ne pouvez pas interroger avec fiabilité l’objet pour une autre interface dont vous pourriez avoir besoin afin de libérer certaines ressources. Et ce n'est pas tout. Dans le cas d'une destruction non triviale, il vous faut une solution plus flexible. C'est là qu'intervient la fonction **final_release** de C++/WinRT.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

Nous avons mis à jour l'implémentation C++/WinRT de **Release** pour appeler votre **final_release** au moment où le décompte des références de votre objet atteint 0. L’objet sait ainsi qu'il n'existe pas d'autres références en suspens et qu'il détient sa propre propriété. Dès lors, il peut transférer sa propre propriété à la fonction **final_release** statique.

En d’autres termes, l’objet a opéré sa propre transformation, d’un objet prenant en charge une propriété partagée en objet détenant sa propre propriété. **std::unique_ptr** détient la propriété exclusive de l’objet, et détruira naturellement cet objet dans le cadre de sa sémantique&mdash;, d'où le besoin d'un destructeur public&mdash;lorsque **STD::unique_ptr** est hors de portée (sous réserve de ne pas être, au préalable, déplacé ailleurs). C’est la clé. Vous pouvez utiliser l’objet indéfiniment, à condition que **std::unique_ptr** le maintienne actif. Voici une illustration de la façon dont vous pouvez déplacer l’objet ailleurs.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

Vous pouvez considérer qu'il s'agit d'un récupérateur de mémoire déterministe. De manière plus concrète et plus performante, vous pouvez transformer la fonction **final_release** en coroutine et gérer son éventuelle destruction à un même emplacement, tout en conservant la possibilité de suspendre et de basculer les threads, si besoin.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

Une suspension pointera les causes du thread appelant&mdash;qui a initié l'appel à la **fonction** IUnknown::Release&mdash; à retourner, et donc à signaler à l'appelant que l’objet qu’il conservait n'est plus disponible via ce pointeur d’interface. Les infrastructures d’interface utilisateur doivent souvent s’assurer que les objets sont détruits sur le thread d’interface utilisateur spécifique qui a créé l'objet. Cette fonctionnalité rend la satisfaction de cette exigence triviale, car la destruction est séparée de la libération de l’objet.

## <a name="safe-queries-during-destruction"></a>Requêtes sécurisées lors de la destruction

En s'appuyant sur la notion de destruction différée, il est possible d'interroger en toute sécurité les interfaces lors de la destruction.

COM classique repose sur deux concepts centraux. Le premier correspond au décompte des références, et le second à l'interrogation des interfaces. En plus de **AddRef** et **Release**, l'interface **IUnknown** fournit [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void). Cette méthode est particulièrement sollicitée par certaines infrastructures d'interface utilisateur&mdash;telles que XAML, pour parcourir la hiérarchie XAML lorsqu’elle simule son système de type composable. Voici un exemple simple.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

Cela peut *paraître* anodin. Cette page XAML entend effacer son contexte de données dans son destructeur. Mais [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) est une propriété de la classe de base **FrameworkElement** et réside dans l'interface **IFrameworkElement** distincte. Dès lors, C++/WinRT doit injecter un appel dans **QueryInterface** pour rechercher la vtable qui convient avant de pouvoir appeler la propriété **DataContext**. Mais le fait que le décompte des références ait atteint 0 explique notre présence dans le destructeur. L'appel de **QueryInterface** perturbe temporairement le décompte des références et, lorsque celui-ci atteint à nouveau 0, l’objet est une nouvelle fois détruit.

C++/WinRT 2.0 a été renforcé pour prendre en charge cela. Voici l'implémentation C++/WinRT 2.0 de Release, sous une forme simplifiée.

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

Comme vous l'aviez peut-être envisagé, il décrémente d’abord le décompte des références et n'intervient qu'en l'absence de références en suspens. Toutefois, avant d’appeler la fonction **final_release** décrite précédemment dans cette rubrique, il stabilise le décompte des références en le définissant sur 1. Nous appelons cela *l'anti-rebond* (un terme emprunté au domaine électrique). C'est essentiel pour empêcher la libération de la dernière référence. Si cela venait à se produire, le décompte de références serait instable et incapable de prendre efficacement en charge un appel à **QueryInterface**.

Appeler **QueryInterface** est dangereux une fois la dernière référence libérée, car le décompte des références est alors susceptible d'augmenter indéfiniment. Il vous incombe d’appeler uniquement des chemins de code connus qui ne prolongeront pas la vie de l’objet. C++/WinRT vous y aide en veillant à ce que ces appels **QueryInterface** s'effectuent de manière fiable.

Pour ce faire, il stabilise le décompte des références. Une fois la dernière référence libérée, le décompte des références réel affiche 0 ou une valeur totalement imprévisible. Ce dernier cas peut se produire en présence de références faibles. Une telle situation n’est pas viable si un nouvel appel à **QueryInterface** intervient car cela provoque nécessairement une augmentation temporaire du décompte des références&mdash; d'où la référence à l'anti-rebond. Définir le décompte sur 1 permet de s'assurer qu'un dernier appel de **Release** n'intervienne jamais sur cet objet. Et c'est précisément ce que nous recherchons, puisque **std::unique_ptr** détient désormais l'objet, et que les appels limités aux paires **QueryInterface**/**Release** seront sécurisés.

Prenons un exemple plus intéressant encore.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

Tout d’abord, la fonction **final_release** est appelée, ce qui informe l'implémentation que le moment est venu de procéder au nettoyage. Ici, **final_release** relève d'une coroutine. Pour simuler un premier point de suspension, il commence par attendre le pool de threads pendant quelques secondes. Il reprend ensuite sur le thread du répartiteur de la page. Cette dernière étape implique une requête, puisque [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) est une propriété de la classe de base **DependencyObject**. Enfin, la page est supprimée suite à l'affectation de `nullptr` à **std::unique_ptr**. Cela appelle ensuite le destructeur de la page.

À l’intérieur du destructeur, nous effaçons le contexte de données, ce qui, comme nous le savons, requiert une requête pour la classe de base **FrameworkElement**.

Tout cela est possible grâce à l'anti-rebond du décompte des références (ou à la stabilisation du décompte des références) offert par C++/WinRT 2.0.