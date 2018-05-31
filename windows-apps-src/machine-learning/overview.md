---
author: serenaz
title: Vue d’ensemble de WindowsML
description: Apprenez-en davantage sur l'apprentissage automatique Windows et sur le développement avec Windows ML
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, apprentissage automatique windows
ms.localizationpriority: medium
ms.openlocfilehash: 2ef6ea1a4e1dab23f5ff6a09aec9b8c49c135f5e
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817660"
---
# <a name="windows-ml-overview"></a>Vue d’ensemble de WindowsML

![Graphisme d'apprentissage automatique Windows](images/brain.png)

## <a name="what-is-machine-learning"></a>Qu'est-ce que l'apprentissage automatique?

L’apprentissage automatique (Machine Learning, ML) permet aux ordinateurs d’utiliser des données existantes pour prédire les comportements et les résultats attendus. En traitant les données recueillies au préalable, les algorithmes ML créent des modèles capables de prédire la sortie appropriée, en fonction de la nouvelle entrée reçue. Par exemple, un modèle peut être entraîné pour déterminer si des e-mails (entrée) représentent du courrier indésirable ou non (sortie).

La phase de création di modèle est appelée «entraînement». Une fois entraîné avec les données existantes, le modèle peut formuler des prévisions avec de nouvelles données qui étaient non visibles jusqu'à présent, ce qui s'appelle «inférence», «évaluation» ou «notation».

Les modèles entraînés produisent souvent de meilleurs résultats que les programmes conçus pour suivre un ensemble d'instructions strict, en particulier pour les tâches complexes dotées de plusieurs combinaisons possibles d'entrées et de sorties. Par exemple, les algorithmes de recommandation fournissent des recommandations personnalisées à des millions d’utilisateurs sur des sites d'e-commerce et de diffusion multimédia en continu, ce qui serait quasiment impossible sans l'apprentissage automatique. La prise de vue des ordinateurs est un autre domaine qui utilise l'apprentissage automatique, et permet aux ordinateurs de classer et d'identifier des images après un entraînement avec des images précédemment étiquetées.

Les possibilités et les applications d’apprentissage automatique sont illimitées; pour en savoir plus sur la recherche et les solutions dans ce domaine, visitez [Intelligence artificielle de Microsoft](https://www.microsoft.com/ai) et la [Plateforme MicrosoftIA](https://azure.microsoft.com/en-us/overview/ai-platform/). Si vous souhaitez créer des modèles d'apprentissage automatique et d'intelligence artificielle, vous pouvez également consulter les [services Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/overview-what-is-azure-ml).

## <a name="what-is-windows-ml"></a>Qu’est-ce que Windows ML?

Windows ML est une plateforme qui évalue les modèles d’apprentissage automatique entraînés sur les appareils Windows10, ce qui permet aux développeurs d’utiliser l’apprentissage automatique dans leurs applications Windows.

Voici quelques points essentiels sur Windows ML:

- **Accélération matérielle**
    
    Sur les appareils compatibles DirectX12, Windows ML accélère l’évaluation des modèles d'apprentissage profond à l’aide du GPU. En outre, les optimisations de l'unité centrale (UC) permettent d'activer l'évaluation hautes performances des algorithmes ML classiques et d'apprentissage profond.

- **Évaluation locale**

    Windows ML évalue le matériel local, éliminant ainsi tout problème de connectivité, de bande passante et de confidentialité des données. L'évaluation locale permet également d'assurer une faible latence et de hautes performances pour obtenir rapidement des résultats d'évaluation.

- **Traitement d’images**

    Pour les scénarios de prise de vue d’ordinateur, Windows ML simplifie et optimise l’utilisation des données d’image, vidéo et d'appareil photo en gérant le prétraitement de la trame et en assurant la mise en place de l'infrastructure en vue de la saisie du modèle.

## <a name="how-to-develop-with-windows-ml"></a>Comment développer avec Windows ML

> [!VIDEO https://www.youtube.com/embed/8MCDSlm326U]

### <a name="system-requirements"></a>Configuration requise

Pour créer des applications qui utilisent Windows ML, vous aurez besoin du [SDK Windows - Build17110 ou version ultérieure](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).

### <a name="onnx-models"></a>Modèles ONNX

Pour utiliser Windows ML, vous aurez besoin d’un modèle d'apprentissage automatique préalablement entraîné au format [Open Neural Network Exchange (ONNX)](https://onnx.ai). Windows ML prend en charge la versionv1.0 du format ONNX, ce qui permet aux développeurs d’utiliser des modèles produits par différentes infrastructures d'entraînement.

Pour obtenir la liste des modèles ONNX publiquement disponibles, consultez [Modèles ONNX](https://github.com/onnx/models) sur GitHub.

Pour savoir comment entraîner un modèle ONNX avec Visual Studio Tools for AI, consultez [Entraîner un modèle ](train-ai-model.md).

### <a name="convert-existing-models-to-onnx"></a>Convertir les modèles existants ONNX

De nombreuses infrastructures d'entraînement prennent déjà en charge les modèles ONNX, et il existe des outils de convertisseur pour de nombreuses infrastructures et bibliothèques. Pour savoir comment exporter à partir d’infrastructures telles que Caffe 2, PyTorch, CNTK, Chainer, etc., consultez [Didacticiels ONNX](https://github.com/onnx/tutorials) sur GitHub.

Vous pouvez également utiliser [WinMLTools](https://pypi.org/project/winmltools/) pour convertir le modèle d’apprentissage automatique entraîné au format ONNX accepté par Windows ML. WinMLTools prend en charge la conversion des formats suivants:

- Core ML
- Scikit-Learn
- XGBoost
- LibSVM

Pour savoir comment installer et utiliser WinMLTools, consultez [Convertir un modèle](conversion-samples.md).

### <a name="onnx-operators"></a>Opérateurs ONNX

Windows ML prend en charge plus de 100 opérateurs ONNX sur l'UC et accélère les opérations de calcul sur les GPU compatibles DirectX12. Pour obtenir la liste complète des signatures d’opérateur, consultez la documentation des schémas des opérateurs ONNX pour les espaces de noms [ai.onnx](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators.md) (par défaut) et [ai.onnx.ml](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators-ml.md).

Windows ML prend en charge tous les opérateurs définis dans la documentation ONNX v1.0 avec les différences suivantes:

- Opérateurs marqués «expérimental» pris en charge par Windows ML:
    - Affine
    - Crop
    - FC
    - Identity
    - ImageScaler
    - MeanVarianceNormalization
    - ParametricSoftplus
    - ScaledTanh
    - ThresholdedRelu
    - Upsample
- MatMul - les produits de multiplication des matrices supérieures à 2D ne sont pas actuellement pris en charge, prise en charge uniquement sur l'UC.
- Cast - pris en charge uniquement sur l'UC
- Les opérateurs suivants ne sont pas pris en charge actuellement:
    - RandomUniform
    - RandomUniformLike
    - RandomNormal
    - RandomNormalLike

### <a name="automatic-interface-code-generation"></a>Génération de code d’interface automatique

Avec un fichier de modèle ONNX, le générateur de code Windows ML crée une interface pour interagir avec le modèle dans votre app. L’interface générée inclut les classes de wrapper qui représentent le modèle, les entrées et les sorties. Le code généré appelle l'[API WindowsML](/uwp/api/windows.ai.machinelearning.preview) pour vous, ce qui vous permet de charger, lier et évaluer aisément le modèle dans votre projet. Le générateur de code prend actuellement en charge C# et C++/CX.

Pour les développeurs UWP, le générateur de code automatique de Windows ML est intégré en mode natif à [Visual Studio (version 15.7 - Aperçu 1)](https://www.visualstudio.com/vs/preview/). (**Remarque**: dans Visual Studio Installer, vous devrez désactiver l’option Kit de développement logiciel (SDK)Windows10Anniversary, Build 17110. Dans votre projet Visual Studio, ajoutez simplement votre fichier ONNX en tant qu'élément existant, et Visual Studio générera des classes de wrapper Windows ML dans un nouveau fichier d'interface.

Vous pouvez également utiliser l’outil de ligne de commande `mlgen.exe` fourni avec le SDK Windows, pour générer les classes de wrapper Windows ML. L’outil se trouve dans `(SDK_root)\bin\<version>\x64`ou `(SDK_root)\bin\<version>\x86`, SDK_root étant le répertoire d’installation SDK. Pour exécuter l'outil, utilisez la commande ci-dessous.

```
mlgen -i INPUT-FILE -l LANGUAGE -n NAMESPACE [-o OUTPUT-FILE]
```

Saisissez la définition des paramètres:

- `INPUT-FILE`: le fichier de modèle ONNX
- `LANGUAGE`: CPPCX ou CS
- `NAMESPACE`: l'espace de noms du code généré
- `OUTPUT-FILE`: le chemin d'accès où le code généré sera écrit. Si FICHIER DE SORTIE n’est pas spécifié, le code généré sera écrit dans la sortie standard

Pour savoir comment utiliser le code généré dans votre app, voir [Intégrer un modèle](integrate-model.md).

## <a name="next-steps"></a>Étapes suivantes

Essayez de créer votre première app Windows ML en suivant un didacticiel pas à pas dans [Prise en main](get-started.md).