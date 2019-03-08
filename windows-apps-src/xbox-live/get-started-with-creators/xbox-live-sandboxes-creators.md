---
title: Les bacs à sable Xbox Live
description: Présentation des bacs à sable Xbox Live
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590224"
---
# <a name="xbox-live-sandboxes-introduction"></a>Présentation des bacs à sable Xbox Live

Dans le [configuration du service Xbox Live](xbox-live-service-configuration-creators.md) article, il a été expliqué que vous devez configurer les informations sur le titre dans [partenaires](https://partner.microsoft.com/dashboard). Ces informations incluent des éléments tels que les statistiques, tableaux de résultats, localisation et bien plus encore. Modifications apportées à votre configuration de service Xbox Live doivent être publiés à partir du centre de partenaires dans votre bac à sable de développement avant que les modifications sont récupérées par le reste de la Xbox Live et sont accessibles dans votre titre.

Un bac à sable de développement vous permet de travailler sur les modifications apportées à votre titre dans un environnement isolé. Les bacs à sable offrent plusieurs avantages :

1. Vous pouvez itérer sur les modifications apportées à une mise à jour pour votre titre sans affecter la version en production.
2. Pour des raisons de sécurité, certains outils fonctionnent uniquement dans un bac à sable de développement.
3. Autres éditeurs ne peuvent pas voir ce que vous travaillez sur sans avoir accès à votre sandbox.

Par défaut, les consoles Xbox One et PC Windows 10 sont dans le bac à sable de vente au détail. Vous devez passer votre Xbox One et/ou des PC pour le bac à sable de développement pour accéder à cette version de la configuration du service Xbox Live. Il est important de se rappeler pour l’appareil rétablir le bac à sable de vente au détail si vous avez besoin tester quelque chose dans la vente au détail ou faites une pause pour lire votre favori Xbox Live game.

## <a name="finding-out-about-your-sandbox"></a>S’informer sur votre bac à sable

Un bac à sable est créé pour vous lorsque vous créez un titre. Vous pouvez trouver votre ID de bac à sable en ouvrant votre produit dans **partenaires** et en accédant à **Services** > **Xbox Live**. Le **ID de bac à sable** apparaît en haut de la page.

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>Basculer la sandbox de développement de votre PC
Vous pouvez basculer votre PC dans le bac à sable de développement à l’aide de Unity, Windows Device Portal (WPD) ou par le biais de ligne de commande.

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>Conditions préalables
Les conditions suivantes doivent être effectuées avant que vous pouvez basculer vers et depuis le bac à sable de développement dans Unity :

1. [Configurer Xbox Live dans Unity](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>Les bacs à sable de commutateur
Intégré dans la Configuration de Xbox Live fenêtre vous permet de basculer entre votre développement et les bacs à sable de la vente au détail facilement. Pour commencer, accédez à **Xbox Live** > **Configuration** dans le menu. Vous pouvez voir le bac à sable en cours dans le **Configuration du Mode développeur** section.

1. Si **Mode développeur** indique **activé**, puis vous êtes actuellement dans le bac à sable de développement associé à votre jeu. Vous pouvez cliquer sur le **revenir au Mode de vente au détail** bouton pour l’extraction.
2. Si **Mode développeur** indique **désactivé**, puis vous êtes actuellement dans le bac à sable de vente au détail. Vous pouvez cliquer sur le **basculer vers le Mode développeur** bouton à insérer.

![XBL activé](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>Conditions préalables
Les conditions suivantes doivent être effectuées avant que vous basculez votre bac à sable dans Windows Device Portal (WPD) :

1. [Le programme d’installation portail des appareils sur le bureau de Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>Les bacs à sable de commutateur

1. Ouvrez **portail de développement Windows** en vous connectant à ce dernier dans votre navigateur web, comme décrit dans la [portail des appareils le programme d’installation sur le bureau de Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) article.
2. Cliquez sur **Xbox Live**.
3. Entrez votre sandbox de développement dans le champ de texte et cliquez sur **modifier**.

![](../images/getting_started/wdp_switch_sandbox.png)

Pour revenir à la vente au détail, vous pouvez entrer ici vente au détail.

### <a name="command-line"></a>Ligne de commande

#### <a name="prerequisites"></a>Conditions préalables
Les conditions suivantes doivent être effectuées avant que vous pouvez basculer vers et depuis le bac à sable de développement par le biais de ligne de commande :

1. Télécharger le Package d’outils Xbox Live à [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) et décompressez-le.

#### <a name="switch-sandboxes"></a>Les bacs à sable de commutateur
1. Exécutez le fichier de commandes SwitchSandbox.cmd **mode administrateur**.

Exécutez cette procédure en mode administrateur pour basculer votre bac à sable. Le premier argument est le bac à sable. Par exemple, si vous essayez de basculer vers le bac à sable MJJSQH.58, vous utiliseriez cette commande :

```cmd
SwitchSandbox.cmd MJJSQH.58
```

Pour revenir à la vente au détail, vous qui fournissez simplement comme deuxième argument.

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Basculer votre sandbox de développement de console Xbox One

### <a name="using-xbox-dev-portal"></a>À l’aide du portail de développement de la Xbox

Vous pouvez utiliser le portail de développement Xbox pour modifier le bac à sable sur votre console. Pour ce faire, accédez à [Dev accueil](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) sur votre console et [activer le portail de l’appareil](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox). Une fois que vous avez le portail de développement Xbox ouvrir :

2. Cliquez sur **Xbox Live**.
3. Entrez votre sandbox de développement dans le champ de texte et chiches **modifier**.

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>À l’aide de l’interface utilisateur de console Xbox One

Vous pouvez utiliser [Dev accueil](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) pour modifier le bac à sable sur votre console directement :

1. Cliquez sur **bac à sable modification**, qui se trouve sous **Actions rapides**.
2. Entrez l’ID de bac à sable et puis cliquez sur **enregistrer et de redémarrer**.

### <a name="sign-in-with-the-xbox-app"></a>Connectez-vous à l’aide de l’application Xbox

Une fois que vous avez commencé votre PC de développement pour utiliser le bac à sable approprié pour votre titre que vous souhaitez vérifier que vous êtes connecté à Xbox Live avec un compte de test éligibles. Cela est possible en vous connectant à la [Xbox Live application](https://www.xbox.com/en-US/xbox-app). Une fois que votre environnement de développement démarre à l’aide de l’application Xbox bac à sable souhaitée traitera connexion, les utilisateurs à l’aide des mêmes contraintes comme n’importe quel autre Xbox Live de service en cours d’exécution sur le bac à sable. Cela est utile pour vérifier que vous utilisez un compte valide pour le bac à sable.
