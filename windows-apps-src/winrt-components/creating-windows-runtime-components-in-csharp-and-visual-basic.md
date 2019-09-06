---
title: Windows Runtime composants avec C# et Visual Basic
description: À compter de .NET 4,5, vous pouvez utiliser du code managé pour créer vos propres types de Windows Runtime, empaquetés dans un composant Windows Runtime.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 351d59cbecd0941cdc6218d02672b2a679cf3fce
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393725"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>Windows Runtime composants avec C# et Visual Basic

Vous pouvez utiliser du code managé pour créer vos propres types de Windows Runtime et les empaqueter dans un composant Windows Runtime. Vous pouvez utiliser votre composant dans les applications plateforme Windows universelle (UWP) écrites en C++, JavaScript, Visual Basic ou. C# Cette rubrique décrit les règles de création d’un composant, et présente certains aspects de la prise en charge de .NET pour le Windows Runtime. En général, la prise en charge est conçue pour être transparente pour le programmeur .NET. Toutefois, lorsque vous créez un composant à utiliser avec JavaScript ou C++, vous devez tenir compte des différences de prise en charge de Windows Runtime par ces langages.

Si vous créez un composant à utiliser uniquement dans des applications UWP écrites en Visual Basic ou C#, et que le composant ne contient pas de contrôles UWP, onsider à l’aide du modèle Bibliothèque de **classes** au lieu du projet **Windows Runtime Component** modèle dans Microsoft Visual Studio. Il existe moins de restrictions sur une bibliothèque de classes simple.

## <a name="declaring-types-in-windows-runtime-components"></a>Déclaration de types dans les composants Windows Runtime

En interne, les types de Windows Runtime dans votre composant peuvent utiliser toutes les fonctionnalités .NET autorisées dans une application UWP. Pour plus d’informations, consultez [.net pour les applications UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0).

En externe, les membres de vos types peuvent exposer uniquement les types de Windows Runtime pour leurs paramètres et valeurs de retour. La liste suivante décrit les limitations sur les types .NET exposés à partir d’un composant Windows Runtime.

- Les champs, paramètres et valeurs de retour de tous les types et membres publics de votre composant doivent être de type Windows Runtime. Cette restriction comprend les types de Windows Runtime que vous créez ainsi que les types fournis par le Windows Runtime lui-même. Il comprend également un certain nombre de types .NET. L’inclusion de ces types fait partie de la prise en charge fournie par .net pour permettre l’utilisation naturelle de l’Windows Runtime en&mdash;code managé. votre code semble utiliser des types .net familiers à la place des types de Windows Runtime sous-jacents. Par exemple, vous pouvez utiliser des types primitifs .NET tels que **Int32** et **double**, certains types fondamentaux tels que **DateTimeOffset** et **URI**, et certains types d’interfaces génériques couramment utilisés tels que **IEnumerable&lt;T&gt;** (IEnumerable (Of T) dans Visual Basic) et **IDictionary&lt;TKey, TValue&gt;** . Notez que les arguments de type de ces types génériques doivent être des types Windows Runtime. Ce sujet est abordé dans les sections [transmission de Windows Runtime types au code managé](#passing-windows-runtime-types-to-managed-code) et [passage de types managés à la Windows Runtime](#passing-managed-types-to-the-windows-runtime), plus loin dans cette rubrique.

- Les interfaces et classes publiques peuvent contenir des méthodes, propriétés et événements. Vous pouvez déclarer des délégués pour vos événements ou utiliser le **délégué&lt;EventHandler&gt; T** . Une classe ou une interface publique ne peut pas :
    - être générique ;
    - Implémentez une interface qui n’est pas une interface Windows Runtime (Toutefois, vous pouvez créer vos propres interfaces Windows Runtime et les implémenter).
    - Dérivez des types qui ne sont pas dans le Windows Runtime, tels que **System. exception** et **System. EventArgs**.

- Tous les types publics doivent posséder un espace de noms racine qui correspond au nom de l’assembly, et le nom de l’assembly ne doit pas commencer par « Windows ».

    > **Conseil**. Par défaut, les projets Visual Studio ont des noms d’espaces de noms qui correspondent au nom de l’assembly. En Visual Basic, la déclaration d’espace de noms pour cet espace de noms par défaut n’est pas affichée dans votre code.

- Les structures publiques ne peuvent pas avoir d’autres membres que les champs publics, et ces champs doivent être des types de valeur ou des chaînes.
- Les classes publiques doivent être **sealed** (**NotInheritable** en Visual Basic). Si votre modèle de programmation requiert le polymorphisme, vous pouvez créer une interface publique et implémenter cette interface sur les classes qui doivent être polymorphes.

## <a name="debugging-your-component"></a>Débogage de votre composant

Si votre application UWP et votre composant sont générés avec du code managé, vous pouvez les déboguer tous les deux en même temps.

Lorsque vous testez votre composant dans le cadre d’une application UWP C++à l’aide de, vous pouvez déboguer du code managé et natif en même temps. La valeur par défaut est uniquement le code natif.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>Pour déboguer à la fois du code C++ natif et du code managé
1.  Ouvrez le menu contextuel de votre projet Visual C++, puis choisissez **Propriétés**.
2.  Dans les pages de propriétés, sous **Propriétés de configuration**, choisissez **Débogage**.
3.  Choisissez **Type de débogueur**, et dans la zone de liste déroulante remplacez la valeur **Natif uniquement** par **Mixte (managé et natif)** . Choisissez **OK**.
4.  Définissez des points d’arrêt dans le code natif et le code managé.

Lorsque vous testez votre composant dans le cadre d’une application UWP à l’aide de JavaScript, par défaut, la solution est en mode de débogage JavaScript. Dans Visual Studio, vous ne pouvez pas déboguer du code JavaScript et du code managé en même temps.

## <a name="to-debug-managed-code-instead-of-javascript"></a>Pour déboguer du code managé au lieu de JavaScript
1.  Ouvrez le menu contextuel de votre projet JavaScript, puis choisissez **Propriétés**.
2.  Dans les pages de propriétés, sous **Propriétés de configuration**, choisissez **Débogage**.
3.  Choisissez **Type de débogueur**, et dans la zone de liste déroulante remplacez la valeur **Script uniquement** par **Managé uniquement**. Choisissez **OK**.
4.  Définissez des points d’arrêt dans le code managé et déboguez comme d’habitude.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Passage de types Windows Runtime au code managé
Comme mentionné précédemment dans la section [déclaration des types dans les composants Windows Runtime](#declaring-types-in-windows-runtime-components), certains types .NET peuvent apparaître dans les signatures des membres des classes publiques. Cela fait partie de la prise en charge fournie par .NET pour permettre l’utilisation naturelle de l’Windows Runtime dans du code managé. Elle inclut des types primitifs et certaines classes et interfaces. Lorsque votre composant est utilisé à partir de JavaScript, C++ ou à partir du code, il est important de savoir comment vos types .net apparaissent à l’appelant. Pour obtenir des exemples avec JavaScript [, consultez Procédure pas à pas de création d’un C# composant Visual Basic Windows Runtime et appel de ce dernier à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) . Cette section aborde les types couramment utilisés.

Dans .NET, les types primitifs tels que la structure **Int32** ont de nombreuses propriétés et méthodes utiles, telles que la méthode **TryParse** . En revanche, les structures et types primitifs dans le Windows Runtime ont uniquement des champs. Quand vous transmettez ces types au code managé, ils semblent être des types .NET et vous pouvez utiliser les propriétés et les méthodes des types .NET comme vous le feriez normalement. La liste suivante récapitule les substitutions effectuées automatiquement dans l’IDE :

-   Pour les primitives de Windows Runtime **Int32**, **Int64**, **Single**, **double**, **Boolean**, **String** (une collection immuable de caractères Unicode), **enum**, **UInt32**, **UInt64**et **GUID** , utilisez le type du même nom dans l’espace de noms System.
-   Pour **UInt8**, utilisez **System. Byte**.
-   Pour **Char16**, utilisez **System. Char**.
-   Pour l’interface **IInspectable** , utilisez **System. Object**.

Si C# ou Visual Basic fournit un mot clé de langage pour l’un de ces types, vous pouvez utiliser le mot clé de langage à la place.

En plus des types primitifs, certains types de Windows Runtime de base, couramment utilisés, apparaissent dans du code managé comme leurs équivalents .NET. Par exemple, supposons que votre code JavaScript utilise la classe **Windows. Foundation. Uri** et que vous souhaitez le passer à C# une méthode ou Visual Basic. Le type équivalent dans le code managé est la classe .NET **System. Uri** , et il s’agit du type à utiliser pour le paramètre de la méthode. Vous pouvez savoir quand un type de Windows Runtime apparaît en tant que type .NET, car IntelliSense dans Visual Studio masque le type de Windows Runtime lorsque vous écrivez du code managé et présente le type .NET équivalent. (Généralement les deux types ont le même nom. Toutefois, Notez que la structure **Windows. Foundation. DateTime** apparaît dans le code managé sous la forme **System. DateTimeOffset** et non en tant que **System. DateTime**.)

Pour certains types de collections couramment utilisés, le mappage s’effectue entre les interfaces implémentées par un type Windows Runtime et les interfaces implémentées par le type .NET correspondant. Comme pour les types mentionnés ci-dessus, vous déclarez des types de paramètres à l’aide du type .NET. Cela masque certaines différences entre les types et rend l’écriture du code .NET plus naturelle.

Le tableau suivant répertorie les types d’interface générique les plus courants, ainsi que d’autres classes et mappages d’interface courants. Pour obtenir la liste complète des types de Windows Runtime mappés par .NET, consultez [mappages .net des types de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

| Windows Runtime                                  | .NET                                    |
|-|-|
| Interface iiterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

Lorsqu’un type implémente plusieurs interfaces, vous pouvez utiliser n’importe quelle interface qu’il implémente comme type de paramètre ou type de retour d’un membre. Par exemple, vous pouvez passer ou retourner un **dictionnaire&lt;int, String&gt;**  (**dictionary (Of Integer, String)** dans Visual Basic) As **IDictionary&lt;int, String&gt;** , **IReadOnlyDictionary int,String&gt;ou IEnumerable System. Collections. Generic. KeyValuePair TKey, TValue. &lt;** **&gt;&gt;&lt;&lt;**

> [!IMPORTANT]
> JavaScript utilise l’interface qui apparaît en premier dans la liste des interfaces implémentées par un type managé. Par exemple, si vous retournez **Dictionary&lt;int, String&gt;**  to JavaScript code, il apparaît comme **IDictionary&lt;int, String&gt;**  , quelle que soit l’interface que vous spécifiez comme type de retour. Cela signifie que si la première interface n’inclut pas un membre qui apparaît sur les interfaces ultérieures, ce membre n’est pas visible pour JavaScript.

Dans le Windows Runtime, **IMap&lt;k, v&gt;**  et **IMapView&lt;k, v&gt;**  sont itérés à l’aide de IKeyValuePair. Quand vous les transmettez à du code managé, elles apparaissent sous la forme **IDictionary&lt;TKey, TValue&gt;**  et **IReadOnlyDictionary&lt;TKey, TValue&gt;** . vous utilisez **donc naturellement System. Collections. Generic. KeyValuePair&lt;TKey, TValue&gt;**  pour les énumérer.

La façon dont les interfaces apparaissent dans le code managé affecte la façon dont les types qui implémentent ces interfaces apparaissent. Par exemple, la classe **PropertySet** implémente **IMap&lt;K, V&gt;** , qui apparaît dans le code managé **comme&lt;IDictionary TKey,&gt;TValue**. **PropertySet** s’affiche comme s’il implémentait **&lt;IDictionary TKey&gt; , TValue** au lieu de **&lt;IMap&gt;K, V**, donc dans le code managé, il semble avoir une méthode **Add** , qui se comporte comme la méthode **Add** sur les dictionnaires .net. Il ne semble pas avoir une méthode **Insert** . Vous pouvez voir cet exemple dans la rubrique [procédure pas à pas C# de création d’un composant ou Visual Basic Windows Runtime et appel de ce dernier à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Passage de types managés au Windows Runtime

Comme indiqué dans la section précédente, certains types de Windows Runtime peuvent apparaître en tant que types .NET dans les signatures des membres de votre composant, ou dans les signatures des membres Windows Runtime quand vous les utilisez dans l’IDE. Lorsque vous transmettez des types .NET à ces membres ou que vous les utilisez comme valeurs de retour des membres de votre composant, ils apparaissent dans le code de l’autre côté en tant que type de Windows Runtime correspondant. Pour obtenir des exemples des effets que cela peut avoir quand votre composant est appelé à partir de JavaScript, consultez la section « retour des types managés à partir de votre composant » dans [procédure pas à pas de création C# d’un composant Visual Basic Windows Runtime et appel de ce dernier à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Méthodes surchargées

Dans le Windows Runtime, les méthodes peuvent être surchargées. Toutefois, si vous déclarez plusieurs surcharges avec le même nombre de paramètres, vous devez appliquer l’attribut [**Windows. Foundation. Metadata. DefaultOverloadAttribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) à une seule de ces surcharges. Cette surcharge est la seule que vous pouvez appeler depuis JavaScript. Par exemple, dans le code suivant la surcharge qui prend **int** (**Integer** en Visual Basic) est la surcharge par défaut.

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> PRÉCIEUSE JavaScript vous permet de passer n’importe quelle valeur à **OverloadExample**et convertit la valeur en type requis par le paramètre. Vous pouvez appeler **OverloadExample** avec « 42 », « 42 » ou 42,3, mais toutes ces valeurs sont passées à la surcharge par défaut. La surcharge par défaut dans l’exemple précédent retourne 0, 42 et 42, respectivement.

Vous ne pouvez pas appliquer l’attribut **DefaultOverloadAttribut**e aux constructeurs. Tous les constructeurs d’une classe doivent avoir des numéros de paramètres différents.

## <a name="implementing-istringable"></a>Implémentation d’IStringable

À partir de Windows 8.1, le Windows Runtime inclut une interface **IStringable** dont la méthode unique, **IStringable. ToString**, fournit une prise en charge de base de la mise en forme comparable à celle fournie par **Object. ToString**. Si vous choisissez d’implémenter **IStringable** dans un type managé public qui est exporté dans un composant Windows Runtime, les restrictions suivantes s’appliquent :

-   Vous pouvez définir l’interface **IStringable** uniquement dans une relation « la classe implémente », par exemple le code suivant C#dans :

    ```cs
    public class NewClass : IStringable
    ```

    Ou le code Visual Basic suivant :

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Vous ne pouvez pas implémenter **IStringable** sur une interface.
-   Vous ne pouvez pas déclarer un paramètre de type **IStringable**.
-   **IStringable** ne peut pas être le type de retour d’une méthode, d’une propriété ou d’un champ.
-   Vous ne pouvez pas masquer votre implémentation **IStringable** à partir des classes de base à l’aide d’une définition de méthode telle que la suivante :

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Au lieu de cela, l’implémentation de **IStringable. ToString** doit toujours remplacer l’implémentation de la classe de base. Vous pouvez masquer une implémentation **ToString** uniquement en l’appelant sur une instance de classe fortement typée.

> [!NOTE]
> Dans diverses conditions, les appels de code natif à un type managé qui implémente **IStringable** ou masquent son implémentation **ToString** peuvent entraîner un comportement inattendu.

## <a name="asynchronous-operations"></a>Opérations asynchrones

Pour implémenter une méthode asynchrone dans votre composant, ajoutez « Async » à la fin du nom de la méthode et retournez l’une des interfaces Windows Runtime qui représentent des actions ou des opérations asynchrones : **IAsyncAction**, **IAsyncActionWithProgress&lt;TProgress&gt;** , **IAsyncOperation&lt;TResultou&gt;** **IAsyncOperationWithProgressTResult,&lt;TProgress &gt;** .

Vous pouvez utiliser des tâches .net (la classe de [**tâche**](/dotnet/api/system.threading.tasks.task) et la classe de [ **&lt;&gt; tâche**](/dotnet/api/system.threading.tasks.task-1) générique) pour implémenter votre méthode asynchrone. Vous devez retourner une tâche qui représente une opération en cours, telle qu’une tâche retournée à partir d’une méthode asynchrone C# écrite dans ou Visual Basic, ou une tâche retournée à partir de la méthode [Task. Run](/dotnet/api/system.threading.tasks.task.run) . Si vous utilisez un constructeur pour créer la tâche, vous devez appeler sa méthode [Task.Start](/dotnet/api/system.threading.tasks.task.start) avant de la retourner.

Une méthode qui utilise `await` (`Await` dans Visual Basic) requiert le `async` mot clé`Async` (dans Visual Basic). Si vous exposez une telle méthode à partir d’un composant `async` Windows Runtime, appliquez le mot clé au délégué que vous transmettez à la méthode **Run** .

Pour les actions et opérations asynchrones qui ne prennent pas en charge l’annulation ou le rapport de progression, vous pouvez utiliser la méthode d’extension [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) ou [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) pour encapsuler la tâche dans l’interface appropriée. Par exemple, le code suivant implémente une méthode asynchrone à l’aide de la méthode **Task&gt; . Run&lt;TResult** pour démarrer une tâche. La méthode d’extension **AsAsyncOperation&lt;TResult&gt;**  retourne la tâche sous la forme d’une opération asynchrone Windows Runtime.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

Le code JavaScript suivant montre comment la méthode peut être appelée à l’aide d’un objet [**WinJS.** ](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10)) promise. La fonction qui est passée à la méthode then est exécutée lorsque l’appel asynchrone se termine. Le paramètre stringList contient la liste des chaînes retournées par la méthode **DownloadAsStringAsync** , et la fonction effectue le traitement requis.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Pour les actions et opérations asynchrones qui prennent en charge l’annulation ou le rapport de progression, utilisez la classe [**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) pour générer une tâche démarrée et pour raccorder les fonctionnalités d’annulation et de rapport de progression de la tâche avec l’annulation et la progression. fonctionnalités de création de rapports de l’interface de Windows Runtime appropriée. Pour obtenir un exemple qui prend en charge à la fois l’annulation et le rapport de progression, consultez [procédure pas à pas de création d’un C# composant Visual Basic Windows Runtime et appel de ce dernier à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Notez que vous pouvez utiliser les méthodes de la classe **AsyncInfo** même si votre méthode asynchrone ne prend pas en charge l’annulation ou le rapport de progression. Si vous utilisez une Visual Basic fonction lambda ou une C# méthode anonyme, ne fournissez pas de paramètres pour le jeton et l’interface [IProgress&lt;t&gt; ](https://docs.microsoft.com/dotnet/api/system.iprogress-1?redirectedfrom=MSDN) . Si vous utilisez une fonction lambda en C#, fournissez un paramètre de jeton, mais ignorez-le. L’exemple précédent, qui utilisait la&lt;méthode&gt; AsAsyncOperation TResult, se présente comme suit quand vous utilisez [**AsyncInfo.&lt;Run&gt;TResult (&lt;Func CancellationToken, Task&lt;TResult)àlaplace.&gt;&gt;** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime?redirectedfrom=MSDN)

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

Si vous créez une méthode asynchrone qui prend éventuellement en charge l’annulation ou le rapport de progression, envisagez d’ajouter des surcharges qui n’ont pas de paramètres pour un jeton d’annulation ou l’interface **IProgress&lt;t&gt;**  .

## <a name="throwing-exceptions"></a>Levée des exceptions

Vous pouvez lever n’importe quel type d’exception inclus dans les applications .NET pour Windows. Vous ne pouvez pas déclarer vos propres types d’exceptions publics dans un composant Windows Runtime, mais vous pouvez déclarer et lever des types non publics.

Si votre composant ne gère pas l’exception, une exception correspondante est levée dans le code qui appelle votre composant. La façon dont l’exception s’affiche pour l’appelant dépend de la méthode dont son langage prend en charge Windows Runtime.

-   Dans JavaScript, l’exception s’affiche en tant qu’objet dans lequel le message d’exception est remplacé par une trace de la pile. Lorsque vous déboguez votre application dans Visual Studio, vous pouvez voir le texte du message d’origine affiché dans la boîte de dialogue d’exception du débogueur identifié comme « Informations WinRT ». Vous ne pouvez pas accéder au texte du message d’origine à partir du code JavaScript.

    > **Conseil**. Actuellement, la trace de la pile contient le type d’exception managée, mais nous vous déconseillons d’analyser la trace pour identifier le type d’exception. Utilisez plutôt une valeur HRESULT comme décrit plus loin dans cette section.

-   En C++, l’exception s’affiche comme une exception de plateforme. Si la propriété HResult de l’exception managée peut être mappée à la valeur HRESULT d’une exception de plateforme spécifique, l’exception spécifique est utilisée. dans le cas contraire, une exception [**Platform :: COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class) est levée. Le texte du message de l’exception managée n’est pas disponible pour le code C++. Si une exception de plateforme spécifique est levée, le texte du message par défaut pour ce type d’exception apparaît. Dans le cas contraire, aucun texte de message n’apparaît. Voir [Exceptions (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx).
-   En C# ou Visual Basic, l’exception est une exception managée normale.

Lorsque vous levez une exception de votre composant, vous pouvez permettre plus facilement à l’appelant JavaScript ou C++ de gérer l’exception en levant un type d’exception non public dont la valeur de la propriété HResult est spécifique à votre composant. HRESULT est disponible pour un appelant JavaScript via la propriété Number de l’objet exception et à un C++ appelant via la propriété [COMException :: HRESULT](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult) .

> [!NOTE]
> Utilisez une valeur négative pour votre HRESULT. Une valeur positive est interprétée comme une réussite et aucune exception n’est levée dans l’appelant JavaScript ou C++.

## <a name="declaring-and-raising-events"></a>Déclaration et déclenchement des événements

Lorsque vous déclarez un type pour contenir les données de votre événement, dérivez de Object au lieu de EventArgs, car EventArgs n’est pas un type Windows Runtime. Utilisez [**EventHandler&lt;TEventArgs&gt;** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1?redirectedfrom=MSDN) comme type de l’événement et utilisez votre type d’argument d’événement comme argument de type générique. Déclenchez l’événement comme vous le feriez dans une application .NET.

Lorsque votre composant Windows Runtime est utilisé à partir de JavaScript ou C++, l’événement suit le modèle d’événement Windows Runtime attendu par ces langages. Lorsque vous utilisez le composant à C# partir de ou Visual Basic, l’événement apparaît comme un événement .net ordinaire. Un exemple est fourni dans [procédure pas à pas C# de création d’un Visual Basic Windows Runtime composant, et appel de ce dernier à partir de JavaScript](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript).

Si vous implémentez les accesseurs d’événement personnalisés (si vous déclarez un événement avec le mot-clé **Custom** en Visual Basic), vous devez suivre le modèle d’événement Windows Runtime dans votre implémentation. Consultez [événements personnalisés et accesseurs d’événement dans Windows Runtime composants](custom-events-and-event-accessors-in-windows-runtime-components.md). Notez que lorsque vous gérez l’événement à C# partir de ou Visual Basic Code, il semble toujours être un événement .net ordinaire.

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez créé un composant Windows Runtime pour votre propre usage, vous découvrirez peut-être que la fonctionnalité qu’il encapsule est utile à d’autres développeurs. Vous avez deux possibilités pour empaqueter un composant afin de le distribuer à d’autres développeurs. Voir [Distribution d’un composant Windows Runtime managé](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140)).

Pour plus d’informations sur les C# fonctionnalités de Visual Basic et de langage, ainsi que sur la prise en charge de .net pour l’Windows Runtime, consultez [Visual Basic C# et référence du langage](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).

## <a name="related-topics"></a>Rubriques connexes
* [.NET pour les applications UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Procédure pas à pas C# de création d’un Visual Basic Windows Runtime composant, et appel de ce dernier à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
