---
author: jnHs
Description: "Une fois que vous avez terminé de créer la soumission de votre application et cliqué sur Soumettre au Windows Store, l’application passe à l’étape de certification."
title: Processus de certification des applications
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 87fc6e35a2a9818478aca4119850ae20165d2a64
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="the-app-certification-process"></a>Processus de certification des applications


Une fois que vous avez terminé de créer la soumission de votre application et cliqué sur **Soumettre au Windows Store**, l’application passe à l’étape de certification. Ce processus s’effectue généralement en quelques heures, bien qu’il nécessite parfois troisjournées ouvrables entières. Une fois votre soumission certifiée, un maximum de 16heures peut s’écouler avant l’apparition de sa description (ou de vos mises à jour d’une application publiée précédemment) dans le WindowsStore. Une notification s’affiche lorsque votre soumission est publiée et disponible pour les clients; l’état de l’application dans le tableau de bord sera **dans le Windows Store**.

## <a name="preprocessing"></a>Prétraitement

Une fois que vous avez chargé avec succès les packages de l’application et soumis l’application pour certification, les packages sont mis en file d’attente à des fins de test. Un message s’affiche si des erreurs sont détectées pendant le pré-traitement. Pour plus d’informations sur les erreurs potentielles, voir [Résoudre les erreurs de soumission](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Plusieurs tests sont exécutés pendant cette phase:

-   **Tests de sécurité :** Ce premier test vérifie que les packages de votre application sont exempts de virus et de programmes malveillants. Si votre application échoue à ce test, vous devez vérifier votre système de développement en exécutant l’antivirus le plus récent, puis régénérer le package de votre application sur un système propre.
-   **Tests de conformité technique :** la conformité technique est testée à l’aide du Kit de certification des applications Windows. (Vous devez toujours prendre soin de [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre au Windows Store.)
-   **Conformité du contenu :** la durée de cette opération varie en fonction de la complexité de votre application, de la quantité de contenu visuel qu’elle comporte et du nombre d’applications que vous avez soumises récemment. Veillez à fournir toutes les informations que les testeurs doivent connaître dans la page [Remarques pour la certification](notes-for-certification.md).

Une fois le processus de certification terminé, vous recevez un rapport de certification indiquant si votre application a été ou non certifiée. Si votre application n’a pas été certifiée, le rapport indique le test auquel elle a échoué ou la [stratégie](https://msdn.microsoft.com/library/windows/apps/dn764944) qui n’a pas été respectée. Après avoir corrigé le problème, vous pouvez créer une autre soumission pour votre application afin de recommencer le processus de certification.

## <a name="release"></a>Mise sur le marché

Lorsque votre application obtient la certification, elle est prête pour l’étape **Publication**. Si vous avez demandé que votre soumission soit publiée le plus tôt possible, cette opération s’exécute immédiatement. Si vous avez spécifié que la sortie de l’application ne doit pas s’effectuer avant une date spécifique, nous attendrons jusqu’à cette date, sauf si vous cliquez sur le lien **Changer la date de sortie**. Si vous avez choisi de publier la soumission manuellement, nous n’effectuerons la publication que lorsque vous aurez cliqué sur le bouton **Publier maintenant**, ou cliqué sur le lien **Changer la date de sortie** et sélectionné une date spécifique.

## <a name="publishing"></a>Publication

Les packages de votre application sont signés numériquement pour être protégés contre toute falsification après leur publication. Une fois que cette phase a commencé, vous ne pouvez plus annuler votre soumission ni en modifier la date de sortie.

Lorsque votre application est en phase de publication, le lien **Afficher les détails** dans la colonne État de soumission de votre application vous indique quand vos nouveaux packages et vos descriptions dans le Store sont disponibles pour les clients sur chacune de vos versions de système d’exploitation prises en charge. Votre application reste en phase de publication jusqu’à ce que les nouveaux packages et les descriptions soient disponibles pour tous les clients potentiels de votre application. Ce processus peut prendre jusqu’à 16heures. 

## <a name="in-the-store"></a>Dans le WindowsStore 

Après avoir correctement effectué les étapes ci-dessus, l’état de la soumission passe de **Publication** à **Dans le Windows Store**. Les clients peuvent ainsi télécharger votre soumission à partir du WindowsStore (sauf si vous avez choisi une autre option [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility)). 

**Remarque** Nous effectuons également des vérifications ponctuelles sur les applications publiées pour identifier les problèmes potentiels et vérifier que votre application est conforme à toutes les [politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944). Si nous détectons un problème, nous vous en informons et vous indiquons comment le résoudre, ou nous vous prévenons que votre application a été retirée du WindowsStore.

 

 

 




