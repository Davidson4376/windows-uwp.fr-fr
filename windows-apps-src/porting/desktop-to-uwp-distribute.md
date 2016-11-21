---
author: awkoren
Description: Distribuer votre application UWP convertie avec Desktop to UWP Bridge
Search.Product: eADQiWindows 10XVcnh
title: Distribuer votre application UWP convertie avec Desktop to UWP Bridge
translationtype: Human Translation
ms.sourcegitcommit: fe96945759739e9260d0cdfc501e3e59fb915b1e
ms.openlocfilehash: fbe8a77e3d5735b14088efb0ff6aa01be8d4badf

---

# Distribuer des applications converties avec Desktop Bridge

Pour déployer une application convertie, vous disposez de trois méthodes: le Windows Store, le chargement indépendant et l’inscription de fichiers libres.  

## Windows Store

Le Windows Store est la méthode la plus pratique pour rendre votre application accessible aux clients. Pour commencer, renseignez le formulaire dans [Transférez vos applications et vos jeux vers le WindowsStore avec Desktop Bridge](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft vous contactera pour démarrer le processus d’intégration. 

Notez que, pour transférer l’application ou le jeu vers le Windows Store, vous devez être son développeur et/ou son éditeur. En tant que tel, vous devez vous assurer que votre nom et votre adresse e-mail correspondent avec le site Web soumis dans l’URL ci-dessous. Cela nous permet de valider votre statut de développeur et/ou d’éditeur.

## Chargement indépendant

Le chargement indépendant simplifie le déploiement sur plusieurs ordinateurs. Il est particulièrement utile lorsque vous souhaitez contrôler de plus près la distribution des applications d’entreprise/métier sans avoir à vous inquiéter du certificat du Windows Store.

Avant de déployer votre application via un chargement indépendant, vous devez la signer avec un certificat. Pour plus d’informations sur la création d’un certificat, voir [Signature de votre package .Appx](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx). 

Voici comment vous importez un certificat que vous avez créé précédemment. Vous pouvez importer le certificat directement à l’aide de CERTUTIL, ou l’installer à partir d’un Appx que vous avez signé, comme le fera le client. 

Pour installer le certificat à l’aide de CERTUTIL, exécutez la commande suivante à partir d’une invite de commandes administrateur:

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

Pour importer le certificat à partir de l’Appx, comme le ferait un client:

1.  Dans l’Explorateur de fichiers, cliquez avec le bouton droit sur un Appx que vous avez signé avec un certificat de test, puis choisissez **Propriétés** dans le menu contextuel.
2.  Cliquez ou appuyez sur l’onglet **Signatures numériques**.
3.  Cliquez ou appuyez sur le certificat et choisissez **Détails**.
4.  Cliquez ou appuyez sur **Afficher le certificat**.
5.  Cliquez ou appuyez sur **Installer le certificat**.
6.  Dans le groupe **Emplacement de stockage**, sélectionnez **Ordinateur local**.
7.  Cliquez ou appuyez sur **Suivant** et sur **OK** pour confirmer la boîte de dialogue UAC.
8.  Dans l’écran suivant de l’Assistant Importation du certificat, remplacez l’option sélectionnée par **Placer tous les certificats dans le magasin suivant**.
9.  Cliquez ou appuyez sur **Parcourir**. Dans la fenêtre Sélectionner un magasin de certificats, faites défiler et sélectionnez **Personnes autorisées**, puis cliquez ou appuyez sur **OK**.
10. Cliquez ou appuyez sur **Suivant**. Un nouvel écran s’affiche. Cliquez ou appuyez sur **Terminer**.
11. Une boîte de dialogue de confirmation doit s’afficher. Si tel est le cas, cliquez sur **OK**. Si une autre boîte de dialogue indique que le certificat pose problème, vous devrez peut-être résoudre les problèmes liés au certificat.

Remarque: Pour que Windows approuve le certificat, ce dernier doit se trouver dans le nœud **Certificats (Ordinateur local)&gt; Autorités de certification racines de confiance&gt; Certificats** ou dans le nœud **Certificats (Ordinateur local)&gt; Personnes autorisées&gt; Certificats**. Seuls les certificats figurant à ces deux emplacements peuvent valider les certificats de confiance dans le contexte de l’ordinateur local. Autrement, un message d’erreur ressemblant à la chaîne suivante s’affiche:

```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

Maintenant que le certificat a été approuvé, il existe 2façons d’installer le package: par le biais de PowerShell ou en double-cliquant simplement sur le fichier de package Appx pour l’installer.  Pour effectuer l’installation par le biais de PowerShell, exécutez l’applet de commande suivante:

```powershell
Add-AppxPackage <MyApp>.appx
```

### Inscription de fichiers libres

L’inscription de fichiers libres est utile à des fins de débogage quand les fichiers sont placés sur un disque dans un emplacement auquel vous pouvez facilement accéder et que vous pouvez facilement mettre à jour. De plus, elle ne nécessite pas de signature ou de certificat.  

Pour déployer votre application pendant le développement, exécutez l’applet de commande PowerShell suivante: 

```Add-AppxPackage –Register AppxManifest.xml```

Pour mettre à jour les fichiers .exe ou .dll de votre application, remplacez simplement les fichiers existants dans votre package par les nouveaux, augmentez le nombre de versions dans AppxManifest.xml, puis exécutez à nouveau la commande ci-dessus.

Points à prendre en considération: 

* Tout lecteur sur lequel vous installez votre application convertie doit être formaté au format NTFS.
* Une application convertie s’exécute toujours en tant qu’utilisateur interactif.


<!--HONumber=Nov16_HO1-->


