---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Informations de référence sur les API principales Device Portal
description: Découvrez les API REST principales Windows Device Portal que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.
---

# Informations de référence sur les API principales Device Portal

Dans Windows Device Portal, tout repose sur les API REST que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.

## Déploiement des applications

---
### Installer une application

**Requête**

Vous pouvez installer une application en utilisant le format de requête suivant.

Méthode      | URI de requête
:------     | :-----
POST | /api/appx/packagemanager/package

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
package   | (**requis**) Nom de fichier du package à installer.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Obtenir l’état de l’installation de l’application

**Requête**

Vous pouvez obtenir l’état d’installation d’une application actuellement en cours d’exécution en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/appx/packagemanager/state

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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
DELETE | /api/appx/packagemanager/package


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
GET | /api/appx/packagemanager/packages


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

La réponse comprend une liste des packages installés avec les détails associés.

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
GET | /api/devicemanager/devices


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend une structure JSON qui contient une arborescence de périphériques hiérarchiques.

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/usermode/dumps


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend une liste des vidages sur incident pour chaque application chargée de manière indépendante.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Obtenir les paramètres de collection de vidage sur incident pour une application

**Requête**

Vous pouvez obtenir les paramètres de collection de vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
fileName   | (**requis**) Nom du fichier de vidage à supprimer.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Télécharger le vidage sur incident pour une application chargée de manière indépendante

**Requête**

Vous pouvez télécharger le vidage sur incident d’une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/usermode/crashdump


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.
fileName   | (**requis**) Nom du fichier de vidage à télécharger.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend un fichier de vidage. Vous pouvez utiliser WinDbg ou Visual Studio pour examiner le fichier de vidage.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Désactiver les vidages sur incident pour une application chargée de manière indépendante

**Requête**

Vous pouvez activer les vidages sur incident pour une application chargée de manière indépendante en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
packageFullname   | (**requis**) Nom complet du package pour l’application chargée de manière indépendante.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Obtenir la liste des fichiers de vérification d’erreur

**Requête**

Vous pouvez obtenir la liste des fichiers minidump de vérification d’erreur en utilisant le format de demande suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/kernel/dumplist


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend une liste des noms de fichier de vidage et leur taille.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Télécharger un fichier de vidage de vérification d’erreur

**Requête**

Vous pouvez télécharger un fichier de vidage de vérification d’erreur en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/kernel/dump


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
filename   | (**requis**) Nom du fichier de vidage. Vous pouvez le rechercher à l’aide de l’API pour obtenir la liste de vidage.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend le fichier de vidage. Vous pouvez examiner ce fichier à l’aide de WinDbg.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir les paramètres de contrôle d’incident de la vérification d’erreur

**Requête**

Vous pouvez obtenir la liste des paramètres de contrôle d’incident de la vérification d’erreur en utilisant le format de demande suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend les paramètres de contrôle d’incident. Pour plus d’informations sur CrashControl, voir l’article [CrashControl](https://technet.microsoft.com/library/cc951703.aspx).

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir un vidage du noyau dynamique

**Requête**

Vous pouvez obtenir un vidage du noyau dynamique en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/livekernel


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend le vidage en mode noyau complet. Vous pouvez examiner ce fichier à l’aide de WinDbg.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir un vidage à partir d’un processus utilisateur dynamique

**Requête**

Vous pouvez obtenir le vidage pour le processus utilisateur dynamique en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/debug/dump/usermode/live


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
pid   | (**requis**) Id unique du processus qui vous intéresse.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend le fichier de vidage du processus. Vous pouvez examiner ce fichier à l’aide de WinDbg ou de Visual Studio.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir les paramètres de contrôle d’incident de la vérification d’erreur

**Requête**

Vous pouvez définir les paramètres de la collecte de données de vérification d’erreur en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
autoreboot   | (**facultatif**) True ou false. Cette valeur indique si le système redémarre automatiquement suite à un échec ou à un verrouillage.
dumptype   | (**facultatif**) Type de vidage. Pour les valeurs prises en charge, voir l’[Énumération CrashDumpType](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**facultatif**) Nombre maximal de vidages à enregistrer.
overwrite   | (**facultatif**) True ou false. Cela indique s’il convient d’écraser ou non les anciens vidages lorsque le seuil du nombre de vidages défini par *maxdumpcount* est atteint.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
## ETW
---
### Créer une session ETW en temps réel via un websocket

**Requête**

Vous pouvez créer une session ETW en temps réel en utilisant le format de requête suivant. Cette opération est gérée via un websocket.
 
Méthode      | URI de requête
:------     | :-----
GET/WebSocket | /api/etw/session/realtime


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend les événements ETW issus des fournisseurs activés.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Énumérer les fournisseurs ETW enregistrés

**Requête**

Vous pouvez énumérer les fournisseurs enregistrés en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/etw/providers


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend la liste des fournisseurs ETW. La liste comprend le nom convivial et le GUID de chaque fournisseur.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
## Mise en réseau
---
### Obtenir la configuration IP actuelle

**Requête**

Vous pouvez obtenir la configuration IP actuelle en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/networking/ipconfig


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend la configuration IP

**Code d’état**

Le tableau suivant présente les codes d’état supplémentaires qui peuvent être renvoyés à la suite de cette opération.

Code d’état HTTP      | Description
:------     | :-----
200 | L’installation est réussie
500 | Une erreur de serveur interne est survenue

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## Informations sur le système d’exploitation
---
### Obtenir le nom de l’ordinateur

**Requête**

Vous pouvez obtenir le nom d’un ordinateur en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/os/machinename


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
GET | /api/os/info


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
POST | /api/os/machinename


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
name | (**requis**) Nouveau nom de l’ordinateur.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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

Vous pouvez obtenir la liste des processus en cours d’exécution en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/resourcemanager/processes


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend une liste des processus et les détails associés. Les informations sont au format JSON.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Obtenir les statistiques des performances du système

**Requête**

Vous pouvez obtenir les statistiques des performances du système en utilisant le format de requête suivant. Cela comprend des informations relatives aux cycles de lecture et d’écriture, par exemple, et la quantité de mémoire utilisée.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/resourcemanager/systemperf


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- La réponse comprend les statistiques relatives aux performances du système, notamment sur l’utilisation du processeur et du GPU, ainsi que sur l’accès à la mémoire et au réseau. Ces informations sont au format JSON.

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/battery


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Obtenir le schéma d’alimentation actif

**Requête**

Vous pouvez obtenir le schéma d’alimentation actif en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/activecfg


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir la sous-valeur pour un schéma d’alimentation

**Requête**

Vous pouvez obtenir la sous-valeur pour un schéma d’alimentation actif en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir l’état d’alimentation du système

**Requête**

Vous pouvez obtenir l’état d’alimentation du système en utilisant le format de requête suivant. Cela vous permet de vérifier s’il se trouve en mode de faible consommation d’énergie.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/state


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Obtenir un rapport d’étude sur la suspension d’activité

**Requête**

Vous pouvez obtenir un rapport d’étude sur la suspension d’activité en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/sleepstudy/reports


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
FileName | (**requis**) Nom de fichier du rapport d’étude sur la suspension d’activité que vous souhaitez télécharger.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Définir le schéma d’alimentation actif

**Requête**

Vous pouvez définir le schéma d’alimentation actif en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/power/activecfg


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
scheme | (**requis**) GUID du schéma que vous voulez définir en tant que schéma d’alimentation actif pour le système.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir la sous-valeur pour un schéma d’alimentation

**Requête**

Vous pouvez obtenir la sous-valeur pour un schéma d’alimentation en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
valueAC | (**requis**) Valeur à utiliser pour l’alimentation secteur.
valueDC | (**requis**) Valeur à utiliser pour l’alimentation de la batterie.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Énumérer les rapports d’étude sur la suspension d’activité disponibles

**Requête**

Vous pouvez obtenir les rapports d’étude sur la suspension d’activité disponibles en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/sleepstudy/reports


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
### Obtenir la transformation de l’étude sur la suspension d’activité

**Requête**

Vous pouvez obtenir la transformation de l’étude sur la suspension d’activité en utilisant le format de requête suivant. Il s’agit d’une XSLT qui convertit le rapport d’étude sur la suspension d’activité en un format XML pouvant être lu par une personne.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/power/sleepstudy/reports


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* IoT

---
## Contrôle à distance
---
### Redémarrer l’ordinateur cible.

**Requête**

Vous pouvez redémarrer l’ordinateur cible en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/control/restart


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
POST | /api/control/shutdown


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
POST | /api/taskmanager/app


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
appid   | (**requis**) PRAID de l’application que vous voulez démarrer. Cette valeur doit être codée en hex64.
package   | (**requis**)Nom complet du package d’application que vous voulez démarrer. Cette valeur doit être codée en hex64.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
package   | (**requis**) Nom complet du package d’application que vous voulez arrêter. Cette valeur doit être codée en hex64.
forcestop   | (**facultatif**) La valeur **yes** indique que le système doit forcer tous les processus à s’arrêter.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

---
## WiFi
---
### Énumérer les interfaces réseau sans fil

**Requête**

Vous pouvez énumérer les interfaces sans fil disponibles en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wifi/interfaces


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Liste des interfaces sans fil disponibles et leurs détails. Les détails comprennent les éléments tels que, le GUID, la description, le nom convivial et plus encore.

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wifi/networks


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
interface   | (**requis**) GUID de l’interface réseau à utiliser pour rechercher des réseaux sans fil.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Liste des réseaux sans fil détectés sur l’*interface* fournie. Cela comprend les détails pour les réseaux.

**Code d’état**

- Codes d’état standard.

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
 
Méthode      | URI de requête
:------     | :-----
POST | /api/wifi/network


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
interface   | (**requis**) GUID de l’interface réseau à utiliser pour se connecter au réseau.
op   | (**requis**) Indique l’action à entreprendre. Les valeurs possibles sont connect ou disconnect.
ssid   | (**requis si *op* == connect**) SSID auquel se connecter.
key   | (**requis si *op* == connect**) Clé partagée.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
interface   | (**requis**) GUID de l’interface réseau associée au profil à supprimer.
profile   | (**requis**) Nom du profil à supprimer.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

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

Vous pouvez télécharger un fichier de rapport d’erreurs Windows en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wer/reports/file


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
user   | (**requis**) Nom d’utilisateur associé au rapport.
type   | (**requis**) Type de rapport. Il peut s’agir du type **queried** ou **archived**.
name   | (**requis**) Nom du rapport.
file   | (**requis**) Nom du fichier à télécharger à partir du rapport.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Énumérer les fichiers dans un rapport d’erreurs Windows

**Requête**

Vous pouvez énumérer les fichiers dans un rapport d’erreurs Windows en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wer/reports/files


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
user   | (**requis**) Utilisateur associé au rapport.
type   | (**requis**) Type de rapport. Il peut s’agir du type **queried** ou **archived**.
name   | (**requis**) Nom du rapport.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
### Répertorier les rapports d’erreurs Windows

**Requête**

Vous pouvez obtenir les rapports d’erreurs Windows en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wer/reports


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Aucun

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Desktop
* HoloLens
* IoT

---
## Enregistreur de performance Windows (WPR) 
---
### Démarrer le suivi avec un profil personnalisé

**Requête**

Vous pouvez charger un profil WPR et démarrer le suivi à l’aide de ce profil en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/wpr/customtrace


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Corps HTTP à parties multiples conforme contenant le profil WPR personnalisé.

**Réponse**

- Renvoie l’état de la session WPR.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Démarrer une session de suivi des performances de démarrage

**Requête**

Vous pouvez démarrer une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/wpr/boottrace


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
profile   | (**requis**) Ce paramètre est requis au démarrage. Nom du profil devant démarrer une session de suivi des performances. Les profils possibles sont stockés dans perfprofiles/profiles.json.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Au démarrage, cette API renvoie l’état de la session WPR.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Arrêter une session de suivi des performances de démarrage

**Requête**

Vous pouvez arrêter une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wpr/boottrace


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Renvoie le fichier ETL de suivi.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Démarrer une session de suivi des performances

**Requête**

Vous pouvez démarrer une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de requête
:------     | :-----
POST | /api/wpr/trace


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI | Description
:---          | :---
profile   | (**requis**) Nom du profil devant démarrer une session de suivi des performances. Les profils possibles sont stockés dans perfprofiles/profiles.json.

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Au démarrage, cette API renvoie l’état de la session WPR.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Arrêter une session de suivi des performances

**Requête**

Vous pouvez arrêter une session de suivi WPR en utilisant le format de requête suivant. Également connue sous le nom de session de suivi des performances.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wpr/trace


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- Renvoie le fichier ETL de suivi.

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

---
### Récupérer l’état d’une session de suivi

**Requête**

Vous pouvez récupérer l’état de la session WPR actuelle en utilisant le format de requête suivant.
 
Méthode      | URI de requête
:------     | :-----
GET | /api/wpr/status


**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

- État de la session de suivi WPR

**Code d’état**

- Codes d’état standard.

**Familles d’appareils disponibles**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT


<!--HONumber=Mar16_HO5-->


