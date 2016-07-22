---
author: msatranjr
title: "Événements et accesseurs d’événement personnalisés dans les composants Windows Runtime"
description: "La prise en charge de .NET Framework pour les composants Windows Runtime facilite la déclaration de composants d’événements, car les différences entre le modèle d’événement de la plateforme UWP et le modèle d’événement .NET Framework sont masquées."
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
translationtype: Human Translation
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 1308989c8d1c6959560458dd4d87119b4bfa74b0

---

# Événements et accesseurs d’événement personnalisés dans les composants Windows Runtime


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

La prise en charge de .NET Framework pour les composants Windows Runtime facilite la déclaration de composants d’événements, car les différences entre le modèle d’événement de la plateforme Windows universelle (UWP) et le modèle d’événement .NET Framework sont masquées. Toutefois, lorsque vous déclarez des accesseurs d’événement personnalisés dans un composant Windows Runtime, vous devez suivre le modèle utilisé dans UWP.

## Inscription d’événements


Lorsque vous effectuez une inscription pour gérer un événement dans UWP, l’accesseur add retourne un jeton. Pour annuler l’inscription, vous devez transmettre ce jeton à l’accesseur remove. Cela signifie que les accesseurs add et remove pour les événements UWP ont des signatures différentes des accesseurs auxquels vous êtes habitué.

Heureusement, les compilateurs Visual Basic et C# simplifient ce processus: lorsque vous déclarez un événement avec des accesseurs personnalisés dans un composant Windows Runtime, les compilateurs utilisent automatiquement le modèle UWP. Par exemple, vous obtenez une erreur de compilation si votre accesseur add ne renvoie pas de jeton. Deux types sont proposés par .NET Framework pour prendre en charge l’implémentation :

-   La structure [EventRegistrationToken](https://msdn.microsoft.com/library/windows/apps/windows.foundation.eventregistrationtoken.aspx) représente le jeton.
-   La classe [EventRegistrationTokenTable&lt;T&gt;](https://msdn.microsoft.com/library/hh138412.aspx) crée les jetons et maintient un mappage entre les jetons et les gestionnaires d’événements. L’argument de type générique est le type d’argument d’événement. Vous devez créer une instance de cette classe pour chaque événement la première fois qu’un gestionnaire d’événements est inscrit pour l’événement en question.

Le code suivant pour l’événement NumberChanged illustre le modèle de base pour les événements UWP. Dans cet exemple, le constructeur de l’objet d’argument d’événement, NumberChangedEventArgs, prend un seul paramètre entier représentant la valeur numérique modifiée.

> **Remarque** Le même modèle est utilisé par les compilateurs pour les événements ordinaires que vous déclarez dans un composant Windows Runtime.

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

La méthode statique (partagée en Visual Basic) GetOrCreateEventRegistrationTokenTable crée tardivement l’instance d’événement de l’objet EventRegistrationTokenTable&lt;T&gt;. Transmettez le champ de niveau classe qui contiendra l’instance de table de jeton à cette méthode. Si le champ est vide, la méthode crée la table, stocke une référence vers la table dans le champ et retourne une référence à la table. Si le champ contient déjà une référence de table de jeton, la méthode retourne simplement cette référence.

> **Important** Pour garantir la sécurité des threads, le champ qui contient l’instance EventRegistrationTokenTable&lt;T&gt; de l’événement doit être un champ de niveau classe. S’il s’agit d’un champ de niveau classe, la méthode GetOrCreateEventRegistrationTokenTable garantit que lorsque plusieurs threads tentent de créer la table de jeton, ils obtiennent tous la même instance de la table. Pour un événement donné, tous les appels à la méthode GetOrCreateEventRegistrationTokenTable doivent utiliser le même champ de niveau classe.

Le fait d’appeler la méthode GetOrCreateEventRegistrationTokenTable dans l’accesseur remove et dans la méthode [RaiseEvent](https://msdn.microsoft.com/library/fwd3bwed.aspx) (méthode OnRaiseEvent en C#) garantit qu’aucune exception ne se produit si ces méthodes sont appelées avant que les délégués de gestionnaire d’événements n’aient été ajoutés.

Les autres membres de la classe EventRegistrationTokenTable&lt;T&gt; utilisés dans le modèle d’événement UWP incluent les éléments suivants :

-   La méthode [AddEventHandler](https://msdn.microsoft.com/library/hh138458.aspx) génère un jeton pour le délégué de gestionnaire d’événements, stocke le délégué dans la table, l’ajoute à la liste d’appel, puis retourne le jeton.
-   La surcharge de méthode [RemoveEventHandler(EventRegistrationToken)](https://msdn.microsoft.com/library/hh138425.aspx) supprime le délégué de la table et de la liste d’appel.

    >**Remarque** Les méthodes AddEventHandler et RemoveEventHandler(EventRegistrationToken) verrouillent la table afin de garantir la sécurité des threads.

-   La propriété [InvocationList](https://msdn.microsoft.com/library/hh138465.aspx) retourne un délégué qui inclut tous les gestionnaires d’événements actuellement inscrits pour gérer l’événement. Utilisez ce délégué pour déclencher l’événement ou utilisez les méthodes de la classe Delegate pour appeler les gestionnaires individuellement.

    >**Remarque** Nous vous recommandons de suivre le modèle indiqué dans l’exemple fourni précédemment dans cet article et de copier le délégué dans une variable temporaire avant de l’appeler. Cela permet d’éviter une condition de concurrence dans laquelle le thread supprime le dernier gestionnaire, réduisant ainsi le délégué à la valeur null juste avant qu’un autre thread tente d’appeler le délégué. Les délégués étant immuables, la copie reste valide.

Placez votre propre code dans les accesseurs si nécessaire. Si la sécurité des threads pose problème, vous devez fournir votre propre verrouillage pour votre code.

Utilisateurs de C#: lorsque vous écrivez des accesseurs d’événement personnalisés dans le modèle d’événement UWP, le compilateur ne fournit pas les raccourcis de syntaxe habituels. Il génère des erreurs si vous utilisez le nom de l’événement dans votre code.

Utilisateurs de Visual Basic : dans .NET Framework, un événement est simplement un délégué multicast qui représente tous les gestionnaires d’événements inscrits. Le fait de déclencher l’événement signifie simplement appeler le délégué. La syntaxe Visual Basic masque généralement les interactions avec le délégué et le compilateur copie le délégué avant de l’appeler, comme indiqué dans la remarque sur la sécurité des threads. Lorsque vous créez un événement personnalisé dans un composant Windows Runtime, vous devez gérer le délégué directement. Cela signifie également que vous pouvez, par exemple, utiliser la méthode [MulticastDelegate.GetInvocationList](https://msdn.microsoft.com/library/system.multicastdelegate.getinvocationlist.aspx) pour obtenir un tableau qui contient un délégué distinct pour chaque gestionnaire d’événements, si vous voulez appeler les gestionnaires séparément.

## Rubriques connexes

* [Événements (Visual Basic)](https://msdn.microsoft.com/library/ms172877.aspx)
* [Événements (Guide de programmation C#)](https://msdn.microsoft.com/library/awbftdfh.aspx)
* [Vue d’ensemble de .NET pour les applications du Windows Store](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Procédure pas à pas : création d’un composant Windows Runtime simple et appel de ce composant à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)



<!--HONumber=Jun16_HO5-->


