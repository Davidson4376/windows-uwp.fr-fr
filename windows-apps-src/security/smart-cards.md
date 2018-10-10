---
title: Cartes à puce
description: Cette rubrique explique comment les applications de plateforme Windows universelle (UWP) peuvent utiliser des cartes à puce pour connecter des utilisateurs à des services réseau sécurisés, notamment comment accéder aux lecteurs de carte à puce physiques, créer des cartes à puce virtuelles, communiquer avec des cartes à puce, authentifier des utilisateurs, réinitialiser des PIN d’utilisateur et supprimer ou déconnecter des cartes à puce.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: dda2b580a82c72ad2e31c771a9c76f8d770049ec
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4498169"
---
# <a name="smart-cards"></a>Cartes à puce




Cette rubrique explique comment les applications de plateforme Windows universelle (UWP) peuvent utiliser des cartes à puce pour connecter des utilisateurs à des services réseau sécurisés, notamment comment accéder aux lecteurs de carte à puce physiques, créer des cartes à puce virtuelles, communiquer avec des cartes à puce, authentifier des utilisateurs, réinitialiser des PIN d’utilisateur et supprimer ou déconnecter des cartes à puce. 

## <a name="configure-the-app-manifest"></a>Configurer le manifeste de l’application


Pour que votre application puisse authentifier des utilisateurs à l’aide de cartes à puce ou de cartes à puce virtuelles, vous devez configurer la fonction **Certificats utilisateur partagés** dans le fichier Package.appxmanifest du projet.

## <a name="access-connected-card-readers-and-smart-cards"></a>Accéder à des lecteurs de cartes et à des cartes à puce connectés


Vous pouvez rechercher des lecteurs et des cartes à puce attachées en passant l’ID de l’appareil (spécifié dans [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)) à la méthode [**SmartCardReader.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn263890). Pour accéder aux cartes à puce actuellement attachées à l’appareil de lecture retourné, appelez [**SmartCardReader.FindAllCardsAsync**](https://msdn.microsoft.com/library/windows/apps/dn263887).

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

Vous devez également configurer votre application de manière à surveiller les événements [**CardAdded**](https://msdn.microsoft.com/library/windows/apps/dn263866) en implémentant une méthode pour déterminer le comportement de l’application lors de l’insertion d’une carte.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

Vous pouvez ensuite passer chaque objet [**SmartCard**](https://msdn.microsoft.com/library/windows/apps/dn297565) retourné à [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) pour accéder aux méthodes permettant à votre application d’accéder à sa configuration et de la personnaliser.

## <a name="create-a-virtual-smart-card"></a>Créer une carte à puce virtuelle


Pour créer une carte à puce virtuelle à l’aide de [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801), votre application doit d’abord fournir un nom convivial, une clé d’administration et un [**SmartCardPinPolicy**](https://msdn.microsoft.com/library/windows/apps/dn297642). Le nom convivial est généralement fourni à l’application, mais votre application doit fournir une clé d’administration et générer une instance du **SmartCardPinPolicy** actuel avant de passer les trois valeurs à [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830).

1.  Créer une instance d’un [**SmartCardPinPolicy**](https://msdn.microsoft.com/library/windows/apps/dn297642)
2.  Pour générer la valeur de la clé d’administration, appelez [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392) sur la valeur de la clé d’administration fournie par le service ou l’outil de gestion.
3.  Passez ces valeurs avec la chaîne *FriendlyNameText* à [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830).

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

Une fois que la méthode [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830) retourne l’objet [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) associé, la carte à puce virtuelle est mise en service et prête à l’emploi.

## <a name="handle-authentication-challenges"></a>Gérer les demandes d’authentification


Pour authentifier des utilisateurs au moyen de cartes à puce ou de cartes à puce virtuelles, votre application doit être en mesure de répondre aux demandes entre les données des clés d’administration stockées sur la carte et les données des clés d’administration gérées par le serveur d’authentification ou l’outil de gestion.

L’exemple de code suivant montre comment prendre en charge l’authentification par carte à puce pour des services ou la modification des détails d’une carte à puce physique ou virtuelle. Si les données générées à l’aide de la clé d’administration sur la carte (« challenge ») sont identiques aux données de la clé d’administration fournies par le serveur ou l’outil de gestion (« adminkey »), l’authentification réussit.

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

Ce code est référencé dans le reste de cette rubrique pour illustrer comment remplir une action d’authentification et comment appliquer des modifications aux informations d’une carte à puce et d’une carte à puce virtuelle.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>Vérifier la réponse d’authentification par carte à puce ou carte à puce virtuelle


Maintenant que nous avons défini la logique pour les demandes d’authentification, nous pouvons soit communiquer avec le lecteur pour accéder à la carte à puce, soit accéder à une carte à puce virtuelle à des fins d’authentification.

1.  Pour commencer la demande, appelez [**GetChallengeContextAsync**](https://msdn.microsoft.com/library/windows/apps/dn263811) à partir de l’objet [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) associé à la carte à puce. Une instance de [**SmartCardChallengeContext**](https://msdn.microsoft.com/library/windows/apps/dn297570) qui contient la valeur [**Challenge**](https://msdn.microsoft.com/library/windows/apps/dn297578) de la carte est alors générée.

2.  Passez ensuite la valeur de demande de la carte et la clé d’administration fournie par le service ou l’outil de gestion à **ChallengeResponseAlgorithm** (défini dans l’exemple précédent).

3.  [**VerifyResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn297627) retourne **true** si l’authentification réussit.

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>Modifier ou réinitialiser un code PIN d’utilisateur


Pour modifier le code PIN associé à une carte à puce :

1.  Accédez à la carte et générez l’objet [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) associé.
2.  Appelez [**RequestPinChangeAsync**](https://msdn.microsoft.com/library/windows/apps/dn263823) pour présenter à l’utilisateur une interface utilisateur afin qu’il puisse effectuer cette opération.
3.  Si le code PIN a été correctement modifié, l’appel retourne **true**.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

Pour demander une réinitialisation du code PIN:

1.  Appelez [**RequestPinResetAsync**](https://msdn.microsoft.com/library/windows/apps/dn263825) pour initier l’opération. Cet appel comprend une méthode [**SmartCardPinResetHandler**](https://msdn.microsoft.com/library/windows/apps/dn297701) qui représente la carte à puce et la demande de réinitialisation du code PIN.
2.  [**SmartCardPinResetHandler**](https://msdn.microsoft.com/library/windows/apps/dn297701) fournit des informations utilisées par **ChallengeResponseAlgorithm**, encapsulé dans un appel [**SmartCardPinResetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn297693), pour comparer la valeur de demande de la carte et la clé d’administration fournie par le service ou l’outil de gestion pour authentifier la demande.

3.  Si la demande réussit, l’appel à [**RequestPinResetAsync**](https://msdn.microsoft.com/library/windows/apps/dn263825) est terminé, et la valeur **true** est retournée si le code PIN est correctement réinitialisé.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>Supprimer une carte à puce ou une carte à puce virtuelle


Quand une carte à puce physique est retirée, un événement [**CardRemoved**](https://msdn.microsoft.com/library/windows/apps/dn263875) est déclenché lorsque la carte est supprimée.

Associez le déclenchement de cet événement sur le lecteur de carte à la méthode qui définit le comportement de votre application suite au retrait de la carte ou du lecteur comme gestionnaire d’événements. Vous pouvez simplement fournir à l’utilisateur une notification indiquant que la carte a été retirée.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

Le retrait d’une carte à puce virtuelle est géré par programme. Pour cela, récupérez la carte, puis appelez [**RequestVirtualSmartCardDeletionAsync**](https://msdn.microsoft.com/library/windows/apps/dn263850) à partir de l’objet [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) retourné.

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```