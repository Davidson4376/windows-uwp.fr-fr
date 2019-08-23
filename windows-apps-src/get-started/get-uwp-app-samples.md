---
title: Obtenir des exemples d’applications UWP
description: Découvrez comment télécharger les exemples de code UWP à partir de GitHub.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, exemple de code, exemples de code
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 6b0e30804eabb7e50c5a7319bba9a6b2c83e1d7e
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487857"
---
# <a name="get-uwp-app-samples"></a>Obtenir des exemples d’applications UWP

Des exemples d’applications de plateforme Windows universelle (UWP) sont disponibles dans des dépôts sur GitHub. Consultez [Exemples](https://developer.microsoft.com/windows/samples) pour obtenir une liste par catégorie pouvant faire l’objet d’une recherche, ou parcourez le (https://github.com/Microsoft/Windows-universal-samples "dépôt GitHub d’exemples d’applications de plateforme Windows universelle") [Microsoft/Windows-universal-samples]. Le dépôt Windows-universal-samples contient des exemples illustrant toutes les fonctionnalités UWP et les modèles d’utilisation de leurs API.

![Dépôt GitHub d’exemples UWP](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Télécharger le code

Pour télécharger les exemples, accédez au [dépôt](https://github.com/Microsoft/Windows-universal-samples "Dépôt GitHub d’exemples d’applications de plateforme Windows universelle"). Sélectionnez **Clone or download**, puis **Download ZIP**. 

![Téléchargement des exemples](images/SamplesDownloadButton.png)

Vous pouvez également [télécharger les exemples](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Téléchargement de fichiers zip d’exemples d’applications de plateforme Windows universelle") à partir de cet article.

Le fichier .zip contient toujours les exemples les plus récents. Vous n’avez pas besoin d’un compte GitHub pour télécharger le fichier. Lors de la publication d’une mise à jour du kit SDK, ou si vous souhaitez obtenir des changements et ajouts récents, il vous suffit de télécharger le dernier fichier zip.

> [!NOTE]
> Pour ouvrir, créer et exécuter des exemples UWP, vous devez avoir Visual Studio 2015 ou ultérieur et le SDK Windows. Vous pouvez obtenir une [copie gratuite de Visual Studio Community](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Téléchargements des outils de développement Windows"). Visual Studio Community prend en charge la création d’applications UWP.  
>
> Pour que les exemples fonctionnent correctement, veillez à décompresser l’intégralité de l’archive, et pas simplement les exemples individuels. Tous les exemples dépendent du dossier SharedContent de l’archive. Les exemples de fonctionnalités UWP utilisent des fichiers liés dans Visual Studio pour réduire la duplication de fichiers communs, notamment les fichiers de modèles d’exemples et les composants d’images. Les fichiers communs sont stockés dans le dossier SharedContent à la racine du dépôt. Les liens sont utilisés dans les fichiers projet pour faire référence aux fichiers communs.
> 

## <a name="open-the-samples"></a>Ouvrir les exemples

Après avoir téléchargé le fichier .zip, ouvrez les exemples dans Visual Studio.

1.  Avant de décompresser l’archive, cliquez avec le bouton droit sur le fichier et sélectionnez **Propriétés** > **Débloquer** > **Appliquer**. Décompressez ensuite l’archive dans un dossier local sur votre ordinateur.

    ![Archive décompressée](images/SamplesUnzip1.png)
2.  Chaque dossier du dossier Samples contient un exemple de fonctionnalité UWP.

    ![Dossiers d’exemples](images/SamplesUnzip2.png)
3.  Sélectionnez un exemple, par exemple Altimeter. Les sous-dossiers indiquent les langues prises en charge.

    ![Dossiers de langages](images/SamplesUnzip3.png)
4.  Sélectionnez le dossier correspondant à la langue que vous souhaitez utiliser. Dans le contenu du dossier, vous verrez un fichier de solution Visual Studio (.sln) que vous pouvez ouvrir dans Visual Studio. Par exemple, *Altimeter.sln*.

    ![Solution VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Formuler des commentaires, poser des questions et signaler des problèmes

En cas de problème ou question, utilisez l’onglet **Problèmes** dans le dépôt pour signaler un nouveau problème. Nous ferons tout notre possible pour vous aider.

![Image de commentaires](images/GitHubUWPSamplesFeedback.png)
