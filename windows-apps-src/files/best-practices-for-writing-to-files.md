---
title: Bonnes pratiques concernant l’écriture de données dans des fichiers
description: Découvrez les bonnes pratiques relatives aux différentes méthodes d’écriture dans les fichiers pour les classes FileIO et PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a6a1d93b1deaad084ff25db946199b678b35703c
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369512"
---
# <a name="best-practices-for-writing-to-files"></a>Bonnes pratiques concernant l’écriture de données dans des fichiers

**API importantes**

* [**Classe FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Classe PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Les développeurs peuvent rencontrer des problèmes courants lorsqu’ils utilisent les méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) afin d’effectuer les opérations d’E/S du système de fichiers. Voici quelques exemples de problèmes courants :

* Un fichier est partiellement écrit.
* L’application reçoit une exception lors de l’appel d’une des méthodes.
* Les opérations laissent de côté les fichiers .TMP dont le nom de fichier ressemble au nom de fichier cible.

Les méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) incluent les éléments suivants :

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Cet article fournit des détails sur le fonctionnement de ces méthodes pour permettre aux développeurs de mieux comprendre quand et comment les utiliser. Cet article fournit des conseils et non une solution pour tous les problèmes d’E/S de fichiers possibles. 

> [!NOTE]
> Cet article se concentre sur les méthodes **FileIO** dans les exemples et les discussions. Toutefois, les méthodes **PathIO** suivent un modèle similaire et la plupart des conseils de cet article s’appliquent également à ces méthodes. 

## <a name="convenience-vs-control"></a>Commodité et contrôle

Un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) n’est pas un descripteur de fichier, tel que le modèle de programmation Win32 natif. Un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) est en fait la représentation d’un fichier comportant des méthodes pour manipuler son contenu.

Il est utile de comprendre ce concept lors de l’exécution d’E/S avec un objet **StorageFile**. Par exemple, la section [Écriture dans un fichier](quickstart-reading-and-writing-files.md#writing-to-a-file) présente trois façons d’écrire dans un fichier :

* À l’aide de la méthode [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync).
* En créant une mémoire tampon, puis en appelant la méthode [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync).
* À l’aide du modèle en quatre étapes qui utilise un flux de données :
  1. [Ouvrez](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) le fichier pour obtenir un flux de données.
  2. [Obtenez](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) un flux de sortie.
  3. Créez un objet [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) et appelez la méthode **Write** correspondante.
  4. [Validez](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) les données dans l’enregistreur de données et [videz](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) le flux de sortie.

Les deux premiers scénarios sont les plus couramment utilisés par les applications. L’écriture dans le fichier en une seule opération est plus facile à coder et à mettre à jour, et elle supprime la responsabilité de l’application à gérer les nombreuses complexités liées aux E/S de fichier. Toutefois, cette commodité a un coût : la perte de contrôle sur l’intégralité de l’opération et la possibilité d’intercepter les erreurs à des moments spécifiques.

## <a name="the-transactional-model"></a>Modèle transactionnel

Les méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) encapsulent les étapes du troisième modèle d’écriture décrit ci-dessus, avec une couche supplémentaire. Cette couche est encapsulée dans une transaction de stockage.

Pour protéger l’intégrité du fichier d’origine en cas d’erreur lors de l’écriture des données, les méthodes **Write** utilisent un modèle transactionnel et ouvrent le fichier à l’aide d’[**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Ce processus crée un objet [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction). Une fois que cet objet de transaction est créé, les API écrivent les données de la même manière que l’exemple d’[accès aux fichiers](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) ou l’exemple de code dans l’article [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction).

Le diagramme suivant illustre les tâches sous-jacentes effectuées par le la méthode **WriteTextAsync** dans une opération d’écriture réussie. Cette illustration présente une vue simplifiée de l’opération. Par exemple, elle ignore les étapes telles que l’encodage de texte et les opérations asynchrones sur plusieurs threads.

![Diagramme de séquence d’appel d’API UWP pour écrire dans un fichier](images/file-write-call-sequence.svg)

L’utilisation des méthodes **Write** des classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) et [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) à la place du modèle plus complexe en quatre étapes faisant appel à un flux de données présente les avantages suivants :

* Un appel d’API pour gérer toutes les étapes intermédiaires, y compris les erreurs.
* Le fichier d’origine est conservé en cas de problème.
* L’état du système tente de rester aussi propre que possible.

Toutefois, avec autant de points de défaillance intermédiaires possibles, les risques de défaillance sont accrus. Lorsqu’une erreur se produit, il peut être difficile de définir où le processus a échoué. Les sections suivantes présentent certaines défaillances que vous pouvez rencontrer lors de l’utilisation des méthodes **Write** et fournissent des solutions possibles.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Codes d’erreur courants pour les méthodes Write des classes FileIO et PathIO

Ce tableau présente les codes d’erreur courants que les développeurs d’applications rencontrent lorsqu’ils utilisent les méthodes **Write**. Les étapes décrites dans le tableau correspondent aux étapes dans le diagramme précédent.

|  Nom de l’erreur (valeur)  |  Étapes  |  Causes  |  Solutions  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  Le fichier d’origine peut être marqué pour suppression, éventuellement à cause d’une opération précédente.  |  Recommencez l'opération.</br>Vérifiez que l’accès au fichier est synchronisé.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  Le fichier d’origine est ouvert par une autre écriture exclusive.   |  Recommencez l'opération.</br>Vérifiez que l’accès au fichier est synchronisé.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  Le fichier d’origine (file.txt) n’a pas pu être remplacé, car il est en cours d’utilisation. Un autre processus ou une autre opération a obtenu un accès au fichier avant qu’il ne puisse être remplacé.  |  Recommencez l'opération.</br>Vérifiez que l’accès au fichier est synchronisé.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  Le modèle transactionnel crée un fichier supplémentaire, ce qui utilise un espace de stockage supplémentaire.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  Cela peut se produire en raison de plusieurs opérations d’E/S en attente ou de fichiers volumineux.  |  Une approche plus granulaire avec un contrôle du flux peut résoudre l’erreur.  |
|  E_FAIL (0x80004005) |  Indéfini  |  Divers  |  Recommencez l'opération. En cas de nouvel échec, il peut s’agir d’une erreur de la plateforme. L’application doit alors fermer, car elle se trouve dans un état incohérent. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Autres considérations relatives aux états de fichier, susceptibles d’entraîner des erreurs

Outre les erreurs retournées par les méthodes **Write**, voici quelques informations sur ce qu’une application peut attendre lors de l’écriture dans un fichier.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Les données ont été écrites dans le fichier uniquement si l’opération est terminée

Votre application ne doit pas faire d’hypothèses sur les données présentes dans le fichier pendant une opération d’écriture. Les tentatives d’accès au fichier avant la fin d’une opération peuvent engendrer des données incohérentes. Votre application doit être responsable du suivi des E/S en attente.

### <a name="readers"></a>Lecteurs

Si le fichier en cours d’écriture est également utilisé par un lecteur courtois (autrement dit, ouvert avec [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)), les lectures suivantes échouent et présentent l’erreur ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Parfois, les applications tentent d’ouvrir le fichier pour une nouvelle lecture pendant l’opération d’**écriture**. Cela peut entraîner une condition de concurrence aboutissant à l’échec de l’opération d’**écriture** lorsque vous essayez de remplacer le fichier d’origine, car ce dernier ne peut pas être remplacé.

### <a name="files-from-knownfolders"></a>Fichiers issus de KnownFolders

Votre application n’est peut-être pas la seule application qui tente d’accéder à un fichier résidant sur l’un des [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Il n’est pas garanti que, si l’opération a réussi, le contenu écrit par une application dans le fichier subsiste la prochaine fois qu’elle essaiera de lire le fichier. En outre, les erreurs de partage et d’accès refusé sont plus courantes dans ce scénario.

### <a name="conflicting-io"></a>Conflit d’E/S

Les risques d’erreur d’accès concurrentiel peuvent être réduits si notre application utilise les méthodes **Write** pour les fichiers dans ses données locales, mais une attention particulière est toujours requise. Si plusieurs opérations **Write** sont envoyées simultanément dans le fichier, il n’existe aucune garantie concernant les données ajoutées dans le fichier. Pour limiter ce risque, il est recommandé que votre application sérialise les opérations **Write** dans le fichier.

### <a name="tmp-files"></a>Fichiers ~TMP

Parfois, si l’opération est annulée volontairement (par exemple, si l’application a été suspendue ou arrêtée par le système d’exploitation), la transaction n’est pas validée ou fermée de manière appropriée. Des fichiers portant l’extension (.~TMP) peuvent alors subsister. Envisagez de supprimer ces fichiers temporaires (s’ils existent dans les données locales de l’application) lors de la gestion de l’activation de l’application.

## <a name="considerations-based-on-file-types"></a>Considérations relatives aux types de fichiers

Certaines erreurs peuvent devenir plus courantes selon le type de fichier, la fréquence à laquelle les fichiers sont ouverts et la taille des fichiers. En règle générale, votre application peut accéder à trois catégories de fichiers :

* Fichiers créés et modifiés par l’utilisateur dans le dossier de données locales de votre application. Ces derniers sont créés et modifiés uniquement lors de l’utilisation de votre application, et ils existent uniquement dans l’application.
* Métadonnées de l’application. Votre application utilise ces fichiers pour effectuer le suivi de son propre état.
* Autres fichiers dans les emplacements du système de fichiers pour lesquels votre application a déclaré des capacités d’accès. Ces derniers sont généralement situés dans l’un des [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Votre application a un contrôle total sur les deux premières catégories de fichiers, car il s’agit des fichiers de package de votre application, auxquels seule votre application peut accéder. Pour les fichiers de la dernière catégorie, votre application doit savoir que d’autres applications ainsi que les services du système d’exploitation peuvent accéder aux fichiers simultanément.

Selon l’application, l’accès aux fichiers peut varier en fonction de la fréquence :

* Très faible. Il s’agit généralement de fichiers qui sont ouverts une fois lors du lancement des applications et sont enregistrés lors de l’interruption des applications.
* Faible. Il s’agit de fichiers sur lesquels l’utilisateur réalise une action spécifique (par exemple, enregistrement ou chargement).
* Moyenne ou élevée. Il s’agit de fichiers dans lesquels l’application doit mettre constamment à jour les données (par exemple, fonctionnalités de sauvegarde automatique ou suivi constant des métadonnées).

Pour la taille des fichiers, tenez compte des données de performances indiquées dans le tableau suivant afin d’appliquer la méthode **WriteBytesAsync**. Ce tableau compare le temps nécessaire à une opération en fonction de la taille de fichier, sur une performance moyenne de 10 000 opérations par taille de fichier dans un environnement contrôlé.

![Performances WriteBytesAsync](images/writebytesasync-performance.png)

Les valeurs de temps sur l’axe y sont intentionnellement omises dans ce graphique, car les valeurs de temps absolues diffèrent en fonction du matériel et des configurations. Toutefois, nous avons observé régulièrement ces tendances dans nos tests :

* Pour les très petits fichiers (< = 1 Mo) : Le temps d’exécution des opérations est rapide.
* Pour les fichiers plus volumineux (> 1 Mo) : Le temps d’exécution des opérations commence à augmenter de manière exponentielle.

## <a name="io-during-app-suspension"></a>E/S lors de l’interruption de l’application

Votre application doit être conçue de manière à gérer les interruptions si vous souhaitez conserver les informations d’état ou les métadonnées pour des sessions ultérieures. Pour plus d’informations générales sur l’interruption d’une application, consultez l’article [Cycle de vie de l’application](../launch-resume/app-lifecycle.md) et [ce billet de blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

À moins que le système d’exploitation n’autorise une exécution étendue à votre application, lorsque votre application est interrompue, elle a 5 secondes pour libérer toutes ses ressources et enregistrer ses données. Pour optimiser la fiabilité et l’expérience utilisateur, supposez toujours que le temps dont vous disposez pour gérer les tâches liées à une interruption est limité. N’oubliez pas ce qui suit pendant les 5 secondes dédiées à la gestion des tâches liées à l’interruption :

* Essayez de réduire les E/S au minimum afin d’éviter des conditions de concurrence provoquées par les opérations de vidage et de libération.
* Évitez d’écrire dans des fichiers qui nécessitent au minimum des centaines de millisecondes pour les opérations d’écriture.
* Si votre application utilise les méthodes **Write**, n’oubliez pas toutes les étapes intermédiaires requises par ces méthodes.

Si votre application s’exécute sur une petite quantité de données d’état pendant l’interruption, vous pouvez utiliser dans la plupart des cas les méthodes **Write** pour vider les données. Toutefois, si votre application utilise une grande quantité de données d’état, envisagez d’utiliser des flux de données pour stocker directement vos données. Cela peut réduire la latence introduite par le modèle transactionnel des méthodes **Write**. 

Pour en savoir plus, reportez-vous à l’exemple [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension).

## <a name="other-examples-and-resources"></a>Autres exemples et ressources

Voici plusieurs exemples et ressources dédiés à des scénarios spécifiques.

### <a name="code-example-for-retrying-file-io-example"></a>Exemple de code pour une nouvelle tentative d’E/S de fichier

Voici un exemple de pseudo-code dédié à la réexécution d’une écriture (C#), en supposant que l’écriture ne doit être exécutée qu’une fois que l’utilisateur a sélectionné un fichier pour l’enregistrement :

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

Le [blog .Parallel Programming with .NET](https://devblogs.microsoft.com/pfxteam/) est une ressource précieuse pour obtenir des conseils sur la programmation parallèle. Le [billet concernant AsyncReaderWriterLock](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) explique notamment comment conserver un accès exclusif à un fichier pour les écritures tout en autorisant un accès en lecture simultané. N’oubliez pas que la sérialisation des E/S a un impact sur les performances.

## <a name="see-also"></a>Voir également

* [Créer, écrire et lire un fichier](quickstart-reading-and-writing-files.md)
