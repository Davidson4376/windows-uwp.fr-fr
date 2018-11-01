---
author: mijacobs
Description: Learn how to store and retrieve local, roaming, and temporary app data.
title: Stocker et récupérer des paramètres et autres données d’application
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.author: mijacobs
ms.date: 11/14/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2af9aec9742a950f7bd35794ff4d10763e6c2c5c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5886499"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Stocker et récupérer des paramètres et autres données d’application

Les *données d’application* sont des données mutables spécifiques à une application particulière. Elles comprennent l’état d’exécution, les préférences utilisateur et d’autres paramètres. Les données d’application diffèrent des *données utilisateur* que l’utilisateur crée et gère en utilisant l’application. Elles incluent les fichiers de documents ou multimédias, les transcriptions de courrier électronique ou de communication ou les enregistrements de base de données dont le contenu a été créé par l’utilisateur. Les données utilisateur peuvent s’avérer utiles ou judicieuses pour plusieurs applications. Il s’agit souvent de données que l’utilisateur veut manipuler ou transmettre sous forme d’entité indépendante de l’application elle-même, par exemple un document.

**Remarque importante sur les données d’application: **la durée de vie des données d’application est liée à celle de l’application. Si l’application est supprimée, toutes les données d’application sont par conséquent perdues. N’utilisez pas les données d’application pour stocker des données utilisateur ni d’autres éléments que les utilisateurs peuvent considérer comme précieux et irremplaçables. Nous vous conseillons d’utiliser les bibliothèques de l’utilisateur et Microsoft OneDrive pour stocker ce type d’informations. Les données d’application sont idéales pour le stockage des préférences utilisateur, paramètres et favoris spécifiques à l’application.

## <a name="types-of-app-data"></a>Types de données d’application


Il existe deux types de données d’application: les fichiers et les paramètres.

-   **Paramètres**

    Utilisez les paramètres pour stocker les préférences de l’utilisateur et les informations relatives à l’état de l’application. L’API de données d’application permet de créer et récupérer facilement des paramètres (exemples fournis plus loin dans cet article).

    Voici les types de données que vous pouvez utiliser pour les paramètres d’application:

    -   **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
    -   **Booléen**
    -   **Char16**, **String**
    -   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588) : ensemble de paramètres d’application associés qui doivent être sérialisés et désérialisés de façon atomique. Utilisez des paramètres composites pour gérer facilement les mises à jour atomiques des paramètres interdépendants. Le système assure l’intégrité des paramètres composites lors d’accès simultanés et dans le cadre de l’itinérance. Les paramètres composites étant optimisés pour de faibles volumes de données, leur utilisation pour des jeux de données volumineux peut nuire aux performances.
-   **Fichiers**

    Utilisez des fichiers pour stocker des données binaires ou pour activer vos propres types sérialisés personnalisés.

## <a name="storing-app-data-in-the-app-data-stores"></a>Stockage des données d’application dans les magasins de données d’application


Quand une application est installée, le système lui attribue ses propres magasins de données par utilisateur pour y stocker des paramètres et des fichiers. Vous n’avez pas besoin de savoir où et comment ces données existent, car le système est responsable de la gestion du stockage physique, en veillant à ce que les données soient conservées isolément d’autres applications et utilisateurs. De même, le système préserve le contenu de ces magasins de données lorsque l’utilisateur installe une mise à jour de votre application et supprime intégralement l’ensemble du contenu de ces magasins de données lorsque votre application est désinstallée.

Le magasin de données de chaque application comprend des répertoires racines définis par le système : un pour les fichiers locaux, un pour les fichiers itinérants et un pour les fichiers temporaires. Votre application peut ajouter de nouveaux fichiers et conteneurs à chacun de ces répertoires racines.

## <a name="local-app-data"></a>Données d’application locale


Les données d’application locale doivent être utilisées pour toutes les informations à préserver entre les sessions d’application. Elles ne conviennent pas pour les données d’application itinérantes. Les données non applicables à d’autres appareils doivent également y être conservées. Il n’existe pas de limitation générale de taille sur les données locales stockées. Utilisez le magasin de données d’application local pour les données qui n’ont pas lieu d’utiliser un profil itinérant et pour les jeux de données volumineux.

### <a name="retrieve-the-local-app-data-store"></a>Récupérer le magasin de données d’application locale

Avant de pouvoir lire ou écrire des données d’application locale, vous devez extraire le magasin de données d’application locale. Pour récupérer le magasin de données d’application locales, utilisez la propriété [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) pour obtenir les paramètres locaux de l’application en tant qu’objet [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599). Utilisez la propriété [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) pour obtenir les fichiers d’un objet [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). Utilisez la propriété [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825) pour obtenir le dossier du magasin de données d’application locales dans lequel vous pouvez enregistrer des fichiers qui ne sont pas inclus dans la sauvegarde et la restauration.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Créer et récupérer un paramètre local unique

Pour créer ou écrire un paramètre, utilisez la propriété [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) pour accéder aux paramètres inclus dans le conteneur `localSettings` abordé à l’étape précédente. Cet exemple crée un paramètre intitulé `exampleSetting`.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Pour récupérer le paramètre, vous utilisez la propriété [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) que vous avez utilisée pour créer le paramètre. Cet exemple montre comment récupérer le paramètre que nous venons de créer.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Créer et récupérer une valeur locale composite

Pour créer ou écrire une valeur composite, créez un objet [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588). Cet exemple crée un paramètre composite intitulé `exampleCompositeSetting`, puis l’ajoute au conteneur `localSettings`.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

Cet exemple montre comment récupérer la valeur composite que nous venons de créer.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>Créer et lire un fichier local

Pour créer et mettre à jour un fichier dans le magasin de données d’application locales, utilisez des API de fichier telles que [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) et [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). Cet exemple crée un fichier appelé `dataFile.txt` dans le conteneur `localFolder`, puis écrit la date et l’heure actuelles dans le fichier. La valeur **replaceExisting** de l’énumération [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indique de remplacer le fichier s’il existe.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Pour ouvrir et lire un fichier dans le magasin de données d’application locales, utilisez des API de fichier telles que [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) et [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). Cet exemple ouvre le fichier `dataFile.txt` créé à l’étape précédente, puis lit la date du fichier. Pour obtenir des détails sur le chargement de ressources de fichiers à partir de différents emplacements, voir [Comment charger des ressources de fichiers](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>Données d’itinérance


Si vous utilisez des données itinérantes dans votre application, vos utilisateurs peuvent facilement maintenir les données de votre application synchronisées sur les différents appareils. Si un utilisateur installe votre application sur plusieurs périphériques, le système d’exploitation maintient les données d’application synchronisées, ce qui réduit le nombre de tâches de configuration de l’application que l’utilisateur doit effectuer sur ses autres périphériques. De même, l’itinérance permet aux utilisateurs de reprendre une tâche (par exemple, composer une liste) à l’endroit même où ils l’ont abandonnée, y compris sur un autre périphérique. Le système d’exploitation réplique les données itinérantes sur le Cloud quand elles sont mises à jour et les synchronise sur les autres périphériques sur lesquels l’application est installée.

Le système d’exploitation limite la taille des données d’application que chaque application peut rendre itinérantes. Voir [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625). Si l’application atteint cette limite, aucune donnée de l’application n’est répliquée sur le Cloud tant que le volume total des données d’application itinérantes de l’application n’est pas inférieur à la limite. Pour cette raison, il est recommandé d’utiliser les données itinérantes uniquement pour les préférences utilisateur, les liens et les petits fichiers de données.

Les données itinérantes d’une application sont disponibles dans le Cloud du moment où l’utilisateur y a accédé à partir d’un appareil dans le délai imparti. Si l’utilisateur n’exécute pas une application au-delà de ce délai, ses données itinérantes sont supprimées du Cloud. Si un utilisateur désinstalle une application, ses données itinérantes ne sont pas supprimées automatiquement du Cloud : elles sont préservées. De ce fait, si l’utilisateur réinstalle l’application dans les délais, les données itinérantes sont synchronisées à partir du Cloud.

### <a name="roaming-data-dos-and-donts"></a>Pratiques conseillées et déconseillées en matière de données itinérantes

-   Utilisez l’itinérance pour les préférences utilisateur et les personnalisations, les liens et les petits fichiers de données. Par exemple, utilisez l’itinérance pour conserver la couleur d’arrière-plan choisie par un utilisateur sur tous les appareils.
-   Utilisez l’itinérance pour permettre aux utilisateurs de poursuivre une tâche sur plusieurs périphériques. Par exemple, utilisez l’itinérance pour des données d’application telles que le contenu d’un e-mail à l’état de brouillon ou la dernière page consultée dans une application de lecture.
-   Gérez l’événement [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) en mettant à jour les données d’application. Cet événement se produit juste après la fin de la synchronisation des données d’application à partir du cloud.
-   Utilisez l’itinérance pour les références au contenu plutôt que les données brutes. Par exemple, rendez une URL itinérante plutôt que le contenu d’un article en ligne.
-   Pour les paramètres importants, où le temps joue un rôle critique, utilisez le paramètre *HighPriority* associé à [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624).
-   Ne rendez pas itinérantes des données d’application spécifiques à un appareil. Certaines informations ne sont pertinentes que d’un point de vue local, par exemple le chemin d’accès d’un fichier local. Si vous décidez de rendre itinérantes des informations locales, assurez-vous que l’application peut récupérer son état d’exécution si ces informations ne sont pas valides sur l’appareil secondaire.
-   Ne rendez pas itinérants de grands ensembles de données d’application. Il existe une limite à la quantité de données d’application qu’une application peut utiliser de manière itinérante. Servez-vous de la propriété [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) pour atteindre cette limite. Si une application atteint cette limite, l’itinérance des données n’est plus possible tant que la taille du magasin de données d’application reste au-dessus de cette limite. Quand vous concevez votre application, vous devez penser à restreindre les grandes quantités de données pour éviter de dépasser cette limite. Par exemple, si l’enregistrement de l’état d’une partie demande 10 Ko, l’application pourrait autoriser l’utilisateur à conserver seulement 10 parties au maximum.
-   N’utilisez pas l’itinérance pour les données qui reposent sur une synchronisation instantanée. Windows ne garantit pas une synchronisation instantanée. L’itinérance peut être retardée de manière importante si un utilisateur est hors connexion ou sur un réseau dont la latence est élevée. Assurez-vous que votre interface utilisateur ne dépend pas d’une synchronisation instantanée.
-   N’utilisez pas d’itinérance pour les données qui changent fréquemment. Ainsi, si votre application effectue le suivi d’informations qui changent souvent, par exemple la position de lecture d’une chanson à la seconde près, ne stockez pas ces informations en tant que données d’application itinérantes. À la place, choisissez une représentation moins fréquente qui offre tout de même une expérience utilisateur intéressante, par exemple la chanson en cours de lecture.

### <a name="roaming-pre-requisites"></a>Conditions préalables d’itinérance

Tous les utilisateurs peuvent bénéficier de l’itinérance des données d’application s’ils utilisent un compte Microsoft pour se connecter à leur appareil. Toutefois, les utilisateurs et les administrateurs de stratégie de groupe peuvent désactiver l’itinérance des données d’application sur un appareil à tout moment. Si un utilisateur décide de ne pas utiliser un compte Microsoft ou s’il désactive les fonctionnalités d’itinérance des données, il peut continuer à se servir de votre application. Toutefois, les données d’application sont gérées de manière locale sur chaque appareil.

Les données stockées dans [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) sont transférées uniquement si un utilisateur a « approuvé » un appareil. Si un appareil n’est pas approuvé, les données sécurisées dans ce coffre ne seront pas itinérantes.

### <a name="conflict-resolution"></a>Résolution des conflits

L’itinérance des données d’application n’est pas prévue pour être utilisée sur plusieurs appareils à la fois. Si un conflit se produit durant la synchronisation en raison du changement d’une unité de données sur deux appareils, le système favorise toujours la valeur écrite en dernier. Cette précaution garantit que l’application exploite les informations les plus récentes. Si l’unité de données est un composite de paramètre, la résolution des conflits se produit au niveau de l’unité du paramètre; en d’autres termes, le composite possédant le changement le plus récent est synchronisé.

### <a name="when-to-write-data"></a>Quand écrire les données

Selon la durée de vie prévue du paramètre, les données doivent être écrites à différents moments. Les données d’application qui changent rarement ou lentement doivent être écrites immédiatement. Toutefois, les données d’application qui évoluent fréquemment doivent être écrites seulement à intervalles réguliers (par exemple toutes les 5 minutes) et durant la suspension de l’application. Par exemple, une application de musique peut écrire le paramètre «chanson actuelle» au début de chaque nouvelle chanson, mais la position réelle dans la chanson doit être écrite uniquement au moment de l’interruption.

### <a name="excessive-usage-protection"></a>Protection contre une utilisation excessive

Le système possède divers mécanismes de protection pour éviter une utilisation inappropriée des ressources. Si les données d’application ne se transfèrent pas comme prévu, il est probable que l’appareil a été provisoirement limité. Cette situation est en général automatiquement résolue en attendant quelque temps, et aucune mesure n’est nécessaire.

### <a name="versioning"></a>Contrôle de version

Les données d’application peuvent utiliser le contrôle de version pour passer d’une structure de données à une autre. Le numéro de version est différent de la version de l’application ; il peut être défini comme bon vous semble. Même si ce n’est pas obligatoire, il est vivement conseillé d’utiliser des numéros de version croissants. En effet, des complications (notamment des pertes de données) peuvent se produire si vous essayez d’effectuer un transfert vers un numéro de version de données inférieur représentant des données plus récentes.

Les données d’application ne sont itinérantes qu’entre les applications installées qui possèdent le même numéro de version. En d’autres termes, les appareils de la version 2 peuvent transférer des données entre eux, tout comme les appareils de la version 3. Toutefois, aucune itinérance n’a lieu entre un appareil de la version 2 et un appareil de la version 3. Si vous installez une nouvelle application qui utilisait plusieurs numéros de version sur d’autres appareils, l’application récemment installée synchronise les données d’application associées au numéro de version le plus élevé.

### <a name="testing-and-tools"></a>Test et outils

Les développeurs peuvent verrouiller leur appareil pour déclencher une synchronisation des données d’application itinérantes. Si les données d’application ne semblent pas se transférer après un intervalle de temps donné, vérifiez les éléments suivants :

-   Vos données itinérantes ne dépassent pas la taille maximale (pour plus d’informations, voir [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)).
-   Vos fichiers sont fermés et publiés correctement.
-   Il existe au moins deux appareils qui exécutent la même version de l’application.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>S’inscrire pour recevoir les notifications sur les changements de données en itinérance

Pour utiliser des données d’application itinérante, vous devez vous inscrite aux modifications de données itinérantes et récupérer les conteneurs de données itinérantes afin de pouvoir lire et écrire des paramètres.

1.  Inscrivez-vous pour recevoir une notification quand les données d’itinérance changent.

    L’événement [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) vous avertit en cas de changement des données d’itinérance. Cet exemple définit `DataChangeHandler` comme gestionnaire des modifications des données itinérantes.

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  Obtenez les conteneurs pour les paramètres et les fichiers de l’application.

    Utilisez la propriété [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) pour obtenir les paramètres et la propriété [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) pour déterminer les fichiers.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Créer et récupérer des paramètres d’itinérance

Utilisez la propriété [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) pour accéder aux paramètres inclus dans le conteneur `roamingSettings` abordé dans la section précédente. Cet exemple crée un paramètre nommé `exampleSetting` et une valeur composite nommée `composite`.

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

Cet exemple récupère les paramètres que nous venons de créer.

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>Créer et récupérer des fichiers d’itinérance

Pour créer et mettre à jour un fichier dans le magasin de données d’application itinérantes, faites appel à des API de fichier telles que [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) et [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). Cet exemple crée un fichier appelé `dataFile.txt` dans le conteneur `roamingFolder`, puis écrit la date et l’heure actuelles dans le fichier. La valeur **replaceExisting** de l’énumération [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indique de remplacer le fichier s’il existe.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Pour ouvrir et lire un fichier dans le magasin de données d’application itinérantes, utilisez des API de fichier telles que [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) et [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). Cet exemple ouvre le fichier `dataFile.txt` créé dans la section précédente, puis lit la date du fichier. Pour obtenir des détails sur le chargement de ressources de fichiers à partir de différents emplacements, voir [Comment charger des ressources de fichiers](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>Données d’application temporaires


Le magasin de données d’application temporaires fonctionne comme un cache. Ses fichiers n’utilisent pas de profil itinérant et ne peuvent pas être supprimées à tout moment. La tâche Maintenance du système peut supprimer automatiquement les données stockées à cet emplacement à tout moment. L’utilisateur peut également effacer les fichiers du magasin de données temporaires à l’aide de l’outil Nettoyage de disque. Les données d’application temporaires sont utilisables pour stocker des informations provisoires au cours d’une session d’application. Il n’existe aucune garantie de persistance de ces données au-delà de la fin de la session d’application, car le système peut récupérer l’espace utilisé si nécessaire. L’emplacement est disponible via la propriété [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629).

### <a name="retrieve-the-temporary-data-container"></a>Récupérer le conteneur de données temporaires

Utilisez la propriété [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) pour obtenir les fichiers. Les étapes suivantes s’appuient sur la variable `temporaryFolder` de cette étape.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Créer et lire les fichiers temporaires

Pour créer et mettre à jour un fichier dans le magasin de données d’application temporaires, utilisez des API de fichier telles que [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) et [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). Cet exemple crée un fichier appelé `dataFile.txt` dans le conteneur `temporaryFolder`, puis écrit la date et l’heure actuelles dans le fichier. La valeur **replaceExisting** de l’énumération [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indique de remplacer le fichier s’il existe.


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Pour ouvrir et lire un fichier dans le magasin de données d’application temporaires, utilisez des API de fichier telles que [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) et [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). Cet exemple ouvre le fichier `dataFile.txt` créé à l’étape précédente, puis lit la date du fichier. Pour obtenir des détails sur le chargement de ressources de fichiers à partir de différents emplacements, voir [Comment charger des ressources de fichiers](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>Organiser les données d’application avec des conteneurs


Pour vous aider à organiser vos paramètres et fichiers de données d’application, vous créez des conteneurs (représentés par des objets [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599)) au lieu de travailler directement avec des répertoires. Vous pouvez ajouter des conteneurs aux magasins de données local, d’itinérance et temporaire. Les conteneurs peuvent être imbriqués sur 32 niveaux.

Pour créer un conteneur de paramètres, appelez la méthode [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611). Cet exemple crée un conteneur de paramètres local nommé `exampleContainer`, puis ajoute un paramètre nommé `exampleSetting`. La valeur **Always** de l’énumération [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616) indique que le conteneur est créé s’il n’existe pas.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>Supprimer des paramètres et conteneurs d’application


Pour supprimer un paramètre dont votre application n’a plus besoin, utilisez la méthode [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608). Cet exemple supprime le paramètre local `exampleSetting` que nous avons créé précédemment.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Pour supprimer un paramètre composite, utilisez la méthode [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597). Cet exemple supprime le paramètre composite `exampleCompositeSetting` local que nous avons créé dans l’exemple précédent.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Pour supprimer un conteneur, appelez la méthode [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612). Cet exemple supprime le conteneur de paramètres `exampleContainer` local que nous avons créé précédemment.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Contrôle de version de vos données d’application


Vous pouvez éventuellement créer différentes versions des données d’application pour votre application. Vous pouvez par exemple créer une nouvelle version de votre application qui aura pour effet de changer le format de ses données d’application sans pour autant causer de problèmes de compatibilité avec la version précédente de l’application. L’application examine la version des données d’application dans le magasin de données et si la version est antérieure à la version attendue par l’application, celle-ci doit mettre à jour les données d’application vers le nouveau format et mettre à jour la version. Pour en savoir plus, consultez la propriété [**Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630) et la méthode [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429).

## <a name="related-articles"></a>Articles connexes

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)
