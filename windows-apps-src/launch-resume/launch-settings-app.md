---
author: mcleblanc
title: Lancer l’application Paramètres Windows
description: Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
---

# Lancer l’application Paramètres Windows


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI **ms-settings:**. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.

Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Pour plus d’informations, voir [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/hh768223).

## Comment lancer l’application Paramètres


Si les paramètres de confidentialité n’autorisent votre application à accéder à une ressource sensible, nous vous recommandons de fournir un lien pratique permettant d’accéder aux paramètres de confidentialité dans l’application **Paramètres**. Cela peut aider les utilisateurs à modifier leurs paramètres.

Pour lancer directement l’application **Paramètres**, utilisez le schéma d’URI `ms-settings:` comme illustré dans les exemples suivants.

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

Par ailleurs, votre application peut appeler la méthode[**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) pour lancer l’application **Paramètres** à partir du code.

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Cet exemple montre comment ouvrir la page des paramètres de confidentialité relatifs à l’appareil photo en utilisant l’URI `ms-settings:privacy-webcam`.

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations sur les URI de lancement, voir [Lancer l’application par défaut pour un URI](launch-default-app.md).

## Référence de schéma d’URI ms-settings:


Utilisez les URI suivants pour ouvrir diverses pages de l’application Paramètres. Notez que la colonne des références prises en charge signale l’existence ou non de la page de paramètres dans les versions de Windows 10 pour éditions de bureau (Famille, Professionnel, Entreprise et Éducation) et/ou dans Windows 10 Mobile.

| Catégorie           | Page de paramètres                          | Références prises en charge | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| Page d’accueil          | Page de destination des paramètres              | Les deux           | ms-settings:                              |
| Système             | Affichage                                | Les deux           | ms-settings:screenrotation                |
|                    | Notifications et actions                | Les deux           | ms-settings:notifications                 |
|                    | Téléphone                                  | Mobile uniquement    | ms-settings:phone                         |
|                    | Messagerie                              | Mobile uniquement    | ms-settings:messaging                     |
|                    | Économiseur de batterie                          | Les deux           | ms-settings:batterysaver                  |
|                    | Économiseur de batterie/paramètres d’économiseur de batterie | Les deux           | ms-settings:batterysaver-settings         |
|                    | Économiseur de batterie/utilisation de batterie            | Les deux           | ms-settings:batterysaver-usagedetails     |
|                    | Alimentation et mise en veille                          | Bureau uniquement   | ms-settings:powersleep                    |
|                    | Bureau : Informations système                         | Les deux           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | Mobile : Chiffrement de l’appareil              |                |                                           |
|                    | Cartes hors connexion                           | Les deux           | ms-settings:maps                          |
|                    | Informations système                                  | Les deux           | ms-settings:about                         |
| Périphériques            | Appareil photo par défaut                         | Mobile uniquement    | ms-settings:camera                        |
|                    | Bluetooth                              | Bureau uniquement   | ms-settings:bluetooth                     |
|                    | Souris et pavé tactile                       | Les deux           | ms-settings:mousetouchpad                 |
|                    | Communication en champ proche                                    | Les deux           | ms-settings:nfctransactions               |
| Réseau et sans fil | Wi-Fi                                  | Les deux           | ms-settings:network-wifi                  |
|                    | Mode avion                          | Les deux           | ms-settings:network-airplanemode          |
| Réseau et Internet | Consommation des données                             | Les deux           | ms-settings:datausage                     |
|                    | Réseau cellulaire et SIM                         | Les deux           | ms-settings:network-cellular              |
|                    | Point d’accès sans fil mobile                         | Les deux           | ms-settings:network-mobilehotspot         |
|                    | Proxy                                  | Les deux           | ms-settings:network-proxy                 |
| Personnalisation    | Personnalisation (catégorie)             | Les deux           | ms-settings:personalization               |
|                    | Arrière-plan                             | Bureau uniquement   | ms-settings:personalization-background    |
|                    | Couleurs                                 | Les deux           | ms-settings:personalization-colors        |
|                    | Sons                                 | Mobile uniquement    | ms-settings:sounds                        |
|                    | Écran de verrouillage                            | Les deux           | ms-settings:lockscreen                    |
| Comptes           | Adresse de messagerie et comptes                | Les deux           | ms-settings:emailandaccounts              |
|                    | Accès professionnel                            | Les deux           | ms-settings:workplace                     |
|                    | Synchroniser vos paramètres                     | Les deux           | ms-settings:sync                          |
| Heure et langue  | Date et heure                            | Les deux           | ms-settings:dateandtime                   |
|                    | Region & language                      | Poste de travail uniquement   | ms-settings:regionlanguage                |
| Options d’ergonomie     | Narrateur                               | Les deux           | ms-settings:easeofaccess-narrator         |
|                    | Loupe                              | Les deux           | ms-settings:easeofaccess-magnifier        |
|                    | Contraste élevé                          | Les deux           | ms-settings:easeofaccess-highcontrast     |
|                    | Sous-titres                        | Les deux           | ms-settings:easeofaccess-closedcaptioning |
|                    | Clavier                               | Les deux           | ms-settings:easeofaccess-keyboard         |
|                    | Souris                                  | Les deux           | ms-settings:easeofaccess-mouse            |
|                    | Autres options                          | Les deux           | ms-settings:easeofaccess-otheroptions     |
| Confidentialité            | Emplacement                               | Les deux           | ms-settings:privacy-location              |
|                    | Caméra                                 | Les deux           | ms-settings:privacy-webcam                |
|                    | Microphone                             | Les deux           | ms-settings:privacy-microphone            |
|                    | Mouvement                                 | Les deux           | ms-settings:privacy-motion                |
|                    | Voix, entrée manuscrite et saisie                | Les deux           | ms-settings:privacy-speechtyping          |
|                    | Informations sur le compte                           | Les deux           | ms-settings:privacy-accountinfo           |
|                    | Contacts                               | Les deux           | ms-settings:privacy-contacts              |
|                    | Calendrier                               | Les deux           | ms-settings:privacy-calendar              |
|                    | Historique des appels                           | Les deux           | ms-settings:privacy-callhistory           |
|                    | Courrier électronique                                  | Les deux           | ms-settings:privacy-email                 |
|                    | Messagerie                              | Les deux           | ms-settings:privacy-messaging             |
|                    | Radios                                 | Les deux           | ms-settings:privacy-radios                |
|                    | Applications en arrière-plan                        | Les deux           | ms-settings:privacy-backgroundapps        |
|                    | Autres appareils                          | Les deux           | ms-settings:privacy-customdevices         |
|                    | Commentaires et diagnostics                 | Les deux           | ms-settings:privacy-feedback              |
| Mise à jour et sécurité  | Pour les développeurs                         | Les deux           | ms-settings:developers                    |
 





<!--HONumber=May16_HO2-->


