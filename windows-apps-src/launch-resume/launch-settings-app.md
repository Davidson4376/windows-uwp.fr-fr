---
author: TylerMSFT
title: "Lancer l’application Paramètres Windows"
description: "Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 06/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 2a30e14f7c275c48f52253157fc9c67bf05d259e
ms.sourcegitcommit: 00c3f5a1208bd0125f5b275f972cf2a82d8eb9b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2017
---
# <a name="launch-the-windows-settings-app"></a>Lancer l’application Paramètres Windows

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**API importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI **ms-settings:**. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.

Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Pour plus d’informations, voir [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/hh768223).

## <a name="how-to-launch-the-settings-app"></a>Comment lancer l’application Paramètres

Pour lancer l’application **Paramètres**, utilisez le schéma d’URI `ms-settings:` comme illustré dans les exemples suivants.

Dans cet exemple, un contrôle Hyperlink XAML est utilisé pour ouvrir la page des paramètres de confidentialité relatifs au microphone en utilisant l’URI `ms-settings:privacy-microphone`.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Par ailleurs, votre application peut appeler la méthode[**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) pour lancer l’application **Paramètres** à partir du code. Cet exemple montre comment ouvrir la page des paramètres de confidentialité relatifs à l’appareil photo en utilisant l’URI `ms-settings:privacy-webcam`.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Le code ci-dessus ouvre la page des paramètres de confidentialité relatifs à l’appareil photo:

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations sur les URI de lancement, voir [Lancer l’application par défaut pour un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Référence de schéma d’URI ms-settings:

Utilisez les URI suivants pour ouvrir diverses pages de l’application Paramètres.

> Notez que la disponibilité d’une page de paramètres varie selon la référence Windows. Toutes les pages de paramètres disponibles sur Windows10 pour éditions de bureau ne sont pas disponibles sur Windows10Mobile et inversement. La colonne Remarques capture également des exigences supplémentaires qui doivent être remplies pour qu’une page soit disponible.

<table border="1">
 <tr>
  <th>Catégorie</th>
  <th>Page de paramètres</th>
  <th>URI</th>
  <th>Remarques</th>
 </tr>
 <tr>
  <td rowspan="6">Comptes</td>
  <td>Accéder à un compte professionnel ou scolaire</td>
  <td>ms-settings:workplace</td>
  <td></td>
 </tr>
 <tr>
  <td>Comptes de messagerie et d’application</td>
  <td>ms-settings:emailandaccounts</td>
  <td></td>
 </tr>
 <tr>
  <td>Famille et autres personnes</td>
  <td>ms-settings:otherusers</td>
  <td></td>
 </tr>
 <tr>
  <td>Options de connexion</td>
  <td>ms-settings:signinoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Synchroniser vos paramètres</td>
  <td>ms-settings:sync</td>
  <td></td>
 </tr>
 <tr>
  <td>Vos informations</td>
  <td>ms-settings:yourinfo</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="4">Applications</td>
  <td>Applications et fonctionnalités</td>
  <td>ms-settings:appsfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td>Applications pour les sites web</td>
  <td>ms-settings:appsforwebsites</td>
  <td></td>
 </tr>
 <tr>
  <td>Applications pardéfaut</td>
  <td>ms-settings:defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Applications et fonctionnalités</td>
  <td>ms-settings:optionalfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="12">Appareils</td>
  <td>USB</td>
  <td>ms-settings:usb</td>
  <td></td>
 </tr>
 <tr>
  <td>Fonctions audio et vocales</td>
  <td>ms-settings:holographic-audio</td>
  <td>Disponible uniquement si l’application Portail de réalité mixte est installée (disponible dans le Windows Store)</td>
 </tr>
 <tr>
  <td>Lecture automatique</td>
  <td>ms-settings:autoplay</td>
  <td></td>
 </tr>
 <tr>
  <td>Pavé tactile</td>
  <td>ms-settings:devices-touchpad</td>
  <td>Disponible uniquement en présence d’un pavé tactile</td>
 </tr>
 <tr>
  <td>Stylet et WindowsInk</td>
  <td>ms-settings:pen</td>
  <td></td>
 </tr>
 <tr>
  <td>Imprimantes et scanneurs</td>
  <td>ms-settings:printers</td>
  <td></td>
 </tr>
 <tr>
  <td>Saisie</td>
  <td>ms-settings:typing</td>
  <td></td>
 </tr>
 <tr>
  <td>Molette</td>
  <td>ms-settings:wheel</td>
  <td>Disponible uniquement si Dial est jumelé</td>
 </tr>
 <tr>
  <td>Appareil photo par défaut</td>
  <td>ms-settings:camera</td>
  <td></td>
 </tr>
 <tr>
  <td>Bluetooth</td>
  <td>ms-settings:bluetooth</td>
  <td></td>
 </tr>
 <tr>
  <td>Appareils connectés</td>
  <td>ms-settings:connecteddevices</td>
  <td></td>
 </tr>
 <tr>
  <td>Souris et pavé tactile</td>
  <td>ms-settings:mousetouchpad</td>
  <td>Paramètres du pavé tactile disponibles uniquement sur les appareils dotés d’un pavé tactile</td>
 </tr>
 <tr>
  <td rowspan="7">Options d’ergonomie</td>
  <td>Narrateur</td>
  <td>ms-settings:easeofaccess-narrator</td>
  <td></td>
 </tr>
 <tr>
  <td>Loupe</td>
  <td>ms-settings:easeofaccess-magnifier</td>
  <td></td>
 </tr>
 <tr>
  <td>Contraste élevé</td>
  <td>ms-settings:easeofaccess-highcontrast</td>
  <td></td>
 </tr>
 <tr>
  <td>Sous-titres codés</td>
  <td>ms-settings:easeofaccess-closedcaptioning</td>
  <td></td>
 </tr>
 <tr>
  <td>Clavier</td>
  <td>ms-settings:easeofaccess-keyboard</td>
  <td></td>
 </tr>
 <tr>
  <td>Souris</td>
  <td>ms-settings:easeofaccess-mouse</td>
  <td></td>
 </tr>
 <tr>
  <td>Autres options</td>
  <td>ms-settings:easeofaccess-otheroptions</td>
 </tr>
 <tr>
  <td>Bonus</td>
  <td>Bonus</td>
  <td>ms-settings:extras</td>
  <td>Disponible uniquement si les «applications de paramètres» sont installées (par exemple, par un tiers)</td>
 </tr>
 <tr>
  <td rowspan="4">Jeux</td>
  <td>Diffusion</td>
  <td>ms-settings:gaming-broadcasting</td>
  <td></td>
 </tr>
 <tr>
  <td>Barre de jeux</td>
  <td>ms-settings:gaming-gamebar</td>
  <td></td>
 </tr>
 <tr>
  <td>Jeux DVR</td>
  <td>ms-settings:gaming-gamedvr</td>
  <td></td>
 </tr>
 <tr>
  <td>Mode jeu</td>
  <td>ms-settings:gaming-gamemode</td>
  <td></td>
 </tr>
 <tr>
  <td>Page d’accueil</td>
  <td>Page d’accueil des paramètres</td>
  <td>ms-settings:</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="10">Réseau et Internet</td>
  <td>Ethernet</td>
  <td>ms-settings:network-ethernet</td>
  <td></td>
 </tr>
 <tr>
  <td>Réseau privé virtuel</td>
  <td>ms-settings:network-vpn</td>
  <td></td>
 </tr>
 <tr>
  <td>Dial-up</td>
  <td>ms-settings:network-dialup</td>
  <td></td>
 </tr>
 <tr>
  <td>DirectAccess</td>
  <td>ms-settings:network-directaccess</td>
  <td>Disponible uniquement si DirectAccess est activé.</td>
 </tr>
 <tr>
  <td>Appels Wi-Fi</td>
  <td>ms-settings:network-wificalling</td>
  <td>Disponible uniquement si les appels Wi-Fi sont désactivés.</td>
 </tr>
 <tr>
  <td>Consommation des données</td>
  <td>ms-settings:datausage</td>
  <td></td>
 </tr>
 <tr>
  <td>Réseau cellulaire et SIM</td>
  <td>ms-settings:network-cellular</td>
  <td></td>
 </tr>
 <tr>
  <td>Point d’accès sans fil mobile</td>
  <td>ms-settings:network-mobilehotspot</td>
  <td></td>
 </tr>
 <tr>
  <td>Proxy</td>
  <td>ms-settings:network-proxy</td>
  <td></td>
 </tr>
 <tr>
  <td>État</td>
  <td>ms-settings:network-status</td>
  <td></td>
 </tr>
 <tr>
  <td>Gérer les réseaux connus</td>
  <td>ms-settings:network-wifisettings</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="3">Réseau et sans fil</td>
  <td>NFC</td>
  <td>ms-settings:nfctransactions</td>
  <td></td>
 </tr>
 <tr>
  <td>Wi-Fi</td>
  <td>ms-settings:network-wifi</td>
  <td>Disponible uniquement si l’appareil dispose d’une carte Wi-Fi</td>
 </tr>
 <tr>
  <td>Mode avion</td>
  <td>ms-settings:network-airplanemode</td>
  <td>Utilisez ms-settings:proximity sur Windows8.x</td>
 </tr>
 <tr>
  <td rowspan="10">Personnalisation</td>
  <td>Démarrer</td>
  <td>ms-settings:personalization-start</td>
  <td></td>
 </tr>
 <tr>
  <td>Thèmes</td>
  <td>ms-settings:themes</td>
  <td></td>
 </tr>
 <tr>
  <td>Coup d’œil</td>
  <td>ms-settings:personalization-glance</td>
  <td></td>
 </tr>
 <tr>
  <td>Barre de navigation</td>
  <td>ms-settings:personalization-navbar</td>
  <td></td>
 </tr>
 <tr>
  <td>Personnalisation (catégorie)</td>
   <td>ms-settings:personalization</td>
   <td></td>
 </tr>
 <tr>
  <td>Arrière-plan</td>
   <td>ms-settings:personalization-background</td>
   <td></td>
 </tr>
 <tr>
  <td>Couleurs</td>
   <td>ms-settings:personalization-colors</td>
   <td></td>
 </tr>
 <tr>
  <td>Sons</td>
   <td>ms-settings:sounds</td>
   <td></td>
 </tr>
 <tr>
  <td>Écran de verrouillage</td>
   <td>ms-settings:lockscreen</td>
   <td></td>
 </tr>
 <tr>
  <td>Barre des tâches</td>
   <td>ms-settings:taskbar</td>
   <td></td>
 </tr>
 <tr>
  <td rowspan="22">Confidentialité</td>
  <td>Diagnostics des applications</td>
   <td>ms-settings:privacy-appdiagnostics</td>
   <td></td>
 </tr>
 <tr>
  <td>Notifications</td>
   <td>ms-settings:privacy-notifications</td>
   <td></td>
 </tr>
 <tr>
  <td>Tâches</td>
   <td>ms-settings:privacy-tasks</td>
   <td></td>
 </tr>
 <tr>
  <td>Généralités</td>
   <td>ms-settings:privacy-general</td>
   <td></td>
 </tr>
 <tr>
  <td>Applications pour accessoires</td>
   <td>ms-settings:privacy-accessoryapps</td>
   <td></td>
 </tr>
 <tr>
  <td>Identifiant de publicité</td>
   <td>ms-settings:privacy-advertisingid</td>
   <td></td>
 </tr>
 <tr>
  <td>Appels téléphoniques</td>
   <td>ms-settings:privacy-phonecall</td>
   <td></td>
 </tr>
 <tr>
  <td>Emplacement</td>
   <td>ms-settings:privacy-location</td>
   <td></td>
 </tr>
 <tr>
  <td>Caméra</td>
   <td>ms-settings:privacy-webcam</td>
   <td></td>
 </tr>
 <tr>
  <td>Microphone</td>
   <td>ms-settings:privacy-microphone</td>
   <td></td>
 </tr>
 <tr>
  <td>Mouvement</td>
   <td>ms-settings:privacy-motion</td>
   <td></td>
 </tr>
 <tr>
  <td>Voix, entrée manuscrite et saisie</td>
   <td>ms-settings:privacy-speechtyping</td>
   <td></td>
 </tr>
 <tr>
  <td>Informations sur le compte</td>
   <td>ms-settings:privacy-accountinfo</td>
   <td></td>
 </tr>
 <tr>
  <td>Contacts</td>
   <td>ms-settings:privacy-contacts</td>
   <td></td>
 </tr>
 <tr>
  <td>Calendrier</td>
   <td>ms-settings:privacy-calendar</td>
   <td></td>
 </tr>
 <tr>
  <td>Historique des appels</td>
   <td>ms-settings:privacy-callhistory</td>
   <td></td>
 </tr>
 <tr>
  <td>Courrier électronique</td>
  <td>ms-settings:privacy-email</td>
  <td></td>
 </tr>
 <tr>
  <td>Messagerie</td>
    <td>ms-settings:privacy-messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>Radios</td>
    <td>ms-settings:privacy-radios</td>
  <td></td>
 </tr>
 <tr>
  <td>Applications en arrière-plan</td>
    <td>ms-settings:privacy-backgroundapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Autres appareils</td>
    <td>ms-settings:privacy-customdevices</td>
  <td></td>
 </tr>
 <tr>
  <td>Commentaires et diagnostics</td>
    <td>ms-settings:privacy-feedback</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Surface Hub</td>
  <td>Comptes</td>
    <td>ms-settings:surfacehub-accounts</td>
      <td></td>
  </tr>
  <tr>
    <td>Conférence d’équipe</td>
      <td>ms-settings:surfacehub-calling</td>
      <td></td>
  </tr>
  <tr>
    <td>Gestion des appareils de l’équipe</td>
      <td>ms-settings:surfacehub-devicemanagenent</td>
      <td></td>
  </tr>
  <tr>
    <td>Nettoyage de la session</td>
      <td>ms-settings:surfacehub-sessioncleanup</td>
      <td></td>
  </tr>
  <tr>
    <td>Écran d’accueil</td>
      <td>ms-settings:surfacehub-welcome</td>
      <td></td>
  </tr>
    <td rowspan="19">Système</td>
    <td>Expériences partagées</td>
      <td>ms-settings:crossdevice</td>
    <td></td>
 </tr>
 <tr>
  <td>Affichage</td>
    <td>ms-settings:display</td>
  <td></td>
 </tr>
 <tr>
  <td>Multitâches</td>
    <td>ms-settings:multitasking</td>
  <td></td>
 </tr>
 <tr>
  <td>Projection vers cePC</td>
    <td>ms-settings:project</td>
  <td></td>
 </tr>
 <tr>
  <td>Mode tablette</td>
    <td>ms-settings:tabletmode</td>
  <td></td>
 </tr>
 <tr>
  <td>Barre des tâches</td>
    <td>ms-settings:taskbar</td>
  <td></td>
 </tr>
 <tr>
  <td>Téléphone</td>
    <td>ms-settings:phone-defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Affichage</td>
    <td>ms-settings:screenrotation</td>
  <td></td>
 </tr>
 <tr>
  <td>Notifications et actions</td>
    <td>ms-settings:notifications</td>
  <td></td>
 </tr>
 <tr>
  <td>Téléphone</td>
    <td>ms-settings:phone</td>
  <td></td>
 </tr>
 <tr>
  <td>Messagerie</td>
    <td>ms-settings:messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>Économiseur de batterie</td>
  <td>ms-settings:batterysaver</td>
  <td>Disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette</td>
 </tr>
 <tr>
  <td>Utilisation de la batterie</td>
  <td>ms-settings:batterysaver-usagedetails</td>
  <td>Disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette</td>
 </tr>
 <tr>
  <td>Alimentation et mise en veille</td>
  <td>ms-settings:powersleep</td>
  <td></td>
 </tr>
 <tr>
  <td>À propos de</td>
    <td>ms-settings:about</td>
  <td></td>
 </tr>
 <tr>
  <td>Stockage</td>
    <td>ms-settings:storagesense</td>
  <td></td>
 </tr>
 <tr>
  <td>Assistant Stockage</td>
    <td>ms-settings:storagepolicies</td>
  <td></td>
 </tr>
 <tr>
  <td>Chiffrement</td>
    <td>ms-settings:deviceencryption</td>
  <td></td>
 </tr>
 <tr>
  <td>Cartes hors connexion</td>
    <td>ms-settings:maps</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Heure et langue</td>
  <td>Date et heure</td>
    <td>ms-settings:dateandtime</td>
  <td></td>
 </tr>
 <tr>
  <td>Région et langue</td>
    <td>ms-settings:regionlanguage</td>
  <td></td>
 </tr>
 <tr>
     <td>Langue vocale</td>
     <td>ms-settings:speech</td>
     <td></td>
 </tr>
 <tr>
     <td>Clavier Pinyin</td>
     <td>ms-settings:regionlanguage-chsime-pinyin</td>
     <td>Disponible si l’éditeur de méthode de saisie MicrosoftPinyin est installé.</td>
 </tr>
 <tr>
     <td>Mode de saisie Wubi</td>
     <td>ms-settings:regionlanguage-chsime-wubi</td>
     <td>Disponible si l’éditeur de méthode de saisie MicrosoftWubi est installé.</td>
 </tr>
 <tr>
  <td rowspan="13">Mise à jour et sécurité</td>
  <td>Configuration de Windows Hello.</td>
    <td>ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment</td>
  </tr>
  <tr>
    <td>Sauvegarde</td>
      <td>ms-settings:backup</td>
    <td></td>
 </tr>
 <tr>
  <td>Localiser mon appareil</td>
    <td>ms-settings:findmydevice</td>
  <td></td>
 </tr>
 <tr>
  <td>Programme WindowsInsider</td>
  <td>ms-settings:windowsinsider</td>
  <td>Présent uniquement si l’utilisateur est inscrit dans TEC</td>
 </tr>
 <tr>
  <td>Windows Update</td>
  <td>ms-settings:windowsupdate</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-history</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-options</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-restartoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-action</td>
  <td></td>
 </tr>
 <tr>
  <td>Activation</td>
    <td>ms-settings:activation</td>
  <td></td>
 </tr>
 <tr>
  <td>Récupération</td>
    <td>ms-settings:recovery</td>
  <td></td>
 </tr>
 <tr>
  <td>Résoudre les problèmes</td>
    <td>ms-settings:troubleshoot</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Defender</td>
    <td>ms-settings:windowsdefender</td>
  <td></td>
 </tr>
 <tr>
  <td>Pour les développeurs</td>
    <td>ms-settings:developers</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="2">Comptes d’utilisateurs</td>
  <td>Windows Anywhere</td>
  <td>ms-settings:windowsanywhere</td>
  <td>L’appareil doit être compatible avec Windows Anywhere</td>
 </tr>
 <tr>
  <td>Approvisionnement</td>
  <td>ms-settings:workplace-provisioning</td>
  <td>Disponible uniquement si l’entreprise a déployé un package d’approvisionnement</td>
 </tr>
</table><br/>  
