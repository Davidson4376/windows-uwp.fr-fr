---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: Envoyer un élément de travail au pool de threads
description: Découvrez comment effectuer des tâches dans un thread distinct en envoyant un élément de travail au pool de threads.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, threads, pool de threads
ms.localizationpriority: medium
ms.openlocfilehash: 423f0efa9118f581d6e768a815dd2550801aa87e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658014"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>Envoyer un élément de travail au pool de threads

\[ Mise à jour pour les applications UWP sur Windows 10. Pour Windows 8.x articles, consultez le [archive](https://go.microsoft.com/fwlink/p/?linkid=619132) \]

<b>API importantes</b>

-   [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)

Découvrez comment effectuer des tâches dans un thread distinct en envoyant un élément de travail au pool de threads. Cela vous permet de maintenir une interface utilisateur réactive tout en effectuant des tâches dont l’exécution nécessite un temps non négligeable, et utilisez-la pour accomplir plusieurs tâches en parallèle.

## <a name="create-and-submit-the-work-item"></a>Créer et envoyer l’élément de travail

Créez un élément de travail en appelant [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593). Fournissez un délégué pour effectuer le travail (vous pouvez utiliser une expression lambda ou une fonction déléguée). Notez que **RunAsync** retourne un objet [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580). Stockez cet objet en vue de son utilisation à la prochaine étape.

Trois versions de [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) sont disponibles pour que vous puissiez éventuellement spécifier la priorité de l’élément de travail et contrôler s’il est exécuté simultanément avec d’autres éléments de travail.

>[!NOTE]
>Utilisez [ **CoreDispatcher.RunAsync** ](https://msdn.microsoft.com/library/windows/apps/Hh750317) pour accéder à la thread d’interface utilisateur et afficher la progression à partir de l’élément de travail.

L’exemple suivant crée un élément de travail et fournit une expression lambda pour effectuer la tâche :

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new Windows::System::Threading::WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

Après l’appel à [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593), l’élément de travail est mis en file d’attente par le pool de threads et s’exécute lorsque le thread devient disponible. Les éléments de travail du pool de threads s’exécutent de manière asynchrone. De plus, étant donné qu’ils peuvent s’exécuter dans n’importe quel ordre, assurez-vous qu’ils fonctionnent de manière indépendante.

Notez que l’élément de travail vérifie la propriété [**IAsyncInfo.Status**](https://msdn.microsoft.com/library/windows/apps/BR206593) et se ferme s’il est annulé.

## <a name="handle-work-item-completion"></a>Gérer l’achèvement de l’élément de travail

Fournissez un gestionnaire d’achèvement en définissant la propriété [**IAsyncAction.Completed**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.completed.aspx) de l’élément de travail. Fournissez un délégué (vous pouvez utiliser une expression lambda ou une fonction déléguée) pour gérer l’achèvement de l’élément de travail. Par exemple, utilisez [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) pour accéder au thread d’interface utilisateur et afficher le résultat.

L’exemple suivant met à jour l’interface utilisateur avec le résultat de l’élément de travail envoyé à l’étape 1 :

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

Notez que le gestionnaire d’achèvement vérifie si l’élément de travail a été annulé avant de diffuser une mise à jour de l’interface utilisateur.

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Vous trouverez plus en téléchargeant le code à partir de ce démarrage rapide dans le [création d’un pool de threads de travail élément exemple](https://go.microsoft.com/fwlink/p/?LinkID=328569) écrite pour Windows 8.1, ou en réutilisant le code source dans un fichier win\_unap Windows 10 application.

## <a name="related-topics"></a>Rubriques connexes

* [Soumettre un élément de travail au pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Meilleures pratiques pour utiliser le pool de threads](best-practices-for-using-the-thread-pool.md)
* [Utiliser un minuteur pour soumettre un élément de travail](use-a-timer-to-submit-a-work-item.md)
 
