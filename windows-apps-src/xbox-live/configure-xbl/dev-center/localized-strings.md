---
title: Chaînes localisées
description: Découvrez comment localiser les chaînes dans l’espace partenaires
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jeux, uwp, windows 10, Xbox, des chaînes, les partenaires localisées
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656594"
---
# <a name="configuring-localized-strings-in-partner-center"></a>Configuration des chaînes traduites dans Partner Center

Vous pouvez utiliser cette page pour localiser toutes vos configurations de Xbox Live à tous les langages qui prend en charge de votre jeu. Toutes les configurations de service que vous avez créés sur les pages de Xbox Live suivantes seront ajoutées au fichier que vous devez télécharger.

Vous pouvez utiliser [partenaires](https://partner.microsoft.com/dashboard) pour configurer les chaînes localisées dans toutes les langues associées à votre jeu. Ajoutez la configuration en procédant comme suit :

1. Accédez à la **des chaînes localisées** section pour votre titre, situé sous **Services** > **Xbox Live** > **localisée chaînes**.
2. Cliquez sur le **télécharger** bouton qui télécharge un fichier localization.xml sur votre ordinateur local.

![Capture d’écran de la page de configuration des chaînes localisées dans des partenaires](../../images/dev-center/localized-strings/localized-strings-1.png)

3. Vous pouvez ajouter les chaînes localisées en dupliquant le <Value locale="en-US">Mazes lus</Value> balise et en modifiant la valeur des paramètres régionaux pour la langue de votre choix et la valeur de la chaîne localisée. Vous devez disposer d’au moins une balise value dans le paramètres régionaux d’affichage développeur pour éviter les erreurs.

![modifier les chaînes localisées](../../images/dev-center/localized-strings/localized-strings.gif)

4. Une fois que vous avez ajouté toutes les chaînes localisées, vous pouvez télécharger le fichier en le faisant glisser ou en parcourant votre ordinateur local.

![Image du bouton pour télécharger le fichier localization.xml](../../images/dev-center/localized-strings/localized-strings-2.png)

Veuillez noter que les erreurs suivantes peuvent apparaître lorsque vous téléchargez le fichier localization.xml :

| Erreur | Raison |
|---------------------------|-------------|
| Validation XSD ayant échoué : L’élément 'LocalizedString' dans l’espace de noms 'http://config.mgt.xboxlive.com/schema/localization/1' ne peut pas contenir de texte. Liste des éléments possibles attendus : 'Value' dans l’espace de noms 'http://config.mgt.xboxlive.com/schema/localization/1' | Cela se produit lorsque le document XML est incorrect |
| Il manque une entrée pour les paramètres régionaux d’affichage de développeur dans la chaîne de localisation | Cela se produit lorsqu’une chaîne localisée il manque une entrée dont les paramètres régionaux ne correspond pas aux paramètres régionaux d’affichage de développement |
| Validation XSD ayant échoué : L’attribut 'locale' est non valide : la valeur ' « n’est pas valide selon son type de données » http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString»-la contrainte de modèle a échoué. | Cela se produit lorsqu’une chaîne localisée il manque la valeur de paramètres régionaux dans le <Value> tag|
