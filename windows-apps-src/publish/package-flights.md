---
author: jnHs
Description: You can use package flights to distribute packages that are only given to a limited test group.
title: Versions d’évaluation de package
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp, distribution de version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: a873b6f6c0d1a35667b47109f5cc2205e5a02158
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6657566"
---
# <a name="package-flights"></a>Versions d’évaluation de package

Vous pouvez utiliser des versions d’évaluation de package pour distribuer des packages spécifiques à un groupe limité de testeurs. Les packages que vous avez déjà publiés dans le magasin serviront pour vos autres clients, afin que leur expérience ne sont pas être interrompue.

Avec les versions d’évaluation du package, seuls les packages sont différents; informations de description du Windows Store sera identique pour tous vos clients. Toute personne de votre groupe de versions d’évaluation recevront les packages que vous incluez dans la version d’évaluation du package, tandis que les clients qui ne sont pas dans le groupe de versions d’évaluation continuent à recevoir vos packages standard (standard).  Si vous décidez ultérieurement que vous souhaitez rendre les packages à partir d’une version d’évaluation disponible pour tous vos clients, vous pouvez facilement utiliser ces packages mêmes lors d’une soumission standard. Notez que les versions d’évaluation de package doivent réussir le [processus de certification](the-app-certification-process.md), exactement le même que toute soumission.

Lorsque vous configurez des versions d’évaluation de package, vous pouvez spécifier les personnes qui doivent bénéficier de packages spécifiques en les ajoutant à un **groupe d’utilisateurs connus** (parfois appelés groupe de versions d’évaluation). Tout membre d’un groupe de versions d’évaluation disposant d’un appareil qui exécute une version de Windows10 prenant en charge les versions d’évaluation de package (build Windows.Desktop 10586 ou ultérieure, build Windows.Mobile 10586.63 ou ultérieure, ou XboxOne) bénéficie des versions d’évaluation de package que vous attribuez à ce groupe spécifique. (Vos versions d’évaluation de package peuvent inclure des packages ciblant n’importe quelle version du système d’exploitation, y compris Windows 8.1 / Windows Phone 8.1 ou version antérieure si votre application publiée précédemment déjà les prend en charge.) Toute personne qui n’a pas été ajouté à l’un de vos groupes de versions d’évaluation, ou d’un appareil qui ne prennent pas en charge les versions d’évaluation du package, obtiendront les packages à partir de la soumission standard.

> [!IMPORTANT] 
> Sur les appareils mobiles et de bureau, les membres de vos groupes de versions d’évaluation obtiendront automatiquement les packages de votre version d’évaluation chaque fois que vous fournirez des mises à jour. Toutefois, **les membres de vos groupes de versions d’évaluation qui utilisent des appareils Xbox devront rechercher manuellement les mises à jour** afin d’obtenir les derniers packages. Pour cela, ils devront se connecter à leur appareil à l’aide de leur compte Microsoft (avec l’adresse e-mail associée que vous avez incluse dans votre groupe d’utilisateurs connus).

Notez que les versions d’évaluation de package ne seront pas distribuées par le biais de [MicrosoftStore pour Entreprises](https://businessstore.microsoft.com/store) et de [MicrosoftStore pour Éducation](https://educationstore.microsoft.com/store). En effet, les membres de vos groupes d’utilisateurs connus doivent être connectés avec leur compte Microsoft afin de pouvoir recevoir une version d’évaluation de package. Toutes les acquisitions effectuées par l’intermédiaire de MicrosoftStore pour Entreprises ou de MicrosoftStore pour Éducation recevront vos packages standard.

> [!TIP]
> Les versions d’évaluation de package offrent des packages uniquement aux clients sélectionnés que vous avez spécifiés. Pour distribuer des packages à une sélection aléatoire de clients selon un pourcentage donné, vous pouvez utiliser le [lancement de package progressif](gradual-package-rollout.md). Vous pouvez également combiner le lancement avec vos versions d’évaluation de package si vous souhaitez distribuer progressivement une mise à jour à l’un de vos groupes de versions d’évaluation.
>
> À la différence des versions d’évaluation de package, vos sélections de lancement progressif de packages s’appliquent aux clients qui achètent votre application par le biais de MicrosoftStore pour Entreprises et de MicrosoftStore pour Éducation. 

> [!TIP]
> Réfléchissez à la manière dont vous souhaitez que les utilisateurs de votre version d’évaluation de package puissent donner leur avis sur l’application. Nous vous suggérons [d’inclure dans votre application un contrôle lançant le Hub de commentaires](../monetize/launch-feedback-hub-from-your-app.md) pour permettre à vos clients de communiquer directement leur avis. Vous pourrez ensuite consulter leurs commentaires dans le [Rapport sur les commentaires](feedback-report.md) de votre application).


## <a name="create-a-new-package-flight"></a>Créer une version d’évaluation de package

Une fois que vous avez publié une soumission pour votre application, une section **Versions d’évaluation de package** s’affiche sur la page de vue d’ensemble de l’application. Cliquez sur **Nouvelle version d’évaluation de package** pour commencer.

Si vous n’avez pas encore créé de groupes d’utilisateurs connus, vous êtes invité à en créer un avant de continuer. Pour plus d’informations, consultez l’article [Créer des groupes d’utilisateurs connus](create-known-user-groups.md). Vous pouvez créer un groupe d’utilisateurs connus directement à partir de cette page en sélectionnant **Create a flight group**.

Sur la page de création de version d’évaluation de package, vous devez entrer un nom pour votre version d’évaluation et définir au moins un groupe de versions d’évaluation. Après avoir effectué cette opération, sélectionnez **Créer une version d’évaluation**. Vous ne pourrez plus modifier ces informations par la suite (toutefois, si les données que vous avez entrées ne vous conviennent pas, vous pourrez supprimer cette version d’évaluation et en créer une autre à utiliser à la place).

> [!NOTE]
> Si vous disposez de plusieurs versions d’évaluation de package, vous devrez les classer. Pour plus d’informations, voir [Ajouter et versions d’évaluation de package supplémentaires rang](#add-and-rank-additional-package-flights) ci-dessous.


## <a name="specify-packages-to-include-in-your-package-flight"></a>Indiquez les packages à inclure dans votre version d’évaluation de package.

Une fois que vous avez enregistré les informations de la version d’évaluation de package, la page de synthèse correspondante s’affiche. Cliquez sur **Packages** pour indiquer les packages que vous souhaitez inclure dans la version d’évaluation. Vous pouvez inclure des packages ciblant n’importe quelle version du système d’exploitation que votre application prend en charge.

Vous pouvez sélectionner des packages associés à une soumission précédemment publiée (une soumission standard ou l’une de vos autres versions d’évaluation de package, si vous en possédez plusieurs). Si vous avez besoin charger de nouveaux packages à utiliser pour cette version d’évaluation de package, vous pouvez les télécharger ici (en utilisant le [même processus que lorsque vous téléchargez des packages d’application pour une soumission standard](upload-app-packages.md)). Cliquez sur **Enregistrer** dès que vous avez fini d’indiquer les packages à inclure dans cette version d’évaluation de package.

Si votre application prend en charge plusieurs familles d’appareils, veillez à inclure les packages permettant de prendre en charge les mêmes familles d’appareils dans votre version d’évaluation. Les utilisateurs inclus dans vos groupes de versions d’évaluation pourront **uniquement** obtenir des packages à partir de cette version d’évaluation. Ils ne pourront pas accéder aux packages des autres versions d’évaluation, ni de votre soumission standard. 

Rappelez-vous que vos informations de description et la disponibilité de la famille d’appareils de périphérique repose sur votre soumission standard. Les clients de vos groupes de versions d’évaluation peuvent uniquement télécharger l’application sur une famille d’appareils prise en charge par votre soumission standard. Pour plus d’informations, consultez [Prise en charge des familles d’appareils](#device-family-support). 


## <a name="gradual-package-rollout"></a>Lancement de package progressif

Par défaut, les packages de votre soumission seront disponibles simultanément pour tous les membres de votre groupe de versions d’évaluation. Pour modifier ce comportement, vous pouvez cocher la case **Déployer progressivement la mise à jour après la publication de cette soumission (pour les clients Windows10 uniquement)**. Vous pouvez choisir un pourcentage de personnes dans votre groupe de versions d’évaluation pour obtenir les packages à partir de la nouvelle soumission, afin d’être en mesure d’analyser les commentaires et les données analytiques et de vérifier le contenu de la mise à jour avant de la déployer au reste du groupe de versions d’évaluation. Vous pouvez augmenter le pourcentage (ou arrêter la mise à jour) à tout moment sans avoir à créer une autre soumission pour votre version d’évaluation de package. 

> [!IMPORTANT]
> Lorsque le lancement progressif de packages dans une version d’évaluation de package, les personnes qui ne sont pas incluses dans le pourcentage et qui récupère vos nouveaux packages obtiendront les packages à partir de la soumission précédente (sauf si une version d’évaluation de niveau supérieur est disponible sur ces derniers).

Pour plus d’informations, voir [Lancement de package progressif](gradual-package-rollout.md).


## <a name="configure-additional-package-flight-options"></a>Configurer des options de versions d’évaluation de package supplémentaires

Par défaut, votre version d’évaluation du package est publiée et mise à disposition de votre groupe de versions d’évaluation une fois le processus de certification terminé. Si vous voulez modifier la [date de publication](set-app-pricing-and-availability.md#publish-date) ou ajouter des [remarques pour la certification](notes-for-certification.md), vous pouvez le faire dans la section **Flight options**. Cliquez sur **Enregistrer** pour revenir à la page de vue d’ensemble de la version d’évaluation de package. 


## <a name="submit-your-package-flight-to-the-store"></a>Soumettre votre version d’évaluation de package au WindowsStore

Une fois les packages définis et les options nécessaires configurées, cliquez sur **Envoyer au Store**. Votre version d’évaluation de package sera alors soumise au [processus de certification des applications](the-app-certification-process.md). Notez que les packages inclus dans votre version d’évaluation de package doivent être conformes aux [Politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies), à l’instar de toutes les soumissions.

Les utilisateurs de vos groupes de versions d’évaluation associés à cette version d’évaluation de package qui possèdent déjà votre application obtiendront une mise à jour lorsqu’ils utiliseront les packages inclus dans votre version d’évaluation. Si ces utilisateurs ne disposent pas encore de votre application, ils obtiendront les packages de votre version d’évaluation de package au moment de l’installation. 

> [!NOTE]
> Les utilisateurs qui possèdent un package uniquement disponible dans une version d’évaluation peuvent attribuer à l’application une évaluation à l’aide d’étoiles et laisser des avis, mais les autres clients n’auront pas accès à ces informations. (Cela ne concerne pas les packagesXAP7.x ou 8.0 hérités; les évaluations et avis laissés par des membres de vos groupes de versions d’évaluation qui utilisent ces packages seront visibles par les autres clients.) Vous pourrez visualiser les évaluations et les commentaires de tous les clients, y compris ceux des membres de vos groupes de versions d’évaluation, dans les rapports **Avis** et **Commentaires** concernant l’application.


## <a name="device-family-support"></a>Prise en charge des familles d’appareils

Dans la plupart des cas, vous pouvez proposer les packages compatibles avec les gammes d’appareils prises en charge par votre soumission standard. La disponibilité d’une famille d’appareils pour une application est toujours basée sur la soumission standard, qu’un client soit ou non membre d’un groupe de versions d’évaluation.

**Si votre soumission standard prend en charge une famille d’appareils que votre version d’évaluation ne prend pas en charge**, les membres de votre groupe de versions d’évaluation ne pourront pas la télécharger sur cette famille d’appareils. Par exemple, si votre soumission standard comprend des packages pour appareils mobiles et de bureau, et que vous créez une version d’évaluation de package qui inclut uniquement un package pour appareils mobiles, les membres de votre groupe de versions d’évaluation pourront uniquement télécharger l’application sur les appareils mobiles, même s’il existe un package pour appareils de bureau pour les clients qui ne font pas partie du groupe de versions d’évaluation. Même si vous utilisez uniquement la version d’évaluation du package pour tester les modifications dans votre package pour appareils mobiles, vous devez inclure le package de bureau de votre soumission standard dans la version d’évaluation du package afin que les clients du groupe de versions d’évaluation soient en mesure de télécharger votre application sur les appareils de bureau.

**Si la version d’évaluation de votre package prend en charge une famille d’appareils que votre soumission standard ne prend pas en charge**, aucun utilisateur ne pourra télécharger l’application sur cette famille d’appareils, qu’il fasse partie du groupe de versions d’évaluation ou non. Par exemple, si votre soumission standard comprend uniquement un package pour appareils mobiles, et que vous créez une version d’évaluation de package qui inclut des packages pour appareils mobiles et de bureau, les membres de votre groupe de versions d’évaluation pourront uniquement télécharger l’application sur les appareils mobiles. Le package pour appareils de bureau ne sera proposé à personne, pas même aux membres de votre groupe de versions d’évaluation. Si vous voulez rendre un package pour appareils de bureau disponible pour les membres de votre groupe de versions d’évaluation, vous devez tout d’abord mettre à jour votre soumission standard pour inclure un package pour appareils de bureau. Pour que tous les clients de votre application bénéficient de la meilleure expérience possible, votre soumission standard doit prendre en charge les même familles d’appareils que la version d’évaluation de votre package. 

> [!NOTE]
> Les packages ajoutés aux versions d’évaluation de votre package peuvent prendre en charge n’importe quelle version du système d’exploitation (ou n’importe quelle build Windows10). Néanmoins, comme indiqué plus haut, les membres des groupes de versions d’évaluation doivent utiliser un appareil exécutant une version de Windows10 qui prend en charge les versions d’évaluation du package (Windows.Desktop build10586 ou version ultérieure; Windows.Mobile build10586.63 ou version ultérieure) afin d’obtenir des packages à partir de la version d’évaluation.


## <a name="update-or-modify-your-package-flight"></a>Mettre à jour ou modifier votre version d’évaluation de package

Pour créer une nouvelle soumission pour une version d’évaluation de package que vous avez déjà publiés, cliquez sur **mise à jour** en regard du nom de la version d’évaluation sur la page de vue d’ensemble de votre application. Vous pouvez ensuite charger de nouveaux packages (et supprimer les packages dont vous n’avez pas besoin), comme vous le feriez avec une soumission standard. Apportez les modifications nécessaires, puis cliquez sur **Envoyer au Store** afin de transmettre la version d’évaluation de package mise à jour vers le [processus de certification des applications](the-app-certification-process.md).

Pour modifier une version d’évaluation existante sans créer et soumettre une nouvelle mise à jour, cliquez sur **Modifier** en regard du nom de la version d’évaluation. Cette opération vous permet de modifier différents détails, tels que les groupes de versions d’évaluation, le nom et le classement, sans que la version d’évaluation de package doive repasser par le processus de certification. Notez que si vous disposez d’une mise à jour en cours d’exécution, ou si votre version d’évaluation de package n’a pas encore été publiée, vous ne verrez pas l’option **Modifier** . 


## <a name="add-and-rank-additional-package-flights"></a>Ajouter et classer des versions d’évaluation de package supplémentaires

Vous pouvez créer plusieurs versions d’évaluation de package pour une même application afin de distribuer différents packages à divers ensembles de clients. 

Une fois que vous avez créé votre première version d’évaluation de package, vous pouvez en créer une autre en suivant le processus décrit ci-dessus. La seule différence réside dans le fait que, si vous avez déjà créé une version d’évaluation de package, vous devrez spécifier l’ordre de priorité de l’ensemble des versions d’évaluation de package dans la section **Rang**. Cela permet de déterminer les packages à distribuer à un client donné s’ils appartiennent à plusieurs de vos groupes de versions d’évaluation le Windows Store. Les membres de vos groupes de versions d’évaluation bénéficieront toujours de la version d’évaluation de package la mieux classée disponible, même si une version d’évaluation de package moins bien classée comporte des packages présentant un numéro de version plus élevé.

Par défaut, votre nouvelle version d’évaluation de package sera classée au premier rang. Si vous souhaitez modifier son classement, vous pouvez la déplacer vers le bas (ou de nouveau vers le haut) dans la liste des versions d’évaluation de package.

Notez que votre soumission standard est toujours classée le plus bas (n ° 1). Concrètement, cela signifie que les utilisateurs qui n’appartiennent à aucun de vos groupes de versions d’évaluation peuvent uniquement obtenir des packages de votre soumission standard par le biais du Store. Personnes dans un groupe de versions d’évaluation bénéficieront toujours des packages à partir de la version d’évaluation du package classée disponible (mais jamais de la soumission standard, dans la mesure où il a le rang le plus bas). Ainsi, vous pouvez choisir la manière dont vous souhaitez distribuer vos packages aux utilisateurs qui sont membres de plusieurs de vos groupes de versions d’évaluation.

Par exemple, considérons que vous souhaitez créer deux versions d’évaluation de package en complément de votre soumission standard: une qui est relativement stable et prête pour l’étape de test auprès d’un public large, et une autre qui présente davantage d’incertitudes et qui sera présentée à un groupe limité de testeurs. Vous pourriez créer un groupe de versions d’évaluation appelé Testeurs et dans lequel vous incluriez une version d’évaluation de package appelée Version d’évaluation Testeurs, puis créer un groupe de versions d’évaluation appelé Enthousiastes plus ouvert, dans lequel vous incluriez une autre version d’évaluation appelée Version d’évaluation Enthousiastes. Si vous classez la Version d’évaluation Testeurs à un rang plus élevé que la Version d’évaluation Enthousiastes, vous pouvez utiliser les packages que vous jugez fiables dans la Version d’évaluation Enthousiastes, tout en attribuant les packages présentant davantage de risques uniquement aux utilisateurs de la version d’évaluation Testeurs. Les membres du groupe Testeurs bénéficieront toujours des packages de la version d’évaluation Testeurs, même s’ils appartiennent également au groupe Enthousiastes. (Par la suite, si les packages de la Version d’évaluation Testeurs sont performants, vous pouvez mettre à jour la Version d’évaluation Enthousiastes afin que ses membres puissent utiliser les packages distribués à l’origine dans la Version d’évaluation Testeurs, et en définitive utiliser ces packages dans votre soumission standard.)


## <a name="make-packages-from-a-package-flight-available-to-all-your-customers"></a>Rendre des packages d’une version d’évaluation de package disponible à l’ensemble de vos clients

Si vous décidez de mettre à la disposition des clients n’appartenant pas à un groupe de versions d’évaluation un ou plusieurs des packages inclus dans une version d’évaluation publiée, vous pouvez mettre à jour votre soumission standard pour qu’elle utilise ces packages, sans avoir besoin de recharger ces packages. 

Lorsque vous créez une autre soumission, la page [**Packages**](upload-app-packages.md) affiche une liste déroulante comportant une option de copie des packages provenant de l’une de vos versions d’évaluation de package précédentes. Sélectionnez la version d’évaluation de package comportant les packages que vous souhaitez intégrer. Vous pouvez transférer une partie ou la totalité des packages dans la soumission standard.

Notez que l’ensemble des règles de validation de package s’appliquent, même si vous utilisez des packages d’une soumission précédemment publiée. 


## <a name="delete-a-package-flight"></a>Supprimer une version d’évaluation de package

Pour supprimer une version d’évaluation de package que vous ne souhaitez plus prendre en charge, cliquez sur le nom correspondant sur la page de présentation de l’application. Dans la page de vue d’ensemble de la version d’évaluation, cliquez sur **Modifier**, puis cliquez sur le lien **Supprimer** pour supprimer la version d’évaluation du package. (Si vous disposez d’une soumission non publiée de la version d’évaluation du package, vous devrez commencer par supprimer cette dernière.) Cette opération peut prendre jusqu’à 30minutes.

Lorsque vous supprimez une version d’évaluation d’un package, les clients qui utilisent les packages distribués dans celle-ci bénéficient d’une mise à jour applicative si un package présentant un numéro de version supérieur existe (ou dès la mise à disposition d’un tel package). Si les clients désinstallent l’application avant de la réinstaller par la suite, cette opération sera considérée comme une nouvelle acquisition, et les clients bénéficieront alors de la version la plus élevée disponible. 
