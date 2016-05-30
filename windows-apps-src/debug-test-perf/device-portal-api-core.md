---
author: dbirtolo
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Informations de référence sur les API principales Device Portal
description: Découvrez les API REST principales Windows Device Portal que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.
---

# Référence sur les API principales Device Portal

Dans Windows Device Portal, tout repose sur les API REST que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.

## Déploiement des applications

---
### Installer une application

**Requête**

Vous pouvez installer une application en utilisant le format de requête suivant.

Méthode      | URI de la requête
:------     | :-----
POST | /api/app/packagemanager/package
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
package   | (**requis**) Nom de fichier du package à installer.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Requête de déploiement acceptée et traitée
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Obtenir l’état de l’installation de l’application

**Requête**

Vous pouvez obtenir l’état d’installation d’une application actuellement en cours d’exécution en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/app/packagemanager/state
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Le résultat du dernier déploiement
204 | L’installation est en cours d’exécution
404 | Aucune action d’installation n’a été détectée
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Désinstaller une application

**Requête**

Vous pouvez désinstaller une application en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
DELETE | /api/app/packagemanager/package
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Obtenir des applications installées

**Requête**

Vous pouvez obtenir une liste des applications installées sur le système en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/app/packagemanager/packages
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend une liste des packages installés avec les détails associés. Le modèle de cette réponse est le suivant.
```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Gestionnaire de périphériques
---
### Obtenir les périphériques installés sur l’ordinateur

**Requête**

Vous pouvez obtenir une liste des périphériques installés sur l’ordinateur en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/devicemanager/devices
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse inclut un tableau d’appareils JSON joint à l’appareil.
``` 
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* IoT

---
## Collection de vidages
---
### Obtenir la liste de tous les vidages sur incident pour les applications

**Requête**

Vous pouvez obtenir la liste de tous les vidages sur incident disponibles pour toutes les applications chargées de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/usermode/dumps
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend une liste des vidages sur incident pour chaque application chargée de manière indépendante.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Obtenir les paramètres de collection de vidage sur incident pour une application

**Requête**

Vous pouvez obtenir les paramètres de collection de vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse a le format suivant.
```
{"CrashDumpEnabled": bool}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Supprimer un vidage sur incident pour une application chargée de manière indépendante

**Requête**

Vous pouvez supprimer le vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
fileName   | (**requis**) Nom du fichier de vidage à supprimer.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Désactiver les vidages sur incident pour une application chargée de manière indépendante

**Requête**

Vous pouvez désactiver les vidages sur incident pour une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol

<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Télécharger le vidage sur incident pour une application chargée de manière indépendante

**Requête**

Vous pouvez télécharger le vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/usermode/crashdump
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
fileName   | (**requis**) Nom du fichier de vidage à télécharger.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend un fichier de vidage. Vous pouvez utiliser WinDbg ou Visual Studio pour examiner le fichier de vidage.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Désactiver les vidages sur incident pour une application chargée de manière indépendante

**Requête**

Vous pouvez activer les vidages sur incident pour une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Obtenir la liste des fichiers de vérification d’erreur

**Requête**

Vous pouvez obtenir la liste des fichiers minidump de vérification d’erreur en utilisant le format de demande suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/kernel/dumplist
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend une liste des noms de fichier de vidage et leur taille. Cette liste doit avoir le format suivant. Le second paramètre *FileName* correspond à la taille du fichier. Il s’agit d’un bogue connu.
```
{"DumpFiles": [
    {
        "FileName": string,
        "FileName": string
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Télécharger un fichier de vidage de vérification d’erreur

**Requête**

Vous pouvez télécharger un fichier de vidage de vérification d’erreur en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/kernel/dump
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
filename   | (**requis**) Nom du fichier de vidage. Vous pouvez le rechercher à l’aide de l’API pour obtenir la liste de vidage.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend le fichier de vidage. Vous pouvez examiner ce fichier à l’aide de WinDbg.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir les paramètres de contrôle d’incident de la vérification d’erreur

**Requête**

Vous pouvez obtenir la liste des paramètres de contrôle d’incident de la vérification d’erreur en utilisant le format de demande suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol

<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend les paramètres de contrôle d’incident. Pour plus d’informations sur CrashControl, voir l’article [CrashControl](https://technet.microsoft.com/library/cc951703.aspx). Le modèle de la réponse est le suivant.
```
{
    "autoreboot": int,
    "dumptype": int,
    "maxdumpcount": int,
    "overwrite": int
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir un vidage du noyau dynamique

**Requête**

Vous pouvez obtenir un vidage du noyau dynamique en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/livekernel
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend le vidage en mode noyau complet. Vous pouvez examiner ce fichier à l’aide de WinDbg.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir un vidage à partir d’un processus utilisateur dynamique

**Requête**

Vous pouvez obtenir le vidage pour le processus utilisateur dynamique en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/debug/dump/usermode/live
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
pid   | (**requis**) Id unique du processus qui vous intéresse.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend le fichier de vidage du processus. Vous pouvez examiner ce fichier à l’aide de WinDbg ou de Visual Studio.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir les paramètres de contrôle d’incident de la vérification d’erreur

**Requête**

Vous pouvez définir les paramètres de la collecte de données de vérification d’erreur en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
autoreboot   | (**facultatif**) True ou false. Cette valeur indique si le système redémarre automatiquement suite à un échec ou à un verrouillage.
dumptype   | (**facultatif**) Type de vidage. Pour les valeurs prises en charge, consultez [Énumération CrashDumpType](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**facultatif**) Le nombre maximal de vidages à enregistrer.
overwrite   | (**facultatif**) True ou false. Cela indique s’il convient d’écraser ou non les anciens vidages lorsque le seuil du nombre de vidages défini par *maxdumpcount* est atteint.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
## ETW
---
### Créer une session ETW en temps réel via un websocket

**Requête**

Vous pouvez créer une session ETW en temps réel en utilisant le format de requête suivant. Cette opération est gérée via un websocket.  Les événements ETW sont regroupés sur le serveur et envoyés vers le client une fois par seconde. 
 
Méthode      | URI de requête
:------     | :-----
GET/WebSocket | /api/etw/session/realtime
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend les événements ETW issus des fournisseurs activés.  Voir les commandes WebSocket ETW ci-dessous. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

### Commandes WebSocket ETW
Ces commandes sont envoyées du client vers le serveur.

Commande | Description
:----- | :-----
provider *{guid}* enable *{level}* | Activez le fournisseur marqué par *{guid}* (sans crochets) au niveau spécifié. *{level}* est un entier **int** de 1 (peu détaillé) à 5 (très détaillé).
provider *{guid}* disable | Désactivez le fournisseur marqué par *{guid}* (sans crochets).

Cette réponse est envoyée du serveur vers le client. Elle est envoyée sous forme de texte et vous obtenez le format suivant en analysant le JSON.
```
{
    "Events":[
        {
            "Timestamp": int,
            "Provider": string,
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
```
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

---
### Énumérer les fournisseurs ETW enregistrés

**Requête**

Vous pouvez énumérer les fournisseurs enregistrés en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/etw/providers
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend la liste des fournisseurs ETW. La liste comprend le nom convivial et le GUID de chaque fournisseur au format suivant.
```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Énumérez les fournisseurs ETW personnalisés exposés par la plate-forme.

**Requête**

Vous pouvez énumérer les fournisseurs enregistrés en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/etw/customproviders
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

200 OK. La réponse comprend la liste des fournisseurs ETW. La liste comprend le nom convivial et le GUID de chaque fournisseur.

```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Code d’état**

- Codes d’état standard.
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
## Informations sur le système d’exploitation
---
### Obtenir le nom de l’ordinateur

**Requête**

Vous pouvez obtenir le nom d’un ordinateur en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/os/machinename
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse inclut le nom de l’ordinateur au format suivant. 

```
{"ComputerName": string}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Obtenir les informations du système d’exploitation

**Requête**

Vous pouvez obtenir les informations du système d’exploitation pour un ordinateur en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/os/info
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse inclut des informations sur le système d’exploitation au format suivant.

```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Obtenir le nom de l’ordinateur

**Requête**

Vous pouvez définir le nom d’un ordinateur en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/os/machinename
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
name | (**requis**) Nouveau nom de l’ordinateur.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Données relatives aux performances
---
### Obtenir la liste des processus en cours d’exécution

**Requête**

Vous pouvez obtenir la liste des processus en cours d’exécution en utilisant le format de requête suivant.  Il peut également être mis à niveau vers une connexion WebSocket, avec les mêmes données JSON transmises au client une fois par seconde. 
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/resourcemanager/processes
GET/WebSocket | /api/resourcemanager/processes
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend une liste des processus et les détails associés. Les informations sont au format JSON et suivent le modèle suivant.
```
{"Processes": [
    {
        "CPUUsage": int,
        "ImageName": string,
        "PageFileUsage": int,
        "PrivateWorkingSet": int,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": int,
        "WorkingSetSize": int
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Obtenir les statistiques des performances du système

**Requête**

Vous pouvez obtenir les statistiques des performances du système en utilisant le format de requête suivant. Cela comprend des informations relatives aux cycles de lecture et d’écriture, par exemple, et la quantité de mémoire utilisée.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/resourcemanager/systemperf
GET/WebSocket | /api/resourcemanager/systemperf
<br />
Ce format peut être mis à niveau vers une connexion WebSocket.  Il fournit les mêmes données JSON ci-dessous une fois toutes les secondes. 

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend les statistiques relatives aux performances du système, notamment sur l’utilisation du processeur et du GPU, ainsi que sur l’accès à la mémoire et au réseau. Ces informations sont au format JSON et suivent le modèle suivant.
```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Alimentation
---
### Obtenir l’état actuel de la batterie

**Requête**

Vous pouvez obtenir l’état actuel de la batterie en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/power/battery
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Les informations d’état actuel de la batterie sont renvoyées à l’aide du format suivant.
```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT
* Mobile

---
### Obtenir le schéma d’alimentation actif

**Requête**

Vous pouvez obtenir le schéma d’alimentation actif en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/power/activecfg
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Le schéma d’alimentation actif a le format suivant.
```
{"ActivePowerScheme": string (guid of scheme)}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir la sous-valeur pour un schéma d’alimentation

**Requête**

Vous pouvez obtenir la sous-valeur pour un schéma d’alimentation actif en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*
<br />
Options :
- SCHEME_CURRENT

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

Liste complète des états d’alimentation disponibles déterminée par application et paramètres de marquage des différents états d’alimentation comme le niveau faible ou critique de la batterie. 

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir l’état d’alimentation du système

**Requête**

Vous pouvez obtenir l’état d’alimentation du système en utilisant le format de requête suivant. Cela vous permet de vérifier s’il se trouve en mode de faible consommation d’énergie.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/power/state
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Les informations sur l’état d’alimentation suivent le modèle suivant.
```
{"LowPowerStateAvailable": bool}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Définir le schéma d’alimentation actif

**Requête**

Vous pouvez définir le schéma d’alimentation actif en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/power/activecfg
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
scheme | (**requis**) GUID du schéma que vous voulez définir en tant que schéma d’alimentation actif pour le système.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir la sous-valeur pour un schéma d’alimentation

**Requête**

Vous pouvez obtenir la sous-valeur pour un schéma d’alimentation en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
valueAC | (**requis**) Valeur à utiliser pour l’alimentation secteur.
valueDC | (**requis**) Valeur à utiliser pour l’alimentation de la batterie.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir un rapport d’étude sur la suspension d’activité

**Requête**

Méthode      | URI de la requête
:------     | :-----
GET | /api/power/sleepstudy/report
<br />
Vous pouvez obtenir un rapport d’étude sur la suspension d’activité en utilisant le format de requête suivant.

**Paramètres d’URI**
Paramètre d’URI | Description
:---          | :---
FileName | (**requis**) Nom complet du fichier que vous voulez télécharger. Cette valeur doit être codée en hex64.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse est un fichier contenant l’étude de veille. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Énumérer les rapports d’étude sur la suspension d’activité disponibles

**Requête**

Vous pouvez obtenir les rapports d’étude sur la suspension d’activité disponibles en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/power/sleepstudy/reports
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La liste des rapports disponibles suit le modèle suivant.

```
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir la transformation de l’étude sur la suspension d’activité

**Requête**

Vous pouvez obtenir la transformation de l’étude sur la suspension d’activité en utilisant le format de requête suivant. Il s’agit d’une XSLT qui convertit le rapport d’étude sur la suspension d’activité en un format XML pouvant être lu par une personne.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/power/sleepstudy/transform
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse contient la transformation de l’étude de veille.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
## Contrôle à distance
---
### Redémarrer l’ordinateur cible.

**Requête**

Vous pouvez redémarrer l’ordinateur cible en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/control/restart
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Arrêter l’ordinateur cible

**Requête**

Vous pouvez éteindre l’ordinateur cible en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/control/shutdown
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Gestionnaire des tâches
---
### Démarrer une application moderne

**Requête**

Vous pouvez démarrer une application moderne en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/taskmanager/app
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
appid   | (**requis**) PRAID de l’application que vous voulez démarrer. Cette valeur doit être codée en hex64.
package   | (**requis**)Nom complet du package d’application que vous voulez démarrer. Cette valeur doit être codée en hex64.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Arrêter une application moderne

**Requête**

Vous pouvez arrêter une application moderne en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
DELETE | /api/taskmanager/app
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
package   | (**requis**) Nom complet du package d’application que vous voulez arrêter. Cette valeur doit être codée en hex64.
forcestop   | (**facultatif**) La valeur **yes** indique que le système doit forcer tous les processus à s’arrêter.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Mise en réseau
---
### Obtenir la configuration IP actuelle

**Requête**

Vous pouvez obtenir la configuration IP actuelle en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/networking/ipconfig
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse inclut la configuration IP dans le modèle suivant.

```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

--
### Énumérer les interfaces réseau sans fil

**Requête**

Vous pouvez énumérer les interfaces sans fil disponibles en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wifi/interfaces
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Liste des interfaces sans fil disponibles et leurs détails au format suivant.

``` 
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Énumérer les réseaux sans fil

**Requête**

Vous pouvez énumérer la liste des réseaux sans fil disponibles sur l’interface spécifiée en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wifi/networks
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
interface   | (**requis**) GUID de l’interface réseau à utiliser pour rechercher des réseaux sans fil, sans crochets. 
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Liste des réseaux sans fil détectés sur l’*interface* fournie. Cela comprend les détails pour les réseaux au format suivant.

```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Se connecter à un réseau Wi-Fi et se déconnecter

**Requête**

Vous pouvez vous connecter à un réseau Wi-Fi ou vous déconnecter en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/wifi/network
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
interface   | (**requis**) GUID de l’interface réseau à utiliser pour se connecter au réseau.
op   | (**requis**) Indique l’action à entreprendre. Les valeurs possibles sont connect ou disconnect.
ssid   | (**requis si*op* == connecter**) Le SSID auquel se connecter.
key   | (**requis si*op* == connecter et le réseau exige une authentification**) La clé partagée.
createprofile | (**requis**) Créez un profil pour le réseau sur l’appareil.  Cela obligera l’appareil à se connecter automatiquement au réseau à l’avenir. Cela peut être **yes** ou **no**. 

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
### Supprimer un profil Wi-Fi

**Requête**

Vous pouvez supprimer un profil associé à un réseau sur une interface spécifique en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
DELETE | /api/wifi/network
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
interface   | (**requis**) GUID de l’interface réseau associée au profil à supprimer.
profile   | (**requis**) Nom du profil à supprimer.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Rapport d’erreurs Windows
---
### Télécharger un fichier de rapport d’erreurs Windows

**Requête**

Vous pouvez télécharger un fichier associé à un rapport d’erreurs Windows en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wer/report/file
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
user   | (**requis**) Nom d’utilisateur associé au rapport.
type   | (**requis**) Type de rapport. Il peut s’agir du type **queried** ou **archived**.
name   | (**requis**) Nom du rapport. Doit être codé en base64. 
file   | (**requis**) Nom du fichier à télécharger à partir du rapport. Doit être codé en base64. 
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse contient le fichier demandé. 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Énumérer les fichiers dans un rapport d’erreurs Windows

**Requête**

Vous pouvez énumérer les fichiers dans un rapport d’erreurs Windows en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wer/report/files
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
user   | (**requis**) Utilisateur associé au rapport.
type   | (**requis**) Type de rapport. Il peut s’agir du type **queried** ou **archived**.
name   | (**requis**) Nom du rapport. Doit être codé en base64. 
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Répertorier les rapports d’erreurs Windows

**Requête**

Vous pouvez obtenir les rapports d’erreurs Windows en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wer/reports
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Les rapports d’erreur suivants sont présentés au format suivant.

```
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

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
## Enregistreur de performance Windows (WPR) 
---
### Démarrer le suivi avec un profil personnalisé

**Requête**

Vous pouvez charger un profil WPR et démarrer le suivi à l’aide de ce profil en utilisant le format de requête suivant.  Une seule trace peut s’exécuter à la fois. Le profil ne restera pas sur l’appareil. 
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/wpr/customtrace
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Corps HTTP à parties multiples conforme contenant le profil WPR personnalisé.

**Réponse**

L’état de session WPR au format suivant.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Démarrer une session de suivi des performances de démarrage

**Requête**

Vous pouvez démarrer une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/wpr/boottrace
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
profile   | (**requis**) Ce paramètre est requis au démarrage. Nom du profil devant démarrer une session de suivi des performances. Les profils possibles sont stockés dans perfprofiles/profiles.json.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Au démarrage, cette API renvoie l’état de session WPR au format suivant.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Arrêter une session de suivi des performances de démarrage

**Requête**

Vous pouvez arrêter une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wpr/boottrace
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Renvoie le fichier ETL de suivi.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Démarrer une session de suivi des performances

**Requête**

Vous pouvez démarrer une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.  Une seule trace peut s’exécuter à la fois. 
 
Méthode      | URI de la requête
:------     | :-----
POST | /api/wpr/trace
<br />

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
profile   | (**requis**) Nom du profil devant démarrer une session de suivi des performances. Les profils possibles sont stockés dans perfprofiles/profiles.json.
<br />
**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

Au démarrage, cette API renvoie l’état de session WPR au format suivant.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Arrêter une session de suivi des performances

**Requête**

Vous pouvez arrêter une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wpr/trace
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Renvoie le fichier ETL de suivi.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Récupérer l’état d’une session de suivi

**Requête**

Vous pouvez récupérer l’état de la session WPR actuelle en utilisant le format de requête suivant.
 
Méthode      | URI de la requête
:------     | :-----
GET | /api/wpr/status
<br />

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

L’état de la session de suivi WPR au format suivant.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | OK
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT


<!--HONumber=May16_HO2-->


