---
author: mcleanbyron
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: "Avant de soumettre votre application, nous vous recommandons de tester l’implémentation de votre médiation publicitaire."
title: "Tester l’implémentation de votre médiation publicitaire"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 0805ed5462a4b100b837ed9c11ec2d9e7caabc34

---

# Tester l’implémentation de votre médiation publicitaire


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Avant de soumettre votre application, nous vous recommandons de tester l’implémentation de votre médiation publicitaire.

## Test avec les valeurs de test de configuration des réseaux publicitaires


Si vous exécutez votre application sans entrer de configuration des réseaux publicitaires en lançant **Services connectés** pour votre projet dans Visual Studio, la médiation publicitaire utilise automatiquement les valeurs de configuration de test lorsque vous exécutez votre application sur votre ordinateur de développement (pour les applications de la plateforme Windows universelle [UWP]) et Windows 8.1 XAML) ou sur l’émulateur ou l’appareil (pour les applications Windows Phone). Cela vous permet de tester rapidement votre application et de vous assurer qu’elle est codée correctement avant d’entrer les paramètres requis pour vos réseaux publicitaires.

Les réseaux publicitaires tournent de manière séquentielle, un réseau étant affiché après un autre pendant une durée égale. Veillez à attendre suffisamment longtemps pour que l’exécution se déroule sur quelques cycles afin de pouvoir visualiser tous les réseaux publicitaires et de réduire le risque de problèmes de connectivité temporaires.

Les publicités test seront affichées pour les réseaux publicitaires qui les prennent en charge. Notez que les publicités test peuvent parfois ressembler à des erreurs. Veillez à vérifier vos événements pour déterminer si des erreurs se sont produites.

> **Remarque** Lorsque vous testez une application Silverlight pour Windows Phone, Google AdMob renvoie toujours une erreur **Demande non valide**, car il n’utilise pas de métadonnées de test. Pour vérifier votre implémentation Google AdMob, vous devez entrer les paramètres requis conformément à la description de la section suivante.

 

Si vous avez utilisé le code de gestion des événements indiqué dans [Ajouter et utiliser le contrôle Ad Mediator](add-and-use-the-ad-mediator-control.md), toutes les erreurs s’affichent sur la sortie de la console.

## Test avec vos valeurs de configuration des réseaux publicitaires


Après avoir testé votre application avec les données de configuration de test, vous devez la tester avec les valeurs de configuration des réseaux publicitaires que vous envisagez d’utiliser pour la version publiée dans le Windows Store.

Tout d’abord, ouvrez la fenêtre **Ajouter un service connecté** (Visual Studio 2015) ou la fenêtre **Gestionnaire de services** (Visual Studio 2013), et configurez chaque réseau publicitaire conformément à la procédure décrite dans [Ajouter et utiliser le contrôle Ad Mediator](add-and-use-the-ad-mediator-control.md). Entrez les paramètres requis pour chaque réseau publicitaire.

Vous êtes maintenant prêt à tester votre application. Veillez à exécuter l’application suffisamment longtemps pour vérifier que chaque réseau publicitaire peut afficher correctement une publicité. Vérifiez les exceptions et corrigez les erreurs de codage avant de soumettre votre application.

Lorsque vous soumettez votre package d’application au tableau de bord du Centre de développement Windows, les valeurs de configuration que vous entrez dans Visual Studio sont automatiquement renseignées dans la page du tableau de bord **Monétiser avec des publicités**. Vous pouvez modifier ces valeurs pour configurer le comportement de votre réseau publicitaire pour votre application dans le Windows Store. Pour en savoir plus, voir [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md).

## Rubriques connexes

* [Sélectionner et gérer vos réseaux publicitaires](select-and-manage-your-ad-networks.md)
* [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md)
* [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md)
* [Résoudre les problèmes liés à la médiation publicitaire](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


