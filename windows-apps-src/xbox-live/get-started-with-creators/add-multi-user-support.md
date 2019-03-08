---
title: Ajouter la prise en charge multi-utilisateur pour votre jeu Unity
description: Ajouter la prise en charge multi-utilisateur pour votre jeu Unity à l’aide du plug-in Xbox Live Unity
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity, multi-utilisateurs
ms.localizationpriority: medium
ms.openlocfilehash: 483a0e966be756de483bf7e2ab8458647397687b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613734"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>Ajouter la prise en charge multi-utilisateur pour votre jeu Unity
> [!IMPORTANT]
> Le plug-in de Xbox Live Unity ne prend pas en charge les primes ou mode multijoueur en ligne et qu’il est recommandé uniquement pour [programme Xbox Live Creators](../developer-program-overview.md) membres.

Prise en charge multi-utilisateur est maintenant pris en charge par le Xbox Live Unity plug-in pour les scénarios qui impliquent plusieurs lecteurs locaux. Chaque lecteur peut utiliser son propre compte Xbox Live pour les statistiques et de connexion. Votre jeu peut également activer des invités pour les utilisateurs qui n’ont pas de comptes Xbox Live à lire. Cette fonctionnalité est uniquement pris en charge sur les consoles Xbox.

Ce didacticiel vous guidera tout au long de l’ajout de prise en charge multi-utilisateur à l’autre Xbox Live Prefabs.

> [!IMPORTANT]
> Scénarios d’utilisateurs multiples sont uniquement pris en charge sur les consoles Xbox. Cette fonctionnalité ne fonctionne pas sur les PC.

## <a name="prerequisites"></a>Conditions préalables
Vous devez avoir suivi le [mise en route](configure-xbox-live-in-unity.md) didacticiel pour activer et configurer la Xbox Live.

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>Ajout et la signature dans plusieurs utilisateurs Xbox Live

1. À partir de la **actifs** > **Xbox Live** > **Prefabs** dossier, faire glisser sur votre scène autant **XboxLiveUser** prefab instances comme il y aura des joueurs. Pour ce didacticiel, il y aura deux joueurs donc deux **XboxLiveUser** instances seront ajoutés à la scène. Nous allons y faire référence en tant que **XboxLiveUser** et **XboxLiveUser2** pour des raisons pratiques.

2. Pour ajouter des utilisateurs et les signer dans correctement, ajoutez deux instances de la **UserProfile** prefab à la scène, une pour chaque **XboxLiveUser**. Vérifiez que vous disposez d’une instance de la `XboxLiveServices` prefab sur la scène. En outre, assurez-vous de déplacer les deux **UserProfile** objets sur la scène pour distinguer les uns des autres. Étant donné que ces prefabs utilisent le Eventsystem Unity, vérifiez que vous disposez d’une instance de la `EventSystem` sur la scène.

    ![Hiérarchie de prise en charge multi-utilisateur dans Xbox Live projet du didacticiel de plug-in Unity](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Jeu scène de prise en charge multi-utilisateur dans le projet de didacticiel de plug-in Xbox Live Unity](../images/unity/MUA-Tutorial-GameScene.png)

3. Assignez une instance de la **XboxLiveUser** prefabs sur la scène à chacun de la **UserProfile** objets.

    ![Préfabriqué UserProfile pour la prise en charge multi-utilisateur](../images/unity/user-profile-for-mua.png)

4. Étant donné que les applications multi-utilisateurs sont uniquement pris en charge sur les appareils Xbox One, ajout de prise en charge de contrôleur pour le **UserProfile** objects est requis. Sur chaque **UserProfile** de l’objet, il existe un champ appelé `InputControllerButton` où vous pouvez spécifier la manette de jeu et bouton numéros chacun **UserProfile** doit écouter.

Pour ce didacticiel, nous allons utiliser `joystick 1 button 0` pour le **UserProfile** affecté au lecteur 1 et `joystick 2 button 0` pour le lecteur 2 et le second **UserProfile** objet de jeu. Cela permet d’affecter le `A` bouton permettant d’interagir avec **UserProfile** pour les deux contrôleurs.

> [!Note]
> Pour plus d’informations sur la prise en charge du contrôleur Xbox du plug-in Xbox Live Unity, consultez : [Ajouter le Support de contrôleur à Prefabs Live Xbox](add-controller-support-to-xbox-live-prefabs.md)

5. Exécuter la scène dans l’éditeur et positionnement s’exécutent sur chacun des boutons pour vérifier que tout est correctement configuré.

    ![Prise en charge multi-utilisateur test dans l’éditeur Unity](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>Génération et test de la plateforme Windows universelle

1. Une fois suivant les étapes décrites à la [développer des titres de créateurs avec Unity](configure-xbox-live-in-unity.md) didacticiel, ouvrez la solution UWP exportée dans Visual Studio.

2. Sous le projet UWP de votre jeu, un clic droit sur le `package.appxmanifest.xml` de fichier et choisissez **afficher le Code**.

3. Sous le `<Properties></Properties>` section, ajoutez le code suivant, ce qui permet l’entrée de plusieurs utilisateur pour l’application : `<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. Pour tester sur Xbox, suivez la documentation relative à la [configurer votre UWP sur un environnement de développement Xbox](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) didacticiel.

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>À l’aide de l’autre Xbox Live Prefabs avec plusieurs utilisateurs

Dans le **exemples** dossier le plug-in, il existe un **MultiUserExample** scène qui montre comment les prefabs différents peuvent utiliser le **XboxLiveUser** instances pour afficher spécifique informations pour chaque utilisateur.
