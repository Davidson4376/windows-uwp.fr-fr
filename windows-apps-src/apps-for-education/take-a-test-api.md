---
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: API JavaScript d’Examen.
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: Windows 10, uwp, éducation
ms.localizationpriority: medium
ms.openlocfilehash: d64901c08e2945f34e66055d8e2e7d3a8301f66e
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5639648"
---
# <a name="take-a-test-javascript-api"></a>API JavaScript d’Examen

[Examen](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) est une application UWP basée sur navigateur qui génère des évaluations en ligne verrouillé pour les tests stratégiques, ce qui permet aux enseignants de se concentrer sur l’évaluation de contenu au lieu de la façon de fournir un environnement de test sécurisé. Pour y parvenir, elle utilise une API JavaScript que n’importe quelle application web peut utiliser. L’API d’Examen prend en charge la [norme d’API de navigateur SBAC](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) pour des examens courants dont les enjeux sont importants.

Consultez [Informations techniques de référence sur l’application Examen](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) pour plus d’informations sur l’application. Pour obtenir de l’aide pour la résolution des problèmes, consultez [Résoudre les problèmes de Microsoft Examen avec l’observateur d’événements](troubleshooting.md).

## <a name="reference-documentation"></a>Documentation de référence
Les API Examen existent dans les espaces de noms suivants. Notez que toutes les API dépendent d’un objet `SecureBrowser` global.

| Espace de noms | Description |
|-----------|-------------|
|[espace de noms de sécurité](#security-namespace)|Contient les API qui permettent de verrouiller le périphérique pour le test et de mettre en œuvre un environnement de test. |

### <a name="security-namespace"></a>Espace de noms de sécurité

L’espace de noms de sécurité vous permet de verrouiller l’appareil, vérifiez la liste des processus utilisateur et système, obtenir des adresses IP et MAC, et effacer des ressources web mises en cache.

| Méthode | Description   |
|--------|---------------|
|[lockDown](#lockDown) | Verrouille l’appareil pour le test. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Détermine si le contexte de verrouillage est toujours appliqué à l’appareil. |
|[getDeviceInfo](#getDeviceInfo) | Obtient des informations sur la plateforme sur laquelle l’application de test est en cours d’exécution. |
|[examineProcessList](#examineProcessList)|Obtient la liste des processus utilisateur et système en cours d’exécution.|
|[close](#close) | Ferme le navigateur et déverrouille l’appareil. |
|[getPermissiveMode](#getPermissiveMode)|Vérifie si le mode permissif est activé ou désactivé.|
|[setPermissiveMode](#setPermissiveMode)|Active/désactive le mode permissif.|
|[emptyClipBoard](#emptyClipBoard)|Efface le contenu du presse-papiers du système.|
|[getMACAddress](#getMACAddress)|Obtient la liste des adresses MAC pour l’appareil.|
|[getStartTime](#getStartTime) | Obtient l’heure à laquelle l’application de test a été démarrée. |
|[getCapability](#getCapability) | Demande si une fonctionnalité est activée ou désactivée. |
|[setCapability](#setCapability)|Active ou désactive la fonctionnalité spécifiée.| 
|[isRemoteSession](#isRemoteSession) | Vérifie si la session active est journalisée à distance. |
|[isVMSession](#isVMSession) | Vérifie si la session active est en cours d’exécution sur un ordinateur virtuel. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
Verrouille l’appareil. Également utilisé pour déverrouiller l’appareil. L’application web de test appelle cet appel avant d’autoriser les étudiants à démarrer le test. Le responsable de l’implémentation doit prendre les mesures nécessaires pour sécuriser l’environnement de test. Les étapes nécessaires pour sécuriser l’environnement sont spécifiques des périphériques et, par exemple, incluent des aspects tels que la désactivation des captures d’écran, la désactivation de la conversation en mode sécurisé, l’annulation du Presse-papiers du système, la saisie en mode plein écran, la désactivation des espaces dans des appareils OSX10.7+, etc. L’application de test active le verrouillage avant le début d’une évaluation et désactive le verrouillage lorsque l’étudiant a terminé l’évaluation et se trouve hors du test sécurisé.

**Syntaxe**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Paramètres**  
* `enable` - **true** pour exécuter l’application Examen au-dessus de l’écran de verrouillage et appliquer les stratégies décrites dans ce [document](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** arrête l’exécution de l’application Examen au-dessus de l’écran de verrouillage et la ferme, sauf si l’application n’est pas verrouillée, auquel cas il est sans effet.  
* `onSuccess` - [facultatif] Fonction à appeler après l’action ou la désactivation du verrouillage. Elle doit être au format `Function(Boolean currentlockdownstate)`.  
* `onError` - [facultatif] Fonction à appeler si l’opération lockDown a échoué. Elle doit être au format `Function(Boolean currentlockdownstate)`.  

**Configuration requise**  
Windows10, version1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Détermine si le contexte de verrouillage est toujours appliqué à l’appareil. L’application web de test l’appelle avant d’autoriser les étudiants à démarrer le test et régulièrement une fois dans le test.

**Syntaxe**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Paramètres**  
* `callback` Fonction à appeler lorsque cette fonction a terminé. Elle doit être au format `Function(String state)` où `state` est une chaîne JSON contenant deux champs. Le premier est le champ `secure`, qui affiche `true` uniquement si tous les verrous nécessaires ont été activés (ou les fonctionnalités désactivées) pour activer un environnement de test sécurisé et qu’aucun de ces éléments n’a été compromis depuis que l’application est entré en mode verrouillage. L’autre champ, `messageKey`, inclut d’autres détails ou informations spécifiques du fournisseur. Le but ici est de permettre aux fournisseurs de placer des informations supplémentaires qui augmentent le drapeau `secure` booléen:

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Configuration requise**  
Windows10, version1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
Obtient des informations sur la plateforme sur laquelle l’application de test est en cours d’exécution. Cette fonction est utilisée pour augmenter toutes les informations qui étaient visibles pour l’agent utilisateur.

**Syntaxe**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Paramètres**  
* `callback` Fonction à appeler lorsque cette fonction a terminé. Elle doit être au format `Function(String infoObj)` où `infoObj` est une chaîne JSON contenant plusieurs champs. Les champs suivants doivent être pris en charge:
    * `os` représente le type de système d’exploitation (par exemple: Windows, Mac OS, Linux, iOS, Android, etc.)
    * `name` représente le nom de la version du système d’exploitation, le cas échéant (par exemple: Sierra, Ubuntu).
    * `version` représente la version du système d’exploitation (par exemple: 10.1, 10Pro, etc.)
    * `brand` représente la personnalisation du navigateur sécurisé (par exemple: OAKS, autorité de confiance, SmarterApp, etc.)
    * `model` représente le modèle d’appareil pour les appareils mobiles uniquement; nul/non utilisé pour les navigateurs de bureau.

**Configuration requise**  
Windows10, version1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Obtient la liste de tous les processus en cours d’exécution sur l’ordinateur client appartenant à l’utilisateur. L’application de test l’appelle pour examiner la liste et la compare avec une liste des processus qui ont été considérés comme placés sur liste rouge au cours du cycle de test. Cet appel doit être appelé à la fois au début d’une évaluation et régulièrement pendant que l’étudiant réalise l’évaluation. Si un processus placé sur liste rouge est détecté, l’évaluation doit être arrêtée pour préserver l’intégrité du test.

**Syntaxe**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Paramètres**  
* `blacklistedProcessList` - Liste des processus que l’application de test a placés sur liste rouge.  
`callback` - Fonction à appeler une fois que les processus actifs ont été trouvés. Elle doit être au format: `Function(String foundBlacklistedProcesses)` où `foundBlacklistedProcesses` est au format:`"['process1.exe','process2.exe','processEtc.exe']"`. Elle sera vide si aucun processus placé sur liste rouge n’a été trouvé. Si elle est nulle, cela indique qu’une erreur s’est produite dans l’appel de fonction d’origine.

**Remarque** La liste n’inclut pas les processus système.

**Configuration requise**  
Windows10, version1709

---

<span id="close"/>

### <a name="close"></a>close
Ferme le navigateur et déverrouille l’appareil. L’application de test doit l’appeler lorsque l’utilisateur choisit de quitter le navigateur.

**Syntaxe**  
`void SecureBrowser.security.close(restart);`

**Paramètres**  
* `restart` -Ce paramètre est ignoré, mais doit être fourni.

**Remarque** Dans Windows10, version1607, l’appareil doit être verrouillé initialement. Dans les versions ultérieures, cette méthode ferme le navigateur, que l’appareil soit verrouillé ou non.

**Configuration requise**  
Windows10, version1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
L’application web de test doit appeler cette fonction pour déterminer si le mode permissif est activé ou désactivé. En mode permissif, un navigateur doit alléger certains de ses crochets de sécurité restrictifs pour permettre à une technologie d’assistance de fonctionner avec le navigateur sécurisé. Par exemple, les navigateurs qui empêchent de manière agressive les interfaces utilisateur d’autres applications à se présenter par-dessus eux peuvent souhaiter alléger cette fonction en mode permissif. 

**Syntaxe**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Paramètres**  
* `callback` - Fonction à appeler lorsque cet appel est terminé. Elle doit être au format: `Function(Boolean permissiveMode)` où `permissiveMode` indique si le navigateur est actuellement en mode permissif. Si elle est nulle ou non définie, une erreur s’est produite dans l’opération get.

**Configuration requise**  
Windows10, version1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
L’application web de test doit appeler cette fonction pour activer et désactiver le mode permissif. En mode permissif, un navigateur doit alléger certains de ses crochets de sécurité restrictifs pour permettre à une technologie d’assistance de fonctionner avec le navigateur sécurisé. Par exemple, les navigateurs qui empêchent de manière agressive les interfaces utilisateur d’autres applications à se présenter par-dessus eux peuvent souhaiter alléger cette fonction en mode permissif. 

**Syntaxe**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Paramètres**  
* `enable` - Valeur booléenne indiquant l’état du mode permissif prévu.  
* `callback` - Fonction à appeler lorsque cet appel est terminé. Elle doit être au format: `Function(Boolean permissiveMode)` où `permissiveMode` indique si le navigateur est actuellement en mode permissif. Si elle est nulle ou non définie, une erreur s’est produite dans l’opération set.

**Configuration requise**  
Windows10, version1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Efface le contenu du presse-papiers du système. L’application de test doit appeler cette fonction pour forcer l’effacement des données qui peuvent être stockées dans le Presse-papiers du système. La fonction **[lockDown](#lockDown)** effectue également cette opération.

**Syntaxe**  
`void SecureBrowser.security.emptyClipBoard();`

**Configuration requise**  
Windows10, version1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Obtient la liste des adresses MAC pour l’appareil. L’application de test doit appeler cette fonction pour aider au diagnostic. 

**Syntaxe**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Paramètres**  
* `callback` - Fonction à appeler lorsque cet appel est terminé. Elle doit être au format: `Function(String addressArray)` où `addressArray` est au format:`"['00:11:22:33:44:55','etc']"`.

**Notes**  
Il est difficile de s’appuient sur les adressesIP sources pour faire la distinction entre les ordinateurs utilisateur final dans les serveurs de tests, car les pare-feu/NAT/proxys sont couramment utilisés dans les établissements scolaires. Les adresses MAC permettent à l’application de faire la distinction entre les ordinateurs clients finaux derrière un pare-feu commun à des fins de diagnostic.

**Configuration requise**  
Windows10, version1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Obtient l’heure à laquelle l’application de test a été démarrée.

**Syntaxe**  
`DateTime SecureBrowser.settings.getStartTime();`

**Return**  
Objet DateTime indiquant l’heure de démarrage de l’application de test.

**Configuration requise**  
Windows10, version1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Demande si une fonctionnalité est activée ou désactivée. 

**Syntaxe**  
`Object SecureBrowser.security.getCapability(String feature)`

**Paramètres**  
`feature` - Chaîne pour identifier la fonctionnalité à interroger. Les chaînes de fonctionnalités valides sont «screenMonitoring», «printing» et «textSuggestions» (casse non prise en compte).

**Valeur renvoyée**  
Cette fonction retourne un objet ou un littéral JavaScript au format:`{<feature>:true|false}`. **true** si la fonctionnalité interrogée est activée, **false** si la fonctionnalité n’est pas activée ou si la chaîne de fonctionnalité n’est pas valide.

**Configuration requise** Windows10, version1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Active ou désactive une fonctionnalité spécifique dans le navigateur.

**Syntaxe**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Paramètres**  
* `feature` - Chaîne pour identifier la fonctionnalité à définir. Les chaînes de fonctionnalités valides sont `"screenMonitoring"`, `"printing"` et `"textSuggestions"` (casse non prise en compte).  
* `value` - Paramètre prévu pour la fonctionnalité. Doit être`"true"` ou `"false"`.  
* `onSuccess` - [facultatif] Fonction à appeler une fois l’opération set terminée. Elle doit être au format: `Function(String jsonValue)` où *jsonValue* est au format: `{<feature>:true|false|undefined}`.  
* `onError` - [facultatif] Fonction à appeler si l’opération set a échoué. Elle doit être au format: `Function(String jsonValue)` où *jsonValue* est au format: `{<feature>:true|false|undefined}`.

**Notes**  
Si la fonctionnalité ciblée est inconnue du navigateur, cette fonction transmet la valeur `undefined` à la fonction de rappel.

**Configuration requise** Windows10, version1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Vérifie si la session active est journalisée à distance.

**Syntaxe**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Valeur renvoyée**  
**true** si la session active est distante, sinon **false**.

**Configuration requise**  
Windows10, version1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Vérifie si la session active est en cours d’exécution au sein d’un ordinateur virtuel.

**Syntaxe**  
`Boolean SecureBrowser.security.isVMSession();`

**Valeur renvoyée**  
**true** si la session active est en cours d’exécution sur un ordinateur virtuel, sinon **false**.

**Notes**  
Cette vérification API ne détecte que les sessions de machine virtuelle en cours d’exécution dans certains hyperviseurs qui implémentent les API appropriées

**Configuration requise**  
Windows10, version1709

---