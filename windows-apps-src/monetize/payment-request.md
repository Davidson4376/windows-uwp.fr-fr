---
description: L’API de demande de paiement fournit une solution intégrée pour les applications UWP ignorer le processus de la demande d’un utilisateur entrer les informations de paiement et sélectionnez les modes de livraison.
title: Simplifier les paiements avec l’API de demande de paiement
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, uwp, demande de paiement
ms.openlocfilehash: e5fb5cead7833b8cc213c6633cae6cee0da3466b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607864"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Simplifier les paiements avec l’API de demande de paiement
L’API de demande de paiement pour les applications UWP est basé sur le [spécifications de l’API de demande de paiement W3C](https://w3c.github.io/browser-payment-api/). Elle vous permet de rationaliser le processus de validation dans vos applications UWP. Les utilisateurs peuvent accélérer Finalisation de l’achat par à l’aide des options de paiement et de distribuer des adresses déjà enregistrés avec leur compte Microsoft. Vous pouvez augmenter votre taux de conversion et réduire le risque de violations de données, car les informations de paiement sont sous forme de jetons. À compter de Windows 10 Creators Update, les utilisateurs peuvent utiliser leurs options de paiement enregistré pour payer facilement différentes expériences dans les applications UWP.

## <a name="prerequisites"></a>Conditions préalables
Avant de commencer à l’aide de l’API de demande de paiement, il existe quelques éléments, vous devez faire ou être conscient.

### <a name="getting-a-merchant-id"></a>Obtention d’un ID de marchand
Dans le cadre du processus de demande de paiement, Microsoft demande des jetons de paiement à votre place à partir de votre fournisseur de services. Par conséquent, avant de commencer à l’aide de l’API, nous avons besoin de votre autorisation pour demander ces jetons.  Vous devez suivre quelques étapes pour vous inscrire pour un compte de vendeur et accordez les autorisations nécessaires. Pour ce faire, accédez à [Microsoft Seller Center](https://seller.microsoft.com/en-us/dashboard/registration/seller/?accountprogram=uwp). Une fois que vous avez fait, copiez le marchand résultant ID à partir du centre de partenaires dans votre application lors de la construction de la demande de paiement. Ensuite, lorsque votre application envoie une demande de paiement, vous recevrez un jeton de paiement de votre processeur que vous devez envoyer votre paiement.

### <a name="geographic-restrictions-and-language-support"></a>Restrictions géographiques et prise en charge linguistique
L’API de demande de paiement peut être utilisée uniquement par les entreprises des États-Unis pour traiter les transactions aux États-Unis.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>À l’aide de l’API de demande de paiement dans votre application : étape par étape
Cette section montre comment utiliser le [API de demande de paiement UWP](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments) dans votre application. Nous utilisons l’API ici dans sa forme la plus simple, par souci de clarté. Pour obtenir un exemple d’utilisation plus avancée de ces API, consultez le [exemple d’application UWP Shopping sur GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Créer un ensemble de toutes les options de paiement que vous acceptez.
> [!Note]
> Remplacez le **merchant id de vendeur portail** texte avec le marchand ID que vous avez reçu à partir du centre du vendeur.

[!code-cs[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Rassembler les informations de paiement. 

Ces détails seront affichera à l’utilisateur dans l’application de paiement. 

[!code-cs[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. Inclure les taxes sur les ventes. 

> [!Important]
> L’API de ne pas ajouter les éléments ou calculer la taxe pour vous. N’oubliez pas que les taux d’imposition varie selon la juridiction. Pour plus de clarté, nous utilisons un taux d’imposition de 9,5 % hypothétique.

[!code-cs[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (Facultatif)  Ajoutez les remises ou autres modificateurs au total. 

Voici un exemple d’ajout d’une remise pour l’utilisation d’une carte de crédit Contoso spécifique pour les éléments d’affichage. (*Contoso* est un nom fictif.)

[!code-cs[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. Assembler tous les détails de paiement.

[!code-cs[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-cs[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. Envoyer la demande de paiement. 

Appelez le **SubmitPaymentRequestAsync** méthode pour envoyer votre demande de paiement. Ceci fait apparaître l’application de paiement montrant les options de paiement disponibles.

[!code-cs[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

L’utilisateur est invité à vous connecter avec leur compte Microsoft.

Une fois que l’utilisateur se connecte, ils peuvent sélectionner les options de paiement et l’adresse d’expédition qui ont été précédemment enregistrés.

![Demande de paiement UI](./images/33.png "paiement demande l’interface utilisateur")

Votre application attend que l’utilisateur d’appuyer sur **payer**, puis termine l’ordre.

[!code-cs[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Une fois que le paiement est terminé, l’utilisateur est présenté avec un **commande confirmée** écran.

![Commande confirmée](./images/44.png "commande confirmée ")

## <a name="see-also"></a>Voir également
- [Documentation de référence Windows.ApplicationModel.Payments](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [Exemple d’application d’achat UWP sur GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [Spécification de l’API de demande de paiement de W3C](https://www.w3.org/TR/payment-request/)
- [Demande de paiement API ](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/device/payment-request-api)

