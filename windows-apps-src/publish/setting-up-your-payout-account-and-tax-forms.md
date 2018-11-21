---
author: jnHs
Description: In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms.
title: Configurer votre compte de paiement et vos déclarations fiscales
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.author: wdg-dev-content
ms.date: 12/14/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 767ea7ca6a5f226fda75d97c3497ae77a5819626
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7444242"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>Configurer votre compte de paiement et vos déclarations fiscales


Pour recevoir l’argent des ventes dans le Microsoft Store, vous devez configurer votre compte de revenu et remplir les déclarations fiscales appropriées dans [L’espace partenaires](https://partner.microsoft.com/dashboard).

Si vous envisagez de référencer uniquement des applications gratuites (et que vous ne voulez pas proposer d’achats in-app ou utiliser Microsoft Advertising), vous n’avez pas besoin de configurer de compte de revenu ni de remplir de déclaration fiscale. Si vous changez d’avis ultérieurement et que vous décidez de vendre des applications (ou modules complémentaires), vous pouvez configurer votre compte de revenu et remplir les déclarations fiscales à ce moment-là. Vous ne pourrez pas soumettre d’applications ou d’extensions payantes avant d’avoir créé votre compte de paiement et votre profil fiscal.

> [!NOTE]
> Dans [certains marchés](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), les développeurs peuvent uniquement soumettre des applications gratuites. Si votre compte est enregistré sur l’un de ces marchés, vous n’aurez pas la possibilité de configurer un compte de paiement.

Une fois que vous avez [configuré votre compte de développeur](opening-a-developer-account.md), il existe deux choses à faire avant de pouvoir vendre des applications (ou modules complémentaires) dans le Microsoft Store:

-   [Configurer votre compte de paiement](#payout-account)
-   [Remplir vos déclarations fiscales](#tax-forms)

> [!NOTE]
> Pour plus d’informations sur le mode et l’échéance des paiements de l’argent que vos applications vous ont rapporté, consultez l’article [Rémunération](getting-paid-apps.md).
 

## <a name="payout-account"></a>Compte de revenu

Un compte de revenu est le compte bancaire vers lequel nous transférons l'argent de vos ventes. Ce compte bancaire doit se trouver dans le même pays que celui dans lequel vous avez enregistré votre compte de développeur.

> [!NOTE]
> Dans certains marchés, PayPal peut être utilisé pour votre compte de paiement. Pour savoir si PayPal est pris en charge pour un marché spécifique, consultez l’article [Types de compte, emplacements et frais](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), et lisez la section [Informations PayPal](#paypal-info) ci-après pour en savoir plus.

 
**Pour configurer votre compte de paiement**

1.  Dans [L’espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône **paramètres de compte** dans le coin supérieur droit, puis sélectionnez les **paramètres de développement**.
2.  Dans le menu de navigation de gauche, sélectionnez le **compte de revenu**.

   > [!NOTE]
   > Étant donné qu’il s’agit d’informations sensibles, vous serez peut-être invité à vous reconnecter.

3.  Sur l’écran qui apparaît, saisissez les informations relatives à votre compte.

   > [!NOTE]
   > Les champs dans lesquels vous entrez des informations sur votre compte acceptent uniquement les caractères alphanumériques.

4.  Enregistrez vos informations.

Si vous devez mettre à jour ou modifier votre compte de paiement, suivez la procédure ci-dessus en remplaçant les informations actuelles par les nouvelles informations.

> [!IMPORTANT]
> En cas de modification de votre compte de paiement, vos paiements peuvent être retardés d’un cycle de paiement au maximum. Ce retard s’explique par le fait que nous devons vérifier la modification apportée au compte, et ce, selon le même processus que celui utilisé lors de la configuration initiale du compte de revenu. Une fois votre compte vérifié, vous serez toujours rémunéré, mais nous ajouterons les paiements du cycle de paiement actuel au prochain cycle de paiement. Pour plus d'informations, voir l'article [Rémunération](getting-paid-apps.md).
 

### <a name="paypal-info"></a>Informations PayPal

Selon votre pays ou votre région, vous avez la possibilité de créer un compte de paiement en saisissant vos informations PayPal. Toutefois, avant de choisir PayPal comme option de compte de paiement, vérifiez les points suivants:

-   Pour savoir si PayPal est un mode de paiement pris en charge dans votre pays ou région, consultez l’article [Types de compte, emplacements et frais](account-types-locations-and-fees.md).
-   Consultez les FAQ ci-après. En fonction de votre situation, il se peut que PayPal ne soit pas l'option idéale pour votre compte de paiement et qu'un compte bancaire soit mieux adapté.

Questions courantes concernant l'utilisation de PayPal comme mode de paiement :

-   **Comment configurer les paramètres PayPal pour recevoir des paiements?** Vous devez vous assurer que votre compte PayPal ne bloque pas les paiements par eCheck. Ce paramètre peut être configuré sur la page des préférences de réception des paiements de votre compte PayPal. Pour plus d'informations, voir la [page de configuration du compte PayPal](http://go.microsoft.com/fwlink/p/?linkid=513139).
-   **Mon pays ou ma région sont-ils pris en charge?** Pour savoir si PayPal est un mode de paiement pris en charge, consultez l’article [Types de compte, emplacements et frais](account-types-locations-and-fees.md).
-   **Mon compte PayPal doit-il être enregistré dans le même pays/région que mon compte du centre de partenaires?** Non. Lorsque vous créez un compte PayPal, vous pouvez accepter la configuration par défaut. Vous ne devriez rencontrer aucun problème d’incompatibilité entre les différents pays ou régions et entre les différentes devises, à moins que vous n’ayez bloqué le paiement dans certaines devises. Ce paramètre peut être configuré sur la page des préférences de réception des paiements de votre compte PayPal.
-   **Dois-je accepter les paiements PayPal manuellement?** Non. Les comptes PayPal demandent par défaut de valider manuellement chaque paiement, ce qui signifie que si vous n’acceptez pas le paiement dans un délai de 30 jours, celui-ci est rejeté. Vous pouvez modifier ce paramètre en désactivant l'option « Me demander » sur la page des paramètres supplémentaires de votre compte PayPal.


### <a name="specific-requirements-for-certain-countriesregions"></a>Exigences spécifiques pour certains pays/régions

Certains pays et régions appliquent des exigences supplémentaires pour les comptes de revenu. Si vous résidez au Pakistan, en Russie ou en Ukraine, notez les obligations suivantes.

#### <a name="pakistan"></a>Pakistan

Le remplissage d’un formulaire R constitue une exigence réglementaire du secteur bancaire au Pakistan. Ce formulaire sert à indiquer la finalité et le motif d’un encaissement de fonds en provenance de l’étranger. Par conséquent, chaque fois que vous pouvez bénéficier d’un revenu mensuel de la part de Microsoft, vous devez soumettre un formulaire R à votre banque pour que ce paiement soit autorisé sur votre compte. Pour connaître la procédure d’obtention d’un formulaire R, contactez votre agence bancaire locale.

Vous devrez soumettre un formulaire R à votre banque pour chacun des mois où vous pouvez bénéficier d’un revenu. Par exemple, si vous prévoyez de recevoir un paiement tous les mois de l'année, vous devrez soumettre un formulaire R à 12 reprises (un par mois).

Une fois que le paiement a été soumis à votre banque, vous disposez de 30 jours pour envoyer un formulaire R. Passé ce délai, les fonds seront retournés à Microsoft.

#### <a name="russia"></a>Russie

Si vous êtes un développeur vivant en Russie, vous pouvez avoir besoin de fournir des documents à votre banque pour que celle-ci puisse déposer des fonds sur votre compte. Si vous pouvez bénéficier d’une rémunération, nous vous fournirons les documents suivants dans un e-mail:

1.  Acceptance Certificate (AC) - contient le montant du paiement transféré sur votre compte.
2.  App Developer Agreement (ADA) - copie signée de l’accord de développeur qui doit être contresignée.

Pour garantir le succès du paiement, notez les points suivants :

-   Le **nom du titulaire compte** saisi pour votre compte de revenu dans l’espace partenaires doit être exactement le même nom associé à votre compte bancaire. Par exemple, si le nom de votre compte bancaire comporte un deuxième prénom, saisissez un deuxième prénom dans le champ **Nom du titulaire du compte**.
-   Les paiements sont transférés directement de Microsoft à votre compte bancaire en roubles (RUB).
-   Les informations bancaires entrées dans l’espace partenaires en caractères latins sont converties en caractères cyrilliques.
-   Les paiements doivent être effectués sur un compte bancaire et non sur une carte bancaire.

#### <a name="ukraine"></a>Ukraine

Si vous êtes un développeur vivant en Ukraine, vous pouvez avoir besoin de fournir des documents à votre banque pour que celle-ci puisse déposer des fonds sur votre compte. Si vous pouvez bénéficier d’une rémunération, nous vous fournirons les documents suivants dans un e-mail:

1.  Acceptance Certificate (AC) - contient le montant du paiement transféré sur votre compte.
2.  App Developer Agreement (ADA) - copie signée de l’accord de développeur qui doit être contresignée.
3.  Amendment Agreement (AA) – ce document est utilisable par votre banque pour faciliter l’identification de vos fonds de paiement.

Microsoft fournit ces trois documents lors de la première tentative de paiement. Pour les paiements ultérieurs, vous ne recevrez plus que le document AC. Conservez les documents ADA et AA au cas où vous en auriez besoin pour recevoir de futurs paiements de votre banque.

Pour garantir le succès du paiement, notez les points suivants :

-   Le **nom du titulaire compte** saisi pour votre compte de revenu dans l’espace partenaires doit être exactement le même nom associé à votre compte bancaire. Par exemple, si le nom de votre compte bancaire comporte un deuxième prénom, saisissez un deuxième prénom dans le champ **Nom du titulaire du compte**.
-   Les paiements sont transférés directement de Microsoft vers votre compte bancaire en dollars USD.
-   Les informations bancaires entrées dans l’espace partenaires en caractères latins sont converties en caractères cyrilliques.


## <a name="tax-forms"></a>Déclarations fiscales

Une fois que vous avez [inscrit pour un compte de développeur](opening-a-developer-account.md) et configurer votre [compte de revenu](#payout-account), vous pouvez créer votre *profil fiscal* pour le Microsoft Store en procédant comme suit:

-   Spécifiez votre pays de résidence et votre nationalité.
-   Remplissez les déclarations fiscales appropriées.

Vous pouvez remplir et envoyer vos déclarations fiscales par voie électronique dans l’espace partenaires; dans la plupart des cas, vous n’avez pas besoin imprimer et envoyer les formulaires.

> [!IMPORTANT]
> L’imposition varie selon les pays et les régions. Le montant exact des taxes dont vous devez vous affranchir varie selon les pays et les régions où vous vendez vos applications. Pour savoir dans quels pays Microsoft vous dispense des taxes d’utilisation et sur les ventes, voir le [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) Dans d’autres pays, selon l’endroit où vous êtes inscrit, vous devrez peut-être verser directement les taxes d’utilisation et les taxes sur les ventes de vos applications à l’administration fiscale locale. Notez également que le produit des ventes d’applications que vous recevez peut être considéré comme un revenu imposable. Nous encourageons vivement à contacter l’autorité compétente de votre pays ou région qui peut mieux vous aider à déterminer les informations fiscales appropriées pour vos activités de développement de Microsoft Store.

 
**Pour créer votre profil fiscal**

1.  Dans [L’espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône **paramètres de compte** dans le coin supérieur droit, puis sélectionnez les **paramètres de développement**.
2.  Dans le menu de navigation de gauche, sélectionnez le **profil fiscal**.

   > [!NOTE]
   > Étant donné qu’il s’agit d’informations sensibles, vous serez peut-être invité à vous reconnecter.

3.  Prenez connaissance de l’état actuel de votre profil fiscal, puis cliquez sur **Modifier** pour remplir les formulaires requis.
4.  Répondez aux questions portant sur la nationalité américaine et la résidence aux États-Unis, puis cliquez sur **Suivant**.
5.  Si votre nom et votre adresse sont affichés, vérifiez-les pour la déclaration de vos revenus.

Les versions électroniques des déclarations fiscales à remplir apparaissent ensuite. Quel que soit votre pays de résidence ou votre nationalité, vous devez remplir les déclarations fiscales des États-Unis pour vendre des applications ou des modules complémentaires via le Microsoft Store. Les développeurs répondant à certains critères de résidence aux États-Unis doivent remplir le formulaire W-9 du fisc américain (IRS). Les autres développeurs résidant en dehors des États-Unis doivent compléter le formulaire W-8 de l'IRS. Vous pouvez remplir ces formulaires en ligne lors de la création de votre profil fiscal.

Il n'est pas nécessaire de fournir un numéro d'identification du contribuable ou ITIN (États-Unis) pour recevoir des paiements de Microsoft ou pour revendiquer des avantages en vertu d'une convention fiscale.

### <a name="withholding-rates"></a>Taux de retenue

Les informations que vous indiquez dans vos déclarations fiscales déterminent le taux de retenue fiscale appropriée. Le taux de retenue s’applique uniquement aux ventes que vous réalisez aux États-Unis ; les ventes réalisées dans les autres pays ne sont pas assujetties à une retenue. Les taux de retenue peuvent varier, mais pour la plupart des développeurs s’inscrivant dans un autre pays que les États-Unis, le taux par défaut est de 30 %. Ce taux peut être revu à la baisse si votre pays a signé une convention fiscale avec les États-Unis.

### <a name="tax-treaty-benefits"></a>Avantages en vertu d’une convention fiscale

Si vous résidez en dehors des États-Unis, vous pouvez tirer parti d’avantages découlant d’une convention fiscale. Ces avantages varient d’un pays à l’autre et peuvent vous permettre de réduire le montant des taxes retenues par le Microsoft Store. Pour revendiquer des avantages en vertu d’une convention fiscale, remplissez la deuxième partie du formulaire W-8BEN. Nous vous recommandons de contacter les autorités appropriées dans votre pays ou région pour déterminer si ces avantages vous concernent.

 

 




