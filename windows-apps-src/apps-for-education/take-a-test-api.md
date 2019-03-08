---
Description: L’API JavaScript pour Microsoft Examen vous permet de sécuriser les évaluations. L’application Examen inclut un navigateur sécurisé qui empêche les étudiants d’utiliser un autre ordinateur ou des ressources Internet pendant l’examen.
title: API JavaScript Examen.
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: Windows 10, uwp, l’enseignement
ms.localizationpriority: medium
ms.openlocfilehash: bee8a04e3b4d57caf7da3e21f2be3c789d83be90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627594"
---
# <a name="take-a-test-javascript-api"></a>API JavaScript Examen

[Effectuer un Test](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) est une application UWP basée sur navigateur qui restitue verrouillé des évaluations en ligne pour le test de l’important, ce qui permet aux enseignants pour vous concentrer sur l’évaluation contenu plutôt que de la façon de fournir une sécurité test environnement. Pour y parvenir, elle utilise une API JavaScript que n’importe quelle application web peut utiliser. L’API d’Examen prend en charge la [norme d’API de navigateur SBAC](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) pour des examens courants dont les enjeux sont importants.

Consultez [Informations techniques de référence sur l’application Examen](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) pour plus d’informations sur l’application. Pour obtenir de l’aide pour la résolution des problèmes, voir [Résoudre les problèmes de Microsoft Examen avec l’observateur d’événements](troubleshooting.md).

## <a name="reference-documentation"></a>Documentation de référence
Les API Examen existent dans les espaces de noms suivants. Notez que toutes les API dépendent d’un objet `SecureBrowser` global.

| Espace de noms | Description |
|-----------|-------------|
|[espace de noms de sécurité](#security-namespace)|Contient les API qui permettent de verrouiller le périphérique pour le test et de mettre en œuvre un environnement de test. |

### <a name="security-namespace"></a>Espace de noms de sécurité

L’espace de noms de sécurité vous permet de verrouiller l’appareil, vérifiez la liste des processus du système et utilisateur, obtenir des adresses IP et MAC et effacer les ressources mises en cache web.

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
Verrouille l’appareil. Également utilisé pour déverrouiller l’appareil. L’application web de test appelle cet appel avant d’autoriser les étudiants à démarrer le test. Le responsable de l’implémentation doit prendre les mesures nécessaires pour sécuriser l’environnement de test. Les mesures prises pour sécuriser l’environnement sont des périphériques spécifiques et, par exemple, inclure des aspects tels que des captures d’écran de la désactivation, la désactivation de la conversation vocale en mode sécurisé, effacer le Presse-papiers du système, une en mode plein écran, la désactivation des espaces dans OS x 10.7 + appareils, etc. Tester l’application sera activer le verrouillage avant une évaluation commence et désactive le verrouillage lors de l’étudiant a terminé l’évaluation et qu’il manque le test sécurisé.

**Syntaxe**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Paramètres**  
* `enable` - **true** pour exécuter l’application Take a Test au-dessus de l’écran de verrouillage et appliquer les stratégies présentées dans cet [document](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** arrête l’exécution de l’application Examen au-dessus de l’écran de verrouillage et la ferme, sauf si l’application n’est pas verrouillée, auquel cas il est sans effet.  
* `onSuccess` -[facultatif] la fonction à appeler une fois que le verrouillage a été activé ou désactivé. Elle doit être au format `Function(Boolean currentlockdownstate)`.  
* `onError` -[facultatif] la fonction à appeler en cas d’échec de l’opération de verrouillage. Elle doit être au format `Function(Boolean currentlockdownstate)`.  

**Configuration requise**  
Windows 10 version 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Détermine si le contexte de verrouillage est toujours appliqué à l’appareil. L’application web de test l’appelle avant d’autoriser les étudiants à démarrer le test et régulièrement une fois dans le test.

**Syntaxe**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Paramètres**  
* `callback` -La fonction à appeler lorsque cette fonction est terminée. Elle doit être au format `Function(String state)` où `state` est une chaîne JSON contenant deux champs. Le premier est le champ `secure`, qui affiche `true` uniquement si tous les verrous nécessaires ont été activés (ou les fonctionnalités désactivées) pour activer un environnement de test sécurisé et qu’aucun de ces éléments n’a été compromis depuis que l’application est entré en mode verrouillage. L’autre champ, `messageKey`, inclut d’autres détails ou informations spécifiques du fournisseur. Le but ici est de permettre aux fournisseurs de placer des informations supplémentaires qui augmentent le drapeau `secure` booléen :

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Configuration requise**  
Windows 10 version 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
Obtient des informations sur la plateforme sur laquelle l’application de test est en cours d’exécution. Cette fonction est utilisée pour augmenter toutes les informations qui étaient visibles pour l’agent utilisateur.

**Syntaxe**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Paramètres**  
* `callback` -La fonction à appeler lorsque cette fonction est terminée. Elle doit être au format `Function(String infoObj)` où `infoObj` est une chaîne JSON contenant plusieurs champs. Les champs suivants doivent être pris en charge :
    * `os` représente le type de système d’exploitation (par exemple : Windows, macOS, Linux, iOS, Android, etc.)
    * `name` représente le nom de la version du système d’exploitation, le cas échéant (par exemple : Sierra Ubuntu).
    * `version` représente la version du système d’exploitation (par exemple : 10.1, 10 pro, etc..)
    * `brand` représente la personnalisation du navigateur sécurisé (par exemple : OAKS, autorité de certification, SmarterApp, etc.)
    * `model` représente le modèle d’appareil pour les appareils mobiles uniquement ; /non utilisé pour les navigateurs de bureau.

**Configuration requise**  
Windows 10 version 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Obtient la liste de tous les processus en cours d’exécution sur l’ordinateur client appartenant à l’utilisateur. L’application de test l’appelle pour examiner la liste et la compare avec une liste des processus qui ont été considérés comme placés sur liste rouge au cours du cycle de test. Cet appel doit être appelé à la fois au début d’une évaluation et régulièrement pendant que l’étudiant réalise l’évaluation. Si un processus placé sur liste rouge est détecté, l’évaluation doit être arrêtée pour préserver l’intégrité du test.

**Syntaxe**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Paramètres**  
* `blacklistedProcessList` -Liste des processus que l’application de test a sur liste rouge.  
`callback` -La fonction à appeler une fois que les processus actifs ont été trouvés. Elle doit être au format : `Function(String foundBlacklistedProcesses)` où `foundBlacklistedProcesses` est au format :`"['process1.exe','process2.exe','processEtc.exe']"`. Elle sera vide si aucun processus placé sur liste rouge n’a été trouvé. Si elle est nulle, cela indique qu’une erreur s’est produite dans l’appel de fonction d’origine.

**Remarques** La liste n’inclut pas les processus système.

**Configuration requise**  
Windows 10 version 1709

---

<span id="close"/>

### <a name="close"></a>close
Ferme le navigateur et déverrouille l’appareil. L’application de test doit l’appeler lorsque l’utilisateur choisit de quitter le navigateur.

**Syntaxe**  
`void SecureBrowser.security.close(restart);`

**Paramètres**  
* `restart` -Ce paramètre est ignoré, mais doit être fourni.

**Remarque** Dans Windows 10, version 1607, l’appareil doit être verrouillé initialement. Dans les versions ultérieures, cette méthode ferme le navigateur, que l’appareil soit verrouillé ou non.

**Configuration requise**  
Windows 10 version 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
L’application web de test doit appeler cette fonction pour déterminer si le mode permissif est activé ou désactivé. En mode permissif, un navigateur doit alléger certains de ses crochets de sécurité restrictifs pour permettre à une technologie d’assistance de fonctionner avec le navigateur sécurisé. Par exemple, les navigateurs qui empêchent de manière agressive les interfaces utilisateur d’autres applications à se présenter par-dessus eux peuvent souhaiter alléger cette fonction en mode permissif. 

**Syntaxe**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Paramètres**  
* `callback` -La fonction à appeler lorsque cet appel est terminé. Elle doit être au format : `Function(Boolean permissiveMode)` où `permissiveMode` indique si le navigateur est actuellement en mode permissif. Si elle est nulle ou non définie, une erreur s’est produite dans l’opération get.

**Configuration requise**  
Windows 10 version 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
L’application web de test doit appeler cette fonction pour activer et désactiver le mode permissif. En mode permissif, un navigateur doit alléger certains de ses crochets de sécurité restrictifs pour permettre à une technologie d’assistance de fonctionner avec le navigateur sécurisé. Par exemple, les navigateurs qui empêchent de manière agressive les interfaces utilisateur d’autres applications à se présenter par-dessus eux peuvent souhaiter alléger cette fonction en mode permissif. 

**Syntaxe**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Paramètres**  
* `enable` -La valeur booléenne indiquant l’état du mode permissif prévue.  
* `callback` -La fonction à appeler lorsque cet appel est terminé. Elle doit être au format : `Function(Boolean permissiveMode)` où `permissiveMode` indique si le navigateur est actuellement en mode permissif. Si elle est nulle ou non définie, une erreur s’est produite dans l’opération set.

**Configuration requise**  
Windows 10 version 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Efface le contenu du presse-papiers du système. L’application de test doit appeler cette fonction pour forcer l’effacement des données qui peuvent être stockées dans le Presse-papiers du système. La fonction **[lockDown](#lockDown)** effectue également cette opération.

**Syntaxe**  
`void SecureBrowser.security.emptyClipBoard();`

**Configuration requise**  
Windows 10 version 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Obtient la liste des adresses MAC pour l’appareil. L’application de test doit appeler cette fonction pour aider au diagnostic. 

**Syntaxe**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Paramètres**  
* `callback` -La fonction à appeler lorsque cet appel est terminé. Elle doit être au format : `Function(String addressArray)` où `addressArray` est au format :`"['00:11:22:33:44:55','etc']"`.

**Remarques**  
Il est difficile de s’appuient sur les adresses IP sources pour faire la distinction entre les ordinateurs utilisateur final dans les serveurs de tests, car les pare-feu/NAT/proxys sont couramment utilisés dans les établissements scolaires. Les adresses MAC permettent à l’application de faire la distinction entre les ordinateurs clients finaux derrière un pare-feu commun à des fins de diagnostic.

**Configuration requise**  
Windows 10 version 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Obtient l’heure à laquelle l’application de test a été démarrée.

**Syntaxe**  
`DateTime SecureBrowser.settings.getStartTime();`

**retour**  
Objet DateTime indiquant l’heure de démarrage de l’application de test.

**Configuration requise**  
Windows 10 version 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Demande si une fonctionnalité est activée ou désactivée. 

**Syntaxe**  
`Object SecureBrowser.security.getCapability(String feature)`

**Paramètres**  
`feature` -La chaîne pour déterminer quelle fonctionnalité de requête. Les chaînes de fonctionnalités valides sont « screenMonitoring », « printing » et « textSuggestions » (casse non prise en compte).

**Valeur de retour**  
Cette fonction retourne un objet ou un littéral JavaScript au format :`{<feature>:true|false}`. **true** si la fonctionnalité interrogée est activée, **false** si la fonctionnalité n’est pas activée ou si la chaîne de fonctionnalité n’est pas valide.

**Configuration requise** Windows 10, version 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Active ou désactive une fonctionnalité spécifique dans le navigateur.

**Syntaxe**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Paramètres**  
* `feature` -La chaîne pour déterminer quelle fonctionnalité à définir. Les chaînes de fonctionnalités valides sont `"screenMonitoring"`, `"printing"` et `"textSuggestions"` (casse non prise en compte).  
* `value` -Le paramètre souhaité pour la fonctionnalité. Doit être`"true"` ou `"false"`.  
* `onSuccess` -[facultatif] la fonction à appeler une fois l’opération de définition a été terminée avec succès. Elle doit être au format : `Function(String jsonValue)` où *jsonValue* est au format : `{<feature>:true|false|undefined}`.  
* `onError` -[facultatif] la fonction à appeler en cas d’échec de l’opération de définition. Elle doit être au format : `Function(String jsonValue)` où *jsonValue* est au format : `{<feature>:true|false|undefined}`.

**Remarques**  
Si la fonctionnalité ciblée est inconnue du navigateur, cette fonction transmet la valeur `undefined` à la fonction de rappel.

**Configuration requise** Windows 10, version 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Vérifie si la session active est journalisée à distance.

**Syntaxe**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Valeur de retour**  
**true** si la session active est distante, sinon **false**.

**Configuration requise**  
Windows 10 version 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Vérifie si la session active est en cours d’exécution au sein d’un ordinateur virtuel.

**Syntaxe**  
`Boolean SecureBrowser.security.isVMSession();`

**Valeur de retour**  
**true** si la session active est en cours d’exécution sur un ordinateur virtuel, sinon **false**.

**Remarques**  
Cette vérification API ne détecte que les sessions de machine virtuelle en cours d’exécution dans certains hyperviseurs qui implémentent les API appropriées

**Configuration requise**  
Windows 10 version 1709

---