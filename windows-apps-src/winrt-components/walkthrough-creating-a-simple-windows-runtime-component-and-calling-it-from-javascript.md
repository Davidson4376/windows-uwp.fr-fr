---
author: msatranjr
title: Création d’un composant Windows Runtime simple et appel de ce composant à partir de JavaScript
description: Cette procédure pas à pas montre comment utiliser .NET Framework avec Visual Basic ou C# pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime, et comment appeler le composant à partir de votre application Windows universelle générée pour Windows à l’aide de JavaScript.
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49b9fe0833151155b11b7d7b796e395bb6a2ca7f
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5564658"
---
# <a name="walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript"></a>Procédure pas à pas: création d’un composant Windows Runtime simple et appel de ce composant à partir de JavaScript




Cette procédure pas à pas montre comment utiliser .NET Framework avec Visual Basic ou C# pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime, et comment appeler le composant à partir de votre application Windows universelle générée pour Windows à l’aide de JavaScript.

Visual Studio facilite l’ajout à votre application d’un composant Windows Runtime écrit en C# ou Visual Basic, et la création de types Windows Runtime que vous pouvez appeler à partir de JavaScript. En interne, vos types Windows Runtime peuvent utiliser toutes les fonctionnalités .NET Framework autorisées dans une application Windows universelle. (Pour plus d’informations, consultez [Création de composants Windows Runtime en c# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) et [.NET pour la vue d’ensemble des applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx).) En externe, les membres de votre type peuvent exposer uniquement les types Windows Runtime pour leurs paramètres et valeurs de retour. Lorsque vous générez votre solution, Visual Studio génère votre projet de composant Windows Runtime .NET Framework, puis exécute une étape de génération qui crée un fichier de métadonnées Windows (.winmd). Il s’agit de votre composant Windows Runtime, que Visual Studio inclut dans votre application.

> **Remarque**.NET Framework mappe automatiquement certains types .NET Framework couramment utilisés, tels que les types de données primitives et les types de collection, à leurs équivalents Windows Runtime. Ces types .NET Framework peuvent être utilisés dans l’interface publique d’un composant Windows Runtime et sont présentés aux utilisateurs du composant comme les types Windows Runtime correspondants. Consultez l’article [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

Cette procédure pas à pas décrit les tâches suivantes. Après avoir terminé la première section, qui permet d’installer l’application Windows avec JavaScript, vous pouvez aborder les sections restantes dans n’importe quel ordre.

## <a name="prerequisites"></a>Éléments requis :

-   Windows10
-   Microsoft Visual Studio2015 ou Microsoft Visual Studio Community2015

## <a name="creating-a-simple-windows-runtime-class"></a>Création d’une classe Windows Runtime simple


Cette section permet de créer une application Windows universelle générée pour Windows à l’aide de JavaScript et d’ajouter un projet de composant Windows Runtime Visual Basic ou C#. Elle explique comment définir un type Windows Runtime managé, créer une instance du type à partir de JavaScript, puis appeler les membres statiques et les membres d’instance. L’affichage de l’exemple d’application est volontairement simple afin de focaliser l’attention sur le composant. N’hésitez pas à l’améliorer.

1.  Dans Visual Studio, créez un projet JavaScript : dans la barre de menus, choisissez **Fichier &gt; Nouveau &gt; Projet**. Dans la section **Modèles installés** de la boîte de dialogue **Nouveau projet**, sélectionnez **JavaScript**, **Windows**, puis **Universel**. (Si Windows n’est pas disponible, vérifiez que vous utilisez Windows 8 ou une version ultérieure.) Choisissez le modèle **Application vide** et nommez le projet SampleApp.
2.  Créez le projet de composant : dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution SampleApp et choisissez **Ajouter**, puis **Nouveau projet** pour ajouter un projet en C# ou Visual Basic à la solution. Dans la section **Modèles installés** de la boîte de dialogue **Ajouter un nouveau projet**, sélectionnez **Visual Basic** ou **Visual C#**, **Windows**, puis **Universel**. Choisissez le modèle **Composant Windows Runtime** et nommez le projet **SampleComponent**.
3.  Remplacez le nom de la classe par **Example**. Notez que la classe est marquée comme **public sealed** par défaut (**Public NotInheritable** en Visual Basic). Toutes les classes Windows Runtime que vous exposez à partir de votre composant doivent être sealed.
4.  Ajoutez deux membres simples à la classe, une méthode **static** (méthode **Shared** en Visual Basic) et une propriété d’instance:

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  Facultatif: pour activer IntelliSense pour les membres récemment ajoutés, dans l’Explorateur de solutions, ouvrez le menu contextuel du projet SampleComponent, puis choisissez **Générer**.
6.  Dans l’Explorateur de solutions, dans le projet JavaScript, ouvrez le menu contextuel de **Références**, puis choisissez **Ajouter une référence** pour ouvrir le **Gestionnaire de références**. Sélectionnez **Projets**, puis **Solution**. Activez la case à cocher du projet SampleComponent et cliquez sur **OK** pour ajouter une référence.

## <a name="call-the-component-from-javascript"></a>Appeler le composant à partir de JavaScript


Pour utiliser le type Windows Runtime à partir de JavaScript, ajoutez le code suivant dans la fonction anonyme du fichier default.js (situé dans le dossier js du projet) fourni par le modèle Visual Studio. Il doit être placé après le gestionnaire d’événements app.oncheckpoint et avant à app.start.

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

Notez que la première lettre majuscule de chaque nom de membre est remplacée par une minuscule. Cette transformation fait partie de la prise en charge que JavaScript fournit pour permettre l’utilisation naturelle de Windows Runtime. Les espaces de noms et les noms de classe utilisent la casse Pascal. Les noms de membres utilisent la casse mixte, à l’exception des noms d’événements, qui sont en minuscules. Consultez l’article [Utilisation de Windows Runtime en JavaScript](https://msdn.microsoft.com/library/hh710230.aspx). Les règles de casse mixte peuvent prêter à confusion. Une série de majuscules initiales apparaît normalement en minuscules, mais si trois lettres majuscules sont suivies par une minuscule, seules les deux premières lettres apparaissent en minuscules : par exemple, un membre nommé IDStringKind apparaît comme idStringKind. Dans Visual Studio, vous pouvez générer votre projet de composant Windows Runtime, puis utiliser IntelliSense dans votre projet JavaScript pour voir la casse correcte.

De la même façon, .NET Framework fournit une prise en charge pour permettre l’utilisation naturelle de Windows Runtime en code managé. Ce sujet est abordé dans les sections suivantes de cet article et dans les articles de la [Création de composants Windows Runtime en c# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) et [Prise en charge de .NET Framework pour les applications UWP et Windows Runtime](https://msdn.microsoft.com/library/hh694558.aspx).

## <a name="create-a-simple-user-interface"></a>Créer une interface utilisateur simple


Dans votre projet JavaScript, ouvrez le fichier default.html et mettez à jour le corps comme illustré dans le code suivant. Ce code inclut l’ensemble complet de contrôles pour l’exemple d’application et spécifie les noms de fonctions pour les événements Click.

> **Remarque**lorsque vous exécutez tout d’abord l’application, seuls les boutons Basics1 et Basics2 sont pris en charge.

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

Dans votre projet JavaScript, dans le dossier css, ouvrez default.css. Modifiez la section body comme illustré et ajoutez des styles pour contrôler la disposition des boutons et le positionnement du texte de sortie.

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

Ajoutez maintenant le code d’inscription du détecteur d’événements en ajoutant une clause then à l’appel de processAll dans app.onactivated dans default.js. Remplacez la ligne de code existante qui appelle setPromise par le code suivant :

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

Cette méthode est plus pratique pour ajouter des événements aux contrôles HTML que l’ajout d’un gestionnaire d’événements Click directement dans le code HTML. Consultez l’article [Créer une application « Hello World » (JS)](https://msdn.microsoft.com/library/windows/apps/mt280216).

## <a name="build-and-run-the-app"></a>Générer et exécuter l’application


Avant de générer l’application, définissez la plateforme cible pour tous les projets sur ARM, x64 ou x86, en fonction de votre ordinateur.

Pour générer et exécuter la solution, appuyez sur la touche F5. (Si vous recevez un message d’erreur d’exécution indiquant que SampleComponent n’est pas défini, la référence au projet de bibliothèque de classes est manquante.)

Visual Studio commence par compiler la bibliothèque de classes, puis exécute une tâche MSBuild qui exécute [Winmdexp.exe (outil d’exportation de métadonnées Windows Runtime)](https://msdn.microsoft.com/library/hh925576.aspx) pour créer votre composant Windows Runtime. Le composant est inclus dans un fichier .winmd qui contient le code managé et les métadonnées Windows qui décrivent le code. WinMdExp.exe génère des messages d’erreur de build lorsque vous écrivez du code non valide dans un composant Windows Runtime et les messages d’erreur sont affichés dans l’IDE de Visual Studio. Visual Studio ajoute votre composant au package d’application (fichier .appx) pour votre application Windows universelle et génère le manifeste approprié.

Cliquez sur le bouton Basics 1 pour affecter la valeur de retour de la méthode statique GetAnswer à la zone de sortie, créer une instance de la classe Example et afficher la valeur de sa propriété SampleProperty dans la zone de sortie. La sortie est illustrée ici :

``` syntax
"The answer is 42."
0
```

Cliquez sur le bouton Basics 2 pour incrémenter la valeur de la propriété SampleProperty et afficher la nouvelle valeur dans la zone de sortie. Les types primitifs tels que les chaînes et les nombres peuvent être utilisés en tant que types de paramètre et types de retour. Ils peuvent par ailleurs être transmis entre le code managé et JavaScript. Étant donné que les nombres JavaScript sont enregistrés au format à virgule flottante double précision, ils sont convertis en types numériques .NET Framework.

> **Remarque**par défaut, vous pouvez définir des points d’arrêt uniquement dans votre code JavaScript. Pour déboguer votre code Visual Basic ou C#, consultez l’article Création de composants Windows Runtime enC# et VisualBasic.

 

Pour arrêter le débogage et fermer votre application, passez de l’application à Visual Studio et appuyez sur Maj+F5.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>Utilisation de Windows Runtime à partir de JavaScript et du code managé


Windows Runtime peut être appelé à partir de JavaScript ou du code managé. Les objets Windows Runtime peuvent être transmis de l’un à l’autre dans les deux sens et les événements peuvent être gérés d’un côté ou de l’autre. Toutefois, l’utilisation des types Windows Runtime dans les deux environnements diffère sur certains points, car JavaScript et .NET Framework ne prennent pas en charge Windows Runtime de la même manière. L’exemple suivant illustre ces différences à l’aide de la classe [Windows.Foundation.Collections.PropertySet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.propertyset.aspx). Dans cet exemple, vous créez une instance de la collection PropertySet en code managé et enregistrez un gestionnaire d’événements pour suivre les modifications apportées à la collection. Vous ajoutez ensuite du code JavaScript pour obtenir la collection, inscrire son propre gestionnaire d’événements et utiliser la collection. Enfin, vous ajoutez une méthode qui apporte des modifications à la collection à partir du code managé et présente la gestion d’une exception managée par JavaScript.

> **Important**dans cet exemple, l’événement est déclenché sur le thread d’interface utilisateur. Si vous déclenchez l’événement à partir d’un thread d’arrière-plan, par exemple dans un appel asynchrone, d’autres opérations seront nécessaires pour que JavaScript gère l’événement. Pour plus d’informations, voir [Déclenchement d’événements dans les composants Windows Runtime](raising-events-in-windows-runtime-components.md).

 

Dans le projet SampleComponent, ajoutez une nouvelle classe **public sealed** (classe **Public NotInheritable** en Visual Basic) nommée PropertySetStats. La classe enveloppe une collection PropertySet et gère son événement MapChanged. Le gestionnaire d’événements assure le suivi du nombre de modifications de chaque type qui se produisent et la méthode DisplayStats génère un rapport au format HTML. Notez l’instruction **using** supplémentaire (instruction **Imports** en Visual Basic); veillez à l’ajouter aux instructions **using** existantes plutôt que de les remplacer.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

Le Gestionnaire d’événements suit le modèle d’événement .NET Framework familier, à ceci près que l’expéditeur de l’événement (dans ce cas, l’objet PropertySet) est casté en le IObservableMap&lt;chaîne, objet&gt; interface (IObservableMap (Of String, Object) dans Visual Basic), qui est une instanciation de l’interface Windows Runtime [IObservableMap&lt;K, V&gt;](https://msdn.microsoft.com/library/windows/apps/br226050.aspx). (Vous pouvez caster l’expéditeur en son type si nécessaire.) En outre, les arguments d’événement sont présentés comme une interface plutôt qu’en tant qu’objet.

Dans le fichier default.js, ajoutez la fonction Runtime1 comme indiqué. Ce code crée un objet PropertySetStats, obtient sa collection PropertySet et ajoute son propre gestionnaire d’événements, la fonction onMapChanged, pour gérer l’événement MapChanged. Après avoir modifié la collection, runtime1 appelle la méthode DisplayStats pour afficher un récapitulatif des types de modifications.

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

La façon dont vous gérez les événements Windows Runtime en JavaScript est très différente de celle dont vous les gérez dans le code .NET Framework. Le gestionnaire d’événements JavaScript prend un seul argument. Lorsque vous affichez cet objet dans le débogueur Visual Studio, la première propriété est l’expéditeur. Les membres de l’interface d’argument d’événement apparaissent également directement sur cet objet.

Pour exécuter l’application, appuyez sur la touche F5. Si la classe n’est pas sealed, vous obtenez le message d’erreur « L’exportation du type unsealed "SampleComponent.Example" n’est pas prise en charge pour le moment. Marquez ce type comme sealed. »

Cliquez sur le bouton **Runtime 1**. Le gestionnaire d’événements affiche des modifications lorsque des éléments sont ajoutés ou modifiés. À la fin, la méthode DisplayStats est appelée pour produire un récapitulatif des nombres. Pour arrêter le débogage et fermer l’application, revenez à Visual Studio et appuyez sur Maj+F5.

Pour ajouter deux éléments supplémentaires à la collection PropertySet à partir du code managé, ajoutez le code suivant à la classe PropertySetStats :

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

Ce code met en évidence une autre différence dans l’utilisation des types Windows Runtime dans les deux environnements. Si vous tapez ce code vous-même, vous remarquerez qu’IntelliSense n’affiche pas la méthode insert que vous avez utilisée dans le code JavaScript. Au lieu de cela, il affiche la méthode Add qui apparaît en général sur des collections dans .NET Framework. En effet, certaines interfaces de collection souvent utilisées ont des noms différents mais des fonctionnalités semblables dans Windows Runtime et .NET Framework. Lorsque vous utilisez ces interfaces en code managé, elles s’affichent sous la forme de leurs équivalents .NET Framework. Ce sujet est abordé dans l’article [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md). Lorsque vous utilisez les mêmes interfaces en JavaScript, la seule différence par rapport à Windows Runtime est que les lettres majuscules au début des noms de membres sont remplacées par des minuscules.

Enfin, pour appeler la méthode AddMore avec la gestion des exceptions, ajoutez la fonction runtime2 à default.js.

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

Ajoutez le code d’inscription du gestionnaire d’événements de la même manière que précédemment.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

Pour exécuter l’application, appuyez sur la touche F5. Cliquez sur **Runtime 1**, puis sur **Runtime 2**. Le gestionnaire d’événements JavaScript indique la première modification apportée à la collection. La deuxième modification, toutefois, présente une clé dupliquée. Les utilisateurs des dictionnaires .NET Framework s’attendent à ce que la méthode Add lève une exception, et c’est ce qui se produit. JavaScript gère l’exception .NET Framework.

> **Remarque**vous ne pouvez pas afficher le message de l’exception à partir du code JavaScript. Le texte du message est remplacé par une trace de la pile. Pour plus d’informations, consultez la section «Levée d’exceptions» dans l’article Création de composants Windows Runtime enC# et VisualBasic.

En revanche, lorsque JavaScript appelle la méthode insert avec une clé dupliquée, la valeur de l’élément est modifiée. Cette différence de comportement est due à la différence de prise en charge de Windows Runtime par JavaScript et .NET Framework, comme expliqué dans l’article [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

## <a name="returning-managed-types-from-your-component"></a>Retour des types managés à partir de votre composant


Comme indiqué précédemment, vous pouvez transmettre librement des types Windows Runtime natifs dans les deux sens entre votre code JavaScript et votre code C# ou Visual Basic. Le plus souvent, les noms de types et les noms de membres sont identiques dans les deux cas (à ceci près que les noms de membres commencent par une lettre minuscule en JavaScript). Cependant, dans la section précédente, la classe PropertySet semblait présenter des membres différents en code managé. (Par exemple, vous avez appelé la méthode insert en JavaScript et la méthode Add dans le code .NET Framework.) Cette section examine l’impact de ces différences sur les types .NET Framework transmis à JavaScript.

En plus de retourner les types Windows Runtime que vous avez créés dans votre composant ou transmis à votre composant à partir de JavaScript, vous pouvez retourner un type managé, créé en code managé, à JavaScript comme s’il s’agissait du type Windows Runtime correspondant. Même dans le premier exemple simple d’une classe Windows Runtime, les paramètres et les types de retour des membres étaient des types primitifs Visual Basic ou C#, qui sont des types .NET Framework. Afin d’illustrer cela pour les collections, ajoutez le code suivant à la classe Example pour créer une méthode qui retourne un dictionnaire générique de chaînes, indexé par des entiers:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

Notez que le dictionnaire doit être retourné comme une interface implémentée par [Dictionary&lt;TKey, TValue&gt;](https://msdn.microsoft.com/library/xfhwa508.aspx) et qui mappe vers une interface Windows Runtime. Dans ce cas, l’interface est IDictionary&lt;int, string&gt; (IDictionary(Of Integer, String) en Visual Basic). Lorsque le type Windows Runtime IMap&lt;int, string&gt; est transmis au code managé, il apparaît sous la forme IDictionary&lt;int, string&gt;. L’inverse est également vrai lorsque le type managé est transmis à JavaScript.

**Important**lorsqu’un type managé implémente plusieurs interfaces, JavaScript utilise l’interface qui apparaît en premier dans la liste. Par exemple, si vous retournez Dictionary&lt;int, string&gt; au code JavaScript, il apparaît comme IDictionary&lt;int, string&gt;, quelle que soit l’interface que vous spécifiez comme type de retour. Cela signifie que si la première interface n’inclut pas un membre qui apparaît sur les interfaces ultérieures, ce membre n’est pas visible pour JavaScript.

 

Pour tester la nouvelle méthode et utiliser le dictionnaire, ajoutez les fonctions returns1 et returns2 à default.js :

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

Ajoutez le code d’inscription d’événement au même bloc then que l’autre code d’inscription d’événement :

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

Plusieurs éléments intéressants peuvent être observés à propos de ce code JavaScript. Tout d’abord, il contient une fonction showMap permettant d’afficher le contenu du dictionnaire au format HTML. Dans le code de showMap, notez le modèle d’itération. Dans .NET Framework, il n’existe aucune méthode First sur l’interface générique IDictionary et la taille est retournée par une propriété Count plutôt que par une méthode Size. Pour JavaScript, IDictionary&lt;int, string&gt; semble être le type Windows Runtime IMap&lt;int, string&gt;. (Consultez l’interface [IMap&lt;K,V&gt;](https://msdn.microsoft.com/library/windows/apps/br226042.aspx).)

Dans la fonction returns2, comme dans les exemples précédents, JavaScript appelle la méthode Insert (insert en JavaScript) pour ajouter des éléments au dictionnaire.

Pour exécuter l’application, appuyez sur la touche F5. Pour créer et afficher le contenu initial du dictionnaire, cliquez sur le bouton **Returns 1**. Pour ajouter deux entrées supplémentaires au dictionnaire, cliquez sur le bouton **Returns 2**. Notez que les entrées sont affichées dans l’ordre d’insertion, comme on peut s’y attendre avec Dictionary&lt;TKey, TValue&gt;. Pour les trier, vous pouvez retourner SortedDictionary&lt;int, string&gt; à partir de GetMapOfNames. (La classe PropertySet utilisée dans les exemples précédents présente une organisation interne différente de celle de Dictionary&lt;TKey, TValue&gt;.)

Naturellement, JavaScript n’est pas un langage fortement typé. L’utilisation de collections génériques fortement typées peut donc entraîner des résultats surprenants. Cliquez à nouveau sur le bouton **Returns 2**. JavaScript convertit obligeamment le « 7 » en 7 numérique et le 7 numérique stocké dans ct en chaîne. Il force également la chaîne « forty » à prendre la valeur « zero ». Mais ce n’est que le début. Cliquez sur le bouton **Returns 2** quelques fois de plus. En code managé, la méthode Add génère des exceptions de clé dupliquée, même si les valeurs ont été castées en types corrects. En revanche, la méthode Insert met à jour la valeur associée à une clé existante et retourne une valeur Boolean qui indique si une nouvelle clé a été ajoutée au dictionnaire. C’est la raison pour laquelle la valeur associée à la clé 7 change constamment.

Autre comportement inattendu : si vous transmettez une variable JavaScript non assignée en tant qu’argument de chaîne, vous obtenez la chaîne « undefined ». En résumé, soyez prudent lorsque vous transmettez des types de collection .NET Framework à votre code JavaScript.

> **Remarque**si vous disposez de grandes quantités de texte à concaténer, vous pouvez le faire plus efficace en déplaçant le code dans une méthode .NET Framework et à l’aide de la classe StringBuilder, comme illustré dans la fonction showMap.

Même si vous ne pouvez pas exposer vos propres types génériques à partir d’un composant Windows Runtime, vous pouvez retourner les collections génériques .NET Framework pour les classes Windows Runtime à l’aide, par exemple, du code suivant:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; implémente IList&lt;T&gt;, qui apparaît comme le type Windows Runtime IVector&lt;T&gt; en JavaScript.

## <a name="declaring-events"></a>Déclaration d’événements


Vous pouvez déclarer des événements à l’aide du modèle d’événement .NET Framework standard ou d’autres modèles utilisés par Windows Runtime. Comme .NET Framework prend en charge l’équivalence entre le délégué System.EventHandler&lt;TEventArgs&gt; et le délégué Windows Runtime EventHandler&lt;T&gt;, l’utilisation de EventHandler&lt;TEventArgs&gt; est une bonne solution pour implémenter le modèle .NET Framework standard. Pour en comprendre le fonctionnement, ajoutez les paires de classes suivantes au projet SampleComponent:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Lorsque vous exposez un événement dans Windows Runtime, la classe d’argument d’événement hérite de System.Object. Elle n’hérite pas de System.EventArgs, comme c’est le cas dans .NET Framework, car EventArgs n’est pas un type Windows Runtime.

Si vous déclarez des accesseurs d’événement personnalisés pour votre événement (mot clé **Custom** en Visual Basic), vous devez utiliser le modèle d’événement Windows Runtime. Voir [Événements et accesseurs d’événement personnalisés dans les composants Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md).

Pour gérer l’événement Test, ajoutez la fonction events1 à default.js. La fonction events1 crée une fonction de gestionnaire d’événements pour l’événement Test et appelle immédiatement la méthode OnTest pour déclencher l’événement. Si vous placez un point d’arrêt dans le corps du gestionnaire d’événements, vous pouvez voir que l’objet transmis au paramètre unique inclut l’objet source et les deux membres de TestEventArgs.

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

Ajoutez le code d’inscription d’événement au même bloc then que l’autre code d’inscription d’événement :

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>Exposition d’opérations asynchrones


.NET Framework offre un ensemble complet d’outils pour le traitement asynchrone et le traitement en parallèle en fonction de la classe Task et de la classe [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) générique. Pour exposer un traitement asynchrone basé sur les tâches dans un composant Windows Runtime, utilisez les interfaces Windows Runtime [IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/br205802.aspx) et [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/br205807.aspx). (Dans Windows Runtime, les opérations retournent des résultats, mais pas les actions.)

Cette section illustre une opération asynchrone annulable qui indique la progression et retourne des résultats. La méthode GetPrimesInRangeAsync utilise la classe [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) pour générer une tâche et pour connecter ses fonctionnalités d’annulation et de rapport de progression à un objet WinJS.Promise. Commencez par ajouter la méthode GetPrimesInRangeAsync à la classe Example:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

De par sa conception, GetPrimesInRangeAsync est un outil de recherche de nombres premiers simple. L’objectif principal est d’implémenter une opération asynchrone. La simplicité est donc importante et une implémentation lente constitue un avantage pour expliquer l’annulation. GetPrimesInRangeAsync trouve des nombres premiers en force brute : en divisant un candidat par tous les entiers qui sont inférieurs ou égaux à sa racine carrée, plutôt qu’en utilisant uniquement les nombres premiers. Exécution pas à pas de ce code :

-   Avant de commencer une opération asynchrone, effectuez des opérations de nettoyage, telles que la validation des paramètres et la levée d’exceptions pour les entrées non valides.
-   L’élément essentiel de cette implémentation est la méthode [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)&gt;), ainsi que le délégué qui est le seul paramètre de la méthode. Le délégué doit accepter un jeton d’annulation et une interface pour indiquer la progression et doit retourner une tâche démarrée qui utilise ces paramètres. Lorsque JavaScript appelle la méthode GetPrimesInRangeAsync, les étapes suivantes se produisent (l’ordre peut être différent de celui donné ici) :

    -   L’objet [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) fournit des fonctions permettant de traiter les résultats retournés, de réagir à l’annulation et de gérer les rapports de progression.
    -   La méthode AsyncInfo.Run crée une source d’annulation et un objet qui implémente l’interface IProgress&lt;T&gt;. Pour le délégué, elle transmet un jeton [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) à partir de la source d’annulation et l’interface [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx).

        > **Remarque**si l’objet Promise ne fournit pas de fonction pour réagir à l’annulation, AsyncInfo.Run transmet toujours un jeton annulable et l’annulation peut encore se produire. Si l’objet Promise ne fournit pas de fonction pour gérer les mises à jour de progression, AsyncInfo.Run fournit toujours un objet qui implémente IProgress&lt;T&gt;, mais ses rapports sont ignorés.

    -   Le délégué utilise la méthode [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](https://msdn.microsoft.com/library/hh160376.aspx)) pour créer une tâche démarrée qui utilise le jeton et l’interface de progression. Le délégué pour la tâche démarrée est fourni par une fonction lambda qui calcule le résultat souhaité. Nous y reviendrons plus en détail dans un instant.
    -   La méthode AsyncInfo.Run crée un objet qui implémente l’interface [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx), connecte le mécanisme d’annulation de Windows Runtime à la source du jeton et connecte la fonction de rapport de progression de l’objet Promise à l’interface IProgress&lt;T&gt;.
    -   L’interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt; est retournée à JavaScript.

-   La fonction lambda qui est représentée par la tâche démarrée ne prend aucun argument. Comme il s’agit d’une fonction lambda, elle a accès au jeton et à l’interface IProgress. Chaque fois qu’un nombre candidat est évalué, la fonction lambda :

    -   Vérifie si le point de pourcentage de progression suivant a été atteint. Si c’est le cas, la fonction lambda appelle la méthode IProgress&lt;T&gt;.Report et le pourcentage est transmis à la fonction spécifiée par l’objet Promise pour indiquer la progression.
    -   Utilise le jeton d’annulation pour lever une exception si l’opération a été annulée. Si la méthode [IAsyncInfo.Cancel](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel.aspx) (dont l’interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt; hérite) a été appelée, la connexion installée par la méthode AsyncInfo.Run garantit que le jeton d’annulation est informé.
-   Quand la fonction lambda retourne la liste de nombres premiers, cette dernière est transmise à la fonction spécifiée par l’objet WinJS.Promise pour traiter les résultats.

Pour créer la méthode Promise JavaScript et installer le mécanisme d’annulation, ajoutez les fonctions asyncRun et asyncCancel à default.js.

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

N’oubliez pas d’ajouter le code d’inscription d’événement de la même manière que précédemment.

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

En appelant la méthode asynchrone GetPrimesInRangeAsync, la fonction asyncRun crée un objet WinJS.Promise. La méthode then de l’objet utilise trois fonctions qui traitent les résultats retournés, réagissent aux erreurs (y compris à l’annulation) et gèrent les rapports de progression. Dans cet exemple, les résultats retournés sont imprimés dans la zone de sortie. L’annulation ou l’achèvement entraîne la réinitialisation des boutons permettant de lancer et d’annuler l’opération. Le rapport de progression met à jour le contrôle de progression.

La fonction asyncCancel appelle simplement la méthode cancel de l’objet WinJS.Promise.

Pour exécuter l’application, appuyez sur la touche F5. Pour démarrer l’opération asynchrone, cliquez sur le bouton **Async**. Les étapes suivantes dépendent de la rapidité de votre ordinateur. Si la barre de progression indique l’achèvement avant que vous ayez pu faire quoi que ce soit, augmentez la taille du nombre de départ transmis à GetPrimesInRangeAsync en le multipliant une ou plusieurs fois par dix. Vous pouvez affiner la durée de l’opération en augmentant ou en diminuant le nombre de nombres à tester. Cependant, l’ajout de zéros au milieu du nombre de départ aura plus d’impact. Pour annuler l’opération, cliquez sur le bouton **Cancel Async**.

## <a name="related-topics"></a>Rubriques connexes

* [.NET pour la vue d’ensemble des applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Procédure pas à pas : création d’un composant Windows Runtime simple et appel de ce composant à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
