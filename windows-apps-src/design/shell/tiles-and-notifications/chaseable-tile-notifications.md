---
author: andrewleader
Description: Use chaseable tile notifications to find out what your app displayed on its Live Tile when the user clicked it.
title: Notifications par vignette pouvant être suivies
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
keywords: windows10, uwp, vignettes pouvant être suivies, vignettes dynamiques, notifications par vignette pouvant être suivies
ms.localizationpriority: medium
ms.openlocfilehash: 8126755dfb6f5f0e117d10daef85a83e8a171f1f
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7300557"
---
# <a name="chaseable-tile-notifications"></a>Notifications par vignette pouvant être suivies

Les notifications par vignette pouvant être suivies vous permettent de déterminer les notifications par vignette affichées par la Vignette dynamique de votre application lorsque l’utilisateur a cliqué dessus.  
Par exemple, une application d’actualités peut utiliser cette fonctionnalité pour déterminer l'article que sa Vignette dynamique affichait lorsque l’utilisateur l'a lancée. Elle peut ainsi garantir que l’article sera affiché de façon visible pour que l’utilisateur puisse le trouver. 

> [!IMPORTANT]
> **Nécessite la mise à jour anniversaire**: pour utiliser les notifications de vignette pouvant être suivies avec les applications UWP en C#, C++ ou VB, vous devez cibler le Kit de développement logiciel (SDK)14393 et exécuter la Build14393 ou une version supérieure. Pour les applications UWP en JavaScript, vous devez cibler le Kit de développement logiciel (SDK)17134 et exécuter la Build17134 ou une version supérieure. 


> **API importantes**: [propriété LaunchActivatedEventArgs.TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [classe TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Principe de fonctionnement

Pour activer les notifications par vignette pouvant être suivies, vous utilisez la propriété **Arguments** sur la charge utile de notification par vignette (semblable à la propriété de lancement de la charge utile de notification toast) pour incorporer des informations sur le contenu dans la notification par vignette.

Lorsque votre application est lancée via la Vignette dynamique, le système retourne une liste d’arguments des notifications par vignette actuellement/récemment affichées.


## <a name="when-to-use-chaseable-tile-notifications"></a>Quand utiliser les notifications par vignette pouvant être suivies

Les notifications par vignette pouvant être suivies s'utilisent généralement lorsque vous utilisez la file d’attente de notification sur votre Vignette dynamique (ce qui signifie que vous parcourez jusqu'à 5notifications différentes par cycle). Elles sont également utiles lorsque le contenu de votre Vignette dynamique est potentiellement désynchronisé avec le dernier contenu de l’application. Par exemple, l’application Actualités actualise sa Vignette dynamique toutes les 30minutes, mais lorsque l’application est lancée, elle charge les toutes dernières informations (qui risquent de ne pas inclure un élément présent sur la vignette depuis le dernier intervalle d’interrogation). Si cela se produit, l’utilisateur peut se sentir frustré de ne pas retrouver l’article repéré sur la Vignette dynamique. C'est là que les notifications par vignette pouvant être suivies sont utiles, en vous permettant de garantir que l’utilisateur retrouve facilement ce qu'il voit sur sa vignette.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Que faire avec des notifications par vignette pouvant être suivies

Le plus important à noter est que, dans la plupart des scénarios, **vous ne devez PAS accéder directement à la notification spécifique** qui était présente sur la Vignette lorsque l’utilisateur a cliqué dessus. Votre Vignette dynamique est utilisée comme point d’entrée de votre application. Deux scénarios sont possibles lorsqu’un utilisateur clique sur votre Vignette dynamique: (1) il veut lancer votre application normalement, ou (2) il veut voir plus d’informations sur une notification spécifique présente dans la Vignette dynamique. Dans la mesure où il n’existe aucun moyen pour l’utilisateur d'indiquer explicitement le comportement qu’il souhaite, l’expérience idéale consiste à **lancer votre application normalement, tout en garantissant que la notification repérée par l’utilisateur soit facilement accessible**.

Par exemple, lorsque vous cliquez sur la Vignette dynamique de l’application MSN Actualité, l’application se lance normalement: elle affiche la page d’accueil ou un article que l’utilisateur a été précédemment lu. Toutefois, sur la page d’accueil, l’application garantit que l’article présent sur la Vignette dynamique est facilement repérable. De cette façon, les deux scénarios sont pris en charge: le scénario dans lequel vous souhaitez simplement lancer/reprendre l’application et le scénario dans lequel vous souhaitez afficher l’article spécifique.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Comment inclure la propriété Arguments dans votre charge utile de notification par vignette

Dans une charge utile de notification, la propriété arguments permet à votre application de fournir des données, qui vous permettent d'identifier ultérieurement la notification. Par exemple, vos arguments peuvent inclure l'identifiant de l'article, afin que lors du lancement, vous puissiez récupérer et afficher l’article. La propriété accepte une chaîne, qui peut être sérialisée comme vous le souhaitez (chaîne de requête, JSON, etc.), mais, il est recommandé, en général, d'utiliser le format de chaîne de requête, qui est léger et s'encode harmonieusement au format XML.

La propriété peut être définie sur les deux éléments **TileVisual** et **TileBinding** et se répercutera en cascade. Si vous souhaitez les mêmes arguments sur chaque taille de vignette, définissez simplement les arguments sur l’élément **TileVisual**. Si vous avez besoin d’arguments spécifiques pour des tailles de vignettes spécifiques, vous pouvez définir les arguments sur des éléments **TileBinding** individuels.

Cet exemple crée une charge utile de notification qui utilise la propriété arguments afin que les notifications puissent être identifiées ultérieurement. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>Procédure de vérification de la propriété arguments au lancement de votre application

La plupart des applications disposent d’un fichier App.xaml.cs qui contient un remplacement de la méthode [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_). Comme son nom l’indique, votre application appelle cette méthode lors de son lancement. Elle prend un seul argument, un objet [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs).

L’objet LaunchActivatedEventArgs a une propriété qui active les notifications pouvant être suivies: la [propriété TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), qui fournit l’accès à un [objet TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo). Lorsque l’utilisateur lance votre application à partir de sa vignette (au lieu de la liste des applications, de la recherche ou de n’importe quel autre point d’entrée), votre application initialise cette propriété.

L'[objet TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) contient une propriété appelée [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), qui contient une liste de notifications qui ont été affichées sur la vignette au cours des 15dernières minutes. Le premier élément de la liste représente la notification actuellement présente sur la vignette et les éléments suivants représentent les notifications que l’utilisateur a vues précédemment. Si votre vignette a été désactivée, cette liste est vide.

Chaque ShownTileNotificationhas une Argumentsproperty. La Argumentsproperty sera initialisée avec l’argumentsstring à partir de votre charge utile de notification par vignette ou null si votre charge utile n’inclut pas l’argumentsstring.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


## <a name="raw-xml-example"></a>Exemple de code XML brut

Si vous utilisez du code XML brut au lieu de la bibliothèque Notifications, voici le code XML.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>Articles associés

- [Propriété LaunchActivatedEventArgs.TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [Classe TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)