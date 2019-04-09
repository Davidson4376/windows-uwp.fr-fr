---
title: Bonnes pratiques concernant l’écriture de données dans des fichiers
description: Découvrez les meilleures pratiques pour l’utilisation de fichier différents, écrire des méthodes des classes FileIO et PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e0fcb903bd272bd10d434a27d41e6e4558a624ea
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334897"
---
# <a name="best-practices-for-writing-to-files"></a>Bonnes pratiques concernant l’écriture de données dans des fichiers

**API importantes**

* [**FileIO classe**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Classe de PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Les développeurs rencontrent parfois en un ensemble de problèmes courants lors de l’utilisation du **écrire** méthodes de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes pour effectuer des opérations d’e/s de système de fichiers. Par exemple, les problèmes courants sont les suivantes :

* Un fichier est partiellement écrit.
* L’application reçoit une exception lors de l’appel d’une des méthodes.
* Les opérations épargner. Fichiers TMP avec un nom de fichier similaire au nom de fichier cible.

Le **écrire** méthodes de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes incluent les éléments suivants :

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Cet article fournit des détails sur le fonctionnement de ces méthodes pour permettre aux développeurs mieux comprennent quand et comment les utiliser. Cet article fournit des instructions et ne tente pas de fournir une solution pour tous les problèmes d’e/s de fichiers possibles. 

> [!NOTE]
> Cet article se concentre sur la **FileIO** méthodes dans les exemples et des discussions. Toutefois, le **PathIO** méthodes suivent un modèle similaire et la plupart des conseils dans cet article s’applique également à ces méthodes. 

## <a name="convenience-vs-control"></a>Plus de commodité et contrôle

Un [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) objet n’est pas un descripteur de fichier, comme le modèle de programmation Win32 natif. Au lieu de cela, un [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) est une représentation sous forme d’un fichier avec des méthodes pour manipuler son contenu.

Comprendre ce concept est utile lors de l’exécution d’e/s avec une **StorageFile**. Par exemple, le [écriture dans un fichier](quickstart-reading-and-writing-files.md#writing-to-a-file) section présente trois façons d’écrire dans un fichier :

* À l’aide de la [ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) (méthode).
* Par la création d’une mémoire tampon, puis en appelant le [ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) (méthode).
* Le modèle de quatre étapes à l’aide d’un flux de données :
  1. [Ouvrez](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) le fichier à obtenir un flux de données.
  2. [Obtenir](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) un flux de sortie.
  3. Créer un [ **DataWriter** ](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) et appelez correspondant **écrire** (méthode).
  4. [Valider](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) les données dans l’enregistreur de données et [vider](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) le flux de sortie.

Les deux premiers scénarios sont les plus couramment utilisées par les applications. Écriture dans le fichier en une seule opération est plus facile à coder et mettre à jour, et elle supprime également la responsabilité de l’application à gérer des nombreuses complexités du fichier d’e/s. Toutefois, cette commodité a un coût : la perte de contrôle sur l’intégralité de l’opération et la possibilité d’intercepter les erreurs à des moments particuliers.

## <a name="the-transactional-model"></a>Le modèle transactionnels

Le **écrire** méthodes de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes encapsulent les étapes sur l’écriture de tiers modèle décrit ci-dessus, avec une couche supplémentaire. Cette couche est encapsulée dans une transaction de stockage.

Pour protéger l’intégrité du fichier d’origine au cas où une erreur survient lors de l’écriture des données, le **écrire** méthodes utilisent un modèle transactionnels en ouvrant le fichier à l’aide [ **OpenTransactedWriteAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Ce processus crée un [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) objet. Une fois que cet objet de transaction est créé, les API écrire les données suivant est similaire à la [accès aux fichiers](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) exemple ou l’exemple de code dans le [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) article.

Le diagramme suivant illustre les tâches sous-jacentes effectuées par le le **WriteTextAsync** méthode dans une opération d’écriture réussie. Cette illustration présente une vue simplifiée de l’opération. Par exemple, il ignore les étapes telles que la saisie de texte de codage et async sur différents threads.

![Diagramme de séquence d’appel API UWP pour écrire dans un fichier](images/file-write-call-sequence.svg)

Les avantages de l’utilisation de la **écrire** méthodes de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) à la place de classes sur le modèle en quatre étapes plus complexe à l’aide d’un flux de données sont :

* Un appel d’API pour gérer toutes les étapes intermédiaires, y compris les erreurs.
* Le fichier d’origine est conservé en cas de problème.
* L’état du système va tenter de rester aussi propre que possible.

Toutefois, avec autant intermédiaires points de défaillance possibles, il est encore plus probable de défaillance. Lorsqu’une erreur se produit, il peut être difficile à comprendre où le processus a échoué. Les sections suivantes présentent certains des échecs que vous pouvez rencontrer lors de l’utilisation de la **écrire** méthodes et fournissent des solutions possibles.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Codes d’erreur courants pour les méthodes d’écriture des classes FileIO et PathIO

Ce tableau présente les codes d’erreur courants que les développeurs d’applications rencontrent lorsque vous utilisez le **écrire** méthodes. Les étapes décrites dans le tableau correspondent aux étapes dans le diagramme précédent.

|  Nom de l’erreur (valeur)  |  Étapes  |  Causes  |  Solutions  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  Le fichier d’origine peut être marqué pour suppression, peut-être à partir d’une opération précédente.  |  Recommencez l'opération.</br>Vérifiez que l’accès au fichier est synchronisé.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  Le fichier d’origine est ouvert par une autre écriture exclusive.   |  Recommencez l'opération.</br>Vérifiez que l’accès au fichier est synchronisé.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  Le fichier d’origine (file.txt) n’a pas pu être remplacé, car il est en cours d’utilisation. Un autre processus ou une opération obtenu un accès au fichier avant qu’il peut être remplacé.  |  Recommencez l'opération.</br>Vérifiez que l’accès au fichier est synchronisé.  |
|  ERROR_DISK_FULL (0 X 80070070)  |  7, 14, 16, 20  |  Le modèle transactionnel crée un fichier supplémentaire, et cela consomme d’espace de stockage supplémentaire.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  Cela peut se produire en raison de plusieurs opérations d’e/s en attente ou de fichiers volumineux.  |  Une approche plus granulaire en contrôlant le flux peut résoudre l’erreur.  |
|  E_FAIL (0x80004005) |  Indéfini  |  Divers  |  Recommencez l'opération. Si elle échoue encore, il peut être une erreur de la plateforme et l’application doit s’arrêter, car il se trouve dans un état incohérent. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Autres considérations pour les États de fichier qui peuvent entraîner des erreurs

Outre les erreurs retournées par la **écrire** méthodes, voici quelques conseils sur ce qu’une application peut attendre lors de l’écriture dans un fichier.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Enregistrement des données dans le fichier uniquement si l’opération terminée

Votre application ne doit pas d’hypothèses sur les données dans le fichier pendant une opération d’écriture est en cours. Tentative d’accès au fichier avant la fin d’une opération peut entraîner des données incohérentes. Votre application doit être responsable de suivi des e/s en suspens.

### <a name="readers"></a>Lecteurs

Si le fichier qui est en cours d’écriture est également utilisé par un lecteur poli (autrement dit, ouvert avec [ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), les lectures suivantes échoue avec une erreur ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Parfois applications nouvelle tentative d’ouverture du fichier en lecture à nouveau lors de la **écrire** opération est en cours. Cela peut entraîner une condition de concurrence sur lequel le **écrire** finalement échoue lorsque vous essayez de remplacer le fichier d’origine, car il ne peut pas être remplacé.

### <a name="files-from-knownfolders"></a>Fichiers à partir de KnownFolders

Votre application peut ne pas être la seule application qui tente d’accéder à un fichier qui réside sur un de la [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Il n’est pas garanti que si l’opération a réussi, le contenu de qu'une application a écrit dans le fichier reste constant pendant la prochaine fois qu’il essaie de lire le fichier. En outre, partage des erreurs ou accès refusé devenus plus courants dans ce scénario.

### <a name="conflicting-io"></a>E/s en conflit

Les risques d’erreurs d’accès concurrentiel peuvent être réduits si notre application utilise le **écrire** méthodes pour les fichiers dans ses données locales, mais faites attention est toujours requis. Si plusieurs **écrire** opérations sont envoyés simultanément dans le fichier, il n’existe aucune garantie sur quelles données finissent dans le fichier. Pour atténuer ce risque, nous recommandons que votre application sérialise **écrire** opérations dans le fichier.

### <a name="tmp-files"></a>~ Fichiers TMP

Parfois, si l’opération est annulée volontairement (par exemple, si l’application a été suspendue ou arrêtée par le système d’exploitation), la transaction n’est pas validée ou fermée de manière appropriée. Cela peut laisser derrière les fichiers avec un (. ~ TMP) extension. Envisagez de supprimer ces fichiers temporaires (s’ils existent dans les données locales de l’application) lors de la gestion de l’activation de l’application.

## <a name="considerations-based-on-file-types"></a>Considérations relatives à la basée sur les types de fichiers

Certaines erreurs peuvent répandu selon le type de fichiers, la fréquence à laquelle elles sont accessibles et leur taille. En règle générale, il existe trois catégories de votre application peut accéder à des fichiers :

* Fichiers créés et modifiés par l’utilisateur dans le dossier de données locales de votre application. Ceux-ci sont créés et modifiés uniquement lors de l’utilisation de votre application, et qu’elles existent uniquement dans l’application.
* Métadonnées de l’application. Votre application utilise ces fichiers pour effectuer le suivi de son propre état.
* Autres fichiers dans les emplacements du système de fichiers où votre application a déclaré des capacités pour accéder. Ceux-ci sont généralement situés dans un de le [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Votre application a un contrôle total sur les deux premières catégories de fichiers, car ils font partie des fichiers de package de votre application et sont accessibles exclusivement par votre application. Pour les fichiers dans la dernière catégorie, votre application doit être conscient que les services du système d’exploitation et les autres applications peuvent être accède aux fichiers simultanément.

Accès aux fichiers peut varier en fonction de l’application, fréquence :

* Très faible. Il s’agit généralement de fichiers qui sont ouverts une fois quand les lancements d’applications et sont enregistrement lors de l’application est interrompue.
* Faible. Il s’agit de fichiers que l’utilisateur est tenu spécifiquement une action sur (par exemple, enregistrer ou charger).
* Moyenne ou élevée. Il s’agit de fichiers dans lequel l’application doit mettre constamment à jour les données (par exemple, les fonctionnalités de sauvegarde automatique ou constante métadonnées de suivi).

Pour la taille de fichier, considérez les données de performances dans le tableau suivant pour connaître la **WriteBytesAsync** (méthode). Ce graphique compare le temps nécessaire à une taille de fichier opération vs, via une performance moyenne de 10 000 opérations par taille de fichier dans un environnement contrôlé.

![Performances de WriteBytesAsync](images/writebytesasync-performance.png)

Les valeurs de temps sur l’axe y sont intentionnellement omis à partir de ce graphique, car les configurations et autre matériel génèrera des valeurs de temps absolues différents. Toutefois, nous avons observé régulièrement ces tendances dans nos tests :

* Pour les très petits fichiers (< = 1 Mo) : Le temps d’exécution des opérations est systématiquement rapide.
* Pour les fichiers plus volumineux (> 1 Mo) : Le temps d’exécution des opérations commence à augmenter de manière exponentielle.

## <a name="io-during-app-suspension"></a>E/s lors de l’interruption de l’application

Votre application doit conçu pour gérer la suspension si vous souhaitez conserver les informations d’état ou les métadonnées pour une utilisation dans des sessions ultérieures. Pour plus d’informations générales sur la suspension d’application, consultez [cycle de vie](../launch-resume/app-lifecycle.md) et [ce billet de blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

À moins que le système d’exploitation autorise l’exécution étendue à votre application, lorsque votre application est suspendue. elle a 5 secondes pour libérer toutes ses ressources et enregistrer ses données. Pour la fiabilité et l’utilisateur meilleure expérience, supposez toujours que le temps dont vous disposez pour gérer des tâches de suspension est limité. N’oubliez pas les instructions suivantes pendant la période de temps pour la gestion des tâches de la suspension de 5 secondes :

* Essayez de réduire les e/s au minimum afin d’éviter des conditions de concurrence provoquées par les opérations de vidage et de mise en production.
* Évitez d’écrire des fichiers qui nécessitent des centaines de millisecondes ou plus à écrire.
* Si votre application utilise le **écrire** méthodes, n’oubliez pas toutes les étapes intermédiaires qui nécessitent ces méthodes.

Si votre application s’exécute sur une petite quantité de données d’état pendant la suspension, dans la plupart des cas vous pouvez utiliser la **écrire** méthodes à vider les données. Toutefois, si votre application utilise une grande quantité de données d’état, envisagez d’utiliser des flux de données pour stocker directement vos données. Cela peut aider à réduire la latence introduite par le modèle transactionnels de la **écrire** méthodes. 

Pour obtenir un exemple, consultez le [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) exemple.

## <a name="other-examples-and-resources"></a>Autres exemples et des ressources

Voici plusieurs exemples et autres ressources pour des scénarios spécifiques.

### <a name="code-example-for-retrying-file-io-example"></a>Exemple de code pour une nouvelle tentative d’exemple de fichier d’e/s

Voici un exemple de pseudo-code sur réexécution d’une écriture (C#), en supposant que l’écriture ne doit être exécuté une fois que l’utilisateur sélectionne un fichier pour l’enregistrement :

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>Synchroniser l’accès au fichier

Le [.NET blog programmation parallèle avec](https://blogs.msdn.microsoft.com/pfxteam/) est une ressource précieuse pour obtenir des conseils sur la programmation parallèle. En particulier, le [concernant AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) décrit comment mettre à jour d’un accès exclusif à un fichier pour les écritures tout en autorisant un accès en lecture simultané. N’oubliez pas qu’e/s aura un impact sur la sérialisation performances.

## <a name="see-also"></a>Voir aussi

* [Créer, écrire et lire un fichier](quickstart-reading-and-writing-files.md)
