---
title: Configurer Xbox Live dans Unity
description: Découvrez comment utiliser le plug-in de Xbox Live Unity pour configurer la Xbox Live dans votre jeu Unity.
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, Unity, configurer
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596724"
---
# <a name="configure-xbox-live-in-unity"></a>Configurer Xbox Live dans Unity

> [!NOTE]
> Le plug-in de Xbox Live Unity est recommandé uniquement pour [programme Xbox Live Creators](../developer-program-overview.md) membres, puisque il n’existe aucune prise en charge pour les primes ou multijoueurs.

Avec le [plug-in Unity Xbox Live](https://github.com/Microsoft/xbox-live-unity-plugin), l’ajout de prise en charge de Xbox Live à un jeu Unity est facile, ce qui vous donne plus de temps pour vous concentrer sur l’utilisation de Xbox Live manières que mieux adapter à votre titre.

Cette rubrique passera par le processus de configuration du plug-in dans Unity Xbox Live.

## <a name="prerequisites"></a>Conditions préalables

Vous avez besoin des éléments avant de pouvoir utiliser Xbox Live dans Unity :

1. Un  **[compte Xbox Live](https://support.xbox.com/browse/my-account/manage-account/Create%20account)**.
1. L’inscription dans le  **[programme pour développeurs partenaires](https://developer.microsoft.com/store/register)**.
2. **[Mise à jour anniversaire de Windows 10](https://microsoft.com/windows)**  ou version ultérieure
3. **[Unity](https://store.unity.com/)**  versions **5.5.4p5** (ou ultérieure), **2017.1p5** (ou ultérieure), ou **2017.2.0f3** (ou version ultérieure) avec **[Microsoft Visual Studio Tools pour Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** et **.NET Store de Windows script principal**.
4. **[Visual Studio 2015](https://www.visualstudio.com/)**  ou **[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** (ou version ultérieure) avec la **outils développement d’applications universelles Windows**.
5. **[Xbox Live des Extensions de la plate-forme SDK](https://aka.ms/xblextsdk)**.


> [!NOTE]
> Si vous souhaitez utiliser le serveur principal de script IL2CPP avec Xbox Live, vous devez Unity 2017.2.0p2 ou une version ultérieure et la version de plug-in de Xbox Live Unity « 1802 préversion » ou une version ultérieure.


## <a name="import-the-unity-plugin"></a>Importer le plug-in Unity

Pour importer le plug-in dans votre projet Unity nouveau ou existant, procédez comme suit :

1. Accédez à l’onglet de mise en production de plug-in Unity Xbox Live sur [ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases).
2. Télécharger **XboxLive.unitypackage**.
3. Dans Unity, cliquez sur **actifs** > **importer un Package** > **Package personnalisé** et accédez à **XboxLive.unitypackage**.

![Importation réussie](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>(Facultatif) Configurer le plug-in fonctionne dans l’éditeur Unity (.NET 4.6 ou IL2CPP uniquement)

> [!NOTE]
> Prise en charge de la modification de la Version du Runtime de script dans Unity nécessite la version de plug-in Unity Xbox Live « 1711 version finale » ou une version ultérieure pour .NET 4.6 et de la version « 1802 préversion » ou une version ultérieure pour IL2CPP.

Il existe trois paramètres qui peuvent être configurés dans Unity pour définir la façon dont votre code est compilé :

1. Le **script principal** est le compilateur qui est utilisé. Unity prend en charge les deux serveurs script principaux différents pour la plateforme Windows universelle : .NET et IL2CPP.
2. Le **Version du Runtime de script** est la version du runtime de script qui exécute l’éditeur Unity.
3. Le **niveau de compatibilité d’API** est la surface d’API que vous allez générer votre jeu sur.

Le tableau suivant présente la matrice de prise en charge de script actuel pour le plug-in Xbox Live Unity :

| Écriture de scripts serveur principal     | Version du Runtime de script | Prise en charge     | Version de Unity minimale requise |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | .NET 3.5 équivalente       | Non            | Non applicable                            |
| Il2CPP                | .NET 4.6 équivalente       | Oui           | 2017.2.0p2                     |
| .NET                  | .NET 3.5 équivalente       | Oui           | Identique à la configuration requise          |
| .NET                  | .NET 4.6 équivalente       | Oui           | Identique à la configuration requise          |

Nous avons ajouté la prise en charge runtime de script supplémentaires à la Xbox Live Unity plug-in, en commençant par la version « 1711 Release ». Par défaut, le plug-in est configuré pour s’exécuter dans l’éditeur Unity avec .NET, scripts de serveur principal ou version du runtime du .NET 3.5. Si votre projet est à l’aide de la version du script runtime de .NET 4.6, vous devrez configurer le plug-in fonctionne correctement dans l’éditeur :

1. Dans l’Explorateur de projets Unity, accédez à **Xbox Live\Libs\UnityEditor\NET46** et sélectionnez toutes les DLL dans le dossier.
2. Dans la fenêtre d’inspecteur, vérifier **éditeur** sous **incluent des plateformes**.
3. Dans l’Explorateur de projets Unity, accédez à **Xbox Live\Libs\UnityEditor\NET35** et sélectionnez toutes les DLL dans le dossier.
4. Dans la fenêtre d’inspecteur, décochez la case **éditeur** sous **incluent des plateformes**.

![modifier le runtime de script](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> Ces étapes doivent être restaurées si vous modifiez la version de runtime de script dans votre projet à 3.5.

## <a name="set-visual-studio-as-the-ide-in-unity"></a>Ensemble de Visual Studio en tant que l’IDE dans Unity

Visual Studio est requis pour générer un [Universal Windows Platform (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) jeu. Vous pouvez définir votre IDE dans Unity pour Visual Studio en accédant à **modifier** > **préférences** > **outils externes** et en définissant le **Éditeur de Script externe** à Visual Studio.

![définir l’outil externe de Visual Studio](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Structure de fichier de plug-in Unity

Structure de fichier du plug-in Unity est divisé en parties suivantes :

* __Xbox Live__ contient les ressources de plug-in réelles qui sont inclus dans les données publiées **.unitypackage**.
    * __Éditeur__ contient des scripts qui fournissent l’interface utilisateur de configuration Unity base et traite les projets pendant la génération.
    * __Exemples__ contient un ensemble de fichiers de scène simples qui montrent comment utiliser les diverses prefabs et les connecter entre eux.
    * __Images__ est un petit ensemble d’images qui sont utilisées par les prefabs.
    * __Bibliothèques__ est où sont stockés les bibliothèques de Xbox Live.
    * __Prefabs__ contient différentes [préfabriqué de Unity](https://docs.unity3d.com/Manual/Prefabs.html) les objets qui implémentent des fonctionnalités Xbox Live.
    * __Scripts__ contient tous les fichiers de code qui appellent les API Xbox Live à partir des préfabriqués. Il s’agit d’un endroit idéal pour rechercher des exemples sur la façon d’appeler correctement les API Live Xbox.
    * __Tools\AssociationWizard__ contient la Xbox Live Assistant Association, utilisée pour extraire la configuration de l’application à partir de [partenaires](https://developer.microsoft.com/windows) pour une utilisation dans Unity.

## <a name="enable-xbox-live"></a>Activer la Xbox Live

Pour votre titre interagir avec Xbox Live, vous devez configurer la configuration initiale de Xbox Live. Faire cela facilement et à l’intérieur d’Unity à l’aide de l’Assistant Association de Xbox Live :

1. Dans le **Xbox Live** menu, sélectionnez **Configuration**.
2. Dans le **Xbox Live** fenêtre, sélectionnez **exécution de Xbox Live l’Assistant Association**.
3. Dans le **associer votre titre avec le Windows Store** boîte de dialogue, cliquez sur **suivant**, puis connectez-vous avec votre compte espace partenaires.
4. Sélectionnez l’application que vous souhaitez associer à ce projet, puis cliquez sur **sélectionnez**. Si vous n’apparaît pas, essayez de cliquer sur **Actualiser**. Vous pouvez également créer une nouvelle application en réservant un nom en cliquant sur **réserve**.
5. Vous devrez activer Xbox Live si vous n’avez pas déjà. Cliquez sur **activer** pour activer la Xbox Live dans le titre de votre.
6. Cliquez sur **Terminer** pour enregistrer votre configuration.

Pour appeler les services Xbox Live, votre ordinateur de bureau doit être en mode développeur et la valeur du même sandbox que votre titre est dans la configuration de Xbox Live. Vous pouvez vérifier les deux en examinant le **Xbox Live Configuration** fenêtre dans Unity :

1. **Configuration du Mode développeur** doit indiquer **activé**. Si l’état est **désactivé**, cliquez sur **basculer vers le Mode développeur**.
2. **Configuration du titre** > **bac à sable** doit avoir le même ID que **Configuration du Mode développeur** > **Mode développeur**.

![XBL activé](../images/unity/unity-xbl-enabled.png)

Consultez [les bacs à sable Xbox Live](../xbox-live-sandboxes.md) pour plus d’informations sur les bacs à sable.

## <a name="build-and-test-the-project"></a>Générer et tester le projet

Lorsque vous exécutez votre titre dans l’éditeur, vous verrez des données fictives lorsque vous essayez d’utiliser des fonctionnalités Xbox Live. Par exemple, si vous [ajouter une inscription dans les fonctionnalités](unity-prefabs-and-sign-in.md) et votre scène essayez de vous connecter, vous verrez **utilisateur fictif** apparaissent en tant que nom de votre profil, avec une icône d’espace réservé. Pour vous connecter avec un profil réel et tester des fonctionnalités Xbox Live dans votre titre, vous devez créer une solution UWP et l’exécuter dans Visual Studio.  Vous pouvez créer le projet UWP dans Unity en suivant ces étapes :

1. Ouvrez le **paramètres de Build** en sélectionnant **fichier** > **paramètres de Build**.
2. Ajoutez tous les scènes que vous souhaitez inclure dans votre build sous le **scènes dans Build** section.
3. Basculez vers le **plateforme Windows universelle** en sélectionnant **plateforme Windows universelle** sous **plateforme** et en cliquant sur **plateforme de commutation**.
4. Définissez **SDK** à **10.0.15063.0** ou supérieur.
5. Pour activer la vérification de débogage de script **Unity C# projets**.
6. Cliquez sur **Build** et spécifiez l’emplacement du projet.

![paramètres de build](../images/unity/build_settings.JPG)

Une fois la build terminée, Unity aura généré un nouveau fichier de solution UWP que vous devez exécuter dans Visual Studio :

1. Dans le dossier que vous avez spécifié, ouvrez  **&lt;nom_projet&gt;.sln** dans Visual Studio.
2. Dans la barre d’outils en haut, sélectionnez **x64** et le déployer vers le **ordinateur Local**.

Si vous avez activé **le débogage de script** lorsque vous avez généré la solution UWP à partir d’Unity, puis vos scripts se trouve sous le **Assembly-CSharp (Windows universel)** projet.

![Utilisateur faux : 123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> Avant d’utiliser votre build de Visual Studio pour tester votre jeu avec des données réelles, suivez [cette liste de vérification](test-visual-studio-build.md) afin de garantir votre titre sera en mesure d’accéder au service Xbox Live.

> [!IMPORTANT]
> En tant que de peut 2018, il est désormais nécessaire que vous apportez une mise à jour au fichier package.appxmanifest.xml afin de tester votre titre UWP correctement dans Visual Studio. Pour ce faire :
>
> 1. Rechercher de l’Explorateur de solutions pour le fichier package.appxmanifest.xml
> 2. Cliquez avec le bouton droit sur le fichier et choisissez Afficher le Code.  
    Si l’option Afficher le Code n’est pas disponible ou le fichier package.appxmanifest ne possède pas d’extension. Vous devez ouvrir le fichier en tant que xml et continuer avec les étapes restantes.
> 3. Sous le `<Properties></Properties>` , ajoutez la ligne suivante : `<uap:SupportedUsers>multiple</uap:SupportedUsers>`.
> 4. Déployer le jeu sur votre Xbox en démarrant une build de débogage à distance à partir de Visual Studio. Vous pouvez trouver des instructions pour configurer votre titre sur une console Xbox dans le [configurer votre UWP sur un environnement de développement Xbox](../../xbox-apps/development-environment-setup.md) article.
>
> L’élément de configuration a changé peut vous sembler multijoueurs activé mais il est toujours nécessaire exécuter votre jeu dans les scénarios de lecteur unique.

## <a name="try-out-the-examples"></a>Essayer les exemples

Vous êtes prêt à commencer à utiliser Xbox Live dans votre projet Unity ! Essayez d’ouvrir l’arrière-plan dans le **Xbox Live/Examples** dossier pour afficher le plug-in en action, ainsi que des exemples montrant comment utiliser la fonctionnalité vous-même. Les exemples en cours d’exécution dans l’éditeur vous donnera des données fictives, mais si vous générez le projet dans Visual Studio et [associer votre compte Xbox Live avec le bac à sable](authorize-xbox-live-accounts.md), vous pouvez connecter avec votre identité.

Essayez le **SignInAndProfile** scène pour vous connecter à votre Account Microsoft, le **classement** scène pour la création d’un classement et le **UpdateStat** scène pour affichage et mise à jour de statistiques.

## <a name="see-also"></a>Voir également

* [Connectez-vous à Xbox Live dans Unity](unity-prefabs-and-sign-in.md)
* [Autoriser des comptes de Xbox Live](authorize-xbox-live-accounts.md)
