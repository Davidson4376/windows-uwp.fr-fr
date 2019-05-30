---
title: À l’aide des propriétés supplémentaires
description: Présentation de l’utilisation des propriétés supplémentaires et des détails sur la façon dont elles ont été implémentées dans Windows
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, uwp, API WinRT, indexeur, recherche
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369266"
---
# <a name="using-supplemental-properties"></a>À l’aide des propriétés supplémentaires  

## <a name="summary"></a>Récapitulatif  
- Propriétés supplémentaires autoriser les applications pour baliser les fichiers avec les propriétés sans modifier le fichier 
- Utile pour les cas où vous avez des propriétés qui sont difficiles à calculer, ou le fichier ne peut pas être modifié. 
- À l’aide des propriétés supplémentaires est identique à l’aide de n’importe quelle autre propriété sur le système de propriétés de Windows  

## <a name="introduction"></a>Introduction 
La plupart des nouvelles applications intéressantes dans ces dernières années requièrent opérations intensives d’UC sur les fichiers d’utilisateur pour extraire des propriétés utiles à partir des fichiers au-delà des tâches de base comme la date de création en cours d’exécution. Ces applications de l’objet ensemble reconnaissance dans les images, intent extraction dans des e-mails et l’analyse de texte pour regrouper les documents. Cela est piloté par l’informatique puissantes est désormais disponible sur les ordinateurs pour la plupart des particuliers.   

Rendre ces métadonnées instantanément consultable permet aux utilisateurs d’être exponentiellement plus productifs. Simplement savoir que votre fille est dans l’image est intéressant, mais la possibilité de rechercher l’image associée avec sa grand-mère est beaucoup plus utile. Il rend l’expérience de l’utilisation d’une idée de l’ordinateur plus personnelle et plus active. Comme une personne de l’ordinateur est contacter pour vous aider à trouver vos précieux métrages. 

Depuis des décennies, la solution pour la recherche rapide sur Windows a été l’indexeur, et dans la mise à jour Creators il a été mis à jour pour prendre en charge ces nouveaux scénarios. Les applications sont désormais en mesure de baliser les fichiers avec des propriétés supplémentaires au-delà de celles qui sont extraites par le système. Ces propriétés sont traitées comme des citoyens de première classe  

## <a name="windows-properties"></a>Propriétés de Windows 
Le [système de propriétés de Windows](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) a été un élément essentiel de l’interaction avec les fichiers depuis des années. Il permet aux applications de lire les propriétés des fichiers sans avoir à comprendre les mécanismes internes de tous les différents formats de fichiers ou un fichier peut être dans des langues. Tout cela est abstrait, pour vous en tant que développeur, il vous suffit poser pour obtenir la liste et spécifier l’ordre croissant ou décroissant.  

Le système de propriétés est étroitement avec l’indexeur de Windows : il lit toutes les propriétés des fichiers dans son étendue et les stocke. Lorsqu’une application demande pour obtenir la liste de tous les .docx dans un dossier à trier par date de modification, à l’exception de ceux créés par John Smith l’indexeur peut revenir ultérieurement la liste instantanément.  

L’inconvénient de la façon dont ces systèmes fonctionnent ensemble est que l’indexeur utilisée pour exiger toutes les propriétés qu’il serait stocker sur un fichier soit disponible instantanément. Cela limitait il de savoir que sur les propriétés plus intéressantes qui prennent plus de temps à calculer dans la mesure où il est nécessaire de mal.  

À l’aide de propriétés est cependant facile, l’application peut demander un ensemble trié de propriétés relatives à un fichier, comme utiliser une base de données, ou il peut transmettre une requête semblable à l’aide d’un moteur de recherche. L’indexeur traite la requête et retourner les résultats. Cela permettent aux développeurs la possibilité de combiner leurs filtres (par exemple uniquement des fichiers de jpg de recherche) avec la requête d’un utilisateur (nom de fichier en commençant par « bird »). 

## <a name="supplemental-properties"></a>Propriétés supplémentaires 

Propriétés supplémentaires se comportent comme des propriétés Windows régulières avec une différence très importante : ils ne vont pas à écrire lorsque le fichier est ajouté à l’indexeur. Une propriété supplémentaire doit être ajoutée par une autre application sur le système plus tard. Cela peut signifier deux minutes plus tard une fois terminée reconnaissance d’objet, ou il peut s’agir jours plus tard. 

Une fois que la propriété est écrite il peut être recherché, filtrée, triée ou regroupée comme toute autre propriété sur le système. Ainsi il peut être utilisé dans les requêtes combinées avec d’autres propriétés sur le système, soit supplémentaires ou non. Cela vous donner la possibilité de combiner facilement des propriétés supplémentaires avec votre code de système de fichiers existant sans avoir à effectuer une réécriture.  

### <a name="example-scenarios"></a>Exemples de scénario 

Il existe des milliers de propriétés différentes, que vous pouvez écrire à une propriété supplémentaire, mais il existe deux principaux scénarios pour lesquels ce didacticiel sera conserver en revenant à :  

#### <a name="tagging-pictures-with-extracted-properties"></a>Marquage des images avec les propriétés extraites 
Ces applications peuvent utiliser un modèle formé ML pour extraire des fonctionnalités à partir d’une image que le système ne connaît comme objet dans l’image. Il peut ensuite prendre les objets qu’il identifie dans l’image et les ajouter au système de propriétés pour les recherches plus tard ou de regroupement.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Fichiers de balisage avec un ID d’application spécifique 
De nombreuses applications de synchronisation de fichier utilisent leur propre ID unique pour effectuer le suivi des fichiers lors de leur déplacement entre le serveur et les appareils clients différents. Le client de synchronisation peut écrire ce code au système de propriétés sans incidence sur le fichier. Cet ID est maintenant disponible à l’application plus tard pour un accès rapide et disponibles pour toute autre application sur le système pour lire lors de la communication avec le fournisseur de synchronisation. 

Il existe de nombreuses autres options pour l’utilisation des propriétés supplémentaires, mais ces deux types d’effectuer les bons exemples, car elles nécessitent une recherche rapide ou recherche, sont des éléments d’information, le système ne connaît pas et ne peut pas être ajouté au fichier lui-même.  

### <a name="using-supplemental-properties"></a>À l’aide des propriétés supplémentaires 
En utilisant les propriétés supplémentaires est identique à l’écriture d’une propriété normale au système de fichiers. Si vous êtes à l’aise avec utilisation StorageFiles et propriétés, vous pouvez ignorer sur cela. Sinon, nous allons étudier un exemple rapide d’écriture d’une propriété unique dans un fichier, puis en lisant plus tard dans la même propriété.  

### <a name="writing-supplemental-properties"></a>Écriture de propriétés supplémentaires  
L’exemple sera modifier uniquement le premier fichier qu’il trouve, par souci de simplicité, mais généralement une application ajoute la propriété à chaque fichier qu’il trouve.  

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

Il existe un contrôle important que si l’emplacement est indexé avant l’écriture d’une propriété. Dans cet exemple, nous utilisons les options de requête pour filtrer uniquement les emplacements indexés. Si ce n’est pas possible, vous pouvez vérifier l’état indexée du dossier parent (fichier. GetParentAsync(). GetIndexedStateAsync()). Dans les deux cas génèrera les mêmes résultats 

### <a name="reading-supplemental-properties"></a>Lecture des propriétés supplémentaires 
Là encore, la lecture d’une propriété supplémentaire est identique à la lecture de toute autre propriété de système de fichiers. Dans cet exemple l’application sera simplement lire une propriété à partir d’un fichier pour qu'a déjà un objet StorageFile, mais il peut également lire les autres propriétés en même temps.  

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
Il existe une vérification de s’assurer que la valeur provenant du système de propriétés est ce que vous attendez. Bien qu’il est peu probable, il est possible de que la valeur a été effacée dans la mesure où votre application a l’écrit. Ce point sera abordé en détail ci-dessous.  

### <a name="implementation-notes"></a>Remarques d’implémentation 
Il existe quelques choix subtiles qui ont été apportées à la conception des propriétés supplémentaires. Pour vous aider à votre implémentation, les sections suivantes ont été copiées à partir de la spécification de conception ingénierie pour la fonctionnalité. Ils fournissent un aperçu dans la façon dont la fonctionnalité a été conçue et pourquoi certaines limitations existent. 

### <a name="supplemental-properties-available"></a>Propriétés supplémentaires disponibles 
Il existe uniquement deux propriétés disponibles à utiliser pour les applications au départ : System.Supplemental.ResourceId et System.Supplemental.AlbumID. S’il est nécessaire pour plus d’informations qu’ils peuvent être ajoutés. L’ID de l’album est une chaîne à valeurs multiples qui peut être utilisée pour de nombreuses applications différentes et l’ID de ressource est utilisé comme un ID unique pour les fournisseurs de synchronisation de cloud. 

#### <a name="file-system-support"></a>Prise en charge du système de fichiers 
Dans la mesure où FAT mis en forme un support amovible est un scénario important, des propriétés supplémentaires prendra en charge des lecteurs FAT et NTFS. Cela garantit que les propriétés supplémentaires seront disponibles pour tous les utilisateurs, quel que soit leur type d’appareil.   

### <a name="non-indexed-locations"></a>Emplacements non indexés  
Sur le bureau, il existe un nombre de dossiers qui ne sont pas indexés. Dans ce cas, les applications peuvent toujours doivent avoir accès aux propriétés supplémentaires. Toutefois, les propriétés supplémentaires ne sont pas disponibles en dehors des emplacements indexés. Ce compromis a été apportée pour plusieurs raisons :  

- Toutes les bibliothèques et les emplacements de stockage cloud sont indexés par défaut.   
  Ceux-ci sont les emplacements que les applications UWP pour être principalement à l’aide. Il existe des autres emplacements qui ne sont pas indexées (lecteurs de système ou réseau), mais ils sont moins fréquemment utilisées pour stocker les données de l’utilisateur. 

- La conception de surface d’API de WinRT suppose que l’indexeur est presque toujours disponible.  
  Par conséquent, l’indexeur est déjà disponible dans la plupart des emplacements sont intéressées par les applications. Si les utilisateurs sont trouvés pour stocker des données dans les emplacements non indexés, la solution la plus simple sera pour ajouter cet emplacement à l’index. Travail de propriétés puis supplémentaires, énumération seront plus rapide et applications seront en mesure de modifier le suivi de l’emplacement.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Lecture ou écriture des propriétés supplémentaires à partir d’un fichier dans un emplacement Non-Indexed 
Dans le cas où une application tente d’écrire une propriété supplémentaire dans un emplacement qui n’est pas actuellement indexé, puis l’appel d’API lèvera une exception. Il s’agit de la même exception est levée en tant que lorsque quelqu'un tente de mettre à jour le System.Music.AlbumArtist sur un fichier .docx (Args non valide).  
 
### <a name="change-notifications"></a>Les notifications de modifications :  
Notifications de modification de la plateforme Windows universelle et le suivi des modifications continueront à fonctionner pour les propriétés supplémentaires, comme ils le font pour les propriétés standards. Cela permettra d’applications qui offrent aux données pour effectuer le suivi de toutes les modifications effectuées à un de leurs applications 
  
### <a name="invalidating-properties"></a>Invalider les propriétés :  
Chaque fois qu’un fichier est modifié ou déplacé sur le système, les propriétés supplémentaires sur un fichier peuvent devenir obsolètes. Applications envoyant les données seront celles avec les informations sur si les données sont valides ou doivent être mis à jour le système propose uniquement les outils pour eux de savoir eux-mêmes.  
 
Dans le cas d’un fichier est modifié, mais pas déplacé ou renommé, toutes les propriétés supplémentaires sur le fichier reste inchangées. Les applications seront en mesure de s’inscrire aux notifications de modification via la surface d’API existante et de mettre à jour les propriétés en fonction des besoins. 
 
Si le fichier est déplacé, les propriétés vont être invalidé. L’application sera recevoir les notifications de modification de soit supprimer, créer, renommé ou déplacé en fonction de l’exactement l’opération soit terminée. Une fois que l’application a reçu la notification de modification, il sera en mesure d’inspecter le fichier et mettre à jour les propriétés supplémentaires sur le fichier en fonction des besoins. 
 
### <a name="indexer-rebuilds"></a>Reconstructions d’indexeur  
Parfois l’index du système doit être reconstruit pour l’une des diverses raisons : le schéma de propriété peut changer, l’utilisateur peut activer EDP ou simplement le fichier de base de données peut être endommagé. Dans ce cas, les propriétés supplémentaires ne seront pas préservées. Nous avons envisagé de travailler pour essayer de conserver les propriétés supplémentaires lors de l’index est reconstruit, mais il y avait deux principaux BLOQUEURS :  

### <a name="protecting-the-data"></a>Protection des données 
Dans le cas où le fichier de base de données est endommagé, soit en erreurs disque ou des logiciels non autorisés, il sera impossible de protéger les données stockées dans ce fichier. Il devra être stockées ailleurs sur le système ou d’une certaine manière isolé du reste de la base de données. 

Étant donné que nous faisons déjà beaucoup de travail pour que l’index moins susceptible d’être endommagée, cela permet de réduire le taux d’incidence de ce cas quand même.  
Maintenir le mappage entre les fichiers et leurs métadonnées pendant la reconstruction 

Même si l’index peut protéger les données sur une reconstruction, il est impossible de savoir si le fichier a changé pendant la reconstruction de l’index. Les données qui protège l’index à partir du fichier peuvent être n’est plus valides si le fichier est modifié ou déplacé.  
Comportement 

Dans le cas d’une reconstruction d’indexeur, toutes les données supplémentaires seront perdues. Applications sera chargées de remettre les données dans l’indexeur a été perdue pendant la reconstruction. Cela place une charge supplémentaire sur les applications, mais est jugé raisonnable dans la mesure où ils seront toujours maintenir l’état maître pour toutes leurs données.  

### <a name="recovering"></a>La récupération 
Une fois que les applications ont remarqué que l’index est reconstruit, ils seront responsables de la mise à jour les propriétés supplémentaires à leur convenance.  
### <a name="privacy"></a>Confidentialité 
Certaines des propriétés qui peuvent être écrites dans les fichiers seront telles que les utilisateurs peuvent souhaiter pas les partager avec d’autres applications. Applications doivent être en mesure d’indiquer que les informations qu’ils écrivent dans les propriétés va être privée soit leurs applications, partagées avec seulement quelques autres applications ou publiques à chaque application sur le système.  

Bien qu’il s’agit potentiellement une fonctionnalité intéressante pour certains des premiers de la fonctionnalité, ils pensent que l’obtention des propriétés publiques toujours va ajouter un grand nombre de fonctionnalités à la conception. Par conséquent, cette fonctionnalité est marquée comme une agréable d’avoir, et nous devons continuer à créer la fonctionnalité sans prise en charge pour masquer les valeurs si nécessaire. Ajout d’ultérieurement s’ouvre davantage de scénarios, il est donc important de prendre en compte dans les conceptions.  

## <a name="conclusions"></a>Conclusions 
C’est tout, les propriétés supplémentaires sont un moyen simple pour stocker les propriétés de fichier plus dans le système. Leur utilisation est bien sûr facultative, mais elle peut octroyer à votre application un avantage sur les autres applications qui ne sont pas en mesure de trier et de rechercher leurs données aussi rapidement. 

Nous attendons de voir les applications à commencer à utiliser ces propriétés. Si vous avez des questions sur la manière dont l’utilisation l’en-tête faites-le nous savoir dans les commentaires ci-dessous 
