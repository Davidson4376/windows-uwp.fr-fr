---
author: wschin
title: Convertir les modèles existants ML au format ONNX
description: Des exemples de code montrent comment utiliser WinMLTools pour convertir des modèles existants dans scikit-learn et Core ML au format ONNX.
ms.author: sezhen
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, apprentissage automatique windows, WinML, WinMLTools, ONNX, ONNXMLTools, scikit-learn, Core ML
ms.localizationpriority: medium
ms.openlocfilehash: 882efca26730c990093a89a5ed3ff4b5587e05bf
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832677"
---
# <a name="convert-existing-ml-models-to-onnx"></a>Convertir les modèles existants ML au format ONNX

[WinMLTools](https://aka.ms/winmltools) permet aux utilisateurs de convertir des modèles entraînés dans d’autres infrastructures au format ONNX. Nous expliquons ici comment installer le package WinMLTools et convertir des modèles existants dans scikit-learn et Core ML au format ONNX via le code Python.

## <a name="install-winmltools"></a>Installer WinMLTools

WinMLTools est un package Python (winmltools) qui prend en charge les versions 2.7 et 3.6. de Python. Si vous travaillez sur un projet scientifique de données, nous vous recommandons d’installer une distribution Python scientifique, comme Anaconda.

WinMLTools suit le [processus d’installation du package python standard](https://packaging.python.org/installing/). À partir de votre environnement python, exécutez:

```
pip install winmltools
```

WinMLTools présente les dépendances suivantes:

- numpy v1.10.0+
- onnxmltools 0.1.0.0+
- protobuf v.3.1.0+

Pour mettre à jour les packages dépendants, exécutez la commande pip avec l'argument «‑U».

```
pip install -U winmltools
```

Veuillez suivre [onnxmltools](https://github.com/onnx/onnxmltools) sur GitHub pour plus d’informations sur les dépendances onnxmltools.

Vous trouverez davantage d’informations sur l’utilisation de WinMLTools en utilisant la fonction d'aide de la documentation spécifique du package.

```
help(winmltools)
```

## <a name="scikit-learn-models"></a>Modèles scikit-learn

L’extrait de code suivant entraîne une machine à vecteur linéaire dans scikit-learn et convertit le modèle au format ONNX.

~~~python
# First, we create a toy data set.
# The training matrix X contains three examples, with two features each.
# The label vector, y, stores the labels of all examples.
X = [[0.5, 1.], [-1., -1.5], [0., -2.]]
y = [1, -1, -1]

# Then, we create a linear classifier and train it.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()
linear_svc.fit(X, y)

# To convert scikit-learn models, we need to specify the input feature's name and type for our converter. 
# The following line means we have a 2-D float feature vector, and its name is "input" in ONNX.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
linear_svc_onnx = convert_sklearn(linear_svc, name='LinearSVC', input_features=[('input', 'float', 2)])    

# Now, we save the ONNX model into binary format.
from winmltools.utils import save_model
save_model(linear_svc_onnx, 'linear_svc.onnx')

# If you'd like to load an ONNX binary file, our tool can also help.
from winmltools.utils import load_model
linear_svc_onnx = load_model('linear_svc.onnx')

# To see the produced ONNX model, we can print its contents or save it in text format.
print(linear_svc_onnx)
from winmltools.utils import save_text
save_text(linear_svc_onnx, 'linear_svc.txt')

# The conversion of linear regression is very similar. See the example below.
from sklearn.svm import LinearSVR
linear_svr = LinearSVR()
linear_svr.fit(X, y)
linear_svr_onnx = convert_sklearn(linear_svr, name='LinearSVR', input_features=[('input', 'float', 2)])   
~~~

Les utilisateurs peuvent remplacer `LinearSVC` par d’autres modèles scikit-learn, tels que `RandomForestClassifier`. Veuillez noter que le [générateur de code automatique](overview.md#automatic-interface-code-generation) utilise le paramètre `name` pour générer des noms de classe et des variables. Si `name`n’est pas fourni, un GUID sera généré, lequel ne satisfera pas aux conventions d’affectation de noms variables pour des langues telles que C++ / C#. 

## <a name="scikit-learn-pipelines"></a>Pipelines scikit-learn

Ensuite, nous montrons comment les pipelines scikit-learn peuvent être converties au format ONNX.

~~~python
# First, we create a toy data set.
# Notice that the first example's last feature value, 300, is large.
X = [[0.5, 1., 300.], [-1., -1.5, -4.], [0., -2., -1.]]
y = [1, -1, -1]

# Then, we declare a linear classifier.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()

# One common trick to improve a linear model's performance is to normalize the input data.
from sklearn.preprocessing import Normalizer
normalizer = Normalizer()

# Here, we compose our scikit-learn pipeline. 
# First, we apply our normalization. 
# Then we feed the normalized data into the linear model.
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(normalizer, linear_svc)
pipeline.fit(X, y)

# Now, we convert the scikit-learn pipeline into ONNX format. 
# Compared to the previous example, notice that the specified feature dimension becomes 3.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
pipeline_onnx = convert_sklearn(linear_svc, name='NormalizerLinearSVC', input_features=[('input', 'float', 3)])   

# We can print the fresh ONNX model.
print(pipeline_onnx)

# We can also save the ONNX model into binary file for later use.
from winmltools.utils import save_model
save_model(pipeline_onnx, 'pipeline.onnx')

# Our conversion framework provides limited support of heterogeneous feature values.
# We cannot have numerical types and string type in one feature vector. 
# Let's assume that the first two features are floats and the last feature is integer (encoded a categorical attribute).
X_heter = [[0.5, 1., 300], [-1., -1.5, 400], [0., -2., 100]]

# One popular way to represent categorical is one-hot encoding.
from sklearn.preprocessing import OneHotEncoder
one_hot_encoder = OneHotEncoder(categorical_features=[2])

# Let's initialize a classifier. 
# It will be right after the one-hot encoder in our pipeline.
linear_svc = LinearSVC()

# Then, we form a two-stage pipeline.
another_pipeline = make_pipeline(one_hot_encoder, linear_svc)
another_pipeline.fit(X_heter, y)

# Now, we convert, save, and load the converted model. 
# For heterogeneous feature vectors, we need to seperately specify their types for all homogeneous segments.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
another_pipeline_onnx = convert_sklearn(another_pipeline, name='OneHotLinearSVC', input_features=[('input', 'float', 2), ('another_input', 'int64', 1)])
save_model(another_pipeline_onnx, 'another_pipeline.onnx')
from winmltools.utils import load_model
loaded_onnx_model = load_model('another_pipeline.onnx')

# Of course, we can print the ONNX model to see if anything went wrong.
print(another_pipeline_onnx)
~~~

## <a name="core-ml-models"></a>Modèles Core ML

Ici, nous partons du principe que le chemin d’accès d’un exemple de fichier de modèle ML Core est *example.mlmodel *.

~~~python
from coremltools.models.utils import load_spec
# Load model file
model_coreml = load_spec('example.mlmodel')
from winmltools import convert_coreml
# Convert it!
# The automatic code generator (mlgen) uses the name parameter to generate class names.
model_onnx = convert_coreml(model_coreml, name='ExampleModel')   
~~~

`model_onnx` est un objet ONNX [ModelProto](https://github.com/onnx/onnxmltools/blob/0f453c3f375c1ae928b83a4c7909c82c013a5bff/onnxmltools/proto/onnx-ml.proto#L176). Nous pouvons l’enregistrer dans deux formats différents.

~~~python
from winmltools.utils import save_model
# Save the produced ONNX model in binary format
save_model(model_onnx, 'example.onnx')
# Save the produced ONNX model in text format
from winmltools.utils import save_text
save_text(model_onnx, 'example.txt')
~~~

**Remarque**: CoreMLTools est un package Python fourni par Apple, mais celui-ci n’est pas disponible sur Windows. Si vous devez installer le package sur Windows, installez le package directement à partir du référentiel:

```
pip install git+https://github.com/apple/coremltools
```

## <a name="core-ml-models-with-image-inputs-or-outputs"></a>Modèles Core ML avec entrées ou sorties d'image

En raison du manque de types d’image dans ONNX, la conversion des modèles d’image Core ML (par exemple, les modèles utilisant des images en tant qu’entrées ou sorties) requiert quelques étapes de prétraitement et de post-traitement.

L’objectif du prétraitement est de vous assurer que l’image d’entrée est formatée correctement en tant que tenseur ONNX. Supposons que *X* est une entrée d'image avec la forme [C, H, W] dans Core ML. Dans ONNX, la variable *X* serait un tenseur à virgule flottante avec la même forme et *X*[0,:,:]/*X*[1,:,:]/*X*[2,:,:] stocke canal vert/rouge/bleu de l’image. Pour les images à nuances de gris dans Core ML, leur format sont des tenseurs [1 H, W] dans ONNX, car nous n'avons qu'un canal.

Si le modèle d’origine Core ML génère une image, convertissez manuellement les tenseurs de sortie à virgule flottante d'ONNX en images. Deux étapes basiques doivent être effectuées. La première étape consiste à tronquer des valeurs supérieures à 255 pour les définir sur 255, et de modifier toutes les valeurs négatives pour les définir sur 0. La deuxième étape consiste à arrondir toutes les valeurs de pixels à des nombres entiers (en ajoutant 0,5, puis en tronquant les décimales). L’ordre de canal de sortie (par exemple, RGB ou BGR) est indiqué dans le modèle Core ML. Pour générer la sortie d’image appropriée, nous pouvons être amené à transposer ou à activer la lecture aléatoire pour récupérer le format souhaité.

Ici, nous considérons un modèle Core ML, FNS-Candy, téléchargé à partir de [GitHub](https://github.com/likedan/Awesome-CoreML-Models), comme un exemple de conversion concret pour illustrer la différence entre les formats ONNX et Core ML. Veuillez noter que toutes les commandes suivantes sont exécutées dans un environnement python.

Tout d’abord, nous chargeons le modèle Core ML et examinons ses formats d’entrée et de sortie.

~~~python
from coremltools.models.utils import load_spec
model_coreml = load_spec('FNS-Candy.mlmodel')
model_coreml.description # Print the content of Core ML model description
~~~

Sortie de l’écran:

~~~
...
input {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
output {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
~~~

Dans ce cas, l’entrée et la sortie sont tous deux une image BGR 720 x 720. L’étape suivante consiste à convertir le modèle Core ML avec WinMLTools.

~~~python
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from onnxmltools import convert_coreml
model_onnx = convert_coreml(model_coreml, name='FNSCandy')    
~~~

Vous pouvez également utiliser la commande suivante pour afficher les formats d'entrée et de sortie du modèle dans ONNX:

~~~python
model_onnx.graph.input # Print out the ONNX input tensor's information
~~~

Sortie de l’écran:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

L’entrée produite (indiquée par *X*) dans ONNX est un tenseur 4-D. Les 3derniers axes sont C, H et W, respectivement. La première dimension est «None» ce qui signifie que ce modèle ONNX peut être appliqué à n’importe quel nombre d’images. Pour appliquer ce modèle afin de traiter un lot de 2images, la première image correspond à *X*[0,:,:,:] tandis que *X*[1,:,:,:] correspond à la seconde image. Les canaux bleu/vert/rouge de la première image sont *X*[0, 0,:,:]/*X*[0, 1,:,:]/*X*[0, 2,:,:], comme pour la seconde image.

~~~python
model_onnx.graph.output # Print out the ONNX output tensor's information
~~~

Sortie de l’écran:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

Comme vous pouvez le constater, le format produit est identique au format d’entrée du modèle d’origine. Toutefois, dans ce cas, il ne s'agit pas d'une image, car les valeurs de pixel sont des nombres entiers, et non pas des nombres à virgule flottante. Pour une nouvelle conversion vers une image, tronquez les valeurs supérieures à 255 pour les définir sur 255, remplacez les valeurs négatives par 0 et arrondissez tous les nombres décimaux à des chiffres entiers.