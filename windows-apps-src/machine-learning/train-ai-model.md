---
author: serenaz
title: Comment entraîner un modèle pour Windows ML dans Visual Studio
description: Apprenez à entraîner un modèle pour Windows ML à l’aide de Visual Studio Tools for AI grâce à ce tutoriel détaillé.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, apprentissage automatique Windows, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 5d8b50b6b8779b98de1d93f449aa560b5dcda893
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690215"
---
# <a name="how-to-train-a-model-for-windows-ml-in-visual-studio"></a>Comment entraîner un modèle pour Windows ML dans Visual Studio
Dans ce tutoriel, nous allons utiliser [Visual Studio Tools for AI](http://aka.ms/vstoolsforai), une extension de développement conçue pour la création, le test et le déploiement de solutions d'apprentissage profond et d'IA, afin d'entraîner un modèle pour l’exemple d’app MNIST dans [Prise en main](get-started.md).

Nous allons entraîner le modèle avec l'infrastructure [Microsoft Cognitive Toolkit (CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit) et le [jeu de données MNIST](http://yann.lecun.com/exdb/mnist/), qui comporte un ensemble de formations de 60000 exemples et un ensemble de tests de 10000 exemples de chiffres manuscrits. Nous enregistrerons ensuite le modèle au format [Open Neural Network Exchange (ONNX)](https://onnx.ai/) en vue d'une utilisation avec Windows ML. 

## <a name="prerequisites"></a>Prérequis
### <a name="install-visual-studio-tools-for-ai"></a>Installez Visual Studio Tools for AI
Pour commencer, vous devez télécharger et installer [Visual Studio](https://www.visualstudio.com/downloads/). Ouvrez Visual Studio, puis activez l'extension **Visual Studio Tools for AI**:

1. Cliquez sur la barre des menus dans Visual Studio et sélectionnez «Extensions et mises à jour...»
2. Cliquez sur l’onglet «En ligne» et sélectionnez «Rechercher Visual Studio Marketplace.»
3. Recherchez «Visual Studio Tools for AI». 
3. Cliquez sur le bouton **Télécharger**. 
4. Après l’installation, redémarrez Visual Studio. 

L’extension sera active une fois que Visual Studio aura redémarré. Si vous rencontrez des difficultés, consultez [Recherche des extensions Visual Studio](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions).

### <a name="download-sample-code"></a>Téléchargement de l’exemple de code
Téléchargez le référentiel [Exemples pour l'IA](https://github.com/Microsoft/samples-for-ai) sur GitHub. Les exemples abordent la prise en main du démarrage profond sur TensorFlow, CNTK, Theano etc.

### <a name="install-cntk"></a>Installer CNTK
Installez [CNTK pour Python sur Windows](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-windows-python?tabs=cntkpy24). Veuillez noter que vous devez également installer Python si vous ne l’avez pas déjà fait.

Pour préparer votre appareil au développement avec le modèle d'apprentissage profond, vous pouvez également consulter [Préparer votre environnement de développement](https://github.com/Microsoft/samples-for-ai/blob/master/README.md) pour obtenir un programme d’installation simplifiée de Python, CNTK, TensorFlow, des pilotes GPU NVIDIA (en option) et bien plus encore.

## <a name="1-open-project"></a>1. Ouvrez le projet

Lancez Visual Studio, puis sélectionnez **Fichier > Ouvrir > Projet/Solution**. Dans le référentiel Exemples pour l'IA, sélectionnez le dossier **examples\cntk\python** et ouvrez le fichier **CNTKPythonExamples.sln**.

![Ouvrez la solution](images/open-solution.png)

## <a name="2-train-the-model"></a>2. Entraîner le modèle

Pour définir le projet MNIST comme projet de démarrage, cliquez avec le bouton droit de la souris sur le projet python et sélectionnez **Définir comme projet de démarrage**.

![Ouvrez la solution](images/mnist-startup.png)

Ensuite, ouvrez le fichier ConvNet_MNIST.py et **exécutez** le projet en appuyant sur F5 ou sur le bouton Exécuter, en vert.

## <a name="3-view-the-model-and-add-it-to-your-app"></a>3. Afficher le modèle et l’ajouter à votre app

Ouvrez le dossier **output/Models** dans le référentiel Exemples pour l'IA. Un fichier de modèle DNN entraîné sera disponible pour chaque domaine de l'expérience d'entraînement, et l'étape finale écrira le fichier de modèle **MNIST.onnx**. 

![Ouvrez la solution](images/onnx-model-output.png)

Désormais, vous pouvez utiliser ce fichier de modèle **MNIST.onnx** entraîné pour générer l’exemple d’app MNIST [Prise en main](get-started.md)! 

## <a name="4-learn-more"></a>4. En savoir plus
Pour savoir comment accélérer l'entraînement des modèles d’apprentissage profond à l’aide des [machines virtuelles avec processeur graphique (GPU) Azure](https://docs.microsoft.com/en-us/visualstudio/ai/tensorflow-vm) et obtenir plus d'informations, visitez [Intelligence artificielle de Microsoft](https://www.microsoft.com/ai) et [Technologies d’apprentissage automatique Microsoft](https://docs.microsoft.com/en-us/azure/machine-learning/#More-Microsoft-Machine-Learning-Technologies).