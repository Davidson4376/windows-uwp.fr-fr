---
title: Utilisation des propriétés supplémentaires
description: Introduction à l’utilisation des propriétés supplémentaires et présentation de leur implémentation dans Windows
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, API WinRT, indexation, recherche, uwp
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369266"
---
# <a name="using-supplemental-properties"></a>Utilisation des propriétés supplémentaires  

## <a name="summary"></a>Résumé  
- Les propriétés supplémentaires permettent aux applications d’étiqueter des fichiers avec des propriétés sans modifier ceux-ci 
- Elles sont utiles lorsque vous disposez de propriétés dont le traitement informatique est difficile ou lorsque le fichier ne peut pas être modifié 
- L’utilisation des propriétés supplémentaires est similaire à celle des autres propriétés du système de propriétés Windows  

## <a name="introduction"></a>Introduction 
La plupart des nouvelles applications passionnantes développées ces dernières années nécessitent l’exécution d’opérations gourmandes en ressources d’UC sur les fichiers utilisateur afin d’en extraire des propriétés plus utiles que les informations de base (la date de création, par exemple). Ces applications permettent la reconnaissance d’objets dans les images, l’extraction d’intentions dans les e-mails et l’analyse de texte pour le regroupement de documents. Cela s’explique par la puissance de calcul qui est désormais disponible sur la plupart des PC grand public.   

La disponibilité instantanée de ces métadonnées accroît la productivité des utilisateurs de façon exponentielle. Il est intéressant de savoir que votre fille figure sur une photo, mais il est beaucoup plus utile de pouvoir trouver la photo d’elle avec sa grand-mère. Cela permet de personnaliser l’utilisation d’un ordinateur et de la rendre plus vivante. Comme si une personne située dans l’ordinateur vous aidait à retrouver vos souvenirs les plus précieux. 

Depuis des décennies, la solution de recherche rapide sous Windows a été l'indexeur, et dans la mise à jour de Creators Update, celui-ci a été mis à jour pour prendre en charge ces nouveaux scénarios. Les applications sont maintenant capables d’étiqueter les fichiers avec des propriétés supplémentaires en plus de celles qui sont extraites par le système. Ces propriétés sont traitées comme des citoyens de première classe  

## <a name="windows-properties"></a>Propriétés Windows 
Le [système de propriétés Windows](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) a été un élément clé de l’interaction avec les fichiers pendant des années. Il permet aux applications de lire les propriétés des fichiers sans qu’il soit nécessaire de comprendre le fonctionnement interne des formats ou des langages dans lesquels les fichiers sont enregistrés. Tout ce qui est abstrait pour vous en tant que développeur, tout ce que vous avez à faire est de demander une liste et de spécifier si vous souhaitez que l’ordre soit croissant ou décroissant.  

Le système de propriétés est étroitement associé à l'indexeur Windows - il lit toutes les propriétés des fichiers dans son étendue et les stocke. Plus tard, lorsqu’une application demande qu’une liste de tous les fichiers .docx d’un dossier soit triée par date de modification à l’exception de ceux écrits par John Smith, l’indexeur peut renvoyer la liste instantanément.  

Dans ce mode de fonctionnement conjoint des systèmes, l’indexeur exigeait que toutes les propriétés qu’il stockait sur un fichier soient disponibles instantanément. En raison de cette contrainte, l’indexeur ne pouvait connaître des propriétés plus intéressantes dont le traitement demande plus de temps.  

L’utilisation des propriétés est cependant aisée ; en effet, l’application peut soit demander un ensemble trié de propriétés relatives à un fichier, un peu comme si elle travaillait avec une base de données, soit envoyer une requête comme si elle utilisait un moteur de recherche. L’indexeur traite la requête et renvoie les résultats. Cela donne aux développeurs la possibilité de combiner leurs filtres (par exemple effectuer des recherches uniquement dans les fichiers jpg) avec la requête de l’utilisateur (nom de fichier commençant par « oiseau »). 

## <a name="supplemental-properties"></a>Propriétés supplémentaires 

Les propriétés supplémentaires se comportent de la même manière que les propriétés Windows normales avec une différence notable : elles ne sont pas écrites lorsque le fichier est ajouté à l’indexeur. Une propriété supplémentaire doit être ajoutée ultérieurement par une autre application sur le système. Une fois la reconnaissance de l’objet terminée, l’opération peut avoir lieu ultérieurement, que ce soit deux minutes ou deux jours plus tard. 

Une fois écrite, la propriété peut être recherchée, filtrée, triée ou regroupée comme n’importe quelle autre propriété du système. Elle peut également être utilisée dans des requêtes combinées avec d’autres propriétés du système, qu’elles soient supplémentaires ou standards. Cela vous donne l’avantage de combiner aisément des propriétés supplémentaires avec le code de votre système de fichiers existant sans avoir à le réécrire.  

### <a name="example-scenarios"></a>Exemples de scénario 

Il existe des milliers de propriétés différentes que vous pourriez écrire dans une propriété supplémentaire, mais, pour des raisons pratiques, ce didacticiel aborde les scénarios clés suivants :  

#### <a name="tagging-pictures-with-extracted-properties"></a>Marquage des images avec les propriétés extraites 
Ces applications peuvent utiliser un modèle formé par ML afin d’extraire les caractéristiques d’une image inconnues du système, comme un objet dans l’image. Ce modèle peut alors récupérer les objets qu’il identifie dans l’image et les ajouter au système de propriétés afin de permettre aux utilisateurs d’effectuer des recherches sur ces critères ou de regrouper les objets ultérieurement.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Marquer les fichiers avec un identifiant spécifique à l'application 
De nombreuses applications de synchronisation de fichiers utilisent les ID uniques des fichiers pour suivre les déplacements de ceux-ci entre le serveur et divers appareils clients. Le client de synchronisation peut écrire cet ID dans le système de propriétés sans affecter le fichier. L’ID est mis à la disposition, à la fois, de l’application en vue d’un accès rapide si nécessaire, et des autres applications du système afin d’être lu dans le cadre de communications avec le fournisseur de synchronisation. 

L’utilisation de propriétés supplémentaires s’accompagne de nombreuses autres options, mais les deux exemples fournis sont satisfaisants, car ces informations sont rapidement identifiables dans le cadre d’une interrogation ou d’une recherche, elles sont inconnues du système et elles ne peuvent pas être ajoutées au fichier lui-même.  

### <a name="using-supplemental-properties"></a>Utilisation des propriétés supplémentaires 
L'utilisation des propriétés supplémentaires est différente de l’écriture d’une propriété normale dans le système de fichiers. Si vous êtes familiarisé avec l'utilisation de StorageFiles et de ses propriétés, vous pouvez ignorer cette étape. Dans le cas contraire, abordons ensemble un court exemple d’écriture d’une propriété dans un fichier, puis lisons la même propriété ultérieurement.  

### <a name="writing-supplemental-properties"></a>Écriture de propriétés complémentaires  
Dans l’exemple, par souci de simplicité, seul le premier fichier trouvé est modifié, mais généralement une application ajoute la propriété à chaque fichier qu’elle rencontre.  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

Avant d’écrire une propriété, il est important de vérifier si l’emplacement est indexé. Dans cet exemple, nous utilisons les options de requête pour filtrer uniquement les emplacements indexés. Si ce n’est pas possible, vous pouvez vérifier l’état indexé du dossier parent (file.GetParentAsync().GetIndexedStateAsync()). Dans les deux cas, vous obtiendrez les mêmes résultats 

### <a name="reading-supplemental-properties"></a>Lecture des propriétés supplémentaires 
Encore une fois, la lecture d’une propriété supplémentaire est similaire à celle de toute autre propriété du système de fichiers. Dans cet exemple, l’application lit une seule propriété d’un fichier pour lequel elle possède déjà un StorageFile, mais elle peut aussi lire d’autres propriétés simultanément.  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
Il est nécessaire de vérifier que la valeur extraite du système de propriétés correspond bien à la valeur attendue. Bien que cela soit peu probable, il est possible que la valeur ait été effacée depuis que votre application l’a écrite. Cette question sera abordée en détail ci-après.  

### <a name="implementation-notes"></a>Remarques d’implémentation 
La conception des propriétés supplémentaires a fait l’objet de quelques choix judicieux. Pour vous aider dans votre implémentation, les sections suivantes ont été copiées à partir des spécifications de conception technique de la fonctionnalité. Elles vous donnent un aperçu sur la conception de la fonctionnalité et les raisons de l’existence de certaines limites définies. 

### <a name="supplemental-properties-available"></a>Propriétés supplémentaires disponibles 
Initialement, seules deux propriétés sont utilisables par les applications : System.Supplemental.ResourceId et System.Supplemental.AlbumID. Il est possible d’en ajouter d’autres si nécessaire. L’ID de l’album est une chaîne à valeurs multiples qui peut être utilisée pour un grand nombre d’applications différentes, tandis que le ResourceId est utilisé comme un ID unique pour les fournisseurs de synchronisation cloud. 

#### <a name="file-system-support"></a>Prise en charge du système de fichiers 
Comme les supports amovibles formatés en FAT sont nombreux, les propriétés supplémentaires prennent en charge les lecteurs FAT et NTFS. Cela garantira que les propriétés supplémentaires seront disponibles pour tous les utilisateurs, quel que soit leur type d’appareil.   

### <a name="non-indexed-locations"></a>Emplacements non indexés  
Sur le bureau, un certain nombre de dossiers ne sont pas indexés. Dans ces cas, les applications peuvent toujours avoir besoin d’accéder à des propriétés supplémentaires. Cependant, les propriétés supplémentaires ne sont pas disponibles à l’extérieur des emplacements indexés. Ce compromis a dû à plusieurs raisons :  

- Les bibliothèques et les emplacements de stockage en ligne sont tous indexés par défaut.   
  Il s’agit des emplacements que les applications UWP utilisent principalement. D’autres emplacements qui ne sont pas indexés (lecteurs système ou réseau), mais ils sont moins souvent utilisés pour stocker les données utilisateur. 

- La conception de surface WinRT API suppose que l’indexeur est presque toujours disponible.  
  Ainsi, l’indexeur est déjà disponible dans la plupart des emplacements qui intéressent les applications. Si les utilisateurs stockent des données dans des emplacements non indexés, la solution la plus simple sera d’ajouter cet emplacement à l'index. Grâce à l’implémentation des propriétés supplémentaires, l’énumération sera plus rapide, et les applications seront en mesure de changer le suivi de l’emplacement.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Lecture ou écriture de propriétés supplémentaires à partir d’un fichier dans un emplacement non indexé 
Dans le cas où une application tente d’écrire une propriété supplémentaire à un emplacement qui n’est pas encore indexé, l’appel d’API génère une exception. Ce sera la même exception qui sera levée, comme lorsque quelqu’un essaie de mettre à jour System.Music.AlbumArtist sur un fichier.docx (arguments non valides).  
 
### <a name="change-notifications"></a>Notifications de modifications :  
Les notifications de modification et le suivi des modifications UWP continueront de fonctionner pour les propriétés supplémentaires comme ils le font pour les propriétés standards. Cela permettra aux applications qui fournissent des données de suivre toutes les modifications apportées à l’une d’elles 
  
### <a name="invalidating-properties"></a>Invalidation de propriétés :  
Les propriétés supplémentaires d’un fichier peuvent devenir obsolètes lorsque celui-ci est modifié ou déplacé sur le système. Les applications qui pousseront les données seront chargées de fournir les informations sur la validité des données ou sur la nécessité de les mettre à jour de sorte que le système leur offrira simplement les outils nécessaires pour qu’elles puissent les comprendre elles-mêmes.  
 
Dans le cas où un fichier est modifié mais pas déplacé ou renommé, toutes les propriétés supplémentaires du fichier restent inchangées. Les applications pourront s’enregistrer afin de recevoir des notifications de modification via la surface d’API existante et de mettre à jour les propriétés si nécessaire. 
 
Si le fichier est déplacé, les propriétés seront invalidées. L’application recevra les notifications de modification de suppression, de création, de changement de nom ou de déplacement en fonction de la manière exacte dont l’opération est effectuée. Après avoir reçu la notification de modification, l’application peut inspecter le fichier et mettre à jour les propriétés supplémentaires de celui-ci si nécessaire. 
 
### <a name="indexer-rebuilds"></a>Reconstructions de l’indexeur  
Il peut arriver que l’index du système doive être reconstruit pour un certain nombre de raisons : le schéma des propriétés peut changer, l'utilisateur peut activer la Protection des données d'entreprise ou simplement le fichier de base de données peut être endommagé. Dans ces cas, les propriétés supplémentaires doivent être effacées. Nous avons tout fait pour essayer de préserver les propriétés supplémentaires lors de la reconstruction de l'index, mais nous avons été confrontés à des contraintes majeures :  

### <a name="protecting-the-data"></a>Protection des données 
Dans le cas où le fichier de base de données est endommagé, soit par des erreurs disque, soit par un logiciel malveillant, il est impossible de protéger les données qui ont été stockées dans ce fichier. Il faudrait que le fichier soit stocké ailleurs dans le système ou isolé du reste de la base de données. 

Nous nous efforçons d’écarter les risques d’endommagement de l’index car nous considérons que l’intégrité de celui-ci est essentielle.  
Maintien de la correspondance entre les fichiers et leurs métadonnées pendant les reconstructions 

Bien que l’index puisse protéger les données dans le cadre d’une reconstruction, il est impossible de savoir si le fichier a été modifié pendant la reconstruction de l'index. Les données du fichier que l’index protège peuvent ne plus être valides si celui-ci est modifié ou déplacé.  
Comportement 

Dans le cas d’une reconstruction de l’indexeur, toutes les données supplémentaires sont perdues. Les applications doivent replacer dans l’indexeur les données qui ont été perdues pendant la reconstruction. Bien que cela représente une charge supplémentaire pour les applications, celle-ci est jugée raisonnable car les applications garderont la maîtrise de toutes leurs données.  

### <a name="recovering"></a>Récupération 
Après avoir identifié que l’index est en cours de reconstruction, les applications doivent mettre à jour les propriétés supplémentaires à leur convenance.  
### <a name="privacy"></a>Confidentialité 
Les utilisateurs peuvent ne pas vouloir partager certaines propriétés avec d’autres applications en raison de la nature de celles-ci. Les applications doivent être en mesure d’indiquer que les informations qu’elles écrivent dans les propriétés seront privées (accessibles par elles uniquement), partagées avec seulement quelques autres applications ou publiques pour chaque application du système.  

Bien qu’il s’agisse d’une fonctionnalité potentiellement intéressante pour certains des utilisateurs précoces de la fonctionnalité, ceux-ci estiment que la récupération de propriétés publiques donnera encore de la valeur à la conception. C’est pourquoi nous devrions continuer à construire la fonctionnalité sans support pour masquer les valeurs si nécessaire. Son ajout ultérieur ouvre d’autres possibilités dont il est important de tenir compte dans toutes les conceptions.  

## <a name="conclusions"></a>Conclusions 
Les propriétés supplémentaires permettent de stocker ainsi une quantité plus grande de propriétés de fichiers dans le système. Bien que facultative, leur utilisation peut donner à votre application un avantage sur d’autres applications qui ne sont pas en mesure de trier et de rechercher leurs données aussi rapidement. 

Nous sommes impatients que des applications commencent à utiliser ces propriétés. Si vous avez des questions sur l’utilisation de l'en-tête, veuillez nous en faire part dans les commentaires ci-dessous 
