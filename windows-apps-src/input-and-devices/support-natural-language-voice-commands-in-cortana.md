---
Description: Découvrez comment enrichir Cortana avec des commandes vocales plus souples et plus naturelles qui permettent à un utilisateur de prononcer le nom de votre application n’importe où dans la commande.
title: Prendre en charge des commandes vocales en langage naturel dans Cortana
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
---

# Prendre en charge des commandes vocales en langage naturel dans Cortana


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Enrichissez **Cortana** avec des commandes vocales plus souples et plus naturelles qui permettent à un utilisateur de prononcer le nom de votre application n’importe où dans la commande.

**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


L’utilisation des commandes vocales pour enrichir Cortana avec les fonctionnalités de votre application nécessite que l’utilisateur spécifie à la fois l’application et la commande ou fonction à exécuter. Cette opération est généralement effectuée en annonçant le nom de l’application au début ou à la fin de la commande vocale. Par exemple, « Adventure Works, ajouter un nouveau voyage à Las Vegas ».

Toutefois, spécifier le nom de l’application de cette manière peut sembler gauche, maladroit, voire sans intérêt. Dans de nombreux cas, la possibilité de dire le nom de l’application ailleurs dans la commande est plus pratique et plus naturel, et contribue à rendre l’interaction plus intuitive et plus conviviale. Notre exemple précédent, « Adventure Works, ajouter un nouveau voyage à Las Vegas », peut être reformulé ainsi : « Ajouter un nouveau voyage Adventure Works à Las Vegas » ou « À l’aide d’Adventure Works, ajoutez un nouveau voyage à Las Vegas ».

Vous pouvez configurer vos commandes vocales pour que le nom de l’application soit pris en charge en tant que :

-   Préfixe : avant l’expression de la commande
-   Infixe : au sein de l’expression de la commande
-   Suffixe : après l’expression de la commande

**Prérequis : **

Cette rubrique s’appuie sur l’article [Lancer une application en arrière-plan avec les commandes vocales de Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). Nous continuons ici à illustrer ces fonctionnalités avec une application de planification et de gestion de voyages nommée **Adventure Works**.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Recommandations en matière d’expérience utilisateur : **

Pour des informations sur la manière d’intégrer votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application dotée de fonctions vocales à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Specify_an_AppName_element_in_the_VCD"> </span> <span id="specify_an_appname_element_in_the_vcd"> </span> <span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"> </span>Spécifier un élément **AppName** dans le fichier VCD


L’élément **AppName** permet de spécifier un nom convivial pour une application dans une commande vocale.

```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"> </span> <span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"> </span> <span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"> </span>Préciser où le nom de l’application peut être prononcé dans la commande vocale


L’élément **ListenFor** possède un attribut **RequireAppName** qui spécifie où le nom de l’application peut apparaître dans la commande vocale. Cet attribut prend en charge quatre valeurs.

1.  **BeforePhrase**

    Valeur par défaut.

    Indique que les utilisateurs doivent dire le nom de votre application avant l’expression de la commande.

    Ici, Cortana entend « Adventure Works, quand aura lieu mon voyage à Las Vegas ? ».

```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    Indique que les utilisateurs doivent dire le nom de votre application après l’expression de la commande.

    Une liste d’expressions localisées de type préposition est fournie par le système. Elle inclut des expressions telles que « à l’aide de », « avec » et « dans ».

    Ici, Cortana entend des commandes comme « Afficher mon prochain voyage à Las Vegas dans Adventure Works » et « Afficher mon prochain voyage à Las Vegas à l’aide d’Adventure Works ».

```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    Indique que les utilisateurs peuvent dire le nom de votre application avant ou après l’expression de la commande.

    Pour la version suffixe, une liste d’expressions localisées de type préposition est fournie par le système. Elle inclut des expressions telles que « à l’aide de », « avec » et « dans ».

    Ici, Cortana entend des commandes comme « Adventure Works, afficher mon prochain voyage à Las Vegas » ou « Afficher mon prochain voyage à Las Vegas dans Adventure Works ».

``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    Indique que les utilisateurs doivent dire le nom de votre application là où vous le spécifiez dans l’expression de la commande. L’utilisateur n’est pas tenu de dire le nom de l’application avant ou après l’expression.

    Vous devez explicitement référencer le nom de votre application à l’aide de la balise **{builtin:AppName}**.

    Ici, Cortana entend des commandes comme « Adventure Works, afficher mon prochain voyage à Las Vegas » ou « Afficher mon prochain voyage Adventure Works à Las Vegas ».

```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"> </span> <span id="special_cases"> </span> <span id="SPECIAL_CASES"> </span>Cas particuliers

Lorsque vous déclarez un élément **ListenFor** où **RequireAppName** est « AfterPhrase » ou « ExplicitlySpecified », vous devez vous assurer que certaines conditions sont remplies :

1.  **{builtin:AppName}** doit apparaître une seule fois si **RequireAppName** est « ExplicitlySpecified ».

    Avec cette valeur, le système ne peut pas déduire où le nom de l’application peut apparaître dans la commande vocale. Vous devez explicitement spécifier cet emplacement.

2.  Vous ne pouvez pas avoir de commande vocale commençant par un élément **PhraseTopic**, qui est généralement utilisé pour la reconnaissance vocale contenant plusieurs mots. Un mot au moins doit le précéder.

    Cela permet de réduire le risque que **Cortana** lance votre application si une commande contient son nom ou une partie de celui-ci, n’importe où dans l’énoncé.

    Voici une déclaration non valide qui peut conduire **Cortana** à lancer l’application **Adventure Works** si l’utilisateur dit quelque chose comme « Afficher les avis concernant Kinect Adventure Works ».

```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  La chaîne **ListenFor** doit contenir au moins deux mots en plus du nom de votre application et des références aux éléments **PhraseTopic**.

    Comme dans le cas n°2, vous devez vous assurer que le contenu de vos commandes est suffisamment phonétique pour réduire les risques de lancement non intentionnel de votre application.

    Cela vous aide à configurer votre application du mieux possible afin qu’elle ne soit pas lancée de façon incorrecte quand l’utilisateur dit par exemple « Rechercher Kinect Adventure Works ».

    Voici des déclarations non valides qui peuvent conduire **Cortana** à lancer l’application **Adventure Works** si l’utilisateur dit quelque chose comme « Hey Adventure Works » ou « Rechercher Kinect Adventure Works ».

```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"> </span> <span id="remarks"> </span> <span id="REMARKS"> </span>Remarques

La prise en charge d’une plus grande variation de la façon dont une commande vocale peut être prononcée par un utilisateur dans **Cortana** augmente également la convivialité de votre application.

Évitez d’avoir « Hey \[nom de l’application\] » en tant que votre **AppName** ou **CommandPrefix**. Les utilisateurs sont beaucoup plus susceptibles de dire « Hey Cortana » pour appeler Cortana via l’activation vocale. Par ailleurs, avoir « Hey \[nom de l’application\] » dans l’énoncé ne semble pas naturel. Par exemple, « Hey Cortana, afficher mon prochain voyage à Las Vegas dans Hey Adventure Works ».

Pensez à ajouter des variantes de type infixe/suffixe à vos commandes vocales existantes. Comme nous l’avons montré ici, il est relativement aisé d’ajouter un attribut supplémentaire à vos éléments **ListenFor** existants et de prendre en charge des variantes de type suffixe. Il semble beaucoup plus naturel de dire « Hey Cortana, afficher mon prochain voyage à Las Vegas dans Adventure Works » que « Hey Cortana, Adventure Works, afficher mon prochain voyage à Las Vegas ».

Pensez à utiliser le nom de votre application comme préfixe dans les cas où la commande vocale est en conflit avec les fonctionnalités **Cortana** existantes (appels, messagerie, etc.). Par exemple, « Adventure Works, envoyer un message à \[agent de voyage\] concernant un voyage à Las Vegas ».

## <span id="Complete_example"> </span> <span id="complete_example"> </span> <span id="COMPLETE_EXAMPLE"> </span>Exemple complet


Voici un fichier VCD qui illustre différentes manières de fournir des commandes vocales en langage plus naturel.

**Remarque** Il est possible d’avoir plusieurs éléments **ListenFor**, chacun avec une valeur d’attribut **RequireAppName** différente.

 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"> </span>Articles connexes


**Développeurs**
* [Interactions avec Cortana](cortana-interactions.md)
* [Définir des contraintes de reconnaissance vocale personnalisées](define-custom-recognition-constraints.md)
* [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


