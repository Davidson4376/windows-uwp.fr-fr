---
Cycle de vie de l’application
Cette rubrique décrit le cycle de vie d’une application de plateforme Windows universelle (UWP), de son activation jusqu’à sa fermeture.
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
---

# Cycle de vie de l’application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Classe Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)
-   [**Espace de noms Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)

Cette rubrique décrit le cycle de vie d’une application de plateforme Windows universelle (UWP), de son activation jusqu’à sa fermeture. De nombreux utilisateurs répartissent leur travail et jouent sur plusieurs appareils et applications. Les utilisateurs s’attendent désormais à ce que votre application mémorise son état tandis qu’ils exécutent diverses tâches sur leur appareil. Par exemple, ils s’attendent à ce qu’une page conserve sa position de défilement et à ce que tous les contrôles restent dans le même état qu’avant. En comprenant le cycle de vie de l’application, lancement, suspension et reprise inclus, vous pouvez offrir ce genre de comportement transparent.

## État d’exécution de l’application


Cette illustration représente les transitions entre les états d’exécution de l’application. Les sections suivantes décrivent ces états et événements. Pour plus d’informations sur chaque transition d’état et quelle doit être la réponse de votre application, voir la documentation relative à l’énumération [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

![Diagramme d’état illustrant les transitions entre les états d’exécution de l’application](images/state-diagram.png)

## Déploiement


Pour qu’une application puis être activée, elle doit être déployée. Votre application est déployée quand un utilisateur l’installe ou quand vous utilisez Visual Studio pour la générer et l’exécuter en cours des phases de développement et de test. Pour plus d’informations sur le déploiement de base et sur les scénarios de déploiement avancé, voir [Packages et déploiement d’application](https://msdn.microsoft.com/library/windows/apps/hh464929).

## Lancement d’une application


Une application est lancée quand sont état est **NotRunning** et lorsque l’utilisateur appuie sur sa vignette dans l’écran d’accueil ou dans la liste des applications. Les applications fréquemment utilisées peuvent également être prélancées pour optimiser la réactivité (voir [Gérer le prélancement d’une application](handle-app-prelaunch.md)). Une application peut être en l’état **NotRunning** parce qu’elle n’a jamais été lancée, parce qu’elle était en cours d’exécution avant d’être bloquée ou parce qu’elle a été suspendue, mais n’a pas pu être conservée en mémoire et a été arrêtée par le système. Le lancement diffère de l’activation. L’activation est l’action consistant à activer une application via un contrat ou une extension telle que le contrat de recherche.

Lors du lancement d’une application, la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) est appelée, y compris quand l’application est suspendue en mémoire. Le paramètre [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contient l’état précédent de votre application et les arguments d’activation.

Lorsque l’utilisateur bascule vers votre application arrêtée, le système envoie les arguments [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731), avec [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) défini sur **Launch** et [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) sur **Terminated** ou **ClosedByUser**. L’application doit charger ses données d’application enregistrées et actualiser son contenu à l’écran.

Si la valeur de [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) est **NotRunning**, l’application doit redémarrer comme s’il s’agissait d’un lancement initial.

Lors du lancement d’une application, Windows affiche un écran de démarrage pour l’application. Pour configurer l’écran de démarrage, voir [Ajout d’un écran de démarrage](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331).

Lorsque l’écran de démarrage est affiché, votre application doit préparer son interface utilisateur. Les tâches principales pour l’application consistent à inscrire des gestionnaires d’événements et à configurer l’interface utilisateur personnalisée dont elle a besoin pour le chargement de la page initiale. Ces tâches ne doivent prendre que quelques secondes. Si une application doit demander des données au réseau ou récupérer de grandes quantités de données à partir du disque, ces activités doivent être effectuées hors activation. Une application peut utiliser sa propre interface utilisateur de chargement personnalisée ou un écran de démarrage étendu pendant qu’elle attend la fin de ces longues opérations. Pour plus d’informations, voir [Afficher un écran de démarrage plus longtemps](create-a-customized-splash-screen.md) et l’[Exemple d’écran de démarrage](http://go.microsoft.com/fwlink/p/?linkid=234889). Une fois que l’application a terminé l’activation, elle passe en l’état **Running** et l’écran de démarrage disparaît (toutes les ressources et tous les objets sont effacés).

## Activation d’une application


Une application peut être activée par l’utilisateur via une série d’extensions et de contrats tels qu’un contrat de partage. Pour obtenir la liste des manières d’activer votre application, voir [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

La classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) définit des méthodes que vous pouvez remplacer pour traiter les différents types d’activation. Certains types d’activations ont une méthode spécifique que vous pouvez remplacer, par exemple, [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336), etc. Pour les autres types d’activations, remplacez la méthode [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

Le code d’activation de votre application peut réaliser un test pour déterminer la raison de son activation et si elle se trouvait déjà dans l’état **Running**.

Votre application peut restaurer des données enregistrées durant son activation au cas où le système d’exploitation mettrait fin à son exécution, et où l’utilisateur la relancerait par la suite. Windows peut arrêter votre application une fois qu’elle a été suspendue pour différentes raisons. L’utilisateur peut fermer manuellement votre application ou se déconnecter, ou le système peut être sur le point de manquer de ressources. Si l’utilisateur lance votre application après que Windows l’a arrêtée, l’application reçoit un rappel [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) et l’utilisateur voit l’écran de démarrage de votre application jusqu’à ce que celle-ci soit activée. Vous pouvez utiliser cet événement pour déterminer si votre application doit restaurer les données qu’elle avait enregistrées lors de sa dernière suspension ou si vous devez charger les données par défaut de votre application. Lorsque l’écran de démarrage est affiché, le code de votre application peut consacrer du temps de traitement à effectuer cette opération sans retard apparent pour l’utilisateur. Les problèmes mentionnés précédemment concernant les opérations de longue durée subsistent cependant aussi quand vous redémarrez l’application ou que continuez à l’utiliser.

Les données de l’événement [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) incluent une propriété [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) qui indique l’état dans lequel se trouvait l’application avant son activation. Cette propriété est l’une des valeurs de l’énumération [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

| Raison de l’arrêt                                                        | Valeur de la propriété [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) | Action à entreprendre          |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| Terminé par le système (par exemple, en raison de contraintes liées aux ressources)       | **Terminated**                                                                                          | Restaurer les données de session    |
| Fermé par l’utilisateur ou processus interrompu par l’utilisateur                             | **ClosedByUser**                                                                                        | Démarrer avec les données par défaut |
| Arrêtée de manière inattendue, ou l’application n’a pas été exécutée depuis le démarrage de la *session utilisateur active* | **NotRunning**                                                                                          | Démarrer avec les données par défaut |

 

**Remarque** La *session utilisateur active* est basé sur l’ouverture de session Windows. Tant que l’utilisateur actuel ne s’est pas déconnecté ou n’a pas arrêté la session de manière explicite, ou que Windows n’a pas redémarré pour une autre raison, la session utilisateur active persiste à travers les événements, tels que l’authentification de l’écran de verrouillage, le changement d’utilisateur, etc.

 

[
            **PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) pourrait également avoir la valeur **Running** ou **Suspended** mais, dans ce cas, votre application n’a pas été arrêtée précédemment et vous n’avez donc pas à restaurer de données, car tout est déjà en mémoire.

**Remarque**  

Si vous ouvrez une session en utilisant le compte Administrateur de l’ordinateur, vous ne pouvez activer aucune application UWP.

Pour plus d’informations, voir [Extensions d’application](https://msdn.microsoft.com/library/windows/apps/hh464906).

### **OnActivated** et activations spécifiques

La méthode [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) permet de gérer tous les types d’activations possibles. Toutefois, il est plus courant d’utiliser différentes méthodes pour gérer les types d’activation les plus courants et d’utiliser **OnActivated** uniquement comme méthode de secours pour les types d’activation moins courants. Par exemple, [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) a une méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) qui est appelée sous forme de rappel chaque fois que [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) est **Launch**. Il s’agit de l’activation standard pour la plupart des applications. Il existe 6 autres méthodes **On\*** pour des activations spécifiques : [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336) et[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806). Les modèles de départ pour une application XAML ont une implémentation pour **OnLaunched** et un gestionnaire pour [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341).

## Suspension d’une application


Le système suspend votre application chaque fois que l’utilisateur passe à une autre application, au Bureau ou à l’écran d’accueil. Votre application peut également être suspendue quand l’appareil passe à l’état de faible consommation d’énergie. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan.

Quand l’utilisateur déplace une application à l’arrière-plan, Windows attend quelques secondes pour voir si l’utilisateur revient immédiatement à l’application afin que, le cas échéant, la transition soit rapide. Si l’utilisateur ne revient pas dans l’application dans ce laps de temps, Windows la suspend.

Si vous devez effectuer des tâches asynchrones lorsque votre application est en cours de suspension, vous devez différer l’exécution de la suspension tant que vos tâches ne sont pas terminées. Vous pouvez utiliser la méthode [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) sur l’objet [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) (disponible via les arguments d’événement) pour retarder l’exécution de la suspension jusqu’à ce que vous appeliez la méthode [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) sur l’objet [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) retourné.

Le système tente de conserver votre application et ses données en mémoire pendant sa suspension. Toutefois, si le système ne dispose pas des ressources pour conserver votre application en mémoire, il met fin à votre application. Les applications ne sont pas notifiées de leur arrêt. De ce fait, vous ne pouvez enregistrer les données de votre application qu’au cours de la suspension. Quand une application détermine qu’elle a été activée après avoir été arrêtée, elle doit charger les données qu’elle avait enregistrées lors de la suspension pour retrouver l’état qui était le sien avant sa suspension. Quand l’utilisateur bascule à nouveau vers une application suspendue qui a été arrêtée, l’application doit restaurer ses données dans sa méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). Le système ne vous notifie pas de l’arrêt d’une application. Celle-ci doit donc enregistrer ses données d’application et libérer les ressources exclusives et descripteurs de fichiers au moment où elle est mise en suspens pour ensuite les restaurer lorsque l’application est activée après avoir été arrêtée.

Si une application a inscrit un gestionnaire d’événements pour l’événement [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), ce code est appelé juste avant la suspension de l’application. Vous pouvez utiliser le gestionnaire d’événements pour enregistrer des données d’utilisateur et d’application. Pour cela, nous vous recommandons d’utiliser les API de données d’application, car elles terminent l’opération avant que l’application n’entre dans l’état **Suspended**. Pour plus d’informations, voir [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/library/windows/apps/mt299098).

Vous devez également libérer les ressources exclusives et les descripteurs de fichiers afin de permettre aux autres applications d’y accéder pendant que votre application ne les utilise pas. Appareils photo, périphériques d’E/S, appareils externes et ressources réseau sont autant d’exemples de ressources exclusives. En libérant explicitement les ressources exclusives et les descripteurs de fichiers, vous permettez aux autres applications d’y accéder pendant que votre application ne les utilise pas. Quand l’application est activée après un arrêt, elle doit rouvrir ses ressources exclusives et descripteurs de fichiers.

En règle générale, votre application doit immédiatement enregistrer son état ainsi que libérer ses ressources et descripteurs de fichiers lors de la gestion de l’événement de suspension. L’exécution du code ne doit pas prendre plus d’une seconde. Si une application ne réagit pas dans les secondes suivant l’événement de suspension, Windows considère que l’application a cessé de répondre et l’arrête.

Certaines applications doivent continuer de s’exécuter pour effectuer des tâches en arrière-plan. Par exemple, votre application peut continuer de lire du contenu audio en arrière-plan. Pour plus d’informations, voir [Contenu audio en arrière-plan](https://msdn.microsoft.com/library/windows/apps/mt282140). Les opérations de transfert en arrière-plan continuent même si votre application est suspendue, voire arrêtée. Pour plus d’informations, voir [Comment télécharger un fichier](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer).

Pour obtenir des recommandations, voir [Recommandations en matière d’interruption et de reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088).

**Remarque concernant le débogage à l’aide de Visual Studio : **Visual Studio empêche Windows de suspendre une application qui est jointe au débogueur afin que l’utilisateur puisse voir l’interface de débogage de Visual Studio pendant l’exécution de l’application. Lorsque vous déboguez une application, vous pouvez lui envoyer un événement de suspension à l’aide de Visual Studio. Assurez-vous que la barre d’outils **Emplacement de débogage** est visible et cliquez sur l’icône **Suspendre**.

## Visibilité de l’application


Quand l’utilisateur passe de votre application à une autre, votre application n’est plus visible, mais demeure dans l’état **Running** jusqu’à ce que Windows la suspende. Si l’utilisateur quitte votre application, mais l’active ou y revient avant qu’elle ne puisse être suspendue, l’application reste en l’état **Running**.

Votre application ne reçoit pas d’événement d’activation quand sa visibilité change, car elle est toujours en cours d’exécution. Windows quitte l’application et y revient simplement autant de fois que nécessaire. Si votre application doit faire quelque chose quand l’utilisateur la quitte et y revient, gérez l’événement [**Window.VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458).

Ne tenez pas compte du classement spécifique de ces événements.

## Reprise d’une application


Une application suspendue est reprise quand l’utilisateur bascule vers elle ou quand elle est active au moment où l’appareil quitte un état de faible consommation d’énergie.

Pour obtenir une énumération des états possibles pour votre application lors de sa reprise, voir [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694). Lorsqu’une application quitte l’état **Suspended**, elle entre dans l’état **Running** et reprend là où elle s’est arrêtée. Aucune donnée d’application stockée en mémoire n’est perdue. Par conséquent, la plupart des applications n’ont aucune opération à effectuer lors de leur reprise. Toutefois, l’application peut avoir été suspendue pendant des heures, voire des jours. Si votre application présente du contenu ou des connexions réseau caduques, il convient de les actualiser lors de la reprise. Si une application a inscrit un gestionnaire d’événements pour l’événement [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), ce gestionnaire d’événements est appelé lorsque l’application reprend en quittant l’état **Suspended**. Vous pouvez actualiser le contenu et les données de votre application à l’aide de ce gestionnaire d’événements.

Si une application suspendue est activée pour participer à un contrat ou une extension d’application, elle reçoit d’abord l’événement **Resuming**, puis l’événement **Activated**.

Lorsqu’une application est suspendue, elle ne reçoit aucun des événements réseau pour la réception desquels elle est inscrite. Ces événements réseau ne sont pas mis en file d’attente, mais simplement manqués. Par conséquent, votre application doit tester l’état du réseau lors de sa reprise.

**Remarque** Dans la mesure où l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) n’est pas déclenché à partir du thread d’interface utilisateur, il convient d’utiliser un répartiteur si le code de votre gestionnaire de reprise communique avec votre interface utilisateur.

 

Pour obtenir des recommandations, voir [Recommandations en matière d’interruption et de reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Fermeture d’une application


Généralement, les utilisateurs n’ont pas besoin de fermer les applications et peuvent laisser Windows les gérer. Toutefois, ils peuvent décider de fermer une application en effectuant un mouvement de fermeture ou en appuyant sur Alt+F4 (sur Windows) ou en utilisant le sélecteur de tâche (sur Windows Phone).

Il n’existe aucun événement spécial pour indiquer que l’utilisateur a fermé l’application.

Une fois qu’une application a été fermée par l’utilisateur, elle est d’abord suspendue et arrêtée, puis entre en l’état **NotRunning**.

Sous Windows 8.1 et versions ultérieures, une fois qu’une application a été fermée par l’utilisateur, elle est supprimée de l’écran et de la liste de répartition sans être arrêtée de manière explicite.

Si un gestionnaire d’événements pour l’événement **Suspending** a été inscrit par une application, il est appelé lors de la suspension de l’application. Vous pouvez utiliser ce gestionnaire d’événements pour enregistrer des données utilisateur et d’application pertinentes sur un périphérique de stockage persistant.

**Fermée par l’utilisateur : si votre application doit se comporter différemment selon qu’elle est fermée par l’utilisateur ou par Windows, vous pouvez utiliser le gestionnaire d’événements d’activation pour déterminer si l’application a été arrêtée par l’utilisateur ou par Windows. Voir les descriptions des états **ClosedByUser** et **Terminated** dans la documentation relative à l’énumération [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

Nous recommandons que les applications ne puissent se fermer par programme qu’en cas d’absolue nécessité. Par exemple, si une application détecte une fuite de mémoire, elle peut se fermer pour assurer la sécurité des données personnelles de l’utilisateur. Lorsque vous fermez une application par programme, le système considère qu’il s’agit d’un blocage d’application.

## Blocage d’application


La procédure en cas de blocage du système est conçue pour permettre aux utilisateurs de revenir à ce qu’ils étaient en train de faire, aussi rapidement que possible. Vous ne devez pas fournir de boîte de dialogue d’avertissement ou d’autres notifications, car celles-ci retarderont l’utilisateur.

Si votre application se bloque, cesse de répondre ou génère une exception, un rapport de problèmes est envoyé à Microsoft via les [paramètres de commentaires et diagnostics](http://go.microsoft.com/fwlink/p/?LinkID=614828) de l’utilisateur. Microsoft vous fournit un sous-ensemble des données d’erreur dans le rapport de problèmes pour que vous puissiez les utiliser afin d’améliorer votre application. Vous pouvez consulter ces données dans la page Qualité du tableau de bord.

Lorsque l’utilisateur active une application après une panne, son gestionnaire d’événements d’activation reçoit une valeur [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) de **NotRunning** et doit afficher son interface utilisateur et ses données d’origine. Après une panne, n’utilisez pas de manière automatique les données d’application utilisées pour **Resuming** avec **Suspended**, car ces données peuvent être endommagées. Consultez [Recommandations en matière d’interruption et de reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Suppression d’une application


Lorsqu’un utilisateur supprime votre application, elle est supprimée avec toutes ses données locales. La suppression d’une application n’affecte pas les données de l’utilisateur stockées dans des emplacements communs, telles les bibliothèques de documents ou d’images.

## Cycle de vie de l’application et modèles de projet Visual Studio


Le code de base pertinent pour le cycle de vie de l’application est fourni dans les modèles de projet de démarrage Visual Studio. L’application de base gère l’activation au lancement, fournit un emplacement pour restaurer vos données d’application, et affiche l’interface utilisateur principale avant même que vous ayez ajouté votre propre code. Pour plus d’informations, voir [Modèles de projet en C#, VB et C++ pour les applications](https://msdn.microsoft.com/library/windows/apps/hh768232).

## API de clé du cycle de vie de l’application


-   Espace de noms [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691)
-   Espace de noms [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   Espace de noms [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   Classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) (XAML)
-   Classe [**Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) (XAML)

**Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


* [Recommandations en matière d’interruption et de reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Gérer le prélancement d’une application](handle-app-prelaunch.md)
* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la suspension d’une application](suspend-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)

 

 





<!--HONumber=Mar16_HO1-->


