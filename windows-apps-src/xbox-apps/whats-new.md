---
author: v-angraf
title: "Nouveautés de la plateforme UWP sur Xbox One"
description: "Cette rubrique présente les nouvelles fonctionnalités des applications UWP sur Xbox One."
ms.author: v-angraf
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 96f9c9ef355382c72423187a7f81635571762071
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Nouveautés de la dernière mise à jour de la plateforme UWP sur Xbox One pour les développeurs

La version de juillet 2016 de la plateforme Windows universelle (UWP) sur Xbox One contient les nouvelles fonctionnalités, mises à jour de fonctionnalités et résolutions de bogues suivantes.

## <a name="networking-using-tcpudp-sockets-is-now-available"></a>La mise en réseau avec des sockets TCP/UDP est désormais disponible  
Les accès réseau entrants et sortants de la console qui utilisent des sockets TCP/UDP traditionnels (WinSock, Windows.Networking.Sockets) sont désormais disponibles.

## <a name="fiddler-support"></a>Prise en charge de Fiddler
Vous pouvez désormais activer Fiddler comme proxy pour une console qui a activé UWP sur Xbox One. Fiddler vous permet de consigner et d’inspecter tout le trafic HTTP/HTTPS vers et à partir de services Xbox et des services web des parties de confiance. Pour plus d’informations, voir [Utilisation de Fiddler avec Xbox One lors du développement pour UWP](uwp-fiddler.md).

## <a name="mouse-mode-is-now-enabled-by-default"></a>Le mode souris est désormais activé par défaut
Le mode souris est désormais activé par défaut pour les applications XAML et les applications web hébergées.
Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser la navigation par commande directionnelle.
Pour savoir comment désactiver le mode souris, voir [Comment désactiver le mode souris](how-to-disable-mouse-mode.md).
Pour plus d’informations sur la façon de créer des applications réussies pour Xbox, voir [Conception pour Xbox et télévision](../input-and-devices/designing-for-tv.md#mouse-mode).

## <a name="extended-uwp-api-surface-area-is-now-functional-on-the-console"></a>La surface d’exposition d’API UWP étendue est désormais fonctionnelle sur la console
Les API UWP supplémentaires sont désormais fonctionnelles sur la console Xbox. Pour en savoir plus sur la prise en charge des API UWP, voir [Fonctionnalités UWP qui ne sont pas encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/p/?LinkID=760755). 

## <a name="background-music-and-audio-capabilities"></a>Fonctionnalités de musique et audio en arrière-plan
Vous pouvez désormais lire des fichiers de musique et audio depuis une application exécutée en arrière-plan.

## <a name="xaml-improvements"></a>Améliorations apportées à XAML
Les améliorations suivantes ont été apportées à la plateforme XAML :
-    Le rectangle de sélection présente désormais un style adapté à une télévision de 3 mètres.
-    Les sons Xbox sont désormais incorporés dans la plateforme XAML.
-    La navigation de type XY dans le focus a été améliorée entre les éléments d’interface utilisateur. 

## <a name="you-can-now-change-the-size-of-allocated-developer-storage-on-the-console"></a>Vous pouvez maintenant modifier la taille du stockage de développement sur la console.
Un nouveau paramètre dans l’application Dev Home vous permet d’augmenter ou de diminuer le volume de stockage alloué au développement sur votre console. Pour plus d’informations sur la modification de la taille de votre espace de stockage de développement, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

## <a name="wdp-tool-enhancements"></a>Améliorations de l’outil WDP
Les améliorations suivantes ont été apportées à l’outil Windows Device Portal (WDP) pour Xbox :
 - L’outil comprend des paramètres de console supplémentaires. Pour plus d’informations sur les paramètres de la console, voir la rubrique de référence [/ext/settings](wdp-xboxsettings-api.md). 
 - Les utilisateurs peuvent se connecter ou se déconnecter sur la console. Pour plus d’informations sur les utilisateurs, voir la rubrique de référence [/ext/user](wdp-user-management.md).
 - Vous pouvez désormais effectuer une capture d’écran de la console. Pour plus d’informations sur la réalisation d’une capture d’écran, voir la rubrique de référence [/ext/screenshot](wdp-media-capture-api.md).
 - L’outil peut déployer une build de fichier isolé de votre application. Pour plus d’informations sur les builds de fichier isolé, consultez la rubrique de référence [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Il est possible d’accéder aux fichiers de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. Pour plus d’informations sur l’accès aux fichiers via l’Explorateur de fichiers, voir la rubrique de référence [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Voir également
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md)

