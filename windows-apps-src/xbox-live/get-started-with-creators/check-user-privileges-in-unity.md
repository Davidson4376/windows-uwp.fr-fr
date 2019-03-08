---
title: Vérifier les paramètres de privilèges d’utilisateur dans Unity
description: Découvrez comment vérifier les paramètres de privilège pour signé dans un compte Xbox Live.
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, comptes, comptes de test, contrôle parental, des privilèges d’utilisateur, la mise en œuvre interdictions, incitatives
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620404"
---
# <a name="check-user-privilege-settings-in-unity"></a>Vérifier les paramètres de privilèges d’utilisateur dans Unity
Sur Xbox Live, compte de chaque utilisateur authentifié est associé à des privilèges. Les privilèges contrôlent les fonctionnalités de Xbox Live un utilisateur peut accéder à un moment donné dans le temps. Certaines de ces privilèges sont pour les fonctionnalités système contrôlé, tandis que d’autres peuvent être associés avec des jeux spécifiques ou des abonnements de l’extension. En outre, contrôle parental et les BAN émis par l’équipe de mise en œuvre de Xbox Live peut limiter les privilèges d’un utilisateur. Ces privilèges couvrent un nombre de scénarios courants, y compris du contenu généré par l’utilisateur en mode multijoueur, l’accès à, discuter, ou vers la vidéo de diffusion en continu. Jeux utilisent ces informations pour prendre des décisions de contrôle et personnalisation des accès.

## <a name="prerequisites"></a>Conditions préalables
Afin de déterminer les paramètres des privilèges utilisateur, doit avoir configuré votre jeu pour l’authentification avec Xbox Live et parvenus à un utilisateur.

>[!IMPORTANT]
> Si vous testez votre jeu dans l’éditeur Unity, votre jeu n’est pas connecté au service Xbox Live et utilise des données fictives pour simuler une connexion. Cela entraîne une valeur null lorsque vous recherchez des privilèges. Pour tester avec des données réelles, effectuer une génération de la plateforme Windows universelle de votre jeu Unity et ouvrez le fichier projet généré dans Visual Studio.

Les articles suivants décrivent les étapes que vous pouvez effectuer :

* [Se connecter à Xbox Live dans Unity (build et test de connexion)](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [Tester votre build de jeux Unity dans Visual Studio](test-visual-studio-build.md)

## <a name="determine-privileges"></a>Déterminer les privilèges
Privilèges d’un utilisateur sont exécutées dans le jeton reçu au moment de l’authentification. Dans Unity, vous pouvez accéder à la liste des privilèges dont dispose un utilisateur dans le `XboxLiveUser` classe, une fois que l’utilisateur est connecté avec succès. Privilèges sont stockés sous forme de chaîne unique, séparé par un espace. Par exemple, vous pouvez voir un utilisateur avec les privilèges suivants :

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

Si vous souhaitez rechercher une autorisation spécifique, vous pouvez vérifier a posteriori si les `Privileges` propriété contient le code associé :

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>Codes de privilège
Voici une liste des codes de privilèges possibles qui peuvent être retournées.

| Code  | privilège  | Description   |
|------ |-----------------------------  |-------------------    |
| 190   | Diffusion             | Diffusez les jeux en direct.     |
| 197   | view_friends_list     | Peut afficher la liste d’amis de l’autre utilisateur.   |
| 198   | game_dvr              | Peuvent charger enregistrés dans le jeu des vidéos dans le cloud.      |
| 199   | share_kinect_content          | Kinect contenu enregistré peut être téléchargé vers le cloud pour l’utilisateur et rendue accessibles à tout le monde. |
| 203   | multiplayer_parties           | Peut rejoindre une session de tiers.     |
| 205   | communication_voice_ingame    | Peuvent faire partie discussion vocale au cours des parties et multijoueurs.    |
| 206   | communication_voice_skype     | Peut utiliser la communication vocale avec Skype sur Xbox One.   |
| 207   | cloud_gaming_manage_session   | Peut allouer et gérer un cluster de calcul cloud pour une session de jeu hébergée.    |
| 208   | cloud_gaming_join_session     | Peut rejoindre une session de calcul cloud.     |
| 209   | cloud_saved_games     | Peut enregistrer des jeux dans le stockage cloud titre.    |
| 211   | share_content     | Partager du contenu avec d’autres utilisateurs.    |
| 214   | premium_content   | Acheter, télécharger et lancer le contenu premium disponible avec l’abonnement Xbox Live Gold.     |
| 219   | subscription_content  | Peut acheter et télécharger le contenu de l’abonnement premium et utiliser les fonctionnalités d’abonnement premium.     |
| 220   | social_network_sharing    | Peut partager des informations de progression sur les réseaux sociaux.    |
| 224   | premium_video     | Peut accéder aux services vidéo premium.    |
| 235   | purchase_content  | Peuvent acheter du contenu.     |
| 247   | user_created_content  | Peut télécharger et afficher le contenu de créés par l’utilisateur en ligne.    |
| 249   | profile_viewing   | Peut afficher les profils de l’autre utilisateur.   |
| 252   | communications    | Pouvez utiliser du texte asynchrone de messagerie avec d’autres personnes.    |
| 254   | multiplayer_sessions  | Pouvez joindre des sessions pour un jeu multijoueur.   |
| 255   | add_friend    | Peut suivre les autres utilisateurs de Xbox Live et ajouter des amis Xbox Live.   |
