---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: Processus de certification des applications
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, publication, prétraitement, certification, publication, en attente, soumettre, publier, état, heure
ms.localizationpriority: medium
ms.openlocfilehash: 8372f316786d83d72dff8ef7a0a8fd53e5390743
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475004"
---
# <a name="the-app-certification-process"></a>Processus de certification des applications

Une fois que vous avez terminé de créer la soumission de votre application et cliqué sur **Envoyer au Store**, la soumission passe à l’étape de certification. Ce processus s’effectue généralement en quelques heures, bien qu’il nécessite parfois troisjournées ouvrables entières. Une fois votre soumission certifiée, elle peut prendre jusqu'à 24 heures pour les clients afficher la description de l’application pour une nouvelle soumission, ou pour une soumission mise à jour avec les modifications apportées aux packages. Si votre mise à jour ne change plus d’informations de description, le processus de publication s’effectue en moins d’une heure.  Vous serez averti lorsque votre soumission est publiée et état de l’application dans le tableau de bord sera **Dans le Windows Store**.

## <a name="preprocessing"></a>Prétraitement

Une fois que vous avez chargé avec succès les packages de l’application et soumis l’application pour certification, les packages sont mis en file d’attente à des fins de test. Un message s’affiche si des erreurs sont détectées pendant le pré-traitement. Pour plus d’informations sur les erreurs potentielles, voir [Résoudre les erreurs de soumission](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Plusieurs tests sont exécutés pendant cette phase:

-   **Tests de sécurité :** Ce premier test vérifie que les packages de votre application sont exempts de virus et de programmes malveillants. Si votre application échoue à ce test, vous devez vérifier votre système de développement en exécutant l’antivirus le plus récent, puis régénérer le package de votre application sur un système propre.
-   **Tests de conformité technique :** la conformité technique est testée à l’aide du Kit de certification des applications Windows. (Vous devez toujours prendre soin de [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre au Windows Store.)
-   **Conformité du contenu :** la durée de cette opération varie en fonction de la complexité de votre application, de la quantité de contenu visuel qu’elle comporte et du nombre d’applications que vous avez soumises récemment. Veillez à fournir toutes les informations que les testeurs doivent connaître dans la page [Remarques pour la certification](notes-for-certification.md).

Une fois le processus de certification terminé, vous recevez un rapport de certification indiquant si votre application a été ou non certifiée. Si votre application n’a pas été certifiée, le rapport indique le test auquel elle a échoué ou la [stratégie](https://docs.microsoft.com/legal/windows/agreements/store-policies) qui n’a pas été respectée. Après avoir corrigé le problème, vous pouvez créer une autre soumission pour votre application afin de recommencer le processus de certification.

## <a name="release"></a>Mise sur le marché

Lorsque votre application certifiée, il est prêt à déplacer vers le processus de **publication** .

- Si vous avez indiqué que votre soumission doit être publiée dès que possible (l’option par défaut), le processus de publication commence immédiatement.
- Si c’est la première fois que vous avez publié l’application et que vous avez spécifié une **date de publication** dans la section [planification](configure-precise-release-scheduling.md#release) , l’application sera disponible en fonction de vos sélections de **date de publication** .
- Si vous avez utilisé les [options de mise en attente de publication](manage-submission-options.md#publishing-hold-options) pour spécifier qu’il ne doit pas être libérée avant une date donnée, nous attendrons jusqu'à cette date de début du processus de publication, sauf si vous sélectionnez **changer la date de publication**.
- Si vous avez utilisé les [options de mise en attente de publication](manage-submission-options.md#publishing-hold-options) pour spécifier que vous souhaitez publier la soumission manuellement, nous ne démarre pas le processus de publication jusqu'à ce que vous sélectionnez **Publier maintenant** (ou **changer la date de publication** et choisissez une date spécifique).


## <a name="publishing"></a>Publication

Les packages de votre application sont signés numériquement pour être protégés contre toute falsification après leur publication. Une fois que cette phase a commencé, vous ne pouvez plus annuler votre soumission ni en modifier la date de sortie.

Pour les applications nouvelles et mises à jour qui incluent des modifications aux packages de l’application, le processus de publication s’effectue dans les 24 heures. Pour les mises à jour uniquement modifier les options telles que des informations de description, mais ne modifiez pas les packages de l’application, le processus de publication prendra moins d’une heure.

Lorsque votre application est en phase de publication, le lien **Afficher les détails** dans la colonne État de la soumission de votre application vous permet de savoir quand vos nouveaux packages et les informations de description sont disponibles pour les clients sur chacune de vos versions de système d’exploitation pris en charge. Les étapes qui n’ont pas encore été réalisées s’affichent comme **En attente**. Votre application reste en phase de publication jusqu'à ce que le processus se termine, ce qui signifie que les nouveaux packages et/ou descriptions sont disponibles pour tous les clients potentiels de votre application.

## <a name="in-the-store"></a>Dans le Store 

Après avoir correctement effectué les étapes ci-dessus, l’état de la soumission passe de **Publication** à **Dans le Store**. Les clients peuvent ainsi télécharger votre soumission à partir du MicrosoftStore (sauf si vous avez choisi une autre option pour [Détectabilité](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nous effectuons également des vérifications ponctuelles sur les applications publiées pour identifier les problèmes potentiels et vérifier que votre application est conforme à toutes les [politiques du MicrosoftStore](https://docs.microsoft.com/legal/windows/agreements/store-policies). Si nous détectons un problème, nous vous en informons et vous indiquons comment le résoudre, ou nous vous prévenons que votre application a été retirée du WindowsStore.

 

 

 




