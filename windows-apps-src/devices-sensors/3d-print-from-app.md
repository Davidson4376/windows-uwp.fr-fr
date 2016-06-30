---
author: PatrickFarley
title: "Impression 3D à partir de votre application"
description: "Découvrez comment ajouter des fonctionnalités d’impression 3D à votre application Windows universelle. Cette rubrique explique comment lancer la boîte de dialogue d’impression 3D après s’être assuré que votre modèle 3D est imprimable et au format qui convient."
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
translationtype: Human Translation
ms.sourcegitcommit: 61d9f5c1fca1ad2e26f052b901361813975ae357
ms.openlocfilehash: e68a9c681974152bc0d4dfa58e824f80e77dc51f

---

# Impression 3D à partir de votre application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

Découvrez comment ajouter des fonctionnalités d’impression 3D à votre application Windows universelle. Cette rubrique explique comment charger des données géométriques 3D dans votre application et lancer la boîte de dialogue d’impression 3D après s’être assuré que votre modèle 3D est imprimable et au format qui convient. Pour obtenir un exemple de mise en pratique de ces procédures, consultez [l’exemple d’impression 3D UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

## Configuration de la classe


Dans la classe devant recevoir les fonctionnalités d’impression 3D, ajoutez l’espace de noms [Windows.Graphics.Printing3D](https://msdn.microsoft.com/library/windows/apps/dn998169).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

Les espaces de noms supplémentaires suivants seront utilisés dans ce guide :

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Ensuite, attribuez à votre classe quelques champs de membre utiles. Déclarez un objet [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) qui servira de référence à la tâche d’impression qui doit être transmise au pilote d’impression. Déclarez un objet [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) destiné à contenir le fichier de données 3D d’origine. Enfin, déclarez un objet [Printing3D3MFPackage](https://msdn.microsoft.com/library/windows/apps/dn998063), qui représente un modèle 3D prêt pour l’impression doté de toutes les métadonnées nécessaires.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## Créer une interface utilisateur simple


Cet exemple utilise trois contrôles utilisateur : un bouton de chargement qui place un fichier dans la mémoire du programme, un bouton de correction qui modifie le fichier si nécessaire et un bouton d’impression qui lance une tâche d’impression. Le code suivant permet de générer ces boutons (avec leurs gestionnaires d’événements Click) dans le fichier XAML de votre classe :

[!code-xml[Boutons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Ajoutez un **TextBlock** pour le commentaire de l’interface utilisateur.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]

## Obtenir les données 3D


La méthode par laquelle votre application acquiert les données de géométrie 3D à imprimer varie. Votre application peut récupérer les données à partir d’une analyse 3D, extraire les données d’une ressource web ou générer un maillage 3D par programme à l’aide d’équations. Par souci de simplicité, ce guide chargera un fichier de données 3D (d’un type de fichier courant) dans la mémoire du programme à partir de l’Explorateur de fichiers.

Dans votre méthode `OnLoadClick`, utilisez la classe [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) pour charger un seul fichier dans la mémoire de votre application.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## Utiliser 3D Builder pour une conversion au format 3D Manufacturing Format (.3mf)

À ce stade, vous êtes en mesure de charger un fichier de données 3D dans la mémoire de votre application. Il existe de nombreux formats de données de géométrie 3D, mais ils ne sont pas tous adaptés pour l’impression 3D. Windows 10 utilise le type de fichier 3D Manufacturing Format (.3mf) pour toutes les tâches d’impression 3D.

> **Remarque** Le type de fichier 3MF offre de nombreuses fonctionnalités qui ne font pas l’objet de ce tutoriel. Pour en savoir plus sur le format 3MF et les fonctionnalités proposées aux fabricants et clients de produits 3D, reportez-vous aux [caractéristiques 3MF](http://3mf.io/what-is-3mf/3mf-specification/). Pour savoir comment tirer parti de ces fonctionnalités à l’aide des API Windows 10, consultez le didacticiel [Générer un package 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

Heureusement, l’application [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) peut ouvrir les formats de fichier 3D les plus courants et les enregistrer sous forme de fichiers .3mf. Dans cet exemple, où le type de fichier peut varier, une solution très simple consiste à ouvrir 3D Builder et à inviter l’utilisateur à enregistrer les données importées dans un fichier .3mf, puis à le recharger.

> **Remarque** En plus de convertir les formats de fichier, **3D Builder** offre des outils simples pour modifier vos modèles, ajouter des données de couleur et effectuer d’autres opérations spécifiques à l’impression, c’est pourquoi, son intégration à une application prenant en charge l’impression 3D est souvent recommandée.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## Réparer des données de modèle pour l’impression 3D

Toutes les données de modèle 3D ne sont pas en mesure d’être imprimées, même au format .3mf. Afin que l’imprimante puisse correctement déterminer les espaces à remplir et à laisser vides, le(s) modèle(s) à imprimer doivent constituer un ensemble unique et homogène, disposer de normales de surface orientées vers l’extérieur et d’une géométrie de collection. Les problèmes dans ces zones peuvent survenir sous diverses formes et peuvent être difficiles à repérer dans des formes complexes. Heureusement, les solutions logicielles actuelles sont souvent adaptées à la conversion de la géométrie brute en formes 3D imprimables. Ce processus s’effectue dans la méthode `OnFixClick`.

Le fichier de données 3D doit être converti pour mettre en œuvre [IRandomAccessStream](https://msdn.microsoft.com/library/windows/apps/br241731), qui peut ensuite être utilisé pour générer un objet [Printing3DModel](https://msdn.microsoft.com/library/windows/apps/mt203679).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

L’objet **Printing3DModel** est maintenant réparé et imprimable. Utilisez **SaveModelToPackageAsync** pour affecter le modèle à l’objet Printing3D3MFPackage que vous avez déclaré lors de la création de la classe.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## Exécuter la tâche d’impression : créer un gestionnaire TaskRequested


Par la suite, lorsque la boîte de dialogue d’impression 3D s’affiche et que l’utilisateur choisit de commencer l’impression, votre application doit transmettre les paramètres souhaités au pipeline d’impression 3D. L’API d’impression 3D déclenche l’événement **TaskRequested**. Vous devez écrire une méthode pour gérer cet événement de manière appropriée. Comme toujours, elle doit être du même type que son événement : l’événement **TaskRequested** dispose d’un objet de paramètres [Print3DManager](https://msdn.microsoft.com/library/windows/apps/dn998029) (objet d’expéditeur) et d’un objet [Print3DTaskRequestedEventArgs](https://msdn.microsoft.com/library/windows/apps/dn998051), qui conserve la plupart des informations pertinentes. Son type de retour est **void**.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

L’objectif principal de cette méthode consiste à utiliser l’objet *args* pour envoyer un objet **Printing3D3MFPackage** via le pipeline. Le type **Print3DTaskRequestedEventArgs** dispose d’une propriété : **Request**. Elle est de type [Print3DTaskRequest](https://msdn.microsoft.com/library/windows/apps/dn998050) et représente une demande de travail d’impression. Sa méthode **CreateTask** permet au programme d’envoyer les informations appropriées relatives à votre travail d’impression, et renvoie une référence à l’objet [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) qui est envoyé via le pipeline d’impression 3D.

**CreateTask** a les paramètres d’entrée suivants : une **chaîne** pour le nom du travail d’impression, une **chaîne** pour l’ID de l’imprimante à utiliser, ainsi qu’un délégué **Print3DTaskSourceRequestedHandler**. Le délégué est automatiquement appelé lorsque l’événement **3DTaskSourceRequested** est déclenché (cette opération est effectuée par l’API elle-même). Il est important de noter que ce délégué est appelé lorsqu’un travail d’impression est lancé et qu’il est responsable de la fourniture du package d’impression 3D approprié.

**Print3DTaskSourceRequestedHandler** prend un paramètre, qui est un objet [Print3DTaskSourceRequestedArgs](https://msdn.microsoft.com/library/windows/apps/dn998056) qui fournit les données à envoyer. **SetSource**, l’une des méthodes publiques de cette classe, accepte le package à imprimer. Implémentez un délégué **Print3DTaskSourceRequestedHandler** comme suit :

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Ensuite, appelez la méthode **CreateTask**, à l’aide du délégué `sourceHandler` que vous venez de définir :

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

L’objet **Print3DTask** renvoyé est affecté à la variable de classe déclarée au début. Vous pouvez maintenant utiliser cette référence pour gérer certains événements levés par la tâche (facultatif) :

[!code-cs[Facultatif](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> **Remarque** vous devez implémenter une méthode `Task_Submitting` et `Task_Completed` si vous souhaitez les inscrire à ces événements.

## Exécuter la tâche d’impression : ouvrir la boîte de dialogue d’impression 3D


Le bit de code final requis pour imprimer à partir de votre application est celui qui lance la boîte de dialogue d’impression 3D. Comme une fenêtre de boîte de dialogue d’impression classique, la boîte de dialogue d’impression 3D fournit un certain nombre de spécifications d’impression de dernière minute et permet à l’utilisateur de choisir l’imprimante à utiliser (qu’elle soit connectée via USB ou le réseau).

Tout d’abord, inscrivez votre méthode `MyTaskRequested` à l’événement **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Après avoir inscrit votre gestionnaire d’événements **TaskRequested**, vous pouvez appeler la méthode **ShowPrintUIAsync**, qui fait apparaître la boîte de dialogue d’impression 3D dans la fenêtre d’application actuelle.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Enfin, il est conseillé de désinscrire vos gestionnaires d’événements une fois que votre application reprend le contrôle : [!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## Rubriques connexes

[Générer un package 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)

[Exemple d’impression 3D UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 







<!--HONumber=Jun16_HO4-->


