---
author: msatranjr
title: Création d’un composant Windows Runtime en C++/CX et appel de ce composant à partir de JavaScript ou C#
description: Cette procédure pas à pas montre comment créer une DLL de composant Windows Runtime de base qui peut être appelée à partir de JavaScript, C# ou Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.author: misatran
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79c2bd63c326b90b0b5d6e5007c4610f22bff670
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4499910"
---
# <a name="walkthrough-creating-a-windows-runtime-component-in-ccx-and-calling-it-from-javascript-or-c"></a>Procédure pas à pas: création d’un composant WindowsRuntime en C++/CX et appel de ce composant à partir de JavaScript ou C#
> [!NOTE]
> Cette rubrique a pour but de vous aider à maintenir votre application C++/CX. Mais nous vous recommandons d’utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour de nouvelles applications. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Pour savoir comment créer un composant Windows Runtime à l’aide de C++ / WinRT, voir [créer des événements en C++ / WinRT](../cpp-and-winrt-apis/author-events.md).

Cette procédure pas à pas montre comment créer une DLL de composant Windows Runtime de base qui peut être appelée à partir de JavaScript, C# ou Visual Basic. Avant d’entreprendre cette procédure pas à pas, vous devez maîtriser des concepts tels que l’interface binaire abstraite (ABI), les classes ref et les extensions des composants Visual C++ qui facilitent l’utilisation des classes ref. Pour plus d’informations, consultez les articles [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) et [Informations de référence sur le langage Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## <a name="creating-the-c-component-dll"></a>Création de la DLL du composant C++
Dans cet exemple, nous commençons par créer le projet du composant, mais vous pouvez très bien créer le projet JavaScript en premier. L’ordre n’a pas d’importance.

Notez que la classe principale du composant contient des exemples de définitions de propriété et de méthode, ainsi qu’une déclaration d’événement. Ces éléments sont fournis uniquement pour vous en montrer le fonctionnement. Ils ne sont pas obligatoires, et dans cet exemple, nous remplacerons l’ensemble du code généré par notre propre code.

## **<a name="to-create-the-c-component-project"></a>Pour créer le projet du composant C++**
Dans la barre de menus Visual Studio, choisissez **Fichier &gt; Nouveau &gt; Projet**.

Dans le volet gauche de la boîte de dialogue **Nouveau projet**, développez **Visual C++**, puis sélectionnez le nœud des applications Windows universelles.

Dans le volet central, sélectionnez **Composant Windows Runtime**, puis nommez le projet WinRT\_CPP.

Cliquez sur le bouton **OK**.

## **<a name="to-add-an-activatable-class-to-the-component"></a>Pour ajouter une classe activable au composant**
Une classe activable est une classe que le code client peut créer à l’aide d’une expression **new** (**New** en Visual Basic ou **ref new** en C++). Dans votre composant, vous devez la déclarer sous la forme **public ref class sealed**. En fait, les fichiers Class1.h et .cpp disposent déjà d’une classe ref. Vous pouvez changer le nom, mais dans cet exemple, nous utiliserons celui par défaut: Class1. Vous pouvez définir des classes ref ou standard supplémentaires dans votre composant si nécessaire. Pour plus d’informations sur les classes ref, voir [Système de types (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

Ajoutez ces directives \#include à Class1.h:

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h est le fichier d’en-tête pour les classes C++ concrètes, telles que les classes Platform::Collections::Vector et Platform::Collections::Map, qui implémentent des interfaces indépendantes du langage définies par WindowsRuntime. Les en-têtes amp sont utilisés pour exécuter des calculs sur le GPU. Ils n’ont pas d’équivalent dans Windows Runtime, mais cela ne pose pas de problème, car ils sont privés. En règle générale, pour des raisons de performance, vous devez utiliser du code C++ ISO et des bibliothèques standard en interne dans le composant; seule l’interface Windows Runtime doit être exprimée en types Windows Runtime.

## <a name="to-add-a-delegate-at-namespace-scope"></a>Pour ajouter un délégué à la portée espace de noms
Un délégué est une construction qui définit les paramètres et le type de retour pour les méthodes. Un événement est une instance d’un type délégué particulier, et toute méthode de gestionnaire d’événements qui s’abonne à l’événement doit présenter la signature spécifiée dans le délégué. Le code suivant définit un type délégué qui prend un int et retourne void. Ensuite, le code déclare un événement public de ce type; cela permet au code client de fournir les méthodes appelées lorsque l’événement est déclenché.

Ajoutez la déclaration delegate suivante à la portée espace de noms dans Class1.h, juste avant la déclaration Class1.

```cpp
public delegate void PrimeFoundHandler(int result);
```

Si le code ne s’aligne pas correctement lorsque vous le collez dans Visual Studio, il vous suffit d’appuyer sur Ctrl+K+D pour corriger la mise en retrait pour le fichier complet.

## <a name="to-add-the-public-members"></a>Pour ajouter les membres publics
La classe expose trois méthodes publiques et un événement public. La première méthode est synchrone, car elle s’exécute toujours très rapidement. Étant donné que les deux autres méthodes peuvent prendre un certain temps, elles sont asynchrones afin de ne pas bloquer le thread d’interface utilisateur. Ces méthodes retournent les interfaces IAsyncOperationWithProgress et IAsyncActionWithProgress. La première définit une méthode asynchrone qui retourne un résultat et la seconde une méthode asynchrone qui retourne void. Ces interfaces permettent également au code client de recevoir des mises à jour sur la progression de l’opération.

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## <a name="to-add-the-private-members"></a>Pour ajouter les membres privés
La classe contient trois membres privés: deux méthodes d’assistance pour les calculs numériques et un objet CoreDispatcher utilisé pour marshaler les appels d’événements des threads de travail vers le thread d’interface utilisateur.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

## <a name="to-add-the-header-and-namespace-directives"></a>Pour ajouter les directives d’en-tête et d’espace de noms
Dans Class1.cpp, ajoutez ces directives #include:

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

Ajoutez ensuite ces instructions using pour récupérer les espaces de noms requis:

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>Pour ajouter l’implémentation pour ComputeResult
Dans Class1.cpp, ajoutez l’implémentation de méthode suivante. Cette méthode s’exécute de façon synchrone sur le thread appelant, mais elle est très rapide, car elle utilise C++ AMP pour paralléliser le calcul sur le GPU. Pour plus d’informations, consultez l’article Vue d’ensemble de C++ AMP. Les résultats sont ajoutés à un type concret Platform::Collections::Vector<T>, qui est implicitement converti en Windows::Foundation::Collections::IVector<T> quand il est retourné.

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/en-us/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>Pour ajouter l’implémentation pour GetPrimesOrdered et sa méthode d’assistance
Dans Class1.cpp, ajoutez les implémentations pour GetPrimesOrdered et la méthode d’assistance is_prime. GetPrimesOrdered utilise une classe concurrent_vector et une boucle de fonction parallel_for afin de diviser le travail et d’utiliser les ressources maximales de l’ordinateur sur lequel le programme est exécuté pour produire des résultats. Une fois que les résultats sont calculés, stockés et triés, ils sont ajoutés à Platform::Collections::Vector<T> et retournés en tant que Windows::Foundation::Collections::IVector<T> au code client.

Notez le code du rapporteur de progression, qui permet au client de connecter une barre de progression ou toute autre interface utilisateur indiquant à l’utilisateur combien de temps l’opération va encore prendre. Le rapport de progression a un coût. Un événement doit être déclenché du côté du composant et géré sur le thread d’interface utilisateur, et la valeur de progression doit être stockée à chaque itération. Un moyen de réduire le coût consiste à limiter la fréquence à laquelle un événement de progression est déclenché. Si le coût est encore excessif, ou si vous ne pouvez pas estimer la longueur de l’opération, envisagez d’utiliser une boucle de progression afin d’indiquer qu’une opération est en cours sans afficher le temps restant avant la fin.

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## <a name="to-add-the-implementation-for-getprimesunordered"></a>Pour ajouter l’implémentation pour GetPrimesUnordered
La dernière étape pour créer le composant C++ consiste à ajouter l’implémentation pour GetPrimesUnordered dans Class1.cpp. Cette méthode retourne chaque résultat dès qu’il est trouvé, sans attendre que tous les résultats soient trouvés. Chaque résultat est retourné dans le gestionnaire d’événements et affiché dans l’interface utilisateur en temps réel. Là encore, notez qu’un rapporteur de progression est utilisé. Cette méthode utilise également la méthode d’assistance is_prime.

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## <a name="creating-a-javascript-client-app"></a>Création d’une application cliente JavaScript
Si vous souhaitez simplement créer un client C#, vous pouvez ignorer cette section.

## <a name="to-create-a-javascript-project"></a>Pour créer un projet JavaScript
Dans l’Explorateur de solutions, ouvrez le menu contextuel du nœud Solution, puis choisissez **Ajouter &gt; Nouveau projet**.

Développez JavaScript (il peut être imbriqué dans **Autres langages**) et choisissez **Application vide (Windows universel)**.

Acceptez le nom par défaut, App1, en cliquant sur le bouton **OK**.

Ouvrez le menu contextuel du nœud de projet App1, puis choisissez **Définir comme projet de démarrage**.

Ajoutez une référence de projet à WinRT_CPP :

Ouvrez le menu contextuel du nœud Références, puis choisissez **Ajouter une référence**.

Dans le volet gauche de la boîte de dialogue Gestionnaire de références, sélectionnez **Projets**, puis **Solution**.

Dans le volet central, sélectionnez WinRT_CPP, puis cliquez sur le bouton **OK**.

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>Pour ajouter le code HTML qui appelle les gestionnaires d’événements JavaScript
Collez ce code HTML dans le nœud <body> de la page default.html:

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## <a name="to-add-styles"></a>Pour ajouter des styles
Dans le fichier default.css, supprimez le style body, puis ajoutez ces styles:

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>Pour ajouter les gestionnaires d’événements JavaScript qui appellent le composant DLL
Ajoutez les fonctions suivantes à la fin du fichier default.js. Ces fonctions sont appelées lorsque les boutons de la page principale sont sélectionnés. Notez que JavaScript active la classe C++, puis appelle ses méthodes et utilise les valeurs de retour pour remplir les étiquettes HTML.

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

Ajoutez du code pour ajouter les détecteurs d’événements en remplaçant l’appel existant à WinJS.UI.processAll dans app.onactivated dans le fichier default.js par le code suivant, qui implémente l’inscription d’événements dans un bloc then. Pour obtenir une explication détaillée, consultez l’article Créer une application «Hello World» (JS).

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

Appuyez sur F5 pour exécuter l’application.

## <a name="creating-a-c-client-app"></a>Création d’une application cliente C#

## <a name="to-create-a-c-project"></a>Pour créer un projet C#
Dans l’Explorateur de solutions, ouvrez le menu contextuel du nœud Solution, puis choisissez **Ajouter &gt; Nouveau projet**.

Développez Visual C# (il peut être imbriqué dans **Autres langages**), sélectionnez **Windows** puis **Universel** dans le volet gauche, puis sélectionnez **Application vide** dans le volet central.

Nommez cette application CS_Client, puis cliquez sur le bouton **OK**.

Ouvrez le menu contextuel du nœud de projet CS_Client, puis choisissez **Définir comme projet de démarrage**.

Ajoutez une référence de projet à WinRT_CPP :

Ouvrez le menu contextuel du nœud **Références**, puis choisissez **Ajouter une référence**.

Dans le volet gauche de la boîte de dialogue **Gestionnaire de références**, sélectionnez **Projets**, puis **Solution**.

Dans le volet central, sélectionnez WinRT_CPP, puis cliquez sur le bouton **OK**.

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>Pour ajouter le code XAML qui définit l’interface utilisateur
Dans le fichier MainPage.xaml, copiez le code suivant dans l’élément Grid.

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## <a name="to-add-the-event-handlers-for-the-buttons"></a>Pour ajouter les gestionnaires d’événements pour les boutons
Dans l’Explorateur de solutions, ouvrez MainPage.xaml.cs. (Ce fichier pourrait se trouver sous MainPage.xaml.) Ajoutez une directive using pour System.Text, puis ajoutez le gestionnaire d’événements pour le calcul de logarithme dans la classe MainPage.

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

Ajoutez le gestionnaire d’événements pour le résultat ordonné:

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

Ajoutez le gestionnaire d’événements pour le résultat non ordonné, et pour le bouton qui efface les résultats afin de pouvoir exécuter à nouveau le code.

```csharp
private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (unordered): ");
    PrimesUnOrderedResult.Text = sb.ToString();

    // primeFoundEvent is a user-defined event in nativeObject
    // It passes the results back to this thread as they are produced
    // and the event handler that we define here immediately displays them.
    nativeObject.primeFoundEvent += (n) =>
    {
        sb.Append(n.ToString()).Append(" ");
        PrimesUnOrderedResult.Text = sb.ToString();
    };

    // Call the async method.
    var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

    // Provide a handler for the Progress event that the asyncResult
    // object fires at regular intervals. This handler updates the progress bar.
    asyncResult.Progress += (asyncInfo, progress) =>
        {
            PrimesUnOrderedProgress.Value = progress;
        };
}

private void Clear_Button_Click(object sender, RoutedEventArgs e)
{
    PrimesOrderedProgress.Value = 0;
    PrimesUnOrderedProgress.Value = 0;
    PrimesUnOrderedResult.Text = "";
    PrimesOrderedResult.Text = "";
    Result1.Text = "";
}
```

## <a name="running-the-app"></a>Exécution de l’application
Sélectionnez le projet C# ou le projet JavaScript en tant que projet de démarrage en ouvrant le menu contextuel du nœud de projet dans l’Explorateur de solutions et en sélectionnant **Définir comme projet de démarrage**. Appuyez ensuite sur F5 pour une exécution avec débogage ou sur Ctrl+F5 pour une exécution sans débogage.

## <a name="inspecting-your-component-in-object-browser-optional"></a>Examen de votre composant dans l’Explorateur d’objets (facultatif)
Dans l’Explorateur d’objets, vous pouvez examiner tous les types Windows Runtime définis dans les fichiers.winmd. Cela inclut les types de l’espace de noms Platform et de l’espace de noms par défaut. Toutefois, étant donné que les types de l’espace de noms Platform::Collections sont définis dans le fichier d’en-tête collections.h et non dans un fichier winmd, ils n’apparaissent pas dans l’Explorateur d’objets.

## **<a name="to-inspect-a-component"></a>Pour examiner un composant**
Dans la barre de menus, choisissez **Afficher, Explorateur d’objets** (Ctrl+Alt+J).

Dans le volet gauche de l’Explorateur d’objets, développez le nœud WinRT\_CPP afin d’afficher les types et les méthodes définis dans votre composant.

## <a name="debugging-tips"></a>Conseils de débogage
Pour optimiser le débogage, téléchargez les symboles de débogage à partir des serveurs de symboles publics de Microsoft:

## **<a name="to-download-debugging-symbols"></a>Pour télécharger les symboles de débogage**
Dans la barre de menus, choisissez **Outils, Options**.

Dans la boîte de dialogue **Options**, développez **Débogage** et sélectionnez **Symboles**.

Sélectionnez **Serveurs de symboles Microsoft**, puis cliquez sur le bouton **OK**.

Le téléchargement des symboles peut prendre un certain temps la première fois. Pour accélérer les performances la prochaine fois que vous appuierez sur F5, spécifiez un répertoire local dans lequel mettre en cache les symboles.

Lorsque vous déboguez une solution JavaScript qui contient une DLL de composant, vous pouvez configurer le débogueur de manière à activer l’exécution pas à pas du script ou du code natif du composant, mais pas les deux à la fois. Pour changer ce paramètre, ouvrez le menu contextuel du nœud de projet JavaScript dans l’Explorateur de solutions, puis sélectionnez **Propriétés, Débogage, Type de débogueur**.

Veillez à sélectionner les fonctionnalités appropriées dans le concepteur de packages. Vous pouvez lancer le concepteur de packages en ouvrant le fichier Package.appxmanifest. Par exemple, si vous essayez d’accéder par programmation à des fichiers du dossier Images, veillez à activer la case à cocher **Bibliothèque d’images** dans le volet **Capacités** du concepteur de packages.

Si votre code JavaScript ne reconnaît pas les propriétés ou méthodes publiques du composant, assurez-vous que vous utilisez la casse mixte dans JavaScript. Par exemple, la méthode C++ `ComputeResult` doit être référencée sous la forme `computeResult` dans JavaScript.

Si vous supprimez un projet de composant Windows Runtime C++ dans une solution, vous devez également supprimer manuellement la référence de ce projet dans le projet JavaScript. Sinon, il ne sera plus possible d’effectuer d’opérations de débogage ou de génération. Si nécessaire, ajoutez ensuite une référence d’assembly à la DLL.

## <a name="related-topics"></a>Rubriquesconnexes
* [Création de composants Windows Runtime en C++/CX](creating-windows-runtime-components-in-cpp.md)
