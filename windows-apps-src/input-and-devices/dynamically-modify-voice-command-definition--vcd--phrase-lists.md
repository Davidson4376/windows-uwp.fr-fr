---
Description: Découvrez comment accéder à la liste des expressions prises en charge (éléments PhraseList) d’un fichier de définition des commandes vocales (VCD) et comment la mettre à jour à l’aide du résultat de reconnaissance vocale au moment de l’exécution.
title: Modifier de manière dynamique les listes d’expressions de définition des commandes vocales (VCD)
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modifier les listes d’expressions de définition des commandes vocales (VCD)
template: detail.hbs
---

# Modifier de manière dynamique les listes d’expressions de définition des commandes vocales (VCD)


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Découvrez comment accéder à la liste des expressions prises en charge (éléments **PhraseList**) d’un fichier de définition des commandes vocales (VCD) et comment la mettre à jour à l’aide du résultat de reconnaissance vocale au moment de l’exécution.

Il peut être utile de modifier de manière dynamique une liste d’expressions au moment de l’exécution si la commande vocale est propre à une tâche impliquant un certain type de favori défini par l’utilisateur ou des données d’application provisoires.

Supposons par exemple que vous ayez une application de voyage dans laquelle les utilisateurs peuvent entrer des destinations, et que vous souhaitiez que les utilisateurs puissent la démarrer en disant son nom suivi de la mention « Afficher le voyage à &lt; destination &gt; ». Vous n’avez pas besoin de créer un élément **ListenFor** distinct pour chaque destination possible. Vous pouvez remplir dynamiquement **PhraseList** au moment de l’exécution avec les destinations créées par l’utilisateur. Dans l’élément **ListenFor** proprement dit, vous pourriez indiquer par exemple `<ListenFor> Show trip to {destination}  </ListenFor>`, où la « destination » est la valeur de l’attribut **Label** pour l’élément **PhraseList**.

Pour plus d’informations sur **PhraseList** et d’autres éléments du fichier VCD, voir la documentation de référence [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

**Prérequis : **

Cette rubrique s’appuie sur [Lancer une application au premier plan avec les commandes vocales de Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md). Nous continuons ici à illustrer ces fonctionnalités avec une application de planification et de gestion de voyages nommée **Adventure Works**.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Recommandations en matière d’expérience utilisateur :**

Pour des informations sur la manière d’intégrer votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application, dotée de fonctions vocales, à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Identify_the_command"> </span> <span id="identify_the_command"> </span> <span id="IDENTIFY_THE_COMMAND"> </span>Identifier la commande


Pour mettre à jour un élément **PhraseList** du fichier VCD, obtenez l’élément **CommandSet** contenant la liste d’expressions. Utilisez l’attribut **Name** de cet élément **CommandSet** comme clé d’accès à la propriété [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257) (**Name** doit être unique dans le fichier VCD) et obtenez la référence [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258).

## <span id="Replace_the_phrase_list"> </span> <span id="replace_the_phrase_list"> </span> <span id="REPLACE_THE_PHRASE_LIST"> </span>Remplacer la liste d’expressions


Après avoir identifié l’ensemble de commandes, obtenez une référence à la liste d’expressions à modifier, puis appelez la méthode [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261), utilisez l’attribut **Label** de l’élément **PhraseList** et un tableau de chaînes comme nouveau contenu de la liste d’expressions.

**Remarque** Si vous modifiez une liste d’expressions, la totalité de la liste d’expressions est remplacée. Si vous voulez insérer de nouveaux éléments dans une liste d’expressions, vous devez spécifier à la fois les éléments existants et les nouveaux éléments dans l’appel à [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261).

 

Le fichier VCD définit une commande **Command** « showTripToDestination » avec une **PhraseList** qui définit trois options permettant de sélectionner une destination dans notre application de voyage **Adventure Works**. Lorsque l’utilisateur enregistre et supprime des destinations dans l’application, celle-ci met à jour les options de la **PhraseList**.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>
```

Voici comment mettre à jour la **PhraseList** affichée dans l’exemple précédent en ajoutant Phoenix comme destination.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"> </span> <span id="remarks"> </span> <span id="REMARKS"> </span>Remarques


L’utilisation d’une **PhraseList** pour limiter la reconnaissance est appropriée pour une phrase relativement courte. Si la phrase est trop longue (si elle contient des centaines de mots par exemple) ou si elle ne doit pas être limitée du tout, utilisez l’élément **PhraseTopic** et un élément **Subject** pour affiner la pertinence des résultats de reconnaissance vocale et améliorer l’évolutivité.

Dans notre exemple, la **PhraseTopic** contient un élément **Scenario** « Search » (Recherche), affiné par un élément **Subject** « City/State  (Ville/État).

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <span id="related_topics"> </span>Articles connexes


**Développeurs**
* [Interactions avec Cortana](cortana-interactions.md)
* [Lancer une application au premier plan avec les commandes vocales de Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [Lancer une application en arrière-plan à l’aide des commandes vocales de Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)
* [
            **VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)
**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 




<!--HONumber=Mar16_HO1-->
