---
title: Prise en main des jeux de cross-play
description: Découvrez comment développer des jeux de cross-play qui s’exécutent sur PC et Xbox One consoles.
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, cross-play, lire n’importe où
ms.localizationpriority: medium
ms.openlocfilehash: 79b0c211291d8a27456126ea0378f9093d1dccab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646724"
---
# <a name="get-started-with-cross-play-games"></a>Prise en main des jeux de cross-play

## <a name="summary"></a>Résumé

Avec le lancement de Windows 10, les développeurs de jeux sera en mesure de lancer les produits uniques à la fois sur Xbox One (comme un jeu XDK) et Windows 10 (comme un jeu UWP). Dans certains cas, les développeurs souhaitent activer cross-play pour ces jeux, où les versions Xbox One et Windows 10 de leur jeu sont unifiées sur Xbox Live services tels que multijoueurs, enregistrer des jeux, primes et bien plus encore. Pour activer cross-play, ces jeux partagent une configuration de service unique ID de titre et Xbox Live sur les XDK UWP versions et du jeu.

Réception d’un jeu aujourd'hui XDK + UWP nécessite 4 étapes principales :

1.  Créer votre produit UWP dans Partner Center

2.  Créer votre jeu XDK dans XDP, en sélectionnant les plateformes que vous souhaitez partager configuration XBL sur divers

3.  Lier vos informations de produit UWP à votre produit XDK dans XDP

4.  Configurer et publier la Xbox Live via XDP

L’objectif de ce document est de détail sur ce sujet ces 4 étapes pour le rendre aussi simple que possible pour les employés de Xbox recevoir des jeux multiplateforme XDK + UWP

## <a name="terminology"></a>Terminology

### <a name="scenario-terms"></a>Termes du contrat de scénario

1.  Cross-Play : Un jeu qui publie sur plusieurs plateformes, mais partage un Xbox unique ID du titre et la configuration du service. Le résultat final est que les deux versions du jeu partagent la même Xbox Live configuration - primes, tableaux de résultats, jeu enregistre, mode multijoueur et bien plus encore.

2.  Partenaires : Le [portal](https://partner.microsoft.com/dashboard) où vous pouvez réserver des identités d’application pour une utilisation dans UWP aujourd'hui et le programme d’installation Xbox Live configuration de développement pour UWP.

3.  XDP : La Xbox portail des développeurs, qui existe aujourd'hui pour recevoir, configurer, publier des jeux XDK une Xbox et déposé et s’affiche à utiliser pour ingérer, configurer et publier des jeux XDK + UWP Cross-Play.

### <a name="identity-terms"></a>Termes du contrat de l’identité

1.  ID de titre : Il s’agit de l’ID de titre Xbox, utilisé pour identifier chaque jeu Xbox Live. Un titre est mappé à un produit unique, qui s’étend sur plusieurs plateformes.

2.  ID de Configuration de service (SCID) : Chaque titre de Xbox (identifié par un ID de titre) possède un ID de Configuration de Service correspondant (également appelé SCID). Cet ID permet à Xbox Live identifier les règles / configuration à utiliser pour interagir avec le titre de la.

3.  Nom de famille de packages (NFP) : Il s’agit d’une identité affectée à chaque produit créé dans l’espace partenaires. Une fois que vous liez votre UWP à l’identité de ce produit Partner Center, il accepte cette NFP. Pfns aura fini est des identificateurs de produit unique, qui s’étend sur plusieurs plateformes. Pfns aura fini est 1:1 avec l’ID de titre Xbox.

4.  ID d’application de compte de service administré : Également appelé ID de Client MSA, il s’agit une autre identité de l’application affectée par MSA au moment de la création de produit au centre de partenaires. Cette identité permet les services Microsoft d’identifier votre application. ID d’application MSA sont 1:1 avec pfns aura fini (et en conséquence avec l’ID de titre Xbox).

## <a name="scenario-overview"></a>Vue d’ensemble du scénario

### <a name="what-is-cross-play"></a>Qu’est Play croisée ?

Une vitrine expérience de Windows 10 ; Cross-play est jeux inter-périphériques entre la Xbox One et PC, avec des jeux partage une seule configuration de Xbox Live entre les versions d’appareil pour égayer les scénarios, tels que le mode multijoueur entre les périphériques, des primes & résultats commerciaux, et enregistre de jeu.

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>Quels sont les avantages et inconvénients de Cross-Play ?

Cross-Play est probablement la bonne approche pour vous si vous souhaitez que les versions XDK et UWP de votre jeu :

-   S’engager dans inter-périphériques multijoueurs (vs Xbox One. PC) au moins un mode de jeu multijoueur

-   Partager un enregistrement de jeu unique que l’utilisateur peut utiliser sur des appareils

-   Possèdent un ensemble unique de primes & Gamescore / défis / leaderboards peuvent feu progression de celui-ci contre sur des appareils

Cross-Play n’est probablement pas la bonne approche pour vous si :

-   Vous souhaitez empêcher les PC et Xbox One des lecteurs de votre jeu multijoueur présentes sur des appareils dans tous les modes de jeu multijoueurs

-   Vous souhaitez conserver Xbox One et jeu PC enregistrer distinct (peut-être pour des raisons de sécurité ou de confiance)

-   Vous souhaitez que les versions Xbox One et PC de votre jeu pour que le score de joueur distinct (également appelés utilisateurs d’acheter une Xbox et PC peuvent recevoir Gamescore 1000 pour chaque plateforme au lieu d’un 1000 partagé)

En règle générale, Cross-Play ajoute la valeur de la plupart à :

-   Gratuit-to-Play / Xbox jouer en tout lieu, qui soulignent la continuité entre les versions de PC et Xbox One du jeu

-   Jeux présentant le mode multijoueur inter-périphériques entre le PC et Xbox One

**REMARQUE** : Cross-Play est disponible à la fois pour les nouveaux jeux qui publient simultanément les versions XDK et UWP de leur jeu, ainsi que des jeux qui ont déjà été fournis un XDK, mais ajoutant une version UWP.

### <a name="what-are-the-restrictions-of-cross-play"></a>Quelles sont les restrictions de Cross-Play ?

Jusqu'à ce que Xbox One prend en charge les jeux UWP, jeux de Cross-Play nécessite un XDK (pour la console Xbox One) et une version UWP (pour les PC Windows 10) du jeu.

N’importe quel XDK + UWP Cross-Play jeu est fourni avec certaines restrictions importantes :

1.  **Les titres XDK doivent être ingérées dans XDP**. À la fois à partir d’une configuration du service et l’expérience de publication principaux, partenaires n’est pas équipé pour les titres XDK en fonction de prendre en charge.

2.  **Une configuration de service unique créée dans XDP peut être utilisé à la fois par la version XDK et UWP d’un jeu**. Nous avons ajouté de nouvelles fonctionnalités à XDP pour permettre un jeu de partager une configuration de service unique entre ses versions XDK et UWP. La version UWP devra toujours être publiée dans le centre de partenaires pour les packages / catalogue, mais tous les publication de configuration de service peut être effectuée dans XDP.

3.  **Configuration du service ne peut pas être fractionnée entre XDP et partenaires**. XDP et les partenaires ne sont pas prenant en charge les uns des autres – publication dans une seule écritures excédentaire existants publie à partir de l’autre. Cela est susceptible d’irrémédiablement rompre la configuration du service et créer des expériences utilisateur terrible (primes fuites, perdu des jeux d’enregistrements, etc.), et par conséquent, nous demandons 100 % de la configuration du service à faire dans XDP pour les jeux de cross-play XDK + UWP.

### <a name="create-your-uwp-product-in-partner-center"></a>Créer votre produit UWP dans Partner Center

Créer un produit UWP dans partenaires en suivant le guide : [Ajout de Xbox Live à un projet UWP nouveau ou existant](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>Le programme d’installation de votre produit XDK dans XDP

Maintenant que votre UWP est créé, vous êtes prêt à configurer votre produit XDK dans XDP. Si vous ne disposez pas d’un titre XDK, vous devez créer un.

### <a name="create-your-xdp-product"></a>Créer votre produit XDP

Travailler avec votre responsable de compte pour créer un nouveau produit sous votre serveur de publication dans XDP ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher)).

Lorsque vous créez le produit dans XDP, assurez-vous que vous faites défiler vers le bas de la section gauche de l’interface utilisateur pour sélectionner vos plateformes. Vérifiez toutes les plateformes que vous avez **prévoyez un jour** libérer le jeu sur avec l’intégration de cross-play Xbox Live.

Une fois que vous avez sélectionné vos plateformes, spécifiez le type d’accès aux ressources pour votre jeu (très probablement un titre XDK) et les mécanismes de mise en production prévue pour ce produit.


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>Mettre à jour vos plateformes de produit XDP

Si vous avez déjà un produit XDK existant dans XDP, il doit être mis à jour pour prendre en charge de la plate-forme PC. Pour ce faire, une seule fois sur le produit, accédez au programme d’installation du produit &gt; Type de plateforme.

![](../images/ingesting_crossplay_games_xdp/image10.png)

Dans cette page, sélectionnez les plateformes que vous souhaitez prendre en charge (options sont Xbox One, seuil PC et Windows Mobile). Une fois que vous êtes satisfait de votre sélection, sélectionnez le bouton « Envoyer les plateformes ».

Cette modification ne sera immédiatement être répercutée en direct (aucun nécessaire pour une configuration du service, un catalogue ou un fichier binaire ne publier pour que cela prenne effet). Notez que cette configuration s’étend sur des bacs à sable : vous ne pouvez avoir des types de plateformes différentes par le bac à sable pour votre jeu.

### <a name="enter-your-msa-app-id"></a>Entrez votre ID d’application MSA

Une fois le produit XDP est créé, accédez à la page de configuration du produit pour le produit à entrer l’ID d’application MSA créé précédemment. Le programme d’installation du produit est accessible en cliquant sur le plus à gauche « état box » dans la barre des tâches pour le produit dans XDP, comme indiqué ci-dessous.

![](../images/ingesting_crossplay_games_xdp/image11.png)

Une fois que vous rendre sur la page d’installation du produit, sélectionnez la section « Configuration ID de l’Application ». Dans cette zone, vous pouvez entrer l’ID d’application de compte de service administré que vous avez récupéré et placez-le dans le champ « ID d’Application », comme indiqué ci-dessous.

![](../images/ingesting_crossplay_games_xdp/image12.png)

**Vous** **n’avez pas besoin d’entrer d’attribution de nom et le serveur de publication**, **et spécifiquement ne devez pas utiliser le lien « Obtenir l’ID Application » dans la page**, que vous disposez déjà d’un ID d’application MSA vous devez entrer dans cette champ et ne souhaitez pas d’un nouveau généré pour votre application.

Une fois que vous avez entré votre ID d’application MSA dans le champ « ID d’Application », cliquez sur le bouton « Envoyer Application ID d’installation ». Cela va enregistrer vos informations d’ID d’application de compte de service administré avec sécurité Xbox Live – chaque fois que vous effectuez une requête pour extraire un XToken votre UWP, la revendication de titre contenue dans maintenant mappera à ce produit XDP (tant que la plateforme Windows universelle est correctement à l’aide de l’identité du manifeste AppX vous créer d dans partenaires !)

### <a name="enter-the-partner-center-pfn-into-xdp"></a>Entrez le NFP partenaires dans XDP

Bien que les étapes ci-dessus sont suffisantes pour faire passer votre jeu UWP authentifié et à l’aide de Xbox Live avec la Configuration de Service, vous créez et publiez dans XDP, certaines fonctionnalités Xbox Live (telles que les invitations multijoueurs) doivent être conscient NFP de votre jeu UWP pour fonctionner correctement.

Pour ce faire, accédez au programme d’installation du produit &gt; liaison de centre de développement

![](../images/ingesting_crossplay_games_xdp/image13.png)

Dans cette page, entrez le NFP pour votre application UWP (comme récupérée dans la section 4.1.1), puis sélectionnez le bouton « Enregistrer ».

Cette configuration n’est immédiatement effectuée en direct. Il est mis en direct via futur Configuration du Service publie dans un bac à sable. Par conséquent, ces informations sont sable et doivent être publiée pour chaque bac à sable soit disponible.

### <a name="flag-your-app-for-xbox-cert-in-partner-center"></a>Indicateur de votre application pour Cert Xbox dans partenaires

Aujourd'hui, l’obtention de votre jeu reconnu comme compatibles pour Xbox Cert Xbox Live nécessite une intervention manuelle. Travail avec votre gestionnaire de versions pour marquer votre application est que Xbox Live activé dans l’espace partenaires pour la détection de SmartCert.

### <a name="update-your-uwp-game-in-the-xbox-app"></a>Mettre à jour votre jeu UWP dans l’application Xbox

Normalement, l’application Xbox utilise un ID de titre généré automatiquement pour tous les jeux UWP à la Xbox Live expériences au sein de l’application Xbox de puissance. Pour que votre jeu UWP utiliser correctement l’ID titre XDP dans l’application Xbox, une mise à jour de données doit être effectué au sein de partenaires, **avant de soumettre votre UWP jeu pour la mise en production.**

Pour ce faire, veuillez contacter votre mère et leur dire que vous souhaitez mettre à jour de votre ID de titre d’application Xbox pour votre nom de titre.  Veillez à inclure l’ID de titre créé dans XDP (visibles dans XDP sous le programme d’installation du produit &gt; détails du produit) et l’URL pour Windows 10 pour votre UWP (visible dans le centre partenaires sous gestion des applications &gt; identité de l’application).

## <a name="configure-xbox-live-in-xdp"></a>Configurer Xbox Live dans XDP

### <a name="service-configuration"></a>Configuration du service

Avec nos produits XDP et partenaires correctement configuré et lié, vous êtes libre de configurer votre configuration de Xbox Live partagée au sein de XDP comme vous le feriez normalement pour un titre XDK.

**RAPPEL** – lié XDK + UWP jeux doivent, en aucun cas, activer, configurer ou publier la configuration du service Xbox Live via l’espace partenaires. Suivez ces conseils peut endommager définitivement de la configuration de Xbox Live à votre jeu.

### <a name="catalog-configuration"></a>Configuration du catalogue

Pour un jeu XDK + UWP, configuration du catalogue doit être le programme d’installation à deux reprises : une fois dans XDP pour votre XDK et également dans le centre de partenaires pour votre UWP.

Pour la configuration XDP, c’est exactement identique à un produit XDK normal. Pour la configuration de partenaires, vous pouvez trouver les étapes plus détaillées [ici](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Pour les jeux UWP uniquement, qu’un ensemble limité de configuration du catalogue doit être le programme d’installation. En particulier, les informations Marketing doit être remplie pour les jeux UWP uniquement, comme la configuration du service consomme les chaînes entrées ici pour n’importe quel XDP configurer jeu Xbox Live. En outre, les disponibilités doivent être configuré avec « non disponible » pour vous assurer que le jeu n’apparaîtra jamais dans le catalogue Xbox One si/quand les informations de catalogue pour le jeu est publié.
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>Configuration binaire

Pour un jeu XDK + UWP, configuration binaire doit être le programme d’installation à deux reprises : une fois dans XDP pour votre XDK et également dans le centre de partenaires pour votre UWP.

Pour la configuration XDP, c’est exactement identique à un produit XDK normal. Pour la configuration de partenaires, vous pouvez trouver les étapes plus détaillées [ici](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Pour les jeux UWP uniquement, il est inutile pour configuration binaire dans XDP. Vous pouvez ignorer cette étape entièrement.
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>Publier dans XDP

En règle générale, la publication d’un jeu XDK + UWP suit le même processus que la publication d’un titre XDK séparément dans XDP et votre UWP dans Partner Center. Par conséquent la ci-dessous se concentre sur les processus unique ou considérations à faire pour les jeux de cross-play.

### <a name="dev-sandbox-publishing"></a>Publication de bac à sable de développement

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>Aucun équivalent du catalogue de bac à sable de développement pour Microsoft Store

Bien que XDP vous permet de publier votre catalogue et les binaires vers la version de bac à sable de développement du catalogue Xbox One, le catalogue de Microsoft Store n’a aucune prise en charge de bac à sable. Par conséquent, testez votre UWP dans un bac à sable de développement vous oblige à effectuer un chargement indépendant que UWP et le lire directement. Cela n’affecte pas la Xbox Live test, mais peut modifier votre processus d’évaluation.

<table>
  <tr>
    <td>
      Pour un jeu UWP uniquement, il est toujours requis pour publier le XDK informations de catalogue de titre pour débloquer la configuration du service de publication, même si le jeu UWP uniquement n’a pas Xbox One présence de catalogue.
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>Publication de bac à sable CERT

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Coordination entre la publication de XDP et la publication de partenaires

Dans XDP, le déplacement de certificat à la vente au détail est deux actions distinctes et explicites. Toutefois, dans le centre de partenaires, le processus de soumission déplace automatiquement votre jeu par la certification et à la vente au détail. Par conséquent, il existe un ordre des opérations à respecter.

Lorsque vous êtes prêt à passer à la certification, vous devez suivre les étapes suivantes dans l’ordre :

1.  Publier votre produit XDK dans XDP vers le certificat (y compris le catalogue, binaire et configuration de service)

2.  Démarrez votre soumission de partenaires de votre produit UWP

    1.  **Veillez à sélectionner « Publier cette application manuellement » ou « ne pas avant \[date\]» dans le champ de date de publication !** Si vous ne le faites pas, votre jeu UWP peut être publié pour la vente au détail automatiquement sans votre intervention.

<table>
  <tr>
    <td>
      Pour un jeu UWP uniquement, il est toujours requis pour publier votre configuration de service et de catalogue vers le certificat dans XDP avant de commencer votre soumission Partner Center, même si vous n’avez pas un binaire de titre XDK que vous publiez.
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>Publication de bac à sable de vente au détail

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Coordination entre la publication de XDP et la publication de partenaires

Une fois la certification du titre du XDK est terminée et que votre UWP a quitté de certification et est prêt à publier, vous êtes prêt à publier votre application vers une version commerciale. Là encore, il est important de suivre ce processus dans un ordre spécifique pour garantir que votre titre XDK et le UWP restent alignés.

1.  Une fois que vous êtes prêt, publier votre produit XDK dans XDP vers une version commerciale (y compris le catalogue, binaire et configuration de service)

2.  Dans le centre de partenaires, sur la page de certification de votre produit, sélectionnez « Publier maintenant » si vous aviez précédemment sélectionné « Publier cette application manuellement », ou attendre la publication se produire si vous avez sélectionné « ne pas avant \[date\]».

Une fois ces étapes terminées, vous devez avoir votre titre XDK et un jeu UWP publié dans le monde, avec une configuration de service partagé dans la vente au détail. Félicitations !

<table>
  <tr>
    <td>
      Pour un jeu UWP uniquement, il est toujours requis pour publier votre catalogue et configuration de service pour la vente au détail dans XDP avant la publication terminée dans le centre de partenaires, sinon la UWP publiée ne seront pas en mesure d’accéder à Xbox Live.
    </td>
  </tr>
</table>
