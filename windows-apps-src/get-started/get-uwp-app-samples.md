---
title: "Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub"
description: "Découvrez comment télécharger les exemples de fonctionnalités UWP à partir de GitHub"
author: JoshuaPartlow
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.openlocfilehash: e576150cd06000405fa4754e147fbc1546cdf703
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#<a name="get-the-universal-windows-platform-uwp-samples-from-github"></a>Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub
Les exemples d’applications UWP sont disponibles par le biais de référentiels sur GitHub. Si vous utilisez UWP pour la première fois, le mieux est de commencer par le référentiel [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Universal Windows Platform app samples GitHub repository"), qui contient des exemples illustrant toutes les fonctionnalités UWP ainsi que les modèles d’utilisation de leurs API.  
![Référentiel d’exemples UWP de GitHub](images/GitHubUWPSamplesPage.png) Vous pouvez obtenir des exemples supplémentaires dans la section [Exemples](https://developer.microsoft.com/windows/samples "Dev Center samples") du Centre de développement.  

##<a name="download-the-code"></a>Télécharger le code
Pour télécharger les exemples, accédez au [référentiel](https://github.com/Microsoft/Windows-universal-samples "Référentiel GitHub d’exemples d’application de plateforme Windows universelle"), puis sélectionnez **Clone or download**, et ensuite **Download ZIP**. Une autre possibilité consiste simplement à cliquer [ici](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Téléchargement de fichierZIP d’exemples d’application de plateforme Windows universelle").

Le fichierZIP contient toujours les exemples les plus récents. Vous n’avez pas besoin d’un compte GitHub pour le télécharger. Lors de la publication d’une mise à jour du Kit de développement logiciel, ou si vous souhaitez obtenir des changements ou ajouts récents, recherchez simplement le dernier fichier Zip.

![Téléchargement de l’exemple](images/SamplesDownloadButton.png)


> **Remarque**: l’ouverture, la création et l’exécution d’exemples UWP requièrent l’installation de VisualStudio2015 et du SDK Windows. Si vous n’avez pas encore installé VisualStudio, vous pouvez obtenir une copie gratuite de VisualStudio2015 CommunityEdition avec prise en charge de la génération d’applications UWP [ici](http://go.microsoft.com/fwlink/p/?LinkID=280676 "Téléchargements des outils de développement Windows").  
>
> Veillez également à décompresser l’intégralité de l’archive, et pas seulement les exemples. Tous les exemples dépendent du dossier SharedContent de l’archive. Les exemples de fonctionnalités UWP utilisent des fichiers liés dans VisualStudio pour réduire la duplication de fichiers communs, notamment les fichiers de modèles d’exemples et les composants d’images. Ces fichiers communs sont stockés dans le dossier SharedContent à la racine du référentiel et sont référencés dans les fichiers de projet à l’aide de liens.

Après avoir téléchargé le fichier zip, ouvrez les exemples dans VisualStudio:

1.  Avant de décompresser l’archive, sélectionnez-la à l’aide du bouton droit et cliquez sur **Propriétés** > **Débloquer** > **Appliquer**. Décompressez ensuite l’archive dans un dossier local sur votre ordinateur.

    ![Archive décompressée](images/SamplesUnzip1.png)
2.  Dans le dossier d’exemples, vous verrez un certain nombre de dossiers contenant chacun un exemple de fonctionnalité UWP.

    ![Dossiers d’exemples](images/SamplesUnzip2.png)

3.  Sélectionnez un exemple, par exemple Altimeter. Vous obtenez plusieurs dossiers indiquant les langages pris en charge.

    ![Dossiers de langages](images/SamplesUnzip3.png)

4.  Sélectionnez la langue que vous souhaitez utiliser, par exemple CS pour C\#. Vous obtenez un fichier de solution VisualStudio que vous pouvez ouvrir dans Visual Studio.

    ![Solution VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Formuler des commentaires, poser des questions et signaler des problèmes

En cas de problème ou question, utilisez simplement l’onglet Problèmes dans le référentiel pour signaler un nouveau problème et obtenir de l’aide.

![Image de commentaires](images/GitHubUWPSamplesFeedback.png)
