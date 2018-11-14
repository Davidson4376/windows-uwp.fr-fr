---
author: normesta
description: Comment utiliser les informations relatives aux contacts et calendriers dans votre application UWP.
title: Contacts et calendriers
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
keywords: windows10, uwp, contacts, calendrier, rendez-vous, messages électroniques
ms.localizationpriority: medium
ms.openlocfilehash: c020a871863df6fac3dabc3ffab4bafc57227b50
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6192920"
---
# <a name="contacts-my-people-and-calendar"></a>Contacts, Mes Contacts et calendrier


Vous pouvez permettre aux utilisateurs d’accéder à leurs contacts et rendez-vous afin de partager du contenu, des courriers électroniques, des informations de calendrier ou des messages, ou toute autre fonctionnalité de votre conception.

Pour en savoir plus sur les différentes façons d’accéder aux contacts et aux rendez-vous dans votre application, voir les rubriques suivantes:

| Rubrique | Description |
|-------|-------------|
| [Sélectionner des contacts](selecting-contacts.md) | L’espace de noms [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002) fournit plusieurs options de sélection des contacts. Ici, nous allons vous montrer comment sélectionner un ou plusieurs contacts, et comment configurer le sélecteur de contacts pour récupérer uniquement les informations de contact dont votre application a besoin. |
| [Envoyer un courrier électronique](sending-email.md) | Montre comment lancer la boîte de dialogue de rédaction d’un courrier électronique pour permettre à l’utilisateur d’envoyer un courrier électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi. |
| [Envoyer un SMS](sending-an-sms-message.md) | Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un SMS pour permettre à l’utilisateur d’envoyer un SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi. |
| [Gérer des rendez-vous](managing-appointments.md) | L’espace de noms [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359) vous permet de créer et gérer des rendez-vous dans l’application Calendrier d’un utilisateur. Ici, nous allons vous montrer comment créer un rendez-vous, l’ajouter à l’application Calendrier, le remplacer dans l’application Calendrier et le supprimer de l’application Calendrier. Nous allons également expliquer comment afficher une période de temps pour une application Calendrier et créer un objet appointment-recurrence. |
| [Connecter votre application à des actions sur une carte de visite](integrating-with-contacts.md) | Explique comment faire apparaître votre application à côté des actions sur une carte de visite ou une mini carte de visite. Les utilisateurs peuvent choisir votre application pour effectuer une action telle qu’ouvrir une page de profil, effectuer un appel ou envoyer un message. |
| [Ajout de la prise en charge de Mes Contacts à une application](my-people-support.md) | Explique comment ajouter la prise en charge de Mes Contacts à une application et comment épingler et désépingler des contacts sur la barre des tâches. |
| [Partage de Mes Contacts](my-people-sharing.md) | Explique comment ajouter la prise en charge du partage de Mes Contacts qui permet aux utilisateurs de partager du contenu avec leurs contacts épinglés en faisant glisser des fichiers à partir de l’Explorateur de fichiers vers une épingle de Mes Contacts. |
| [Notifications de mes contacts](my-people-notifications.md) | Explique comment créer et utiliser les Notifications de Mes Contacts, un nouveau type de notification toast envoyée à partir d'un contact épinglé. |

 

## <a name="related-topics"></a>Rubriquesconnexes

* [Exemple d’API de rendez-vous](http://go.microsoft.com/fwlink/p/?linkid=309836)
* [Exemple d’API du Gestionnaire de contacts](http://go.microsoft.com/fwlink/p/?LinkID=310079)
* [Exemple d’application du sélecteur de contacts](http://go.microsoft.com/fwlink/p/?linkid=231575)
* [Exemple de gestion des actions de contact](http://go.microsoft.com/fwlink/p/?LinkID=320151)
* [Exemple d’intégration à une carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
