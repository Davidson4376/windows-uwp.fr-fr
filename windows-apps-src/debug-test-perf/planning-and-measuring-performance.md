---
author: jwmsft
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: Planification des performances
description: Les utilisateurs attendent de leurs applications qu’elles soient réactives, conviviales et qu’elles ne déchargent pas la batterie.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: d25620c0fc86f76b8c0d4de6e606250186b9ce37
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2017
ms.locfileid: "894789"
---
# <a name="planning-for-performance"></a>Planification des performances

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Les utilisateurs attendent de leurs applications qu’elles soient réactives, conviviales et qu’elles ne déchargent pas la batterie. Techniquement, la performance est une exigence non fonctionnelle mais le fait de considérer les performances comme une fonctionnalité vous aidera à répondre aux attentes de vos utilisateurs. La spécification des objectifs et la mesure sont des facteurs essentiels. Déterminez quels sont les scénarios pour lesquels les performances sont essentielles et définissez ce que vous entendez par bonnes performances. Effectuez ensuite des mesures précoces et régulières tout au long du cycle de vie de votre projet pour être sûr d’atteindre vos objectifs.

## <a name="specifying-goals"></a>Spécification des objectifs

L’expérience utilisateur est l’une des façons les plus simples de définir ce que l’on appelle « bonnes performances ». Le temps de démarrage de votre application peut être un facteur déterminant de la façon dont un utilisateur perçoit ses performances. Il peut envisager un temps de démarrage inférieur à une seconde comme excellent, inférieur à 5 secondes comme bon et supérieur à 5 secondes comme insatisfaisant.

D’autres facteurs, tels que la mémoire, ont moins d’impact sur l’expérience utilisateur. Le risque qu’une application s’arrête lorsqu’elle est en pause ou inactive augmente avec la quantité de mémoire utilisée par l’application active. Il est communément établi qu’une utilisation intensive de la mémoire dégrade l’expérience utilisateur pour toutes les applications du système, c’est pourquoi il est recommandé de se fixer un objectif en matière de consommation de mémoire. Prenez en considération la taille approximative de votre application telle qu’elle est perçue par les utilisateurs (petite, moyenne ou grande). Les attentes en matière de performances vont se baser sur cette perception. Par exemple, vous souhaiterez peut-être qu’une petite application utilisant peu de contenu multimédia consomme moins de 100 Mo de mémoire.

Il est préférable de définir un objectif initial et de le modifier ultérieurement, plutôt que de ne pas avoir d’objectif du tout. Les objectifs de performances de votre application doivent être précis et mesurables, et respecter trois grands critères : le temps nécessaire aux utilisateurs, ou à l’application, pour effectuer les tâches (temps) ; la vitesse et la facilité avec lesquelles elle se redessine en réponse à l’interaction utilisateur (fluidité) ; et la conservation des ressources système, y compris la batterie (efficacité).

## <a name="time"></a>Temps

Pensez à la durée acceptable (*classes d’interaction*) nécessaire pour que les utilisateurs effectuent leurs tâches dans votre application. Pour chaque classe d’interaction, attribuez une étiquette et définissez la perception de l’utilisateur ainsi que les durées idéales et maximum. Voici quelques suggestions.

| Étiquette de classe d’interaction | Perception de l’utilisateur                 | Durée idéale            | Durée maximum          | Exemples                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| Rapidité                    | Délai presque imperceptible      | 100 millisecondes | 200 millisecondes | Afficher la barre de l’application ; appuyer sur un bouton (première réponse)                        |
| Typique                 | Rapide, mais pas très rapide             | 300 millisecondes | 500 millisecondes | Redimensionner ; zoom sémantique                                                        |
| Réactivité              | Pas rapide, mais semble réactive | 500 millisecondes | 1 seconde         | Accéder à une autre page ; relancer l’application à partir d’un état interrompu          |
| Lancement                  | Expérience compétitive          | 1 seconde         | 3 secondes        | Lancer l’application pour la première fois ou après un arrêt |
| Continu              | Ne semble plus répondre      | 500 millisecondes | 5 secondes        | Télécharger un fichier d’Internet                                            |
| Captif                 | Long ; l’utilisateur peut décider de quitter    | 500 millisecondes | 10 secondes       | Installer plusieurs applications à partir du Store                                         |

 

Vous pouvez désormais attribuer des classes d’interaction aux scénarios de performances de votre application. Vous pouvez attribuer la référence dans le temps de l’application, une partie de l’expérience utilisateur et une classe d’interaction à chaque scénario. Vous trouverez ci-dessous des suggestions pour un exemple d’application de cuisine et restauration.


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>Scénario</th><th>Point dans le temps</th><th>Expérience utilisateur</th><th>Classe d’interaction</th></tr>
<tr><td rowspan="3">Accéder à une page de recette </td><td>Première réponse</td><td>Démarrage de l’animation de transition de page</td><td>Rapide (100-200 millisecondes)</td></tr>
<tr><td>Réactivité</td><td>Chargement de la liste des ingrédients ; sans images</td><td>Dynamique (500 millisecondes - 1 seconde)</td></tr>
<tr><td>Visuel terminé</td><td>Contenu chargé ; images affichées</td><td>Continu (500 millisecondes - 5 secondes)</td></tr>
<tr><td rowspan="2">Rechercher des recettes</td><td>Première réponse</td><td>Clic sur le bouton Rechercher</td><td>Rapide (100 - 200 millisecondes)</td></tr>
<tr><td>Visuel terminé</td><td>Liste des titres de recettes en local affichée</td><td>Typique (300 - 500 millisecondes)</td></tr>
</table>

Si votre application contient du contenu dynamique, pensez également aux objectifs de rafraîchissement du contenu. Est-ce que le contenu doit être actualisé toutes les secondes ? Est-ce que le contenu doit être actualisé toutes les minutes, toutes les heures ou une fois par jour ?

Une fois vos objectifs spécifiés, vous pouvez désormais mieux tester, analyser et optimiser votre application.

## <a name="fluidity"></a>Fluidité

Voici quelques exemples d’objectifs de fluidité mesurables:

-   Pas d’arrêt et de redémarrage du dessin à l’écran (problèmes).
-   Rendu des animations à 60 images par seconde (FPS).
-   Lorsque l’utilisateur effectue un panoramique/fait défiler l’écran, l'application affiche 3 à 6 pages de contenu par seconde.

## <a name="efficiency"></a>Efficacité

Voici quelques exemples d’objectifs d’efficacité mesurables:

-   Concernant la capacité de traitement de votre application, le pourcentage du processeur est égal ou inférieur à *N* et l’utilisation de la mémoire (en Mo) est égale ou inférieure à *M* à tout moment.
-   Lorsque l’application est inactive, les valeurs *N* et *M* sont égales à zéro pour la capacité de traitement de votre application.
-   Votre application peut fonctionner de manière active pendant *X* heures sur la batterie. En cas d’inactivité, l'appareil conserve sa charge pendant *Y* heures.

## <a name="design-your-app-for-performance"></a>Conception de votre application dans un objectif de performances

Vous pouvez désormais utiliser vos objectifs de performances pour déterminer la façon dont vous allez concevoir votre application. Reprenons l’exemple de l’application de cuisine et restauration. Lorsque l’utilisateur accède à la page d’une recette, vous pouvez décider de [mettre à jour les différents éléments de manière incrémentielle](optimize-gridview-and-listview.md#update-items-incrementally) de telle sorte que le nom de la recette s’affiche en premier, puis les ingrédients et enfin les photos du plat. Ce procédé permet de conserver la réactivité et la fluidité de l’interface utilisateur lors du panoramique/défilement, en affichant le rendu d’une totale fidélité après le ralentissement de l’interaction afin de permettre au thread de l’interface utilisateur de rattraper le retard. Voici quelques éléments à prendre en compte.

**Interface utilisateur**

-   Optimisez le temps d’analyse et de chargement ainsi que l’efficacité de la mémoire de chaque page de l'interface utilisateur de votre application (surtout la page d’accueil) en [optimisant votre balisage XAML](optimize-xaml-loading.md). Autrement dit, différez le chargement de l’interface utilisateur et du code jusqu’à ce que cela soit nécessaire.
-   Pour [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) et [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705), faites en sorte que tous les éléments aient la même taille et utilisez autant de [techniques d'optimisation des commandes ListView et GridView](optimize-gridview-and-listview.md) que possible.
-   Déclarez l’interface utilisateur sous forme de balisage, pouvant être chargé et réutilisé sous forme de segments par l’infrastructure, au lieu de vouloir à tout prix la construire dans le code.
-   Différez la création des éléments de l’interface utilisateur jusqu’à ce que l’utilisateur en ait besoin. Consultez l’attribut [**x:Load**](../xaml-platform/x-load-attribute.md).
-   Préférez les transitions et les animations thématiques aux animations dans une table de montage. Pour plus d’informations, voir [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/Mt187350). Rappelez-vous que les animations dans une table de montage nécessitent que l’affichage soit en permanence mis à jour et que le processeur et les transformations graphiques restent actifs. Pour préserver la batterie, ne lancez pas d’animations si l’utilisateur n’interagit pas avec l’application.
-   Les images doivent être chargées à une taille appropriée à la vue dans laquelle vous les présentez, à l’aide de la méthode [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210).

**Processeur, mémoire et alimentation**

-   Planifiez des tâches de priorité inférieure à exécuter sur les threads de priorité inférieure et/ou les programmes principaux. Voir [Programmation asynchrone](https://msdn.microsoft.com/library/windows/apps/Mt187335), la propriété [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) et la classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).
-   Minimisez l’encombrement mémoire de votre application en mettant les ressources coûteuses (le contenu multimédia, par exemple) en suspens.
-   Réduisez la plage de travail de votre code.
-   Évitez les fuites de mémoire en désinscrivant les gestionnaires d’événements et en déréférençant les éléments d’interface utilisateur quand cela est possible.
-   Afin d’économiser la batterie, faites preuve de parcimonie en ce qui concerne la fréquence de collecte des données, d’interrogation d’un capteur ou de planification des tâches sur le processeur lorsque celui-ci est inactif.

**Accès aux données**

-   Dans la mesure du possible, préchargez le contenu. Pour une prérécupération automatique, voir la classe [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042). Pour une prérécupération manuelle, reportez-vous à l’espace de noms [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) et la classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517).
-   Dans la mesure du possible, placez en cache le contenu coûteux d’accès. Consultez les propriétés [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) et [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622).
-   En cas d’échecs de mise en cache, affichez aussi rapidement que possible une interface utilisateur d’espace réservé indiquant que l’application est toujours en train de charger le contenu. Passez de l’espace réservé au contenu en direct d’une façon qui ne soit pas déplaisante pour l’utilisateur. Par exemple, ne modifiez pas la position du contenu sous le doigt de l’utilisateur ou le pointeur de la souris quand l’application charge le contenu en direct.

**Lancer et reprendre l’application **

-   Retardez l’écran de démarrage de l’application et ne l’étendez que si cela est nécessaire. Pour plus d’informations, voir [Création d’une expérience de lancement d’application rapide et fluide](http://go.microsoft.com/fwlink/p/?LinkId=317595) et [Afficher un écran de démarrage plus longtemps](https://msdn.microsoft.com/library/windows/apps/Mt187309).
-   Désactivez les animations qui se produisent immédiatement après la disparition de l’écran de démarrage, car celles-ci entraînent uniquement une perception de retard du moment du démarrage de l’application.

**Interface utilisateur adaptative et orientation**

-   Utilisez la classe [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021).
-   Effectuez uniquement les tâches requises immédiatement, en différant les tâches d’application intensives pour plus tard. Votre application dispose de 200 à 800millisecondes pour effectuer les tâches avant que l’interface utilisateur de votre application n’apparaisse rognée pour l’utilisateur.

Une fois la conception liée aux performances en place, vous pouvez commencer à coder votre application.

## <a name="instrument-for-performance"></a>Instrumenter des applications performantes

Lorsque vous développez du code, ajoutez du code qui enregistre les messages et les événements à différents moments lors de l’exécution de votre application. Ensuite, lors du test de votre application, vous pouvez utiliser des outils de profilage tels que l’Enregistreur de performance Windows et Windows Performance Analyzer (inclus dans le [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)) pour créer et afficher un rapport sur les performances de votre application. Dans ce rapport, vous pouvez rechercher ces messages et événements qui vous permettent une meilleure analyse des résultats du rapport.

La plateforme Windows universelle (UWP) fournit des API d’enregistrement, basées sur le [Suivi des événements pour Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/Bb968803), qui proposent un enregistrement détaillé et une solution de suivi complète. Ces API, qui font partie de l’espace de noms [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677), comprennent les classes [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138), [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195), [**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) et [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217).

Pour enregistrer un message dans le rapport à un point spécifique de l’exécution de l’application, créez un objet **LoggingChannel**, puis appelez la méthode [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingchannel.logmessage.aspx) de l’objet, ainsi.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel("MyLoggingChannel");

myLoggingChannel.LogMessage(LoggingLevel.Information, "Here' s my logged message.");

// ...
```

Pour enregistrer les événements de démarrage et d’arrêt dans le rapport pendant une durée donnée pendant l’exécution de l’application, créez un objet **LoggingActivity**, puis appelez le constructeur [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingactivity.loggingactivity.aspx) de l’objet, ainsi.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel is defined and initialized in the previous code example.
using (myLoggingActivity = new LoggingActivity("MyLoggingActivity"), myLoggingChannel))
{   // After this logging activity starts, a start event is logged.
    
    // Add code here to do something of interest.
    
}   // After this logging activity ends, an end event is logged.

// ...
```

Reportez-vous également à l’[exemple de journalisation](http://go.microsoft.com/fwlink/p/?LinkId=529576).

Une fois votre application instrumentée, vous pouvez tester et mesurer les performances de l’application.

## <a name="test-and-measure-against-performance-goals"></a>Tester et mesurer les performances par rapport aux objectifs

Lorsque vous planifiez les performances, vous devez définir les points du cycle de développement qui feront l’objet d’une mesure des performances. Selon que vous effectuiez les mesures lors du cycle de prototypage, de développement ou de déploiement, les valeurs relevées n’auront pas le même usage. Il peut être extrêmement utile de mesurer les performances dès les premières phases de prototypage. C’est pourquoi nous vous recommandons de le faire dès que vous utilisez du code pour effectuer des tâches importantes. Les premières mesures vous donnent une bonne idée des points où se concentrent les principaux coûts dans votre application, et peuvent orienter vos décisions de conception. Au final, vos applications gagneront en performances et en évolutivité. Plus les conceptions sont modifiées tardivement et plus cela coûte cher. Mesurer les performances tardivement dans le cycle du projet peut occasionner des modifications de dernière minute et nuire aux performances.

Utilisez les techniques et les outils suivants pour tester les performances de l’application par rapport aux objectifs définis:

-   Effectuez des tests sur différentes configurations matérielles, notamment des PC de bureau et tout-en-un, des ordinateurs portables et ultraportables, des tablettes et d’autres appareils mobiles.
-   Effectuez des tests sur différentes tailles d’écran. Si des tailles d’écran plus larges permettent d’afficher beaucoup plus de contenu, tout ce contenu supplémentaire peut avoir un impact négatif sur les performances.
-   Supprimez le plus de variables de test possible.
    -   Désactivez les applications en arrière-plan sur l’appareil de test. Pour cela, dans Windows, sélectionnez **Paramètres** dans le menu Démarrer &gt;**Personnalisation** &gt;**Écran de verrouillage**. Sélectionnez chaque application active et sélectionnez **Aucun**.
    -   Compilez votre application en code natif en l’intégrant dans la configuration de mise sur le marché avant de la déployer sur l’appareil de test.
    -   Pour vous assurer que la maintenance automatique n’affecte pas les performances de l’appareil de test, déclenchez-la manuellement et attendez qu’elle se termine. Sous Windows, dans le menu Démarrer, recherchez **Sécurité et maintenance**. Dans la zone **Maintenance**, sous **Maintenance automatique**, sélectionnez **Commencer la maintenance** et attendez que l’état passe à **Maintenance en cours**.
    -   Exécutez l’application plusieurs fois pour mieux éliminer les variables de test aléatoires et garantir des mesures cohérentes.
-   Effectuez des tests en condition de faible alimentation électrique. Il se peut que l’appareil de vos utilisateurs ne bénéficie pas d’une alimentation aussi puissante que votre ordinateur de développement. Windows a été conçu pour fonctionner de manière optimale avec des appareils à faible consommation, tels que des appareils mobiles. Vous devez vous assurer que les applications qui s’exécutent sur la plateforme fonctionnent correctement sur ces périphériques. Pour bien définir vos objectifs, gardez à l’esprit qu’un appareil à faible consommation d’énergie est environ 4fois plus lent qu’un ordinateur de bureau.
-   Utilisez plusieurs outils, tels que Microsoft Visual Studio et Windows Performance Analyzer pour mesurer les performances de l’application. Visual Studio est conçu pour fournir des analyses sur l’application, par exemple les liaisons de code source. Windows Performance Analyzer est conçu pour fournir des analyses sur le système, par exemple des informations sur le système, des informations sur les événements de manipulation tactile, des informations sur les entrées/sorties disque et le coût de l’unité centrale graphique. Ces deux outils permettent de capturer et d’exporter les résultats, et peuvent rouvrir des suivis post-mortem et partagés.
-   Avant de soumettre votre application sur le Store pour certification, incorporez dans vos plans de test les tests liés aux performances décrits dans la section «Tests de performances» des [tests du Kit de certification des applications Windows](windows-app-certification-kit-tests.md) et dans la section «Performances et stabilité» des [cas de test des applications du Windows Store](https://msdn.microsoft.com/library/windows/apps/Dn275879).

Pour en savoir plus, consultez ces ressources et outils de profilage.

-   [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)
-   [Analyser les performances à l’aide des outils de diagnostic de Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/hh696636.aspx)
-   La session //build/ [Performances XAML](https://channel9.msdn.com/Events/Build/2015/3-698)
-   La session //build/ [Nouveaux outils XAML dans Visual Studio 2015](https://channel9.msdn.com/Events/Build/2015/2-697)

## <a name="respond-to-the-performance-test-results"></a>Actions suite aux résultats du test de performances

Après avoir analysé les résultats des tests de performances, déterminez si des modifications sont nécessaires, par exemple:

-   Devez-vous modifier l’une de vos conceptions d'application ou bien optimiser votre code?
-   L’instrumentation dans le code doit-elle être ajoutée, supprimée ou modifiée?
-   Devez-vous revoir l’un de vos objectifs de performances?

Si des modifications sont nécessaires, faites-les, puis revenez à l’instrumentation ou au test et répétez l’opération.

## <a name="optimizing"></a>Optimisation

Optimisez uniquement les chemins de code critiques en termes de performances, c’est-à-dire ceux qui occupent le plus de temps. Le profilage vous indiquera quels sont ces chemins. Il y a souvent un compromis à faire entre créer une application respectueuse des meilleures pratiques de conception et écrire du code qui s’exécute de façon optimale. En règle générale, il vaut mieux privilégier la productivité du développeur et la qualité de la conception pour les aspects de l’application où les performances ne sont pas essentielles.