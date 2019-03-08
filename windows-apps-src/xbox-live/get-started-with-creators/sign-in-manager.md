---
title: Connectez-vous avec le SignInManager dans Unity
description: Vue d’ensemble du Gestionnaire de connexion du plug-in Unity
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641724"
---
# <a name="scripting-sign-in"></a>Script de connexion

Pour ajouter une connexion à vos propres objets de jeu personnalisés, que vous devez créez un script dans un GameObject. Supposons que le préfabriqué PlayerAuthentication ne tient pas votre jeu et vous souhaitez avoir votre propre connexion-dans Panneau de configuration, cet article vous guidera dans les étapes de base de l’ajout de logique de connexion pour votre titre.

## <a name="sign-in-with-the-signinmanager"></a>Connectez-vous avec le SignInManager

Le plug-in Xbox Live Unity contient un script pour le `SignInManager` sous le chemin d’accès du fichier **actifs >> XboxLive >> Scripts >> SignInManager.cs**. Le gestionnaire est une classe Singleton qui peut être appelée formulaire n’importe où dans votre titre en faisant référence au titre de le *Instance* de la `SignInManager`. Cela *Instance* ne pas devoir être initialisé et vous pouvez utiliser l’informatique dès que votre jeu commence. Vous pouvez accéder à tous les ses propriétés publiques et les fonctions en faisant référence à la *Instance* comme `SignInManager.Instance`.

Le `SignInManager` contient tout le code nécessaire pour cela inclut la gestion de l’authentification pour votre titre, connectez-vous, déconnexion, et l’obtention d’informations sur les utilisateurs qui sont connecté en tant que le lecteur.

### <a name="calls-and-results"></a>Les appels et les résultats

Le `SignInManager` a trois fonctions de routine copropriétaire async `SignInPlayer(int playerNumber)`, `SignOutPlayer(int playerNumber)`, et `SwitchUser(int playerNumber)`, cet événement de déclencheur des fonctions pour collecter les résultats de l’appel et d’agir en conséquence. Vous pouvez ajouter des fonctions correspondantes à votre script et les assigner à la `SignInManager.Instance`de liste de rappel. Les fonctions d’événement sont `OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, et `OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`. Chacune sur des fonctions de l’événement écoute l’événement décrit dans son nom. Vous pouvez ajouter vos propres fonctions à la liste du gestionnaire rappel dans le titre de votre `Start()` fonction avec le code suivant.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

Cet extrait de code ajoute des écouteurs de connexion et de déconnexion pour le joueur associé playerNumber de cette GameObject. De cette GameObject `OnPlayerSignIn` fonction sera appelée lorsque le `SignInManager` détecte une tentative de connexion est terminée et sa `OnPlayerSignOut` fonction sera appelée lorsque le SignInManager détecte une déconnexion. Les fonctions d’événement dans votre GameObject doivent avoir un type de retour et paramètres pour correspondre au type de la fonction appelé par le SignInManager. À la fois le `OnPlayerSignIn` et `OnPlayerSignOut` sont des fonctions void nécessitant un `XboxLiveUser`, `XboxLiveAuthStatus`et une chaîne en tant que leurs paramètres. L’interpréteur de commandes de vos fonctions peut ressembler à ce qui suit :

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

Dans les deux fonctions vérifier le `XboxLiveAuthStatus` pour vous assurer que votre appel à la `SignInManager.Instance` a réussi. Sur un appel réussi le `XboxLiveUser` sera le `XboxLiveUser`, qui a été signé dans notre hors par `SignInManager`. Lorsque l’appel ne réussit le `errorMessage` chaîne contient plus d’informations sur la raison de l’échec.

Ajout de quelques lignes de code de vérification pour un appel réussi entraînerait de code semblable au suivant :

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

En appelant connectez-vous et capturer l’événement pour le résultat obtenu, vous pouvez gérer les connexion et de déconnexion pour votre titre.

## <a name="get-signed-in-player-information"></a>Se connecter dans les informations de lecteur

En plus de se connecter au service des lecteurs le SignInManager effectue le suivi de tous les utilisateurs connectés. Sur les PC, ce sera limité à un seul lecteur connecté, et sur la Xbox, il est limité à 16. Vous pouvez vérifier comment proche de la limite sont en comparant le résultat de `SignInManager.Instance.GetCurrentNumberOfPlayers()` au résultat de `SignInManager.Instance.GetMaximumNumberOfPlayers()`. Le SignInManager a un dictionnaire de joueurs connecté indexées par ce joueur *playerNumber*. Vous pouvez l’utiliser pour récupérer des informations de base sur le lecteur accessible à partir de leur sont associées `XboxLiveUser`.

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

Ce petit bout de code vérifie s’il existe un lecteur connecté à l’emplacement numéro player pour cette GameObject, puis stocke ce gamertag les utilisateurs à afficher s’ils sont connectés. Alors que le `XboxLiveUser` contient le signé dans l’identité des utilisateurs et l’ID d’utilisateur Xbox (xuid), vous devez appeler d’autres services tels que le `SocialManager` pour accéder aux informations telles que gamerpic et le score de joueur.

## <a name="destroying-your-sign-in-gameobject"></a>Détruire votre GameObject connectez-vous

Lors de la destruction d’un objet de jeu qui utilise un des gestionnaires de plug-in Xbox Live comme le `SignInManager` ou `SocialManager`, généralement lors du chargement d’une nouvelle scène, il est important de supprimer toutes les fonctions ajoutées à la liste des écouteurs d’événements pour le gestionnaire. Dans l’exemple de code pour cet article, nous devons supprimer les fonctions que nous avons ajouté pour les écouteurs d’événements pour l’authentification et de déconnexion. Nous supprimons ces fonctions à partir de la `SignInManager` dans le `OnDestroy()` fonction de notre GameObject.

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

Ce code supprime les fonctions de rappel de connexion et de déconnexion pour le lecteur associé à cette GameObject.

## <a name="testing-you-code-in-visual-studio"></a>Testez votre code dans Visual Studio

Outre le [étapes requises pour créer votre jeu dans Visual Studio](configure-xbox-live-in-unity.md#build-and-test-the-project), mentionné dans le [configurer votre titre Live Xbox pour Unity](configure-xbox-live-in-unity.md) l’article, il existe une étape supplémentaire nécessaire pour tester votre jeu correctement dans Visual Studio. Vous devez mettre à jour une propriété du fichier package.appxmanifest.xml. Pour ce faire :

1. Rechercher de l’Explorateur de solutions pour le fichier package.appxmanifest.xml
2. Cliquez avec le bouton droit sur le fichier et choisissez Afficher le Code
3. Sous le `<Properties><\/Properties>` , ajoutez la ligne suivante : ' < uap:SupportedUsers > plusieurs <\/uap:SupportedUsers >.
4. Déployer le jeu sur votre Xbox en démarrant une build de débogage à distance à partir de Visual Studio. Vous pouvez trouver des instructions pour configurer votre titre sur une console Xbox dans le [configurer votre UWP sur un environnement de développement Xbox](../../xbox-apps/development-environment-setup.md) article.

> [!NOTE]
> L’élément de configuration a changé peut vous sembler multijoueurs activé mais il est toujours nécessaire exécuter votre jeu dans les scénarios de lecteur unique.

## <a name="policies-and-limitations"></a>Stratégies et Limitations

Il existe quelques limitations du titre que vous pouvez prendre en compte lorsque vous développez votre expérience de connexion et les stratégies de connexion.

- Après la du titre de votre première connexion, vous devez conserver au moins un lecteur connecté. Le `SignInManager` génère une erreur et de l’appel d’échouer si vous tentez de vous déconnecter du dernier utilisateur connecté. Il est également important de noter que vous ne pouvez pas appeler `SignInManager.Instance.SwitchUser(int playerNumber)`, dans le dernier connecté dans le lecteur comme il essaiera de se déconnecter le lecteur avant de vous connecter un nouveau lecteur.

- PC peut uniquement un utilisateur de connexion à la fois, Console peut se connecter jusqu'à 16 lecteurs à la fois.

- Le titre n’est pas réellement autorisé à se déconnecter un lecteur du système d’exploitation, en raison de cette déconnexion peut ne pas fonctionner comme prévu. Le SignInManager peut déconnecter un utilisateur et de sortie où le titre est concerné, mais ne peut pas tout le monde se déconnecter de l’ordinateur, sur que le titre est déployé.

- Plusieurs connexion de l’utilisateur est uniquement disponible sur la Xbox, une seule Console.

- Les comptes invités ne sont pas disponibles pour les titres de programme Xbox Live Creators.