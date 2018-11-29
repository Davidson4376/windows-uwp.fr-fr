---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Découvrez comment installer le kit SDK Microsoft Advertising.
title: Installer le SDK MicrosoftAdvertising
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, pub, publicités, installer, SDK, bibliothèque de publicités
ms.localizationpriority: medium
ms.openlocfilehash: 2066d055f7abf0e9a34e245d9c6a95e14596d362
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7983202"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Installer le SDK MicrosoftAdvertising

Pour afficher des publicités dans vos applications UWP pour Windows10, installez le [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp). Ce kit de développement logiciel(SDK) est une extension de VisualStudio2015 et des versions ultérieures.

> [!NOTE]
> Si vous développez une application UWP JavaScript/HTML et que vous avez installé le SDK Windows 10 version 10.0.14393 (mise à jour anniversaire) ou une version ultérieure, vous devez également installer la bibliothèque [WinJS](https://github.com/winjs/winjs) . Cette bibliothèque était incluse dans les versions précédentes du Kit de développement logiciel pour Windows10, mais depuis le SDK Windows10 version10.0.14393 (Mise à jour anniversaire), elle doit être installée séparément.

<span id="install-msi" />

## <a name="install-via-msi"></a>Installer via MSI

Pour installer le SDK Microsoft Advertising via le programme d’installationMSI:

1.  Fermez toutes les instances de Visual Studio.

2. Si vous aviez précédemment installé une version antérieure du Kit de développement MicrosoftAdvertising, du kit MicrosoftUniversalAdClient, de l’extensionAdMediator ou du SDK d’engagement et de monétisation de la Boutique Microsoft, désinstallez ces versions maintenant. Si vous le souhaitez, ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les anciennes versions de SDK Microsoft Advertising qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Téléchargez et installez le [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp). L’installation peut prendre quelques minutes. Attendez la fin du processus.

4.  Redémarrez VisualStudio.

5.  Si vous possédez un projet existant qui référence des bibliothèques de publicités d’une version antérieure du SDK Microsoft Advertising, du SDK Universal Ad Client ou du SDK Microsoft Store Engagement and Monetization, nous vous recommandons d’ouvrir votre projet dans VisualStudio, puis de le nettoyer et le régénérer (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet et sélectionnez **Nettoyer**. Ensuite, cliquez de nouveau avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK Microsoft Advertising pour la première fois dans votre projet, vous pouvez désormais [ajouter une référence au SDK Microsoft Advertising](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Installer via NuGet

Pour installer le SDKMicrosoft Advertising dans un projet UWP spécifique via NuGet:

1.  Fermez toutes les instances de Visual Studio.

2.  Si vous aviez précédemment installé une version antérieure du Kit de développement MicrosoftAdvertising, du kit MicrosoftUniversalAdClient, de l’extensionAdMediator ou du SDK d’engagement et de monétisation de la Boutique Microsoft, désinstallez ces versions maintenant. Si vous le souhaitez, ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les anciennes versions de SDK Microsoft Advertising qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Démarrez VisualStudio, puis ouvrez le projet dans lequel vous souhaitez utiliser le SDK Microsoft Advertising.
    > [!NOTE]
    > Si votre projet inclut déjà des références de bibliothèque d’une installation MSI antérieure du SDK, supprimez ces références de votre projet. Des icônes s’afficheront en regard de ces références, car les bibliothèques auxquelles elles sont associées ont été supprimées au cours des étapes précédentes.

4. Dans VisualStudio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.

5. Dans la zone de recherche, tapez **Microsoft.Advertising.XAML** (pour un projet XAML) ou **Microsoft.Advertising.JS** (pour un projet JavaScript/HTML) et installez le package correspondant. Lorsque l’installation du package est terminée, enregistrez votre solution.
    > [!NOTE]
    > Si la fenêtre **Sortie** signale une erreur *Install-Package* qui fait état d’une longueur trop importante du chemin spécifié, il vous faudra éventuellement configurer NuGet pour l’extraction des packages vers un autre emplacement présentant un chemin plus court que l’emplacement par défaut. Pour ce faire, ajoutez la valeur ```repositoryPath``` à un fichier nuget.config sur votre ordinateur, puis affectez-la à un chemin court de dossier, dans lequel extraire les packages. Pour plus d’informations, consultez [cet article](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) de la documentation NuGet. Sinon, vous pouvez essayer de déplacer votre projetVisual Studio vers un dossier différent présentant un chemin plus court.

6. Fermez votre solution, puis rouvrez-la.

7.  Si votre projet référence déjà des bibliothèques d’une version antérieure du SDK Microsoft Advertising installées via NuGet et que vous avez mis à jour votre projet vers une version plus récente du SDK, nous vous recommandons de nettoyer et de régénérer votre projet (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Nettoyer**, cliquez de nouveau avec le bouton droit sur le nœud de projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter une référence au SDK Microsoft Advertising](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Ajouter une référence au SDK Microsoft Advertising

Après avoir installé le SDK Microsoft Advertising, suivez ces instructions pour référencer le SDK dans votre projet afin de pouvoir utiliser les API publicitaires.

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence au SDK Microsoft Advertising dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

2. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

3. Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis activez la case à cocher **SDK Microsoft Advertising pour XAML** (pour les applications XAML) ou **SDK Microsoft Advertising pour JavaScript** (pour les applications créées à l’aide de JavaScript et HTML).

4.  Dans **Gestionnaire de références**, cliquez sur OK.

Vous trouverez des procédures pas à pas montrant comment commencer à utiliser les API publicitaires dans les articles suivants:

* [Spots](interstitial-ads.md)
* [Publicités natives](native-ads.md)
* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML5 et JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Comprendre les packages d’infrastructure du SDK Microsoft Advertising

La bibliothèque Microsoft.Advertising.dll du [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp) (pour applications UWP) est configurée comme un *package d’infrastructure*. Cette bibliothèque contient les API publicitaires des espaces de noms [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) et [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui).

Cette bibliothèque étant un package d’infrastructure, cela signifie que dès qu’un utilisateur installe une version de votre application qui l’utilise, elle sera automatiquement mise à jour sur l’appareil via WindowsUpdate lors de la publication d’une nouvelle version de la bibliothèque intégrant des correctifs et améliorant ses performances. Ainsi, vos clients sont toujours assurés de disposer de la dernière version disponible de la bibliothèque sur leurs appareils.

Si nous publions une nouvelle version du SDK qui introduit de nouvelles API ou fonctionnalités dans cette bibliothèque, vous devrez installer la dernière version du SDK pour bénéficier de ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le WindowsStore.
