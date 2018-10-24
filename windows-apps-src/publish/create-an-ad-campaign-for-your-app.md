---
author: JnHs
Description: You can create an ad campaign using the Dev Center dashboard to help promote your app and grow your app's user base.
title: Création d’une campagne de publicité pour votre application
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, ActiveDirectory, de campagne, promouvoir
ms.localizationpriority: medium
ms.openlocfilehash: 8c7e7c1e2cd9a2ca083cef5fed27a9067cf1e7e7
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5468510"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Créer une campagne de publicité pour votre application

Vous pouvez créer une campagne de publicité à l’aide du tableau de bord du Centre de développement pour vous aider à promouvoir votre application et à développer la base des utilisateurs de votre application. Par défaut, nous allons choisir le public cible de vos publicités d’après les paramètres de votre application dans le tableau de bord du Centre de développement. Vous avez toutefois la possibilité de définir votre propre public. Vous pouvez également utiliser un ensemble de modèles de publicité par défaut ou charger vers le serveur vos propres conceptions d’annonces. Pour plus d’informations sur les campagnes de publicité, voir [Questions courantes sur les campagnes de publicité](common-questions.md).

Vous ne pouvez créer des campagnes de publicité que pour les applications qui ont réussi la phase de publication finale du [processus de certification des applications](the-app-certification-process.md).

> [!NOTE]
> Cette section de la documentation explique comment créer une campagne de publicité dans le tableau de bord du Centre de développement. Vous pouvez également utiliser l’[API de promotions du MicrosoftStore](../monetize/run-ad-campaigns-using-windows-store-services.md) pour créer et gérer les campagnes de publicité par programme.

## <a name="instructions"></a>Instructions

Voici comment créer une campagne de publicité pour promouvoir une application:

1.  Dans le menu de navigation de gauche du tableau de bord, développez l’option **Attract**, puis sélectionnez **Campagnes de publicité**.
2.  Sélectionnez **Créer une campagne** (ou si vous avez déjà créé des campagnes auparavant, sélectionnez **Nouvelle campagne**).
3.  Sur la page suivante, dans la section **Type d’objectif**, choisissez l’une des options suivantes:
    * **Augmenter les installations de votre application**. Sélectionnez cette option si votre campagne de publicité est conçue pour convaincre les clients d’installer votre application.
    * **Augmenter l’intérêt pour votre application**. Sélectionnez cette option si votre campagne de publicité vise à inciter les clients à utiliser votre application plus souvent. Lorsque vous sélectionnez cette option, vous pouvez cibler votre campagne de publicité sur des [segments de clientèle](create-customer-segments.md) spécifiques que vous définissez.

4.  Sélectionnez l’application que vous souhaitez promouvoir avec cette campagne. Notez que l’application doit être déjà disponible dans le WindowsStore.
5.  Dans le champ **Nom de la campagne**, examinez le nom fourni pour votre campagne et modifiez-le si vous le souhaitez.
6.  Sous **Type de campagne**, choisissez l’une des options suivantes:
    * **Paid ad**: ces publicités s’exécutent dans n’importe quelle application correspondant à l’appareil et à la catégorie de votre application. Pour les nouvelles campagnes créées après le 9 janvier2017, ces publicités apparaîtront également dans MSN.com, Outlook.com, Skype et les autres propriétés premium de Microsoft. Les campagnes de promotion d’applications qui ciblent les applications et les propriétés Premium de Microsoft sont appelés campagnes *universelles*.
    * **Community ad (free)**: ces publicités s’exécutent dans les applications publiées par d’autres développeurs, qui créent également des campagnes de publicité de la communauté. Avant de sélectionner cette option, vous devez avoir accepté l’affichage des publicités de la communauté dans la page **Monétiser** -> **Publicités dans l’application**. Pour plus d’informations, consultez l’article [À propos des annonces de la communauté](about-community-ads.md).
    * **House ad (free)**: ces publicités s’exécutent uniquement dans vos applications correspondant au type d’appareil de l’application publiée. Les publicités maison sont gratuites. Pour plus d’informations, consultez l’article [À propos des publicités maison](about-house-ads.md).

7.  Pour les campagnes de publicité payées, vérifiez l’option sélectionnée dans **Durée de la campagne** (période pendant laquelle le budget de votre campagne sera dépensé). L’option par défaut est **Mensuelle**, ce qui signifie que votre budget de campagne sera utilisé tous les mois de manière récurrente jusqu’à l’arrêt de la campagne. Si vous disposez d’un compte Premium, vous pouvez éventuellement choisir l’option **Personnalisée** pour spécifier une date personnalisée et un intervalle de temps pendant lequel votre budget de campagne sera dépensé. Pour plus d’informations sur les comptes Premium, consultez l’article [Questions courantes sur les campagnes de publicité](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Vérifiez vos informations de budget et de paiement. (Si vous créez une campagne maison ou une campagne de publicité de la communauté, ces options n’apparaîtront pas, car ces campagnes sont gratuites.)
    * Sous **Budget**, utilisez le curseur pour définir la somme que vous souhaitez dépenser chaque mois pour exécuter la publicité (ou le budget total, si vous avez sélectionné une durée de campagne personnalisée).

        Le budget mensuel est calculé au prorata du mois au cours duquel vous avez créé la campagne de publicité. En d’autres termes, si vous créez une campagne au milieu du mois, vous êtes facturé la moitié du budget mensuel fixé pour le mois concerné.

    * Spécifiez un mode de paiement pour votre campagne de publicité en cliquant sur **Ajouter un mode de paiement**, puis renseignez les détails de votre compte. Si vous avez déjà fourni un instrument de paiement, vous pouvez sélectionner l’option **Choose a different payment method** si vous devez mettre à jour cette information. Le pays ou la région de l’adresse de facturation de votre mode de paiement doit correspondre au pays ou à la région associés à votre compte du Centre de développement.

    * Si vous avez reçu un coupon d’un représentant Microsoft pour payer une campagne de publicité, cliquez sur **Utiliser un coupon**, entrez le code du coupon, puis cliquez sur **Appliquer** pour appliquer le coupon à la campagne.

    Lorsque vous avez terminé, cliquez sur **Enregistrer et Suivant** afin de poursuivre à l’étape **Audience**. Cette étape n’est pas disponible pour les campagnes de publicité maison, dans la mesure où elles s’exécutent uniquement dans vos propres applications.

9.  Dans la page **Audience**, nous affichons les paramètres d’audience que nous recommandons pour votre campagne. Si vous le souhaitez, vous pouvez ajuster ces informations:
    * **Pays/régions**: choisissez jusqu’à 5pays ou régions dans lesquels vous souhaitez faire apparaître votre publicité. Pour obtenir la liste des régions ou pays pris en charge, consultez l’article [Questions courantes sur les campagnes de publicité](common-questions.md#where-will-my-ad-appear).

    * **Appareils**: choisissez les types d’appareils sur lesquels vous voulez que ces publicités apparaissent. Seuls les types d’appareils pris en charge par votre application sont affichés.

    * **Surface**: choisissez **Universelle** si vous voulez que votre publicité s’affiche dans les applications, ainsi que dans MSN.com, Outlook.com, Skype et les autres propriétés Premium de Microsoft. Si vous préférez que votre publicité apparaisse uniquement dans les applications, choisissez **Application**.

    * **Système d’exploitation**: choisissez le ou les systèmes d’exploitation sur lesquels votre publicité doit apparaître. Seuls les systèmes d’exploitation pris en charge par votre application sont affichés.

    * **Sexe**: déterminez si vous souhaitez restreindre l’audience de votre publicité à un sexe spécifique.

    * **Tranche d’âge**: sélectionnez la ou les tranches d’âge de l’audience ciblée.

    Cette section affiche également un graphique **Portée estimée**. Ce graphique présente l’audience que vous pouvez envisager d’atteindre avec les choix de ciblage en cours. Il s’agit d’un pourcentage de l’ensemble des utilisateurs d’applications Windows affichant des publicités sur les marchés sélectionnés.

10.  Si vous avez choisi **Augmenter l’intérêt pour votre application** comme objectif de campagne, vous pouvez sélectionner l’un des segments de clients à cibler. Les publicités créées à l’aide de cette campagne sont vues seulement par les clients qui sont inclus dans le segment. Vous ne pouvez sélectionner qu’un segment par campagne de publicité. Pour plus d’informations sur les segments, consultez l’article [Créer des segments de clients](create-customer-segments.md). Lorsque vous avez terminé, cliquez sur **Enregistrer et Suivant** afin de poursuivre à l’étape **Conception d’annonces**. Cette étape n’est pas disponible pour les campagnes de publicité maison, car elles s’exécutent uniquement dans vos propres applications.

11.  Dans la page **Conception d’annonces**, choisissez l’une des options suivantes:
    * **Auto-généré**. Il s’agit de l’option par défaut, et il vous permet de créer une publicité à partir de nos modèles par défaut. Vous pouvez effectuer des sélections pour personnaliser le contenu de votre application, et nous afficherons alors un aperçu de l’aspect de votre publicité en fonction de vos choix (automatiquement mis à jour dès que vous effectuez des sélections).
        * Dans la liste déroulante **Langue**, sélectionnez la langue de votre publicité. Le texte du badge MicrosoftStore s’affichera dans la langue que vous avez sélectionnée.
        * Pour ajouter une ligne de texte supplémentaire à votre publicité, entrez ce texte dans le champ **Slogan personnalisé**.
            > [!NOTE]
            > Le texte que vous entrez ici doit être localisé dans la langue sélectionnée. Si le texte n’est pas conforme aux [politiques Bing Ads](http://go.microsoft.com/fwlink?LinkId=398341), le slogan personnalisé sera rejeté. Lisez cette page pour obtenir des conseils sur le style et les contenus non autorisés.
        * Pour personnaliser l’annonce davantage, développez **Personnaliser la conception d’annonce/Voir toutes les tailles d’annonces** et choisissez l’une des options suivantes:
            * **Couleur d’arrière-plan**. Choisissez parmi les options disponibles.
            * **Images**. Choisissez l’une des images disponibles (issues de la description de votre application dans le WindowsStore).
            * **Afficher la notation de mon application**. Activez cette case à cocher si vous souhaitez afficher la notation de l’application.
            * **Indiquer que mon application est gratuite**. Si votre application est gratuite sur l’ensemble des marchés sélectionnés, vous avez la possibilité de cocher cette case.
            * **Appel à l’action**. Si vous avez choisi **Augmenter l’intérêt pour votre application** comme objectif de campagne, vous pouvez définir le bouton d’appel à l’action de votre annonce sur **Ouvrir**, **Lecture**, **Lire**, **Écouter** ou **Acheter**.  

    * **Personnalisée**. Choisissez cette option pour utiliser vos propres conceptions de publicités. Notez que si vous avez sélectionné un segment de clientèle précédemment, vous devez utiliser des options créatives personnalisées. Vous pouvez charger des fichiers distincts pour chacune des tailles de publicité disponibles. Les fichiers doivent répondre aux exigences et directives suivantes:
        * Chaque fichier doit être un .png ou .jpg inférieur ou égal à 40Ko.
        * Vos conceptions d’annonces doivent respecter les critères spécifiés dans la [Microsoft Creative Acceptance Policy](http://go.microsoft.com/fwlink?LinkId=532595).
        * Le contenu de vos conceptions d’annonces doit être approprié à l’application dont vous faites la promotion. Les conceptions d’annonces qui ne sont pas liées à l’application ne seront pas distribuées aux publicités au sein des autres applications.
        * Tout le contenu de vos conceptions d’annonces doit être clairement lisible. Par exemple, le contenu ne doit pas être flou, pixellisé ni déformé.

12.  Si vous disposez d’un [compte Premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), vous pouvez utiliser la zone **URL de destination** pour contrôler ce qui se passe lorsqu’un client clique sur votre publicité.
    * Si vous laissez la zone vide, quand un client clique sur votre annonce, la liste de vos applications dans le Windows Store s’affiche.
    * Si vous utilisez Kochava ou Tune pour mesurer les analyses d’installation de votre application, entrez votre URL de suivi d’installation à partir de Kochava ou de Tune. Lorsque vous enregistrez la campagne, l’URL de suivi est validée pour garantir sa résolution dans la page de description de votre application dans le MicrosoftStore. Pour plus d’informations sur le suivi d’installation avec Kochava et Tune, consultez la documentation de [Kochava](http://support.kochava.com/) et de [Tune](https://help.tune.com/).
    * Si vous avez choisi **Augmenter l’intérêt pour votre application** comme objectif de campagne, vous pouvez spécifier un [URI de lien ciblé](../launch-resume/handle-uri-activation.md) pour rediriger les clients du segment sélectionné vers une page spécifique de votre application.
    * Si vous spécifiez une destination qui n’est pas la page de description de votre application ni une page interne à votre application, votre campagne est automatiquement suspendue.

13.  Enfin, cliquez sur **Révision** pour vérifier les paramètres de votre campagne de publicité et pour vérifier ses informations de budget et de paiement s’il s’agit d’une campagne de publicité payante. Cliquez sur **Confirmer**. Dans les heures qui suivent, vos annonces s’afficheront progressivement sur les appareils.

## <a name="review-ad-campaign-performance"></a>Vérifier les performances des campagnes de publicité

Pour vérifier l’efficacité de vos campagnes, revenez à la page **Campagnes de publicité**. Sélectionnez **Filtres de section** pour limiter les éléments inclus dans le rapport par **Date**, **Objectif de campagne**, **Nom d’application**, **Type de campagne** ou **État**. Outre l’accès à des informations sur les **impressions**, les **clics**, les **conversions** et les **dépenses** de votre campagne, vous pouvez utiliser le rapport pour **interrompre** ou **reprendre** une campagne. Pour plus d’informations, consultez l’article [Rapport de campagne de publicité](promote-your-app-report.md).

Pour modifier une campagne, sélectionnez son nom dans la liste.

## <a name="related-topics"></a>Rubriques connexes

* [Gestion de votre campagne de publicité](managing-your-ad-campaign.md)
* [À propos des publicités maison](about-house-ads.md)
* [Rapport Promouvoir votre application](promote-your-app-report.md)
* [Questions courantes sur les campagnes de publicité](common-questions.md)
