---
Description: Lorsque vous terminez la création de soumission de votre application et cliquez sur Envoyer vers le Store, la soumission passe à l’étape de certification.
title: Processus de certification des applications
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, publication, prétraitement, certification, mise en production en attente, envoyer, publier, état, heure
ms.localizationpriority: medium
ms.openlocfilehash: 733d5ff882d7ed7c574f6fe6fedd28b79c3913d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597074"
---
# <a name="the-app-certification-process"></a>Processus de certification des applications

Une fois que vous avez terminé de créer la soumission de votre application et cliqué sur **Envoyer au Store**, la soumission passe à l’étape de certification. Ce processus s’effectue généralement en quelques heures, bien qu’il nécessite parfois trois journées ouvrables entières. Une fois votre soumission Obtient la certification, il peut prendre jusqu'à 24 heures avant que les clients voient l’annonce de l’application pour une nouvelle soumission, ou pour un envoi mis à jour avec les modifications apportées aux packages. Si votre mise à jour modifie uniquement les détails de l’annonce de Store, le processus de publication sont effectué en moins d’une heure.  Vous serez averti lorsque votre envoi est publié, et le statut de l’application dans le tableau de bord **dans le Store**.

## <a name="preprocessing"></a>Prétraitement

Une fois que vous avez chargé avec succès les packages de l’application et soumis l’application pour certification, les packages sont mis en file d’attente à des fins de test. Un message s’affiche si des erreurs sont détectées pendant le pré-traitement. Pour plus d’informations sur les erreurs potentielles, voir [Résoudre les erreurs de soumission](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Plusieurs tests sont exécutés pendant cette phase :

-   **Tests de sécurité :** Ce premier test vérifie les packages de votre application pour les virus et les logiciels malveillants. Si votre application échoue à ce test, vous devez vérifier votre système de développement en exécutant l’antivirus le plus récent, puis régénérer le package de votre application sur un système propre.
-   **Tests de compatibilité technique :** Conformité technique est testée par le Kit de Certification des applications Windows. (Vous devez toujours prendre soin de [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre au Windows Store.)
-   **Conformité du contenu :** La quantité de que cette opération varie selon la complexité de votre application est la quantité de contenu visual il possède et combien d’applications ont été soumis récemment. Veillez à fournir toutes les informations que les testeurs doivent connaître dans la page [Remarques pour la certification](notes-for-certification.md).

Une fois le processus de certification terminé, vous recevez un rapport de certification indiquant si votre application a été ou non certifiée. Si votre application n’a pas été certifiée, le rapport indique le test auquel elle a échoué ou la [stratégie](https://docs.microsoft.com/legal/windows/agreements/store-policies) qui n’a pas été respectée. Après avoir corrigé le problème, vous pouvez créer une autre soumission pour votre application afin de recommencer le processus de certification.

## <a name="release"></a>Release

Lorsque votre application obtient la certification, il est prêt à passer à la **publication** processus.

- Si vous avez indiqué que votre envoi doit être publiée dès que possible (l’option par défaut), le processus de publication commence immédiatement.
- Si c’est la première fois vous avez publié l’application, et que vous avez spécifié un **date de publication** dans le [planification](configure-precise-release-scheduling.md#release) section, l’application devient disponible en fonction de votre **Release date**sélections.
- Si vous avez utilisé [publication contiennent des options](manage-submission-options.md#publishing-hold-options) pour spécifier qu’il ne doit pas être libérée jusqu'à une certaine date, nous vous attendrons jusqu'à cette date pour commencer le processus de publication, sauf si vous sélectionnez **date de publication de modification**.
- Si vous avez utilisé [publication contiennent des options](manage-submission-options.md#publishing-hold-options) pour spécifier que vous souhaitez publier la soumission manuellement, nous ne démarre pas le processus de publication jusqu'à ce que vous sélectionniez **publier maintenant** (ou sélectionnez **modification date de publication** et choisir une date spécifique).


## <a name="publishing"></a>Publication

Les packages de votre application sont signés numériquement pour être protégés contre toute falsification après leur publication. Une fois que cette phase a commencé, vous ne pouvez plus annuler votre soumission ni en modifier la date de sortie.

Pour les applications nouvelles et mises à jour qui incluent des modifications aux packages de l’application, le processus de publication s’effectue dans les 24 heures. Pour les mises à jour uniquement modifier les options telles que les détails de l’annonce de Store, mais ne modifiez pas les packages de l’application, le processus de publication prendra moins d’une heure.

Pendant que votre application se trouve dans la phase de publication, le **afficher les détails** lien dans la colonne Status pour soumission de votre application vous permet de savoir quand vos nouveaux packages et les détails de l’annonce de Store sont disponibles pour les clients sur chaque de votre système d’exploitation pris en charge versions. Les étapes qui n’ont pas encore été réalisées s’affichent comme **En attente**. Votre application reste dans la phase de publication jusqu'à ce que le processus est terminé, ce qui signifie que les nouveaux packages et/ou détails de l’annonce sont disponibles pour tous les clients potentiels de votre application.

## <a name="in-the-store"></a>Dans le Windows Store 

Après avoir correctement effectué les étapes ci-dessus, l’état de la soumission passe de **Publication** à **Dans le Windows Store**. Les clients peuvent ainsi télécharger votre soumission à partir du Microsoft Store (sauf si vous avez choisi une autre option pour [Détectabilité](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nous effectuons également des vérifications ponctuelles sur les applications publiées pour identifier les problèmes potentiels et vérifier que votre application est conforme à toutes les [politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Si nous détectons un problème, nous vous en informons et vous indiquons comment le résoudre, ou nous vous prévenons que votre application a été retirée du Windows Store.

 

 

 




