---
author: v-angraf
title: "Nouveautés de la plateformeUWP sur Xbox One"
description: "Cette rubrique souligne les nouvelles fonctionnalités pour la plateformeUWP sur les applicationsXboxOne."
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: edc9a914f200c643b1133cf07778e2ca3931d0d9

---

# Nouveautés de la version préliminaire pour développeurs de juillet2016 de la plateformeUWP sur XboxOne

La version préliminaire pour développeurs de juillet2016 de la plateformeUWP sur XboxOne contient les nouvelles fonctionnalités, mises à jour de fonctionnalités et résolutions de bogues suivantes.

## Mise en réseau utilisant des sockets TCP/UDP maintenant disponible  
Les accès réseau entrants et sortants de la console qui utilisent des sockets TCP/UDP traditionnels (WinSock, Windows.Networking.Sockets) sont désormais disponibles.

## Prise en charge de Fiddler
Vous pouvez désormais activer Fiddler en tant que proxy pour une console qui a activé la plateforme Windows universelle (UWP) sur XboxOne. Fiddler vous permet de consigner et d’inspecter tout le trafic HTTP/HTTPS vers et à partir de services Xbox et des services web des parties de confiance. Pour plus d’informations, voir [Utilisation de Fiddler avec XboxOne lors du développement pour UWP](uwp-fiddler.md).

## Le modesouris est désormais activé par défaut
Le modesouris est désormais activé par défaut pour les applications XAML et les applications web hébergées.
Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser l’application pour la navigation de contrôleur directionnelle.
Pour savoir comment désactiver le mode souris, voir [Comment désactiver le mode souris](how-to-disable-mouse-mode.md).
Pour plus d’informations sur la façon de créer des applications réussies pour Xbox, voir [Conception pour Xbox et télévision](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode).

## La surface d’exposition d’APIUWP étendue est fonctionnelle sur la console
Les APIUWP supplémentaires sont désormais fonctionnelles sur la consoleXbox. Pour en savoir plus sur la prise en charge des APIUWP, voir [FonctionnalitésUWP qui ne sont pas encore prises en charge surXbox](http://go.microsoft.com/fwlink/?LinkID=760755). 

## Fonctionnalités de musique et audio en arrière-plan
Vous pouvez désormais lire des fichiers de musique et audio depuis une application exécutée en arrière-plan.

## Améliorations apportées à XAML
Les améliorations suivantes ont été apportées à la plateforme XAML:
-   Le rectangle de sélection présente désormais un style adapté à une télévision de 3mètres.
-   Les sonsXbox sont désormais incorporés dans la plateformeXAML.
-   La navigation de typeXY dans le focus a été améliorée entre les éléments d’interface utilisateur. 

## Vous pouvez maintenant modifier la taille du stockage de développement sur la console.
Un nouveau paramètre dans l’application Dev Home vous permet d’augmenter ou de diminuer le volume de stockage alloué au développement sur votre console. Pour plus d’informations sur la modification de la taille de votre espace de stockage de développement, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

## Améliorations de l’outil WDP
Les améliorations suivantes ont été apportées à l’outil Windows Device Portal (WDP) pour Xbox:
 - L’outil comprend des paramètres de console supplémentaires. Pour plus d’informations sur les paramètres de la console, voir la rubrique de référence [/ext/settings](wdp-xboxsettings-api.md). 
 - Les utilisateurs peuvent se connecter ou se déconnecter sur la console. Pour plus d’informations sur les utilisateurs, voir la rubrique de référence [/ext/user](wdp-user-management.md).
 - Vous pouvez désormais effectuer une capture d’écran de la console. Pour plus d’informations sur la réalisation d’une capture d’écran, voir la rubrique de référence [/ext/screenshot](wdp-media-capture-api.md).
 - L’outil peut déployer une build de fichier isolé de votre application. Pour plus d’informations sur les builds de fichier isolé, consultez la rubrique de référence [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Il est possible d’accéder aux fichiers de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. Pour plus d’informations sur l’accès aux fichiers via l’Explorateur de fichiers, voir la rubrique de référence [/ext/smb/developerfolder](wdp-smb-api.md).

## Voir également
- [UWP sur XboxOne](index.md)
- [Problèmes connus](known-issues.md)



<!--HONumber=Jul16_HO2-->


