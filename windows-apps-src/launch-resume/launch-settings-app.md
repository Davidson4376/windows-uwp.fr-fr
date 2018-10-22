---
author: TylerMSFT
title: Lancer l’application Paramètres Windows
description: Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 03/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 22727f8d09b3d68970301677cdf632a0981c616a
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5403393"
---
# <a name="launch-the-windows-settings-app"></a>Lancer l’application Paramètres Windows


**API importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Découvrez comment lancer l’application Paramètres Windows. Cette rubrique décrit le schéma d’URI **ms-settings:**. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.

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

Par ailleurs, votre application peut appeler la méthode [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) pour lancer l’application **Paramètres**. Cet exemple montre comment ouvrir la page des paramètres de confidentialité relatifs à l’appareil photo en utilisant l’URI `ms-settings:privacy-webcam`.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Le code ci-dessus ouvre la page des paramètres de confidentialité relatifs à l’appareil photo:

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations sur les URI de lancement, voir [Lancer l’application par défaut pour un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Référence de schéma d’URI ms-settings:

Utilisez les URI suivants pour ouvrir diverses pages de l’application Paramètres.

> Notez que la disponibilité d’une page de paramètres varie selon la référence Windows. Toutes les pages de paramètres disponibles sur Windows10 pour éditions de bureau ne sont pas disponibles sur Windows10Mobile et inversement. La colonne Remarques capture également des exigences supplémentaires qui doivent être remplies pour qu’une page soit disponible.

## <a name="accounts"></a>Comptes

|Page Paramètres| URI |
|-------------|-----|
| Accéder à un compte professionnel ou scolaire | ms-settings:workplace |
| Comptes de messagerie et d’application  | ms-settings:emailandaccounts |
| Famille et autres personnes | ms-settings:otherusers |
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
| Applications pardéfaut | ms-settings:defaultapps |
| Gérer les fonctionnalités optionnelles | ms-settings:optionalfeatures |
| Cartes hors connexion | ms-settings:maps |
| Applications de démarrage | ms-settings:startupapps |
| Lecture vidéo | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|Page Paramètres| URI |
|-------------|-----|
| Autorisations et historique | ms-settings:cortana-permissions |
| En savoir plus | ms-settings:cortana-moredetails |
| Cortana sur mes appareils | ms-settings:cortana-notifications |
| Parler à Cortana | ms-settings:cortana-language |

> [!NOTE] 
> Cette section de paramètres sur le bureau sera appelée recherche lorsque le PC est défini sur les régions où Cortana n’est pas actuellement disponible ou Cortana a été désactivé. Des pages spécifiques de Cortana (Cortana sur mes appareils) et parler à Cortana ne seront pas indiquées dans ce cas. 

## <a name="devices"></a>Appareils

|Page Paramètres| URI |
|-------------|-----|
| Fonctions audio et vocales | ms-settings:holographic-audio (disponible uniquement si l’application Portail de réalité mixte est installée [disponible dans le Microsoft Store]) |
| Lecture automatique | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| Appareils connectés | ms-settings:connecteddevices |
| Appareil photo par défaut | ms-settings:camera |
| Souris et pavé tactile | ms-settings:mousetouchpad (paramètres du pavé tactile disponibles uniquement sur les appareils dotés d’un pavé tactile) |
| Stylet et WindowsInk | ms-settings:pen |
| Imprimantes et scanneurs | ms-settings:printers |
| Pavé tactile | ms-settings:devices-touchpad (nécessite du matériel de pavé tactile) |
| Saisie | ms-settings:typing |
| USB | ms-settings:usb |
| Molette | ms-settings:wheel (disponible uniquement si Dial est jumelé) |
| Votre téléphone | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>Options d’ergonomie

|Page Paramètres| URI |
|-------------|-----|
| Audio | ms-settings:easeofaccess-audio |
| Sous-titres codés | ms-settings:easeofaccess-closedcaptioning |
| Écran | ms-settings:easeofaccess-display |
| Contrôle visuel | ms-settings:easeofaccess-eyecontrol |
| Polices | ms-settings:fonts |
| Contraste élevé | ms-settings:easeofaccess-highcontrast |
| Casque holographique | ms-settings:holographic-headset (nécessite du matériel holographique) |
| Clavier | ms-settings:easeofaccess-keyboard |
| Loupe | ms-settings:easeofaccess-magnifier |
| Souris | ms-settings:easeofaccess-mouse |
| Narrateur | ms-settings:easeofaccess-narrator |
| Autres options | ms-settings:easeofaccess-otheroptions |
| Voix | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>Bonus

|Page Paramètres| URI |
|-------------|-----|
| Bonus | ms-settings:extras (disponible uniquement si les «applications de paramètres» sont installées, par exemple par un tiers) |

## <a name="gaming"></a>Jeux

|Page Paramètres| URI |
|-------------|-----|
| Diffusion | ms-settings:gaming-broadcasting |
| Barre de jeux | ms-settings:gaming-gamebar |
| Jeux DVR | ms-settings:gaming-gamedvr |
| Mode jeu | ms-settings:gaming-gamemode |
| Lecture d’un jeu en plein écran | ms-settings:quietmomentsgame |
| TruePlay | ms-settings:gaming-trueplay |
| Xbox Networking | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>Page d’accueil

|Page Paramètres| URI |
|-------------|-----|
| Page d'accueil Paramètres | ms-settings: |


## <a name="network--internet"></a>Réseau et Internet

|Page Paramètres| URI |
|-------------|-----|
| Mode avion | ms-settings:network-airplanemode (utilisez ms-settings:proximity sur Windows8.x) |
| Réseau cellulaire et SIM | ms-settings:network-cellular |
| Consommation des données | ms-settings:datausage |
| Dial-up | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess (disponible uniquement si DirectAccess est activé) |
| Ethernet | ms-settings:network-ethernet |
| Gérer les réseaux connus | ms-settings:network-wifisettings |
| Point d’accès sans fil mobile | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| État | ms-settings:network-status |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (disponible uniquement si l’appareil dispose d’une carte Wi-Fi) |
| Appels Wi-Fi | ms-settings:network-wificalling (disponible uniquement si les appels Wi-Fi sont activés) |

## <a name="personalization"></a>Personnalisation

|Page Paramètres| URI |
|-------------|-----|
| Arrière-plan | ms-settings:personalization-background |
| Choisir les dossiers qui s’affichent dans le menu Démarrer | ms-settings:personalization-start-places |
| Couleurs | ms-settings:personalization-colors |
| Coup d’œil | ms-settings:personalization-glance |
| Écran de verrouillage | ms-settings:lockscreen |
| Barre de navigation | ms-settings:personalization-navbar |
| Personnalisation (catégorie) | ms-settings:personalization |
| Démarrer | ms-settings:personalization-start |
| Barre des tâches | ms-settings:taskbar |
| Thèmes | ms-settings:themes |

## <a name="phone"></a>Téléphone

|Page Paramètres| URI |
|-------------|-----|
| Votre téléphone | ms-settings:mobile-devices  |

## <a name="privacy"></a>Confidentialité

|Page Paramètres| URI |
|-------------|-----|
| Applications pour accessoires | ms-settings:privacy-accessoryapps |
| Informations sur le compte | ms-settings:privacy-accountinfo |
| Historique d'activités | ms-settings:privacy-activityhistory |
| Identifiant de publicité | ms-settings:privacy-advertisingid |
| Diagnostics des applications | ms-settings:privacy-appdiagnostics |
| Téléchargements de fichiers automatiques | ms-settings:privacy-automaticfiledownloads |
| Applications en arrière-plan | ms-settings:privacy-backgroundapps |
| Calendrier | ms-settings:privacy-calendar |
| Historique des appels | ms-settings:privacy-callhistory |
| Caméra | ms-settings:privacy-webcam |
| Contacts | ms-settings:privacy-contacts |
| Documents | ms-settings:privacy-documents |
| E-mail | ms-settings:privacy-email |
| Dispositif de suivi oculaire | ms-settings:privacy-eyetracker (nécessite du matériel de suivi oculaire) |
| Commentaires et diagnostics | ms-settings:privacy-feedback |
| Système de fichiers | ms-settings:privacy-broadfilesystemaccess |
| Généralités | ms-settings:privacy-general |
| Services de localisation | ms-settings:privacy-location |
| Messages | ms-settings:privacy-messaging |
| Micro | ms-settings:privacy-microphone |
| Mouvement | ms-settings:privacy-motion |
| Notifications | ms-settings:privacy-notifications |
| Autres appareils | ms-settings:privacy-customdevices |
| Images | ms-settings:privacy-pictures |
| Appels téléphoniques | ms-settings:privacy-phonecall |
| Radios | ms-settings:privacy-radios |
| Voix, entrée manuscrite et saisie |ms-settings:privacy-speechtyping |
| Tâches | ms-settings:privacy-tasks |
| Vidéos | ms-settings:privacy-videos |

## <a name="surface-hub"></a>Surface Hub

|Page Paramètres| URI |
|-------------|-----|
| Comptes | ms-settings:surfacehub-accounts |
| Nettoyage de la session | ms-settings:surfacehub-sessioncleanup |
| Conférence d’équipe | ms-settings:surfacehub-calling |
| Gestion des appareils de l’équipe | ms-settings:surfacehub-devicemanagenent |
| Écran de démarrage | ms-settings:surfacehub-welcome |

## <a name="system"></a>Système

|Page Paramètres| URI |
|-------------|-----|
| À propos de | ms-settings:about |
| Paramètres d’affichage avancés | ms-settings:display-advanced (disponible uniquement sur les appareils qui prennent en charge les options d’affichage avancées) |
| Économiseur de batterie | ms-settings:batterysaver (disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette) |
| Paramètres d'économiseur de batterie | ms-settings:batterysaver-settings (disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette) |
| Utilisation de la batterie | ms-settings:batterysaver-usagedetails (disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette) |
| Écran | ms-settings:display |
| Localisations de sauvegarde par défaut | ms-settings:savelocations |
| Écran | ms-settings:screenrotation |
| Duplication de mon écran | ms-settings:quietmomentspresentation |
| Pendant ces heures | ms-settings:quietmomentsscheduled |
| Chiffrement | ms-settings:deviceencryption |
| Assistant Focus | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| Paramètres des graphiques | ms-settings:display-advancedgraphics (disponible uniquement sur les appareils qui prennent en charge les options graphiques avancées) |
| Messages | ms-settings:messaging |
| Multitâches | ms-settings:multitasking |
| Paramètres d'éclairage nocturne | ms-settings:nightlight |
| Téléphone | ms-settings:phone-defaultapps |
| Projection vers cePC | ms-settings:project |
| Expériences partagées | ms-settings:crossdevice |
| Mode tablette | ms-settings:tabletmode |
| Barre des tâches | ms-settings:taskbar |
| Notifications et actions | ms-settings:notifications |
| Bureau à distance | ms-settings:remotedesktop |
| Téléphone | ms-settings:phone |
| Alimentation et mise en veille | ms-settings:powersleep |
| Sons | ms-settings:sounds |
| Stockage | ms-settings:storagesense |
| Assistant Stockage | ms-settings:storagepolicies |

## <a name="time-and-language"></a>Heure et langue

|Page Paramètres| URI |
|-------------|-----|
| Date et heure | ms-settings:dateandtime |
| Paramètres IME Japon | ms-settings:regionlanguage-jpnime (disponible si l’éditeur de méthode de saisie MicrosoftJapon est installé) |
| Paramètres IME Pinyin | ms-settings:regionlanguage-chsime-pinyin (disponible si l’éditeur de méthode de saisie MicrosoftPinyin est installé) |
| Région et langue | ms-settings:regionlanguage |
| Langue vocale | ms-settings:speech |
| Paramètres IME Wubi  | ms-settings:regionlanguage-chsime-wubi (disponible si l’éditeur de méthode de saisie MicrosoftWubi est installé) |

## <a name="update--security"></a>Mise à jour et sécurité

|Page Paramètres| URI |
|-------------|-----|
| Activation | ms-settings:activation |
| Sauvegarde | ms-settings:backup |
| Optimisation de la distribution | ms-settings:delivery-optimization |
| Localiser mon appareil | ms-settings:findmydevice |
| Pour les développeurs | ms-settings:developers |
| Récupération | ms-settings:recovery |
| Résoudre les problèmes | ms-settings:troubleshoot |
| Sécurité de Windows | ms-settings:windowsdefender |
| Programme WindowsInsider | ms-settings:windowsinsider (présent uniquement si l’utilisateur est inscrit dans TEC) |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| WindowsUpdate - Options avancées | ms-settings:windowsupdate-options |
| WindowsUpdate - Options de redémarrage | ms-settings:windowsupdate-restartoptions |
| Windows Update - Afficher l'historique des mises à jour | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>Comptes d’utilisateurs

|Page Paramètres| URI |
|-------------|-----|
| Approvisionnement | ms-settings:workplace-provisioning (disponible uniquement si l’entreprise a déployé un package d’approvisionnement) |
| Approvisionnement | ms-settings:provisioning (disponible uniquement sur mobile et si l’entreprise a déployé un package d’approvisionnement) |
| Windows Anywhere | ms-settings:windowsanywhere (l’appareil doit être compatible avec Windows Anywhere) |
