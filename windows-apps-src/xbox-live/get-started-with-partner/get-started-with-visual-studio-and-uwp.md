---
title: Bien démarrer avec Visual Studio pour les jeux UWP
description: Découvrez comment configurer un projet Visual Studio pour activer la Xbox Live pour un jeu UWP
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 314d5e937bb8680dc26b7dfdfa15ff90f8f26c81
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651404"
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>Prise en main à l’aide de Visual Studio pour les jeux UWP

## <a name="requirements"></a>Configuration requise

1. L’inscription dans le  **[programme pour développeurs partenaires](https://developer.microsoft.com/store/register)**.
2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**  avec la **outils de développement d’application Windows universelle**. La version minimale requise pour les applications UWP Visual Studio 2015 Update 3. Nous vous recommandons d’utiliser la dernière version de Visual Studio pour les mises à jour de sécurité et de développement. 
4. **[SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** ou version ultérieure.

> [!IMPORTANT]
> Visual Studio 2017 est requis si vous utilisez le SDK Windows 10 version 10.0.15063.0 (également appelée Creators Update) ou version ultérieure.

## <a name="create-a-new-product-in-partner-center"></a>Créer un nouveau produit dans l’espace partenaires

Chaque titre de Xbox Live doit avoir un produit créé dans [partenaires](https://partner.microsoft.com/dashboard) avant que vous serez en mesure de se connecter et effectuer des appels de Service Xbox Live. Consultez [créer un titre sur UDC](create-a-new-title.md) pour plus d’informations.

## <a name="configuring-your-development-device"></a>Configuration de votre appareil de développement

Les étapes de configuration préliminaires suivantes sont nécessaires sur votre appareil, afin que vous puissiez correctement la connexion avec Xbox Live et appelez les divers Services Xbox Live.

### <a name="set-your-sandbox"></a>Définir votre bac à sable

Les bacs à sable offrent un moyen de conserver votre [Configuration du Service Xbox Live](../xbox-live-service-configuration.md) isolé à partir de la vente au détail jusqu'à ce que vous êtes prêt à publier votre titre. Certaines données que vous collectez sont spécifiques à un bac à sable. Par exemple que votre titre définit un stat appelé *Headshots*, et vous accumuler un certain nombre de Headshots dans un compte d’utilisateur lors du test de votre titre. Cette valeur doit être spécifique à un bac à sable de développement de votre titre, et si vous avez basculé de lecture de la version commerciale de votre titre, les headshots ne seraient pas reportées.

Consultez le [bacs à sable de Xbox Live](../xbox-live-sandboxes.md) article pour en savoir plus et découvrir comment définir votre bac à sable.

### <a name="sign-in-with-a-test-account"></a>Connectez-vous avec un compte de test

Pour vous connecter à votre sandbox de développement, vous devez créer un compte de test, ou configurer un compte Microsoft régulière (MSA) pour l’accès à votre sandbox. Cela fournit une sécurité améliorée pour vos titres de développement, ainsi que certains autres avantages.

Pour en savoir plus sur les comptes de test et comment en créer un, consultez [Xbox Live Test Accounts](../xbox-live-test-accounts.md)

## <a name="visual-studio-project-setup"></a>Configuration de projet Visual Studio

### <a name="1-open-a-uwp-project"></a>1. Ouvrez un projet UWP
Si vous n’avez pas déjà un projet UWP existant, vous pouvez créer un en procédant comme suit :

1. Dans Visual Studio, **fichier** > **nouveau** > **projet**.
2. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C#**   >  **Windows** > **universelle** nœud dans le volet gauche, cliquez sur **application vide (Windows universel)** dans le volet droit.
3. Dans la partie inférieure de la boîte de dialogue, donnez un nom au projet et spécifiez l’emplacement du projet.
4. Spécifier la Version cible et la Version minimale de Windows 10 SDK. Consultez [choisir une version UWP](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) pour plus d’informations.

![créer un projet dans Visual Studio](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> Xbox Live API (XSAPI) nécessite une version minimale 10.0.10586.0 ou une version ultérieure.

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. Ajouter des références à l’API Xbox Live (XSAPI) dans votre projet

L’API de Services Xbox est fourni dans les versions pour UWP et XDK et pour C++ et WinRT et avait leur espace de noms sont structurées sous la forme **Microsoft.Xbox.Live.SDK.*. UWP** et **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** est destiné aux développeurs qui créent un jeu UWP, ce qui peut s’exécuter sur des PC, Xbox One ou Windows Phone.
2. **XboxOneXDK** concerne ID@Xbox et géré les développeurs qui utilisent le XDK une Xbox.
3. Le SDK de C++ peut être utilisé pour les moteurs de jeux de C++, alors que le SDK WinRT est pour les moteurs de jeux écrits avec C++, C#, ou JavaScript.
4. Lorsque vous utilisez WinRT avec un moteur de C++, vous devez utiliser C++ / c++ / CX qui utilise des rôles (^). C++ est l’API recommandée à utiliser pour les moteurs de jeux de C++.  

> [!TIP]
> Vous trouverez plus d’informations sur l’exécution UWP sur Xbox One à [mise en route du développement d’applications UWP sur Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Pour utiliser l’API de Xbox Live à partir de votre projet, vous pouvez soit ajouter des références aux fichiers binaires à l’aide de packages NuGet ou ajout de la source de l’API. L'ajout de packages NuGet rend la compilation plus rapide, alors que l’ajout de la source facilite le débogage. Cet article vous guide lors de l’utilisation de packages NuGet. Si vous souhaitez utiliser la source, puis consultez [compilation le Xbox Live API Source dans votre projet UWP](add-xbox-live-apis-source-to-a-uwp-project.md). Vous pouvez ajouter le package NuGet de kit de développement logiciel Xbox Live par :

1. Dans Visual Studio, accédez à **outils** > **Gestionnaire de Package NuGet** > **gérer les Packages NuGet pour la Solution...** .
2. Dans le Gestionnaire de package NuGet, cliquez sur **Parcourir** et entrez **Xbox.Live.SDK** dans la zone de recherche.
3. Sélectionnez la version de Xbox Live SDK que vous souhaitez utiliser dans la liste sur la gauche. Dans ce cas, nous allons utiliser le package Microsoft.Xbox.Live.SDK.WinRT.UWP.
3. Sur le côté droit de la fenêtre, cochez la case en regard de votre projet et cliquez sur **installer**.

![Ajouter XBL via NuGet](../images/getting_started/vs-add-nuget-xbl.gif)


> [!IMPORTANT]
> Pour `Microsoft.Xbox.Live.SDK.Cpp.*` projets, assurez-vous d’inclure l’en-tête `#include <xsapi\services.h>` dans la source de votre projet.

### <a name="3-optional-using-connected-storage-andor-secure-sockets"></a>3. (Facultatif) À l’aide de Secure Sockets et/ou de stockage connectés
Selon la version du SDK Windows que vous utilisez, vous devrez peut-être installer du contenu supplémentaire ou ajouter manuellement des références à votre projet pour pouvoir utiliser Xbox Live [stockage connecté](../storage-platform/connected-storage/connected-storage-technical-overview.md) ou sécuriser des Sockets. Si vous souhaitez utiliser la fonctionnalité de stockage connecté, vous devrez accéder à la `Windows.Gaming.XboxLive.Storage` espace de noms. Si vous souhaitez utiliser des Sockets sécurisé, vous devrez accéder `Windows.Networking.XboxLive`.

#### <a name="windows-10-sdk-version-10016299-or-higher"></a>SDK Windows 10 version 10.0.16299 ou ultérieure
Si vous avez ciblés Windows 10 SDK 10.0.16299 ou une version ultérieure, puis vous serez en mesure d’accéder à l’espace de noms de stockage connecté sans effectuer aucun travail supplémentaire. Pour accéder à des Sockets sécurisés, vous devez ajouter une référence à **Extensions de bureau Windows pour UWP**. Vous pouvez faire cela :

1. Dans le **l’Explorateur de solutions**, cliquez avec le bouton droit sur le **références** nœud afin de sélectionner les **ajouter une référence...**
2. Sur le côté gauche de la **Gestionnaire de références** boîte de dialogue, sélectionnez **Windows universel** > **Extensions**.
3. Dans la liste qui s’affiche, recherchez **Extensions de bureau Windows pour UWP** et sélectionnez la case à cocher en regard de la version qui correspond à votre Kit de développement logiciel de Windows 10.
4. Cliquez sur **OK**.

![Ajouter une nouvelle référence dans Visual Studio](../images/getting_started/get-started-vs-add-ref.png)

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Version du SDK Windows 10 10.0.15063 ou un niveau inférieur
Si vous souhaitez utiliser le stockage connecté ou Secure Sockets, vous devrez installer le kit SDK Extensions plateformes Xbox Live avant que vous pouvez ajouter des références à votre projet. Vous pouvez faire cela :

1. Téléchargez et extrayez le [Xbox Live Platform Extensions SDK](https://aka.ms/xblextsdk).
2. Une fois extraites, exécutez le fichier MSI inclus qui correspond à la version du SDK Windows 10 que vous utilisez.

Une fois que vous avez installé le kit SDK Extensions Platform Xbox Live, vous devez ajouter une référence à celle-ci dans Visual Studio. Vous pouvez faire cela :

1. Dans le **l’Explorateur de solutions**, cliquez avec le bouton droit sur le **références** nœud afin de sélectionner les **ajouter une référence...**
2. Sur le côté gauche de la **Gestionnaire de références** boîte de dialogue, sélectionnez **Windows universel** > **Extensions**.
3. Dans la liste qui s’affiche, recherchez **Extensions de bureau Windows pour UWP** et sélectionnez la case à cocher en regard de la version qui correspond à votre Kit de développement logiciel de Windows 10.
4. Cliquez sur **OK**.

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. Associer votre projet Visual Studio à votre application UWP

Pour votre jeu être en mesure d’ouverture de session, il doit être associé au produit que vous avez créé dans l’espace partenaires. Vous pouvez associer votre jeu dans Visual Studio à l’aide de l’Assistant Association de Store. Dans Visual Studio, procédez comme suit :

1.  Cliquez avec le bouton droit sur le projet principal (le projet de démarrage), cliquez sur **Store** > **associer l’application avec le Store...**
2.  Connectez-vous avec le **compte de développeur de Windows** utilisé pour la création de l’application si demandé et suivez les invites.

> [!TIP]
> Consultez [empaquetage d’applications](https://docs.microsoft.com/windows/uwp/packaging/) pour plus d’informations sur la préparation de votre jeu pour Windows Store.

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. Ajouter des capacités Internet à votre projet Visual Studio
Votre projet UWP devez spécifier les capacités d’internet pour communiquer avec Xbox Live. Vous pouvez définir ces propriétés :

1. Double-cliquez sur le **package.appxmanifest** fichier dans Visual Studio pour ouvrir le **Concepteur de manifeste**.
2. Cliquez sur le **fonctionnalités** et vérifiez **Internet (Client)**.

![Ajouter une nouvelle référence dans Visual Studio](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. Associer votre projet Visual Studio avec le titre de la Xbox Live est activée

Pour communiquer avec le service Xbox Live, vous devez ajouter un fichier de configuration de service à votre projet. Cela est possible facilement :

1. Dans votre projet de démarrage, avec le bouton droit sur et sélectionnez **ajouter** > **un nouvel élément**.
2. Sélectionnez le **fichier texte** tapez et nommez-le **xboxservices.config**.
3. Cliquez avec le bouton droit sur le fichier, sélectionnez **propriétés** et vérifiez que :
    1. **Action de génération** a la valeur **contenu**, et  
    2. **Copier dans le répertoire de sortie de** a la valeur **toujours copier**.
5.  Modifiez le fichier de configuration avec le modèle suivant, en remplaçant le **n ° titre** et **PrimaryServiceConfigId** avec les valeurs applicables à votre titre. Vous pouvez obtenir les valeurs correctes à partir de la page Xbox Live racine dans l’espace partenaires. Le **PrimaryServiceConfigId** apparaît dans le centre de partenaires en tant que **SCID**.

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID"
    }
```

Exemple :

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

> [!TIP]
> Toutes les valeurs à l’intérieur de xboxservices.config respectent la casse. Consultez [Configuration du Service](../xbox-live-service-configuration.md) pour plus d’informations sur l’obtention des n ° titre et PrimaryServiceConfigId.

### <a name="7-optional-add-multiplayer-capabilities"></a>7. (Facultatif) Ajouter des fonctionnalités multijoueurs

Si vous prévoyez d’ajouter multijoueurs prennent en charge pour votre titre et souhaitez implémenter la possibilité pour les joueurs à inviter d’autres utilisateurs pour un jeu multijoueur, puis vous devez ajouter un autre champ à votre fichier AppXManifest. Consultez [configurer votre AppXManifest pour mode multijoueur](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md) pour plus d’informations.

## <a name="learn-more"></a>En savoir plus

Le [exemples Xbox Live SDK](https://github.com/Microsoft/xbox-live-samples) constituent un bon moyen pour voir comment les API de Xbox Live sont utilisés et présenter les API qui permettent aux développeurs dans le ID@Xbox programme. Pour utiliser les exemples, vous devrez modifier votre bac à sable à XDKS.1.
