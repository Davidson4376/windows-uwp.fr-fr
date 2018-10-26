---
Description: Troubleshoot Microsoft Take a Test events and errors with the event viewer.
title: Résolvez à l’aide de l’observateur d’événements.
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: Windows 10, uwp, éducation
ms.localizationpriority: medium
ms.openlocfilehash: eaf4c8e2641359e6d9a92444f66a1b6d4e5e5881
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5556573"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Résolvez à l’aide de l’observateur d’événements.

Vous pouvez utiliser l’observateur d’événements pour afficher les événements et les erreurs de l’application Examen. L’application Examen consigne les événements lorsqu’une demande de verrouillage a été reçue, qu’une inscription d’appareil a été menée à bien, que des politiques de verrouillage ont été appliquées, et plus encore.

Pour activer l’affichage des événements dans l’observateur d’événements:
1. Ouvrez le dossier `Event Viewer`
2. Accédez à `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. Cliquez avec le bouton droit sur `Operational`, puis sélectionnez `Enable Log`

Pour enregistrer les journaux d’événement:
1. Clic droit `Operational`
2. Clic `Save All Events As…`
