---
author: normesta
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: Cet article décrit la meilleure façon d’utiliser des méthodes asynchrones dans les extensions de composant Visual c++ (C++ / CX) à l’aide de la classe de tâche définie dans l’espace de noms concurrency dans ppltasks.h.
title: Programmation asynchrone en C++
ms.author: normesta
ms.date: 05/14/2018
ms.topic: article
keywords: Windows10, uwp, threads, asynchrone, C++
ms.localizationpriority: medium
ms.openlocfilehash: 33b110e713608260cd5c19544292e9211904a730
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6209671"
---
# <a name="asynchronous-programming-in-ccx"></a>Programmation asynchrone en C++/CX
> [!NOTE]
> Cette rubrique a pour but de vous aider à maintenir votre application C++/CX. Mais nous vous recommandons d’utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour de nouvelles applications. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Cet article décrit la meilleure façon d’utiliser des méthodes asynchrones dans les extensions de composant Visual c++ (C++ / CX) à l’aide de la `task` classe qui est définie dans le `concurrency` espace de noms dans ppltasks.h.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Types asynchrones de plateforme Windows universelle (UWP)
Les fonctionnalités de plateforme Windows universelle UWP comprennent un modèle bien défini pour l’appel de méthodes asynchrones, et fournissent les types dont vous avez besoin pour consommer de telles méthodes. Si vous n'avez pas l'habitude d'utiliser le modèle asynchrone UWP, veuillez consulter l'article [Programmation asynchrone][AsyncProgramming] avant de poursuivre la lecture de cet article.

Bien qu'il soit possible d'utiliser des API UWP asynchrones directement dans C++, il est recommandé d'utiliser la [**classe task**][task-class] et ses types et fonctions associés (contenus dans l'espace de noms [**concurrency**][concurrencyNamespace] et définis dans `<ppltasks.h>`). Le type **concurrency::task** est un type à usage général, mais lorsque le commutateur du compilateur **/ZW**, requis pour les applications et composants de plateforme Windows universelle (UWP), est utilisé, la classe task encapsule les types asynchrones UWP, ce qui facilite les opérations suivantes :

-   chaîner plusieurs opérations asynchrones et synchrones

-   gérer des exceptions dans des chaînes de tâches

-   effectuer des annulations dans des chaînes de tâches

-   garantir que des tâches individuelles s’exécutent dans le contexte de thread ou le cloisonnement de threads approprié

Cet article fournit des indications de base sur la façon d’utiliser la classe **task** avec les API asynchrones UWP. Pour obtenir une documentation complète sur la classe **task** et ses méthodes, notamment [**create\_task**][createTask], voir [Parallélisme des tâches (runtime d’accès concurrentiel)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Consommation d’une opération asynchrone à l’aide d’une tâche
L'exemple suivant montre comment utiliser la classe task pour utiliser une méthode **async** qui retourne une interface [**IAsyncOperation**][IAsyncOperation] et dont l'opération produit une valeur. Voici les étapes de base à effectuer :

1.  Appelez la méthode `create_task` et passez-lui l’objet **IAsyncOperation^**.

2.  Appelez la fonction membre [**task::then**][taskThen] sur la tâche et fournissez une expression lambda qui sera appelée à la fin de l'exécution de l’opération asynchrone.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

La tâche créée et retournée par la fonction [**task::then**][taskThen] est appelée *continuation*. L’argument d’entrée (dans le cas présent) pour l’expression lambda fournie par l’utilisateur est le résultat que l’opération de la tâche produit quand elle se termine. C’est la même valeur que celle qui serait récupérée en appelant la méthode [**IAsyncOperation::GetResults**](https://msdn.microsoft.com/library/windows/apps/br206600) si vous utilisiez directement l’interface **IAsyncOperation**.

La méthode [**task::then**][taskThen] renvoie immédiatement un résultat, et son délégué ne s’exécute que quand le travail asynchrone se termine correctement. Dans cet exemple, si l’opération asynchrone entraîne la levée d’une exception, ou se termine dans l’état d’annulation suite à une demande d’annulation, la continuation ne s’exécute jamais. Nous expliquerons ultérieurement comment écrire des continuations qui s’exécutent même si la tâche précédente a été annulée ou a échoué.

Bien que vous déclariez la variable de la tâche sur la pile locale, elle gère sa durée de vie de façon à ne pas être supprimée tant que toutes ses opérations ne sont pas terminées et que toutes les références à celle-ci ne sortent pas de l’étendue, même si la méthode retourne un résultat avant que les opérations soient terminées.

## <a name="creating-a-chain-of-tasks"></a>Création d’une chaîne de tâches
Dans la programmation asynchrone, il est courant de définir une séquence d’opérations, également appelée *chaîne de tâches*, dans laquelle chaque continuation s’exécute uniquement si la précédente est terminée. Dans certains cas, la tâche précédente (aussi appelée *antécédent*) produit une valeur que la continuation accepte comme entrée. En utilisant la méthode [**task::then**][taskThen], vous pouvez créer des chaînes de tâches d’une manière intuitive et simple; la méthode renvoie une valeur **task<T>** où **T** est le type de retour de la fonction lambda. Vous pouvez composer plusieurs continuations dans une chaîne de tâches : `myTask.then(…).then(…).then(…);`

Les chaînes de tâches sont particulièrement utiles lorsqu’une continuation crée une nouvelle opération asynchrone; une telle tâche est appelée «tâche asynchrone». L’exemple suivant illustre une chaîne de tâches comportant deux continuations. La tâche initiale acquiert le handle vers un fichier existant, et lorsque cette opération se termine, la première continuation démarre une nouvelle opération asynchrone pour supprimer le fichier. Lorsque l’opération se termine, la deuxième continuation s’exécute, et génère un message de confirmation.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

L’exemple précédent illustre quatre points importants:

-   La première continuation convertit l’objet [**IAsyncAction^**][IAsyncAction] en un type **task<void>**  et renvoie l’objet **task**.

-   La deuxième continuation n’effectue aucune gestion des erreurs, et par conséquent, prend **void** et non pas **task<void>** comme entrée. C’est une continuation basée sur les valeurs.

-   La deuxième continuation ne s’exécute pas tant que l’opération [**DeleteAsync**][deleteAsync] n’est pas terminée.

-   Étant donné que la deuxième continuation est basée sur les valeurs, si l’opération qui a été démarrée par l’appel à l’opération [**DeleteAsync**][deleteAsync] lève une exception, la deuxième continuation ne s’exécute pas du tout.

**Remarque**création d’une chaîne de tâches est juste l’un des moyens d’utiliser la classe **task** pour composer des opérations asynchrones. Vous pouvez également composer des opérations en utilisant des opérateurs de jointure et de choix **&&** et **||**. Pour plus d’informations, voir [Parallélisme des tâches (runtime d’accès concurrentiel)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Types de retour de la fonction lambda et types de retour d’une tâche
Dans une continuation de tâche, le type de retour de la fonction lambda est encapsulé dans un objet **task**. Si l’expression lambda retourne une valeur **double**, le type de la tâche de continuation est **task<double>**. Toutefois, l’objet task est conçu de façon à ne pas produire inutilement des types de retour imbriqués. Si une expression lambda retourne un **IAsyncOperation&lt;SyndicationFeed^&gt;^**, la continuation retourne un **task&lt;SyndicationFeed^&gt;**, et non un **task&lt;task&lt;SyndicationFeed^&gt;&gt;** ou **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^**. Ce processus, qui porte le nom de *désencapsulation asynchrone*, garantit également l’achèvement de l’opération asynchrone à l’intérieur de la continuation avant l’appel de la continuation suivante.

Dans l’exemple précédent, notez que la tâche retourne un type **task<void>** en dépit du fait que l’expression lambda ait renvoyé un objet [**IAsyncInfo**][IAsyncInfo]. Le tableau suivant résume les conversions de types qui se produisent entre une fonction lambda et la tâche englobante :

| | |
|--------------------------------------------------------|---------------------|
| type de retour de la fonction lambda                                     | `.then` type de retour |
| TResult                                                | tâche<TResult> |
| IAsyncOperation<TResult>^                        | tâche<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | tâche<TResult> |
|IAsyncAction^                                           | tâche<void>    |
| IAsyncActionWithProgress<TProgress>^             |tâche<void>     |
| tâche<TResult>                                    |tâche<TResult>  |


## <a name="canceling-tasks"></a>Annulation de tâches
Il est souvent conseillé de donner à l’utilisateur la possibilité d’annuler une opération asynchrone. Et dans certains cas, vous pouvez avoir besoin d’annuler une opération par programme depuis l’extérieur de la chaîne de tâches. Bien que chaque type de retour \***Async** ait une méthode [**Cancel**][IAsyncInfoCancel] qui hérite de l'objet [**IAsyncInfo**][IAsyncInfo], il est compliqué de l’exposer à des méthodes extérieures. Le meilleur moyen de prendre en charge l’annulation dans une chaîne de tâches consiste à utiliser un objet [**cancellation\_token\_source**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749985.aspx) pour créer un objet [**cancellation\_token**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749975.aspx), puis à transmettre le jeton au constructeur de la tâche initiale. Si une tâche asynchrone est créée avec un jeton d’annulation, et que la classe [**cancellation\_token\_source::cancel**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750076.aspx) est appelée, la tâche appelle automatiquement la méthode **Cancel** sur l’opération **IAsync\*** et transmet la demande d’annulation à sa chaîne de continuation. Le pseudo-code suivant présente l’approche de base.

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

Lorsqu’une tâche est annulée, une exception [**task\_canceled**][taskCanceled] est propagée vers la chaîne de tâches. Les continuations basées sur les valeurs ne s’exécutent simplement pas, mais les continuations basées sur les tâches entraînent la levée d’une exception lorsque la méthode [**task::get**][taskGet] est appelée. Si vous avez une continuation de gestion des erreurs, assurez-vous qu’elle intercepte l’exception **task\_canceled** de manière explicite. (Cette exception n’est pas dérivée de [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755825.aspx).)

L’annulation est coopérative. Si votre continuation effectue un travail de longue durée au-delà du simple appel de méthode UWP, il vous incombe de vérifier régulièrement l’état du jeton d’annulation et d’arrêter l’exécution s’il est annulé. Après avoir nettoyé toutes les ressources qui ont été allouées dans la continuation, appelez [**cancel\_current\_task**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749945.aspx) pour annuler la tâche et propager l’annulation à toutes les continuations basées sur les valeurs qui la suivent. Voici un autre exemple : vous pouvez créer une chaîne de tâches qui représente le résultat d’une opération [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/BR207871). Si l’utilisateur choisit le bouton **Cancel**, la méthode [**IAsyncInfo::Cancel**][IAsyncInfoCancel] n'est pas appelée. Au lieu de cela, l’opération réussit mais renvoie **nullptr**. La continuation peut tester le paramètre d’entrée et appeler **cancel\_current\_task** si l’entrée est **nullptr**.

Pour plus d’informations, voir [Annulation dans la bibliothèque de modèles parallèles](https://msdn.microsoft.com/library/windows/apps/xaml/dd984117.aspx)

## <a name="handling-errors-in-a-task-chain"></a>Gestion des erreurs dans une chaîne de tâches
Si vous voulez qu’une continuation s’exécute même si l’antécédent a été annulé ou a levé une exception, transformez la continuation en continuation basée sur les tâches en spécifiant l’entrée de sa fonction lambda en tant que type **task<TResult>** ou **task<void>** si la fonction lambda de la tâche précédente (antécédent) renvoie un objet [**IAsyncAction^**][IAsyncAction].

Pour gérer les erreurs et l’annulation dans une chaîne de tâches, il n’est pas nécessaire de transformer chaque continuation en continuation basée sur les tâches ou de placer chaque opération qui pourrait être déclenchée dans un bloc `try…catch`. Au lieu de cela, vous pouvez ajouter une continuation basée sur les tâches à la fin de la chaîne et gérer toutes les erreurs à cet emplacement. Toute exception, y compris une exception [**task\_canceled**][taskCanceled], se propage dans la chaîne de tâches et ignore toutes les continuations basées sur les valeurs, de sorte que vous pouvez la gérer dans la continuation de gestion des erreurs basée sur les tâches. Vous pouvez récrire l’exemple précédent pour utiliser une continuation de gestion des erreurs basée sur les tâches :

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

Dans une continuation basée sur les tâches, nous appelons la fonction membre [**task::get**][taskGet] pour obtenir les résultats de la tâche. Nous devons toujours appeler **task::get** même si l’opération était un type [**IAsyncAction**][IAsyncAction] qui ne produit aucun résultat, car **task::get** obtient également toutes les exceptions qui ont été acheminées jusqu’à la tâche. Si la tâche d’entrée stocke une exception, celle-ci est levée lors de l’appel à **task::get**. Si vous n’appelez pas **task::get**, que vous n’utilisez pas de continuation basée sur les tâches à la fin de la chaîne ou que vous n’interceptez pas le type d’exception qui est levé, une exception **unobserved\_task\_exception** est levée quand toutes les références à la tâche ont été supprimées.

Interceptez uniquement les exceptions que vous pouvez gérer. Si votre application rencontre une erreur de laquelle vous ne pouvez pas récupérer, il est préférable de laisser l’application se bloquer plutôt que de laisser son exécution se poursuivre dans un état inconnu. En général, n’essayez pas non plus d’intercepter l’exception **unobserved\_task\_exception**. Cette exception est principalement destinée aux opérations de diagnostic. Quand une exception **unobserved\_task\_exception** est levée, elle indique généralement un bogue dans le code. La cause est souvent soit une exception qui doit être gérée, soit une exception irrécupérable qui est provoquée par une autre erreur dans le code.

## <a name="managing-the-thread-context"></a>Gestion du contexte de thread
L’interface utilisateur d’une application UWP s’exécute dans un thread unique cloisonné (STA). Une tâche dont l’expression lambda renvoie un objet [**IAsyncAction**][IAsyncAction] ou un objet [**IAsyncOperation**][IAsyncOperation] prend en charge le cloisonnement. Sauf spécification contraire, si la tâche est créée dans le thread unique cloisonné (STA), toutes les continuations qu’elle contient s’exécuteront également par défaut. En d’autres termes, l’ensemble de la chaîne de tâches hérite de la prise en charge du cloisonnement de la tâche parente. Ce comportement permet de simplifier les interactions avec les contrôles d’interface utilisateur, lesquels sont accessibles uniquement à partir du thread unique cloisonné (STA).

Par exemple, dans une application UWP, dans la fonction membre de toute classe représentant une page XAML, vous pouvez remplir un contrôle [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) à partir d’une méthode [**task::then**][taskThen] sans avoir à utiliser l’objet [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

Si une tâche ne renvoie pas un objet [**IAsyncAction**][IAsyncAction] ou un objet [**IAsyncOperation**][IAsyncOperation], c’est qu’elle ne prend pas en charge le cloisonnement et, par défaut, ses continuations sont exécutées sur le premier thread d’arrière-plan disponible.

Vous pouvez remplacer le contexte de thread par défaut pour n’importe quelle sorte de tâche en utilisant la surcharge de la fonction [**task::then**][taskThen] qui prend une classe [**task\_continuation\_context**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749968.aspx). Par exemple, dans certains cas, il peut être souhaitable de planifier la continuation d’une tâche prenant en charge le cloisonnement sur un thread d’arrière-plan. Si tel est le cas, vous pouvez transmettre la méthode [**task\_continuation\_context::use\_arbitrary**][useArbitrary] pour planifier le travail de la tâche sur le thread disponible suivant d’un multithread cloisonné. Cela peut améliorer les performances de la continuation, car son travail n’a pas besoin d’être synchronisé avec un autre travail qui se produit sur le thread d’interface utilisateur.

L’exemple suivant montre quand il est utile de spécifier l’option [**task\_continuation\_context::use\_arbitrary**][useArbitrary], et comment le contexte de continuation par défaut est utile pour la synchronisation d’opérations simultanées sur des collections qui ne sont pas thread-safe. Dans ce code, nous parcourons en boucle une liste d’URL pour des flux RSS, et pour chaque URL, nous démarrons une opération asynchrone pour récupérer les données de flux. Nous ne pouvons pas contrôler l’ordre dans lequel les flux sont récupérés, et cela n’a pas vraiment d’importance. Quand chaque opération [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR210642) se termine, la première continuation accepte l’objet [**SyndicationFeed^**](https://msdn.microsoft.com/library/windows/apps/BR243485) et l’utilise pour initialiser un objet `FeedData^` défini par l’application. Chacune de ces opérations étant indépendante des autres, nous pouvons éventuellement accélérer le processus en spécifiant le contexte de continuation **task\_continuation\_context::use\_arbitrary**. Toutefois, une fois chaque objet `FeedData` initialisé, nous devons l’ajouter à une classe [**Vector**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx), qui n’est pas une collection thread-safe. Par conséquent, nous créons une continuation et spécifions [**task\_continuation\_context::use\_current**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) pour garantir que tous les appels à la méthode [**Append**](https://msdn.microsoft.com/library/windows/apps/BR206632) se produisent dans le même contexte de cloisonnement de threads unique d’application (ASTA, Application Single-Threaded Apartment). Étant donné que la méthode [**task\_continuation\_context::use\_default**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) est le contexte par défaut, nous n’avons pas besoin de la spécifier explicitement, mais nous le faisons par souci de clarté.

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

Les tâches imbriquées, qui sont de nouvelles tâches créées à l’intérieur d’une continuation, n’héritent pas de la prise en charge du cloisonnement de la tâche initiale.

## <a name="handing-progress-updates"></a>Gestion des mises à jour de l’avancement
Les méthodes qui prennent en charge [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) ou [**IAsyncActionWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx) fournissent régulièrement des mises à jour de l’avancement pendant que l’opération est en cours, avant qu’elle ne se termine. Le signalement de l’avancement est indépendant de la notion de tâches et de continuations. Vous fournissez simplement le délégué pour la propriété [**Progress**](https://msdn.microsoft.com/library/windows/apps/br206594) de l’objet. Le délégué est généralement utilisé pour mettre à jour une barre de progression dans l’interface utilisateur.

## <a name="related-topics"></a>Rubriques connexes
* [Création d’opérations asynchrones en C++/CX pour des applications UWP](https://msdn.microsoft.com/library/hh750082)
* [Informations de référence en matière de langage Visual C++](http://msdn.microsoft.com/library/windows/apps/hh699871.aspx)
* [Programmation asynchrone][AsyncProgramming]
* [Parallélisme des tâches (runtime d’accès concurrentiel)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "Espace de noms concurrency"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Classe task"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "Parallélisme des tâches"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
