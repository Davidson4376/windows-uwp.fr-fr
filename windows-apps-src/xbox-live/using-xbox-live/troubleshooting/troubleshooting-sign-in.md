---
title: Résolution des problèmes Xbox Live connectez-vous
description: Découvrez comment résoudre les problèmes liés à Xbox Live connectez-vous.
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, connectez-vous, résoudre les problèmes
ms.localizationpriority: medium
ms.openlocfilehash: ff8d66105d8a1a44708bf23a681767a3044cb654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630324"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Résolution des problèmes Xbox Live connectez-vous

Il existe plusieurs problèmes qui peuvent provoquer des difficultés à signature-dans.  Si vous suivez les étapes de [bien démarrer avec Visual Studio pour les jeux UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) vous pouvez réduire le risque pour les erreurs inattendues.

## <a name="common-issues"></a>Problèmes courants

### <a name="sandbox-problems"></a>Problèmes de bac à sable

En règle générale, vous devez vous familiariser avec le concept de bacs à sable et comment elles se rapportent à la Xbox Live.  Vous pouvez voir plus d’informations dans le [bacs à sable de Xbox Live](../../xbox-live-sandboxes.md) guide.

En bref, les bacs à sable appliquent contenu isolation et contrôle d’accès avant la mise en production de la vente au détail.  Les utilisateurs sans accès à votre sandbox de développement ne peut pas effectuer une lecture opérations ou d’écriture qui se rapportent à votre titre.  Vous pouvez également publier des variations de la configuration de service pour différents bacs à sable pour le test.

Quelques éléments à surveiller en ce qui concerne les bacs à sable sont décrites ci-dessous.

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>Compte de développeur n’a pas accès le bac à sable droite pour l’accès d’exécution

* Un compte de test (également connu sous le compte de développement) ou un compte de développeur autorisés doit être utilisé pour la connexion à un titre qui se trouve dans le développement.  Vérifiez que vous essayez de connexion à l’aide d’une ou les autres comptes de Test sont créés sur XDP à [ https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts ](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts). Vous pouvez autoriser un compte de développeur associé live xbox sur les partenaires à [https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* Vérifiez que le compte a accès au bac à sable dans que votre titre est publié.  Les comptes de test que vous créez dans XDP héritent des autorisations du compte XDP qui les a créés

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>Votre appareil n’est pas sur le bac à sable correct

L’appareil que vous développez sur doit être définie à un bac à sable de développement.  Sur Xbox One, vous pouvez définir votre bac à sable à l’aide *Xbox One Manager*.  Pour Windows 10 Desktop, vous pouvez utiliser le script SwitchSandbox.cmd qui se trouve dans le répertoire des outils de l’installation de Xbox Live SDK.

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>Configuration du service de votre titre n’est pas publiée dans le bac à sable de développement correct

Vérifiez la que configuration du service de votre titre est publiée dans un bac à sable de développement.  Vous ne peut pas connectez-vous à Xbox Live dans un bac à sable de développement donné pour un titre, à moins que titre publié dans le même bac à sable.  Veuillez consulter la [documentation de XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) pour plus d’informations sur comment effectuer cette opération. Vous pouvez lire le [Partner Center Documentation](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration) pour apprendre à publier votre configuration de partenaires.

### <a name="ids-configured-incorrectly"></a>ID pas configurés correctement.

Il existe plusieurs éléments d’ID requis pour configurer votre jeu.  Vous pouvez voir plus d’informations dans [bien démarrer avec Visual Studio pour les jeux UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) et [mise en route avec les jeux de cross-play](../../get-started-with-partner/get-started-with-cross-play-games.md) selon le type du titre que vous créez.

Quelques éléments à surveiller sont :

* Vérifiez que votre ID d’application a été entré correctement dans XDP ou partenaires
* Vérifiez que votre NFP est conclu XDP ou partenaires correctement
* Vérifiez vous avez créé un xboxservices.config dans le même répertoire que votre projet Visual Studio, comme décrit dans la [Ajout Xbox Live à un projet UWP existant ou nouveau](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) guide.
* Vérifiez que l’identité « Package » dans votre appxmanifest est correcte.  Cela est illustré dans l’espace partenaires en tant que « Package/Identity/Name » dans la section de l’identité de l’application.

### <a name="title-id-or-scid-not-configured-correctly"></a>ID de titre ou SCID ne pas configuré correctement

* Pour les titres UWP, titre de votre ID et SCID doit être définie à la valeur correcte dans votre fichier xboxservices.config.  Vérifiez également que ce fichier est correctement formaté en tant qu’UTF-8.  Vous pouvez voir plus d’informations dans [bien démarrer avec Visual Studio pour les jeux UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md). Le fichier xboxservices.config respecte la casse.
* Pour les titres XDK, ces valeurs sont définies dans votre fichier package.appxmanifest.
* Vous pouvez voir des exemples pour UWP et XDK configuration titre dans le répertoire d’exemples du SDK Xbox Live.

## <a name="test-using-the-xbox-app"></a>Tester à l’aide de l’application Xbox

Si vous développez une application UWP, vous pouvez déboguer certains problèmes à l’aide de l’application Xbox :

1. Définir le bac à sable de votre appareil à un bac à sable de développement à l’aide du script SwitchSandbox.cmd
2. Ouvrez l’application Xbox et tenter de se connecter à l’aide d’un compte de test ayant accès à la même bac à sable.

Si vous êtes en mesure de correctement connectez-vous, puis vérifier si votre bac à sable de développement a été correctement sur votre appareil, et votre compte de test a accès à celui-ci.

Si vous obtenez encore des erreurs de connexion, il est probable que votre configuration de service n’est pas publiée dans votre bac à sable ou votre xboxservices.config n’est pas configurée correctement. Le fichier xboxservices.config respecte la casse.

## <a name="debug-based-on-error-code"></a>Selon le code d’erreur de débogage

Voici quelques-uns des codes d’erreur que peut s’afficher lors de connexion et d’étapes à que suivre pour déboguer ces.  Vous verrez le code d’erreur comme illustré dans la capture d’écran ci-dessous.

![capture d’écran de 0x8015DC12-erreur de connexion](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003 l’application est activé ou configurée de manière incorrecte

1. Essayez de supprimer votre fichier PFX.

![fichier pfx dans l’Explorateur de solutions](../../images/troubleshooting/pfx_file.png)

Si vous n’avez pas connectez-vous à Visual Studio avec le Microsoft Account utilisé pour la configuration de l’application Centre de partenaires, Visual Studio sont automatiquement générer une signature d’un fichier pfx en fonction de votre personnel Account Microsoft ou votre compte de domaine. Lorsque vous créez le package appx, Visual Studio utilise ce auto pfx généré pour signer le package & modifier la partie « publisher » de l’identité du package dans le package.appxmanifest. Par conséquent, les bits de produit (plus précisément, le appxmanifest.xml) aura une identité de package différent à ce que vous souhaitez utiliser. 

2. Vérifiez que votre fichier package.appxmanifest est définie sur la même identité d’application que le titre de votre centre de partenaires. Vous pouvez cliquez avec le bouton droit sur votre projet et choisissez Store -> associer l’application avec Store... comme indiqué dans la capture d’écran ci-dessous. Ou modifiez manuellement votre fichier package.appxmanifest. Consultez [bien démarrer avec Visual Studio pour les jeux UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) pour plus d’informations.

![Associer au Windows store](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>Erreur de l’Isolation du contenu 0x8015DC12

En résumé, cela signifie que l’appareil ou l’utilisateur n’a pas accès pour le titre spécifié.

1. Cela signifie vous n’utilisez pas un compte de test pour tenter de connexion ou votre compte de test n’a pas accès à la même sandbox que vous êtes connecté en tant que. Vérifiez que les instructions sur la création de comptes de test dans le [documentation de XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx) et [documentation de partenaires.](../../xbox-live-test-accounts.md) Si nécessaire créer un compte de test avec un accès pour le bac à sable approprié.

Vous devrez peut-être supprimer votre ancien compte de Windows 10, vous pouvez la faire et qui en accédant aux paramètres dans le Menu Démarrer et en accédant ensuite aux comptes

2. Vérifiez que votre titre est publié dans le bac à sable que que vous voulez utiliser. Consultez le [documentation de XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) ou [Documentation du centre de partenaires](../../xbox-live-service-configuration.md#sandbox-ids) pour plus d’informations sur la procédure à suivre.

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 inattendu ou inconnu titre

Vérifiez la configuration de l’ID d’Application et le centre de développement obligatoire dans XDP. Vous pouvez consulter les instructions dans [Ajout Xbox Live prise en charge vers une nouvelle ou existante Visual Studio plateforme UWP](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)

### <a name="0x87dd000e-title-not-authorized"></a>titre 0x87DD000E pas autorisé

Vérifiez que votre appareil est configuré pour le bac à sable de développement appropriés et que l’utilisateur a accès au bac à sable. Consultez le [tester à l’aide de l’application Xbox](#test-using-the-xbox-app) section pour plus d’informations sur la façon dont vous pouvez vérifier ces éléments avec l’application Xbox.

Si cela ne résout pas le problème, puis vérifier également la configuration de liaison de centre de développement et l’ID d’application comme décrit ci-dessus.

Si vous obtenez une erreur ne pas décrite ici, reportez-vous à la liste d’erreurs dans la documentation xbox::services::xbox_live_error_code pour obtenir plus d’informations sur les codes d’erreur. Vous pouvez également faire référence à errors.h dans le XSAPI inclut.

Après toutes ces étapes, si vous toujours ne peut pas connectez-vous avec votre titre, Veuillez publier un thread de prise en charge sur le [forums](https://forums.xboxlive.com) ou contacter votre mère.
