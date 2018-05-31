---
author: serenaz
title: Intégrer un modèle dans votre app avec WindowsML
description: Intégrer un modèle dans votre app en suivant le modèle de charge, de liaison et d'évaluation.
ms.author: sezhen
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, winml, apprentissage automatique Windows
ms.localizationpriority: medium
ms.openlocfilehash: 87454c46a08b80b8a315ede1d2e6a1f6cda909df
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815474"
---
# <a name="integrate-a-model-into-your-app-with-windows-ml"></a>Intégrer un modèle dans votre app avec WindowsML

La fonction de [génération de code automatique](overview.md#automatic-interface-code-generation) de Windows ML crée une interface dénommée [API WindowsML](/uwp/api/windows.ai.machinelearning.preview) pour vous, ce qui vous permet d'interagir aisément avec votre modèle. À l’aide des classes de wrapper générées par l’interface, vous suivrez le modèle de chargement, de liaison et d'évaluation pour intégrer votre modèle ML à votre app.

![charger, lier, évaluer](images/load-bind-evaluate.png)

Dans cet article, nous allons utiliser le modèle MNIST de l'article [Prise en main](get-started.md) pour montrer comment charger, lier et évaluation un modèle dans votre app.

## <a name="add-the-model"></a>Ajouter le modèle

Tout d’abord, vous devez ajouter votre modèle ONNX aux ressources de votre projet Visual Studio. Si vous créez une app UWP avec [Visual Studio (version 15.7 - Aperçu 1)](https://www.visualstudio.com/vs/preview/), Visual Studio générera automatiquement les classes de wrapper dans un nouveau fichier. Pour les autres flux de travail, vous devez utiliser l'outil [mlgen](overview.md#automatic-interface-code-generation)afin de générer des classes de wrapper.

Vous trouverez ci-dessous les classes de wrapper Windows ML générées pour le modèle MNIST. Le reste de cet article explique comment utiliser ces classes dans votre app.

```csharp
public sealed class MNISTModelInput
{
    public VideoFrame Input3 { get; set; }
}

public sealed class MNISTModelOutput
{
    public IList<float> Plus214_Output_0 { get; set; }
    public MNISTModelOutput()
    {
        this.Plus214_Output_0 = new List<float>();
    }
}

public sealed class MNISTModel
{
    private LearningModelPreview learningModel;
    public static async Task<MNISTModel> CreateMNISTModel(StorageFile file)
    {
        LearningModelPreview learningModel = await LearningModelPreview.LoadModelFromStorageFileAsync(file);
        MNISTModel model = new MNISTModel();
        model.learningModel = learningModel;
        return model;
    }
    public async Task<MNISTModelOutput> EvaluateAsync(MNISTModelInput input) {
        MNISTModelOutput output = new MNISTModelOutput();
        LearningModelBindingPreview binding = new LearningModelBindingPreview(learningModel);
        binding.Bind("Input3", input.Input3);
        binding.Bind("Plus214_Output_0", output.Plus214_Output_0);
        LearningModelEvaluationResultPreview evalResult = await learningModel.EvaluateAsync(binding, string.Empty);
        return output;
    }
}
```

**Remarque**: afin de vous assurer que le modèle se construit lorsque vous compilez votre application, cliquez avec le bouton droit de la souris sur le fichier `.onnx`, puis sélectionnez **Propriétés**. Pour **Action de génération**, sélectionnez **Contenu**.

## <a name="load"></a>Charger

Une fois que vous avez généré les classes de wrapper, vous devez charger le modèle dans votre app.

La classe MNISTModel représente le modèle MNIST, et pour charger ce modèle, nous appelons la méthode CreateMNISTModel, en communiquant le fichier ONNX comme paramètre.

```csharp
// Load the model
StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
MNISTModel model = MNISTModel.CreateMNISTModel(modelFile);
```

## <a name="bind"></a>Lier

Le code généré inclut également des classes de wrapper d’entrée et de sortie. La classe d’entrée représente les entrées attendue du modèle, et la classe de sortie, les sorties attendues du modèle.

Pour initialiser l’objet d’entrée du modèle, appelez le constructeur de classe d’entrée, en communiquant vos données d’application et assurez-vous que vos données d’entrée correspondent au type d’entrée attendu par votre modèle. La classe MNISTModelInput attend un VideoFrame, par conséquent nous utilisons une méthode d'assistance pour saisir un VideoFrame en tant qu'entrée.

```csharp
//Bind the input data to the model
MNISTModelInputs ModelInput = new MNISTModelInputs();
ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
```

Les objets de sortie sont initialisés avec une valeur *Null* et seront mis à jour avec les résultats du modèle après l’étape suivante, Évaluer.

## <a name="evaluate"></a>Évaluation

Une fois que vos entrées sont initialisées, appelez la méthode EvaluateAsync du modèle pour évaluer votre modèle sur les données d’entrée. EvaluateAsync lie vos entrées et sorties à l’objet du modèle et évalue le modèle sur les entrées.

```csharp
// Evaluate the model
MNISTModelOuput ModelOutput = model.EvaluateAsync(ModelInput);
```

Après l’évaluation, la sortie contient des résultats du modèle, que vous pouvez désormais consulter et analyser. Dans la mesure où le modèle MNIST retourne une liste de probabilités, nous parcourons cette dernière pour rechercher et afficher le chiffre doté de la plus haute probabilité.

```csharp
 //Iterate through output to determine highest probability digit
float maxProb = 0;
int maxIndex = 0;
for (int i = 0; i < 10; i++)
{
    if (ModelOutput.Plus214_Output_0[i] > maxProb)
    {
        maxIndex = i;
        maxProb = ModelOutput.Plus214_Output_0[i];
    }
}
numberLabel.Text = maxIndex.ToString();
```

## <a name="using-the-windows-ml-apis-directly"></a>Utiliser les API Windows ML directement

Bien que le générateur de code automatique de Windows ML fournisse une interface conviviale pour interagir avec votre modèle, vous n’êtes pas obligé d'utiliser les classes de wrapper. Au lieu de cela, vous pouvez appeler des API Windows ML directement dans votre app.
Si vous optez pour cette solution, vous suivez la même procédure: charger votre modèle, lier vos entrées et sorties et évaluer.

Pour plus d’informations sur l’utilisation des API, consultez la [Référence API Windows ML](/uwp/api/windows.ai.machinelearning.preview).
