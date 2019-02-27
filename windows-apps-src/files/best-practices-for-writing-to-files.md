---
title: Meilleures pratiques pour écrire dans des fichiers
description: Découvrez les meilleures pratiques pour l’utilisation de fichier différentes en écrivant des méthodes des classes FileIO et PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115706"
---
# <a name="best-practices-for-writing-to-files"></a>Meilleures pratiques pour écrire dans des fichiers

**API importantes**

* [**Classe FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Classe PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Parfois, les développeurs exécutent dans un ensemble de problèmes courants lorsque vous utilisez les méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) pour effectuer des opérations d’e/s système de fichiers. Par exemple, les problèmes courants sont les suivantes:

• Un fichier est écrit partiellement • l’application reçoit une exception quand vous appelez une des méthodes. • Conserver les opérations en arrière-plan. Fichiers TMP avec un nom de fichier semblable au nom de fichier cible.

Les méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) sont les suivants:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Cet article fournit des détails sur le fonctionnement de ces méthodes pour permettre aux développeurs mieux comprennent quand et comment les utiliser. Cet article fournit des recommandations et n’essaie pas de fournir une solution pour tous les problèmes d’e/s de fichiers possibles. 

> [!NOTE]
> Cet article se concentre sur les méthodes **FileIO** dans les exemples et des discussions. Toutefois, les méthodes de **PathIO** suivent un modèle semblable et la plupart des recommandations de cet article s’applique également à ces méthodes. 

## <a name="conveience-vs-control"></a>Conveience et contrôle

Un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) n’est pas un descripteur de fichier telles que le modèle de programmation Win32 natif. Au lieu de cela, un [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) est une représentation d’un fichier avec des méthodes pour manipuler son contenu.

Comprendre ce concept est utile lorsque vous effectuez des e/s avec un **StorageFile**. Par exemple, la section de [l’écriture dans un fichier](quickstart-reading-and-writing-files.md#writing-to-a-file) présente trois façons d’écrire dans un fichier:

* À l’aide de la méthode [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) .
* En créant une mémoire tampon et puis en appelant la méthode [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) .
* Le modèle de quatre étapes à l’aide d’un flux de données:
  1. [Ouvrez](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) le fichier pour obtenir un flux.
  2. [Obtenir](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) un flux de sortie.
  3. Créez un objet [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) et appeler la méthode correspondante **écrire** .
  4. [Valider](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) les données de l’écriture de données et [Vider](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) le flux de sortie.

Les deux premiers scénarios sont les plus couramment utilisés par les applications. Écriture dans le fichier en une seule opération est plus facile de codage et la maintenance, et elle supprime également la responsabilité de l’application de traiter avec la plupart des complexités d’un fichier e/s. Toutefois, cette a un coût: la perte de contrôle sur l’intégralité de l’opération et la possibilité d’intercepter les erreurs à des points spécifiques.

## <a name="the-transactional-model"></a>Le modèle transactionnel

Les méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) encapsulent les étapes sur le troisième modèle d’écriture décrit ci-dessus, avec une couche supplémentaire. Cette couche est encapsulée dans une opération de stockage.

Pour protéger l’intégrité du fichier d’origine au cas où un problème se produit lors de l’écriture des données, les méthodes **Write** utilisent un modèle transactionnel en ouvrant le fichier à l’aide de [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Ce processus crée un objet [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) . Une fois que cet objet de transaction est créé, les API écrivent les données suite de manière similaire à l’exemple [d’Accès au fichier](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) ou l’exemple de code dans l’article [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) .

Le diagramme suivant illustre les tâches sous-jacentes effectuées par le la méthode **WriteTextAsync** dans une opération d’écriture réussie. L’illustration suivante fournit une vue simplifiée de l’opération. Par exemple, il ignore les étapes telles que la saisie de texte de codage et asynchrone sur différents threads.

![Diagramme de séquence d’appels API UWP pour l’écriture dans un fichier](images/file-write-call-sequence.svg)

Les avantages de l’utilisation des méthodes **écrire** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) au lieu du modèle de quatre étapes plus complexe à l’aide d’un flux de données sont:

* Un appel d’API pour gérer toutes les étapes intermédiaires, y compris les erreurs.
* Le fichier d’origine est conservé en cas de problème.
* L’état du système essaie de rester aussi propres que possible.

Avec autant intermédiaires points de défaillance possibles, il existe toutefois une augmentation risque d’échouer. Lorsqu’une erreur se produit, il peut être difficile à comprendre où le processus a échoué. Les sections suivantes présentent certaines des échecs, vous pouvez rencontrer lorsque vous utilisez les méthodes **Write** et fournir des solutions possibles.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Codes d’erreur courants pour les méthodes d’écriture des classes FileIO et PathIO

Ce tableau présente les codes d’erreur courants que les développeurs d’applications rencontrent en utilisant les méthodes **Write** . Les étapes décrites dans le tableau correspondent aux étapes décrites dans le diagramme précédent.

|  Nom de l’erreur (valeur)  |  Étapes  |  Causes  |  Solutions  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0 X 80070005)  |  5  |  Le fichier d’origine peut-être être marqué pour la suppression, éventuellement à partir d’une opération précédente.  |  Relancez l’opération.</br>Assurez-vous que l’accès au fichier est synchronisé.  |
|  ERROR_SHARING_VIOLATION (0 X 80070020)  |  5  |  Le fichier d’origine est ouvert par une autre écriture exclusive.   |  Relancez l’opération.</br>Assurez-vous que l’accès au fichier est synchronisé.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19 à 20  |  Le fichier d’origine (file.txt) n’a pas pu être remplacé, car il est en cours d’utilisation. Un autre processus ou une opération obtenu l’accès au fichier avant qu’il peut être remplacé.  |  Relancez l’opération.</br>Assurez-vous que l’accès au fichier est synchronisé.  |
|  ERROR_DISK_FULL (0 X 80070070)  |  7, 14, 16, 20  |  Le modèle transactionnel crée un fichier supplémentaire, et cela consomme de stockage supplémentaire.  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  Cela peut se produire en raison de plusieurs opérations d’e/s en attente ou des fichiers volumineux.  |  Une approche plus granulaire en contrôlant le flux peut résoudre l’erreur.  |
|  E_FAIL (0 X 80004005.) |  Indifférent  |  Divers  |  Relancez l’opération. Si elle échoue encore, il peut être une erreur de la plateforme et l’application doit s’arrêter, car c’est dans un état incohérent. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Autres considérations pour les États de fichier qui peuvent entraîner des erreurs

Hormis les erreurs retournées par les méthodes **Write** , voici quelques recommandations sur qu’une application peut s’y attendre en écriture dans un fichier.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Les données ont été écrites dans le fichier si et seulement si l’opération terminée

Votre application ne doit faire toute hypothèse sur les données dans le fichier alors qu’une opération d’écriture est en cours d’exécution. Lors de la tentative d’accès au fichier avant la fin d’une opération peut entraîner des données incohérentes. Votre application doit être responsable du suivi en attente d’e/s.

### <a name="readers"></a>Lecteurs

Si le fichier qui sont écrites dans est également utilisé par un lecteur poli (autrement dit, ouvert avec [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), les lectures suivantes échoue avec une erreur ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Parfois, les applications réessayer puis ouvrez le fichier pour la lecture à nouveau pendant que l’opération **d’écriture** est en cours. Cela peut entraîner une condition de concurrence sur lequel **écrire** finalement échoue lorsque vous essayez de remplacer le fichier d’origine dans la mesure où il ne peut pas être remplacé.

### <a name="files-from-knownfolders"></a>Fichiers à partir de KnownFolders

Votre application n’est peut-être pas l’application uniquement qui tente d’accéder à un fichier qui réside sur une de l' [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Il n’existe aucune garantie que si l’opération réussit, le contenu de qu'une application écrivent des données dans le fichier reste constant pour la prochaine fois qu’il tente de lire le fichier. En outre, le partage erreurs ou accès refusé deviennent plus courants dans ce scénario.

### <a name="conflicting-io"></a>E/s conflictuelles

Les risques d’erreurs d’accès concurrentiel peuvent être réduits si notre application utilise les méthodes **d’écrire** des fichiers dans ses données locales, mais avec prudence est toujours nécessaire. Si plusieurs opérations **d’écriture** sont envoyées simultanément dans le fichier, il n’existe aucune garantie sur finissent par les données dans le fichier. Pour atténuer ce risque, nous recommandons que votre application sérialise **écrire** des opérations sur le fichier.

### <a name="tmp-files"></a>~ Fichiers TMP

Parfois, si l’opération est annulée force (par exemple, si l’application a été suspendue ou arrêtée par le système d’exploitation), la transaction n’est pas validée ou fermée de manière appropriée. Cela peut laisser des fichiers avec un (. ~ TMP) extension. Envisagez de supprimer ces fichiers temporaires (s’ils existent dans les données locales de l’application) lors de la gestion de l’activation de l’application.

## <a name="considerations-based-on-file-types"></a>Considérations relatives à la fonction de types de fichiers

Certaines erreurs peuvent devenir plus répandus selon le type de fichiers, la fréquence à laquelle elles sont accessibles et leur taille de fichier. En règle générale, il existe trois catégories de votre application peut accéder à des fichiers:

* Fichiers créés et modifiés par l’utilisateur dans le dossier de données local de votre application. Ils sont créés et modifiés uniquement lors de l’utilisation de votre application, et qu’elles existent uniquement au sein de l’application.
* Métadonnées de l’application. Votre application utilise ces fichiers pour effectuer le suivi des son propre état.
* Autres fichiers d’où votre application a déclaré des fonctionnalités pour accéder à des emplacements du système de fichiers. Ils sont généralement situés dans parmi les [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Votre application dispose d’un contrôle total sur les deux premières catégories de fichiers, dans la mesure où ils figurent dans le fichiers de package de votre application et sont accessibles par votre application exclusivement. Pour les fichiers dans la dernière catégorie, votre application doit être conscient qu’autres applications et services du système d’exploitation peuvent être accéder aux fichiers simultanément.

Accès aux fichiers des varie en fonction de l’application, sur la fréquence:

* Très faible. Il s’agit généralement de fichiers qui sont ouverts une fois que lorsque les lancements de l’application et sont enregistrement lorsque l’application est suspendue.
* Faible. Il s’agit de fichiers que l’utilisateur spécifiquement réalise une action sur (tels qu’enregistrer ou charger).
* Moyen ou élevé. Il s’agit de fichiers dans lequel l’application doit mettre constamment à jour les données (par exemple, les fonctionnalités d’enregistrement automatique ou les métadonnées constant de suivi).

Pour la taille de fichier, envisagez les données de performances dans le tableau suivant pour la méthode **WriteBytesAsync** . Ce graphique compare la durée de réalisation d’une taille de l’opération de fichier de Visual Studio, via une performance moyenne des 10000 opérations par la taille du fichier dans un environnement contrôlé.

![Performances WriteBytesAsync](images/writebytesasync-performance.png)

Les valeurs de temps sur l’axe y sont omises intentionnellement à partir de ce graphique, car les configurations et les différents matériels génère des valeurs de temps absolu différents. Toutefois, nous avons observé régulièrement ces tendances dans nos tests:

* Pour les très petits fichiers (< = 1 Mo): le temps pour effectuer les opérations est rapide de manière cohérente.
* Pour les fichiers volumineux (> 1 Mo): le temps pour effectuer les opérations commence à augmenter de façon exponentielle.

## <a name="io-during-app-suspension"></a>E/s pendant la suspension d’une application

Votre application doit conçu pour gérer la suspension si vous souhaitez conserver informations d’état ou les métadonnées pour une utilisation dans des sessions ultérieures. Pour des informations générales sur la suspension d’une application, consultez [cycle de vie d’application](../launch-resume/app-lifecycle.md) et [ce billet de blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

Sauf si le système d’exploitation accorde l’exécution étendue à votre application, si votre application est suspendue il dispose de 5 secondes à libérer toutes ses ressources et les enregistrer ses données. Pour bénéficier de la fiabilité et utilisateur expérience, supposez toujours le temps que vous devez gérer les tâches de suspension est limité. Gardez à l’esprit les instructions suivantes pendant la période de temps pour la gestion des tâches de suspension de 5 secondes:

* Faites en sorte que des e/s au minimum pour éviter des conditions de concurrence provoquées par les opérations de vidage et de lancement.
* Évitez d’écrire des fichiers qui nécessitent des centaines de millisecondes ou plus pour écrire.
* Si votre application utilise les méthodes **Write** , gardez à l’esprit toutes les étapes intermédiaires qui nécessitent de ces méthodes.

Si votre application fonctionne sur une petite quantité de données d’état pendant la suspension, dans la plupart des cas, vous pouvez utiliser les méthodes **Write** vider les données. Toutefois, si votre application utilise une grande quantité de données d’état, envisagez d’utiliser des flux de directement stocker vos données. Cela peut aider à réduire le délai introduit par le modèle transactionnel des méthodes **écrire** . 

Pour obtenir un exemple, consultez l’exemple [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) .

## <a name="other-examples-and-resources"></a>Autres exemples et des ressources

Voici quelques exemples et autres ressources pour des scénarios spécifiques.

### <a name="code-example-for-retrying-file-io-example"></a>Exemple de code pour la nouvelle tentative d’exemple de fichier e/s

Voici un exemple de pseudo-code sur comment relancer une écriture (c#), en supposant que l’écriture consiste à le faire une fois que l’utilisateur sélectionne un fichier pour l’enregistrement:

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

La [Programmation parallèle avec .NET blog](https://blogs.msdn.microsoft.com/pfxteam/) est une excellente ressource pour obtenir des conseils sur la programmation parallèle. En particulier, les [publier sur AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) décrit comment mettre à jour d’un accès exclusif à un fichier pour les écritures tout en autorisant l’accès en lecture simultané. N’oubliez pas que la sérialisation e/s impact sur les performances.

## <a name="see-also"></a>Voir aussi

* [Créer, écrire et lire un fichier](quickstart-reading-and-writing-files.md)
