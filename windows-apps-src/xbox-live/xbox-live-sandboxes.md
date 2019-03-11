---
title: Les bacs à sable Xbox Live
description: Découvrez les bacs à sable pour le développement Xbox Live.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599474"
---
# <a name="xbox-live-sandboxes-intro"></a>Xbox Live bacs à sable Intro

Dans [Configuration du Service Xbox Live](xbox-live-service-configuration.md), il a été expliqué que vous devez configurer les informations sur le titre en ligne, généralement en [partenaires](https://partner.microsoft.com/dashboard).  Ces informations comprennent des choses telles que les classements que votre titre souhaite afficher des primes permettant de déverrouiller des lecteurs, configuration de matchmaking, etc.

Lorsque vous apportez des modifications à votre configuration de service, ceux-ci doivent être publiés à partir du centre de partenaires avant que les modifications sont récupérées par le reste de la Xbox Live et peuvent être consultées par votre titre.

Vous publiez sur ce que l'on appelle un bac à sable de développement.  Ceux-ci vous autorisent à travailler sur les modifications apportées à votre titre dans un environnement isolé.  Offre plusieurs avantages décrits dans la section ci-dessous.

Par défaut, les Consoles une Xbox et PC Windows 10 sont dans le bac à sable de vente au détail.

## <a name="benefits"></a>Avantages

Les bacs à sable de développement offrent quelques avantages :


1. Vous pouvez itérer sur les modifications apportées à une mise à jour pour votre titre sans affecter la version actuellement disponible.
2. Certains outils fonctionnent uniquement dans un bac à sable de développement pour des raisons de sécurité.
3. Certains développeurs de votre équipe voudront aux modifications de configuration de service « branche » et de test sans affecter votre configuration de service de développement principal.
4. Autres éditeurs ne peuvent pas voir ce que vous travaillez sur sans avoir accès à votre sandbox.

Vous pouvez également **éventuellement** créer des comptes de test.  Vous pouvez les utiliser si vous ne souhaitez pas utiliser votre compte Xbox Live classique pour tester votre titre, ou si vous avez besoin de plusieurs comptes pour tester des scénarios tels que des interactions sociales (par exemple : afficher les statistiques d’un ami) ou multijoueurs.

Comptes de test peuvent uniquement connectez-vous aux bacs à sable de développement et seront expliqués dans la section ci-dessous.

## <a name="finding-out-about-your-sandbox"></a>S’informer sur votre bac à sable

La grande majorité des développeurs ne besoin qu’un bac à sable.  Fort heureusement un bac à sable est créé pour vous lorsque vous créez un titre.

1. Vous découvrez votre bac à sable en accédant aux partenaires ici : ![](images/getting_started/first_xbltitle_dashboard.png)

1. Cliquez ensuite sur votre titre : ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. Enfin, cliquez sur Services -> Xbox Live dans le menu de gauche ![](images/getting_started/first_xbltitle_leftnav.png)

1. Vous pouvez maintenant voir votre bac à sable répertorié comme suit ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>Comment votre bac à sable a un impact sur votre flux de travail

En général, vous travaillez avec les bacs à sable comme suit :

1. (Une fois) Basculer votre PC ou votre Xbox One à votre sandbox de développement.
2. (Plusieurs fois) Lorsque vous apportez des modifications à votre configuration de service, vous allez publier les modifications apportées à votre sandbox de développement.  Modifications de configuration de service sont des éléments tels que la définition des primes, en ajoutant des tableaux de résultats ou modification d’un modèle de Session multijoueurs.
3. (Plusieurs fois) Si vous travaillez avec d’autres membres d’équipe, vous pouvez leur donner accès à votre sandbox
4. (Une fois) Si vous avez besoin tester quelque chose dans la vente au détail, ou faites une pause pour lire votre jeu Xbox préféré, vous devrez basculer votre bac à sable vers une version commerciale.

Ces scénarios sont décrites plus en détail ci-dessous.  Le processus présente des différences sur les PC et consoles, il y a des sections distinctes pour chacune.

## <a name="switch-your-pcs-development-sandbox"></a>Basculer la sandbox de développement de votre PC

Si vous souhaitez basculer la sandbox de développement de votre PC, la méthode recommandée pour ce faire, est à l’aide de Windows Device Portal (WDP).  Vous pouvez également le faire via la ligne de commande.  Nous allons décrire les deux sens.

### <a name="windows-device-portal"></a>Windows Device Portal

Si vous n’avez pas encore activé WDP sur votre PC, suivez ces instructions pour le faire. [Le programme d’installation portail des appareils sur le bureau de Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

Une fois que vous l’avez fait, ouvrez le portail de développement de Windows en vous connectant à ce dernier dans votre navigateur web, comme décrit dans l’article ci-dessus.

Vous cliquez ensuite sur « Xbox Live » pour accéder la section appropriée, comme indiqué ci-dessous.

![](images/getting_started/wdp_switch_sandbox.png)

Vous pouvez saisir votre sandbox que vous avez obtenu via les étapes décrites dans le *recherche Out votre Sandbox* et cliquez sur « Modifier ».

Pour revenir à la vente au détail, vous pouvez entrer ici vente au détail.

### <a name="powershell-module"></a>Module PowerShell

[Xbox Live PowerShell Module](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule contient divers utilitaires pour aider à y compris la modification des bacs à sable sur PC ou console de développement Xbox Live.

* Pour l’utiliser à partir de [PowerShell Gallery](https://www.powershellgallery.com/packages/XboxlivePSModule), ouvrez une fenêtre PowerShell :
    1. Téléchargez et installez le module : `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. Commencer à utiliser en exécutant `Import-Module XboxlivePSModule`
    3. Exécuter les applets de commande, par exemple, Set-XblSandbox XDKS.1 ou Get-XblSandbox

* Pour l’utiliser à partir d’un fichier zip à [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools), ouvrez une fenêtre PowerShell,
    1. Exécutez `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. Exécuter les applets de commande, par exemple, Set-XblSandbox XDKS.1 ou Get-XblSandbox

### <a name="command-prompt-script"></a>Script de l’invite de commandes

Télécharger le Package d’outils Xbox Live à [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) et décompressez-le.  Vous trouverez un fichier de commandes SwitchSandbox.cmd dans.

Exécutez cette procédure en mode administrateur pour basculer votre bac à sable.  Le premier argument est le bac à sable.  Par exemple si vous essayez de basculer vers le bac à sable XDKS.1, vous le feriez :

```
SwitchSandbox.cmd XDKS.1
```

Pour revenir à la vente au détail, vous qui fournissez simplement comme deuxième argument.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Basculer votre sandbox de développement de console Xbox One

### <a name="using-windows-dev-portal"></a>À l’aide du portail de développement Windows

Vous pouvez utiliser le portail de développement Windows pour modifier le bac à sable sur votre console.  Pour ce faire, accédez à « Dev page d’accueil » sur votre console et activez-la.

Après cela, vous pouvez taper l’adresse IP sur le navigateur web sur votre ordinateur pour vous connecter à votre console.  Vous pouvez ensuite cliquer sur « Xbox Live » et entrez le bac à sable dans la zone de texte.

### <a name="using-xbox-one-manager"></a>À l’aide d’un seul gestionnaire de Xbox

Xbox One Manager vous permet de gérer certains aspects de votre console à partir de votre PC.  Cela inclut le redémarrage, la gestion des applications installées et la modification de votre bac à sable.

Cliquez avec le bouton droit sur la console que vous souhaitez modifier le bac à sable pour et accédez à « Paramètres... »

Vous pouvez ensuite entrer un bac à sable il.

### <a name="using-xbox-one-console-ui"></a>À l’aide de l’interface utilisateur de console Xbox One

Si vous souhaitez modifier votre sandbox de développement directement à partir de votre console, vous pouvez accéder à « Paramètres ».  Puis accédez à « Paramètres de développeur » et vous verrez une option permettant de modifier votre bac à sable.

## <a name="sandbox-uses"></a>Bac à sable utilise

### <a name="data-that-is-sandboxed"></a>Données de sable
Vous pouvez utiliser les fonctionnalités de bac à sable pour gérer l’accès entre les développeurs de votre équipe pendant le processus de développement.  Par exemple, vous souhaiterez isoler des données entre votre équipe de développement et les testeurs.

Données sable incluent :
- Primes, les classements et les statistiques pour un utilisateur.  Primes accumulées pour un utilisateur dans un bac à sable n’est pas traduit en un autre bac à sable.
- Mode multijoueur et Matchmaking.  Les utilisateurs ne peuvent pas lire une mode multijoueur jeu avec une personne est un bac à sable différents.
- Configuration du service.  Si vous ajoutez une prime en nouveau titre dans un bac à sable, il n’est pas visible dans un bac à sable différents.  Cela s’applique à toutes les données de configuration de service.

Données non-sandbox sont principalement sociaux plus d’informations.  Ainsi par exemple, si un utilisateur suit un autre utilisateur, la relation est indépendant du bac à sable.

### <a name="examples"></a>Exemples
Quelques exemples sont fournis ci-dessous afin d’illustrer certains des avantages de la raison pour laquelle vous pouvez souhaiter utiliser plusieurs bacs à sable.

> **Remarque** : Si vous êtes dans le programme Creators Xbox, peut uniquement avoir un bac à sable.  Si vous avez devez créer plusieurs bacs à sable, appliquez à la ID@Xbox programme.

#### <a name="service-config-isolation"></a>Isolation de configuration de service
Comme mentionné ci-dessus, la configuration du service est spécifique de bac à sable.  Vous pouvez donc un *développement* bac à sable et un *test* bac à sable.  Lorsque vous donnez une build de votre titre à vos testeurs, vous devez publier votre [configuration du service](xbox-live-service-configuration.md) à la *test* bac à sable.

Puis en attendant, vous pouvez ajouter des primes, ou types de session multijoueur différente pour votre *développement* sandbox sans affecter la configuration du service qui rencontrent des vos testeurs.

#### <a name="multiplayer"></a>Mode multijoueur
Prenons l’exemple ci-dessus avec un *développement* et *test* bac à sable.  Configuration de votre service est peut-être le même entre les bacs à sable, mais vos développeurs créent des fonctionnalités multijoueurs et souhaitez test matchmaking entre eux.  Les testeurs sont également test multijoueurs.

Dans un cas comme celui-ci, vos développeurs veulent pas le Xbox Live service à mettre en correspondance avec les testeurs, car ils sont problèmes de débogage séparément.  Un bon moyen d’éviter ce problème serait pour vos développeurs se trouver dans le *développement* bac à sable et aux testeurs de se trouver dans un distinct *test* bac à sable.  Cela permet de conserver les deux groupes isolés.

## <a name="advanced"></a>Avancé

Pour simplifier votre processus de développement, commencez avec votre sandbox par défaut et ajouter de nouveaux bacs à sable judicieusement.

Une fois que vous trouvez votre isolation de contrôle et de données access a besoin croissant, vous pouvez voir [avancé Xbox Live les bacs à sable](advanced-xbox-live-sandboxes.md) article.  
