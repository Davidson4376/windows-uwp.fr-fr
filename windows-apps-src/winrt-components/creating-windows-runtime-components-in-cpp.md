---
author: msatranjr
title: Création de composants Windows Runtime en C++
description: Cette rubrique montre comment utiliser C++ / CX pour créer un composant Windows Runtime, qui est un composant qui peut être appelé à partir d’une application Windows universelle générée à l’aide de c#, Visual Basic, C++ ou Javascript.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.author: misatran
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5515d0ed5dc6e200c7c4fc9a7785c993d4cab59
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536836"
---
# <a name="creating-windows-runtime-components-in-ccx"></a>Création de composants Windows Runtime en C++/CX
> [!NOTE]
> Cette rubrique a pour but de vous aider à maintenir votre application C++/CX. Mais nous vous recommandons d’utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour de nouvelles applications. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Pour savoir comment créer un composant Windows Runtime à l’aide de C++ / WinRT, voir [créer des événements en C++ / WinRT](../cpp-and-winrt-apis/author-events.md).

Cette rubrique montre comment utiliser C++ / CX pour créer un composant Windows Runtime, qui est un composant qui peut être appelé à partir d’une application Windows universelle générée à l’aide de c#, Visual Basic, C++ ou Javascript.

Il existe plusieurs raisons pour la création d’un composant Windows Runtime.
- obtenir les avantages en termes de performances qu’offre C++ dans les opérations complexes ou nécessitant de nombreuses ressources de calcul ;
- réutiliser le code déjà écrit et testé.

Lorsque vous générez une solution qui contient un projet JavaScript ou .NET et un projet de composant Windows Runtime, les fichiers projet JavaScript et la DLL compilée sont fusionnés dans un package qui peut être débogué en local dans le simulateur, ou à distance sur un périphérique attaché. Le projet seul peut aussi être distribué en tant que kit de développement logiciel (SDK) de l’extension. Pour plus d’informations, voir [Création d’un kit de développement logiciel (SDK)](https://msdn.microsoft.com/library/hh768146.aspx).

En règle générale, lorsque vous codez votre C + / composant CX, utilisez la bibliothèque C++ standard et les types intégrés, sauf à la limite de l’interface binaire abstraite (ABI) où vous transmettez des données vers et depuis du code dans un autre package .winmd. Il, utilisez les types Windows Runtime et la syntaxe spéciale qui C++ / CX prend en charge pour créer et manipuler ces types. En outre, dans votre C++ / CX de code, utilisez des types tels que delegate et event pour implémenter des événements qui peuvent être déclenchés à partir de votre composant et gérés dans JavaScript, Visual Basic, C++ ou c#. Pour plus d’informations sur C++ / syntaxe CX, voir [référence du langage Visual C++ (C++ / CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## <a name="casing-and-naming-rules"></a>Règles de casse et d’appellation

### <a name="javascript"></a>JavaScript
JavaScript respecte la casse. Par conséquent, vous devez suivre les conventions de casse indiquées ci-dessous :

-   Lorsque vous référencez des espaces de noms et classes C++, utilisez la même casse que celle utilisée du côté C++.
-   Lorsque vous appelez des méthodes, utilisez la casse mixte même si le nom de la méthode est en majuscules du côté C++. Par exemple, une méthode GetDate() C++ doit être appelée depuis JavaScript sous la forme getDate().
-   Les noms de classe et les noms d’espace de noms activables ne peuvent pas contenir de caractères UNICODE.

### <a name="net"></a>.NET
Les langages .NET suivent leurs règles de casse normales.

## <a name="instantiating-the-object"></a>Instanciation de l’objet
Seuls les types Windows Runtime peuvent franchir la limite ABI. Le compilateur déclenche une erreur si le composant a un type tel que std::wstring comme type de retour ou paramètre dans une méthode publique. Les extensions de composant Visual C++ (C++ / CX) types intégrés incluent les scalaires habituels, tels qu’int et double, ainsi que leurs équivalents typedef int32, float64 et ainsi de suite. Pour plus d’informations, voir [Système de types (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C++ / CX types intégrés, types de bibliothèques et types Windows Runtime
Une classe activable (appelée également classe ref) est une classe qui peut être instanciée depuis un autre langage comme JavaScript, C# ou Visual Basic. Pour être utilisable à partir d’un autre langage, un composant doit contenir au moins une classe activable.

Un composant Windows Runtime peut contenir plusieurs classes publiques activables, ainsi que des classes supplémentaires qui sont connues par le composant uniquement en interne. Appliquez l’attribut [WebHostHidden](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.webhosthiddenattribute.aspx) vers C++ / types CX qui ne sont pas censées être visibles par JavaScript.

Toutes les classes publiques doivent résider dans le même espace de noms racine, qui porte le même nom que le fichier de métadonnées du composant. Par exemple, une classe nommée A.B.C.MyClass peut être instanciée uniquement si elle est définie dans un fichier de métadonnées nommé A.winmd, A.B.winmd ou A.B.C.winmd. Le nom de la DLL ne doit pas obligatoirement correspondre à celui du fichier .winmd.

Le code client crée une instance du composant à l’aide du mot clé **new** (**New** en Visual Basic) comme pour n’importe quelle classe.

Une classe activable doit être déclarée comme **public ref class sealed**. Le mot clé **ref class** indique au compilateur de créer la classe comme un type compatible Windows Runtime, et le mot clé sealed spécifie que la classe ne peut pas être héritée. Windows Runtime ne prend pas en charge de modèle d’héritage généralisé actuellement; un modèle d’héritage limité prend en charge la création de contrôles XAML personnalisés. Pour plus d’informations, voir [Classes et structures de référence (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699870.aspx).

Pour C++ / CX, toutes les primitives numériques sont définies dans l’espace de noms par défaut. L’espace de noms [plateforme](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx) contient C++ / système de type de classes CX qui sont propres à Windows Runtime. Il s’agit des classes [Platform::String](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) et [Platform::Object](https://msdn.microsoft.com/library/windows/apps/xaml/hh748265.aspx). Les types de collection concrets tels que les classes [Platform::Collections::Map](https://msdn.microsoft.com/library/windows/apps/xaml/hh441508.aspx) et [Platform::Collections::Vector](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx) sont définis dans l’espace de noms [Platform::Collections](https://msdn.microsoft.com/library/windows/apps/xaml/hh710418.aspx). Les interfaces publiques implémentées par ces types sont définies dans [Windows::Foundation::Collections (espace de noms) (C++ /CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh441496.aspx). Il s’agit des types d’interfaces qui sont utilisés par JavaScript, C# et Visual Basic. Pour plus d’informations, voir [Système de types (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

## <a name="method-that-returns-a-value-of-built-in-type"></a>Méthode qui retourne une valeur de type intégré
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>Méthode qui retourne une structure de valeur personnalisée
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

Pour passer les structures de valeur définies par l’utilisateur à travers l’ABI, définissez un objet JavaScript qui a les mêmes membres que la structure de valeur qui est définie en C++ / CX. Vous pouvez ensuite passer cet objet comme argument pour C++ / méthode CX afin que l’objet soit implicitement converti en C++ / type CX.

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

Une autre approche consiste à définir une classe qui implémente IPropertySet (non affichée).

Dans les langages .NET, vous créez simplement une variable du type qui est définie dans le C + / composant CX.

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>Méthodes surchargées
C++ / classe ref publique de CX peut contenir des méthodes surchargées, mais la capacité de différencier les méthodes surchargées est limitée à JavaScript. Par exemple, il peut faire la différence entre les signatures suivantes :

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

Mais il ne peut pas faire la différence entre celles-ci :

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

En cas d’ambiguïté, assurez-vous que JavaScript appelle toujours une surcharge spécifique en appliquant l’attribut [Windows::Foundation::Metadata::DefaultOverload](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) à la signature de méthode dans le fichier d’en-tête.

Ce JavaScript appelle toujours la surcharge attribuée :

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
Les langages .NET reconnaissent les surcharges dans C++ / classe ref CX comme dans n’importe quelle classe .NET Framework.

## <a name="datetime"></a>DateTime
Dans le Windows Runtime, un objet [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/windows.foundation.datetime.aspx) est un entier signé 64 bits qui représente le nombre d’intervalles de 100 nanosecondes avant ou après le 1er janvier 1601. Il n’existe aucune méthode sur un objet Windows:Foundation::DateTime. À la place, chaque langage projette le contrôle DateTime de la manière qui lui est native : l’objet Date en JavaScript et les types System.DateTime et System.DateTimeOffset dans le .NET Framework.

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

Lorsque vous passez une valeur DateTime de C++ / CX à JavaScript, JavaScript accepte en tant que d’un objet Date et l’affiche par défaut en tant qu’une chaîne de date de longue durée.

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

Lorsqu’un langage .NET passe un System.DateTime à C++ / composant CX, la méthode l’accepte en tant que Windows::Foundation:: DateTime. Lorsque le composant passe un Windows::Foundation::DateTime à une méthode .NET Framework, la méthode l’accepte en tant que DateTimeOffset.

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>Collections et tableaux
Les collections sont toujours passées à travers la limite ABI sous forme de handles aux types Windows Runtime tels que Windows::Foundation::Collections::IVector^ et Windows::Foundation::Collections::IMap^. Par exemple, si vous retournez un handle à une Platform::Collections::Map, il convertit implicitement en Windows::Foundation::Collections::IMap^. Les interfaces de collection sont définies dans un espace de noms distinct à partir de la C + / classes CX qui fournissent des implémentations concrètes. Les langages JavaScript et .NET utilisent les interfaces. Pour plus d’informations, voir [Collections (C++/CX)](https://msdn.microsoft.com//library/windows/apps/hh700103.aspx) et [Array et WriteOnlyArray (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh700131.aspx).

## <a name="passing-ivector"></a>Passage d’IVector
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

Les langages .NET considèrent IVector&lt;T&gt; comme IList&lt;T&gt;.

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>Passage d’IMAP
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

Les langages .NET considèrent IMap comme IDictionary&lt;K, V&gt;.

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>Propriétés
Une classe ref publique en C++ / extensions de composant CX expose les données membres publiques sous forme de propriétés en utilisant le mot clé property. Le concept est identique aux propriétés .NET Framework. Une propriété triviale ressemble à des données membres, car ses fonctionnalités sont implicites. Une propriété non triviale a des accesseurs get et set explicites et une variable privée nommée qui est le «magasin de stockage» de la valeur. Dans cet exemple, le membre privé variable \_propertyAValue est le magasin de stockage de PropertyA. Une propriété peut déclencher un événement lorsque sa valeur change. Une application cliente peut s’inscrire pour recevoir cet événement.

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

Les langages .NET aux propriétés sur un C + native / objet CX comme ils le feraient sur un objet .NET Framework.

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>Délégués et événements
Un délégué est un type Windows Runtime qui représente un objet de fonction. Les délégués peuvent être utilisés en liaison avec des événements, des rappels et des appels de méthode asynchrones pour spécifier une action à exécuter ultérieurement. Tout comme un objet de fonction, le délégué fournit la sécurité de type en permettant au compilateur de vérifier le type de retour et les types de paramètre de la fonction. La déclaration d’un délégué ressemble à une signature de fonction, son implémentation ressemble à une définition de classe et son appel ressemble à un appel de fonction.

## <a name="adding-an-event-listener"></a>Ajout d’un écouteur d’événements
Vous pouvez utiliser le mot clé event pour déclarer un membre public d’un type de délégué spécifié. Le code client s’abonne à l’événement à l’aide des mécanismes standard fournis dans le langage particulier.

```cpp
public:
    event SomeHandler^ someEvent;
```

Cet exemple utilise le même code C++ que pour la section de propriétés précédente.

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

Dans les langages .NET, s’abonner à un événement dans un composant C++ revient à s’abonner à un événement dans une classe .NET Framework :

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>Ajout de plusieurs écouteurs d’événements pour un événement
JavaScript comporte une méthode addEventListener qui permet à plusieurs gestionnaires de s’abonner à un événement unique.

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

En C#, un nombre quelconque de gestionnaires d’événements peuvent s’abonner à l’événement à l’aide de l’opérateur +=, comme indiqué dans l’exemple précédent.

## <a name="enums"></a>Énumérations
Une énumération Windows Runtime en C++ / CX est déclarée à l’aide de la classe publique enum; Il ressemble à une énumération limitée en C++ standard.

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

Les valeurs enum sont passées entre C++ / CX et JavaScript sous forme d’entiers. Vous pouvez déclarer éventuellement un objet JavaScript qui contient les mêmes valeurs nommées que C++ / CX enum et à utiliser comme suit.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# et Visual Basic disposent tous les deux de la prise en charge des langages pour les énumérations. Ces langages considèrent une classe enum publique C++ de la même manière qu’une énumération .NET Framework.

## <a name="asynchronous-methods"></a>Méthodes asynchrones
Pour utiliser des méthodes asynchrones exposées par d’autres objets Windows Runtime, utilisez la [classe de tâche (runtime d’accès concurrentiel)](https://msdn.microsoft.com/library/hh750113.aspx). Pour plus d’informations, voir [Parallélisme des tâches (runtime d’accès concurrentiel)](https://msdn.microsoft.com/library/dd492427.aspx).

Pour implémenter des méthodes asynchrones en C++ / CX, utilisez la fonction [create\_async](https://msdn.microsoft.com/library/hh750102.aspx) qui est définie dans ppltasks.h. Pour plus d’informations, voir [Création d’opérations asynchrones en C++ / CX pour les applications UWP](https://msdn.microsoft.com/library/vstudio/hh750082.aspx). Pour obtenir un exemple, voir [procédure pas à pas: création d’un composant Windows Runtime de base en C++ / CX et appel de ce à partir de JavaScript ou c#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). Les langages .NET utilisent C++ / des méthodes asynchrones CX comme ils le feraient n’importe quelle méthode asynchrone qui est défini dans le .NET Framework.

## <a name="exceptions"></a>Exceptions
Vous pouvez lever n’importe quel type d’exception défini par Windows Runtime. Vous ne pouvez pas dériver des types personnalisés d’un type d’exception Windows Runtime. Toutefois, vous pouvez lever une exception COMException et fournir un HRESULT personnalisé, accessible par le code qui intercepte l’exception. Il n’existe aucun moyen de spécifier un message personnalisé dans une COMException.

## <a name="debugging-tips"></a>Conseils de débogage
Lorsque vous déboguez une solution JavaScript qui contient une DLL de composant, vous pouvez configurer le débogueur de manière à activer l’exécution pas à pas du script ou du code natif du composant, mais pas les deux à la fois. Pour changer ce paramètre, sélectionnez le projet JavaScript dans l’Explorateur de solutions, puis sélectionnez Propriétés, Débogage, Type de débogueur.

Veillez à sélectionner les fonctionnalités appropriées dans le concepteur de packages. Par exemple, si vous essayez d’ouvrir un fichier image de la bibliothèque Images de l’utilisateur à l’aide des API Windows Runtime, veillez à cocher la case Bibliothèque d’images du volet Capacités du concepteur de manifeste.

Si votre code JavaScript ne semble pas reconnaître les propriétés ou méthodes publiques du composant, assurez-vous que vous utilisez la casse mixte dans JavaScript. Par exemple, le c++ LogCalc / méthode CX doit être référencée sous la forme logCalc dans JavaScript.

Si vous supprimez C++ / CX Windows Runtime projet du composant à partir d’une solution, vous devez également manuellement supprimer la référence de projet à partir du projet JavaScript. Sinon, il ne sera plus possible d’effectuer d’opérations de débogage ou de génération. Si nécessaire, ajoutez ensuite une référence d’assembly à la DLL.

## <a name="related-topics"></a>Rubriques connexes
* [Procédure pas à pas: création d’un composant WindowsRuntime de base en C++/CX et appel de ce composant à partir de JavaScript ou C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)
