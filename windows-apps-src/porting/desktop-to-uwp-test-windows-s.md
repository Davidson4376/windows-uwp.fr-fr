---
author: normesta
Description: Test your app for Windows 10 in S mode.
Search.Product: eADQiWindows 10XVcnh
title: Tester votre application Windows pour Windows10S
ms.author: normesta
ms.date: 05/11/2017
ms.topic: article
keywords: windows10 S, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3307506c5cf62d04cd19fbc302ad14bfcedd0045
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7167751"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>Tester votre application Windows pour Windows10 en modeS.

Vous pouvez tester votre application Windows afin de vous assurer qu’elle fonctionnera correctement sur les appareils qui exécutent Windows10 en modeS. En fait, si vous envisagez de publier votre application dans le MicrosoftStore, vous devez le faire, car c'est une exigence du Store. Afin de tester votre application, vous pouvez appliquer une stratégie d’intégrité du code Device Guard sur un appareil qui exécute Windows10 Professionnel.

> [!NOTE]
> L'appareil sur lequel vous appliquez la stratégie d'intégrité du code DeviceGuard doit être exécuté sur Windows10 édition Creators (10.0; Build 15063) ou version ultérieure.

La stratégie d'intégrité du code DeviceGuard applique les règles auxquelles les applications doivent se conformer pour fonctionner avec Windows10 S.

> [!IMPORTANT]
>Nous vous recommandons d’appliquer ces stratégies sur une machine virtuelle, mais si vous souhaitez les mettre en œuvre sur votre machine locale, consultez préalablement nos conseils sur les meilleures pratiques dans la section «Ensuite, installez la stratégie et redémarrez votre système» de cette rubrique.

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>Tout d’abord, téléchargez les stratégies, puis faites votre choix

Téléchargez les stratégies d’intégrité du code Device Guard [ici](https://go.microsoft.com/fwlink/?linkid=849018).

Ensuite, choisissez celle qui vous convient le mieux. Voici un résumé de chaque stratégie.

|Stratégie |Mise en œuvre |Certificat de signature |Nom de fichier |
|--|--|--|--|
|Stratégie de mode d’audit |Consigne les problèmes/ne bloque pas |WindowsStore |SiPolicy_Audit.p7b |
|Stratégie de mode de production |Oui |WindowsStore |SiPolicy_Enforced.p7b |
|Stratégie de mode de produit avec applications auto-signées |Oui |Certificat de Test AppX  |SiPolicy_DevModeEx_Enforced.p7b |

Nous vous recommandons de commencer par la stratégie de mode d’audit. Vous pouvez passer en revue les journaux des événements d’intégrité du code et utiliser ces informations pour vous aider à régler votre application. Ensuite,lorsque vous êtes prêt pour la phase finale du test, appliquez la stratégie de mode de production.

Voici un peu plus d’informations sur chaque stratégie.

### <a name="audit-mode-policy"></a>Stratégie de mode d’audit
Avec ce mode, votre application fonctionne même si elle effectue des tâches qui ne sont pas prises en charge sur Windows10 S. Windows enregistre les fichiers exécutables qui auraient dû être bloqués dans les journaux des événements d’intégrité du code.

Vous trouverez ces journaux en ouvrant l'**Observateur d’événements**et en accédant à l'emplacement suivant: Journaux des applications et des services->Microsoft->Windows->CodeIntegrity->Operational.

![code-integrity-event-logs](images/desktop-to-uwp/code-integrity-logs.png)

Ce mode est sécurité et n'empêchera pas votre système de démarrer.

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(Facultatif) Rechercher des points de défaillance spécifiques dans la pile des appels
Pour rechercher des points spécifiques dans la pile des appels où des problèmes de blocage se produisent, ajoutez cette clé de Registre, puis [configurez un environnement de débogage en mode noyau](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging).

|Clé|Nom|Type|Valeur|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>Stratégie de mode de production
Cette stratégie applique des règles d’intégrité du code correspondant à Windows10 S pour vous permettre de simuler une exécution sur Windows10 S. Il s’agit de la stratégie la plus stricte, qui est très utile pour tester la production finale. Dans ce mode, votre application est soumise aux mêmes restrictions que sur le périphérique d’un utilisateur. Pour utiliser ce mode, votre application doit être signée par le MicrosoftStore.

### <a name="production-mode-policy-with-self-signed-apps"></a>Stratégie de mode de production avec applications auto-signées
Ce mode est similaire à la stratégie du mode de production, mais il permet également d’exécuter certains éléments qui sont signés avec le certificat de test inclus dans le fichier zip. Installez le fichier PFX qui est inclus dans le dossier **AppxTestRootAgency** de ce fichier zip. Signez ensuite votre application à l'aide de celui-ci. Ainsi, vous pouvez répéter rapidement le processus sans avoir besoin de la signature du WindowsStore.

Comme le nom de l’éditeur de votre certificat doit correspondre à celui de votre application, vous devez remplacer temporairement la valeur de l'attribut **Publisher** de l’élément **Identity** par «CN=Appx Test Root Agency Ex». Une fois que vous avez effectué vos tests, vous pouvez rendre à cet attribut sa valeur d’origine.

## <a name="next-install-the-policy-and-restart-your-system"></a>Ensuite, installez la stratégie et redémarrez votre système

Nous vous recommandons d’appliquer ces stratégies sur une machine virtuelle, dans la mesure où elles peuvent entraîner des échecs au démarrage. Ceci est dû au fait que ces stratégies bloquent l’exécution du code non signé par le MicrosoftStore, y compris les pilotes.

Si vous souhaitez appliquer ces stratégies sur votre ordinateur local, il est préférable de démarrer avec la stratégie de mode d’Audit. Cette stratégie vous permet de consulter les journaux des événements d’intégrité du code pour vous assurer qu’aucun élément critique ne sera bloqué par une stratégie appliquée.

Lorsque vous êtes prêt à appliquer une stratégie, recherchez le fichier .P7B correspondant à la stratégie choisie, renommez-le **SIPolicy.P7B**, puis enregistrez-le dans cet emplacement sur votre système: **C:\Windows\System32\CodeIntegrity\\**.

Redémarrez ensuite votre système.

>[!NOTE]
>Pour supprimer une stratégie de votre système, supprimez le fichier .P7B, puis redémarrez votre système.

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Examiner un article de blog détaillé, publié par notre équipe App Consult Team**

Voir [Portage et test de vos applications de bureau classiques sur Windows10 S avec le Pont du bureau](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/).

**En savoir plus sur les outils qui facilitent les tests pour Windows en modeS**

Consultez [Décompresser, modifier, générer un nouveau package, signer un APPX](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/).
