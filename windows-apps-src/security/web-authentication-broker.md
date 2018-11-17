---
title: Service Broker d’authentification web
description: Cet article explique comment connecter votre application de plateforme Windows universelle (UWP) à un fournisseur d’identité en ligne qui utilise des protocoles d’authentification comme OpenID ou OAuth (par exemple, Facebook, Twitter, Flickr, Instagram, etc.).
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 88210182f46fa8149e4b2d0278d7df89033e62b6
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7163508"
---
# <a name="web-authentication-broker"></a>Service Broker d’authentification web




Cet article explique comment connecter votre application de plateforme Windows universelle (UWP) à un fournisseur d’identité en ligne qui utilise des protocoles d’authentification comme OpenID ou OAuth (par exemple, Facebook, Twitter, Flickr, Instagram, etc.). La méthode [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) envoie une demande au fournisseur d’identité en ligne, puis obtient en retour un jeton d’accès qui décrit les ressources du fournisseur auxquelles l’application a accès.

>[!NOTE]
>Pour obtenir un exemple de code utilisable complet, clonez le [référentiel WebAuthenticationBroker sur GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620622).

 

## <a name="register-your-app-with-your-online-provider"></a>Inscrire votre application auprès de votre fournisseur en ligne


Vous devez inscrire votre application auprès du fournisseur d’identité en ligne auquel vous voulez vous connecter. Vous pouvez déterminer comment inscrire votre application auprès du fournisseur d’identité. Après l’inscription, le fournisseur en ligne vous donne généralement un ID et/ou une clé secrète pour votre application.

## <a name="build-the-authentication-request-uri"></a>Créer l’URI de la demande d’authentification


L’URI de la demande se compose de l’adresse à laquelle vous envoyez la demande d’authentification à votre fournisseur en ligne, complétée par d’autres informations requises, comme un ID d’application ou secret, un URI de redirection où l’utilisateur est envoyé après avoir effectué l’authentification, ainsi que le type de réponse attendu. Vous pouvez déterminer auprès de votre fournisseur les paramètres requis.

L’URI de la demande est envoyé en tant que paramètre *requestUri* de la méthode [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066). Il doit s’agir d’une adresse sécurisée (commençant par `https://`).

L’exemple suivant explique comment créer l’URI de la demande.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>Se connecter au fournisseur en ligne


Vous appelez la méthode [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) pour vous connecter au fournisseur d’identité en ligne et obtenir un jeton d’accès. La méthode prend l’URI construit à l’étape précédente comme paramètre *requestUri* et un URI vers lequel vous voulez rediriger l’utilisateur comme paramètre *callbackUri*.

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI, 
        endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

>[!WARNING]
>En plus de [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066), l’espace de noms [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044) contient une méthode [**AuthenticateAndContinue**](https://msdn.microsoft.com/library/windows/apps/dn632425). N’appelez pas cette méthode. Il est conçu pour les applications ciblant uniquement Windows Phone 8.1 et avec Windows 10 est désapprouvé.

## <a name="connecting-with-single-sign-on-sso"></a>Connexion par authentification unique (SSO).


Par défaut, le service Broker d’authentification web n’autorise pas la persistance des cookies. C’est pourquoi, même si l’utilisateur de l’application indique qu’il souhaite rester connecté (par exemple, en activant une case à cocher dans la boîte de dialogue de connexion du fournisseur), il doit se connecter chaque fois qu’il souhaite accéder aux ressources de ce fournisseur. Pour se connecter avec l’authentification unique, votre fournisseur d’identité en ligne doit avoir activé l’authentification unique pour le service Broker d’authentification web et votre application doit appeler la surcharge de la méthode [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212068) qui ne prend pas de paramètre *callbackUri*. Ceci permettra aux cookies persistants d’être stockés par le service Broker d’authentification web, de sorte que les futurs appels d’authentification par la même application ne nécessiteront pas une connexion répétée de la part de l’utilisateur (l’utilisateur est effectivement «connecté» jusqu’à ce que le jeton d’accès expire).

Pour prendre en charge l’authentification unique, le fournisseur en ligne doit vous permettre d’inscrire un URI de redirection sous la forme `ms-app://<appSID>`, où `<appSID>` correspond au SID de votre application. Vous pouvez obtenir le SID de votre application à partir de la page du développeur correspondant à votre application ou en appelant la méthode [**GetCurrentApplicationCallbackUri**](https://msdn.microsoft.com/library/windows/apps/br212069).

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

## <a name="debugging"></a>Débogage


Il existe plusieurs façons de résoudre les problèmes liés aux API du service Broker d’authentification web. Vous pouvez notamment examiner les journaux des opérations ou bien passer en revue les demandes et réponses web avec Fiddler.

### <a name="operational-logs"></a>Journaux des opérations

Il est souvent possible de déterminer ce qui ne fonctionne pas à l’aide des journaux des opérations. Il existe un canal dédié du journal des événements, Microsoft-Windows-WebAuth\Operational, qui permet aux développeurs de sites web de comprendre comment leurs pages web sont traitées par le service Broker d’authentification web. Pour l’activer, lancez eventvwr.exe et activez le journal Opérationnel sous Journaux des applications et des services\Microsoft\Windows\WebAuth. Le service Broker d’authentification web ajoute également une chaîne unique à la chaîne de l’agent utilisateur pour s’identifier sur le serveur web. Cette chaîne est «MSAuthHost/1.0». Notez que le numéro de version est susceptible de changer dans le futur. Vous ne devez donc pas nécessairement utiliser ce numéro de version dans votre code. Voici un exemple de chaîne d’agent utilisateur complète, suivi par des étapes de débogage complètes.

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  Activez les journaux opérationnels.
2.  Exécutez l’application sociale Contoso. ![Observateur d’événements affichant les journaux opérationnels WebAuth](images/wab-event-viewer-1.png)
3.  Les entrées des journaux générés peuvent être utilisées pour comprendre le comportement du service Broker d’authentification web plus en détail. Dans ce cas, ces entrées peuvent être les suivantes :
    -   Début de la navigation: consigne le moment où AuthHost démarre, et contient des informations sur les URL de démarrage et de terminaison.
    -   ![Illustration des détails de la section Début de la navigation](images/wab-event-viewer-2.png)
    -   Achèvement de la navigation: consigne l’achèvement du chargement d’une page web.
    -   Balise META : consigne le moment où une balise META est rencontrée, y compris les détails.
    -   Arrêt de la navigation : navigation arrêtée par l’utilisateur.
    -   Erreur de navigation : AuthHost rencontre une erreur de navigation au niveau d’une URL incluant HttpStatusCode.
    -   Fin de la navigation : une URL de terminaison est rencontrée.

### <a name="fiddler"></a>Fiddler

Le débogueur web Fiddler peut être utilisé avec des applications.

1.  Étant donné qu’AuthHost s’exécute dans son propre conteneur d’application, pour lui donner la fonctionnalité réseau privé, vous devez définir une clé de Registre: Windows Registry Editor Version 5.00

    **HKEY\_LOCAL\_MACHINE**\\**SOFTWARE**\\**Microsoft**\\**WindowsNT**\\**CurrentVersion**\\**Image File Execution Options**\\**authhost.exe**\\**EnablePrivateNetwork** = 00000001

    Si vous ne disposez pas de cette clé de Registre, vous pouvez la créer dans une invite de commandes avec des privilèges d’administrateur.

    ```cmd 
    REG ADD "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe" /v EnablePrivateNetwork /t REG_DWORD /d 1 /f
    ```

2.  Ajoutez une règle pour AuthHost, car c’est de là que provient le trafic sortant.
    ```syntax
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.a.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
    D:\Windows\System32>CheckNetIsolation.exe LoopbackExempt -s
    List Loopback Exempted AppContainers
    [1] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
        SID:  S-1-15-2-1973105767-3975693666-32999980-3747492175-1074076486-3102532000-500629349
    [2] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
        SID:  S-1-15-2-166260-4150837609-3669066492-3071230600-3743290616-3683681078-2492089544
    [3] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.a.p_8wekyb3d8bbwe
        SID:  S-1-15-2-3506084497-1208594716-3384433646-2514033508-1838198150-1980605558-3480344935
    ```

3.  Ajoutez une règle de pare-feu pour le trafic entrant vers Fiddler.