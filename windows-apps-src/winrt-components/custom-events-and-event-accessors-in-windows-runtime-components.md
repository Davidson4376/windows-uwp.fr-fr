---
title: Événements et accesseurs d’événement personnalisés dans les composants Windows Runtime
description: La prise en charge de .NET pour les composants Windows Runtime facilite la déclaration des composants d’événements en masquant les différences entre le modèle d’événement plateforme Windows universelle (UWP) et le modèle d’événement .NET.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1891a89d83ea8a193d5c0fcedf939101323981e6
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340534"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Événements et accesseurs d’événement personnalisés dans les composants Windows Runtime

La prise en charge de .NET pour les composants Windows Runtime facilite la déclaration des composants d’événements en masquant les différences entre le modèle d’événement plateforme Windows universelle (UWP) et le modèle d’événement .NET. Toutefois, lorsque vous déclarez des accesseurs d’événement personnalisés dans un composant Windows Runtime, vous devez suivre le modèle utilisé dans la UWP.

## <a name="registering-events"></a>Inscription d’événements

Lorsque vous effectuez une inscription pour gérer un événement dans UWP, l’accesseur add retourne un jeton. Pour annuler l’inscription, vous devez transmettre ce jeton à l’accesseur remove. Cela signifie que les accesseurs add et remove pour les événements UWP ont des signatures différentes des accesseurs auxquels vous êtes habitué.

Heureusement, les Visual Basic et C# les compilateurs simplifient ce processus : Quand vous déclarez un événement avec des accesseurs personnalisés dans un composant Windows Runtime, les compilateurs utilisent automatiquement le modèle UWP. Par exemple, vous obtenez une erreur de compilation si votre accesseur add ne renvoie pas de jeton. .NET fournit deux types pour prendre en charge l’implémentation :

-   La structure [EventRegistrationToken](https://docs.microsoft.com/uwp/api/windows.foundation.eventregistrationtoken) représente le jeton.
-   La classe [EventRegistrationTokenTable&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1) crée les jetons et maintient un mappage entre les jetons et les gestionnaires d’événements. L’argument de type générique est le type d’argument d’événement. Vous devez créer une instance de cette classe pour chaque événement la première fois qu’un gestionnaire d’événements est inscrit pour l’événement en question.

Le code suivant pour l’événement NumberChanged illustre le modèle de base pour les événements UWP. Dans cet exemple, le constructeur de l’objet d’argument d’événement, NumberChangedEventArgs, prend un seul paramètre entier représentant la valeur numérique modifiée.

> **Notez**  Le est le même modèle que les compilateurs utilisent pour les événements ordinaires que vous déclarez dans un composant Windows Runtime.

 
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

> **Important**  Pour garantir la sécurité des threads, le champ qui contient l’instance de l’événement EventRegistrationTokenTable @ No__t-2T @ no__t-3 doit être un champ de niveau classe. S’il s’agit d’un champ de niveau classe, la méthode GetOrCreateEventRegistrationTokenTable garantit que lorsque plusieurs threads tentent de créer la table de jeton, ils obtiennent tous la même instance de la table. Pour un événement donné, tous les appels à la méthode GetOrCreateEventRegistrationTokenTable doivent utiliser le même champ de niveau classe.

Le fait d’appeler la méthode GetOrCreateEventRegistrationTokenTable dans l’accesseur remove et dans la méthode [RaiseEvent](https://docs.microsoft.com/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) (méthode OnRaiseEvent en C#) garantit qu’aucune exception ne se produit si ces méthodes sont appelées avant que les délégués de gestionnaire d’événements n’aient été ajoutés.

Les autres membres de la classe EventRegistrationTokenTable&lt;T&gt; utilisés dans le modèle d’événement UWP incluent les éléments suivants :

-   La méthode [AddEventHandler](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_) génère un jeton pour le délégué de gestionnaire d’événements, stocke le délégué dans la table, l’ajoute à la liste d’appel, puis retourne le jeton.
-   La surcharge de méthode [RemoveEventHandler(EventRegistrationToken)](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_) supprime le délégué de la table et de la liste d’appel.

    >**Remarque**  Les méthodes AddEventHandler et RemoveEventHandler (EventRegistrationToken) verrouillent la table pour garantir la sécurité des threads.

-   La propriété [InvocationList](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList) retourne un délégué qui inclut tous les gestionnaires d’événements actuellement inscrits pour gérer l’événement. Utilisez ce délégué pour déclencher l’événement ou utilisez les méthodes de la classe Delegate pour appeler les gestionnaires individuellement.

    >**Remarque**  We vous recommande de suivre le modèle indiqué dans l’exemple fourni plus haut dans cet article et de copier le délégué vers une variable temporaire avant de l’appeler. Cela permet d’éviter une condition de concurrence dans laquelle le thread supprime le dernier gestionnaire, réduisant ainsi le délégué à la valeur null juste avant qu’un autre thread tente d’appeler le délégué. Les délégués étant immuables, la copie reste valide.

Placez votre propre code dans les accesseurs si nécessaire. Si la sécurité des threads pose problème, vous devez fournir votre propre verrouillage pour votre code.

C#utilisateurs Lorsque vous écrivez des accesseurs d’événement personnalisés dans le modèle d’événement UWP, le compilateur ne fournit pas les raccourcis syntaxiques habituels. Il génère des erreurs si vous utilisez le nom de l’événement dans votre code.

Visual Basic utilisateurs : Dans .NET, un événement est simplement un délégué multicast qui représente tous les gestionnaires d’événements inscrits. Le fait de déclencher l’événement signifie simplement appeler le délégué. La syntaxe Visual Basic masque généralement les interactions avec le délégué et le compilateur copie le délégué avant de l’appeler, comme indiqué dans la remarque sur la sécurité des threads. Lorsque vous créez un événement personnalisé dans un composant Windows Runtime, vous devez gérer le délégué directement. Cela signifie également que vous pouvez, par exemple, utiliser la méthode [MulticastDelegate.GetInvocationList](https://docs.microsoft.com/dotnet/api/system.multicastdelegate.getinvocationlist#System_MulticastDelegate_GetInvocationList) pour obtenir un tableau qui contient un délégué distinct pour chaque gestionnaire d’événements, si vous voulez appeler les gestionnaires séparément.

## <a name="related-topics"></a>Rubriques connexes

* [Événements (Visual Basic)](https://docs.microsoft.com/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [Événements (C# Guide de programmation)](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/events/index)
* [Vue d’ensemble de .NET pour les applications UWP](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [.NET pour les applications UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Procédure pas à pas pour créer un composant C# ou Visual Basic et l’appeler à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
