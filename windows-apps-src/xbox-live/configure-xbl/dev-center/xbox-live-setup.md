---
title: Configuration du programme d’installation de Xbox Live
description: Décrit comment vous pouvez configurer le programme d’installation de Xbox Live dans Partner Center.
ms.assetid: ''
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, jeux, uwp, windows 10, Xbox une, partenaires, le programme d’installation de Xbox Live
ms.openlocfilehash: 9a846a4b7f0069216e92eb123b33d9fc0f7f67c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612824"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>Configurer le programme d’installation en direct de Xbox dans Partner Center

Vous pouvez utiliser [partenaires](https://developer.microsoft.com/dashboard) pour configurer l’ensemble initial de propriétés Xbox Live qui sont associés à votre jeu. Ajoutez la configuration en procédant comme suit :

1. Accédez à la **le programme d’installation de Xbox Live** section pour votre titre, situé sous **Services** > **Xbox Live** > **le programme d’installation de Xbox Live** .
2. Dans cette page, vous pouvez définir les noms de titre, de paramètres régionaux par défaut, de type de produit, de familles d’appareils et de la date à embargo. Une fois que vous avez terminé à la définition de votre configuration, cliquez sur le **enregistrer** bouton pour envoyer les modifications.

## <a name="title-names"></a>Noms de titre
En cliquant sur **ajouter localisé titre**, vous pouvez entrer un nom pour votre produit et sélectionnez une langue à localiser. Notez ici que le nom doit correspondre aux noms de produits localisée que vous avez définies dans la page de propriétés de l’envoi. Valeur par défaut est anglais (en-US).

![Image de la boîte de dialogue Ajouter titre localisé Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>Paramètres régionaux par défaut
Cette option vous permet de définir la langue par défaut à utiliser pour configurer toutes vos chaînes dans la configuration du service Xbox Live. Par exemple, si vous définissez les paramètres régionaux par défaut vers l’espagnol (es-ES) et que vous souhaitez configurer une prime, puis au minimum, le nom de la réalisation et la description aurait en espagnol. En d’autres termes, vous ne peut pas définir cette option vers l’espagnol, mais uniquement fournir les informations de prime en anglais. Toute votre configuration de service Xbox Live doit être fournie dans la même version que celle des paramètres régionaux par défaut. Par défaut, les paramètres régionaux par défaut sont définie sur anglais (en-US).
> [!NOTE]
> En outre, toutes les chaînes peuvent être localisés dans la page de chaînes traduites.  

![Image de la liste déroulante pour choisir vos paramètres régionaux par défaut dans l’espace partenaires, sélectionnez](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>Type de produit
Le menu de liste déroulante vous permet de que modifier le type de produit. Les valeurs par défaut pour le type **jeu**. Le choix que vous effectuez aura un impact sur les fonctionnalités Xbox Live à votre disposition. Vous avez le choix entre trois options :
1. App 
2. Game 
3. Démo du jeu 

![Image de la liste déroulante pour choisir votre type de produit dans l’espace partenaires, sélectionnez](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>Familles d’appareils
Cette configuration vous permet de choisir le type de périphériques sur lequel votre titre peut accéder à Xbox Live. Par défaut, toutes les familles d’appareils sont activés. Vous pouvez vérifier les appareils pour les activer.

![Image des cases à cocher Sélection pour sélectionner les familles d’appareils de partenaires](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>Date de embargo
La date que vous sélectionnez déterminent quand votre configuration Xbox Live est publiée pour le grand public. Il est important de noter que même si vous avez publié vos modifications à la vente au détail leur ne seront pas mise en service, sauf si la date à embargo a été remplie. Pour expliquer plus en détail :
1. Si vous sélectionnez une date dans le futur, les modifications deviennent disponibles au public à cette date.
2. Si vous sélectionnez une date dans le passé, les modifications sera disponibles au public, dès que vous publiez vos modifications à la vente au détail.

Cliquez sur le sélecteur de date-heure, et il se développe pour vous permettre de sélectionner la date et l’heure. Une fois que vous cliquez sur **OK**, la date à embargo sera définie.

![Image de la définition de la date à embargo dans Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>Paramètres avancés

Vous pouvez cliquer sur **Show options** pour définir le **plusieurs points de présence**. Plusieurs points de présence permet à l’utilisateur même vous connecter à Xbox Live à partir de plusieurs appareils en même temps. Fonctionnalités telles que Xbox Live, primes et mode multijoueur seront ont un accès limité. Par conséquent, cette option n’est pas recommandée pour les jeux. Activez cette option en cochant la case.
