---
title: "Répondre aux retours des clients"
description: "Vous pouvez répondre directement à un commentaire laissé par vos clients dans le Hub de commentaires."
author: JnHs
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 04983b80-2a18-4ace-93d3-e8c33c04bfb9
ms.openlocfilehash: 8ce325c212b9f5aba11ab35f3cfcfeea9a44bcbe
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="respond-to-customer-feedback"></a>Répondre aux retours des clients

Vous pouvez utiliser le [rapport de commentaires](feedback-report.md) pour passer en revue les commentaires formulés par les clients Windows10 au sujet de votre application qu’ils ont publiés dans le Hub de commentaires, puis répondre directement à ces commentaires. Vous avez la possibilité de publier vos réponses dans le Hub de commentaires afin qu’elles soient visibles à tous (soit sous forme de commentaires individuels, soit en mettant à jour l’état d’une partie du commentaire et en ajoutant une description). Dans vos réponses, vous pouvez informer les clients sur de nouvelles fonctionnalités ou des correctifs de bogues, ou leur demander des commentaires plus précis sur la façon d’améliorer votre application. Vous pouvez également envoyer votre réponse par courrier électronique directement au client qui a laissé le commentaire.

> **Conseil** Vous pouvez encourager les clients à laisser des commentaires à l’aide de l’API de commentaires du [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) pour ajouter un contrôle qui permettra aux clients [d’ouvrir directement le Hub de commentaires à partir de votre application UWP](../monetize/launch-feedback-hub-from-your-app.md). N’oubliez pas que tout client ayant téléchargé votre application sur un appareil Windows10 prenant en charge le Hub de commentaires a la possibilité de laisser des commentaires à son sujet directement à partir de cette application. C’est la raison pour laquelle vous pouvez voir des commentaires de clients dans ce rapport, même si vous n’avez pas spécifiquement demandé de commentaires depuis votre application.

Pour répondre à n’importe quel commentaire, cliquez sur le lien **Répondre aux commentaires** qui s’affiche en regard du commentaire dans votre **rapport de commentaires**.

Le Centre de développement Windows prend en charge trois options pour répondre aux clients qui formulent des commentaires sur votre application. Quelle que soit l’option que vous choisissez, n’oubliez pas que chaque réponse est soumise à une restriction de 1000caractères.

## <a name="public-comments-in-feedback-hub"></a>Publication de commentaires publics dans le Hub de commentaires

Par défaut, le bouton **Commentaire** est sélectionné lorsque vous cliquez sur **Répondre aux commentaires**. Pour pouvoir publier une réponse publique aux commentaires du client, laissez ce bouton sélectionné. Entrez votre commentaire dans la zone, puis cliquez sur **Envoyer**.

Le commentaire que vous avez saisi s’affichera sous la forme d’un commentaire dans le Hub de commentaires, en même temps que ceux envoyés par d’autres clients. Votre nom d’éditeur et le nom de votre application s’afficheront avec votre commentaire pour vous identifier en tant que développeur. Vous pouvez rédiger autant de commentaires que vous le souhaitez pour chaque commentaire publié, mais notez que vous ne pouvez ni modifier ni supprimer vos commentaires après les avoir envoyés. Les cinq commentaires les plus récents ajoutés à un commentaire apparaîtront dans votre **rapport de commentaires** (ainsi que dans le Hub de commentaires). Au-delà de cinq commentaires, cliquez sur **Afficher tous les commentaires** pour les lire en intégralité dans le Hub de commentaires.

## <a name="private-responses-via-email"></a>Envoi de réponses privées par courrier électronique

Si vous préférez ne pas publier de réponse publique, vous pouvez cocher la case **Envoyer le commentaire sous forme de message** pour envoyer une réponse privée directement au client (à condition qu’il ait communiqué une adresse électronique et qu’il n’ait pas choisi de ne plus recevoir de réponses par courrier électronique). Lorsque vous procédez ainsi, Microsoft envoie au client un message électronique en votre nom. Le message contient ses commentaires d’origine ainsi que la réponse que vous avez rédigée.

Après avoir coché la case **Envoyer le commentaire sous forme de message**, entrez votre commentaire, puis cliquez sur **Envoyer**. Notez que vous devez renseigner une adresse électronique dans le champ **Adresse e-mail du contact du support technique** lorsque vous utilisez cette option. Par défaut, nous utilisons l’adresse électronique que vous avez spécifiée dans vos coordonnées de compte. Si vous préférez utiliser une autre adresse électronique, vous pouvez mettre à jour le champ **Adresse e-mail du contact du support technique** pour en utiliser une autre. Le client qui reçoit votre réponse sera en mesure d’envoyer sa réponse directement à cette adresse électronique.

## <a name="public-status-updates-and-descriptions-in-feedback-hub"></a>Mises à jour de l’état public et descriptions dans le Hub de commentaires

Pour les réponses publiques, vous disposez d’une troisième option qui consiste à définir l’état sur une partie des commentaires pour indiquer à vos clients que vous cherchez à corriger le problème, ou que vous l’avez déjà résolu. Lorsque vous mettez à jour l’état d’une partie des commentaires, celle-ci s’affiche avec les commentaires dans le Hub de commentaires.

Pour utiliser cette option, sélectionnez la case **Mettre à jour l’état**. Sélectionnez ensuite l’une des options suivantes:

- **Investigation**: vous connaissez le problème et êtes en train de l’examiner.
- **En cours**: vous êtes en train de résoudre un problème ou d’ajouter une fonctionnalité qui vous a été demandée.
- **Terminé**: vous avez publié une mise à jour pour corriger le problème ou ajouter la fonctionnalité demandée.

Tout en mettant à jour l’état, vous pouvez entrer un commentaire pour fournir davantage d’informations, par exemple une estimation du délai de résolution du problème ou des informations complémentaires sur les dernières modifications apportées. Cette description s’affiche en haut de la liste des commentaires (et le rapport de commentaires indique l’état actuel et la description).

L’option **Mettre à jour l’état** vous permet de changer d’état chaque fois que vous le souhaitez (et de mettre à jour en parallèle les descriptions pour chaque changement d’état). À chaque fois que vous modifiez l’état d’une partie des commentaires, cette mise à jour se reflète dans le Hub de commentaires de sorte que les clients puissent lire votre dernière réponse.

## <a name="guidelines-for-responses"></a>Recommandations en matière de réponses
Quelle que soit la méthode utilisée pour répondre à des commentaires d’un client, vous devez suivre ces recommandations pour toutes vos réponses.
- Les réponses ne doivent pas comporter plus de 1000caractères.
- Vous ne pouvez proposer aux utilisateurs aucun type de rémunération, y compris sous forme d’éléments d’applications numériques, en contrepartie de leurs commentaires publics.
- N’incluez ni contenu marketing ni publicités dans votre réponse. N’oubliez pas que la personne qui a déposé un commentaire est déjà votre client.
- Ne faites pas la promotion d’autres applications ou services dans votre réponse.
- Votre réponse doit être directement liée à l’application et au commentaire concernés.
- N’incluez pas de commentaires profanes, agressifs, personnels ou malveillants dans votre réponse. Soyez toujours poli et gardez à l’esprit que les clients satisfaits seront probablement les plus grands promoteurs de votre application.

> **Remarque** Les clients peuvent signaler un développeur à Microsoft en cas de réponse inappropriée à un commentaire. Ils peuvent également choisir de ne plus recevoir de réponses à leurs commentaires par e-mail.

Votre relation avec vos clients est de votre responsabilité. Microsoft ne prend pas parti en cas de litiges entre les développeurs et les clients. Toutefois, si vous pensez que le contenu d’un commentaire client concernant votre produit est inapproprié, veuillez soumettre un [ticket de support](http://go.microsoft.com/fwlink/p/?LinkID=401178).
