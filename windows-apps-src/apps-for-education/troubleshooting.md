---
Description: Résolvez les événements et les erreurs de l’application Examen à l’aide de l’observateur d’événements.
title: Résolvez à l’aide de l’observateur d’événements.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, éducation
ms.localizationpriority: medium
ms.openlocfilehash: 2f4bdcf45c7dd37dd540a666d99b5fa2fd2d49f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598474"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Résolvez à l’aide de l’observateur d’événements.

Vous pouvez utiliser l’observateur d’événements pour afficher les événements et les erreurs de l’application Examen. L’application Examen consigne les événements lorsqu’une demande de verrouillage a été reçue, qu’une inscription d’appareil a été menée à bien, que des politiques de verrouillage ont été appliquées, et plus encore.

Pour activer l’affichage des événements dans l’observateur d’événements :
1. Ouvrez la `Event Viewer`
2. Accédez à `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. Avec le bouton droit `Operational` et sélectionnez `Enable Log`

Pour enregistrer les journaux d’événement :
1. Avec le bouton droit `Operational`
2. Cliquez sur `Save All Events As…`
