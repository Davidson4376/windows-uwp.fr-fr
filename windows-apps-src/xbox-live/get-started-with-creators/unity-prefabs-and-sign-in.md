---
title: XBL dans Unity Prefabs et connectez-vous
description: Aborde les prefabs sociales et les exemples de scripts pour les services sociaux sur Xbox Live
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660034"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Unity Prefabs et connectez-vous l’aide de scripts

Cet article va vous guider dans Ajout Xbox Live connectez-vous à vos projets Unity. Il existe deux façons, vous pouvez obtenir connectez-vous si vous avez téléchargé le [plug-in Unity Xbox Live](https://github.com/Microsoft/xbox-live-unity-plugin). Vous pouvez utiliser les prefabs contenus dans le plug-in ou utiliser les scripts et les bibliothèques incluses pour générer un script Xbox Live connectez-vous dans vos propres GameObjects personnalisé.

> [!IMPORTANT]
> Cet article s’applique à une version du plug-in avant une mise à jour effectuée en mai 2018 (version 1804). Si vous installé le plug-in Xbox Live passé ce délai, ou que vous ne le n'avez pas encore téléchargé peut avoir une version plus récente qui a des différences significatives pour la connexion est effectuée. En outre, vous constaterez que les captures d’écran de ce plug-in ne correspondent pas à ceux de la version la plus récente. Reportez-vous à la place à la [article pour la mise à jour préfabriqué connectez-vous](playerauthentication-prefab-sign-in.md) ainsi que [l’article détaillant les méthodes de mise à jour pour les scripts de connexion](sign-in-manager.md).

## <a name="before-you-begin"></a>Avant de commencer

Avant de commencer à ajouter les services sociaux Xbox Live à votre jeu Unity, il existe quelques étapes, que vous devez suivre avant de vous plonger. Tout d’abord, assurez-vous que vous avez téléchargé et intégré le [plug-in de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin). Deuxièmement, vous voudrez avoir votre titre réservé et publiées via le [Microsoft Development Center](https://developer.microsoft.com/en-us/games/uwp). Lecture [créer un nouveau titre de programme Xbox Live Creators](../get-started-with-creators/create-and-test-a-new-creators-title.md) pour obtenir des instructions sur la publication de votre titre.
Pour finir, lire [configurer le Live dans Unity Xbox](../get-started-with-creators/configure-xbox-live-in-unity.md) pour configurer votre environnement de Unity correctement et de configurer votre titre pour utiliser les services Xbox Live. Une fois que votre projet Unity est correctement configuré, il est temps pour en savoir plus sur les outils que vous pouvez utiliser dans le titre de la Xbox Live est activé, ainsi que les deux méthodes principales que vous pouvez implémenter la Xbox Live dans Unity : prefabs et les scripts.

## <a name="prefabs"></a>Prefabs

Unity a un type de ressource prefab qui vous permet de stocker un GameObject terminée avec des composants et propriétés. Le préfabriqué agit comme un modèle à partir de laquelle vous pouvez créer des nouvelles instances d’objet dans votre scène Unity.
[En savoir plus sur prefabs depuis le site Web Unity](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage).

Le plug-in de Xbox Live Unity fournit quelques prefabs que vous pouvez utiliser dans votre projet pour utiliser des fonctionnalités Xbox Live. Les prefabs décrites dans cet article vous permettra de se connecter, [ajouter la prise en charge multi-utilisateur](../get-started-with-creators/add-multi-user-support.md) à votre titre ou d’afficher la liste d’amis de signé dans Xbox Live profil. Vous pouvez trouver ces et autres prefabs sous le **projet** onglet en suivant le chemin d’accès : **Ressources > Xbox Live > Prefabs**.

### <a name="the-userprofile-prefab"></a>Le préfabriqué UserProfile

Le préfabriqué sociaux premier et le plus important est le **UserProfile** prefab. Le **UserProfile** préfabriqué a tous les éléments requis pour permettre une Xbox Live connectez-vous. Ceci est très important, car vous devez connectez-vous un utilisateur avant d’utiliser les services Xbox Live. Le préfabriqué contient le bouton de connexion et un GameObject pour représenter un connecté dans le lecteur par leur Gamertag, Gamerpic et leur score de joueur.

> [!NOTE]
> Pour utiliser une des autres prefabs Xbox Live, vous devez inclure un **UserProfile** prefab ou appeler manuellement la connexion à l’API.

![Préfabriqué UserProfile actifs et de hiérarchie](../images/unity/unity-userprofile-views.png)

Si vous développez le **UserProfile** prefab dans le **projet** Panneau de configuration ou dans le **hiérarchie** après qu’il a été ajouté à une scène, vous verrez que le **UserProfile**  préfabriqué contient deux GameObjects qu’il contient. Le premier objet est la **SignInPanel** qui contient l’expérience du bouton de connexion. Le deuxième objet est la **ProfileInfo** qui contiendra les informations relatives à l’utilisateur une fois qu’ils se connectent. Le **UserProfile** préfabriqué est ce que vous allez utiliser pour représenter les informations de n’importe quel utilisateur Xbox Live signé dans localement votre titre.

### <a name="the-xboxliveuser-prefab"></a>Le préfabriqué XboxLiveUser

Le **UserProfile** préfabriqué utilise une deuxième sociaux préfabriqué dans son code appelé le **XboxLiveUser**. Utilisation de cette préfabriqué n’est pas immédiatement évidente qu’il dispense à ajouter à la hiérarchie de la scène, comme il peut simplement être instanciée dans le code. Le **XboxLiveUser** n’a aucune représentation visuelle, elle contient simplement les détails relatifs à l’utilisateur de Xbox Live. Vous devez une instance de la **XboxLiveUser** pour chaque instance de la **UserProfile**. Ceci est important lorsque [Ajout de prise en charge multi-utilisateur](../get-started-with-creators/add-multi-user-support.md) pour votre titre. En plus de contenant les informations relatives à l’utilisateur une fois la connexion cette préfabriqué est également un wrapper pour le code utilisé pour la connexion à un utilisateur de Xbox Live.

## <a name="sign-in-with-the-userprofile-prefab"></a>Connectez-vous avec le préfabriqué UserProfile

Les prefabs de plug-in de Xbox Live Unity existent pour effectuer certaines tâches de développement beaucoup plus facile. Pour activer la Xbox Live connexion pour votre projet Unity, vous devez simplement à utiliser le **UserProfile** et **XboxLiveServices** prefabs ainsi que d’un Unity **EventSystem**.

Faites glisser au préalable le **UserProfile** prefab dans une scène. Dans l’idéal, le **UserProfile** doivent être placés sur l’écran du menu initial de votre projet.

![Faites glisser userprofile à la hiérarchie](../images/unity/drag-userprofile.gif)

Outre le **UserProfile** préfabriqué, vous devez également vous assurer que le **XboxLiveServices** préfabriqué est présent dans au moins la première scène de votre projet.
Le **XboxLiveServices** préfabriqué vous permet d’activer ou non certaines prefabs enregistrera les informations pour le débogage. Cela est utile pour vérifier sur le comportement prefab.

![Recherchez les xboxliveservices prefab](../images/unity/check-for-xboxliveservices.gif)

Enfin, le **UserProfile** requiert également un **EventSystem** pour s’exécuter correctement. Il peut être ajouté en effectuant un clic droit sur la scène connectez-vous, puis en suivant les étapes **GameObject--> interface utilisateur--> EventSystem**.

![Ajouter un événement système](../images/unity/add_event_system.gif)

Si vous entrez en mode lecture, le service sera automatiquement connecter un utilisateur. Dans Unity, le Kit de développement Xbox Live simule des appels au service Xbox Live et envoie des données fictives précédent pour travailler avec. Pour afficher les données actives, vous devez générer le projet comme une application UWP et exécutez-le à partir de Visual Studio. Consultez [configurer Xbox Live dans Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) pour plus d’informations. Lorsque vous entrez mode de jeu dans Unity, le préfabriqué sera rempli avec les données factices, qui simulent des informations telles que le Gamertag, Gamerpic et le score de joueur d’un lecteur. Voici les informations qui doivent être affichées par le **UserProfile** prefab.

Une réussite de connexion à ressemblera à ce qui suit : ![userprofile réussie play](../images/unity/correct-user-profile-play.gif)

Si vous n’avez pas configuré votre projet pour vous connecter à Xbox Live correctement passer en mode de lecture désactivera le bouton se connecter et afficher un message d’erreur.

Voici un exemple d’un échec connexion en raison d’une mauvaise configuration des applications Xbox Live.
![Échec de la lecture de profil utilisateur](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>Script de connexion

Maintenant que vous savez comment utiliser le **UserProfile** prefab il serait préférable de consulter le script sous-jacent qui régit les fonctionnalités de la préfabriqué. Si vous examinez le **UserProfile** dans le **inspecteur** vous verrez qu’il a un **UserProfile.cs** script attaché. Ce script a tout ce dont vous avez besoin pour connecter un utilisateur et de charger les informations de profil que vous souhaitez afficher sur la connexion. Toutefois, au lieu de regarder le préfabriqué entière (qui peut être mis à jour au fil du temps) nous allons examiner quelques exemples de lignes de code pour comprendre ce qui est nécessaire de vous connecter à un utilisateur de Xbox Live.

### <a name="the-xboxliveuser-class"></a>La classe XboxLiveUser

Les appels nécessaires pour la connexion à un utilisateur sont encapsulées dans le `XboxLiveUserInfo` classe. Dans le **UserProfile.cs** script, vous pouvez constater que qu’une instance de la `XboxLiveUserInfo` classe appelée `XboxLiveUser`. Nous allons utiliser le même nom de variable dans notre exemple de code. Le `XboxLiveUserInfo` classe contient une instance de la `XboxLiveUser` classe appelée `User` comme l’un de ses variables membres. Le `XboxLiveUser` classe contient les fonctions d’authentification requises pour se connecter le `XboxLiveUser`. Vous allez utiliser l’instance de la `XboxLiveUser` classe `User` connectez-vous avec les utilisateurs, ainsi que pour obtenir des informations qui les décrit comme leurs Gamertag, Gamerpic et score de joueur. Pour ce faire, vous devez initialiser une instance de la `XboxLiveUserInfo` classe et utiliser résultant `XboxLiveUserInfo.User` à appel connectez-vous.

### <a name="initialize-the-xboxliveuser"></a>Initialiser le XboxLiveUser

L’initialisation de l’utilisateur de Live Xbox est la première étape avant de réellement vous connecter leur Xbox Live. Cela très simplement dans le code en utilisant le `XboxLiveUserInfo.Initialize()` (fonction).
Dans notre exemple de code que nous utilisons le `XboxLiveUser` variable de membre en tant que notre `XboxLiveUserInfo` de l’instance et l’initialiser à utiliser pour la connexion.

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

Examinez cet exemple de code, vous verrez que le `XboxLiveUserInfo.Initialize()` fonction est appelée en réponse à un clic de bouton. La version complète **UserProfile.cs** prefab script a du code qui permet de pour la connexion automatique où `XboxLiveUserInfo.Initialize()` est appelée sans interaction.
Le `XboxLiveUserInfo.Initialize()` fonction créera un nouveau `XboxLiveUserInfo.User` qui nous permettra d’appeler les fonctions d’authentification contenues dans le `XboxLiveUserInfo.User` classe.

### <a name="call-sign-in"></a>Connexion de l’appel

Une fois que le XboxLiveUser a été initialisé, il est temps de dans l’appel de connexion. Dans **UserProfile.cs** connexion est appelée le `SignInAsync()` de UserProfile.cs (fonction). Dans l’exemple de code précédent nous simplement attendre le `XboxLiveUser` être initialisé avant d’appeler le `SignInAsync()` (fonction).

> [!NOTE]
> Il est nécessaire d’attendre le `XboxLiveUser` être initialisée avant l’appel de connexion, car le `XboxLiveUser` contient le `XboxLiveUser.User` propriété qui sert à appel connectez-vous.

Dans **UserProfile.cs** le `SignInAsync()` fonction contient deux fonctions connexion qui peuvent être utilisées pour connecter l’utilisateur. `XboxLiveUser.User.SignInSilentlyAsync()` et `XboxLiveUser.User.SignInAsync()` ce sont des fonctions de connecter l’utilisateur. Le `SignInAsync()` (fonction) est un bon exemple d’utilisation de ces fonctions en conséquence. L’exemple de code suivant montre une méthode appropriée pour appeler les deux fonctions d’authentification :

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

Dans cet exemple, les résultats des appels de connexion sont stockés dans la variable `signInStatus`. Cela nous permet de vérifier ou non la connexion a réussi et d’agir en conséquence. Dans cet exemple, que la fonction tente tout d’abord se connecter en mode silencieux, en cas de la connexion en mode silencieux la fonction appelle ensuite le signe normal dans la fonction. Une fois que vous avez un appel réussi à une fonctions de connexion de que l’utilisateur est connecté. Vous pouvez maintenant utiliser le `XboxLiveUser.User` pour obtenir et afficher des détails sur l’utilisateur connecté. Jetez un coup de œil à la `LoadProfileInfo()` fonctionner dans **UserProfile.cs** pour obtenir un exemple montrant comment utiliser le `XboxLiveUser.User` pour afficher des informations relatives à un utilisateur connecté.

## <a name="build-and-test-sign-in"></a>Générer et tester la connexion

Lorsque vous exécutez votre titre dans l’éditeur, vous verrez des données fictives lorsque vous essayez d’utiliser des fonctionnalités Xbox Live. Pour vous connecter avec un profil réel et tester des fonctionnalités Xbox Live dans votre titre, vous devez créer une solution UWP et l’exécuter dans Visual Studio.  Vous pouvez créer le projet UWP dans Unity en suivant ces étapes :

1. Ouvrez le **paramètres de Build** en sélectionnant **fichier** > **paramètres de Build**.
2. Ajoutez tous les scènes que vous souhaitez inclure dans votre build sous le **scènes dans Build** section.
3. Basculez vers le **plateforme Windows universelle** en sélectionnant **plateforme Windows universelle** sous **plateforme** et en cliquant sur **plateforme de commutation**.
4. Définissez **SDK** à **10.0.15063.0** ou supérieur.
5. Pour activer la vérification de débogage de script **Unity C# projets**.
6. Cliquez sur **Build** et spécifiez l’emplacement du projet.

Une fois la build terminée, Unity aura généré un nouveau fichier de solution UWP que vous devez exécuter dans Visual Studio :

1. Dans le dossier que vous avez spécifié, ouvrez  **&lt;nom_projet&gt;.sln** dans Visual Studio.
2. Dans la barre d’outils en haut, sélectionnez **x64** et le déployer vers le **ordinateur Local**.

Si vous avez activé **le débogage de script** lorsque vous avez généré la solution UWP à partir d’Unity, puis vos scripts se trouve sous le **Assembly-CSharp (Windows universel)** projet.

> [!NOTE]
> Avant d’utiliser votre build de Visual Studio pour tester votre jeu avec des données réelles, suivez [cette liste de vérification](test-visual-studio-build.md) afin de garantir votre titre sera en mesure d’accéder au service Xbox Live.

## <a name="troubleshooting"></a>Résolution des problèmes

Si vous rencontrez des problèmes de connexion à la Xbox Live essayez de lire [dépannage Xbox Live connectez-vous](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md).
