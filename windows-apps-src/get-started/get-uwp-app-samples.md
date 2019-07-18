---
title: Obtenir des exemples d’applications UWP
description: Découvrez comment télécharger les exemples de code UWP à partir de GitHub
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, exemple de code, exemples de code
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 7f56b0f9e4cb7f89b8bc929015ecdf6d5c64d42e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321075"
---
# <a name="get-uwp-app-samples"></a>Obtenir des exemples d’applications UWP

Des exemples d’applications de plateforme Windows universelle (UWP) sont disponibles dans des dépôts sur GitHub. Consultez [Exemples](https://developer.microsoft.com/windows/samples%20%22Dev%20Center%20samples%22) pour obtenir une liste par catégorie pouvant faire l’objet d’une recherche, ou parcourez le (https://github.com/Microsoft/Windows-universal-samples "dépôt GitHub d’exemples d’application de plateforme Windows universelle") [Microsoft/Windows-universal-samples] qui contient des exemples illustrant toutes les fonctionnalités UWP et les modèles d’utilisation de leurs API.  
![Dépôt d’exemples UWP sur GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Télécharger le code

Pour télécharger les exemples, accédez au [référentiel](https://github.com/Microsoft/Windows-universal-samples "Référentiel GitHub d’exemples d’application de plateforme Windows universelle"), sélectionnez **Clone or download** (Cloner ou télécharger), puis **Download ZIP** (Télécharger un fichier zip). Une autre possibilité consiste simplement à cliquer [ici](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Téléchargement de fichier ZIP d’exemples d’application de plateforme Windows universelle").

Le fichier Zip contient toujours les exemples les plus récents. Vous n’avez pas besoin d’un compte GitHub pour le télécharger. Lors de la publication d’une mise à jour du Kit de développement logiciel, ou si vous souhaitez obtenir des changements ou ajouts récents, recherchez simplement le dernier fichier Zip.

![Téléchargement de l’exemple](images/SamplesDownloadButton.png)


> [!NOTE]
> L’ouverture, la création et l’exécution d’exemples UWP requièrent l’installation de Visual Studio 2015 ou version ultérieure et du Kit de développement logiciel (SDK) Windows. Vous pouvez obtenir une copie gratuite de Visual Studio Community avec un support pour la création d’applications UWP [ici](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Téléchargements des outils de développement Windows").  
>
> Veillez également à décompresser l’intégralité de l’archive, et pas seulement les exemples. Tous les exemples dépendent du dossier SharedContent de l’archive. Les exemples de fonctionnalités UWP utilisent des fichiers liés dans Visual Studio pour réduire la duplication de fichiers communs, notamment les fichiers de modèles d’exemples et les composants d’images. Ces fichiers communs sont stockés dans le dossier SharedContent à la racine du référentiel et sont référencés dans les fichiers de projet à l’aide de liens.

Après avoir téléchargé le fichier zip, ouvrez les exemples dans Visual Studio :

1.  Avant de décompresser l’archive, sélectionnez-la à l’aide du bouton droit et cliquez sur **Propriétés** > **Débloquer** > **Appliquer**. Décompressez ensuite l’archive dans un dossier local sur votre ordinateur.

    ![Archive décompressée](images/SamplesUnzip1.png)
2.  Dans le dossier d’exemples, vous verrez un certain nombre de dossiers contenant chacun un exemple de fonctionnalité UWP.

    ![Dossiers d’exemples](images/SamplesUnzip2.png)

3.  Sélectionnez un exemple, par exemple Altimeter. Vous obtenez plusieurs dossiers indiquant les langages pris en charge.

    ![Dossiers de langages](images/SamplesUnzip3.png)

4.  Sélectionnez le langage que vous souhaitez utiliser, par exemple CS pour C\#. Vous obtenez un fichier de solution Visual Studio que vous pouvez ouvrir dans Visual Studio.

    ![Solution VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Formuler des commentaires, poser des questions et signaler des problèmes

En cas de problème ou question, utilisez simplement l’onglet Problèmes dans le référentiel pour signaler un nouveau problème et obtenir de l’aide.

![Image de commentaires](images/GitHubUWPSamplesFeedback.png)
