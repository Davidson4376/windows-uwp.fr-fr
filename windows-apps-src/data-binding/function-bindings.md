---
description: L’extension de balisage permet aux fonctions à utiliser dans le balisage.
title: Fonctions dans x:Bind
ms.date: 04/26/2018
ms.topic: article
keywords: Windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 371b64a6161911242acd5b9abf97cfa7d2f05358
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7981361"
---
# <a name="functions-in-xbind"></a>Fonctions dans x:Bind

**Remarque**pour des informations générales sur l’utilisation des données de liaison dans votre application avec **{x: Bind}** (et pour une comparaison entre **{x: Bind}** et **{Binding}**), consultez [liaison de données en profondeur](https://msdn.microsoft.com/library/windows/apps/mt210946).

À compter de Windows10, version1607, **{x: Bind}** prend en charge l’utilisation d’une fonction comme niveau feuille du chemin de liaison. Cela permet de:

- Facilite la conversion de valeur
- Permet aux liaisons de dépendre de plus d’un paramètre

> [!NOTE]
> Pour utiliser des fonctions avec **{x:Bind}**, la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas utiliser des fonctions si votre application cible des versions antérieures de Windows10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Dans l’exemple suivant, l’arrière-plan et le premier plan de l’élément sont liés à des fonctions de conversion reposant sur le paramètre de couleur:

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Chemin de la fonction

Le chemin de la fonction est spécifié comme tout autre chemin de propriété et peut inclure des points (.), des indexeurs ou des casts pour localiser la fonction.

Des fonctions statiques peuvent être spécifiées en utilisant la syntaxe XMLNamespace:ClassName.MethodName. Par exemple, utilisez le ci-dessous syntaxe pour la liaison à des fonctions statiques dans le code-behind.

```xaml
<Page 
     xmlns:local="using:MyPage">
     ...
     <Grid x:Name="myGrid" Background="Black" >
        <TextBlock Foreground="{x:Bind local:GenerateAppropriateForeground(myGrid.Background)}" Text="Hello World!" />
    </Grid>
</Page>
```
```csharp
public class MyPage : Page
{
    public static GenerateAppropriateForeground(SolidColorBrush background)
    {
        //Implement static function
        ...
    }
}
```

Vous pouvez également utiliser les fonctions système directement dans le balisage pour accomplir des scénarios simples, comme la date de mise en forme, la mise en forme de texte, concaténations de texte, etc., par exemple:
```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyPage">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

Si le mode est OneWay/TwoWay, la détection de modification est réalisée sur le chemin de la fonction et la liaison est réévaluée si des modifications sont apportées à ces objets.

La fonction en cours de liaison doit:

- Être accessible par le code et les métadonnées (donc travail interne/privée dans C#), mais C++/CX aura besoin de méthodes qui soient des méthodes WinRT publiques.
- La surcharge repose sur le nombre d’arguments, pas sur le type; la fonction essaiera de trouver une correspondance avec la première surcharge présentant ces nombreux arguments.
- Les types d’arguments doivent correspondre aux données transmises. Nous ne faisons pas de conversions restrictives.
- Le type de retour de la fonction doit correspondre au type de la propriété qui utilise la liaison.

À partir de la prochaine mise à jour majeure vers Windows 10, le moteur de liaison sera réagir aux notifications de modification de propriété déclenchées avec le nom de la fonction et réévaluer les liaisons en fonction des besoins. Exemple : 

```XAML
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```
```csharp
public class Person:INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set 
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> Vous pouvez utiliser des fonctions dans x: Bind afin d’obtenir les mêmes scénarios que ce qui a été pris en charge par le biais des convertisseurs et MultiBinding dans WPF.

## <a name="function-arguments"></a>Arguments de la fonction

Plusieurs arguments peuvent être spécifiés dans la fonction. Ils sont séparés par une virgule (,).

- Chemin de liaison. Même syntaxe que si vous établissiez une liaison directement à cet objet.
  - Si le mode est OneWay/TwoWay, la détection de modification sera réalisée et la liaison réévaluée lors des modifications de l’objet.
- Chaîne de constante entourée de guillemets (les guillemets sont nécessaires pour la désigner comme chaîne). L’accent circonflexe (^) peut être utilisé comme caractère d’échappement des guillemets dans les chaînes.
- Numéro de constante. Par exemple, 123.456
- Valeur booléenne. Sous la forme «x: True» ou «x: False»

### <a name="two-way-function-bindings"></a>Liaisons de fonctions bidirectionnelles

Dans un scénario de liaison bidirectionnelle, une deuxième fonction doit être spécifiée pour la direction inverse de la liaison. Cette opération est effectuée à l’aide de la propriété de liaison **BindBack** . Dans l’exemple indiqué ci-dessous, la fonction doit prendre un argument qui est la valeur qui doit être transmises en retour au modèle.
```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```
