---
title: Configuration de présence riche dans partenaires
description: Découvrez comment configurer des chaînes de présence enrichie de partenaires
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jeux, uwp, windows 10, Xbox une, chaînes de présence enrichie, partenaires
ms.openlocfilehash: 5c2add99c6fc6ad1c7f085eb35dc4fdba9e0688d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624124"
---
# <a name="configure-rich-presence-in-partner-center"></a>Configurer la présence enrichie de partenaires

Chaînes de présence enrichie affichent l’activité de dans le jeu d’un utilisateur. Ils sont affichés sous l’identité d’un joueur dans le **amis & clubs** liste ainsi que dans leur profil utilisateur Xbox Live. Les chaînes de présence enrichie configurées sont ajoutées pour le nom du jeu en cours de lecture. Si vous créez un jeu appelé BubblePop et que vous configurez la chaîne de présence enrichie « Popping bulles avec vos amis », votre chaîne configuré produirait « BubblePop - bulles Popping avec vos amis » sous forme d’un état. Vous trouverez ci-dessous comment une chaîne de présence enrichie apparaîtra dans le contexte.

Dans les utilisateurs de Xbox Live de capture d’écran suivants **rugir dernière** et **Lucha Uno** évoluez jeux à l’aide de chaînes de présence enrichie.

![Exemple de liste d’amis](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

Dans la capture d’écran suivante, vous pouvez voir **de Lucha Uno** complet de chaîne de présence enrichie dans son profil.

![Exemple de profil](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> Chaînes de présence enrichie ne sont pas disponibles pour les titres de programme Xbox Live Creators et par conséquent, ne sont pas configurables pour les titres. Le contenu dans cet article concerne ID@Xbox et titres de partenaires gérés.

## <a name="requirements"></a>Configuration requise

Avant de configurer des chaînes de présence enrichie vous et votre titre doivent respecter les critères suivants :

- Vous devez disposer d’un compte de développement Windows.
- Votre compte de développement doit être enregistré dans le ID@Xbox programme ou en tant qu’un compte de développeur partenaire géré.
- Votre titre doit être inscrite dans le centre partenaires et être Xbox Live est activé.

Avant de pouvoir utiliser les chaînes de présence enrichie vous devez les configurer dans l’espace partenaires.

## <a name="rich-presence-configuration-page"></a>Page de Configuration de présence riche

Chaînes de présence enrichie sont configurés dans le cadre du Service Xbox Live pour votre titre dans [partenaires](https://partner.microsoft.com/dashboard).

Accédez à la page de configuration riche présence avec les instructions suivantes :

1. Accédez à [partenaires](https://partner.microsoft.com/dashboard) sur developer.microsoft.com.
2. Connectez-vous à votre compte de développeur Windows enregistré si la connexion est demandée.
3. Choisissez votre titre Xbox Live est activé ou une application à partir de la **vue d’ensemble** page. Ne sélectionnez pas un titre de programme Creators comme il n’est pas activé pour la configuration de chaîne de présence enrichie.
4. Cliquez sur le **Services** vers le bas et sélectionnez Xbox Live.
5. Faites défiler jusqu'à la **présence enrichie** lier, puis cliquez dessus.

La page de présence enrichie affiche une brève description du service, un bouton pour créer une nouvelle chaîne de présence enrichie et une liste de recherche de vos chaînes précédemment configurés. À partir de cette page, vous pouvez configurer de nouvelles chaînes ainsi que modifier et passez en revue vos chaînes configurés.

![Exemple de chaîne de présence enrichie Page de configuration](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> Chaînes telles que « Jouer Net Runner - deathmatch multijoueur sur la Base de la lune avec 10 TUE. » ne sera pas disponible pour les développeurs à compter de la mise à jour de 2017 de plateforme de données. Plateforme de données 2013 *Variables* ne sont pas disponibles dans 2017 de plateforme de données. Dans ce cas, la variable est que le nombre d’arrête « 10 ». La chaîne équivalente après que la mise à jour de 2017 de plateforme de données serait « Jouer Net Runner - deathmatch multijoueur sur la Base de la lune. » « Lecture Net Runner - dans les menus » est toujours une présence enrichie chaîne valide.

## <a name="create-a-new-rich-presence-string"></a>Créer une nouvelle chaîne de présence enrichie

Pour créer une chaîne de présence enrichie, cliquez sur le bouton intitulé **chaîne de nouveau présence riche**. S’affiche avec l’interface utilisateur à renseigner le **détails de la présence** qui incluent le **Unique ID de présence riche** ainsi que le **afficher chaîne** pour votre nouvelle chaîne de présence enrichie.

![nouvelle chaîne de présence de la riche interface utilisateur](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**ID unique de la présence riche** -l’ID de présence riche Unique est une chaîne utilisée pour identifier votre chaîne de présence enrichie. Cette chaîne permet de définir l’état de joueurs pour votre jeu et est associée à la chaîne particulière que vous souhaitez afficher. Votre ID peut être un maximum de 50 caractères.

**Afficher chaîne** -afficher le chaîne est la chaîne que vous souhaitez afficher ajouté à l’état de certains gamers jeu votre. Voici où vous allez fournir dans la chaîne de présence enrichie que vous souhaitez afficher pour susciter l’intérêt de votre jeu. Votre affichage peut être un maximum de 100 caractères, mais il y aura des instances où seuls les 40 premiers caractères seront affichera.

Pour créer votre nouvelle chaîne de présence enrichie, renseignez les champs et appuyez sur la **enregistrer** bouton.
Une fois que vous cliquez sur Enregistrer, que vous serez renvoyé à la Page de Configuration de présence riche où vous verrez votre nouvelle chaîne de présence enrichie ajouté à la liste de chaînes configurés.

## <a name="review-edit-and-delete-strings"></a>Passez en revue, modifier et supprimer des chaînes

Ici, vous pouvez voir une page de configuration riche présence avec quelques chaînes configurés.
![Page de présence riche configuré exemple](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

Pour passer en revue les chaînes créés précédemment, simplement parcourir la liste dans la page de configuration riche présence. Vous pouvez voir à la fois l’ID Unique de la présence riche et afficher la chaîne entre eux. Cela sera utile lorsque vous devez utiliser l’Id de présence riche Unique dans le code de votre titre pour spécifier une chaîne de présence enrichie.

Pour modifier une présence riche chaîne cliquez simplement sur le **Unique ID de présence riche** lien pour la chaîne que vous souhaitez modifier. Vous accéderez à la même interface utilisateur utilisée pour créer une nouvelle chaîne de présence enrichie avec les paramètres actuels de chaîne renseignés pour la modification. Après avoir apporté les modifications cliquez sur le **enregistrer** bouton Mettre à jour la chaîne configurée avec vos modifications.

Pour supprimer un clic de chaîne de présence enrichie configuré le **supprimer** lien sur la page de Configuration de présence riche dans la même ligne que la chaîne de présence enrichie vous souhaitez supprimer. Vous devrez confirmer la suppression.

## <a name="further-reading"></a>Ressources supplémentaires

Pour obtenir plus d’informations conceptuelles détaillées sur les fonctionnalités de chaîne de présence enrichie et comment les implémenter, lisez le [documentation de présence enrichie](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview).