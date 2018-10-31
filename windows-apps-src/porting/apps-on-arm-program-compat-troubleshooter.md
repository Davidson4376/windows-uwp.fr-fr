---
title: Utilitairede résolution de problèmes de compatibilité des programmes sur ARM
author: msatranjr
description: Conseils de réglage des paramètres de compatibilité si votre application ne fonctionne pas correctement sur ARM
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows10s, toujours connecté, résolution des problèmes de compatibilité, windows sur ARM
ms.localizationpriority: medium
ms.openlocfilehash: 4765ad324e90167c7279c9245bccd840bce1163d
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839523"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Utilitairede résolution de problèmes de compatibilité des programmes sur ARM
L'émulation visant à la prises en charge des applications x86 constitue la nouvelle fonctionnalité, créée pour Windows10 sur ARM64. Il arrive que l'émulation réalise des tâches d'optimisation qui ne parviennent pas à rendre la meilleure expérience. Vous pouvez utiliser l'utilitaire de résolution des problèmes de compatibilité des programmes pour permuter les paramètres d'émulation de votre application x86 et ainsi réduire les optimisations par défaut et potentiellement augmenter la compatibilité.

## <a name="start-the-program-compatibility-troubleshooter"></a>Démarrer l’utilitaireRésolution de problèmes de compatibilité des programmes
Vous démarrez manuellement l'[utilitaire de résolution des problèmes de compatibilité des programmes](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible) de la même manière que sur tout PC Windows10: cliquez avec le bouton droit sur le fichier d'exécution (.exe) et sélectionnez **Résoudre les problèmes de compatibilité**. Cet écran s’affiche.

![Capture d'écran de l'option Résoudre les problèmes de compatibilité](images/arm/Capture4.png)

Si vous cliquez sur **Résoudre les problèmes du programme**, vous obtenez les options suivantes.

![Capture d'écran de l'option Résoudre les problèmes de compatibilité](images/arm/Capture5.png)

Toutes les options activent les paramètres qui sont applicables et appliquées à tous les PC de bureau Windows10. En outre, la première, deuxième et la quatrième option appliquent les paramètres d'émulation [Désactiver le cache d’application](#disable-app-cache) et [Désactiver le mode d’exécution hybride](#disable-hybrid-exec-mode).

## <a name="toggling-emulation-settings"></a>Activation/désactivation des paramètres d'émulation
> [!WARNING]
> La modification des paramètres d'émulation peut provoquer l'arrêt inattendu ou empêcher totalement le lancement de l'application.

Vous pouvez basculer les paramètres d’émulation de cliquant avec le bouton droit sur le fichier d'exécution et en sélectionnant **Propriétés**.

Sur l'ARM, une section intitulée **Windows 10 sur ARM** sera disponible dans l'onglet **Compatibilité**. Cliquez sur **Modifier les paramètres d'émulation** pour lancer une deuxième fenêtre, comme ci-après.

![Modifier les paramètres d'émulation, Capture d'écran](images/arm/Capture.png)

Cette fenêtre offre deux possibilité pour modifier les paramètres d'émulation. Vous pouvez sélectionnez un groupe prédéfini de paramètres d'émulation, ou cliquer sur l'option **Utiliser les paramètres avancés** pour activer le choix des paramètres individuels.

Les paramètres d'émulation groupés réduisent les optimisations de performance en faveur de la qualité. Vous trouverez ci-dessus certains des paramètres groupés que vous avez sélectionnés.

![Modifier les paramètres d'émulation, Capture d'écran n°2](images/arm/Capture2.png)

Sélectionnez **Utiliser les paramètres avancés** pour choisir les paramètres individuels, comme décrit dans le tableau suivant.

| Paramètre d’émulation | Résultat |
| ----------------- | ----------- |
| <p id="disable-app-cache">Désactiver le cache d'application</p> | Le système d'exploitation met en cache les blocs de code compilés afin de réduire le traitement d'émulation sur les exécutions qui suivent. Ce paramètre nécessite que l'émulateur recompile tous les codes d'application lors de l'exécution. |
| <p id="disable-hybrid-exec-mode">Désactiver le mode d’exécution hybride</p> | L'exécution portable hybride compilé (CHPE), les binaires sont compatibles avec les applications x86 qui incluent le code ARM64 natif pour améliorer la performance, mais sont parfois incompatibles avec certaines applications. Ce paramètre force l'utilisation des binaires uniquement de type x86. |
| Prise en charge du code à modification automatique stricte | Activez cette option pour assurer que le code à modification automatique soit correctement pris en charge dans l'émulation. Les scénarios les plus courants concernant le code à modification automatique sont couverts par le comportement de l'émulateur par défaut. L'activation de cette option réduit grandement la performance du code à modification automatique lors de l'exécution. |
| Désactiver l'optimisation de la performance de page RWX | Cette optimisation améliore la performance du code sur des pages lisibles, inscriptibles et exécutables (RWX), mais n'est pas compatible avec certaine applications. |

Vous pouvez également sélectionner les paramètres multicœurs, comme illustré ici.

![Paramètres multicœurs, Capture d'écran](images/arm/Capture3.png)

Ces paramètres modifient la quantité de barrières mémoires utilisées pour synchroniser les accès à la mémoire ente les cœurs dans les applications lors de l'émulation. Le mode par défaut est **Rapide**, mais les options **strict** et **très strict** augmentent le nombre de barrières. Cela a pour effet de ralentir l'application, mais également de réduire le risque d'erreurs de l'application. L'option **Cœur unique** supprime toutes les barrières, mais force les threads d'application à s'exécuter sur un cœur unique.

Si la modification d'un paramètre spécifique permet de résoudre votre problème, merci d'envoyer un e-mail détaillé à l'adresse *woafeedback@microsoft.com* afin que nous puissions agir en conséquence de votre commentaire.