---
author: TylerMSFT
Description: "L’API JavaScript pour Microsoft Examen vous permet de sécuriser les évaluations. L’application Examen inclut un navigateur sécurisé qui empêche les étudiants d’utiliser un autre ordinateur ou des ressources Internet pendant l’examen."
title: API JavaScript Examen.
translationtype: Human Translation
ms.sourcegitcommit: 7f578d73a9a625b0ac7d9c10f6dc8118c36b07d0
ms.openlocfilehash: c2e1832489d36f4ccbeae4e2f67e18caf941a68f

---

# API JavaScript Examen

[Examen](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) est une application de navigateur qui génère des évaluations en ligne dans un environnement verrouillé pour des examens dont les enjeux sont importants. Elle prend en charge la norme d’API de navigateur SBAC pour les examens de tronc commun dont les enjeux sont importants et vous permet de vous concentrer sur le contenu de l’évaluation plutôt que sur la façon de verrouiller Windows.

L’application Examen, optimisée par le navigateur Edge de Microsoft, intègre une API JavaScript que les applications web peuvent utiliser pour proposer une expérience verrouillée pour passer des examens.

L’API (basée sur [l’API Common Core SBAC](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)) fournit une fonction de conversion de texte par synthèse vocale, avec la possibilité de demander si l’appareil est verrouillé, quels sont les processus système et utilisateur en cours d’exécution, etc.

Voir les [Informations techniques de référence sur l’application Examen](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) pour plus d’informations sur l’application elle-même.

> [!Important]
> Ces API ne fonctionnent pas dans une session à distance.  

Pour obtenir de l’aide pour la résolution des problèmes, voir [Résoudre les problèmes de Microsoft Examen avec l’observateur d’événements](troubleshooting.md).

## Documentation de référence
L’API Examen inclut les espaces de noms suivants. 

| Espace de noms | Description |
|-----------|-------------|
|[espace de noms de sécurité](#security-namespace)|Vous permet de verrouiller l’appareil|
|[espace de noms tts](#tts-namespace)|Fonctionnalité de conversion de texte par synthèse vocale|


 ### Espace de noms de sécurité

L’espace de noms de sécurité vous permet de verrouiller l’appareil, de vérifier la liste des processus utilisateur et système, d’obtenir des adresses IP et MAC, et d’effacer des ressources web mises en cache.

| Méthode | Description   |
|--------|---------------|
|[clearCache](#clearCache) | Efface les ressources web mises en cache |
|[close](#close) | Ferme le navigateur et déverrouille l’appareil |
|[enableLockDown](#enableLockDown) | Verrouille l’appareil. Également utilisé pour déverrouiller l’appareil |
|[getIPAddressList](#getIPAddressList) | Obtient la liste des adresses IP pour l’appareil |
|[getMACAddress](#getMACAddress)|Obtient la liste des adresses MAC pour l’appareil|
|[getProcessList](#getProcessList)|Obtient la liste des processus utilisateur et système en cours d’exécution|
|[isEnvironmentSecure](#isEnvironmentSecure)|Détermine si le contexte de verrouillage est toujours appliqué à l’appareil|  

---
<span id="clearCache"/>
### void clearCache()
Efface les ressources web mises en cache.

**Syntaxe**  
`browser.security.clearCache();`

**Paramètres**  
`None`

**Valeur de retour**  
`None`

**Configuration requise**  
Windows10, version1607

---

<span id="close"/>
### close(boolean restart)
Ferme le navigateur et déverrouille l’appareil.

**Syntaxe**  
`browser.security.close(false);`

**Paramètres**  
`restart` -ce paramètre est ignoré, mais doit être fourni.

**Valeur de retour**  
`None`

**Configuration requise**  
Windows10, version1607

---

<span id="enableLockDown"/>
### enableLockdown(boolean lockdown)
Verrouille l’appareil. Également utilisé pour déverrouiller l’appareil.

**Syntaxe**  
`browser.security.enableLockDown(true|false);`

**Paramètres**  
`lockdown` - `true` pour exécuter l’application Examen au-dessus de l’écran de verrouillage et appliquer les stratégies décrites dans ce [document](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). `False` arrête l’exécution de l’application Examen au-dessus de l’écran de verrouillage et la ferme, sauf si l’application n’est pas verrouillée, auquel cas il est sans effet.

**Valeur de retour**  
`None`

**Configuration requise**  
Windows10, version1607

---

<span id="getIPAddressList"/>
### string[] getIPAddressList()
Obtient la liste des adresses IP pour l’appareil.

**Syntaxe**  
`browser.security.getIPAddressList();`

**Paramètres**  
`None`

**Valeur de retour**  
`An array of IP addresses.`

---

<span id="getMACAddress" />
### string[] getMACAddress()
Obtient la liste des adresses MAC pour l’appareil.

**Syntaxe**  
`browser.security.getMACAddress();`

**Paramètres**  
`None`

**Valeur de retour**  
`An array of MAC addresses.`

**Configuration requise**  
Windows10, version1607

---

<span id="getProcessList" />
### string[] getProcessList()
Obtient la liste des processus en cours d’exécution de l’utilisateur.

**Syntaxe**  
`browser.security.getProcessList();`

**Paramètres**  
`None`

**Valeur de retour**  
`An array of running process names.`

**Remarques** La liste n’inclut pas les processus système.

**Configuration requise**  
Windows10, version1607

---

<span id="isEnvironmentSecure" />
### boolean isEnvironmentSecure()
Détermine si le contexte de verrouillage est toujours appliqué à l’appareil.

**Syntaxe**  
`browser.security.isEnvironmentSecure();`

**Paramètres**  
`None`

**Valeur de retour**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**Configuration requise**  
Windows10, version1607

---

### Espace de noms tts

L’espace de noms tts gère la fonctionnalité de conversion de texte par synthèse vocale de l’application.

| Méthode | Description |
|--------|-------------|
|[getStatus](#getStatus) | Obtient l’état de lecture des fonctions vocales|
|[getVoices](#getVoices) | Obtient la liste des modules vocaux disponibles|
|[pause](#pause)|Met en pause la synthèse vocale|
|[resume](#resume)|Reprend la synthèse vocale suspendue|
|[speak](#speak)|Conversion de texte par synthèse vocale côté client|
|[stop](#stop)|Arrête la synthèse vocale|

> [!Tip]
> [L’API Speech Synthesis de Microsoft Edge](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) est une implémentation de l’[API W3C Speech](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) et nous recommandons aux développeurs de l’utiliser quand cela est possible.

---

<span id="getStatus" />
### string getStatus()
Obtient l’état de lecture des fonctions vocales.

**Syntaxe**  
`browser.tts.getStatus();`

**Paramètres**  
`None`

**Valeur de retour**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**Configuration requise**  
Windows10, version1607

---

<span id="getVoices" />
### string[] getVoices()
Obtient la liste des modules vocaux disponibles.

**Syntaxe**  
`browser.tts.getVoices();`

**Paramètres**  
`None`

**Valeur de retour**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**Configuration requise**  
Windows10, version1607

---

<span id="pause" />
### void pause()

Met en pause la synthèse vocale.

**Syntaxe**  
`browser.tts.pause();`

**Paramètres**

`None`

**Valeur de retour**

`None`

**Configuration requise**  
Windows10, version1607

---

<span id="resume" />
### void resume()
Reprend la synthèse vocale suspendue.

**Syntaxe**  
`browser.tts.resume();`

**Paramètres**
`None`

**Valeur de retour**
`None`

**Configuration requise**  
Windows10, version1607

---

<span id="speak" />
### void speak(string text, object options, function callback)
Démarre la conversion de texte par synthèse vocale côté client.

**Syntaxe**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Paramètres**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**Valeur de retour**  
`None`

**Remarques** Les variables des options doivent être en minuscules. Les paramètres vocaux, de langue et de sexe acceptent les chaînes.
Le volume, la tonalité et le taux doivent être marqués dans le fichier SSML (Speech Synthesis Markup Language), et non dans l’objet options.
L’objet options doit suivre l’ordre, le nommage et la casse illustrés dans l’exemple suivant.

**Configuration requise**  
Windows10, version1607

---

<span id="stop" />
### void stop()
Arrête la synthèse vocale.

**Syntaxe**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Paramètres**  
`None`

**Valeur de retour**  
`None`

**Configuration requise**  
Windows10, version1607



<!--HONumber=Nov16_HO1-->


