---
author: serenaz
title: Prise en main de WindowsML
description: Créez votre première app Windows ML avec ce didacticiel pas à pas.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, apprentissage automatique windows, winml, windows ML
ms.localizationpriority: medium
ms.openlocfilehash: e30786f775a66bcf5c8e6dce0b4aab4f1f239be6
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816584"
---
# <a name="get-started-with-windows-ml"></a>Prise en main de WindowsML

Dans ce didacticiel, nous allons créer une simple app UWP qui utilise un modèle d’apprentissage machine entraîné à reconnaître un chiffre dessiné par l’utilisateur. Ce didacticiel se concentre principalement sur le chargement et l'utilisation de Windows ML dans votre app.

## <a name="prerequisites"></a>Éléments prérequis

- [SDK Windows - Build 17110+](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)
- [Visual Studio (Version 15.7 - Aperçu 1)](https://www.visualstudio.com/vs/preview/) 

    **Remarque**: dans Visual Studio Installer, vous devez désactiver le SDK de la version d’évaluation Windows10 optionnelle (10.0.17110.0).

## <a name="1-download-the-sample"></a>1. Télécharger l’exemple

Tout d’abord, vous devrez télécharger notre [exemple MNIST_GetStarted](https://github.com/Microsoft/Windows-Machine-Learning) depuis GitHub. Nous avons fourni un modèle avec des contrôles et des événements XAML implémentés, notamment:

- Un [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) pour dessiner le chiffre.
- [Buttons](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button) pour interpréter le chiffre et effacer la zone de dessin.
- Des routines d’assistance pour convertir le résultat InkCanvas en un [VideoFrame ](https://docs.microsoft.com/uwp/api/windows.media.videoframe).

Un exemple MNIST complet est également disponible au téléchargement dans GitHub.

## <a name="2-open-project-in-visual-studio-preview"></a>2. Ouvrez votre projet dans la version d'évaluation Visual Studio.

Lancez la version d'évaluation Visual Studio Preview et ouvrez l’exemple d'application MNIST. (Si la solution s'affiche comme non disponible, vous devrez cliquer avec le bouton droit de la souris et sélectionnez «Recharger le projet».)

Dans l’explorateur de solutions, le projet comporte trois fichiers de code principaux:

- `MainPage.xaml` -Tous nos code XAML pour créer l’interface utilisateur pour l’élément InkCanvas, les boutons et les étiquettes.
- `MainPage.xaml.cs` - Où réside notre code d’application.
- `Helper.cs` - Des routines d’assistance pour rogner et convertir les formats d’image.

![Explorateur de solutions Visual Studio pour des fichiers de projet](images/get-started1.png)

## <a name="3-build-and-run-project"></a>3. Générez et exécutez le projet

Dans la barre d’outils Visual Studio, modifiez la plateforme de la solution, depuis «ARM» à «x64» pour exécuter le projet sur votre ordinateur local.

Pour exécuter le projet, cliquez sur le bouton **Start Debugging** bouton sur la barre d’outils, ou appuyez sur F5. L’application doit afficher un InkCanvas où les utilisateurs peuvent écrire un chiffre, un bouton «Reconnaître» pour interpréter ce chiffre, un champ d’étiquette vide où s'affichera le chiffre interprété sous forme de texte et un «Effacer le chiffre» pour effacer l’InkCanvas.

![Capture d’écran de l’application](images/get-started2.png)

**Remarque**: si vous avez téléchargé une version supérieure à 17110, vous devrez peut-être modifier la version cible du déploiement de projet. Sous la solution du projet, accédez à **Propriétés**. Dans l’onglet Application, définissez la version cible pour faire correspondre votre système d’exploitation au kit de développement logiciel.

## <a name="4-download-a-model"></a>4. Télécharger un modèle

Ensuite, nous allons ajouter un modèle d’apprentissage automatique à notre app. Pour ce didacticiel, nous allons utiliser un **modèle MNIST** préalablement entraîné avec [MicrosoftCognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) et [exporté au format ONNX](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb).

Si vous utilisez l’exemple de MNIST_GetStarted à partir de GitHub, le modèle MNIST a déjà été inclus dans votre dossier Actifs, et vous devez l’ajouter à votre application comme un élément existant. Vous pouvez également télécharger le modèle préalablement entraîné à partir du [modèle ONNX Zoo](https://github.com/onnx/models) sur GitHub.

Si vous souhaitez entraîner votre propre modèle, vous pouvez suivre ce [didacticiel](train-ai-model.md), que nous avons utilisé pour entraîner ce modèle MNIST.

## <a name="5-add-the-model"></a>5. Ajouter le modèle

Après avoir téléchargé le modèle MNIST, cliquez avec le bouton droit de la souris sur le dossier Actifs dans l’Explorateur de solutions, puis sélectionnez «**Ajouter** > **Élément existant**». Pointez le sélecteur de fichiers à l’emplacement de votre modèle ONNX et cliquez sur Ajouter. 

Le projet doit désormais comporter deux nouveaux fichiers:

- `MNIST.onnx` - votre modèle entraîné.
- `MNIST.cs` - le code Windows ML généré.

![Explorateur de solutions avec les nouveaux fichiers](images/get-started3.png)

Pour vous assurer que le modèle se construit lorsque nous compilons notre application, cliquez avec le bouton droit de la souris sur le fichier `mnist.onnx`, puis sélectionnez **Propriétés**. Pour **Action de génération**, sélectionnez **Contenu**.

À présent, examinons le code qui vient d’être généré dans le fichier `MNIST.cs`. Nous avons trois classes:

- **MNISTModel** crée la représentation du modèle d'apprentissage automatique, associe les entrées et sorties spécifiques au modèle et évalue le modèle de manière asynchrone. 
- **MNISTModelInput** initialise les types d’entrée prévus par le modèle. Dans ce cas, l’entrée prévoit un VideoFrame.
- **MNISTModelOutput** initialise les types de sortie générés par le modèle. Dans ce cas, la sortie sera une liste appelée «classLabel» de type `<long>`et un dictionnaire appelé «prediction» de type `<long, float>`

Nous allons désormais utiliser ces classes pour charger, associer et évaluer le modèle dans notre projet.

## <a name="6-load-bind-and-evaluate-the-model"></a>6. Charger, associer et évaluer le modèle

Pour les applications Windows ML, le modèle à suivre sera le suivant: Charger > Associer > Évaluer.

- Chargez le modèle d’apprentissage automatique.
- Associer des entrées et sorties au modèle.
- Évaluer le modèle et afficher les résultats.

Nous allons utiliser le code d’interface généré dans `MNIST.cs`pour charger, associer et évaluer le modèle dans notre app.

En premier lieu, nous allons instancier le modèle, les entrées et les sorties dans `MainPage.xaml.cs`.

```csharp
namespace MNIST_Demo
{
    public sealed partial class MainPage : Page
    {
        private MNISTModel ModelGen = new MNISTModel();
        private MNISTModelInput ModelInput = new MNISTModelInput();
        private MNISTModelOutput ModelOutput = new MNISTModelOutput();
        ...
    }
}
```

Ensuite, dans LoadModel(), nous allons charger le modèle.

```csharp
private async void LoadModel()
{
    //Load a machine learning model
    StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
    ModelGen = await MNISTModel.CreateMNISTModel(modelFile);
}
```

Ensuite, nous allons associer des entrées et des sorties au modèle. 

Dans ce cas, notre modèle attend une entrée de type VideoFrame. À l’aide de nos fonctions d'assistance intégrées dans `helper.cs`, nous allons copier les contenus du InkCanvas, les convertir au type VideoFrame et les associer à notre modèle.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
     //Bind model input with contents from InkCanvas
     ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
}
```

Concernant la sortie, nous appellerons simplement EvaluateAsync() avec l’entrée spécifiée. Dans la mesure où le modèle renvoie une liste de chiffres avec une probabilité correspondante, nous devons analyser la liste retournée pour déterminer quel chiffre présente la probabilité la plus élevée afin de l'afficher.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
    //Bind model input with contents from InkCanvas
    ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
    
    //Evaluate the model
    ModelOutput = await ModelGen.EvaluateAsync(ModelInput);
            
    //Iterate through evaluation output to determine highest probability digit
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
}
```

Enfin, nous effacerons l'InkCanvas pour permettre aux utilisateurs de dessiner un autre chiffre.

```csharp
private void clearButton_Click(object sender, RoutedEventArgs e)
{
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    numberLabel.Text = "";
}
```

## <a name="7-launch-the-app"></a>7. Lancez l’app

Une fois que nous aurons créé et lancé l’app, nous serons en mesure de reconnaître un numéro dessiné sur l'InkCanvas.

![app complète](images/get-started4.png)

## <a name="8-next-steps"></a>8. Étapes suivantes

Et voilà, vous avez créé votre première app Windows ML! Pour plus d’exemples montrant comment utiliser Windows ML, consultez [Exemples d’apps](samples.md).