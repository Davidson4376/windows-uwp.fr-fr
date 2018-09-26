---
title: Stockage sécurisé des informations d’identification
description: Cet article décrit comment des applications de plateformeWindows universelle (UWP) peuvent utiliser le stockage sécurisé des informations d’identification pour stocker et récupérer des informations d’identification utilisateur en toute sécurité et les déplacer entre des appareils avec le compte Microsoft de l’utilisateur.
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: c6412f28e60ed0401fb96098fd38128a37491c8d
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4206731"
---
# <a name="credential-locker"></a>Stockage sécurisé des informations d’identification




Cet article décrit comment des applications de plateformeWindows universelle (UWP) peuvent utiliser le stockage sécurisé des informations d’identification pour stocker et récupérer des informations d’identification utilisateur en toute sécurité et les déplacer entre des appareils avec le compte Microsoft de l’utilisateur.

Supposons que vous ayez une application qui se connecte à un service pour accéder à des ressources protégées telles que des fichiers multimédias ou des réseaux sociaux. Votre service exige des informations de connexion pour chaque utilisateur. Vous avez créé une interface utilisateur dans votre application, qui obtient le nom et le mot de passe de l’utilisateur. Ces données sont ensuite utilisées pour connecter l’utilisateur au service. L’API de stockage sécurisé des informations d’identification vous permet de stocker les nom et mot de passe de votre utilisateur, puis de les récupérer facilement pour connecter automatiquement l’utilisateur lors de la prochaine ouverture de l’application, quel que soit l’appareil utilisé.

Les identifiants utilisateur stockés dans le stockage sécurisé des informations d’identification n'arrivent *pas* à expiration, ne sont *pas* affectés par l'[**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)et ne seront *pas* effacés en cas d'inactivité comme les données itinérantes traditionnelles. Cependant, vous pouvez stocker uniquement jusqu'à 20identifiants par application dans le stockage sécurisé des informations d'identification.

Le stockage sécurisé des informations d’identification fonctionne un peu différemment pour les comptes de domaine. Si des informations d’identification sont stockées avec votre compte Microsoft et que vous associez ce compte à un compte de domaine (comme le compte que vous utilisez au travail), vos informations sont transmises à ce compte de domaine. Toutefois, les nouvelles informations d’identification ajoutées lors de la connexion au compte de domaine ne seront pas transmises. Cela permet de s’assurer que les informations d’identification privées pour le domaine ne sont pas exposées à l’extérieur du domaine.

## <a name="storing-user-credentials"></a>Stockage des informations d’identification de l’utilisateur


1.  Obtenez une référence au stockage des informations d’identification de l’utilisateur à l’aide de l’objet [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) de l’espace de noms [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089).
2.  Créez un objet [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) contenant un identificateur pour votre application, le nom d’utilisateur et le mot de passe, puis passez le tout à la méthode [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) pour ajouter les informations d’identification au stockage des informations d’identification de l’utilisateur.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>Récupération des informations d’identification de l’utilisateur


Pour récupérer les informations d’identification de l’utilisateur du stockage des informations d’identification de l’utilisateur suite à la création d’une référence à l’objet [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081), plusieurs options s’offrent à vous.

-   Vous pouvez récupérer toutes les informations d’identification fournies par l’utilisateur pour votre application dans le stockage des informations d’identification de l’utilisateur à l’aide de la méthode [**PasswordVault.RetrieveAll**](https://msdn.microsoft.com/library/windows/apps/br227088).

-   Si vous connaissez le nom d’utilisateur associé aux informations d’identification stockées, vous pouvez récupérer toutes les informations d’identification associées à ce nom d’utilisateur à l’aide de la méthode [**PasswordVault.FindAllByUserName**](https://msdn.microsoft.com/library/windows/apps/br227084).

-   Si vous connaissez le nom de ressource associé aux informations d’identification stockées, vous pouvez récupérer toutes les informations d’identification associées à ce nom de ressource à l’aide de la méthode [**PasswordVault.FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083).

-   Enfin, si vous connaissez à la fois le nom d’utilisateur et le nom de ressource associés aux informations d’identifications, vous pouvez uniquement récupérer ces informations d’identifications à l’aide de la méthode [**PasswordVault.Retrieve**](https://msdn.microsoft.com/library/windows/apps/br227087).

Prenons un exemple. Nous avons stocké le nom de ressource globalement dans une application et nous connectons l’utilisateur automatiquement si nous trouvons des informations d’identification correspondantes. Si nous trouvons plusieurs informations d’identification pour le même utilisateur, nous demandons à l’utilisateur de sélectionner les informations d’identification par défaut à utiliser lors de la connexion.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    var loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }

    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}


private Windows.Security.Credentials.PasswordCredential GetCredentialFromLocker()
{
    Windows.Security.Credentials.PasswordCredential credential = null;

    var vault = new Windows.Security.Credentials.PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);
    if (credentialList.Count > 0)
    {
        if (credentialList.Count == 1)
        {
            credential = credentialList[0];
        }
        else
        {
            // When there are multiple usernames,
            // retrieve the default username. If one doesn't
            // exist, then display UI to have the user select
            // a default username.

            defaultUserName = GetDefaultUserNameUI();

            credential = vault.Retrieve(resourceName, defaultUserName);
        }
    }

    return credential;
}
```

## <a name="deleting-user-credentials"></a>Suppression des informations d’identification de l’utilisateur


La suppression des informations d’identification de l’utilisateur dans le stockage sécurisé des informations d’identification est aussi un processus rapide en deux étapes.

1.  Obtenez une référence au stockage des informations d’identification de l’utilisateur à l’aide de l’objet [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) de l’espace de noms [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089).

2.  Passez les informations d’identification à supprimer à la méthode [**PasswordVault.Remove**](https://msdn.microsoft.com/library/windows/apps/hh701242).

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>Meilleures pratiques


Utilisez uniquement le stockage sécurisé des informations d’identification pour les mots de passe et pas pour des données plus volumineuses.

Enregistrez les mots de passe dans le stockage sécurisé des informations d’identification uniquement si les critères suivants sont satisfaits :

-   L’utilisateur s’est connecté correctement.
-   L’utilisateur a choisi d’enregistrer les mots de passe.

Ne stockez jamais d’informations d’identification en texte brut en utilisant des données d’application ou des paramètres d’itinérance.
