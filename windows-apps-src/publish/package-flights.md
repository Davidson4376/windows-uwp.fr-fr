---
author: jnHs
Description: "Si votre application utilise AdMediatorControl ou AdControl pour afficher des bannières publicitaires, vous pourriez augmenter votre taux de remplissage et vos revenus publicitaires en présentant des annonces des affiliés Microsoft dans votre application."
title: "Versions d’évaluation de package"
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.sourcegitcommit: 9e62a7aa18950f7e1cc26b42762e3bb937c389ac
ms.openlocfilehash: c538da2a58f38925938b9e28ec7ca65cdb9858a3

---

# Versions d’évaluation de package

Vous pouvez utiliser les versions d’évaluation de package pour distribuer les packages proposés uniquement à un groupe de test limité. 

Vous avez ainsi la possibilité de proposer des packages différents à un ensemble désigné de testeurs, sans perturber l’activité des autres clients. Seuls les packages sont différents. Les données de description dans le Windows Store sont identiques pour l’ensemble des clients.

Notez que les versions d’évaluation de package doivent réussir le [processus de certification](the-app-certification-process.md), tout comme dans le cas d’une soumission standard sans version d’évaluation. Si vous décidez par la suite de mettre les packages d’une version d’évaluation à la disposition de l’ensemble de vos clients, vous pouvez déplacer ces packages dans votre soumission standard, comme décrit ci-après.

Lorsque vous configurez des versions d’évaluation de package, vous pouvez choisir les utilisateurs devant bénéficier de packages spécifiques en les ajoutant à un groupe de versions d’évaluation. Tout membre d’un groupe de versions d’évaluation disposant d’un appareil qui exécute une version de Windows 10 prenant en charge les versions d’évaluation de package (build Windows.Desktop 10586 ou ultérieure, build Windows.Mobile 10586.63 ou ultérieure) bénéficiera des versions d’évaluation de package que vous attribuez à ce groupe spécifique. Les utilisateurs qui n’ont été ajoutés à aucun de vos groupes de versions d’évaluation ou dont l’appareil ne prend pas en charge les versions d’évaluation de package obtiendront les packages de la soumission standard.

Une fois que vous avez publié une soumission pour votre application, une section **Versions d’évaluation de package** s’affiche sur la page de présentation de l’application. Cliquez sur **Nouvelle version d’évaluation de package** pour commencer. Si vous n’avez pas encore configuré de groupes de versions d’évaluation, vous êtes invité à en créer un avant de continuer.

## Créer un groupe de versions d’évaluation

Les groupes de versions d’évaluation vous permettent de désigner les personnes que vous souhaitez voir intégrer le groupe. Pour obtenir les versions d’évaluation de package, chaque utilisateur doit être authentifié dans le Store avec le compte Microsoft associé à l’adresse de messagerie fournie, et utiliser un appareil Windows 10 (comme expliqué ci-dessus) pour télécharger l’application.

Attribuez un nom au groupe de versions d’évaluation lors de sa création. Chaque groupe de versions d’évaluation doit contenir au moins une adresse e-mail et jusqu’à 10 000 adresses e-mail maximum. Vous pouvez saisir les adresses e-mail directement dans le champ (séparées par des espaces, des virgules ou des points-virgules) ou cliquer sur le lien **Import .csv** pour créer le groupe de versions d’évaluation à partir d’une liste d’adresses e-mail d’un fichier .csv.

Cliquez sur **Créer un groupe** pour enregistrer le groupe et continuer la configuration de la version d’évaluation du package.

> **Important** Veillez à obtenir le consentement des utilisateurs ajoutés à votre groupe de versions d’évaluation, et vérifiez qu’ils comprennent que les packages qui leur seront proposés seront différents de ceux de votre soumission standard. 
> Vous pouvez également réfléchir à la manière dont vous souhaitez que les utilisateurs de votre version d’évaluation de package donnent leur avis sur l’application. Nous vous suggérons d’[inclure dans votre application un contrôle lançant le Hub de commentaires](../monetize/launch-feedback-hub-from-your-app.md) pour permettre à vos clients de communiquer directement leur avis ; vous pourrez ensuite consulter leurs commentaires dans le [Rapport sur les commentaires](feedback-report.md) de votre application).

Pour modifier ultérieurement votre groupe de versions d’évaluation, cliquez sur **Exporter au format CSV** afin d’enregistrer les données de votre groupe dans un fichier .csv. Effectuez vos modifications dans ce fichier, puis cliquez sur **Importer au format CSV** pour utiliser la nouvelle version afin de mettre à jour l’appartenance au groupe. Notez que l’implémentation des modifications d’appartenance au groupe de versions d’évaluation peut prendre jusqu’à 30 minutes. Si vous ajoutez des contacts à un groupe de versions d’évaluation après avoir publié une version d’évaluation de package associée, les packages seront automatiquement transférés aux nouveaux contacts ; vous n’êtes pas obligé de créer et publier une nouvelle soumission pour cette version d’évaluation de package. 

## Créer une version d’évaluation de package

Une fois que vous avez créé votre premier groupe de versions d’évaluation, une page s’affiche pour vous permettre de finaliser la configuration du groupe. Vous devrez attribuer un nom à la version d’évaluation de package, et définir au moins un groupe de versions d’évaluation. Si vous voulez configurer un nouveau groupe, vous pouvez le faire à partir de cette page.

Cliquez sur **Créer une version d’évaluation** une fois que vous avez entré le nom et que vous avez sélectionné le ou les groupes de versions d’évaluation. Vous ne pourrez plus modifier ces informations par la suite (mais vous pourrez toujours les supprimer et créer une autre version d’évaluation de package à utiliser à la place).

> Remarque Si vous disposez de plusieurs versions d’évaluation de package, vous devrez les classer. Pour plus d’informations, voir la section « Ajouter et classer des versions d’évaluation de package supplémentaires » ci-dessous.

## Indiquez les packages à inclure dans votre version d’évaluation de package.

Une fois que vous avez enregistré les informations de la version d’évaluation de package, la page de synthèse correspondante s’affiche. Cliquez sur **Packages** pour indiquer les packages que vous souhaitez inclure dans la version d’évaluation.

Vous pouvez sélectionner des packages associés à une soumission précédemment publiée (une soumission standard ou l’une de vos autres versions d’évaluation de package, si vous en possédez plusieurs). Si vous avez besoin de charger de nouveaux packages à utiliser pour cette version d’évaluation de package, vous pouvez le faire à ce stade (en suivant le processus permettant de [charger des packages d’application](upload-app-packages.md) dans une soumission standard). Cliquez sur **Enregistrer** dès que vous avez fini d’indiquer les packages à inclure dans cette version d’évaluation de package.

Si votre application prend en charge plusieurs familles d’appareils, veillez à inclure les packages permettant de prendre en charge les mêmes familles d’appareils dans votre version d’évaluation. Les utilisateurs inclus dans vos groupes de versions d’évaluation pourront **uniquement** obtenir des packages à partir de cette version d’évaluation. Ils ne pourront pas accéder aux packages des autres versions d’évaluation, ni de votre soumission standard. 

Sachez également que les données de description sur le Store proviennent de votre soumission standard, y compris les familles d’appareils que votre application prend en charge. Les clients de vos groupes de versions d’évaluation pourront uniquement télécharger l’application sur une famille d’appareils prise en charge par votre soumission standard. Pour plus d’informations, voir [Prise en charge des familles d’appareils](#device-family-support). 

## Configurer des options de versions d’évaluation de package supplémentaires

Par défaut, votre version d’évaluation du package est publiée et mise à disposition de votre groupe de versions d’évaluation une fois le processus de certification terminé. Si vous voulez modifier la [date de publication](set-app-pricing-and-availability.md#publish-date) ou ajouter des [remarques pour la certification](notes-for-certification.md), vous pouvez le faire dans la section **Options**. Cliquez sur **Enregistrer** pour revenir à la page relative à la vue d’ensemble des versions d’évaluation du package. 

## Soumettre votre version d’évaluation de package au Windows Store

Une fois les packages définis et les options nécessaires configurées, cliquez sur **Envoyer au Store**. Votre version d’évaluation de package sera alors soumise au [processus de certification des applications](the-app-certification-process.md). Notez que les packages inclus dans votre version d’évaluation de package doivent être conformes aux [politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx), comme c’est le cas pour l’ensemble des soumissions.

Les utilisateurs de vos groupes de versions d’évaluation associés à cette version d’évaluation de package qui possèdent déjà votre application obtiendront une mise à jour lorsqu’ils utiliseront les packages inclus dans votre version d’évaluation. Si ces utilisateurs ne disposent pas encore de votre application, ils obtiendront les packages de votre version d’évaluation de package au moment de l’installation. 

> Remarque Les utilisateurs qui possèdent un package uniquement disponible dans une version d’évaluation peuvent attribuer à l’application une évaluation en étoiles et laisser des avis, mais les autres clients n’auront pas accès à ces informations. (Cela ne concerne pas les packages XAP 7.x ou 8.0 hérités ; les évaluations et avis laissés par des membres de vos groupes de versions d’évaluation qui utilisent ces packages seront visibles par les autres clients.) Vous pourrez visualiser les commentaires de tous les clients, y compris ceux des membres de vos groupes de versions d’évaluation, dans les rapports Évaluations et avis concernant l’application.

## Prise en charge des familles d’appareils

Dans la plupart des cas, vous pouvez proposer les packages compatibles avec les gammes d’appareils prises en charge par votre soumission standard. La disponibilité d’une famille d’appareils pour une application est toujours basée sur la soumission standard, qu’un client soit ou non membre d’un groupe de versions d’évaluation.

**Si votre soumission standard prend en charge une famille d’appareils que votre version d’évaluation ne prend pas en charge**, les membres de votre groupe de versions d’évaluation ne pourront pas la télécharger sur cette famille d’appareils. Par exemple, si votre soumission standard comprend des packages pour appareils mobiles et de bureau, et que vous créez une version d’évaluation de package qui inclut uniquement un package pour appareils mobiles, les membres de votre groupe de versions d’évaluation pourront uniquement télécharger l’application sur les appareils mobiles, même s’il existe un package pour appareils de bureau pour les clients qui ne font pas partie du groupe de versions d’évaluation. Même si vous utilisez uniquement la version d’évaluation du package pour tester les modifications dans votre package pour appareils mobiles, vous devez inclure le package de bureau de votre soumission standard dans la version d’évaluation du package afin que les clients du groupe de versions d’évaluation soient en mesure de télécharger votre application sur les appareils de bureau.

**Si la version d’évaluation de votre package prend en charge une famille d’appareils que votre soumission standard ne prend pas en charge**, aucun utilisateur ne pourra télécharger l’application sur cette famille d’appareils, qu’il fasse partie du groupe de versions d’évaluation ou non. Par exemple, si votre soumission standard comprend uniquement un package pour appareils mobiles, et que vous créez une version d’évaluation de package qui inclut des packages pour appareils mobiles et de bureau, les membres de votre groupe de versions d’évaluation pourront uniquement télécharger l’application sur les appareils mobiles. Le package pour appareils de bureau ne sera proposé à personne, pas même aux membres de votre groupe de versions d’évaluation. Si vous voulez rendre un package pour appareils de bureau disponible pour les membres de votre groupe de versions d’évaluation, vous devez tout d’abord mettre à jour votre soumission standard pour inclure un package pour appareils de bureau. Pour que tous les clients de votre application profitent de la meilleure expérience possible, votre soumission standard doit prendre en charge les même familles d’appareils que la version d’évaluation de votre package. 

**Remarque** Les packages ajoutés aux versions d’évaluation de votre package peuvent prendre en charge n’importe quelle version du système d’exploitation (ou n’importe quelle build Windows 10). Néanmoins, comme indiqué plus haut, les membres des groupes de versions d’évaluation doivent utiliser un appareil exécutant une version de Windows 10 prenant en charge les versions d’évaluation du package (Windows.Desktop build 10586 ou version ultérieure ; Windows.Mobile build 10586.63 ou version ultérieure) afin d’obtenir des packages à partir de la version d’évaluation.

## Mettre à jour ou modifier votre version d’évaluation de package

Pour créer une autre soumission associée à une version d’évaluation de package existante, cliquez sur **Mettre à jour** en regard du nom de la version d’évaluation sur la page de présentation de votre application. Vous pouvez ensuite charger de nouveaux packages (et supprimer les packages dont vous n’avez pas besoin), comme vous le feriez avec une soumission standard. Apportez les modifications nécessaires, puis cliquez sur **Envoyer au Store** afin de transmettre la version d’évaluation de package mise à jour vers le [processus de certification des applications](the-app-certification-process.md).

Pour modifier une version d’évaluation existante sans créer et soumettre une nouvelle mise à jour, cliquez sur **Modifier** en regard du nom de la version d’évaluation. Cette opération vous permet de modifier différents détails, tels que les groupes de versions d’évaluation, le nom et le classement, sans que la version d’évaluation de package doive repasser par le processus de certification.

## Ajouter et classer des versions d’évaluation de package supplémentaires

Vous pouvez créer plusieurs versions d’évaluation de package pour une même application afin de distribuer différents packages à divers ensembles de clients. 

Une fois que vous avez créé votre première version d’évaluation de package, vous pouvez en créer une autre en suivant le processus décrit ci-dessus. La seule différence réside dans le fait que, si vous avez déjà créé une version d’évaluation de package, vous devrez spécifier l’ordre de priorité de l’ensemble des versions d’évaluation de package dans la section **Rang**. Le Windows Store peut ainsi identifier les packages à distribuer à un client donné, même s’il appartient à plusieurs de vos groupes de versions d’évaluation. Les membres de vos groupes de versions d’évaluation bénéficieront toujours de la version d’évaluation de package la mieux classée disponible, même si une version d’évaluation de package moins bien classée comporte des packages présentant un numéro de version plus élevé.

Par défaut, votre nouvelle version d’évaluation de package sera classée au premier rang. Si vous souhaitez modifier son classement, vous pouvez la déplacer vers le bas (ou de nouveau vers le haut) dans la liste des versions d’évaluation de package.

Notez que votre soumission standard est toujours classée en dernière position. Concrètement, cela signifie que les utilisateurs qui n’appartiennent à aucun de vos groupes de versions d’évaluation peuvent uniquement obtenir des packages de votre soumission standard par le biais du Store. Les utilisateurs appartenant à un groupe de versions d’évaluation bénéficient toujours des packages de la version d’évaluation la mieux classée disponible (mais jamais de ceux de la soumission standard). Ainsi, vous pouvez choisir la manière dont vous souhaitez distribuer vos packages aux utilisateurs qui sont membres de plusieurs de vos groupes de versions d’évaluation.

Par exemple, considérons que vous souhaitez créer deux versions d’évaluation de package en complément de votre soumission standard : une qui est relativement stable et prête pour l’étape de test auprès d’un public large, et une autre qui présente davantage d’incertitudes et qui sera présentée à un groupe limité de testeurs. Vous pourriez créer un groupe de versions d’évaluation appelé Testeurs et dans lequel vous incluriez une version d’évaluation de package appelée Version d’évaluation Testeurs, puis créer un groupe de versions d’évaluation appelé Enthousiastes plus ouvert, dans lequel vous incluriez une autre version d’évaluation appelée Version d’évaluation Enthousiastes. Si vous classez la Version d’évaluation Testeurs à un rang plus élevé que la Version d’évaluation Enthousiastes, vous pouvez utiliser les packages que vous jugez fiables dans la Version d’évaluation Enthousiastes, tout en attribuant les packages présentant davantage de risques uniquement aux utilisateurs de la version d’évaluation Testeurs. Les membres du groupe Testeurs bénéficieront toujours des packages de la version d’évaluation Testeurs, même s’ils appartiennent également au groupe Enthousiastes. (Par la suite, si les packages de la Version d’évaluation Testeurs sont performants, vous pouvez mettre à jour la Version d’évaluation Enthousiastes afin que ses membres puissent utiliser les packages distribués à l’origine dans la Version d’évaluation Testeurs, et en définitive utiliser ces packages dans votre soumission standard.)

## Rendre des packages d’une version d’évaluation de package disponible à l’ensemble de vos clients

Si vous décidez de mettre à la disposition des clients n’appartenant pas à un groupe de versions d’évaluation un ou plusieurs des packages inclus dans une version d’évaluation publiée, vous pouvez mettre à jour votre soumission standard pour qu’elle utilise ces packages, sans avoir besoin de recharger ces packages. 

Lorsque vous créez une autre soumission, la page [**Packages**](upload-app-packages.md) affiche une liste déroulante comportant une option de copie des packages provenant de l’une de vos versions d’évaluation de package précédentes. Sélectionnez la version d’évaluation de package comportant les packages que vous souhaitez intégrer. Vous pouvez transférer une partie ou la totalité des packages dans la soumission standard.

Notez que l’ensemble des règles de validation de package s’appliquent, même si vous utilisez des packages d’une soumission précédemment publiée. 

## Supprimer une version d’évaluation de package

Pour supprimer une version d’évaluation de package que vous ne souhaitez plus prendre en charge, cliquez sur le nom correspondant sur la page de présentation de l’application. Dans la page de vue d’ensemble de la version d’évaluation, cliquez sur **Modifier**, puis cliquez sur le lien **Supprimer** pour supprimer la version d’évaluation du package. (Si vous disposez d’une soumission non publiée de la version d’évaluation du package, vous devrez commencer par supprimer cette dernière.) Cette opération peut prendre jusqu’à 30 minutes.

Lorsque vous supprimez une version d’évaluation d’un package, les clients qui utilisent les packages distribués dans celle-ci bénéficient d’une mise à jour applicative si un package présentant un numéro de version supérieur existe (ou dès la mise à disposition d’un tel package). Si les clients désinstallent l’application avant de la réinstaller par la suite, cette opération sera considérée comme une nouvelle acquisition, et les clients bénéficieront alors de la version la plus élevée disponible. 



<!--HONumber=Jun16_HO4-->


