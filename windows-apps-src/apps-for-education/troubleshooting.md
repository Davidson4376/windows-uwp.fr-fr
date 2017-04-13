---
author: TylerMSFT
Description: "Résolvez les événements et les erreurs de l’application Examen à l’aide de l’observateur d’événements."
title: "Résolvez à l’aide de l’observateur d’événements."
ms.openlocfilehash: 1b99b959cfdde997f7995c1bdf40d51921b2f1d5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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
