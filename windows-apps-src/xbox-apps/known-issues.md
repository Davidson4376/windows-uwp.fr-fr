---
title: Problèmes connus avec UWP sur la version préliminaire pour développeurs de Xbox One
description: 
area: Xbox
---

# Problèmes connus avec UWP sur la version préliminaire pour développeurs de Xbox One

La Mise à jour système de la version préliminaire pour développeurs du programme Xbox inclut des versions logicielles préliminaires anticipées et expérimentales. 
Cela signifie que certains jeux et applications courants ne fonctionneront pas comme prévu, et que vous risquez d’être confronté à des blocages occasionnels et à une perte de données. 
Si vous quittez la version préliminaire pour développeurs, votre console procédera à une réinitialisation aux paramètres d’usine. Vous devrez, par conséquent, réinstaller l’ensemble de vos jeux, de vos applications et de votre contenu.

Pour les développeurs, cela implique que certains outils et API de développement ne fonctionneront pas correctement. 
Cela signifie également que les fonctionnalités destinées à la version finale ne seront pas toutes incluses ou de qualité de mise en production. 
**En conséquence, les performances système de cette version préliminaire ne refléteront pas celles de la version finale.**

La liste ci-après met en relief certains problèmes connus que vous pouvez rencontrer dans cette version, mais cette liste n’est pas exhaustive. 

**Vos commentaires nous intéressent**. Par conséquent, n’hésitez pas à nous faire part de vos éventuels problèmes sur le forum [Développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Si vous êtes bloqué, consultez cette page ainsi que la rubrique [Forum Aux Questions](frequently-asked-questions.md), et utilisez le forum pour demander de l’aide.


## Développement de jeux

### Prise en charge x86/x64

La version finale que nous commercialiserons plus tard dans l’année offrira une prise en charge complète de x86 et de x64, et nous gérons déjà x86 dans cette version préliminaire. 
Toutefois, à ce jour, nous avons effectué un plus grand nombre de tests pour x64 (l’interpréteur de commandes Xbox et toutes les applications actuellement exécutées sur la console reposent sur un processeur x64), et nous vous recommandons donc d’utiliser x64 pour vos projets. 
Cette recommandation s’applique tout spécialement aux jeux.

Si vous décidez d’utiliser x86, signalez vos éventuels problèmes sur le forum.

Reportez-vous également à la section [Alternance de configurations de build pouvant entraîner des problèmes de déploiement](known-issues.md#switching-build-flavors-can-cause-deployment-failures) plus loin sur cette page.

### Moteurs de jeu

Nous n’avons testé que quelques-uns des moteurs de jeu les plus courants, et notre couverture des tests de cette version préliminaire est incomplète. 
Votre propre expérience peut donc différer. 
Nous souhaiterions obtenir vos commentaires sur les problèmes que vous rencontrez. 
Utilisez le forum pour nous faire part de vos difficultés.

### Prise en charge de DirectX 12

UWP sur Xbox One prend en charge la fonctionnalité DirectX 11 niveau 10. 
DirectX 12 n’est pas pris en charge pour l’instant. 
À l’instar de toutes les consoles de jeu traditionnelles, Xbox One est un matériel spécialisé qui requiert un Kit de développement logiciel (SDK) spécifique pour fonctionner au maximum de ses capacités. 
Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel Xbox One, vous pouvez vous inscrire auprès du programme [ID@XBOX](http://www.xbox.com/en-us/Developers/id) pour accéder à ce Kit de développement logiciel, qui inclut la prise en charge de DirectX 12.


## Ressources système pour les applications et jeux UWP sur Xbox One

Les applications et jeux UWP qui s’exécutent sur Xbox One partagent des ressources avec le système et d’autres applications. Par conséquent, les ressources disponibles pour un jeu ou une application quelconques sont régies par le système. 
Vos éventuels problèmes de mémoire ou de performances peuvent donc découler de cette situation. 
Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).


## Mise en réseau à l’aide de sockets traditionnels

Dans cette version préliminaire pour développeurs, les accès réseau entrants et sortants de la console qui utilisent des sockets TCP/UDP traditionnels (WinSock, Windows.Networking.Sockets) ne sont pas disponibles. 
Les développeurs peuvent toujours utiliser HTTP et WebSockets. 


## Couverture des API UWP

Les API UWP ne fonctionnent pas toutes comme prévu sur Xbox dans cette version préliminaire. 
Pour obtenir la liste des API inopérantes, voir [Limitations des fonctionnalités de la famille d’appareils UWP sur Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755). 
Si vous rencontrez des problèmes avec d’autres API, signalez-les sur les forums. 

## Différences d’aspect et de comportement entre les contrôles XAML et les contrôles de l’interpréteur de commandes Xbox One

Les contrôles XAML qui figurent dans cette version préliminaire n’apparaissent pas sous leur forme finale. Plus particulièrement :
* La navigation X-Y par boîtier de commande ne fonctionne pas correctement pour certains contrôles.
* Les contrôles ne ressemblent pas à ceux de l’interpréteur de commandes Xbox. Cela s’applique notamment au contrôle de rectangle de focus.
* La navigation entre les différents contrôles ne s’accompagne pas automatiquement d’effets sonores.

Ces problèmes seront résolus dans une future version préliminaire pour développeurs.

## Problèmes Visual Studio et de déploiement

### Alternance de configurations de build pouvant entraîner des problèmes de déploiement

Le fait d’alterner entre des builds de débogage et de mise en production, entre x86 et x64, ou entre des builds managées et natives .Net peut entraîner l’échec des déploiements. 

La solution la plus simple pour éviter ces problèmes dans cette version préliminaire consiste à s’en tenir à la build de débogage et à une seule architecture. 

Si vous rencontrez ce problème, vous pourrez généralement le résoudre en désinstallant votre application dans l’application Collections de votre console Xbox One.

> **Remarque**&nbsp;&nbsp;La désinstallation de votre application à partir de Windows Device Portal (WDP) ne résoudra pas le problème.

Si vos problèmes persistent, désinstallez votre application ou jeu dans l’application Collections, quittez le mode développeur, redémarrez en mode commercial, puis rebasculez en mode développeur.

Pour plus d’informations, voir la section « Résolution des problèmes de déploiement » de la rubrique [Forum aux questions](frequently-asked-questions.md).

### Échec silencieux de la désinstallation d’une application en cours de débogage dans Visual Studio

Une tentative de désinstallation d’une application en cours d’exécution sous le débogueur par le biais de l’outil WDP « Applications installées » se soldera par un échec silencieux. 
Vous pouvez contourner le problème en arrêtant le débogage de l’application dans Visual Studio avant d’essayer de supprimer cette application par l’intermédiaire de WDP.

### Échecs de couplage de code confidentiel Visual Studio/Xbox

Le couplage de code confidentiel entre Visual Studio et votre console Xbox One peut passer à l’état désynchronisé. 
Si le couplage de code confidentiel échoue, utilisez le bouton « Supprimer tous les couplages » de l’outil Accueil du développeur, redémarrez Xbox One, redémarrez votre PC de développement, puis réessayez. 


## Version préliminaire de Windows Device Portal (WDP)

### Blocage de l’outil Accueil du développeur lors du démarrage de WDP à partir de ce dernier

Lorsque vous démarrez WDP dans l’outil Accueil du développeur, ce dernier se bloque une fois que vous avez entré votre nom d’utilisateur et votre mot de passe et que vous avez cliqué sur **Enregistrer**. 
Les informations d’identification sont enregistrées, mais WDP ne démarre pas. 
Vous pouvez lancer WDP en redémarrant Xbox One. 

### Désactivation de WDP inopérante dans Accueil du développeur

Si vous désactivez WDP dans l’outil Accueil du développeur, WDP se désactive. 
Toutefois, le redémarrage de votre console Xbox One entraînera automatiquement celui de WDP. 
Vous pouvez contourner ce problème en utilisant **Réinitialiser et conserver mes jeux et applications** pour supprimer tous les états stockés sur votre console Xbox One. 
Accédez à Paramètres > Système > Informations et mise à jour de la console > Réinitialiser la console, puis sélectionnez le bouton **Réinitialiser et conserver mes jeux et applications**.

> **Attention**&nbsp;&nbsp;Cette opération supprimera tous les paramètres enregistrés sur votre console Xbox One, notamment les paramètres sans fil, les comptes d’utilisateurs et tous les états d’avancement de jeu non enregistrés dans le stockage cloud.

> **Attention**&nbsp;&nbsp;Ne sélectionnez PAS le bouton **Réinitialiser et supprimer tous les éléments**.
Cette opération supprimera tous vos jeux, applications, paramètres et contenus, désactivera le mode développeur et supprimera votre console du groupe Developer Preview.

### Mise à jour incorrecte des colonnes du tableau « Applications en cours d’exécution » 

Il est parfois possible de résoudre ce problème en effectuant un tri sur une colonne du tableau.

### Affichage incorrect de l’interface utilisateur de WDP dans Internet Explorer 11 

Par défaut, l’interface utilisateur de WDP ne s’affiche pas correctement dans le navigateur lorsque vous utilisez Internet Explorer 11. 
Vous pouvez résoudre ce problème en désactivant l’Affichage de compatibilité d’Internet Explorer 11 pour WDP.

### Avertissement de sécurité déclenché par l’accès à WDP

Vous recevrez un avertissement concernant le certificat fourni, semblable à celui illustré ci-dessous, 
car le certificat de sécurité signé par votre console Xbox One n’est pas considéré comme un éditeur approuvé bien connu. 
Cliquez sur « Poursuivre sur ce site web » pour accéder à Windows Device Portal.

![Avertissement concernant le certificat de sécurité d’un site web](images/security_cert_warning.jpg)

## Accueil du développeur
La sélection de l’option « Gérer Windows Device Portal » dans l’outil Accueil du développeur entraîne parfois une fermeture silencieuse et le réaffichage de l’écran d’accueil de l’outil Accueil du développeur. 
Ce problème est dû à une défaillance de l’infrastructure WDP sur la console et peut être résolu grâce au redémarrage de la console.

## Voir aussi
- [Forum Aux Questions](frequently-asked-questions.md)
- [UWP sur Xbox One](index.md)


<!--HONumber=Mar16_HO5-->


