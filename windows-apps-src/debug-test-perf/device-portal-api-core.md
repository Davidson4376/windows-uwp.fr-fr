---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Informations de référence sur les API principales Device Portal
description: Découvrez les API REST principales Windows Device Portal que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programmation.
ms.date: 4/8/2019
ms.topic: article
keywords: Windows 10, uwp, le portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 58ae7d83c0889131313d136c13048b83a861f601
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244135"
---
# <a name="device-portal-core-api-reference"></a>Informations de référence sur les API principales Device Portal

Toutes les fonctionnalités du portail d’appareil sont basées sur des API REST que les développeurs peuvent appeler directement pour accéder aux ressources et contrôler leurs appareils par programme.

## <a name="app-deployment"></a>Déploiement des applications

### <a name="install-an-app"></a>Installer une application

**Demande**

Vous pouvez installer une application en utilisant le format de requête suivant.

| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/app/packagemanager/package |

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| package   | (**requis**) Nom de fichier du package à installer. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Fichier .appx ou .appxbundle, ainsi que toutes les dépendances dont l’application a besoin. 
- Certificat utilisé pour signer l’application, s’il s’agit d’un appareil IoT ou de bureau Windows. Les autres plateformes n’exigent pas le certificat. 

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
| 200 | Requête de déploiement acceptée et traitée |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Installer un ensemble connexe

**Demande**

Vous pouvez installer un [ensemble connexe](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) en utilisant le format de requête suivant.

| Méthode      | URI de requête |
| :------     | :------ |
| PUBLIER | /api/app/packagemanager/package |

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| package   | (**requis**) Noms de fichiers des packages à installer. |

**En-têtes de requête**

- Aucune

**Corps de la requête** 
- Ajoutez « .opt » aux noms des fichiers de packages facultatifs lorsque vous les spécifiez en tant que paramètre, comme suit : « foo.appx.opt » ou « bar.appxbundle.opt ». 
- Fichier .appx ou .appxbundle, ainsi que toutes les dépendances dont l’application a besoin. 
- Certificat utilisé pour signer l’application, s’il s’agit d’un appareil IoT ou de bureau Windows. Les autres plateformes n’exigent pas le certificat. 

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
| 200 | Requête de déploiement acceptée et traitée |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Inscrire une application dans un dossier isolé

**Demande**

Vous pouvez inscrire une application dans un dossier isolé en utilisant le format de requête suivant.

| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/app/packagemanager/networkapp |

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    }
}
```

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
| 200 | Requête de déploiement acceptée et traitée |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Inscrire un ensemble connexe dans des dossiers de fichiers isolés

**Demande**

Vous pouvez enregistrer un [ensemble connexe](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) dans des dossiers isolés en utilisant le format de requête suivant.

| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/app/packagemanager/networkapp |

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    },
    "optionalpackages" :
    [
        {
            "networkshare" : "\\some\share\path2",
            "username" : "optional_username2",
            "password" : "optional_password2"
        },
        ...
    ]
}
```

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
| 200 | Requête de déploiement acceptée et traitée |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtenir l’état de l’installation de l’application

**Demande**

Vous pouvez obtenir l’état d’installation d’une application actuellement en cours d’exécution en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/app/packagemanager/state |

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
| 200 | Le résultat du dernier déploiement |
| 204 | L’installation est en cours d’exécution |
| 404 | Aucune action d’installation n’a été détectée |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Désinstaller une application

**Demande**

Vous pouvez désinstaller une application en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/app/packagemanager/package |

**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------          | :------ |
| package   | (**obligatoire**) PackageFullName (à partir de GET /api/app/packagemanager/packages) de l’application cible |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtenir des applications installées

**Demande**

Vous pouvez obtenir une liste des applications installées sur le système en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/app/packagemanager/packages |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend une liste des packages installés avec les détails associés. Le modèle de cette réponse est le suivant.
```json
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
## Bluetooth
<hr>

### <a name="get-the-bluetooth-radios-on-the-machine"></a>Obtenir les adaptateurs Bluetooth sur l’ordinateur

**Demande**

Vous pouvez obtenir une liste des adaptateurs Bluetooth installés sur l’ordinateur en utilisant le format de requête suivant. Cela peut mis à niveau vers une connexion de WebSocket, avec les mêmes données JSON.
 
| Méthode        | URI de requête |
| :------          | :------ |
| GET           | /API/BT/getradios |
| GET/WebSocket | /API/BT/getradios |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut un tableau d’appareils JSON d'adaptateurs Bluetooth joint à l’appareil.
```json
{"BluetoothRadios" : [
    {
        "BluetoothAddress" : int64,
        "DisplayName" : string,
        "HasUnknownUsbDevice" : boolean,
        "HasProblem" : boolean,
        "ID" : string,
        "ProblemCode" : int,
        "State" : string
    },...
]}
```
**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP | Description |
| :------             | :------ |
| 200              | OK |
| 4XX              | Codes d’erreur |
| 5XX              | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
### Activer ou désactiver l'adaptateur Bluetooth

**Demande**

Définit un Bluetooth Bluetooth spécifique sur Activé ou Désactivé.
 
| Méthode | URI de requête |
| :------   | :------ |
| PUBLIER   | /api/bt/setradio |

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| ID            | (**requis**) ID de l'appareil de l'adaptateur Bluetooth ; il doit être codé en base 64. |
| État         | (**requis**) il peut s’agir `"On"` ou `"Off"`. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP | Description |
| :------             | :------ |
| 200              | OK |
| 4XX              | Codes d’erreur |
| 5XX              | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
## Gestionnaire de périphériques
<hr>
### Obtenir les périphériques installés sur l’ordinateur

**Demande**

Vous pouvez obtenir une liste des périphériques installés sur l’ordinateur en utilisant le format de requête suivant.

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/devicemanager/devices |

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut un tableau d’appareils JSON joint à l’appareil.
```json
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* IoT

<hr>
### Obtenir des données sur des périphériques/hubs USB connectés

**Demande**

Vous pouvez obtenir la liste des descripteurs USB des hubs et appareils USB en utilisant le format de requête suivant.

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /ext/devices/usbdevices |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse est JSON. Elle inclut des ID de périphérique pour le périphérique USB, ainsi que des descripteurs USB et des informations de port pour les hubs.
```json
{
    "DeviceList": [
        {
        "ID": string,
        "ParentID": string, // Will equal an "ID" within the list, or be blank
        "Description": string, // optional
        "Manufacturer": string, // optional
        "ProblemCode": int, // optional
        "StatusCode": int // optional
        },
        ...
    ]
}
```

**Exemple de données renvoyées**
```json
{
    "DeviceList": [{
        "ID": "System",
        "ParentID": ""
    }, {
        "Class": "USB",
        "Description": "Texas Instruments USB 3.0 xHCI Host Controller",
        "ID": "PCI\\VEN_104C&DEV_8241&SUBSYS_1589103C&REV_02\\4&37085792&0&00E7",
        "Manufacturer": "Texas Instruments",
        "ParentID": "System",
        "ProblemCode": 0,
        "StatusCode": 25174026
    }, {
        "Class": "USB",
        "Description": "USB Composite Device",
        "DeviceDriverKey": "{36fc9e60-c465-11cf-8056-444553540000}\\0016",
        "ID": "USB\\VID_045E&PID_00DB\\8&2994096B&0&1",
        "Manufacturer": "(Standard USB Host Controller)",
        "ParentID": "USB\\VID_0557&PID_8021\\7&2E9A8711&0&4",
        "ProblemCode": 0,
        "StatusCode": 25182218
    }]
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
## Collection de vidages
<hr>
### Obtenir la liste de tous les vidages sur incident pour les applications

**Demande**

Vous pouvez obtenir la liste de tous les vidages sur incident disponibles pour toutes les applications chargées de manière indépendante en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/usermode/dumps |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend une liste des vidages sur incident pour chaque application chargée de manière indépendante.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile (dans le programme Windows Insider)
* Bureau Windows
* HoloLens
* IoT

<hr>
### Obtenir les paramètres de collection de vidage sur incident pour une application

**Demande**

Vous pouvez obtenir les paramètres de collection de vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashcontrol |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse a le format suivant.
```json
{"CrashDumpEnabled": bool}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile (dans le programme Windows Insider)
* Bureau Windows
* HoloLens
* IoT

<hr>
### Supprimer un vidage sur incident pour une application chargée de manière indépendante

**Demande**

Vous pouvez supprimer le vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/debug/dump/usermode/crashdump |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante. |
| fileName   | (**requis**) Nom du fichier de vidage à supprimer. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile (dans le programme Windows Insider)
* Bureau Windows
* HoloLens
* IoT

<hr>
### Désactiver les vidages sur incident pour une application chargée de manière indépendante

**Demande**

Vous pouvez désactiver les vidages sur incident pour une application chargée de manière indépendante en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/debug/dump/usermode/crashcontrol |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile (dans le programme Windows Insider)
* Bureau Windows
* HoloLens
* IoT

<hr>
### Télécharger le vidage sur incident pour une application chargée de manière indépendante

**Demande**

Vous pouvez télécharger le vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashdump |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante. |
| fileName   | (**requis**) Nom du fichier de vidage à télécharger. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend un fichier de vidage. Vous pouvez utiliser WinDbg ou Visual Studio pour examiner le fichier de vidage.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile (dans le programme Windows Insider)
* Bureau Windows
* HoloLens
* IoT

<hr>
### Désactiver les vidages sur incident pour une application chargée de manière indépendante

**Demande**

Vous pouvez activer les vidages sur incident pour une application chargée de manière indépendante en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/debug/dump/usermode/crashcontrol |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 

**Familles d’appareils disponibles**

* Windows Mobile (dans le programme Windows Insider)
* Bureau Windows
* HoloLens
* IoT

<hr>
### Obtenir la liste des fichiers de vérification d’erreur

**Demande**

Vous pouvez obtenir la liste des fichiers minidump de vérification d’erreur en utilisant le format de demande suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dumplist |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend une liste des noms de fichier de vidage et leur taille. Cette liste doit avoir le format suivant. 
```json
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Télécharger un fichier de vidage de vérification d’erreur

**Demande**

Vous pouvez télécharger un fichier de vidage de vérification d’erreur en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dump |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| filename   | (**requis**) Nom du fichier de vidage. Vous pouvez le rechercher à l’aide de l’API pour obtenir la liste de vidage. |


**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend le fichier de vidage. Vous pouvez examiner ce fichier à l’aide de WinDbg.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir les paramètres de contrôle d’incident de la vérification d’erreur

**Demande**

Vous pouvez obtenir la liste des paramètres de contrôle d’incident de la vérification d’erreur en utilisant le format de demande suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/kernel/crashcontrol |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend les paramètres de contrôle d’incident. Pour plus d’informations sur CrashControl, voir l’article [CrashControl](https://technet.microsoft.com/library/cc951703.aspx). Le modèle de la réponse est le suivant.
```json
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**Types de vidage**

0: Désactivée

1 : Image mémoire complète (collecte de toute la mémoire en cours d’utilisation)

2 : Vidage de la mémoire du noyau (ignore la mémoire en mode utilisateur)

3 : Minidump de noyau limitée

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir un vidage du noyau dynamique

**Demande**

Vous pouvez obtenir un vidage du noyau dynamique en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/livekernel |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend le vidage en mode noyau complet. Vous pouvez examiner ce fichier à l’aide de WinDbg.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir un vidage à partir d’un processus utilisateur dynamique

**Demande**

Vous pouvez obtenir le vidage pour le processus utilisateur dynamique en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/debug/dump/usermode/live |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| pid   | (**requis**) Id unique du processus qui vous intéresse. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend le fichier de vidage du processus. Vous pouvez examiner ce fichier à l’aide de WinDbg ou de Visual Studio.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir les paramètres de contrôle d’incident de la vérification d’erreur

**Demande**

Vous pouvez définir les paramètres de la collecte de données de vérification d’erreur en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/debug/dump/kernel/crashcontrol |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| autoreboot   | (**facultatif**) True ou false. Cette valeur indique si le système redémarre automatiquement suite à un échec ou à un verrouillage. |
| dumptype   | (**facultatif**) Type de vidage. Pour les valeurs prises en charge, consultez [Énumération CrashDumpType](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).|
| maxdumpcount   | (**facultatif**) Le nombre maximal de vidages à enregistrer. |
| overwrite   | (**facultatif**) True ou false. Cela indique s’il convient d’écraser ou non les anciens vidages lorsque le seuil du nombre de vidages défini par *maxdumpcount* est atteint. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
## ETW
<hr>
### Créer une session ETW en temps réel via un websocket

**Demande**

Vous pouvez créer une session ETW en temps réel en utilisant le format de requête suivant. Cette opération est gérée via un websocket.  Les événements ETW sont regroupés sur le serveur et envoyés vers le client une fois par seconde. 
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET/WebSocket | /api/etw/session/realtime |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend les événements ETW issus des fournisseurs activés.  Voir les commandes WebSocket ETW ci-dessous. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>Commandes WebSocket ETW
Ces commandes sont envoyées du client vers le serveur.

| Command | Description |
| :----- | :----- |
| provider *{guid}* enable *{level}* | Activez le fournisseur marqué par *{guid}* (sans crochets) au niveau spécifié. *{level}* est un entier **int** de 1 (peu détaillé) à 5 (très détaillé). |
| provider *{guid}* disable | Désactivez le fournisseur marqué par *{guid}* (sans crochets). |

Cette réponse est envoyée du serveur vers le client. Elle est envoyée sous forme de texte et vous obtenez le format suivant en analysant le JSON.
```json
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

Les objets de charge utile sont des paires clé-valeur supplémentaires (chaîne:chaîne) qui sont fournies dans l’événement ETW d’origine.

Exemple :
```json
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

<hr>
### Énumérer les fournisseurs ETW enregistrés

**Demande**

Vous pouvez énumérer les fournisseurs enregistrés en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/etw/providers |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend la liste des fournisseurs ETW. La liste comprend le nom convivial et le GUID de chaque fournisseur au format suivant.
```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
|  200 | OK | 

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Énumérez les fournisseurs ETW personnalisés exposés par la plate-forme.

**Demande**

Vous pouvez énumérer les fournisseurs enregistrés en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/etw/customproviders |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

200 OK. La réponse comprend la liste des fournisseurs ETW. La liste comprend le nom convivial et le GUID de chaque fournisseur.

```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
## Emplacement
<hr>

### <a name="get-location-override-mode"></a>Obtenir le mode remplacement de l’emplacement

**Demande**

Vous pouvez obtenir l'état de remplacement de la pile d'emplacements de l'appareil en utilisant le format de requête suivant. Le mode développeur doit être activé pour que cet appel aboutisse.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /ext/location/override |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut l'état de remplacement de l'appareil au format suivant. 

```json
{"Override" : bool}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
|  200 | OK | 
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

### <a name="set-location-override-mode"></a>Définir le mode remplacement de l’emplacement

**Demande**

Vous pouvez définir l'état de remplacement de la pile d'emplacements de l'appareil en utilisant le format de requête suivant. Lorsqu'elle est activée, la pile d’emplacements permet l’injection de position. Le mode développeur doit être activé pour que cet appel aboutisse.

| Méthode      | URI de requête |
| :------     | :----- |
| PUT | /ext/location/override |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

```json
{"Override" : bool}
```

**Réponse**

La réponse inclut l'état de remplacement auquel l'appareil a été défini, au format suivant. 

```json
{"Override" : bool}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

### <a name="get-the-injected-position"></a>Obtenir la position injectée

**Demande**

Vous pouvez obtenir l'emplacement injecté (falsifié) de l'appareil en utilisant le format de requête suivant. Un emplacement injecté doit être défini, ou une erreur sera levée.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /ext/location/position |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend les valeurs de longitude et de latitude actuelles injectées au format suivant. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

|  Code d’état HTTP      | Description | 
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

### <a name="set-the-injected-position"></a>Définir la position injectée

**Demande**

Vous pouvez définir l'emplacement injecté (falsifié) de l'appareil en utilisant le format de requête suivant. Le mode de remplacement de l’emplacement doit tout d’abord être activé sur l’appareil et l’emplacement défini doit être un emplacement valide ou une erreur sera levée.

| Méthode      | URI de requête |
| :------     | :----- |
| PUT | /ext/location/override |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Réponse**

La réponse inclut l'emplacement qui a été défini au format suivant. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
## Informations sur le système d’exploitation
<hr>
### Obtenir le nom de l’ordinateur

**Demande**

Vous pouvez obtenir le nom d’un ordinateur en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/os/machinename |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut le nom de l’ordinateur au format suivant. 

```json
{"ComputerName": string}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtenir les informations du système d’exploitation

**Demande**

Vous pouvez obtenir les informations du système d’exploitation pour un ordinateur en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/os/info |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut des informations sur le système d’exploitation au format suivant.

```json
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtenir la famille d’appareils 

**Demande**

Vous pouvez obtenir la famille d’appareils (Xbox, téléphone, ordinateur de bureau, etc.) en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/os/devicefamily |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend la famille d’appareils (référence : ordinateur de bureau, Xbox, etc.).

```json
{
   "DeviceType" : string
}
```

DeviceType aura pour valeur une chaîne du type « Windows.Xbox », « Windows.Desktop », etc. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtenir le nom de l’ordinateur

**Demande**

Vous pouvez définir le nom d’un ordinateur en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/os/machinename |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| name | (**requis**) Nouveau nom de l’ordinateur. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
## Informations utilisateur
<hr>
### Obtenir l’utilisateur actif

**Demande**

Vous pouvez obtenir le nom de l'utilisateur actif en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/users/activeuser |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut des informations sur l'utilisateur au format suivant. 

En cas de réussite : 
```json
{
    "UserDisplayName" : string, 
    "UserSID" : string
}
```
En cas d'échec :
```json
{
    "Code" : int, 
    "CodeText" : string, 
    "Reason" : string, 
    "Success" : bool
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
## Données relatives aux performances
<hr>
### Obtenir la liste des processus en cours d’exécution

**Demande**

Vous pouvez obtenir la liste des processus en cours d’exécution en utilisant le format de requête suivant.  Il peut également être mis à niveau vers une connexion WebSocket, avec les mêmes données JSON transmises au client une fois par seconde. 
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/resourcemanager/processes |
| GET/WebSocket | /api/resourcemanager/processes |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend une liste des processus et les détails associés. Les informations sont au format JSON et suivent le modèle suivant.
```json
{"Processes": [
    {
        "CPUUsage": float,
        "ImageName": string,
        "PageFileUsage": long,
        "PrivateWorkingSet": long,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": long,
        "WorkingSetSize": long
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Obtenir les statistiques des performances du système

**Demande**

Vous pouvez obtenir les statistiques des performances du système en utilisant le format de requête suivant. Cela comprend des informations relatives aux cycles de lecture et d’écriture, par exemple, et la quantité de mémoire utilisée.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/resourcemanager/systemperf |
| GET/WebSocket | /api/resourcemanager/systemperf |

Ce format peut être mis à niveau vers une connexion WebSocket.  Il fournit les mêmes données JSON ci-dessous une fois toutes les secondes. 

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse comprend les statistiques relatives aux performances du système, notamment sur l’utilisation du processeur et du GPU, ainsi que sur l’accès à la mémoire et au réseau. Ces informations sont au format JSON et suivent le modèle suivant.
```json
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
## Alimentation
<hr>
### Obtenir l’état actuel de la batterie

**Demande**

Vous pouvez obtenir l’état actuel de la batterie en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/battery |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Les informations d’état actuel de la batterie sont renvoyées à l’aide du format suivant.
```json
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Obtenir le schéma d’alimentation actif

**Demande**

Vous pouvez obtenir le schéma d’alimentation actif en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/activecfg |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Le schéma d’alimentation actif a le format suivant.
```json
{"ActivePowerScheme": string (guid of scheme)}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir la sous-valeur pour un schéma d’alimentation

**Demande**

Vous pouvez obtenir la sous-valeur pour un schéma d’alimentation actif en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/cfg/*<power scheme path>* |

Options :
- SCHEME_CURRENT

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

Liste complète des états d’alimentation disponibles déterminée par application et paramètres de marquage des différents états d’alimentation comme le niveau faible ou critique de la batterie. 

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir l’état d’alimentation du système

**Demande**

Vous pouvez obtenir l’état d’alimentation du système en utilisant le format de requête suivant. Cela vous permet de vérifier s’il se trouve en mode de faible consommation d’énergie.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/state |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Les informations sur l’état d’alimentation suivent le modèle suivant.
```json
{"LowPowerState" : false, "LowPowerStateAvailable" : true }
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
### Définir le schéma d’alimentation actif

**Demande**

Vous pouvez définir le schéma d’alimentation actif en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/power/activecfg |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| scheme | (**requis**) GUID du schéma que vous voulez définir en tant que schéma d’alimentation actif pour le système. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir la sous-valeur pour un schéma d’alimentation

**Demande**

Vous pouvez obtenir la sous-valeur pour un schéma d’alimentation en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/power/cfg/*<power scheme path>* |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| valueAC | (**requis**) Valeur à utiliser pour l’alimentation secteur. |
| valueDC | (**requis**) Valeur à utiliser pour l’alimentation de la batterie. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir un rapport d’étude sur la suspension d’activité

**Demande**

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/sleepstudy/report |

Vous pouvez obtenir un rapport d’étude sur la suspension d’activité en utilisant le format de requête suivant.

**Paramètres d’URI**
| Paramètre d’URI | Description |
| :------          | :------ |
| FileName | (**requis**) Nom complet du fichier que vous voulez télécharger. Cette valeur doit être codée en hex64. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse est un fichier contenant l’étude de veille. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Énumérer les rapports d’étude sur la suspension d’activité disponibles

**Demande**

Vous pouvez obtenir les rapports d’étude sur la suspension d’activité disponibles en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/sleepstudy/reports |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La liste des rapports disponibles suit le modèle suivant.

```json
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
### Obtenir la transformation de l’étude sur la suspension d’activité

**Demande**

Vous pouvez obtenir la transformation de l’étude sur la suspension d’activité en utilisant le format de requête suivant. Il s’agit d’une XSLT qui convertit le rapport d’étude sur la suspension d’activité en un format XML pouvant être lu par une personne.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/power/sleepstudy/transform |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse contient la transformation de l’étude de veille.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* IoT

<hr>
## Télécommande
<hr>
### Redémarrer l’ordinateur cible.

**Demande**

Vous pouvez redémarrer l’ordinateur cible en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/control/restart |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Arrêter l’ordinateur cible

**Demande**

Vous pouvez éteindre l’ordinateur cible en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/control/shutdown |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
## Gestionnaire des tâches
<hr>
### Démarrer une application moderne

**Demande**

Vous pouvez démarrer une application moderne en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/taskmanager/app |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| appid   | (**requis**) PRAID de l’application que vous voulez démarrer. Cette valeur doit être codée en hex64. |
| package   | (**requis**)Nom complet du package d’application que vous voulez démarrer. Cette valeur doit être codée en hex64. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Arrêter une application moderne

**Demande**

Vous pouvez arrêter une application moderne en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/taskmanager/app |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| package   | (**requis**) Nom complet du package d’application que vous voulez arrêter. Cette valeur doit être codée en hex64. |
| forcestop   | (**facultatif**) La valeur **yes** indique que le système doit forcer tous les processus à s’arrêter. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Arrêter le processus par PID

**Demande**

Vous pouvez arrêter un processus en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/taskmanager/process |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| pid   | (**requis**) ID unique du processus à arrêter. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
## Mise en réseau
<hr>
### Obtenir la configuration IP actuelle

**Demande**

Vous pouvez obtenir la configuration IP actuelle en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/networking/ipconfig |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La réponse inclut la configuration IP dans le modèle suivant.

```json
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Définir une adresse IP statique (configuration IPV4)

**Demande**

Définit la configuration IPV4 avec statique IP et DNS. Si une adresse IP statique n’est pas spécifiée, puis il active le protocole DHCP. Si une adresse IP statique est spécifiée, le DNS doit être spécifié d’également.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUT | /api/networking/ipv4config |


**Paramètres d’URI**

| Paramètre d’URI | Description |
| :---          | :--- |
| AdapterName | (**requis**) le GUID d’interface réseau. |
| AdresseIP | L’adresse IP statique à définir. |
| SubnetMask | (**requis** si *IPAddress* n’est pas null) le masque de sous-réseau statique. |
| DefaultGateway | (**requis** si *IPAddress* n’est pas null) la passerelle par défaut statique. |
| PrimaryDNS | (**requis** si *IPAddress* n’est pas null) du DNS principal statique à définir. |
| SecondayDNS | (**requis** si *PrimaryDNS* n’est pas null) serveur DNS secondaire statique à définir. |

Pour plus de clarté, pour définir une interface pour DHCP, sérialiser uniquement le `AdapterName` sur le câble :

```json
{
    "AdapterName":"{82F86C1B-2BAE-41E3-B08D-786CA44FEED7}"
}
```

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT
<hr>
### Énumérer les interfaces réseau sans fil

**Demande**

Vous pouvez énumérer les interfaces sans fil disponibles en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wifi/interfaces |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Liste des interfaces sans fil disponibles et leurs détails au format suivant.

```json 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Énumérer les réseaux sans fil

**Demande**

Vous pouvez énumérer la liste des réseaux sans fil disponibles sur l’interface spécifiée en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wifi/networks |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| interface   | (**requis**) GUID de l’interface réseau à utiliser pour rechercher des réseaux sans fil, sans crochets. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Liste des réseaux sans fil détectés sur l’*interface* fournie. Cela comprend les détails pour les réseaux au format suivant.

```json
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Se connecter à un réseau Wi-Fi et se déconnecter

**Demande**

Vous pouvez vous connecter à un réseau Wi-Fi ou vous déconnecter en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/wifi/network |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| interface   | (**requis**) GUID de l’interface réseau à utiliser pour se connecter au réseau. |
| op   | (**requis**) Indique l’action à entreprendre. Les valeurs possibles sont connect ou disconnect.|
| ssid   | (**requis si *op* == connect**) SSID auquel se connecter. |
| Clé   | (**requis si *op* == connect et que le réseau exige une authentification**) Clé partagée. |
| createprofile | (**requis**) Créez un profil pour le réseau sur l’appareil.  Cela obligera l’appareil à se connecter automatiquement au réseau à l’avenir. Cela peut être **yes** ou **no**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Supprimer un profil Wi-Fi

**Demande**

Vous pouvez supprimer un profil associé à un réseau sur une interface spécifique en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /API/WiFi/Profile |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| interface   | (**requis**) GUID de l’interface réseau associée au profil à supprimer. |
| profile   | (**requis**) Nom du profil à supprimer. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
## Rapport d’erreurs Windows
<hr>
### Télécharger un fichier de rapport d’erreurs Windows

**Demande**

Vous pouvez télécharger un fichier associé à un rapport d’erreurs Windows en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wer/report/file |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| user   | (**requis**) Nom d’utilisateur associé au rapport. |
| Type   | (**requis**) Type de rapport. Il peut s’agir du type **queried** ou **archived**. |
| name   | (**requis**) Nom du rapport. Doit être codé en base64. |
| fichier   | (**requis**) Nom du fichier à télécharger à partir du rapport. Doit être codé en base64. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- La réponse contient le fichier demandé. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
### Énumérer les fichiers dans un rapport d’erreurs Windows

**Demande**

Vous pouvez énumérer les fichiers dans un rapport d’erreurs Windows en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wer/report/files |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| user   | (**requis**) Utilisateur associé au rapport. |
| Type   | (**requis**) Type de rapport. Il peut s’agir du type **queried** ou **archived**. |
| name   | (**requis**) Nom du rapport. Doit être codé en base64. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

```json
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
### Répertorier les rapports d’erreurs Windows

**Demande**

Vous pouvez obtenir les rapports d’erreurs Windows en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wer/reports |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Les rapports d’erreur suivants sont présentés au format suivant.

```json
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Bureau Windows
* HoloLens
* IoT

<hr>
## Enregistreur de performance Windows (WPR) 
<hr>
### Démarrer le suivi avec un profil personnalisé

**Demande**

Vous pouvez charger un profil WPR et démarrer le suivi à l’aide de ce profil en utilisant le format de requête suivant.  Une seule trace peut s’exécuter à la fois. Le profil ne restera pas sur l’appareil. 
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/wpr/customtrace |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Corps HTTP à parties multiples conforme contenant le profil WPR personnalisé.

**Réponse**

L’état de session WPR au format suivant.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Démarrer une session de suivi des performances de démarrage

**Demande**

Vous pouvez démarrer une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/wpr/boottrace |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| profile   | (**requis**) Ce paramètre est requis au démarrage. Nom du profil devant démarrer une session de suivi des performances. Les profils possibles sont stockés dans perfprofiles/profiles.json. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Au démarrage, cette API renvoie l’état de session WPR au format suivant.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Arrêter une session de suivi des performances de démarrage

**Demande**

Vous pouvez arrêter une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wpr/boottrace |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

-  Aucun.  **Remarque :** Il s’agit d’une opération longue.  Elle renverra une réponse à la fin de l’écriture de l’ETL sur le disque.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Démarrer une session de suivi des performances

**Demande**

Vous pouvez démarrer une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.  Une seule trace peut s’exécuter à la fois. 
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/wpr/trace |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| profile   | (**requis**) Nom du profil devant démarrer une session de suivi des performances. Les profils possibles sont stockés dans perfprofiles/profiles.json. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Au démarrage, cette API renvoie l’état de session WPR au format suivant.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Arrêter une session de suivi des performances

**Demande**

Vous pouvez arrêter une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wpr/trace |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucun.  **Remarque :** Il s’agit d’une opération longue.  Elle renverra une réponse à la fin de l’écriture de l’ETL sur le disque.  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Récupérer l’état d’une session de suivi

**Demande**

Vous pouvez récupérer l’état de la session WPR actuelle en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wpr/status |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

L’état de la session de suivi WPR au format suivant.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Répertorier les sessions de suivi terminées (ETL)

**Demande**

Vous pouvez obtenir une liste des traces ETL sur l’appareil en utilisant le format de requête suivant. 

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wpr/tracefiles |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

La liste des sessions de suivi terminées est fournie dans le format suivant.

```json
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Télécharger une session de suivi (ETL)

**Demande**

Vous pouvez télécharger un fichier de suivi (suivi de démarrage ou suivi en mode utilisateur) en utilisant le format de requête suivant. 

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/wpr/tracefile |


**Paramètres d’URI**

Vous pouvez spécifier le paramètre supplémentaire suivant dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| filename   | (**requis**) Nom de la trace ETL à télécharger.  Les traces ETL se trouvent dans /api/wpr/tracefiles. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Renvoie le fichier ETL de suivi.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
### Supprimer une session de suivi (ETL)

**Demande**

Vous pouvez supprimer un fichier de suivi (suivi de démarrage ou suivi en mode utilisateur) en utilisant le format de requête suivant. 

| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/wpr/tracefile |


**Paramètres d’URI**

Vous pouvez spécifier le paramètre supplémentaire suivant dans l’URI de requête :

| Paramètre d’URI | Description |
| :------          | :------ |
| filename   | (**requis**) Nom de la trace ETL à supprimer.  Les traces ETL se trouvent dans /api/wpr/tracefiles. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Renvoie le fichier ETL de suivi.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* IoT

<hr>
## Balises DNS-SD 
<hr>
### Afficher les balises

**Demande**

Affichez les balises actuellement appliquées pour l’appareil.  Ces balises sont annoncées par le biais d’enregistrements DNS-SD TXT dans la clé T.  
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/dns-sd/tags |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse** Balises actuellement appliquées au format suivant. 
```json
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 5XX | Erreur de serveur |


**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Supprimer des balises

**Demande**

Supprimez toutes les balises actuellement signalées par DNS-SD.   
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/dns-sd/tags |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**
 - Aucune

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 5XX | Erreur de serveur |


**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

<hr>
### Supprimer une balise

**Demande**

Supprimez une balise actuellement signalée par DNS-SD.   
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/dns-sd/tag |


**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| tagValue | (**requis**) Balise à supprimer. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**
 - Aucune

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |


**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT
 
<hr>
### Ajouter une balise

**Demande**

Ajoutez une balise à l’annonce DNS-SD.   
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/dns-sd/tag |


**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| tagValue | (**requis**) Balise à ajouter. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**
 - Aucune

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 401 | Dépassement de capacité pour l’espace de balise.  Survient lorsque la balise proposée est trop longue pour l’enregistrement de service DNS-SD résultant. |


**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>Explorateur de fichiers de l’application

<hr>
### Obtenir les dossiers connus

**Demande**

Obtenez la liste des dossiers de niveau supérieur accessibles.

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/filesystem/apps/knownfolders |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse** Dossiers disponibles au format suivant. 
```json
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | Requête de déploiement acceptée et traitée |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |


**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* Xbox
* IoT

<hr>
### Obtenir des fichiers

**Demande**

Obtenez la liste des fichiers d’un dossier.

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/filesystem/apps/files |


**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| knownfolderid | (**requis**) Répertoire de niveau supérieur dans lequel vous voulez faire apparaître la liste des fichiers. Utilisez **LocalAppData** pour accéder aux applications avec chargement indépendant. |
| packagefullname | (**requis si *knownfolderid* == LocalAppData**) Nom complet du package de l’application qui vous intéresse. |
| path | (**facultatif**) Sous-répertoire du dossier ou du package spécifié ci-dessus. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse** Dossiers disponibles au format suivant. 
```json
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* Xbox
* IoT

<hr>
### Télécharger un fichier

**Demande**

Obtenir un fichier à partir d’un dossier connu ou d’appLocalData.

| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/filesystem/apps/file |

**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| knownfolderid | (**requis**) Répertoire de niveau supérieur dans lequel vous voulez télécharger les fichiers. Utilisez **LocalAppData** pour accéder aux applications avec chargement indépendant. |
| filename | (**requis**) Nom du fichier en cours de téléchargement. |
| packagefullname | (**requis si *knownfolderid* == LocalAppData**) Nom complet du package qui vous intéresse. |
| path | (**facultatif**) Sous-répertoire du dossier ou du package spécifié ci-dessus. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Fichier demandé, le cas échéant

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | Fichier demandé |
| 404 | Fichier introuvable |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* Xbox
* IoT

<hr>
### Renommer un fichier

**Demande**

Renommez un fichier dans un dossier.

| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/filesystem/apps/rename |


**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| knownfolderid | (**obligatoire**) Répertoire de niveau supérieur dans lequel se trouve le fichier. Utilisez **LocalAppData** pour accéder aux applications avec chargement indépendant. |
| filename | (**obligatoire**) Nom d’origine du fichier renommé. |
| newfilename | (**obligatoire**) Nouveau nom du fichier.|
| packagefullname | (**requis si *knownfolderid* == LocalAppData**) Nom complet du package de l’application qui vous intéresse. |
| path | (**facultatif**) Sous-répertoire du dossier ou du package spécifié ci-dessus. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |. Le fichier est renommé
| 404 | Fichier introuvable |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* Xbox
* IoT

<hr>
### Supprimer un fichier

**Demande**

Supprimez un fichier dans un dossier.

| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/filesystem/apps/file |

**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| knownfolderid | (**requis**) Répertoire de niveau supérieur dans lequel vous voulez supprimer des fichiers. Utilisez **LocalAppData** pour accéder aux applications avec chargement indépendant. |
| filename | (**requis**) Nom du fichier en cours de suppression. |
| packagefullname | (**requis si *knownfolderid* == LocalAppData**) Nom complet du package de l’application qui vous intéresse. |
| path | (**facultatif**) Sous-répertoire du dossier ou du package spécifié ci-dessus. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |. Le fichier est supprimé. |
| 404 | Fichier introuvable |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* Xbox
* IoT

<hr>
### Charger un fichier

**Demande**

Chargez un fichier dans un dossier.  Ce fichier remplace un fichier existant du même nom, mais ne crée pas de dossier. 

| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/filesystem/apps/file |

**Paramètres d’URI**

| Paramètre d’URI | Description |
| :------     | :----- |
| knownfolderid | (**requis**) Répertoire de niveau supérieur dans lequel vous voulez charger les fichiers. Utilisez **LocalAppData** pour accéder aux applications avec chargement indépendant. |
| packagefullname | (**requis si *knownfolderid* == LocalAppData**) Nom complet du package de l’application qui vous intéresse. |
| path | (**facultatif**) Sous-répertoire du dossier ou du package spécifié ci-dessus. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP      | Description |
| :------     | :----- |
| 200 | OK |. Le fichier est chargé |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Mobile
* Bureau Windows
* HoloLens
* Xbox
* IoT
