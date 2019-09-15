---
title: Lancer l’application Paramètres Windows
description: Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4f44772a9e8b34bf7f19a3b14dc8efd3d16c792f
ms.sourcegitcommit: e5ed95f8252ddc7f39055d8f7276e82167bb9891
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70973711"
---
# <a name="launch-the-windows-settings-app"></a>Lancer l’application Paramètres Windows

**API importantes**

-   [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview)

Découvrez comment lancer l’application Paramètres Windows. Cette rubrique décrit les **paramètres ms :** Schéma d’URI. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.

Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Pour plus d’informations, voir [Recommandations en matière d’applications prenant en charge la confidentialité](https://docs.microsoft.com/windows/uwp/security/index).

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

Par ailleurs, votre application peut appeler la méthode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) pour lancer l’application **Paramètres**. Cet exemple montre comment ouvrir la page des paramètres de confidentialité relatifs à l’appareil photo en utilisant l’URI `ms-settings:privacy-webcam`.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Le code ci-dessus ouvre la page des paramètres de confidentialité relatifs à l’appareil photo :

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations sur les URI de lancement, voir [Lancer l’application par défaut pour un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: Référence du schéma d’URI

Utilisez les URI suivants pour ouvrir diverses pages de l’application Paramètres.

> Notez que la disponibilité d’une page de paramètres varie selon la référence Windows. Toutes les pages de paramètres disponibles sur Windows 10 pour éditions de bureau ne sont pas disponibles sur Windows 10 Mobile et inversement. La colonne Remarques capture également des exigences supplémentaires qui doivent être remplies pour qu’une page soit disponible.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>Comptes

|Page Paramètres| URI |
|-------------|-----|
| Accéder à un compte professionnel ou scolaire | ms-settings:workplace |
| Adresse de messagerie et comptes  | ms-settings:emailandaccounts |
| Famille et autres personnes | ms-settings:otherusers |
| Configurer une borne | MS-paramètres : assignedaccess |
| Options de connexion | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| Synchroniser vos paramètres | ms-settings:sync |
| Configuration de Windows Hello | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| Vos informations | ms-settings:yourinfo |

## <a name="apps"></a>Applications

|Page Paramètres| URI |
|-------------|-----|
| Applications et fonctionnalités | ms-settings:appsfeatures |
| Fonctionnalités de l’application | ms-settings:appsfeatures-app (réinitialiser, gérer l'extension et le contenu téléchargeable, etc. pour l’application)|
| Applications pour les sites web | ms-settings:appsforwebsites |
| Applications par défaut | ms-settings:defaultapps |
| Gérer les fonctionnalités optionnelles | ms-settings:optionalfeatures |
| Cartes hors connexion | ms-settings:maps<br/>MS-Settings : Maps-downloadmaps (télécharger les mappages) |
| Applications de démarrage | ms-settings:startupapps |
| Lecture vidéo | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|Page Paramètres| URI |
|-------------|-----|
| Cortana sur mes appareils | ms-settings:cortana-notifications |
| En savoir plus | ms-settings:cortana-moredetails |
| Historique des autorisations & | ms-settings:cortana-permissions |
| Recherche de fenêtres | MS-paramètres : Cortana-windowssearch |
| Parler à Cortana | ms-settings:cortana-language<br/>MS-paramètres : Cortana<br/>MS-paramètres : Cortana-talktocortana |

> [!NOTE] 
> Cette section de paramètres sur le bureau sera appelée Rechercher lorsque le PC est défini sur des régions où Cortana n’est pas actuellement disponible ou que Cortana a été désactivée. Les pages spécifiques à Cortana (Cortana sur mes appareils et communiquer avec Cortana) ne sont pas listées dans ce cas. 

## <a name="devices"></a>Appareils

|Page Paramètres| URI |
|-------------|-----|
| AutoPlay | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| Appareils connectés | ms-settings:connecteddevices |
| Appareil photo par défaut | MS-Settings : appareil photo (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Souris et pavé tactile | ms-settings:mousetouchpad (paramètres du pavé tactile disponibles uniquement sur les appareils dotés d’un pavé tactile) |
| Stylet et Windows Ink | ms-settings:pen |
| Imprimantes et scanneurs | ms-settings:printers |
| Pavé tactile | ms-settings:devices-touchpad (nécessite du matériel de pavé tactile) |
| Saisie | ms-settings:typing |
| USB | ms-settings:usb |
| Roulette | ms-settings:wheel (disponible uniquement si Dial est jumelé) |
| Votre téléphone | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>Options d’ergonomie

|Page Paramètres| URI |
|-------------|-----|
| Audio | ms-settings:easeofaccess-audio |
| Sous-titres | ms-settings:easeofaccess-closedcaptioning |
| Filtres de couleur | MS-Settings : easeofaccess-colorfilter |
| Taille du pointeur & curseur | MS-Settings : easeofaccess-cursorandpointersize |
| Écran | ms-settings:easeofaccess-display |
| Contrôle visuel | ms-settings:easeofaccess-eyecontrol |
| Polices | ms-settings:fonts |
| Contraste élevé | ms-settings:easeofaccess-highcontrast |
| Clavier | ms-settings:easeofaccess-keyboard |
| Loupe | ms-settings:easeofaccess-magnifier |
| Souris | ms-settings:easeofaccess-mouse |
| Narrator | ms-settings:easeofaccess-narrator |
| Autres options | MS-Settings : easeofaccess-otheroptions (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Fonctions vocales | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>Bonus

|Page Paramètres| URI |
|-------------|-----|
| Bonus | ms-settings:extras (disponible uniquement si les « applications de paramètres » sont installées, par exemple par un tiers) |

## <a name="gaming"></a>Jeux

|Page Paramètres| URI |
|-------------|-----|
| Diffusion | ms-settings:gaming-broadcasting |
| Barre de jeux | ms-settings:gaming-gamebar |
| Jeux DVR | ms-settings:gaming-gamedvr |
| Mode jeu | ms-settings:gaming-gamemode |
| Lecture d’un jeu en plein écran | ms-settings:quietmomentsgame |
| TruePlay | MS-Settings : jeu-trueplay (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Xbox Networking | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>page d'accueil

|Page Paramètres| URI |
|-------------|-----|
| Page d'accueil Paramètres | ms-settings: |

## <a name="mixed-reality"></a>Réalité mixte

> [!NOTE]
> Ces paramètres sont disponibles uniquement si l’application portail de réalité mixte est installée.

| Page Paramètres | URI |
|---------------|-----|
| Fonctions audio et vocales | ms-settings:holographic-audio |
| Environnement | MS-Settings : confidentialité-holographique-environnement |
| Affichage du casque | MS-Settings : holographique-casque |
| Désinstaller l’interface | MS-Settings : holographique-Management |

## <a name="network--internet"></a>Réseau et Internet

|Page Paramètres| URI |
|-------------|-----|
| Mode avion | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Réseau cellulaire et SIM | ms-settings:network-cellular |
| Utilisation des données | ms-settings:datausage |
| Dial-up | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess (disponible uniquement si DirectAccess est activé) |
| Ethernet | ms-settings:network-ethernet |
| Gérer les réseaux connus | ms-settings:network-wifisettings |
| Point d’accès sans fil mobile | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| Statut | ms-settings:network-status<br/>MS-Settings : réseau |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (disponible uniquement si l’appareil dispose d’une carte Wi-Fi) |
| Appels Wi-Fi | ms-settings:network-wificalling (disponible uniquement si les appels Wi-Fi sont activés) |

## <a name="personalization"></a>Personnalisation

|Page Paramètres| URI |
|-------------|-----|
| Présentation | ms-settings:personalization-background |
| Choisir les dossiers qui s’affichent dans le menu Démarrer | ms-settings:personalization-start-places |
| Couleurs | ms-settings:personalization-colors<br/>MS-paramètres : couleurs |
| Coup d’œil | MS-Settings : personnalisation-aperçu (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Écran de verrouillage | ms-settings:lockscreen |
| Barre de navigation | MS-Settings : personnalisation-barre**de navigation (déconseillée dans Windows 10, version 1809 et versions ultérieures**) |
| Personnalisation (catégorie) | ms-settings:personalization |
| Start | ms-settings:personalization-start |
| Barre des tâches | ms-settings:taskbar |
| Thèmes | ms-settings:themes |

## <a name="phone"></a>Phone

|Page Paramètres| URI |
|-------------|-----|
| Votre téléphone | ms-settings:mobile-devices<br/>MS-Settings : Mobile-Devices-addphone<br/>MS-Settings : Mobile-Devices-addphone-direct (ouvre **votre application téléphonique** ) |

## <a name="privacy"></a>Confidentialité

|Page Paramètres| URI |
|-------------|-----|
| Applications pour accessoires | MS-Settings : privacy-accessoryapps (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Informations sur le compte | ms-settings:privacy-accountinfo |
| Historique d'activités | ms-settings:privacy-activityhistory |
| Identifiant de publicité | MS-Settings : privacy-advertisingid (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Diagnostics d'application | ms-settings:privacy-appdiagnostics |
| Téléchargements de fichiers automatiques | ms-settings:privacy-automaticfiledownloads |
| Applications en arrière-plan | ms-settings:privacy-backgroundapps |
| Calendrier | ms-settings:privacy-calendar |
| Historique des appels | ms-settings:privacy-callhistory |
| Appareil photo | ms-settings:privacy-webcam |
| Contacts | ms-settings:privacy-contacts |
| Documents | ms-settings:privacy-documents |
| Email | ms-settings:privacy-email |
| Dispositif de suivi oculaire | ms-settings:privacy-eyetracker (nécessite du matériel de suivi oculaire) |
| Commentaires et diagnostics | ms-settings:privacy-feedback |
| Système de fichiers | ms-settings:privacy-broadfilesystemaccess |
| Généralités | ms-settings:privacy-general |
| Location | ms-settings:privacy-location |
| Messagerie | ms-settings:privacy-messaging |
| Microphone | ms-settings:privacy-microphone |
| Mouvement | ms-settings:privacy-motion |
| Notifications | ms-settings:privacy-notifications |
| Autres appareils | ms-settings:privacy-customdevices |
| Images | ms-settings:privacy-pictures |
| Appels téléphoniques | MS-Settings : confidentialité-phonecalls |
| Radios | ms-settings:privacy-radios |
| Voix, entrée manuscrite et saisie |ms-settings:privacy-speechtyping |
| Tâches | ms-settings:privacy-tasks |
| Vidéos | ms-settings:privacy-videos |
| Activation vocale | MS-Settings : confidentialité-voiceactivation |

## <a name="surface-hub"></a>Surface Hub

|Page Paramètres| URI |
|-------------|-----|
| Comptes | ms-settings:surfacehub-accounts |
| Nettoyage de la session | ms-settings:surfacehub-sessioncleanup |
| Conférence d’équipe | ms-settings:surfacehub-calling |
| Gestion des appareils de l’équipe | ms-settings:surfacehub-devicemanagenent |
| Écran de démarrage | ms-settings:surfacehub-welcome |

## <a name="system"></a>System

|Page Paramètres| URI |
|-------------|-----|
| À propos de | ms-settings:about |
| Paramètres d’affichage avancés | ms-settings:display-advanced (disponible uniquement sur les appareils qui prennent en charge les options d’affichage avancées) |
| Paramètres du volume et de l’appareil de l’application | MS-Settings : apps-volume (**ajouté dans Windows 10, version 1903**)|
| Économiseur de batterie | ms-settings:batterysaver (disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette) |
| Paramètres d'économiseur de batterie | ms-settings:batterysaver-settings (disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette) |
| Utilisation de la batterie | ms-settings:batterysaver-usagedetails (disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette) |
| Presse-papiers | MS-Settings : presse-papiers |
| Écran | ms-settings:display |
| Localisations de sauvegarde par défaut | ms-settings:savelocations |
| Écran | ms-settings:screenrotation |
| Duplication de mon écran | ms-settings:quietmomentspresentation |
| Pendant ces heures | ms-settings:quietmomentsscheduled |
| Chiffrement | ms-settings:deviceencryption |
| Assistant Focus | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| Paramètres des graphiques | ms-settings:display-advancedgraphics (disponible uniquement sur les appareils qui prennent en charge les options graphiques avancées) |
| Messagerie | ms-settings:messaging |
| Multitâches | ms-settings:multitasking |
| Paramètres d'éclairage nocturne | ms-settings:nightlight |
| Phone | ms-settings:phone-defaultapps |
| Projection vers ce PC | ms-settings:project |
| Expériences partagées | ms-settings:crossdevice |
| Mode tablette | ms-settings:tabletmode |
| Barre des tâches | ms-settings:taskbar |
| Notifications et actions | ms-settings:notifications |
| Bureau à distance | ms-settings:remotedesktop |
| Phone | MS-Settings : téléphone (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Alimentation et mise en veille | ms-settings:powersleep |
| Son | MS-Settings : son |
| Stockage | ms-settings:storagesense |
| Assistant Stockage | ms-settings:storagepolicies |

## <a name="time-and-language"></a>Heure et langue

|Page Paramètres| URI |
|-------------|-----|
| Date et heure | ms-settings:dateandtime |
| Paramètres IME Japon | ms-settings:regionlanguage-jpnime (disponible si l’éditeur de méthode de saisie Microsoft Japon est installé) |
| Langue | MS-Settings : clavier<br/>ms-settings:regionlanguage<br/>MS-Settings : regionlanguage-bpmfime<br/>MS-Settings : regionlanguage-cangjieime<br/>MS-Settings : regionlanguage-chsime-pinyin-domainlexicon<br/>MS-Settings : regionlanguage-chsime-pinyin-keyconfig<br/>MS-Settings : regionlanguage-chsime-pinyin-UDP<br/>MS-Settings : regionlanguage-chsime-Wubi-UDP<br/>MS-Settings : regionlanguage-quickime |
| Paramètres IME Pinyin | ms-settings:regionlanguage-chsime-pinyin (disponible si l’éditeur de méthode de saisie Microsoft Pinyin est installé) |
| Fonctions vocales | ms-settings:speech |
| Paramètres IME Wubi  | ms-settings:regionlanguage-chsime-wubi (disponible si l’éditeur de méthode de saisie Microsoft Wubi est installé) |

## <a name="update--security"></a>Mise à jour et sécurité

|Page Paramètres| URI |
|-------------|-----|
| Activation | ms-settings:activation |
| Sauvegarde | ms-settings:backup |
| Optimisation de la distribution | ms-settings:delivery-optimization |
| Localiser mon appareil | ms-settings:findmydevice |
| Pour les développeurs | ms-settings:developers |
| Récupération | ms-settings:recovery |
| Résolution des problèmes | ms-settings:troubleshoot |
| Sécurité Windows | ms-settings:windowsdefender |
| Programme Windows Insider | ms-settings:windowsinsider (présent uniquement si l’utilisateur est inscrit dans TEC)<br/>MS-paramètres : windowsinsider-optin |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows Update - Options avancées | ms-settings:windowsupdate-options |
| Windows Update - Options de redémarrage | ms-settings:windowsupdate-restartoptions |
| Windows Update - Afficher l'historique des mises à jour | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>Comptes d’utilisateurs

|Page Paramètres| URI |
|-------------|-----|
| Approvisionnement | ms-settings:workplace-provisioning (disponible uniquement si l’entreprise a déployé un package d’approvisionnement) |
| Approvisionnement | ms-settings:provisioning (disponible uniquement sur mobile et si l’entreprise a déployé un package d’approvisionnement) |
| Windows Anywhere | ms-settings:windowsanywhere (l’appareil doit être compatible avec Windows Anywhere) |
