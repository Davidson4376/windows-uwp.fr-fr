---
title: Gestion du stockage local de connecté
description: Découvrez comment gérer les données de stockage connecté locales dans un environnement de développement.
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 09fce637c50b0a03230d0808e51a9e5cfadc7179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642054"
---
# <a name="managing-local-connected-storage"></a>Gestion du stockage local de connecté
Stockage connecté est utilisée pour stocker les données de votre jeu dans le cloud, il existe également un composant de stockage local pour le service de stockage connectés. Si vous êtes sur un PC ou de la console il est un cache local des données de stockage connecté qui contient les données synchronisées dans le cloud. Si vous créez un titre XDK ou UWP il existe un outil vous permet de gérer vos données locales de stockage connecté.

Consultez le tableau suivant pour trouver l’outil approprié pour gérer vos données de stockage connecté localement en cache :

|Classification de titre  |Appareil  |Outil de stockage local  |
|---------|---------|---------|
|XDK     |Console de Xbox One     |*xbstorage*         |
|UWP     |PC         |*gamesaveutil*         |
|UWP     |Console de Xbox One     |Portail de l’appareil de Xbox (XDP |)

- *Xbstorage* est un outil de ligne de commande, exécutez à partir de l’invite de commandes XDK, pour la gestion des mises en cache localement connecté de stockage sur la Console une Xbox. Le *xbstorage* outil se trouve dans le XDK une Xbox sous le chemin d’accès de fichier : **/Program fichiers (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* est un outil de ligne de commande de gestion UWP localement en cache connecté de stockage sur PC. Le *gamesaveutil* outil est fournie avec le [SDK Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) pour Fall Creators Update et versions ultérieures (build 10.0.16299.15 et versions ultérieures). Une fois que vous avez installé la version appropriée du SDK Windows 10, *gamesaveutil* se trouve sous le dossier : **ProgramFiles(x86)/Windows Kits/10/bin / [Kit de développement logiciel Version]/x64/gamesaveutil.exe**.
- *Portal(XDP) d’appareil Xbox* est un portail en ligne qui vous permet de gérer les données UWP de stockage connecté mis en cache localement sur votre Xbox une seule Console. Cet article ne couvre pas l’utilisation XDP. Pour apprendre à utiliser XDP lire [portail des appareils pour Xbox](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox).

## <a name="xbstorage"></a>Xbstorage

*Xbstorage* permet l’effacement des données mises en cache localement les données de stockage connectés depuis le disque dur, ainsi que l’importation et exportation de données pour les utilisateurs ou ordinateurs à partir d’espaces de stockage connecté à l’aide de fichiers XML.

Quand une opération est effectuée sur les données locales à l’aide du *xbstorage* outil, le système se comportera comme si cette opération a été effectuée par l’application elle-même, la lecture des données à partir d’un espace de stockage connecté à un fichier local conduit synchronisation avec le cloud avant de copier.

De même, une copie des données à partir d’un fichier XML sur le PC de développement à un conteneur de stockage connectés sur le kit de développement Xbox One entraîne la console démarrer le chargement de ces données vers le cloud. Toutefois, il existe des conditions où cela se produit : si le kit de développement ne peut pas acquérir le verrou, ou s’il existe un conflit de données entre les conteneurs de la console et celles figurant dans le cloud, la console se comporte comme si l’utilisateur avait décidé de ne pas résoudre le conflit par choisir une version du conteneur à conserver et de la console se comporte comme si l’utilisateur en cours de lecture hors connexion jusqu'à ce que la prochaine fois que le titre est démarré.

Est la seule exception à ces commandes **réinitialiser** **/force** qui efface le stockage local des données enregistrées pour tous les utilisateurs et SCIDs, mais ne modifie pas les données stockées dans le cloud. Cela est utile pour placer une console dans l’état, qu'il serait dans si un utilisateur a été itinérance à une console et télécharger des données à partir du cloud lors de la lecture d’un titre.

### <a name="xbstorage-commands"></a>Commandes Xbstorage

Xbstorage a suit les développeurs de six commandes peuvent utiliser avec l’invite de commandes XDK pour gérer les données locales sur le Kit de développement Xbox un :

<a id="xbstorage_reset"></a>

|Commande  |Description  |
|---------|---------|
|Réinitialiser    |Effectue une réinitialisation sur le stockage connecté des paramètres.         |
|importer   |Importer des données à partir du fichier XML spécifié dans un espace de stockage connectés.         |
|Exportation   |Exporte les données à partir d’un espace de stockage connecté dans le fichier XML spécifié.         |
|supprimer   |Supprime les données à partir d’un espace de stockage connectés.         |
|générer |Génère des données fictives et l’enregistre dans le fichier XML spécifié.         |
|Simuler |Simule les conditions d’espace de stockage insuffisant.         |

### <a name="xbstorage-reset"></a>Réinitialisation de Xbstorage

`xbstorage reset [/force]`

Efface toutes les données locales dans le stockage connecté à partir de la console locale, restauration des paramètres d’usine. Les données qui ont été conservées dans le cloud ne sont pas modifiées et seront de nouveau téléchargées si nécessaires.

|Option  |Description  |
|---------|---------|
|/force   |Confirme que le stockage connecté doit être réinitialisé. Exécution de la commande de réinitialisation sans **/force** provoque le message suivant s’affiche :   En tant que fabrique de stockage connecté réinitialisation est une opération potentiellement destructive cette commande n’effectue pas la réinitialisation, sauf si le **/force** indicateur est présent. |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Importation de Xbstorage

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

Importe les données spécifiées dans *filename* à un espace de stockage connectés.
Le fichier est un fichier XML qui contient les données. Pour obtenir un exemple, consultez [xbstorage générer](#xbstorage_generate). Pour plus d’informations sur le format du fichier XML, consultez [format de fichier d’importation et exportation](#xbstorage_fileformat), plus loin dans cette rubrique.
Il existe deux façons de spécifier l’espace de stockage connecté :

- Si le fichier d’entrée a un **ContextDescription** section correctement rempli, il sera alors utilisée pour spécifier l’espace de stockage connectés.
- L’espace de stockage peut également être partiellement ou entièrement spécifié par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs de l’espace de stockage spécifié dans le fichier d’entrée.

Exemples d’utilisation :

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> Avant d’importer à l’espace de stockage connecté spécifié, le système tente de se synchroniser avec le cloud à l’aide de la même logique qui s’exécute lorsqu’un espace de stockage connecté est acquise par une application en cours d’exécution.
>
> Si une application avec le même SCID principal est en cours d’exécution, cette opération risque d’entraîner une condition de concurrence, et le contenu de l’espace de stockage connecté peut être dans un état indéterminé.
>
> Si **/Remplacez** n’est pas spécifié, les conteneurs spécifiés dans le fichier d’entrée seront effacées avant d’écrire les objets BLOB spécifiés dans le fichier d’entrée. Conteneurs dans l’espace de stockage connecté ne pas spécifié dans le fichier d’entrée restera inchangés.

|Option  |Description  |
|---------|---------|
|*file-name*     |Spécifie un fichier XML qui contient les données à importer.         |
|/scid :*SCID*    |Spécifie l’identificateur de Configuration de Service (SCID).         |
|/machine        |Spécifie un espace de stockage connectés par machine.  Cette option ne peut pas être utilisée simultanément avec le **/msa** option.         |
|/msa:*account*  |Spécifie un compte à utiliser pour le stockage par l’utilisateur connecté. L’utilisateur doit être connecté à la console pour l’espace à utiliser.  Cette option ne peut pas être utilisée simultanément avec le **/machine** option.         |
|/replace        |Supprime tous les conteneurs dans l’espace de stockage connecté spécifié avant l’importation.         |
|/verbose        |Affiche l’état de l’importation.         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Exportation de Xbstorage

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Exporte des données à partir d’un espace de stockage connecté vers le fichier spécifié par **outfile**.    Le fichier est un fichier XML qui contient les données. Consultez [xbstorage générer](#xbstorage_generate) pour voir comment générer un exemple. Pour plus d’informations sur le format du fichier XML, consultez [format de fichier d’importation et exportation](#xbstorage_fileformat), plus loin dans cette rubrique. Il existe deux façons de spécifier l’espace de stockage connecté :

- Si le **/context** paramètre est utilisé, et le nom de fichier spécifié par \<infile > a un **ContextDescription** section correctement rempli, puis ce fichier permet de spécifier le Espace de stockage connecté.
- L’espace de stockage peut également être partiellement ou entièrement spécifié par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs de l’espace de stockage spécifié dans le **/context** fichier.

Exemples d’utilisation :

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> Avant d’exporter l’espace de stockage connecté spécifié, le système tente de se synchroniser avec le cloud à l’aide de la même logique qui s’exécute lorsqu’un espace de stockage connecté est acquise par une application en cours d’exécution.
>
> Si une application avec le même SCID principal est en cours d’exécution, cette opération risque d’entraîner une condition de concurrence, et le contenu de l’espace de stockage connecté peut être dans un état indéterminé.

|Option  |Description  |
|---------|---------|
|*outfile*             |Fichier XML les données est exportée vers.         |
|/context:*input-file* |Spécifie un fichier d’entrée à partir duquel lire les informations d’espace.         |
|/scid :*SCID*          |Spécifie l’identificateur de configuration de service (SCID).         |
|/machine              |Spécifie un espace de stockage connectés par machine.  Cette option ne peut pas être utilisée simultanément avec le **/msa** option.         |
|/msa:*account*        |Spécifie un compte à utiliser pour le stockage par l’utilisateur connecté. L’utilisateur doit être connecté à la console pour l’espace à utiliser.  Cette option ne peut pas être utilisée simultanément avec le **/machine** option.         |
|/verbose              |Affiche l’état de l’opération d’exportation.         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Suppression de Xbstorage

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Supprime toutes les données à partir d’un espace de stockage connectés.
Il existe deux façons de spécifier l’espace de stockage connecté :

- Si le **/context** paramètre est utilisé, et le nom de fichier spécifié par \<infile > a un **ContextDescription** section correctement rempli, puis ce fichier permet de spécifier le Espace de stockage connecté.
- L’espace de stockage peut également être partiellement ou entièrement spécifié par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs de l’espace de stockage spécifié dans le **/context** fichier.

Exemples d’utilisation :

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> Avant de supprimer l’espace de stockage connecté spécifié, le système tente de se synchroniser avec le cloud à l’aide de la même logique qui s’exécute lorsqu’un espace de stockage connecté est acquise par une application en cours d’exécution.
>> Si une application avec le même SCID principal est en cours d’exécution, cette opération risque d’entraîner une condition de concurrence, et le contenu de l’espace de stockage connecté peut être dans un état indéterminé.

|Option  |Description |
|---------|---------|
|/context:*input-file*     |Spécifie un fichier d’entrée à partir duquel lire les informations d’espace.         |
|/scid :*SCID*              |Spécifie l’identificateur de configuration de service (SCID).         |
|/machine                  |Spécifie un espace de stockage connectés par machine.  Cette option ne peut pas être utilisée simultanément avec le **/msa** option.         |
|/msa:*account*            |Spécifie un compte à utiliser pour le stockage par l’utilisateur connecté. L’utilisateur doit être connecté à la console pour l’espace à utiliser.  Cette option ne peut pas être utilisée simultanément avec le **/machine** option.         |
|/verbose                  |Affiche l’état de l’opération de suppression.         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Xbstorage générer

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

Génère des données fictives et l’enregistre dans un fichier spécifié par \<filename >. Pour plus d’informations sur le format du fichier XML, consultez [format de fichier d’importation et exportation](#xbstorage_fileformat), plus loin dans cette rubrique.    L’identificateur de configuration de service (SCID) sera défini sur 00000000-0000-0000-0000-000000000000, et le compte sera défini pour un espace de stockage connectés par machine. Si vous souhaitez modifier ces valeurs, vous pouvez modifier le fichier directement après sa création.

Exemples d’utilisation :

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> Les données d’octets sont une simple séquence croissante ; par exemple, un objet blob de cinq octets aurait octets suivants : 00 01 02 03 04. >> Si vous souhaitez spécifier un espace de stockage connectés par l’utilisateur, modifiez le **compte** nœud dans le fichier XML pour ce qui suit :
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|Option  |Description  |
|---------|---------|
|*file-name*     | Fichier XML les données est écrites dans. |
|/Containers :*n* | Spécifie le nombre, *n*, de conteneurs à générer. Le nombre par défaut est 2.  |
|/ objets BLOB :*n*      | Spécifie le nombre, *n*, d’objets BLOB à générer. Le nombre par défaut est 3.  |
|/blobsize :*n*   | Spécifie le nombre, *n*, d’octets par objet blob. La taille par défaut est 1 024 octets.  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Simuler Xbstorage

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

Simule les conditions de stockage local pour le service de stockage connecté insuffisant.

|Option  |Description  |
|---------|---------|
|/reserveremainingspace | Réserve tout l’espace restant dans le stockage connecté. Espace que vous pouvez utiliser quelque chose si vous supprimez ConnectedStorage s’ouvre. |

| / forceoutoflocalstorage | Simule le Service de stockage connecté n’ayant aucun espace disponible restant. Suppression de quelque chose à partir du stockage connecté ne changera pas le service de stockage connectés à partir de la création de rapports en dehors de la mémoire. |

| / arrêter | Arrête toutes les simulations. |

| /verbose | Affiche l’état de l’opération simulée. |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>Options courantes

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|Option  |Description  |
|---------|---------|
| /?                           |  Affiche l’aide de xbstorage.exe |
| /X *:address* [*+accesskey*]  | Spécifie le nom d’hôte ou l’adresse (indiqué en tant que **outils IP** sur la console) d’une console ciblée, mais ne modifie ne pas la console par défaut. Si vous n’utilisez pas cette option, la console par défaut est utilisée. *Accesskey* est une chaîne que vous pouvez utiliser pour restreindre l’accès à une console uniquement aux personnes qui connaissent la clé d’accès. Définir la clé d’accès à l’aide de la commande **xbconfig** **accesskey = *** votre clé*; ensuite, redémarrez votre console pour que la clé d’accès devienne effectif. Pour accéder à une console qui est configurée avec une clé d’accès, vous devez inclure un signe plus (+) et la clé d’accès après le nom d’hôte ou adresse IP de la console.
> [!NOTE]
> Si une clé d’accès est fournie lors de la console par défaut est définie par xbconnect, la clé d’accès est stockée en tant que partie de l’adresse de la console par défaut.
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* vous permet de gérer le stockage de connecté mis en cache localement pour votre application avec toutes les mêmes fonctions que *xbstorage* fournit. Tout comme l’outil xbstorage, *gamesaveutil* offre les mêmes six données la gestion des fonctions, avec quelques différences de comportement. Supprimer l’importation, exportation, et les commandes de réinitialisation requièrent un utilisateur de Xbox Live, de se connecter. Vous pouvez utiliser l’application Xbox dans Windows 10 pour afficher et modifier l’utilisateur actuel.

### <a name="commands"></a>Commandes

|Commande  |Description  |
|---------|---------|
|importer   |Importe des données à partir du fichier XML spécifié         |
|Exportation   |Exporte des données vers le fichier xml spécifié         |
|supprimer   |Supprime les données de l’application spécifiée        |
|Réinitialiser    |Supprime les données locales uniquement pour l’application spécifiée         |
|générer |génère des données fictives et l’enregistre dans le fichier xml spécifié         |
|Simuler |simule les conditions spéciales qui sont difficiles à tester         |

### <a name="gamesaveutil-import"></a>Importation de Gamesaveutil

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

Importe les données spécifiées dans \<filename >

Le fichier est un fichier XML qui contient les données. Type `gamesaveutil help generate` pour voir comment générer un exemple.

Il existe deux façons de spécifier l’application NFP et SCID où les données sont enregistrées :

Si le fichier d’entrée a une section ContextDescription correctement rempli, il sera utilisé pour spécifier l’application cible NFP et SCID.

Le NFP et SCID peuvent être partiellement ou entièrement spécifiés par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs des SCID NFP spécifié à partir du fichier d’entrée.

|Option  |Description  |
|---------|---------|
|/pfn :\<NFP >       |Spécifie la famille de Package Name(PFN) pour l’application pour effectuer l’importation de. L’application doit être installée.         |
|/scid :\<SCID >     |Spécifie l’identificateur de Configuration de Service (SCID). Il s’agit de votre configuration de Xbox Live.         |
|/replace         |Supprimer tous les conteneurs avant d’effectuer l’importation.         |

Exemples d’utilisation :

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> L’application doit être installé et synchroniser avec données déjà pour pouvoir importer.
> 
> Si /replace n’est pas spécifié, les conteneurs existants ne seront pas affectés, sauf si elles sont spécifiées dans le fichier d’entrée.

### <a name="gamesaveutil-export"></a>Exportation de Gamesaveutil

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Exporte des données vers le fichier spécifié par \<outfile >.

Le fichier est un fichier XML qui contient les données. Tapez gamesaveutil help générer pour voir comment générer un exemple.

Il existe deux façons de spécifier l’emplacement des données à exporter :

Si le paramètre /context est utilisé, et le nom de fichier spécifié par \<infile > a une section ContextDescription correctement rempli, ce fichier sera alors utilisée pour spécifier l’emplacement de la source de données.

L’emplacement peut également être spécifié par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs spécifiés par le fichier /context.

|Option  |Description  |
|---------|---------|
|/context:\<infile>     |Utiliser le fichier spécifié pour spécifier l’application NFP et SCID.         |
|/pfn :\<NFP >            |Spécifie la famille de Package Name(PFN) pour l’application pour effectuer l’exportation à partir de. L’application doit être installée.       |
|/scid :\<SCID >          |Spécifie l’identificateur de Configuration de Service (SCID). Il s’agit de votre configuration de Xbox Live.        |

Exemples d’utilisation :

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> L’application doit être installé et synchroniser avec données déjà afin d’exporter.

### <a name="gamesaveutil-delete"></a>Gamesaveutil delete

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Supprime toutes les données pour le NFP et SCID spécifiés.

Il existe deux façons de spécifier l’emplacement des données à supprimer :

Si le paramètre /context est utilisé, et le nom de fichier spécifié par \<infile > a une section ContextDescription correctement rempli, ce fichier sera alors utilisée pour spécifier l’emplacement de la source de données.

L’emplacement peut également être spécifié par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs spécifiés par le fichier /context.

|Option  |Description  |
|---------|---------|
|/context:\<infile>     |Utiliser le fichier spécifié pour spécifier l’application NFP et SCID.         |
|/pfn :\<NFP >            |Spécifie la famille de Package Name(PFN) pour l’application pour supprimer les données à partir de. L’application doit être installée.       |
|/scid :\<SCID >          |Spécifie l’identificateur de Configuration de Service (SCID). Il s’agit de votre configuration de Xbox Live.        |

Exemples d’utilisation :

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> L’application doit être installée pour pouvoir supprimer les conteneurs.

### <a name="gamesaveutil-reset"></a>Réinitialisation de Gamesaveutil

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Supprime toutes les données locales pour le NFP et SCID spécifiés.

Il existe deux façons de spécifier l’emplacement des données à supprimer :

Si le paramètre /context est utilisé, et le nom de fichier spécifié par \<infile > a une section ContextDescription correctement rempli, ce fichier sera alors utilisée pour spécifier l’emplacement de la source de données.

L’emplacement peut également être spécifié par le biais des paramètres de ligne de commande, qui sont prioritaires sur les éléments respectifs spécifiés par le fichier /context.

|Option  |Description  |
|---------|---------|
|/context:\<infile>     |Utiliser le fichier spécifié pour spécifier l’application NFP et SCID.         |
|/pfn :\<NFP >            |Spécifie la famille de Package Name(PFN) pour l’application pour supprimer les données à partir de. L’application doit être installée.       |
|/scid :\<SCID >          |Spécifie l’identificateur de Configuration de Service (SCID). Il s’agit de votre configuration de Xbox Live.        |

Exemples d’utilisation :

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> L’application doit être installée pour pouvoir supprimer les données locales.

### <a name="gamesaveutil-generate"></a>Gamesaveutil générer

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

Génère des données fictives et l’enregistre dans un fichier spécifié par \<filename >.
L’identificateur de Configuration de Service (SCID) est défini à 00000000-0000-0000-0000-000000000000. Modifiez le fichier manuellement après génération pour modifier ces valeurs si vous le souhaitez.

|Option  |Description  |
|---------|---------|
|/Containers :\<n >     |Spécifie le nombre de conteneurs à générer. Valeur par défaut est 2.         |
|/ objets BLOB :\<n >          |Spécifie combien d’objets BLOB par conteneur à générer. La valeur par défaut est 3.        |
|/blobsize :\<n >       |Spécifie le nombre d’octets par objet blob. Valeur par défaut est 1024.         |

Exemples d’utilisation :

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> Les données d’octets sont une simple séquence croissante, par exemple, un objet blob de cinq octets aurait les octets 00 01 02 03 04.

### <a name="gamesaveutil-simulate"></a>Simuler Gamesaveutil

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

Simule les conditions spéciales pour certains scénarios de test.

|Option  |Description  |
|---------|---------|
|/markcontainerschanged     |Force tous les conteneurs s’affichent comme elles ont été modifiées lorsqu’une application a repris à partir de la suspension et obtient un nouveau fournisseur. Affecte toutes les applications jusqu'à ce qu’effacée avec /stop.      |
|/stop                      |Arrête toutes les simulations.         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>Format de fichier d’importation et exportation

Le fichier XML utilisé avec le **importer**, **exporter**, et **générer** commandes avec la *xbstorage* outil a le format indiqué dans le exemple suivant.

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

La seule modification nécessaire pour mettre en forme ce code xml pour **importer**, **exporter**, et **générer** dans *gamesaveutil* consiste à remplacer le \<Compte > nœud membre de la \<ContextDescription > nœud avec un \<PackageFamilyName > nœud.
Cela modifiera le \<ContextDescription > nœud à partir de ce :

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

Par ceci :

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> Le format des données dans ces fichiers XML n’est pas identique à ce qui est sur la plateforme, il est pour l’importation et d’exportation uniquement à des fins. Le format de données pour ces fichiers XML pourrait changer à l’avenir, afin qu’ils doivent être traités comme un format intermédiaire ou de l’utilitaire, pas un format d’archivage.

Le **ContextDescription** nœud est facultatif. Si vous créez un espace de stockage connecté pour un ordinateur, vous pouvez utiliser `<Account machine="true"/>` au lieu de `<Account msa="user@domain.com"/>`. Sinon, le contexte peut être spécifié sur la ligne de commande pour l’importation.
Objets BLOB et conteneurs doivent avoir les noms correspondants qui leur est accordées par le jeu ou d’une application pour laquelle le fichier est généré.
Le contenu de chaque objet blob doit être une chaîne encapsulée dans un **CDATA** balise, qui est généré en appelant **CryptBinaryToStringW** avec l’indicateur **CRYPT_STRING_BASE64** qui fournit les données pour cet objet blob en tant que tableau d’octets bruts. À l’inverse, les données d’objets blob peuvent être converties en appelant **CryptStringToBinary** et en fournissant la chaîne chiffrée précédemment. Voici un exemple d’utilisation de ces deux fonctions dans [CryptBinaryToString retourne les octets non valides](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral) dans les forums MSDN pour Visual Studio.

<a id="ID4EWBAE"></a>