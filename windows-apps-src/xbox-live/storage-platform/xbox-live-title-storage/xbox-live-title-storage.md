---
title: Xbox Live titre stockage
description: Découvrez comment utiliser le stockage de titre Xbox Live pour stocker des informations sur les jeux pour un titre dans le cloud.
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 472a2131c113cdde5d7383e169e4fbe638e4e555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623924"
---
# <a name="xbox-live-title-storage"></a>Xbox Live titre stockage

Le service de stockage de titre Xbox Live fournit un moyen de stocker des informations sur les jeux pour un titre dans le cloud. Jeux qui s’exécutent sur toutes les plateformes peuvent utiliser ce service.

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Fonctionnalités de stockage de titre Xbox Live

Certaines des fonctionnalités de haut niveau de stockage de titre Xbox Live incluent, mais ne sont pas limités à :

-   Peuvent être partagées entre les utilisateurs, les titres et les différentes plateformes
-   Prend en charge JSON, binaire et les fichiers de configuration

Les principales fonctionnalités de stockage de titre Xbox Live sont expliquées plus en détail dans les sections suivantes :

-   [Types de stockage](#ID4ETB)
-   [Types de données](#ID4ECF)
-   [URI de stockage du titre](#ID4EBEAC)
-   [Seuil de limitation](#ID4ETEAC)

<a name="ID4ETB"></a>

Pour les partenaires gérés et ID@Xbox membres :

| Type de stockage       | Quota (gérés Partners/ID@Xbox) | Quota (programme Xbox Live Creators) |  Objectif                                                                                                                                                      | Plateformes                                                                                           | Utilisateurs                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| Plateforme sécurisée   | 256 Mo par utilisateur | 64 Mo par utilisateur    | Jeux de données tels qu’enregistrées par l’utilisateur ou l’état du jeu de lecture/pause/reprise. Mais plus sécurisé, avec les restrictions de plateforme. | N’importe quelle plateforme peut lire, mais uniquement Xbox One, Xbox 360 ou Windows Phone peut écrire.  | Configurable au public ou propriétaire uniquement.       |
| Plateforme universelle | 64 Mo par utilisateur | 64 Mo par utilisateur    | Jeux de données tels qu’enregistrées par l’utilisateur ou l’état du jeu de lecture/pause/reprise. | N’importe quelle plateforme peut écrire, mais uniquement les plateformes autres que Xbox One, Xbox 360 ou Windows Phone peuvent lire. | Configurable au public ou propriétaire uniquement.       |
| Globale             | 256 MO | 256 MO            | Données que tout le monde peut lire, tels que les mémos, des cartes, des défis ou des ressources de l’art. | Uniquement est accessible en écriture via le portail des développeurs Xbox ou partenaires, n’importe quelle plateforme peut lire.                                | Tous les utilisateurs peuvent lire.

### <a name="deprecated-storage-types"></a>Types de stockage obsolète

Les types de stockage suivants sont déconseillés. Elles sont prises en charge uniquement pour les titres qui les utilisent actuellement. Ils ne sont pas disponibles pour les nouveaux titres.

| Type de stockage       | Le quota  |   Objectif                                                                                                                                                      | Plateformes                                                                                           | Utilisateurs                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 64 Mo par utilisateur     | Jeux de données tels qu’enregistrées par l’utilisateur ou l’état du jeu de lecture/pause/reprise. Plus ne sécurisé, aucune restriction de la plateforme, mais avec des données de format restrictions (JSON uniquement). | N’importe quelle plateforme peut lire ou écrire.                                                                     | Configurable au public ou propriétaire uniquement.       |
| Appareil             | 64 Mo par appareil   | Données spécifiques à un appareil, telles que les paramètres ou les préférences de l’appareil.                                                                                            | Uniquement Xbox One, Xbox 360 ou Windows Phone peut écrire. Seul l’appareil qui a écrit les données peut lire.  | Tous les utilisateurs peuvent lire.                         |
| Stockage de session    | 256 Mo par session | Données pour toute personne se joint à une session multijoueur particulier.                                                                                             | N’importe quelle plateforme qui peut-être rejoindre la session.                                                             | Tous les utilisateurs dans la session peuvent lire ou écrire. |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>Types de données

Jeux de spécifient le type de données à utiliser dans le **{type}** paramètre d’une méthode GET ou PUT. La section suivante décrit les trois types de prises en charge :

-   [Informations binaires](#ID4ENF)
-   [Informations JSON](#ID4EUF)
-   [informations de configuration](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>Informations binaires

Images, des sons et des données personnalisées utilisent le type binaire. Étant donné que les données doivent être transmises via HTTP, les données binaires doivent être encodées en caractères HTTP accepte. Par exemple, vous pouvez convertir les données en chaînes hexadécimales ou utiliser l’encodage base64. Le système de stockage de titre ne traite pas les données encodées, donc votre jeu doit utiliser le même schéma de codage et décodage des données lors de la lecture et écriture dans le stockage de titre.

<a name="ID4EUF"></a>

#### <a name="json-information"></a>Informations JSON

Données structurées peuvent utiliser le type JSON. Objets JSON peuvent être utilisés directement dans les langages, tels que JavaScript, qui les prennent en charge. Lorsque vous récupérez des données à partir de fichiers JSON, le jeu peut fournir un *sélectionnez* paramètre pour retourner des éléments spécifiques au sein de la structure. Par exemple, utilisez un fichier au format JSON qui contient les informations suivantes :

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| Remarque                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pour des raisons de sécurité, le premier élément des données JSON ne doit pas être un tableau. Les données JSON envoyées avec un tableau à la racine sont rejetées par le service. |

Jeux peuvent sélectionner des parties de cette structure avec une requête comme suit :

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

Le corps de réponse pour cette requête est :

    {
        "name" : "poison"
    }

Le tableau est accessible avec une requête comme suit :

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

Le corps de réponse pour cette requête est :

    {
        "quest" : "queen"
    }

Les restrictions de longueur suivantes sont appliquées pour les données JSON :

-   Valeur numérique, la longueur maximale = 32
-   Valeur, la longueur maximale de chaîne = 1024
-   Nom de la propriété, la longueur maximale = 64
-   Hiérarchie, la profondeur maximale = 16
-   Tableau, la taille maximale = 1024
-   Les propriétés enfants, maximum dans un objet = 1024

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>informations de configuration

Le **{type}** peut être **config** pour indiquer que les données soient un objet blob de configuration. Objets BLOB de configuration sont des structures de données qui sont stockés dans le stockage global de titre. Le format de l’objet blob est similaire à un objet JSON.

Objets BLOB de configuration peut inclure des nœuds virtuels qui retournent un paramètre dans une liste de possibilités. Nœuds virtuels sont utiles pour fournir des paramètres pour des situations spécifiques, comme un titre ou de paramètres régionaux. Le nœud virtuel inclut plusieurs paramètres possibles, ainsi que les valeurs et les conditions de sélection à partir des valeurs. Dans l’exemple suivant, le **defaultCardDesign** paramètre peut avoir une des valeurs dans le nœud virtuel.

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

Lorsqu’un jeu lit ce fichier, le système sélectionne une des valeurs à partir de la  **\_sourceNodes** tableau. Dans ce cas, l’élément est sélectionné en fonction de l’ID de titre du jeu. Les utilisateurs à jouer le jeu **12456799** consultez :

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

Le reste des utilisateurs, consultez :

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

Jeux peuvent définir des sélecteurs personnalisés qui correspond à un paramètre dans la demande. Par exemple, dans cet objet blob de configuration :

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

Jeux passer une chaîne le **customSelector** paramètre pour sélectionner l’élément à retourner. Par exemple, pour obtenir la deuxième option, un jeu de requêtes :

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

Le  **\_by** valeur indique le type de sélection à faire et le  **\_sélecteur** valeur indique les données à utiliser dans la sélection. Les valeurs possibles sont :

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p>Le <strong>_selector</strong> correspond à l’ID de titre dans la revendication spécifiée.</p></td>
</tr>
<tr>
<td >Paramètres régionaux</td>
<td ><p>Le <strong>_selector</strong> correspond à la chaîne de paramètres régionaux à partir de l’en-tête Accept-Language.</p></td>
</tr>
<tr>
<td >Personnalisé</td>
<td ><p>Le <strong>_selector</strong> correspond à une chaîne personnalisée passée dans le <strong>customSelector</strong> paramètre de requête. Le <strong>customSelector</strong> contient une ou plusieurs requêtes séparées par des virgules. Chaque requête est le nom de la <strong>by</strong> élément et la valeur de la <strong>_selector</strong> élément.</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>URI de stockage du titre

URI de stockage de titre sont mis en forme comme suit :

    https://titlestorage.xboxlive.com/{path}

Le **{path}** partie de l’URI est le type de requête effectuée et doit être 245 caractères ou moins.

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>Seuil de limitation

Il n’existe aucune limite fixe sur le nombre de lectures ou écritures un titre peut rendre par minute, mais il ne peut pas font généralement plusieurs fois par minute en moyenne dans une session d’une heure. Par exemple, un titre peut apporter à 60 lectures ou écritures au début d’une session, mais pas plus pour le reste de l’heure. Titres doivent être renforcées contre plusieurs appels plus tard, au cas où les Services Xbox LIVE doit limiter les demandes.

Si votre titre a des exigences de partitionnement particulières, telles que les lectures supplémentaires ou des écritures, contactez Microsoft.

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>L’utilisation du stockage de titre

Pour bien démarrer avec stockage de titre, vous devez d’abord déterminer quel type de données que vous souhaitez stocker. Certains exemples incluent les jeux enregistrés, état du jeu, défis quotidiens liés aux, jeux de cartes et les ressources de la bibliothèque.

Ensuite, déterminez les titres et les plateformes devront accéder aux données. Titre stockage prend en charge cloud accès aux données à partir d’un titre particulier sur une plateforme unique et à partir de plusieurs titres sur plusieurs plateformes.

Enfin, utilisez les rubriques de cette section pour configurer votre stockage, télécharger vos données et définir des autorisations d’accès appropriée en fonction de votre choix.

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>Dans cette section

[Lecture d’un objet Blob de Configuration dans le stockage Xbox Live titre](reading-configuration-blobs.md)  
Illustre la lecture stockage de titre des objets BLOB de configuration à partir de la Xbox Live.

[Stockage d’un Blob binaire dans le stockage de titre en direct Xbox](storing-binary-blobs.md)  
Illustre le stockage d’objets BLOB binaire dans le stockage de titre de Xbox Live.

[Lire un Blob binaire dans le stockage Xbox Live titre](reading-binary-blobs.md)  
Illustre la lecture des objets BLOB binaires à partir du stockage de titre Xbox Live.

[Stocker un objet Blob JSON dans le stockage Xbox Live titre](storing-jsonblobs.md)  
Montre le stockage BLOB JSON dans le stockage de titre Xbox Live.

[Lecture d’un objet Blob JSON dans le stockage Xbox Live titre](reading-jsonblobs.md)  
Illustre la lecture des objets BLOB JSON à partir de la Xbox Live stockage du titre.

<a name="ID4E4FAC"></a>
