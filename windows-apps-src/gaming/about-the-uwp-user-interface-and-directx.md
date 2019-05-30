---
title: Objet application et DirectX
description: Les applications UWP intégrant des jeux DirectX n’utilisent pas beaucoup d’éléments et d’objets d’interface utilisateur Windows Store.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, directx, objet d'application
ms.localizationpriority: medium
ms.openlocfilehash: c6bf49d5caa1dcf57e6b3622763fd01ad6fcf6b6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369244"
---
# <a name="the-app-object-and-directx"></a>Objet application et DirectX



Les applications UWP intégrant des jeux DirectX n’utilisent pas beaucoup d’éléments et d’objets d’interface utilisateur Windows Store. En effet, comme elles s’exécutent à un niveau inférieur de la pile Windows Runtime, elles doivent interopérer avec l’infrastructure d’interface utilisateur d’une manière plus basique : en accédant directement à l’objet application et en interopérant avec lui. Découvrez quand et comment cette interopération se produit et comment vous, en tant que développeur DirectX, vous pouvez exploiter efficacement ce modèle dans le cadre du développement de vos applications de la plateforme Windows universelle.

Consultez le [glossaire de graphiques Direct3D](../graphics-concepts/index.md) pour plus d’informations sur les conditions de graphiques inhabituels ou des concepts que vous rencontrez lors de la lecture.

## <a name="the-important-core-user-interface-namespaces"></a>Principaux espaces de noms d’interface utilisateur


Pour commencer, intéressons-nous aux espaces de noms Windows Runtime que vous devez inclure (à l’aide de **using**) dans vos applications de la plateforme Windows universelle. Entrons un peu dans les détails.

-   [**Windows.ApplicationModel.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core)
-   [**Windows.ApplicationModel.Activation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation)
-   [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
-   [**Windows.System**](https://docs.microsoft.com/uwp/api/Windows.System)
-   [**Windows.Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation)

> **Remarque**    si vous ne développez pas d’une application UWP, utilisez les composants d’interface utilisateur fournis dans les bibliothèques de JavaScript ou XAML spécifiques à et les espaces de noms au lieu des types fournis dans ces espaces de noms.

 

## <a name="the-windows-runtime-app-object"></a>Objet application Windows Runtime


Dans votre application UWP, vous souhaitez obtenir une fenêtre et un fournisseur de vues à partir duquel vous pouvez obtenir une vue et auquel vous pouvez connecter votre chaîne d’échange (vos mémoires tampons d’affichage). Vous pouvez également « établir un hook » avec cette vue dans les événements spécifiques à la fenêtre pour votre application en cours d’exécution. Pour obtenir la fenêtre parente pour l’objet application, qui est définie par le type [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), créez un type qui implémente [**IFrameworkViewSource**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource), comme nous l’avons fait dans l’extrait de code précédent.

Pour faire en sorte qu’une fenêtre utilise l’infrastructure d’interface utilisateur de base, procédez comme suit :

1.  Créez un type qui implémente [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Il s’agit de votre vue.

    Dans ce type, définissez :

    -   Une méthode [**Initialize**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) qui prend une instance de [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) comme paramètre. Vous pouvez obtenir une instance de ce type en appelant [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview). L’objet application l’appelle au lancement de l’application.
    -   Une méthode [**SetWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) qui prend une instance de [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) comme paramètre. Vous pouvez obtenir une instance de ce type en accédant à la propriété [**CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) de votre nouvelle instance de [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView).
    -   Une méthode [**Load**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load) qui prend une chaîne de point d’entrée comme paramètre unique. L’objet application fournit la chaîne de point d’entrée lorsque vous appelez cette méthode. C’est ici que vous définissez les ressources et c’est ici que vous créez les ressources de votre périphérique. L’objet application l’appelle au lancement de l’application.
    -   Une méthode [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) qui active l’objet [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) et démarre le répartiteur d’événements de fenêtre. L’objet application l’appelle au démarrage du processus de l’application.
    -   Une méthode [**Uninitialize**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) qui nettoie les ressources définies dans l’appel à [**Load**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load). L’objet application appelle cette méthode lorsque l’application est fermée.

2.  Créez un type qui implémente [**IFrameworkViewSource**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource). Il s’agit de votre fournisseur de vues.

    Dans ce type, définissez :

    -   Une méthode nommée [**CreateView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview) qui renvoie une instance de votre implémentation de [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) créée à l’étape 1.

3.  Passez une instance du fournisseur de vues à [**CoreApplication.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.run) à partir de **main**.

Maintenant que nous avons vu les principes de base, examinons les autres options permettant de développer cette approche.

## <a name="core-user-interface-types"></a>Principaux types d’interface utilisateur


Voici d’autres types principaux d’interface utilisateur figurant dans Windows Runtime et qui peuvent s’avérer utiles :

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
-   [**Windows.UI.Core.CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows.UI.Core.CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)

Vous pouvez utiliser ces types pour accéder à la vue de votre application, plus précisément, aux éléments qui dessinent le contenu de la fenêtre parente de l’application et gèrent les événements déclenchés pour cette fenêtre. Le processus de la fenêtre de l’application est un *thread unique cloisonné d’application* (ASTA, Application Single-Threaded Apartment) isolé qui gère tous les rappels.

La vue de votre application est générée par le fournisseur de vues de la fenêtre de votre application et comme dans la plupart des cas, elle est implémentée par un package d’infrastructure spécifique ou par le système lui-même, vous n’avez pas besoin de l’implémenter vous-même. Pour DirectX, vous devez implémenter un fournisseur de vues léger, comme nous l’avons vu précédemment. Il existe une relation un-à-un spécifique entre les composants et les comportements suivants :

-   La vue d’une application, qui est représentée par le type [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) et qui définit la ou les méthodes de mise à jour de la fenêtre.
-   Un thread ASTA, dont l’attribution définit le comportement de thread de l’application. Vous ne pouvez pas créer d’instances de types attribués par un thread STA COM sur un thread ASTA.
-   Un fournisseur de vues, que votre application obtient du système ou que vous implémentez.
-   Une fenêtre parente, qui est représentée par le type [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow).
-   Le sourçage pour tous les événements d’activation. Les vues et les fenêtres ont des événements d’activation distincts.

Pour résumer, l’objet application fournit une fabrique de fournisseurs de vues. Elle crée un fournisseur de vues et instancie une fenêtre parente pour l’application. Le fournisseur de vues définit la vue de l’application pour la fenêtre parente de l’application. À présent, examinons les spécificités de la vue et de la fenêtre parente.

## <a name="coreapplicationview-behaviors-and-properties"></a>Comportements et propriétés de CoreApplicationView


[**CoreApplicationView** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) représentent l’opinion d’application actuelle. Le singleton de l’application crée la vue de l’application pendant l’initialisation, mais la vue reste dormante jusqu’à ce qu’elle soit activée. Vous pouvez obtenir le [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) qui affiche la vue en accédant à sa propriété [**CoreApplicationView.CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow), et vous pouvez gérer les événements d’activation et de désactivation de la vue en inscrivant des délégués avec l’événement [**CoreApplicationView.Activated**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated).

## <a name="corewindow-behaviors-and-properties"></a>Comportements et propriétés de CoreWindow


La fenêtre parente, qui est une instance de [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), est créée et passée au fournisseur de vues lors de l’initialisation de l’objet application. Si l’application a une fenêtre à afficher, elle l’affiche ; sinon, elle ne fait qu’initialiser la vue.

[**CoreWindow** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) fournit un nombre d’événements des comportements spécifiques à la base d’entrée et de fenêtre. Vous pouvez gérer ces événements en inscrivant vos propres délégués avec ces derniers.

Vous pouvez également obtenir le répartiteur d’événements de fenêtre pour la fenêtre en accédant à la propriété [**CoreWindow.Dispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.dispatcher), qui fournit une instance de [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher).

## <a name="coredispatcher-behaviors-and-properties"></a>Comportements et propriétés de CoreDispatcher


Vous pouvez déterminer le comportement de thread de la distribution d’événements pour une fenêtre avec le type [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher). Dans ce type, il existe une méthode particulièrement importante : la méthode [**CoreDispatcher.ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents), qui démarre le traitement des événements de fenêtre. Le fait d’appeler cette méthode avec une option non appropriée peut entraîner toutes sortes de comportements de traitement d’événements inattendus.

| Option CoreProcessEventsOption                                                           | Description                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | Distribue tous les événements actuellement disponibles dans la file d’attente. Si aucun événement n’est en attente, attend le nouvel événement suivant.                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreProcessEventsOption)     | Distribue un événement s’il est en attente dans la file d’attente. Si aucun événement n’est en attente, au lieu d’attendre le déclenchement d’un nouvel événement, effectue un retour immédiat.                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreProcessEventsOption)        | Attend de nouveaux événements et distribue tous les événements disponibles. Poursuit ce comportement jusqu’à ce que la fenêtre se ferme ou que l’application appelle la méthode [**Close**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.close) sur l’instance de [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreProcessEventsOption)     | Distribue tous les événements actuellement disponibles dans la file d’attente. Si aucun événement n’est en attente, effectue un retour immédiat.                                                                                                                                          |

 

Les applications DirectX UWP doivent utiliser l’option [**CoreProcessEventsOption.ProcessAllIfPresent**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreProcessEventsOption) pour empêcher les comportements de blocage qui pourraient interrompre les mises à jour graphiques.

## <a name="asta-considerations-for-directx-devs"></a>Considérations relatives aux threads ASTA pour les développeurs DirectX


L’objet application qui définit la représentation au moment de l’exécution de votre application UWP basée sur DirectX utilise un modèle de thread appelé modèle de thread unique cloisonné d’application (ASTA) pour héberger les affichages d’interface utilisateur de votre application. Si vous développez une application UWP basée sur DirectX, vous connaissez déjà les propriétés d’un thread ASTA, car tout thread que vous distribuez à partir de votre application UWP basée sur DirectX doit utiliser les API [**Windows::System::Threading**](https://docs.microsoft.com/uwp/api/Windows.System.Threading), ou recourir à [**CoreWindow::CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher). (Vous pouvez obtenir l’objet [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) pour le thread ASTA en appelant [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) à partir de votre application.)

En tant que développeur d’une application UWP basée sur DirectX, vous devez veiller à activer votre thread d’application pour distribuer des threads MTA en définissant **Platform::MTAThread** sur **main()** .

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

Quand l’objet d’application de votre application UWP basée sur DirectX s’active, il crée le thread ASTA utilisé pour l’affichage d’interface utilisateur. Le nouveau thread ASTA appelle votre fabrique de fournisseur de vues pour créer le fournisseur de vues de votre objet d’application. Par conséquent, le code de votre fournisseur de vues s’exécute sur ce thread ASTA.

En outre, tous les threads que vous convertissez à partir du thread ASTA doivent être en multithread cloisonné (MTA). N’oubliez pas que tous les threads MTA que vous convertissez peuvent encore créer des problèmes de réentrance et mener à un interblocage.

Si vous portez du code existant afin qu’il s’exécute sur le thread ASTA, n’oubliez pas les points suivants :

-   Les primitives d’attente, par exemple [**CoWaitForMultipleObjects**](https://docs.microsoft.com/windows/desktop/api/combaseapi/nf-combaseapi-cowaitformultipleobjects), ne se comportent pas de la même façon dans un thread ASTA que dans un thread STA.
-   La boucle modale d’appel COM fonctionne différemment dans un thread ASTA. Vous ne pouvez plus recevoir d’appels n’ayant pas de rapport avec un appel sortant en cours. Par exemple, le comportement suivant crée un interblocage à partir d’un thread ASTA (et entraîne immédiatement le blocage de l’application) :
    1.  Le thread ASTA appelle un objet MTA et passe un pointeur d’interface P1.
    2.  Le thread ASTA appelle plus tard le même objet MTA. L’objet MTA appelle P1 avant son retour au thread ASTA.
    3.  P1 ne peut pas entrer dans le thread ASTA, car il est bloqué en tentant d’effectuer un appel sans rapport. Toutefois, le thread MTA est bloqué, car il tente d’effectuer l’appel vers P1.

    Vous pouvez résoudre ce problème en :
    -   utilisant le modèle **async** défini dans la Bibliothèque de modèles parallèles (PPLTasks.h) ;
    -   appelant [**CoreDispatcher::ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) à partir du thread ASTA de votre application (thread principal de votre application) dès que possible pour autoriser les appels arbitraires.

    Cela étant dit, vous ne pouvez pas compter sur une remise immédiate des appels non apparentés au thread ASTA de votre application. Pour plus d’informations sur les appels asynchrones, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

Globalement, lors de la conception de votre application UWP basée sur DirectX, utilisez [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) pour le [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) de votre application et [**CoreDispatcher::ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) pour gérer tous les threads d’interface utilisateur au lieu d’essayer de créer et de gérer vos threads MTA vous-même. Lorsque vous avez besoin d’un thread distinct que vous ne pouvez pas gérer avec **CoreDispatcher**, utilisez des modèles asynchrones et suivez les instructions mentionnées précédemment pour éviter les problèmes de réentrance.

 

 




