---
author: TylerMSFT
title: "Lancement, reprise et tâches en arrière-plan"
description: "Cette section décrit ce qui se produit en cas de démarrage, de suspension, de reprise et d’arrêt d’une application de plateforme Windows universelle (UWP)."
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.sourcegitcommit: a8e6145f7a5c75d3b37277b80b07b0b3ad739d5c
ms.openlocfilehash: ab20c4af5b9a87dc73775d304c314c9861d989d4

---

# Lancement, reprise et tâches en arrière-plan


\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Cette section décrit ce qui se produit en cas de démarrage, de suspension, de reprise et d’arrêt d’une application de plateforme Windows universelle (UWP). Elle explique comment activer les applications à l’aide d’un contrat ou d’une extension, et comment utiliser des tâches en arrière-plan qui permettent à une application UWP d’opérer même quand elle n’est pas au premier plan. Enfin, elle traite de la manière d’ajouter un écran de démarrage à votre application.

## Le cycle de vie de l’application

| Rubrique                                            | Description                                                                                                     |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Cycle de vie de l’application](app-lifecycle.md)               | Découvrez le cycle de vie d’une application UWP et ce qui se passe quand Windows lance, suspend et reprend l’exécution de votre application. |
| [Gérer le prélancement d’une application](handle-app-prelaunch.md) | Découvrez comment gérer le prélancement d’une application.                                                                              |
| [Gérer l’activation d’une application](activate-an-app.md)     | Découvrez comment gérer l’activation d’une application.                                                                             |
| [Gérer la suspension d’une application](suspend-an-app.md)         | Découvrez comment enregistrer d’importantes données d’application lorsque le système suspend l’exécution de votre application.                                 |
| [Gérer la reprise d’une application](resume-an-app.md)           | Découvrez comment actualiser le contenu à l’écran quand le système reprend l’exécution de votre application.                                        |

 

## Lancer des applications


| Activation d’URI et de fichier                                                                         | Description                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Lancer une application pour obtenir des résultats](how-to-launch-an-app-for-results.md)                               | Découvrez comment démarrer une application à partir d’une autre et échanger des données entre les deux.                                                                                             |
| [Lancer l’application par défaut pour un URI](launch-default-app.md)                                      | Découvrez comment lancer l’application par défaut d’un URI (Uniform Resource Identifier).                                                                                               |
| [Gérer l’activation des URI](handle-uri-activation.md)                                              | Découvrez comment inscrire une application afin qu’elle devienne le gestionnaire par défaut pour un nom de schéma d’URI.                                                                                          |
| [Lancer l’application par défaut d’un fichier](launch-the-default-app-for-a-file.md)                      | Découvrez comment lancer l’application par défaut pour un type fichier.                                                                                                                       |
| [Gérer l’activation des fichiers](handle-file-activation.md)                                            | Découvrez comment inscrire votre application pour qu’elle soit le gestionnaire par défaut pour un type de fichier.                                                                                                  |
| [Recommandations en matière de types de fichiers et d’URI](https://msdn.microsoft.com/library/windows/apps/hh700321) | Si vous comprenez la relation entre les applications UWP et les types de fichiers et protocoles qu’elles prennent en charge, vous pouvez fournir une expérience plus cohérente et plus engageante pour vos utilisateurs. |
| [Noms de schéma d’URI et de fichier réservés](reserved-uri-scheme-names.md)                             | Cette rubrique répertorie les noms de schéma d’URI et de fichier réservés, indisponibles pour votre application.                                                                                |
| [Lancer l’application Paramètres Windows](launch-settings-app.md)                                      | Découvrez comment lancer l’application Paramètres Windows.                                                                                                                              |
| [Lancer l’application du Windows Store](launch-store-app.md)                                            | Découvrez comment lancer l’application du Windows Store.                                                                                                                                 |
| [Lancer l’application Cartes Windows](launch-maps-app.md)                                              | Découvrez comment lancer l’application Cartes Windows.                                                                                                                                  |

 

## Services et tâches en arrière-plan



| Rubrique                                                                                                            | Description                                                                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)                             | Les rubriques de cette section expliquent comment exécuter votre propre code léger en arrière-plan en répondant aux déclencheurs au moyen de tâches en arrière-plan.                                                       |
| [Accéder aux capteurs et aux appareils à partir d’une tâche en arrière-plan](access-sensors-and-devices-from-a-background-task.md)       | [ **DeviceUseTrigger** ](https://msdn.microsoft.com/library/windows/apps/dn297337) permet à votre application Windows universelle d’accéder aux capteurs et aux appareils en arrière-plan, même si votre application au premier plan est suspendue. |
| [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)                                           | Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan.                                                                                                                          |
| [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md)                                | Découvrez comment écrire une application UWP capable de fournir des services à d’autres applications UWP, et comment utiliser ces services.                                                                                  |
| [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md)                               | Créez une classe de tâche en arrière-plan et inscrivez-la pour permettre son exécution lorsque votre application ne se trouve pas au premier plan.                                                                                                 |
| [Déboguer une tâche en arrière-plan](debug-a-background-task.md)                                                           | Découvrez comment déboguer une tâche en arrière-plan, notamment dans le cadre de son activation et du suivi de débogage dans le journal des événements Windows.                                                                        |
| [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md) | Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application.                                                                                                       |
| [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)                                     | Découvrez comment faire en sorte qu’une tâche en arrière-plan reconnaisse les demandes d’annulation et arrête le travail, tout en signalant l’annulation à l’application utilisant le dispositif de stockage persistant.                                     |
| [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)           | Découvrez comment votre application peut reconnaître la progression et l’achèvement d’une tâche en arrière-plan.                                                                                                                     |
| [Inscrire une tâche en arrière-plan](register-a-background-task.md)                                                     | Découvrez comment créer une fonction que vous pouvez réutiliser pour inscrire la plupart des tâches en arrière-plan en toute sécurité.                                                                                                  |
| [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)             | Découvrez comment créer une tâche en arrière-plan qui répond aux événements [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).                                                                         |
| [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)                                        | Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.                                                                                                          |
| [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)                 | Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera.                                                                                                                  |
| [Transférer des données en arrière-plan](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | Utilisez l’API de transfert en arrière-plan pour copier des fichiers en arrière-plan.                                                                                                                              |
| [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)                       | Utilisez une tâche en arrière-plan pour mettre à jour une vignette dynamique de votre application avec du contenu actualisé.                                                                                                                      |
| [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)                                                       | Découvrez comment utiliser la classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) pour exécuter du code léger en arrière-plan tandis que l’appareil est branché.                             |

 

## Ajouter un écran de démarrage


Toutes les applications UWP doivent disposer d’un écran de démarrage constitué d’une image d’écran de démarrage et d’une couleur d’arrière-plan, que vous pouvez personnaliser.

Votre écran de démarrage s’affiche dès que l’utilisateur lance votre application. Il fournit des informations pendant que les ressources d’applications sont initialisées. Dès que votre application autorise l’interaction, l’écran de démarrage disparaît.

Un écran de démarrage bien conçu peut contribuer à rendre votre application plus attrayante. Voici un écran de démarrage simple et discret:

![aperçu de l’exemple d’écran de démarrage défini à l’échelle de 75%.](images/regularsplashscreen.png)

Cet écran de démarrage est créé en combinant un arrière-plan vert et une image PNG transparente.

La combinaison de l’image et de la couleur d’arrière-plan choisies permet d’obtenir un écran de démarrage qui s’affiche correctement quel que soit l’appareil sur lequel votre application est installée. Lorsque l’écran de démarrage apparaît, seules les dimensions de l’arrière-plan sont modifiées afin de prendre en charge toute une variété de tailles d’écran. Votre image reste toujours intacte.

De plus, vous pouvez utiliser la classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) pour personnaliser l’expérience de démarrage de votre application. Vous pouvez placer un écran de démarrage étendu créé par vos propres soins, afin d’accorder à votre application davantage de temps pour effectuer des tâches supplémentaires telles que la préparation de l’interface utilisateur ou l’achèvement des opérations de mise en réseau. Vous pouvez également utiliser la classe **SplashScreen** pour vous avertir lorsque l’écran de démarrage est supprimé, ce qui vous permet de lancer les animations d’entrée.

| Rubrique                                                                          | Description                                                                                                                                                                                       |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Ajouter un écran de démarrage](add-a-splash-screen.md)                                 | Définissez l’image et la couleur d’arrière-plan de l’écran de démarrage de votre application.                                                                                                                                          |
| [Afficher un écran de démarrage plus longtemps](create-a-customized-splash-screen.md) | Affichez un écran de démarrage plus longtemps en créant et en affichant un écran de démarrage étendu pour votre application. Cet écran étendu imite l’écran de démarrage affiché quand votre application est lancée. Il peut être personnalisé. |

 

 

 



<!--HONumber=Jun16_HO4-->


