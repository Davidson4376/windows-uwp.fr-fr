---
author: mcleanbyron
description: "Découvrez comment mettre à jour votre application afin d’utiliser les bibliothèques de publicités Microsoft les plus récentes prises en charge et assurez-vous que votre application continue à recevoir des bannières publicitaires."
title: "Mettre à jour votre application avec les bibliothèques de publicités Microsoft les plus récentes"
translationtype: Human Translation
ms.sourcegitcommit: 9bd83a41ea4ec4ec7a75ef89e9c92f73d86cc731
ms.openlocfilehash: 710a5f4a3ae566550939fe783af7e97d20dd1b28


---

# Mettre à jour votre application avec les bibliothèques de publicités Microsoft les plus récentes

À compter de janvier2017, nous ne livrerons plus de bannières publicitaires aux applications qui utilisent des versions plus anciennes du Kit de développement logiciel (SDK) Microsoft Advertising. Si vous disposez d’une application existante (déjà dans le Windows Store ou toujours en cours de développement) qui affiche des bannières publicitaires à l’aide de **AdControl** ou **AdMediatorControl**, vous devrez peut-être mettre à jour votre application pour utiliser la dernière version du SDK Microsoft Advertising pour que votre application continue à recevoir des bannières publicitaires en janvier2017. Suivez les instructions fournies dans cet article pour déterminer si votre application est concernée par ce changement et savoir comment la mettre à jour si nécessaire.

Si votre application est concernée par ce changement et que vous ne mettez pas à jour votre application pour utiliser le dernier SDK Microsoft Advertising, vous verrez le comportement suivant à partir de janvier 2017:

* Les bannières publicitaires ne seront plus fournies aux contrôles **AdControl** ou **AdMediatorControl** dans votre application, et vous ne gagnerez plus de revenus publicitaires à partir de ces contrôles.

* Lorsque le contrôle **AdControl** ou **AdMediatorControl** de votre application demande une nouvelle publicité, l’événement **ErrorOccurred** du contrôle est déclenché et la propriété **ErrorCode** des arguments d’événement prend la valeur **NoAdAvailable**.

Concernant cette modification, voici plus de contexte: nous supprimons la prise en charge des versions antérieures du SDK Microsoft Advertising ne prenant pas en charge un ensemble minimal de fonctionnalités, telles que la capacité à fournir des médias enrichis HTML5 par le biais de la [spécification MRAID (Mobile Rich-media Ad Interface Definitions) 1.0](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) de l’Interactive Advertising Bureau (IAB). La plupart de nos annonceurs veulent profiter de ces fonctionnalités. Nous apportons donc cette modification pour rendre notre écosystème d’applications plus attrayant pour eux, ce qui par ailleurs augmentera vos revenus.

Si vous rencontrez des problèmes ou si vous avez besoin d’une assistance, [contactez le support](http://go.microsoft.com/fwlink/?LinkId=393643).

>**Remarque**&nbsp;&nbsp;Si vous avez précédemment mis à jour votre application pour utiliser [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (pour les applicationsUWP) ou le [SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) (pour les applications Windows8.1 et Windows Phone8.x), votre application utilise déjà le dernier SDK Microsoft Advertising disponible et vous n’avez pas besoin d’apporter d’autres modifications à votre application.

## Éléments prérequis

* Le code source complet et les fichiers de projet Visual Studio de votre application qui utilise **AdControl** ou **AdMediatorControl**.

* Le package .appx ou .xap de votre application.

  >**Remarque**&nbsp;&nbsp;Si vous n’avez plus le package .appx ou .xap de votre application, mais que vous disposez toujours d’un ordinateur de développement avec la version de Visual Studio et le SDK Microsoft Advertising utilisé pour générer votre application, vous pouvez regénérer le package .appx ou .xap dans Visual Studio.

<span id="part-1" />
## Partie 1: Déterminez si vous avez besoin de mettre à jour votre application

Suivez les instructions dans les sections suivantes pour déterminer si vous devez mettre à jour de votre application.

### Votre application utilise AdControl

Si votre application utilise **AdControl** pour afficher des bannières publicitaires, suivez ces instructions.

**Applications UWP pour Windows10**

1. Créez une copie du package .appx pour votre application afin de ne pas modifier l’original, renommez la copie pour qu’elle ait une extension .zip et extrayez le contenu du fichier.

2. Vérifier le contenu extrait du package de votre application:

  * Si vous voyez un fichier Microsoft.Advertising.dll, votre application utilise un ancien SDK et vous devez mettre à jour votre projet en suivant les instructions dans les sections ci-dessous. Passez à la [Partie 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

  * Si vous ne voyez pas de fichier Microsoft.Advertising.dll, votre application UWP utilise déjà le SDK Microsoft Advertising le plus récent disponible et vous n’avez pas besoin d’apporter des modifications à votre projet.

<span/>

**Applications Windows8.1 ou Windows Phone8.x**

1. Créez une copie du package .appx ou .xap pour votre application afin de ne pas modifier l’original, renommez la copie pour qu’elle ait une extension .zip et extrayez le contenu du fichier.

2. Pour les applications XAML ou JavaScript/HTML, vérifiez le contenu extrait du package de votre application:

  * Si vous voyez un fichier Microsoft.Advertising.winmd mais aucun fichier UniversalXamlAdControl.\*.dll (pour les applications XAML) ou UniversalSharedLibrary.Windows.dll (pour les applications JavaScript/HTML), votre application utilise un ancien SDK et vous devez mettre à jour votre projet en suivant les instructions dans les sections ci-dessous. Passez à la [Partie 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

  * Sinon, procédez comme suit.

2. Ouvrez Windows PowerShell, entrez la commande suivante et affectez l’argument ```-Path``` au chemin d’accès complet au contenu extrait du package de votre application. Cette commande affiche toutes les bibliothèques de publicités référencées par votre projet et la version de chaque bibliothèque.

    ```
    get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
    ```
2. Recherchez le fichier répertorié dans le tableau suivant pour la plateforme cible de votre application et comparez les versions de ce fichier avec la version répertoriée dans le tableau.

  <table>
    <colgroup>
      <col width="33%" />
      <col width="33%" />
      <col width="33%" />
    </colgroup>
    <thead>
      <tr class="header">
        <th align="left">Plateforme cible</th>
        <th align="left">Fichiers</th>
        <th align="left">Version</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td align="left"><p>XAML Windows8.1</p></td>
        <td align="left"><p>UniversalXamlAdControl.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>XAML Windows Phone8.1</p></td>
        <td align="left"><p>UniversalXamlAdControl.WindowsPhone.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows8.1 JavaScript/HTML<br/>Windows Phone8.1 JavaScript/HTML</p></td>
        <td align="left"><p>UniversalSharedLibrary.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Silverlight Windows Phone8.1</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>8.1.50112.0</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Silverlight Windows Phone8.0</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>6.2.40501.0</p></td>
      </tr>
    </tbody>
  </table>

3. Si le fichier a une version identique ou postérieure à la version répertoriée dans le tableau ci-dessus, il est inutile d’apporter des modifications à votre projet.

  Si le fichier a un numéro de version inférieur, vous devez mettre à jour votre projet en suivant les instructions dans les sections ci-dessous. Passez à la [Partie 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span/>

**Applications Windows8.0**

* Les applications qui ciblent Windows8.0 ne recevront plus de bannières publicitaires à partir de janvier2017. Pour éviter les pertes d’exposition, nous vous recommandons de convertir votre projet en application UWP ciblant Windows10. La plupart du trafic d’applications Windows8.0 s’exécute maintenant sur des appareils dotés de Windows10.

<span/>

**Applications Windows Phone7.x**

* Les applications qui ciblent Windows Phone7.x ne recevront plus de bannières publicitaires à partir de janvier2017. Pour éviter les pertes d’exposition, nous vous recommandons de convertir votre projet pour cibler en application Windows Phone8.1 cible ou en application UWP ciblant Windows10. La plupart du trafic d’applications Windows7.x s’exécute maintenant sur des appareils dotés de Windows8.1 ou Windows10.

<span/>

### Votre application utilise AdMediatorControl

Si votre application utilise **AdMediatorControl** pour afficher des bannières publicitaires, suivez ces instructions pour déterminer si vous devez mettre à jour de votre application.

**Applications UWP pour Windows10**

* **AdMediatorControl** n’est plus pris en charge pour les applications UWP. Vous devez migrer vers l’utilisation de **AdControl** en suivant les instructions dans les sections ci-dessous. Passez à la [Partie 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span/>

**Applications Windows8.1 ou Windows Phone8.1**

1. Créez une copie du package .appx ou .xap pour votre application afin de ne pas modifier l’original, renommez la copie pour qu’elle ait une extension .zip et extrayez le contenu du fichier.

2. Ouvrez Windows PowerShell, entrez la commande suivante et affectez l’argument ```-Path``` au chemin d’accès complet au contenu extrait du package de votre application. Cette commande affiche toutes les bibliothèques de publicités référencées par votre projet et la version de chaque bibliothèque.

    ```
    get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
    ```

2. Si la version des fichiers Microsoft.AdMediator.\*.dll répertoriés dans la sortie est la version2.0.1603.18005 ou ultérieure, vous n’avez pas besoin d’apporter des modifications à votre projet.

  Si ces fichiers ont un numéro de version inférieur, vous devez mettre à jour votre projet en suivant les instructions dans les sections ci-dessous. Passez à la [Partie 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span id="part-2" />
## Partie 2 : Installez le SDK le plus récent

Si votre application utilise une ancienne version du SDK, suivez ces instructions pour vous assurer que vous disposez du SDK le plus récent sur votre ordinateur de développement.

1. Assurez-vous que votre ordinateur de développement est équipé de Visual Studio2015 (pour les projets UWP, Windows8.1 ou Windows Phone8.x) ou Visual Studio2013 (pour les projets Windows8.1 ou Windows Phone8.x).

  >**Remarque**&nbsp;&nbsp;Si Visual Studio est ouvert sur votre ordinateur de développement, fermez-le avant de passer aux étapes suivantes.

1.  Désinstallez toutes les versions précédentes du SDK Microsoft Advertising et du SDK médiateur de publicités sur votre ordinateur de développement.

2.  Ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:

  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Installez le SDK le plus récent pour votre application:
  * Pour les applications UWP sur Windows10, installez [Microsoft Store Services SDK](http://aka.ms/store-services-sdk).
  * Pour les applications qui ciblent une version antérieure du système d’exploitation, installez le [SDK Microsoft Advertising pour Windows et Windows Phone 8.x](http://aka.ms/store-8-sdk).

## Partie 3: Mettez à jour votre projet

Suivez ces instructions pour mettre à jour votre projet.

### Projets UWP pour Windows10

<span/>

Si votre application utilise **AdMediatorControl**, [refactorisez votre application pour utiliser AdControl](migrate-from-admediatorcontrol-to-adcontrol.md) à la place. **AdMediatorControl** n’est plus pris en charge pour les applications UWP.

Si votre application utilise **AdControl**, supprimez toutes les références existantes aux bibliothèques de publicités Microsoft du projet et suivez les instructions dans [AdControl en XAML](adcontrol-in-xaml-and--net.md) ou [AdControl en HTML](adcontrol-in-html-5-and-javascript.md) pour ajouter les références requises. Ainsi vous êtes certain que votre projet utilise les bibliothèques correctes. Vous pouvez conserver le balisage et le code XAML existants.

<span/>

### Projets Windows8.1 ou Windows Phone 8.1 (XAML ou JavaScript/HTML)

<span/>

1. Supprimez toutes les références Microsoft.Advertising.\* et Microsoft.AdMediator.\* de votre projet. Vous pouvez avoir deux références (une pour Windows et une pour Windows Phone) si vous avez utilisé le modèle de projet universel.

2. Si votre application utilise **AdMediatorControl**, ajoutez à nouveau les références de la bibliothèque en suivant les instructions dans [Ajout et utilisation du contrôle de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx). Si votre application utilise **AdControl**, ajoutez à nouveau les références de la bibliothèque en suivant les instructions dans [AdControl en XAML](adcontrol-in-xaml-and--net.md) ou [AdControl en HTML](adcontrol-in-html-5-and-javascript.md).

<span/>

Points à prendre en considération:

* Si votre application a été compilée précédemment sur la plateforme **Toute UC**, vous devez recompiler votre projet vers une plateforme spécifique à l’architecture (x 86, x64 ou ARM).

* Si vous avez une application XAML Windows Phone 8.x qui utilisait auparavant une version du SDK dans laquelle **AdControl** était définie dans l’espace de noms **Microsoft.Advertising.Mobile.UI**, vous devez mettre à jour votre code pour faire référence à la classe **AdControl** dans l’espace de noms **Microsoft.Advertising.WinRT.UI** (cette classe a déplacé les espaces de noms dans les versions SDK les plus récentes).

* Sauf dans le cas du problème précédemment cité, vous pouvez conserver le balisage et le code XAML existants.

<span/>

### Projets Windows Phone8.xSilverlight

<span/>

1. Supprimez toutes les références Microsoft.Advertising.\* et Microsoft.AdMediator.\* de votre projet.

2. Si votre application utilise **AdMediatorControl**, ajoutez à nouveau les références de la bibliothèque en suivant les instructions dans [Ajout et utilisation du contrôle de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx). Si votre application utilise **AdControl**, ajoutez à nouveau les références de la bibliothèque en suivant les instructions dans [AdControl dans Silverlight Windows Phone](adcontrol-in-windows-phone-silverlight.md).

<span/>

Points à prendre en considération:

* Vous pouvez conserver le balisage et le code XAML existants.

* À partir de **l’Explorateur de solutions**, vérifiez les propriétés de la référence **Microsoft.Advertising.Mobile.UI** dans votre projet. Il doit s’agir de la version6.2.40501.0 si votre application cible Windows Phone8.0 ou 8.1.50112.0 si votre application cible Windows Phone8.1.

* Pour les applications Silverlight Windows Phone8.x, le test des unités de production sur un émulateur n’est pas pris en charge. Nous vous recommandons d’effectuer le test sur un appareil.

## Partie 4: Testez et republiez votre application

Testez votre application pour vous assurer qu’elle affiche les bannières publicitaires comme prévu.

Si la version précédente de votre application est déjà disponible dans le Windows Store, [créez une nouvelle soumission](https://msdns.microsoft.com/windows/uwp/publish/app-submissions) pour votre application mise à jour dans le tableau de bord du Centre de développement Windows pour republier votre application.





 



<!--HONumber=Nov16_HO1-->


