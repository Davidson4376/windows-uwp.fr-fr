---
author: stevewhims
description: Cette rubrique décrit les stratégies de gestion des erreurs lors de la programmation avec C++/WinRT.
title: Gestion des erreurs avec C++/WinRT
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, erreur, gestion, exception
ms.localizationpriority: medium
ms.openlocfilehash: 9a4cf60fea70722e66eb44d52542be248e9ad01c
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4389627"
---
# <a name="error-handling-with-cwinrt"></a>Gestion des erreurs avec C++/WinRT

Cette rubrique décrit les stratégies de gestion des erreurs lors de la programmation avec [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Pour obtenir des informations plus générales, consultez [Gestion des erreurs et des exceptions (C++ moderne)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Éviter d’intercepter et de lever des exceptions
Nous vous recommandons de continuer à écrire du [code pour parer à toute exception](/cpp/cpp/how-to-design-for-exception-safety), mais d'éviter d'intercepter et de lever des exceptions chaque fois que possible. S’il n’existe aucun gestionnaire pour une exception, Windows génère automatiquement un rapport d’erreurs (notamment un fichier minidump de l’incident), ce qui vous permettra d’identifier la cause du problème.

Ne levez pas une exception que vous prévoyez d’intercepter. Et n’utilisez pas les exceptions pour les échecs attendus. Levez une exception *uniquement lorsqu’une erreur d’exécution inattendue se produit* et gérez toutes les autres erreurs avec des codes d'erreur/de résultat&mdash;directement et à proximité de la source de l’échec. De cette façon, lorsqu’une exception *est* levée, vous savez que la cause est soit un bogue dans votre code, soit un état d’erreur exceptionnel dans le système.

Envisagez le scénario d’accès au Registre Windows. Si votre application ne parvient pas à lire une valeur du Registre, c'est normal et vous devez gérer cela correctement. Ne levez pas d'exception; retournez plutôt une valeur `bool` ou `enum` indiquant que (et peut-être pourquoi) la valeur n’a pas été lue. Un échec pour *écrire* une valeur dans le Registre est, en revanche, susceptible d’indiquer qu’il existe un problème plus important que vous pouvez gérer judicieusement dans votre application. Dans un cas comme celui-ci, vous ne devez pas laisser votre application continuer, donc une exception qui produit un rapport d’erreurs est le moyen le plus rapide d'empêcher votre application de causer des dommages.

Comme autre exemple, envisagez de récupérer une image miniature d'un appel à [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_), puis de transmettre cette miniature à [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_). Si cette séquence d’appels vous oblige à passer `nullptr` à **SetSourceAsync** (le fichier image ne peut pas être lu; son extension de fichier fait peut-être croire qu'il contient des données d’image, mais en réalité ce n'est pas le cas), vous allez générer la levée d'une exception de pointeur non valide. Si vous découvrez un cas semblable à celui-ci dans votre code, au lieu d’intercepter et de gérer ce cas comme une exception, vérifiez à la place si la valeur `nullptr` a été renvoyée par **GetThumbnailAsync**.

La levée d’exceptions a tendance à être plus lente que l’utilisation de codes d’erreur. Si vous ne levez une exception que lorsqu’une erreur irrécupérable se produit, si tout se passe bien, ce ne sera jamais au détriment des performances.

Mais un gain de performance plus probable implique la surcharge du runtime de s’assurer que les destructeurs appropriés sont appelés dans l’éventualité peu probable de la levée d'une exception. Cette assurance est coûteuse, qu'une exception soit en réalité levée ou non. Par conséquent, vous devez vous assurer que le compilateur a une idée claire des fonctions susceptibles de lever des exceptions. Si le compilateur peut prouver qu’il n’y aura aucune exception à partir de certaines fonctions (spécification `noexcept`), il peut alors optimiser le code généré.

## <a name="catching-exceptions"></a>Interception des exceptions
Une condition d’erreur qui survient au niveau de la couche [WindowsRuntimeABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) est renvoyée sous la forme d’une valeur HRESULT. Mais vous n’avez pas besoin de gérer des HRESULT dans votre code. Le code de projection C++/WinRT généré pour une API du côté de la consommation détecte un code d'erreur HRESULT au niveau de la couche ABI et le convertit en exception [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error), que vous pouvez capturer et gérer.

Par exemple, si l’utilisateur supprime une image de la bibliothèque d’images pendant que votre application parcourt cette collection, la projection lève une exception. Dans ce cas, vous devez intercepter et gérer cette exception. Voici un exemple de code illustrant ce cas.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            HRESULT hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Utilisez ce même modèle dans une coroutine lorsque vous appelez une fonction faisant l'objet d'une instruction `co_await`. Un autre exemple de cette conversion de HRESULT en exception est le cas où une API du composant retourne E_OUTOFMEMORY, ce qui entraîne la levée d'un **std::bad_alloc**.

## <a name="throwing-exceptions"></a>Levée des exceptions
Dans certains cas, vous déciderez que, si votre appel à une fonction donnée échoue, votre application ne pourra pas être restaurée (vous ne pourrez plus être sûr qu'elle fonctionnera de manière prévisible). L’exemple de code ci-dessous utilise une valeur [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) comme wrapper autour du HANDLE renvoyé par [**CreateEvent**](https://msdn.microsoft.com/library/windows/desktop/ms682396). Il passe ensuite le handle (en créant une valeur `bool` à partir de celui-ci) au modèle de fonction [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool). **winrt::check_bool** fonctionne avec une valeur `bool` ou avec n’importe quelle valeur convertible en valeur `false` (condition d’erreur) ou `true` (condition de réussite).

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Si la valeur que vous passez à [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) est false, la séquence d’actions suivante se produit.

- **winrt::check_bool** appelle la fonction [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error).
- **winrt::throw_last_error** appelle [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) pour récupérer la valeur du dernier code d’erreur du thread appelant et appelle ensuite la fonction [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult).
- **winrt::throw_hresult** lève une exception à l'aide d'un objet [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) (ou d'un objet standard) qui représente le code d’erreur.

Étant donné que les API Windows signalent des erreurs d’exécution à l’aide de différents types de valeur de retour, il existe en plus de **winrt::check_bool** de nombreuses autres fonctions d’assistance pour vérifier les valeurs et lever des exceptions.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Vérifie si le code HRESULT représente une erreur et, si tel est le cas, appelle **winrt::throw_hresult**.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Vérifie si un code représente une erreur et, si tel est le cas, appelle **winrt::throw_hresult**.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Vérifie si un pointeur est null et, si tel est le cas, appelle **winrt::throw_last_error**.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Vérifie si un code représente une erreur et, si tel est le cas, appelle **winrt::throw_hresult**.

Vous pouvez utiliser ces fonctions d’assistance pour les types de code de retour courants, ou vous pouvez répondre à n’importe quelle condition d’erreur et appeler [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) ou [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult). 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Levée d’exceptions lors de la création d’une API
Puisqu'il n'est pas valide qu'une exception franchisse la limite de [WindowsRuntimeABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types), une condition d’erreur qui survient dans une implémentation est renvoyée via la couche ABI sous la forme d’un code d’erreur HRESULT. Lorsque vous créez une API à l’aide de C++/WinRT, le code est généré pour vous afin de convertir en HRESULT les exceptions que vous levez *effectivement* dans votre implémentation. La fonction [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) est utilisée dans ce code généré dans un modèle comme celui-ci.

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) gère les exceptions dérivées de **std::exception** et [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) et ses types dérivés. Dans votre implémentation, vous devez préférer **winrt::hresult_error**, ou un type dérivé, afin que les consommateurs de votre API reçoivent des informations d’erreur complètes. **std::exception** (qui correspond à E_FAIL) est pris en charge en cas d’exceptions survenant en raison de l’utilisation de la bibliothèque de modèles standard.

## <a name="assertions"></a>Assertions
Pour les hypothèses internes dans votre application, il existe des assertions. Privilégiez autant que possible **static_assert** pour la validation au moment de la compilation. Pour les conditions d’exécution, utilisez WINRT_ASSERT avec une expression booléenne.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT est compilé à part dans les versions Release; dans une version de débogage, il arrête l’application dans le débogueur sur la ligne de code où se trouve l’assertion.

Vous ne devez pas utiliser d'exception dans vos destructeurs. Par conséquent, au moins dans les versions de débogage, vous pouvez déclarer le résultat de l'appel d’une fonction à partir d’un destructeur à l'aide de WINRT_VERIFY (avec une expression booléenne) et de WINRT_VERIFY_ (avec un résultat attendu et une expression booléenne).

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>API importantes
* [Modèle de fonction winrt::check_bool](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [Fonction winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Modèle de fonction winrt::check_nt](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [Modèle de fonction winrt::check_pointer](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [Modèle de fonction winrt::check_win32](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [Structure winrt::handle](/uwp/cpp-ref-for-winrt/handle)
* [Structure winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Fonction winrt::throw_hresult](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [Fonction winrt::throw_last_error](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [Fonction winrt::to_hresult](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Rubriquesconnexes
* [Gestion des erreurs et des exceptions (C++ moderne)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Procédure: conception pour parer à toute exception](/cpp/cpp/how-to-design-for-exception-safety)
