---
Description: Vous pouvez générer des codes promotionnels pour une application ou un module complémentaire que vous avez publiés dans le Microsoft Store.
title: Générer des codes promotionnels
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, code promotionnel, codes promotionnels, jeton, jetons
ms.localizationpriority: medium
ms.openlocfilehash: 931b3abfe13a3834d991ee1a0a38c752b9e3f719
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64745031"
---
# <a name="generate-promotional-codes"></a>Générer des codes promotionnels


[Partenaires](https://partner.microsoft.com/dashboard) vous permet de générer des codes promotionnels pour une application ou d’un module complémentaire que vous avez publié dans le Microsoft Store. Les codes promotionnels permettent d’offrir facilement à des utilisateurs influents un accès gratuit à votre application ou votre module complémentaire. Vous pouvez également utiliser un code promotionnel pour les scénarios service client en permettant aux utilisateurs un accès gratuit à votre application ou d’un module complémentaire ou pour [bêta testing](beta-testing-and-targeted-distribution.md) avec Windows 10. 

Chaque code de promotion possède une URL d’échangeables unique correspondante qui a un client peut cliquer pour échanger le code et installer votre application ou un module complémentaire à partir du Microsoft Store.  Notez que votre application doit exécuter la phase finale de publication du [processus de certification des applications](the-app-certification-process.md) avant que les clients ne puissent utiliser un code promotionnel pour l’installer.

Vous pouvez générer des codes d’usage unique (et distribuer un pour chaque client), ou vous pouvez choisir de générer un code qui peut être utilisé plusieurs fois par un nombre spécifié de clients.

> [!TIP]
> Vous pouvez utiliser des [notifications Push ciblées](send-push-notifications-to-your-apps-customers.md) pour distribuer un code promotionnel à un segment de vos clients. Dans ce cas, veillez à utiliser un code promotionnel qui permet à plusieurs clients d'utiliser le même code.


## <a name="promotional-code-policies"></a>Stratégies de code promotionnel

Tenez compte des stratégies suivantes relatives aux codes promotionnels :

-   Vous pouvez générer des codes promotionnels pour toute application ou tout module complémentaire (à l’exception des modules complémentaires d’abonnement) que vous avez publiés dans le Microsoft Store. Les clients peuvent utiliser les codes sur n’importe quelle version de Windows prise en charge par votre application ou module complémentaire.
-   Pour les jeux :
    - Vous pouvez générer des codes promotionnels jusqu'à 5 000 par le jeu.
    - Généré pour les jeux de codes promotionnels n’expirent jamais.
- Pour tous les autres types d’applications ou des modules complémentaires :
    - Pour une période de six mois, vous pouvez générer des codes promotionnels usage unique jusqu'à 1 600, ou un nombre quelconque de l’utilisation de plusieurs codes telles que le total autorisé échanges ne dépasse pas 1600.
    - La période de 6 mois commence lorsque vous générez le premier code de promotion est créé et est valable pendant 6 mois, quel que soit ou non, vous définissez une date d’expiration antérieure sur les codes.
    - Les codes créés pendant une période de six mois existante sera sont considérés comme le nombre de codes générés au sein de cette période, même si elles expirent après la fin de la période (par exemple, si vous générez un code sur le dernier jour de la fenêtre de six mois, il sera va être toujours  être valide pour un total de 6 mois à partir de sa création.)
-   Vous devez suivre les exigences définies dans le [contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), y compris la section **3 k. Code promotionnel**.

> [!NOTE]
> Vous pouvez utiliser des codes promotionnels même si votre application n’est pas disponible pour les clients (autrement dit, si vous avez sélectionné **rendent ce produit disponibles mais ne sont pas détectables dans le Store** avec la **arrêter acquisition : Tous les clients avec un lien direct peuvent voir la liste Store du produit, mais ils peuvent uniquement télécharger si le propriétaire du produit avant, ou un code promotionnel et utilisez un appareil Windows 10** option dans votre soumission [détectabilité ](choose-visibility-options.md#discoverability) section). Avec cette option, les clients doivent être sur Windows 10 (y compris Xbox) afin d’acquérir votre produit avec un code promotionnel.


## <a name="order-promotional-codes"></a>Commander des codes promotionnels

Pour les codes promotionnels de commande pour une application ou d’un module complémentaire :

1.  Dans le menu de navigation gauche de [partenaires](https://partner.microsoft.com/dashboard), développez **attraction** , puis sélectionnez **codes de promotion**.

2.   Dans la page **Codes promotionnels**, cliquez sur **Commander des codes**.

3.  Dans la page **Nouvelle commande de codes promotionnels**, entrez les informations suivantes :
    -   Sélectionnez l’application ou le module complémentaire pour lequel vous voulez générer des codes. (Notez que vous ne pouvez pas générer des codes promotionnels pour les modules complémentaires d’abonnement).
    -   Spécifiez un nom pour la commande. Ce nom permet de différencier les différentes commandes de codes lors de l’examen des données d’utilisation de votre code promotionnel.
    -   Sélectionnez le type de commande. Vous pouvez choisir de générer plusieurs codes promotionnels pouvant servir une seule fois ou un seul code promotionnel pouvant servir plusieurs fois.
    -   Spécifiez le nombre de codes à commander (si vous générez un ensemble de codes) ou le nombre de fois où que le code peut être utilisé (si vous générez un code à utiliser plusieurs fois).
    -   Indiquez à quel moment les codes promotionnels doivent devenir actifs. Pour choisir une date et une heure de début précises, décochez la case **Les codes sont immédiatement actifs**. Sinon, les codes devient actifs immédiatement (bien que votre produit doit avoir terminé le processus de publication dans l’ordre pour un client à utiliser le code).
    -   Indiquez à quel moment les codes promotionnels doivent expirer. Pour choisir une date et une heure d’expiration précises antérieures à 6 mois, décochez la case **Les codes expirent au bout de 6 mois**.

4.  Cliquez sur **Commander des codes**. Vous êtes alors renvoyé à la page **codes promotionnels**, dans laquelle vous pouvez voir votre nouvelle commande dans le tableau récapitulatif des commandes de codes promotionnels pour cette application.


## <a name="download-and-distribute-promotional-codes"></a>Télécharger et distribuer des codes promotionnels

Pour télécharger une commande de codes promotionnels complétée et distribuer les codes aux clients :

1.  Dans le menu de navigation gauche de [partenaires](https://partner.microsoft.com/dashboard), développez **attraction** , puis sélectionnez **codes de promotion.**
2.  Cliquez sur le lien **Télécharger** correspondant à la commande de codes promotionnels, puis enregistrez le fichier généré sur votre ordinateur. Ce fichier contient des informations sur votre commande de codes promotionnels sous la forme de valeurs séparées par des tabulations (.tsv).
3.  Ouvrez le fichier .tsv dans l’éditeur de votre choix. Pour bénéficier d’une expérience optimale, ouvrez le fichier .tsv dans une application pouvant afficher les données dans une structure tabulaire, telle que Microsoft Excel. Toutefois, vous pouvez ouvrir ce fichier dans tout éditeur de texte.

    Le fichier contient les colonnes de données suivantes pour chaque code :

    -   **Nom du produit** : Le nom de l’application ou un composant additionnel que le code est associé.
    -   **Nom de la commande**: Le nom de l’ordre dans lequel ce code a été généré.
    -   **Code de promotion**: Le code lui-même. Il s’agit d’une chaîne de 5x5 caractères alphanumériques séparés par des traits d’union. Exemple : DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **URL échangeables**: L’URL qu’un client peut utiliser pour échanger le code et installer votre application ou le module complémentaire. L’URL a le format suivant : https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; promotional_code >
    -   **Date de début**: La date de que ce code sont devenu actif.
    -   **Expiration date**: Date de qu'expiration de ce code.
    -   **Code ID**: Un ID unique pour ce code.
    -   **ID de commande**: Un ID unique pour l’ordre dans lequel ce code a été généré.
    -   **Donné à**: Un champ vide que vous pouvez utiliser pour suivre le client auquel vous avez donné le code à.
    -   **Disponible** : Le nombre de fois où que le code est toujours disponible pour échanger (au moment que le fichier a été généré).
    -   **Échangé**: Le nombre de fois où le code a été utilisé (en temps que le fichier a été généré).

4.  Distribuez les URL donnant droit à vos clients via le mode de communication de votre choix (par exemple, notifications ciblées, message électronique, SMS ou cartes imprimées). Nous vous recommandons d’inclure dans votre communication les éléments suivants :
    -   Explication relative à l’application ou au module complémentaire auquel le code promotionnel a trait et, éventuellement, description de la raison pour laquelle le client reçoit le code.
    -   URL donnant droit correspondant au code.
    -   Instructions invitant le client à accéder à l’URL donnant droit, à se connecter à l’aide de son compte Microsoft, et à suivre les consignes de téléchargement et d’installation de votre application.


## <a name="code-redemption-user-experience"></a>Expérience utilisateur d’échange du code

Une fois que vous distribuez un code promotionnel (ou son URL échangeables) à un client, ils peuvent cliquer l’URL permettant d’obtenir le produit gratuitement. Lorsqu’il clique sur l'URL donnant droit, une page authentifiée **Utiliser votre code** s’ouvre à l’adresse <https://account.microsoft.com/billing/redeem>. Cette page inclut une description de l’application à laquelle l’utilisateur est sur le point d’accéder. Si le client n’est pas connecté à son compte Microsoft, il peut être invité à le faire. Votre client peut également consulter <https://account.microsoft.com/billing/redeem> et entrer le code directement.

> [!IMPORTANT]
> Nous vous recommandons de ne pas distribuer les codes promotionnels à vos clients tant que votre produit n'a pas terminé le processus de publication (même si vous avez sélectionné **Rendre ce produit disponible mais non détectable dans le Windows Store**). Les clients verront une erreur s’ils tentent d’utiliser un code promotionnel pour un produit qui n’a pas encore été publié.

Lorsque le client clique sur **Utiliser**, le Microsoft Store s’ouvre à la page Vue d’ensemble de l’application (sur un appareil Windows 10 ou Windows 8.1), dans laquelle il peut cliquer sur **Installer** pour télécharger et installer l’application gratuitement. Si le client se connecte à partir d’un ordinateur ou d’un appareil sur lequel Microsoft Store n’est pas installé, le lien ouvre la page web du Microsoft Store pour l’application. Le code sera appliqué au compte Microsoft du client, afin que celui-ci puisse télécharger ultérieurement l’application sur un appareil Windows (associé au même compte Microsoft) gratuitement.

> [!NOTE]
> Dans certains cas, un client peut s’afficher un **acheter** bouton au lieu de **installer**, même si l’application a été échangée via le code promotionnel. Le client peut alors cliquer sur **Acheter** pour installer l’application gratuitement.


## <a name="review-your-promotional-codes"></a>Passer en revue vos codes promotionnels

Pour consulter un résumé détaillé des commandes de code de promotion de vos applications et les modules complémentaires, accédez à la **codes promotionnels** page (dans le menu de navigation gauche de partenaires, développez **attraction** , puis sélectionnez **Codes de promotion**). Vous pouvez consulter les détails suivants pour tous vos codes promotionnels actifs et inactifs :
-   Nom de la commande
-   Application ou module complémentaire
-   Date de début
-   Date d’expiration
-   Disponible
-   Utilisé

Vous pouvez également [télécharger](#download-and-distribute-promotional-codes) une commande à partir de ce tableau.

 
