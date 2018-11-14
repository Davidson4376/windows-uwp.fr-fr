---
author: msatranjr
title: Création de composants Windows Runtime enC# et VisualBasic
description: Depuis le .NET Framework 4.5, vous pouvez utiliser du code managé pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e3b9ed2d256fb9ea8d38690a703baf7fbd3e7f0
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6191506"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>Création de composants Windows Runtime en C# et Visual Basic
Depuis le .NET Framework 4.5, vous pouvez utiliser du code managé pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime. Vous pouvez utiliser votre composant dans les applications de plateforme Windows universelle (UWP) avec C++, JavaScript, Visual Basic ou C#. Cette rubrique présente les règles de création d’un composant et décrit quelques aspects de prise en charge de .NET Framework pour Windows Runtime. En règle générale, cette prise en charge est conçue pour être transparente pour les programmeurs .NET Framework. Toutefois, lorsque vous créez un composant à utiliser avec JavaScript ou C++, vous devez tenir compte des différences de prise en charge de Windows Runtime par ces langages.

Si vous créez un composant à utiliser uniquement dans les applications UWP avec Visual Basic ou C# et que le composant ne contient pas les contrôles UWP, utilisez le modèle **Bibliothèque de classes** à la place du modèle **Composant Windows Runtime**. Il existe moins de restrictions sur une bibliothèque de classes simple.

Cette rubrique contient les sections suivantes:

## <a name="declaring-types-in-windows-runtime-components"></a>Déclaration des types dans les composants Windows Runtime
En interne, les types Windows Runtime de composant peuvent utiliser n’importe quelle fonctionnalité .NET Framework autorisée dans une application Windows universelle. (Voir la vue d’ensemble [.NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx) pour plus d’informations.) En externe, les membres de vos types peuvent exposer uniquement des types Windows Runtime pour leurs paramètres et valeurs de retour. La liste suivante décrit les restrictions sur les types .NET Framework qui sont exposées à partir de composants Windows Runtime.

-   Les champs, paramètres et valeurs de retour de tous les types et membres publics de votre composant doivent être de type Windows Runtime.

    Cette restriction comprend les types Windows Runtime que vous créez, ainsi que les types qui sont fournis directement par Windows Runtime. Elle inclut également un certain nombre de types .NET Framework. L’inclusion de ces types fait partie de la prise en charge fournie par le .NET Framework pour permettre l’utilisation naturelle du Windows Runtime en code managé: votre code utilise les types .NET Framework familiers plutôt que les types Windows Runtime sous-jacents. Par exemple, vous pouvez utiliser les types primitifs .NET Framework tels que Int32 et Double, certains types fondamentaux tels que DateTimeOffset et Uri, et certains types d’interface générique comme IEnumerable&lt;T&gt; (IEnumerable(Of T) en Visual Basic) et IDictionary&lt;TKey,TValue&gt;, couramment utilisés. (Notez que les arguments de type de ces types génériques doivent être des types Windows Runtime). Ce sujet est abordé dans les sections des types de passer de Windows Runtime au code managé et passage managés types Windows Runtime, plus loin dans cette rubrique.

-   Les interfaces et classes publiques peuvent contenir des méthodes, propriétés et événements. Vous pouvez déclarer des délégués pour vos événements ou utiliser le délégué EventHandler&lt;T&gt;. Une classe ou interface publique ne peut pas:

    -   être générique ;
    -   implémenter une interface qui n’est pas une interface Windows Runtime ; (Toutefois, vous pouvez créer vos propres interfaces Windows Runtime et les implémenter.)
    -   dériver des types qui ne se trouvent pas dans le Windows Runtime, tels que System.Exception et System.EventArgs.
-   Tous les types publics doivent posséder un espace de noms racine qui correspond au nom de l’assembly, et le nom de l’assembly ne doit pas commencer par «Windows».

    > **Conseil**par défaut, les projets Visual Studio ont des noms d’espace de noms correspondant au nom de l’assembly. En Visual Basic, la déclaration d’espace de noms pour cet espace de noms par défaut n’est pas affichée dans votre code.

-   Les structures publiques ne peuvent pas avoir d’autres membres que les champs publics, et ces champs doivent être des types de valeur ou des chaînes.
-   Les classes publiques doivent être **sealed** (**NotInheritable** en Visual Basic). Si votre modèle de programmation nécessite le polymorphisme, vous pouvez créer une interface publique et implémenter cette interface sur les classes qui doivent être polymorphes.

## <a name="debugging-your-component"></a>Débogage de votre composant
Si votre application Windows universelle et votre composant sont tous deux créés avec du code managé, vous pouvez les déboguer en même temps.

Lorsque vous testez votre composant dans le cadre d’une application Windows universelle en C++, vous pouvez déboguer les codes managé et natif en même temps. La valeur par défaut est uniquement le code natif.

## **<a name="to-debug-both-native-c-code-and-managed-code"></a>Pour déboguer à la fois du code C++ natif et du code managé**
1.  Ouvrez le menu contextuel de votre projet Visual C++, puis choisissez **Propriétés**.
2.  Dans les pages de propriétés, sous **Propriétés de configuration**, choisissez **Débogage**.
3.  Choisissez **Type de débogueur**, et dans la zone de liste déroulante remplacez la valeur **Natif uniquement** par **Mixte (managé et natif)**. Choisissez **OK**.
4.  Définissez des points d’arrêt dans le code natif et le code managé.

Lorsque vous testez votre composant dans le cadre d’une application Windows universelle à l’aide de JavaScript, par défaut la solution est en mode débogage de JavaScript. Dans Visual Studio, vous ne pouvez pas déboguer du code JavaScript et du code managé en même temps.

## **<a name="to-debug-managed-code-instead-of-javascript"></a>Pour déboguer du code managé au lieu de JavaScript**
1.  Ouvrez le menu contextuel de votre projet JavaScript, puis choisissez **Propriétés**.
2.  Dans les pages de propriétés, sous **Propriétés de configuration**, choisissez **Débogage**.
3.  Choisissez **Type de débogueur**, et dans la zone de liste déroulante remplacez la valeur **Script uniquement** par **Managé uniquement**. Choisissez **OK**.
4.  Définissez des points d’arrêt dans le code managé et déboguez comme d’habitude.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Passage de types Windows Runtime au code managé
Comme mentionné précédemment dans la section Déclaration des types dans les composants Windows Runtime, certains types .NET Framework peuvent apparaître dans les signatures de membres des classes publiques. Cela fait partie de la prise en charge que le .NET Framework fournit pour permettre l’utilisation naturelle du Windows Runtime dans le code managé. Elle inclut des types primitifs et certaines classes et interfaces. Lorsque votre composant est utilisé à partir de JavaScript ou de code C++, il est important de savoir comment vos types .NET Framework s’affichent pour l’appelant. Voir [Procédure pas à pas : création d’un composant simple en C# ou Visual Basic et appel de ce composant depuis JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) pour obtenir des exemples avec JavaScript. Cette section aborde les types couramment utilisés.

Dans le .NET Framework, les types primitifs, comme la structure Int32 ont de nombreuses propriétés et méthodes utiles, telles que la méthode TryParse. En revanche, les structures et types primitifs dans le Windows Runtime ont uniquement des champs. Quand vous passez ces types au code managé, ils apparaissent comme des types .NET Framework, et vous pouvez utiliser les propriétés et méthodes des types .NET Framework comme vous le feriez normalement. La liste suivante récapitule les substitutions effectuées automatiquement dans l’IDE :

-   Pour les primitives Windows Runtime Int32, Int64, Single, Double, Boolean, String (collection non modifiable de caractères Unicode), Enum, UInt32, UInt64 et Guid, utilisez le type portant le même nom que dans l’espace de noms System.
-   Pour UInt8, utilisez System.Byte.
-   Pour Char16, utilisez System.Char.
-   Pour l’interface IInspectable, utilisez System.Object.

Si C# ou Visual Basic fournit un mot clé de langage pour ces types, vous pouvez utiliser ce mot clé à la place.

En plus des types primitifs, certains types Windows Runtime de base couramment utilisés apparaissent dans le code managé sous la forme de leurs équivalents .NET Framework. Par exemple, supposons que votre code JavaScript utilise la classe Windows.Foundation.Uri et que vous voulez le passer à une méthodeC# ou Visual Basic. Le type équivalent en code managé est la classe .NET Framework System.Uri, et c’est le type à utiliser pour le paramètre de méthode. Vous pouvez savoir quand un type Windows Runtime apparaît comme un type .NET Framework, car IntelliSense dans Visual Studio masque le type Windows Runtime lorsque vous écrivez du code managé et présente le type équivalent .NET Framework. (Généralement les deux types ont le même nom. Cependant, notez que la structure Windows.Foundation.DateTime apparaît dans le code managé en tant que System.DateTimeOffset et non pas System.DateTime.)

Pour certains types de collection couramment utilisés, le mappage est compris entre les interfaces qui sont implémentées par un type Windows Runtime et les interfaces implémentées par le type .NET Framework correspondant. Comme avec les types mentionnés ci-dessus, vous déclarez les types de paramètres à l’aide du type .NET Framework. Cela masque certaines différences entre les types et rend l’écriture du code .NET Framework plus naturelle. Le tableau suivant répertorie les types d’interface générique les plus courants, ainsi que d’autres classes et mappages d’interface courants. Pour obtenir la liste complète des types Windows Runtime que mappe le .NET Framework, voir les mappages .NET Framework des types Windows Runtime.

| Windows Runtime                                  | .NET Framework                                    |
|--------------------------------------------------|---------------------------------------------------|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
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

Lorsqu’un type implémente plusieurs interfaces, vous pouvez utiliser n’importe quelle interface qu’il implémente comme type de paramètre ou type de retour d’un membre. Par exemple, vous pouvez passer ou retourner un Dictionary&lt;int, string&gt; (Dictionary (Of Integer, String) en Visual Basic) comme IDictionary&lt;int, string&gt;, IReadOnlyDictionary&lt;int, string&gt;, ou IEnumerable&lt; System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt;.

**Important**JavaScript utilise l’interface qui s’affiche en premier dans la liste des interfaces implémentées par un type managé. Par exemple, si vous retournez Dictionary&lt;int, string&gt; au code JavaScript, il apparaît comme IDictionary&lt;int, string&gt;, quelle que soit l’interface que vous spécifiez comme type de retour. Cela signifie que si la première interface n’inclut pas un membre qui apparaît sur les interfaces ultérieures, ce membre n’est pas visible pour JavaScript.

Dans le Windows Runtime, IMap&lt;K, V&gt; et IMapView&lt;K, V&gt; sont itérés à l’aide de IKeyValuePair. Lorsque vous les passez à un code managé, ils apparaissent sous la forme IDictionary&lt;TKey, TValue&gt; et IReadOnlyDictionary&lt;TKey, TValue&gt;, donc naturellement vous utilisez System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt; pour les énumérer.

La façon dont les interfaces s’affichent dans le code managé affecte l’affichage des types implémentant ces interfaces. Par exemple, la classe PropertySet implémente IMap&lt;K, V&gt;, qui apparaît dans le code managé comme IDictionary&lt;TKey, TValue&gt;. PropertySet s’affiche comme si elle avait implémenté IDictionary&lt;TKey, TValue&gt; au lieu de IMap&lt;K, V&gt; ; par conséquent, dans le code managé elle semble avoir une méthode Add, qui se comporte comme la méthode Add sur les dictionnaires .NET Framework. Elle ne semble pas avoir de méthodeInsert. Vous pouvez consulter cet exemple dans la rubrique [procédure pas à pas: création d’un composant simple en c# ou Visual Basic et appel de ce à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Passage de types managés au Windows Runtime
Comme indiqué dans la section précédente, certains types Windows Runtime peuvent apparaître en tant que types .NET Framework dans les signatures des membres de votre composant, ou dans celles des membres Windows Runtime lorsque vous les utilisez dans l’IDE. Quand vous passez des types .NET Framework à ces membres ou les utilisez en tant que valeurs de retour des membres de votre composant, ils apparaissent dans le code de l’autre côté comme type Windows Runtime correspondant. Pour obtenir des exemples des effets que cela peut avoir quand votre composant est appelé à partir de JavaScript, consultez la section « Retour des types managés à partir de votre composant » dans [Procédure pas à pas : création d’un composant simple en C# ou Visual Basic et appel de ce composant depuis JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Méthodes surchargées
Dans le Windows Runtime, les méthodes peuvent être surchargées. Toutefois, si vous déclarez plusieurs surcharges avec le même nombre de paramètres, vous devez appliquer l’attribut [Windows.Foundation.Metadata.DefaultOverloadAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) à une seule de ces surcharges. Cette surcharge est la seule que vous pouvez appeler depuis JavaScript. Par exemple, dans le code suivant la surcharge qui prend **int** (**Integer** en Visual Basic) est la surcharge par défaut.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public string OverloadExample(string s)
> {
>     return s;
> }
> [Windows.Foundation.Metadata.DefaultOverload()]
> public int OverloadExample(int x)
> {
>     return x;
> }
> ```
> ```vb
> Public Function OverloadExample(ByVal s As String) As String
>     Return s
> End Function
> <Windows.Foundation.Metadata.DefaultOverload> _
> Public Function OverloadExample(ByVal x As Integer) As Integer
>     Return x
> End Function
> ```

 **Attention**JavaScript vous permet de passer n’importe quelle valeur à OverloadExample et force la valeur dans le type requis par le paramètre. Vous pouvez appeler OverloadExample avec «quarante-deux», «42» ou 42,3 mais toutes ces valeurs sont passées à la surcharge par défaut. La surcharge par défaut de l’exemple précédent retourne 0, 42 et 42, respectivement.

Vous ne pouvez pas appliquer l’attribut DefaultOverloadAttribute aux constructeurs. Tous les constructeurs d’une classe doivent avoir des numéros de paramètres différents.

## <a name="implementing-istringable"></a>Implémentation d’IStringable
Depuis Windows 8.1, le Windows Runtime comprend une interface IStringable dont la seule méthode, IStringable.ToString, fournit une prise en charge élémentaire de la mise en forme comparable à celle fournie par Object.ToString. Si vous choisissez d’implémenter IStringable dans un type public managé qui est exporté dans un composant Windows Runtime, les restrictions suivantes s’appliquent:

-   Vous pouvez définir l’interface IStringable uniquement dans une relation de type «la classe implémente», telle que le code suivant enC#:

    ```cs
    public class NewClass : IStringable
    ```

    Ou le code Visual Basic suivant:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Vous ne pouvez pas implémenter IStringable sur une interface.
-   Vous ne pouvez pas déclarer un paramètre de type IStringable.
-   IStringable ne peut pas être le type de retour d’une méthode, d’une propriété ou d’un champ.
-   Vous ne pouvez pas masquer votre implémentation IStringable à partir des classes de base en utilisant une définition de méthode telle que la suivante :

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    À la place, l’implémentation IStringable.ToString doit toujours remplacer l’implémentation de la classe de base. Vous pouvez masquer une implémentation ToString uniquement en l’appelant sur une instance de classe fortement typée.

Notez que dans diverses conditions, les appels à partir de code natif à un type managé qui implémente IStringable ou masque son implémentation ToString peuvent produire un comportement inattendu.

## <a name="asynchronous-operations"></a>Opérations asynchrones
Pour implémenter une méthode asynchrone dans votre composant, ajoutez « Async » à la fin du nom de méthode et retournez l’une des interfaces Windows Runtime qui représentent des opérations ou actions asynchrones : IAsyncAction, IAsyncActionWithProgress&lt;TProgress&gt;, IAsyncOperation&lt;TResult&gt; ou IAsyncOperationWithProgress&lt;TResult, TProgress&gt;.

Vous pouvez utiliser des tâches .NET Framework (la classe [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) générique et la classe [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) générique) pour implémenter votre méthode asynchrone. Vous devez retourner une tâche qui représente une opération en cours, par exemple une tâche qui est retournée à partir d’une méthode asynchrone écrite en C# ou Visual Basic, ou une tâche retournée à partir de la méthode [Task.Run](https://msdn.microsoft.com/library/system.threading.tasks.task.run.aspx). Si vous utilisez un constructeur pour créer la tâche, vous devez appeler sa méthode [Task.Start](https://msdn.microsoft.com/library/system.threading.tasks.task.start.aspx) avant de la retourner.

Une méthode qui utilise await (Await en Visual Basic) requiert le mot clé **async** (**Async** en Visual Basic). Si vous exposez une telle méthode à partir d’un composant Windows Runtime, appliquez le mot clé **async** au délégué que vous passez à la méthode Run.

Pour les actions et opérations asynchrones qui ne prennent pas en charge l’annulation ou le rapport de progression, vous pouvez utiliser la méthode d’extension [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) ou [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) pour encapsuler la tâche dans l’interface appropriée. Par exemple, le code suivant implémente une méthode asynchrone à l’aide de la méthode Task.Run&lt;TResult&gt; pour démarrer une tâche. La méthode d’extension AsAsyncOperation&lt;TResult&gt; renvoie la tâche en tant qu’opération asynchrone Windows Runtime.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return Task.Run<IList<string>>(async () =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     }).AsAsyncOperation();
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>      As IAsyncOperation(Of IList(Of String))
>
>     Return Task.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function).AsAsyncOperation()
> End Function
> ```

Le code JavaScript suivant montre comment la méthode peut être appelée en utilisant un objet [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx). La fonction qui est passée à la méthode then est exécutée lorsque l’appel asynchrone se termine. Le paramètre stringList contient la liste de chaînes qui est retournée par la méthode DownloadAsStringAsync et la fonction exécute tout traitement requis.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Pour les actions et opérations asynchrones qui prennent en charge l’annulation ou le rapport de progression, utilisez la classe [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) pour générer une tâche démarrée et pour connecter l’annulation et la progression des fonctionnalités de création de rapports de la tâche avec l’annulation et la progression des fonctionnalités de création de rapports de l’interface appropriée de Windows Runtime. Pour obtenir un exemple qui prend en charge l’annulation et le rapport de progression, voir [Procédure pas à pas : création d’un composant simple en C# ou Visual Basic et appel de ce composant depuis JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Notez que vous pouvez utiliser les méthodes de la classe AsyncInfo même si votre méthode asynchrone ne prend pas en charge l’annulation ou le rapport de progression. Si vous utilisez une fonction lambda Visual Basic ou une méthode anonyme C#, ne fournissez pas de paramètres pour le jeton et l’interface [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx). Si vous utilisez une fonction lambda en C#, fournissez un paramètre de jeton, mais ignorez-le. L’exemple précédent, qui a utilisé la AsAsyncOperation&lt;TResult&gt; méthode, se présente comme suit lorsque vous utilisez la [AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)) méthode surcharger à la place:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return AsyncInfo.Run<IList<string>>(async (token) =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     });
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>     As IAsyncOperation(Of IList(Of String))
>
>     Return AsyncInfo.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function)
> End Function
> ```

Si vous créez une méthode asynchrone qui peut également prendre en charge l’annulation ou le rapport de progression, ajoutez des surcharges qui n’ont pas de paramètres pour un jeton d’annulation ou l’interface IProgress&lt;T&gt;.

## <a name="throwing-exceptions"></a>Levée des exceptions
Vous pouvez lever n’importe quel type d’exception inclus dans les applications .NET pour Windows. Vous ne pouvez pas déclarer vos propres types d’exceptions publics dans un composant Windows Runtime, mais vous pouvez déclarer et lever des types non publics.

Si votre composant ne gère pas l’exception, une exception correspondante est levée dans le code qui appelle votre composant. La façon dont l’exception s’affiche pour l’appelant dépend de la méthode dont son langage prend en charge Windows Runtime.

-   Dans JavaScript, l’exception s’affiche en tant qu’objet dans lequel le message d’exception est remplacé par une trace de la pile. Lorsque vous déboguez votre application dans Visual Studio, vous pouvez voir le texte du message d’origine affiché dans la boîte de dialogue d’exception du débogueur identifié comme « Informations WinRT ». Vous ne pouvez pas accéder au texte du message d’origine à partir du code JavaScript.

    > **Conseil**actuellement, la trace de pile contient le type d’exception managé, mais nous ne recommandons pas analyser la trace pour identifier le type d’exception. Utilisez plutôt une valeur HRESULT comme décrit plus loin dans cette section.

-   En C++, l’exception s’affiche comme une exception de plateforme. Si la propriété HResult de l’exception managée peut être mappée à la valeur HRESULT d’une exception de plateforme spécifique, l’exception spécifique est utilisée ; dans le cas contraire, une exception [Platform::COMException](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) est levée. Le texte du message de l’exception managée n’est pas disponible pour le code C++. Si une exception de plateforme spécifique est levée, le texte du message par défaut pour ce type d’exception apparaît. Dans le cas contraire, aucun texte de message n’apparaît. Voir [Exceptions (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx).
-   En C# ou VisualBasic, l’exception est une exception managée normale.

Lorsque vous levez une exception de votre composant, vous pouvez permettre plus facilement à l’appelant JavaScript ou C++ de gérer l’exception en levant un type d’exception non public dont la valeur de la propriété HResult est spécifique à votre composant. Le HRESULT est disponible pour un appelant JavaScript via la propriété Number de l’objet d’exception et pour un appelant C++ via la propriété [COMException::HResult](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx).

> **Remarque**utiliser une valeur négative pour votre HRESULT. Une valeur positive est interprétée comme une réussite et aucune exception n’est levée dans l’appelant JavaScript ou C++.

## <a name="declaring-and-raising-events"></a>Déclaration et déclenchement des événements
Lorsque vous déclarez un type pour contenir les données de votre événement, dérivez de Object au lieu de EventArgs, car EventArgs n’est pas un type Windows Runtime. Utilisez [EventHandler&lt;TEventArgs&gt;](https://msdn.microsoft.com/library/db0etb8x.aspx) comme type de l’événement, et utilisez votre type d’argument d’événement comme argument de type générique. Déclenchez l’événement comme dans une application .NET Framework.

Lorsque votre composant Windows Runtime est utilisé à partir de JavaScript ou C++, l’événement suit le modèle d’événement Windows Runtime attendu par ces langages. Lorsque vous utilisez le composant à partir de C# ou Visual Basic, l’événement s’affiche en tant qu’événement .NET Framework ordinaire. Un exemple est fourni dans [Procédure pas à pas : création d’un composant simple en C# ou Visual Basic et appel de ce composant depuis JavaScript]().

Si vous implémentez les accesseurs d’événement personnalisés (si vous déclarez un événement avec le mot-clé **Custom** en Visual Basic), vous devez suivre le modèle d’événement Windows Runtime dans votre implémentation. Voir [Événements personnalisés et accesseurs d’événement dans les composants Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Notez que lorsque vous gérez l’événement à partir du codeC# ou VisualBasic, il apparaît toujours comme un événement .NETFramework ordinaire.

## <a name="next-steps"></a>Étapes suivantes
Une fois que vous avez créé un composant Windows Runtime pour votre propre usage, vous découvrirez peut-être que la fonctionnalité qu’il encapsule est utile à d’autres développeurs. Vous avez deux possibilités pour empaqueter un composant afin de le distribuer à d’autres développeurs. Voir [Distribution d’un composant Windows Runtime managé](https://msdn.microsoft.com/library/jj614475.aspx).

Pour plus d’informations sur les fonctionnalités de langage Visual Basic et C# ainsi que sur la prise en charge de .NET Framework pour Windows Runtime, voir [Informations de référence sur les langages Visual Basic et C#](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx).

## <a name="related-topics"></a>Rubriques connexes
* [.NET pour la vue d’ensemble des applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Procédure pas à pas : création d’un composant Windows Runtime simple et appel de ce composant à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
