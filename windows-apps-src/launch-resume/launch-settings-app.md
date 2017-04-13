---
author: TylerMSFT
title: "Lancer l’application Paramètres Windows"
description: "Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: e9d66feb6117fddfff62c217b55da813a63c4331
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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

Utilisez les URI suivants pour ouvrir diverses pages de l’application Paramètres. Notez que la colonne des références prises en charge signale l’existence ou non de la page de paramètres dans les versions de Windows 10 pour éditions de bureau (Famille, Professionnel, Entreprise et Éducation) et/ou dans Windows 10 Mobile.

<table border="1">
    <tr>
        <th>Catégorie</th>
        <th>Page de paramètres</th>
        <th>Références prises en charge</th>
        <th>URI</th>
    </tr>
    <tr>
        <td>Page d’accueil</td>
        <td>Page d’accueil des paramètres</td>
        <td>Les deux</td>
        <td>ms-settings:</td>
    </tr>
    <tr>
        <td rowspan="10">Système</td>
        <td>Affichage</td>
        <td>Les deux</td>
        <td>ms-settings:screenrotation</td>
    </tr>
    <tr>
        <td>Notifications et actions</td>
        <td>Les deux</td>
        <td>ms-settings:notifications</td>
    </tr>
    <tr>
        <td>Téléphone</td>
        <td>Mobile uniquement</td>
        <td>ms-settings:phone</td>
    </tr>
    <tr>
        <td>Messagerie</td>
        <td>Mobile uniquement</td>
        <td>ms-settings:messaging</td>
    </tr>
    <tr>
        <td>Économiseur de batterie</td>
        <td>Les deux<br>Disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette</td>
        <td>ms-settings:batterysaver</td>
    </tr>
    <tr>
        <td>Utilisation de la batterie</td>
        <td>Les deux<br>Disponible uniquement sur les appareils dotés d’une batterie, tels qu’une tablette</td>
        <td>ms-settings:batterysaver-usagedetails</td>
    </tr>
    <tr>
        <td>Alimentation et mise en veille</td>
        <td>Bureau uniquement</td>
        <td>ms-settings:powersleep</td>
    </tr>
    <tr>
        <td>À propos</td>
        <td>Les deux</td>
        <td>ms-settings:about</td>
    </tr>
    <tr>
        <td>Chiffrement</td>
        <td>Les deux</td>
        <td>ms-settings:deviceencryption</td>
    </tr>
    <tr>
        <td>Cartes hors connexion</td>
        <td>Les deux</td>
        <td>ms-settings:maps</td>
    </tr>
    <tr>
        <td rowspan="4">Appareils</td>
        <td>Appareil photo par défaut</td>
        <td>Mobile uniquement</td>
        <td>ms-settings:camera</td>
    </tr>
    <tr>
        <td>Bluetooth</td>
        <td>Bureau uniquement</td>
        <td>ms-settings:bluetooth</td>
    </tr>
    <tr>
        <td>Appareils connectés</td>
        <td>Poste de travail uniquement</td>
        <td>ms-settings:connecteddevices</td>
    </tr>
    <tr>
        <td>Souris et pavé tactile</td>
        <td>Les deux<br>Paramètres du pavé tactile disponibles uniquement sur les appareils dotés d’un pavé tactile</td>
        <td>ms-settings:mousetouchpad</td>
    </tr>
    <tr>
        <td rowspan="3">Réseau et sans fil</td>
        <td>NFC</td>
        <td>Les deux</td>
        <td>ms-settings:nfctransactions</td>
    </tr>
    <tr>
        <td>Wi-Fi</td>
        <td>Les deux</td>
        <td>ms-settings:network-wifi</td>
    </tr>
    <tr>
        <td>Mode avion</td>
        <td>Les deux</td>
        <td>ms-settings:network-airplanemode</td>
    </tr>
    <tr>
        <td rowspan="5">Réseau et Internet</td>
        <td>Consommation des données</td>
        <td>Les deux</td>
        <td>ms-settings:datausage</td>
    </tr>
    <tr>
        <td>Réseau cellulaire et SIM</td>
        <td>Les deux</td>
        <td>ms-settings:network-cellular</td>
    </tr>
    <tr>
        <td>Point d’accès sans fil mobile</td>
        <td>Les deux</td>
        <td>ms-settings:network-mobilehotspot</td>
    </tr>
    <tr>
        <td>Proxy</td>
        <td>Poste de travail uniquement</td>
        <td>ms-settings:network-proxy</td>
    </tr>
    <tr>
        <td>État</td>
        <td>Bureau uniquement</td>
        <td>ms-settings:network-status</td>
    </tr>
    <tr>
        <td rowspan="5">Personnalisation</td>
        <td>Personnalisation (catégorie)</td>
        <td>Les deux</td>
        <td>ms-settings:personalization</td>
    </tr>
    <tr>
        <td>Arrière-plan</td>
        <td>Bureau uniquement</td>
        <td>ms-settings:personalization-background</td>
    </tr>
    <tr>
        <td>Couleurs</td>
        <td>Les deux</td>
        <td>ms-settings:personalization-colors</td>
    </tr>
    <tr>
        <td>Sons</td>
        <td>Mobile uniquement </td>
        <td>ms-settings:sounds</td>
    </tr>
    <tr>
        <td>Écran de verrouillage</td>
        <td>Les deux</td>
        <td>ms-settings:lockscreen</td>
    </tr>
    <tr>
        <td rowspan="7">Comptes</td>
        <td>Accéder à un compte professionnel ou scolaire</td>
        <td>Les deux</td>
        <td>ms-settings:workplace</td>
    </tr>
    <tr>
        <td>Adresse électronique et comptes</td>
        <td>Les deux</td>
        <td>ms-settings:emailandaccounts</td>
    </tr>
    <tr>
        <td>Famille et autres personnes</td>
        <td>Les deux</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>Options de connexion</td>
        <td>Les deux</td>
        <td>ms-settings:signinoptions</td>
    </tr>
    <tr>
        <td>Synchroniser vos paramètres</td>
        <td>Les deux</td>
        <td>ms-settings:sync</td>
    </tr>
    <tr>
        <td>Autres personnes</td>
        <td>Les deux</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>Vos informations</td>
        <td>Les deux</td>
        <td>ms-settings:yourinfo</td>
    </tr>
    <tr>
        <td rowspan="2">Heure et langue</td>
        <td>Date et heure</td>
        <td>Les deux</td>
        <td>ms-settings:dateandtime</td>
    </tr>
    <tr>
        <td>Region &amp; language</td>
        <td>Poste de travail uniquement</td>
        <td>ms-settings:regionlanguage</td>
    </tr>
    <tr>
        <td rowspan="7">Options d’ergonomie</td>
        <td>Narrateur</td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-narrator</td>
    </tr>
    <tr>
        <td>Loupe</td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-magnifier</td>
    </tr>
    <tr>
        <td>Contraste élevé </td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-highcontrast</td>
    </tr>
    <tr>
        <td>Sous-titres</td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-closedcaptioning</td>
    </tr>
    <tr>
        <td>Clavier</td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-keyboard</td>
    </tr>
    <tr>
        <td>Souris</td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-mouse</td>
    </tr>
    <tr>
        <td>Autres options</td>
        <td>Les deux</td>
        <td>ms-settings:easeofaccess-otheroptions</td>
    </tr>
    <tr>
        <td rowspan="15">Confidentialité</td>
        <td>Emplacement</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-location</td>
    </tr>
    <tr>
        <td>Caméra</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-webcam</td>
    </tr>
    <tr>
        <td>Microphone</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-microphone</td>
    </tr>
    <tr>
        <td>Mouvement</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-motion</td>
    </tr>
    <tr>
        <td>Voix, entrée manuscrite et saisie</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-speechtyping</td>
    </tr>
    <tr>
        <td>Informations sur le compte</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-accountinfo</td>
    </tr>
    <tr>
        <td>Contacts</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-contacts</td>
    </tr>
    <tr>
        <td>Calendrier</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-calendar</td>
    </tr>
    <tr>
        <td>Historique des appels</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-callhistory</td>
    </tr>
    <tr>
        <td>Courrier électronique</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-email</td>
    </tr>
    <tr>
        <td>Messagerie</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-messaging</td>
    </tr>
    <tr>
        <td>Radios</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-radios</td>
    </tr>
    <tr>
        <td>Applications en arrière-plan</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-backgroundapps</td>
    </tr>
    <tr>
        <td>Autres appareils</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-customdevices</td>
    </tr>
    <tr>
        <td>Commentaires et diagnostics</td>
        <td>Les deux</td>
        <td>ms-settings:privacy-feedback</td>
    </tr>
    <tr>
        <td>Mise à jour et sécurité</td>
        <td>Pour les développeurs</td>
        <td>Les deux</td>
        <td>ms-settings:developers</td>
    </tr>
</table><br/>
