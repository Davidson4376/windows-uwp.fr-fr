---
title: Autoriser les comptes Xbox Live dans votre environnement de test
description: Découvrez comment autoriser un compte Xbox Live publique à utiliser pour tester dans votre environnement de développement.
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, comptes, comptes de test
ms.localizationpriority: medium
ms.openlocfilehash: 662f85f985baf58eef050060f8132f5a4b92444d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663854"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>Autoriser les comptes de Xbox Live pour les tests dans votre environnement

Cette rubrique passera par le processus de configuration d’un compte Xbox Live avec votre environnement de test des serveurs de publication

## <a name="prerequisites"></a>Conditions préalables

Vous avez besoin des éléments pour autoriser un compte Xbox Live de test :

* Un [compte Xbox Live](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>Accédez à la page compte de Test Xbox

Celui-ci se trouve dans la section Paramètres de compte de partenaires

Cette section de l’une des deux méthodes permettant d’afficher

1. Dans [partenaires](https://partner.microsoft.com/dashboard/windows/overview), cliquez sur le ⚙️ ENGRENAGE paramètres dans le coin supérieur droit du tableau de bord et sélectionnez **les paramètres du développeur** dans la liste déroulante. Ceci ouvrira un menu de navigation sur le côté gauche de l’écran où vous allez sélectionner la liste déroulante de Xbox Live, puis le **les comptes de test Xbox** lien.
2. À partir de votre page de configuration de Xbox Live Creators, recherchez la section de test et cliquez sur le lien intitulé **autoriser Xbox Live comptes pour votre environnement de test**

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>Autoriser un compte Xbox Live pour votre environnement de test

* Une fois dans la page comptes de test Xbox, vous devez voir une liste de tous les comptes actuellement autorisés. Pour autoriser un nouveau compte, cliquez sur le bouton Ajouter un compte

![Ajout de comptes Xbox Live](../images/creators_udc/add_test_account.png)

* Modal devrait s’afficher dans l’écran avec une zone de texte où vous pouvez entrer l’adresse de messagerie du compte de votre choix

![Ajout de Xbox Live de comptes modale](../images/creators_udc/add_test_account_modal.png)

* Cliquez sur le bouton Ajouter pour valider que l’adresse de messagerie existe et qu’il a un compte Xbox Live associé. Si les vérifications réussissent la fenêtre modale disparaît et vous verrez le nouveau compte dans la table indiquant s’il elle est désormais correctement autorisé pour votre environnement de test

![Réussite de comptes Live Ajout Xbox](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>Résolution des problèmes

L’e-mail entré dans la fenêtre modale est traité par quelques vérifications, y compris une recherche afin de garantir est associé à un compte Xbox Live. Si une de ces vérifications échoue, le compte est pas ajouté à la table et par conséquent pas autorisé, et vous pouvez obtenir une erreur « Nous sommes désolés, un problème d’ajout de votre adresse de messagerie ».

Une vérification de bonne si vous rencontrez des problèmes est d’essayer de vous connecter au compte [Xbox.com](https://www.xbox.com/live/). Si vous ne pouvez pas vous connecter le compte n’est pas un compte Xbox Live.
