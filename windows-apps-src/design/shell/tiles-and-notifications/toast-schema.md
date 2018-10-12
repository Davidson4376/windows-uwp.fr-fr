---
author: andrewleader
Description: The following article describes all of the properties and elements within toast content.
title: Schéma du contenu du toast
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad1700d58ab3568aa3aefa46b5950d0a8bf3c320
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536353"
---
# <a name="toast-content-schema"></a>Schéma du contenu de notification toast

 

L’article suivant décrit toutes les propriétés et tous les éléments du contenu toast.

Si vous préférez utiliser du code XML brut plutôt que la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consultez la rubrique [schéma XML](toast-xml-schema.md).

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent est l’objet de premier niveau qui décrit le contenu d’une notification, notamment les éléments visuels, les éléments audio et les actions.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Launch**| chaîne | false | Chaîne transmise à l’application lorsqu’elle est activée par la notification toast. L’application définit le format et le contenu de cette chaîne pour son propre usage. Lorsque l’utilisateur appuie ou clique sur la notification toast pour lancer l’application associée, la chaîne de lancement précise le contexte à l’application pour permettre à cette dernière de présenter à l’utilisateur une vue adaptée au contenu de la notification toast plutôt qu’une vue par défaut. |
| **Visual** | [ToastVisual](#toastvisual) | true | Décrit la partie visuelle de la notification toast. |
| **Actions** | [IToastActions](#itoastactions) | false | Vous pouvez éventuellement créer des actions personnalisées avec des boutons et des entrées. |
| **Audio** | [ToastAudio](#toastaudio) | false | Décrit la partie audio de la notification toast. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Spécifie le type d’activation qui est utilisé lorsque l’utilisateur clique sur le corps de cette notification toast. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Nouveauté de Creators Update: options supplémentaires relatives à l’activation des notifications toast. |
| **Scenario** | [ToastScenario](#toastscenario) | false | Déclare le scénario d’utilisation de votre notification toast, comme une alarme ou un rappel. |
| **DisplayTimestamp** | DateTimeOffset? | false | Nouveauté de Creators Update: remplacez l’horodatage par défaut par un horodatage personnalisé représentant l’heure à laquelle votre notification a réellement été remise, plutôt que l’heure à laquelle la notification a été reçue par la plateforme Windows. |
| **Header** | [ToastHeader](#toastheader) | false | Nouveauté de Creators Update: ajoutez un en-tête personnalisé à votre notification afin de regrouper plusieurs notifications dans le centre de notifications. |


### <a name="toastscenario"></a>ToastScenario
Spécifie le scénario d’utilisation de la notification toast.

| Valeur | Signification |
|---|---|
| **Default** | Le comportement normal de la notification toast. |
| **Reminder** | Une notification de rappel. Elle s’affiche sous sa forme pré-développée et reste sur l’écran de l’utilisateur jusqu’à ce qu’elle soit ignorée. |
| **Alarm** | Une notification d’alarme. Elle s’affiche sous sa forme pré-développée et reste sur l’écran de l’utilisateur jusqu’à ce qu’elle soit ignorée. L’audio est diffusé en boucle par défaut et utilise un son d’alarme. |
| **IncomingCall** | Une notification d’appel entrant. Elle s’affiche sous sa forme pré-développée dans un format spécifique aux appels et reste sur l’écran de l’utilisateur jusqu’à ce qu’elle soit ignorée. L’audio est diffusé en boucle par défaut et utilise une sonnerie. |


## <a name="toastvisual"></a>ToastVisual
La partie visuelle des toasts contient les liaisons, qui contiennent le texte, les images, le contenu adaptatif et bien plus encore.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | La liaison de toast générique qui peut être affichée sur tous les périphériques. Cette liaison est obligatoire et ne peut pas avoir la valeur null. |
| **BaseUri** | Uri | false | Une URL de base par défaut qui est combinée aux URL relatives contenues dans l’attribut source des images. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |
| **Language**| chaîne | false | Les paramètres régionaux cibles de la charge utile visuelle lors de l’utilisation de ressources localisées, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans le texte ou la liaison. Si les paramètres régionaux ne sont pas spécifiés, les paramètres régionaux système sont utilisés à la place. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
La liaison générique est la liaison par défaut pour les notifications toast, et vous permet de spécifier le texte, les images, le contenu adaptatif et bien plus encore.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Enfants** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | Le contenu du corps de la notification toast, qui peut inclure du texte, des images et des groupes (ajouté dans la Mise à jour anniversaire). Les éléments de texte doivent être placés avant tout autre élément, et seuls 3éléments de texte sont pris en charge. Si un élément de texte est placé après un autre élément, il est remonté tout en haut ou supprimé. Enfin, certaines propriétés de texte comme HintStyle ne sont pas prises en charge sur les éléments de texte racine enfants, et fonctionnent uniquement à l’intérieur de AdaptiveSubgroup. Si vous utilisez AdaptiveGroup sur des appareils qui ne disposent pas de la Mise à jour anniversaire, le contenu du groupe sera simplement ignoré. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | Un logo facultatif pour remplacer le logo de l’application. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | Une image «Hero» recommandée facultative qui s’affiche sur la notification toast et dans le centre de notifications. |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | Texte d’attribution facultatif qui s’affiche en bas de la notification toast. |
| **BaseUri** | Uri | false | Une URL de base par défaut qui est combinée aux URL relatives contenues dans l’attribut source des images. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |
| **Language**| chaîne | false | Les paramètres régionaux cibles de la charge utile visuelle lors de l’utilisation de ressources localisées, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans le texte ou la liaison. Si les paramètres régionaux ne sont pas spécifiés, les paramètres régionaux système sont utilisés à la place. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Interface de marqueurs pour les éléments enfants de toast qui incluent du texte, des images, des groupes et d’autres contenus.

| Implémentations |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Un élément de texte adaptatif. S’il est placé dans le ToastBindingGeneric.Children de premier niveau, seul HintMaxLines s’applique. Mais s’il est l’enfant d’un groupe/sous-groupe, les styles de texte sont intégralement pris en charge.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Text** | chaîne ou [BindableString](#bindablestring) | false | Le texte à afficher. Prise en charge de la liaison de données ajoutée dans Creators Update, mais fonctionne uniquement pour les éléments de texte de premier niveau. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | Le style contrôle la taille, l’épaisseur et l’opacité de la police de texte. Fonctionne uniquement pour les éléments de texte placés à l’intérieur d’un groupe/sous-groupe. |
| **HintWrap** | bool? | false | Définissez sur «true» pour activer la fonctionnalité de renvoi de texte à la ligne. Les éléments de texte de premier niveau ignorent cette propriété et renvoient toujours le texte à la ligne (vous pouvez utiliser HintMaxLines = 1 pour désactiver le renvoi à la ligne pour les éléments de texte de premier niveau). Par défaut, les éléments de texte placés à l’intérieur de groupes/sous-groupes ne renvoient pas le texte à la ligne. |
| **HintMaxLines** | int? | false | Le nombre maximum de lignes que l’élément de texte est autorisé à afficher. |
| **HintMinLines** | int? | false | Le nombre minimum de lignes que l’élément de texte doit afficher. Fonctionne uniquement pour les éléments de texte placés à l’intérieur d’un groupe/sous-groupe. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alignement horizontal du texte. Fonctionne uniquement pour les éléments de texte placés à l’intérieur d’un groupe/sous-groupe. |
| **Language** | chaîne | false | Les paramètres régionaux cibles de la charge utile XML, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Les paramètres régionaux spécifiés ici remplacent tous les autres paramètres régionaux spécifiés, comme ceux spécifiés dans la liaison ou l’élément visuel. Si cette valeur est une chaîne littérale, cet attribut a pour valeur par défaut la langue d’interface utilisateur de l’utilisateur. Si cette valeur est une référence de chaîne, cet attribut a pour valeur par défaut les paramètres régionaux choisis par Windows Runtime pour la résolution de la chaîne. |


### <a name="bindablestring"></a>BindableString
Valeur de liaison des chaînes.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **BindingName** | chaîne | true | Obtient ou définit le nom associé à votre valeur de données de liaison. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
Le style de texte contrôle la taille, l’épaisseur et l’opacité de la police. Utilisez une opacité de 60% pour appliquer une opacité subtile.

| Valeur | Signification |
|---|---|
| **Default** | Valeur par défaut. Le style est déterminé par le convertisseur. |
| **Caption** | Inférieur à la taille de police de paragraphe. |
| **CaptionSubtle** | Identique à Caption, mais avec une opacité subtile. |
| **Body** | Taille de police de paragraphe. |
| **BodySubtle** | Identique à Body, mais avec une opacité subtile. |
| **Base** | Taille de police de paragraphe, épaisseur «bold». Essentiellement la version «bold» de Body. |
| **BaseSubtle** | Identique à Base, mais avec une opacité subtile. |
| **Subtitle** | Taille de police de H4. |
| **SubtitleSubtle** | Identique à Subtitle, mais avec une opacité subtile. |
| **Title** | Taille de police de H3. |
| **TitleSubtle** | Identique à Title, mais avec une opacité subtile. |
| **TitleNumeral** | Identique à Title, mais sans les marges intérieures supérieures/inférieures. |
| **Subheader** | Taille de police de H2. |
| **SubheaderSubtle** | Identique à Subheader, mais avec une opacité subtile. |
| **SubheaderNumeral** | Identique à Subheader, mais sans les marges intérieures supérieures/inférieures. |
| **Header** | Taille de police de H1. |
| **HeaderSubtle** | Identique à Header, mais avec une opacité subtile. |
| **HeaderNumeral** | Identique à Header, mais sans les marges intérieures supérieures/inférieures. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Contrôle l’alignement horizontal du texte.

| Valeur | Signification |
|---|---|
| **Default** | Valeur par défaut. L’alignement est automatiquement déterminé par le convertisseur. |
| **Auto** | L’alignement est déterminé en fonction des paramètres de langue et de culture actuels. |
| **Left** | Aligner le texte à gauche. |
| **Center** | Centrer le texte horizontalement. |
| **Right** | Aligner le texte à droite. |


## <a name="adaptiveimage"></a>AdaptiveImage
Une image incorporée.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Source** | chaîne | true | URL de l’image. ms-appx, ms-appdata et http sont pris en charge. À partir de Fall Creators Update, les images Web peuvent atteindre 3Mo sur les connexions normales et 1Mo sur les connexions limitées. Sur les appareils qui n’exécutent pas encore Fall Creators Update, les images web ne doivent pas dépasser 200Ko. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Nouveauté de la Mise à jour anniversaire: contrôlez le rognage de l’image. |
| **HintRemoveMargin** | bool? | false | Par défaut, les images placées à l’intérieur de groupes/sous-groupes ont une marge de 8px autour d’elles. Vous pouvez supprimer cette marge en définissant cette propriété sur «true». |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alignement horizontal de l’image. Fonctionne uniquement pour les images placées à l’intérieur d’un groupe/sous-groupe. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Spécifie le rognage désiré de l’image.

| Valeur | Signification |
|---|---|
| **Default** | Valeur par défaut. Le comportement de rognage est déterminé par le convertisseur. |
| **None** | L’image n’est pas rognée. |
| **Circle** | L’image est rognée en forme de cercle. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Spécifie l’alignement horizontal d’une image.

| Valeur | Signification |
|---|---|
| **Default** | Valeur par défaut. Le comportement d’alignement est déterminé par le convertisseur. |
| **Stretch** | L’image s’étire pour remplir la largeur disponible (et potentiellement la hauteur disponible selon le positionnement de l’image). |
| **Left** | Aligner l’image à gauche, en affichant l’image dans sa résolution native. |
| **Center** | Centrer l’image horizontalement, en affichant l’image dans sa résolution native. |
| **Right** | Aligner l’image à droite, en affichant l’image dans sa résolution native. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Nouveauté de la Mise à jour anniversaire: les groupes indiquent au niveau sémantique si le contenu du groupe doit s’afficher dans son intégralité, ou ne pas s’afficher s’il ne tient pas. Les groupes permettent également de créer plusieurs colonnes.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Enfants** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Les sous-groupes s’affichent en colonnes verticales. Vous devez utiliser des sous-groupes pour ajouter du contenu dans un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Nouveauté de la Mise à jour anniversaire: les sous-groupes sont des colonnes verticales qui peuvent contenir du texte et des images.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Enfants** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) et [AdaptiveImage](#adaptiveimage) sont des enfants valides de sous-groupes. |
| **HintWeight** | int? | false | Contrôler la largeur de cette colonne de sous-groupe en spécifiant la pondération de l’espace relative aux autres sous-groupes. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | Contrôler l’alignement vertical du contenu de ce sous-groupe. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Interface de marqueurs pour les enfants de sous-groupes.

| Implémentations |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking spécifie l’alignement vertical du contenu.

| Valeur | Signification |
|---|---|
| **Default** | Valeur par défaut. Le convertisseur sélectionne automatiquement l’alignement vertical par défaut. |
| **Top** | Aligner vers le haut. |
| **Center** | Centrer verticalement. |
| **Bottom** | Aligner verticalement vers le bas. |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Nouveauté de Creators Update: une barre de progression. Prise en charge uniquement sur les toasts dans Windows Desktop, build15063 ou une version plus récente.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Title** | chaîne ou [BindableString](#bindablestring) | false | Obtient ou définit une chaîne de titre facultative. Prend en charge la liaison des données. |
| **Value** | double ou [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) ou [BindableProgressBarValue](#bindableprogressbarvalue) | false | Obtient ou définit la valeur de la barre de progression. Prend en charge la liaison des données. La valeur par défaut est 0. |
| **ValueStringOverride** | chaîne ou [BindableString](#bindablestring) | false | Obtient ou définit une chaîne facultative à afficher à la place de la chaîne de pourcentage par défaut. Si elle n’est pas fournie, la valeur «70%» s’affiche. |
| **Status** | chaîne ou [BindableString](#bindablestring) | true | Obtient ou définit une chaîne d’état (obligatoire), qui est affichée sous la barre de progression à gauche. Cette chaîne doit refléter l’état de l’opération, par exemple «Téléchargement en cours...» ou «Installation en cours...» |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Classe qui représente la valeur de la barre de progression.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Value** | double | false | Obtient ou définit la valeur (0.0 - 1.0) qui représente le pourcentage effectué. |
| **IsIndeterminate** | bool | false | Obtient ou définit une valeur qui indique si la barre de progression est indéterminée. Si la valeur est true, **Value** sera ignoré. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Valeur de la barre de progression pouvant être liée.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **BindingName** | chaîne | true | Obtient ou définit le nom associé à votre valeur de données de liaison. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Logo à afficher à la place du logo de l’application.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Source** | chaîne | true | URL de l’image. ms-appx, ms-appdata et http sont pris en charge. La taille maximale des images Http est 200Ko. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | Spécifiez le type de rognage que vous souhaitez appliquer à l’image. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Contrôle le rognage de l’image du logo d’application.

| Valeur | Signification |
|---|---|
| **Default** | Le rognage utilise le comportement par défaut du convertisseur. |
| **None** | L’image n’est pas rognée, et s’affiche en carré. |
| **Circle** | L’image est rognée pour former un cercle. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Une image «Hero» recommandée qui s’affiche sur la notification toast et dans le centre de notifications.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Source** | chaîne | true | URL de l’image. ms-appx, ms-appdata et http sont pris en charge. La taille maximale des images Http est 200Ko. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Texte d’attribution qui s’affiche en bas de la notification toast.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Text** | chaîne | true | Le texte à afficher. |
| **Language** | chaîne | false | Les paramètres régionaux cibles de la charge utile visuelle lors de l’utilisation de ressources localisées, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Si les paramètres régionaux ne sont pas spécifiés, les paramètres régionaux système sont utilisés à la place. |


## <a name="itoastactions"></a>IToastActions
Interface de marqueurs pour les actions/entrées de notifications toast.

| Implémentations |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Implémente [IToastActions](#itoastactions)*

Créez vos propres actions et entrées personnalisées, à l’aide de contrôles comme des boutons, des zones de texte et des entrées de sélection.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Entrées** | IList<[IToastInput](#itoastinput)> | false | Entrées comme des zones de texte et des entrées de sélection. Vous pouvez ajouter jusqu’à 5entrées. |
| **Boutons** | IList<[IToastButton](#itoastbutton)> | false | Les boutons s’affichent après toutes les entrées (ou à côté d’une entrée si le bouton est utilisé comme bouton de réponse rapide). Vous pouvez ajouter jusqu’à 5boutons (moins s’il y a aussi des éléments de menu contextuel). |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Nouveauté de la Mise à jour anniversaire: des éléments de menu contextuel personnalisés, proposant des actions supplémentaires si l’utilisateur clique avec le bouton droit sur la notification. Vous pouvez *combiner* jusqu’à 5boutons et éléments de menu contextuel. |


## <a name="itoastinput"></a>IToastInput
Interface de marqueurs pour les entrées de notification toast.

| Implémentations |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Implémente [IToastInput](#itoastinput)*

Un contrôle de zone de texte dans lequel l’utilisateur peut taper du texte.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Id** | chaîne | true | L’Id est obligatoire, et est utilisé pour mapper le texte saisi par l’utilisateur dans une paire clé-valeur d’id/valeur que votre application utilisera plus tard. |
| **Title** | chaîne | false | Texte de titre qui s’affiche au-dessus de la zone de texte. |
| **PlaceholderContent** | chaîne | false | Texte d’espace réservé qui s’affiche dans la zone de texte tant l’utilisateur n’a pas encore tapé de texte. |
| **DefaultInput** | chaîne | false | Le texte initial à placer dans la zone de texte. Laissez cette valeur sur «null» pour une zone de texte vide. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Implémente [IToastInput](#itoastinput)*

Un contrôle de zone de sélection, qui permet aux utilisateurs de choisir dans une liste déroulante d’options.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Id** | chaîne | true | L’Id est obligatoire. Si l’utilisateur sélectionne cet élément, cet Id est transmis au code de votre application, en indiquant leur choix. |
| **Content** | chaîne | true | Content est obligatoire, et est la chaîne qui s’affiche sur l’élément de sélection. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Un élément de zone de sélection (un élément que l’utilisateur peut sélectionner dans la liste déroulante).

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Id** | chaîne | true | L’Id est obligatoire, et est utilisé pour mapper le texte saisi par l’utilisateur dans une paire clé-valeur d’id/valeur que votre application utilisera plus tard. |
| **Title** | chaîne | false | Texte de titre qui s’affiche au-dessus de la zone de sélection. |
| **DefaultSelectionBoxItemId** | chaîne | false | Contrôle quel élément est sélectionné par défaut, et fait référence à la propriété Id du [ToastSelectionBoxItem](#toastselectionboxitem). Si vous ne spécifiez pas ce paramètre, la sélection par défaut sera vide (l’utilisateur ne verra rien). |
| **Éléments** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Les éléments de sélection que l’utilisateur peut sélectionner à partir de cette SelectionBox. Vous pouvez ajouter jusqu’à 5éléments. |


## <a name="itoastbutton"></a>IToastButton
Interface de marqueurs pour les boutons de notification toast.

| Implémentations |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Implémente [IToastButton](#itoastbutton)*

Un bouton sur lequel l’utilisateur peut cliquer.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Content** | chaîne | true | Obligatoire. Texte à afficher sur le bouton. |
| **Arguments** | chaîne | true | Obligatoire. Chaîne d’arguments définie par l’application, que l’application récupère quand l’utilisateur clique sur ce bouton. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Contrôle le type d’activation qu’utilise ce bouton lorsque l’utilisateur clique dessus. La valeur par défaut est Foreground. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Nouveauté de Creators Update: obtient ou définit des options supplémentaires relatives à l’activation du bouton du toast. |


### <a name="toastactivationtype"></a>ToastActivationType
Détermine le type d’activation utilisé quand l’utilisateur interagit avec une action spécifique.

| Valeur | Signification |
|---|---|
| **Foreground** | Valeur par défaut. Votre application se lance au premier plan. |
| **Background** | La tâche en arrière-plan correspondante (en supposant que vous avez tout configuré) est déclenchée, et vous pouvez exécuter du code en arrière-plan (comme envoyer le message de réponse rapide de l’utilisateur) sans interrompre l’utilisateur. |
| **Protocol** | Lancer une autre application en utilisant l’activation de protocole. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Nouveauté de Creators Update: options supplémentaires relatives à l’activation.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Nouveauté de Fall Creators Update: obtient ou définit le comportement que le toast doit utiliser lorsque l’utilisateur appelle cette action. Il ne fonctionne que sur Windows Desktop, pour [ToastButton](#toastbutton) et [ToastContextMenuItem](#toastcontextmenuitem). |
| **ProtocolActivationTargetApplicationPfn** | chaîne | false | Si vous utilisez *ToastActivationType.Protocol*, vous pouvez spécifier le PFN cible, afin que votre application se lance toujours quel que soit le nombre d’applications inscrites pour gérer le même uri de protocole. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Spécifie le comportement que le toast doit utiliser lorsque l’utilisateur effectue une action sur le toast.

| Valeur | Signification |
|---|---|
| **Default** | Comportement par défaut. Le toast est ignoré lorsque l’utilisateur effectue une action sur le toast. |
| **PendingUpdate** | Une fois que l’utilisateur clique sur un bouton de votre toast, la notification reste présente à l’état visuel «en attente de mise à jour». Vous devez immédiatement mettre à jour votre toast à partir d’une tâche en arrière-plan afin que cet état visuel «en attente de mise à jour» ne s’affiche pas trop longtemps. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Implémente [IToastButton](#itoastbutton)*

Un bouton Répéter géré par le système qui gère automatiquement la répétition de la notification.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **CustomContent** | chaîne | false | Texte personnalisé facultatif affiché sur le bouton qui remplace le texte «Répéter» localisé par défaut. |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Implémente [IToastButton](#itoastbutton)*

Un bouton Ignorer géré par le système qui permet d’ignorer la notification lorsque l’utilisateur clique dessus.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **CustomContent** | chaîne | false | Texte personnalisé facultatif affiché sur le bouton qui remplace le texte «Ignorer» localisé par défaut. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*Implémente [IToastActions](#itoastactions)

Crée automatiquement une zone de sélection pour les intervalles de répétition, et des boutons Répéter/Ignorer, tous automatiquement localisés. La logique de répétition est gérée automatiquement par le système.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Nouveauté de la Mise à jour anniversaire: des éléments de menu contextuel personnalisés, proposant des actions supplémentaires si l’utilisateur clique avec le bouton droit sur la notification. Vous pouvez ajouter jusqu’à 5éléments. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Une entrée d’élément de menu contextuel.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Content** | chaîne | true | Obligatoire. Le texte à afficher. |
| **Arguments** | chaîne | true | Obligatoire. Chaîne d’arguments définie par l’application, que l’application peut récupérer une fois qu’elle est activée quand l’utilisateur clique sur l’élément de menu. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Contrôle le type d’activation qu’utilise cet élément de menu lorsque l’utilisateur clique dessus. La valeur par défaut est Foreground. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Nouveauté de Creators Update: options supplémentaires relatives à l’activation des éléments de menu contextuel de notification toast. |


## <a name="toastaudio"></a>ToastAudio
Spécifiez le son à lire à la réception de la notification toast.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Src** | uri | false | Le fichier multimédia à lire à la place du son par défaut. Seuls ms-appx et ms-appdata sont pris en charge. |
| **Loop** | booléen | false | Définissez sur «true» si le son doit être répété tant que la notification toast est affichée; sur «false» pour lire une seule fois (par défaut). |
| **Silent** | booléen | false | Définissez sur «true» pour désactiver le son; sur «false» pour autoriser la lecture du son de la notification toast (par défaut). |


## <a name="toastheader"></a>ToastHeader
Nouveauté de Creators Update: un en-tête personnalisé qui regroupe plusieurs notifications dans le centre de notifications.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Id** | chaîne | true | Un identificateur créé par un développeur qui identifie spécifiquement cet en-tête. Si deux notifications possèdent le même id d’en-tête, elles s’affichent sous le même en-tête dans le centre de notifications. |
| **Title** | chaîne | true | Un titre pour l’en-tête. |
| **Arguments**| chaîne | true | Obtient ou définit une chaîne d’arguments définie par le développeur qui est retournée à l’application lorsque l’utilisateur clique sur cet en-tête. Ne peut pas être null. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Obtient ou définit le type d’activation que cet en-tête utilise lorsque l’utilisateur clique dessus. La valeur par défaut est Foreground. Notez que seules les valeurs Foreground et Protocol sont prises en charge. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Obtient ou définit des options supplémentaires relatives à l’activation de l’en-tête du toast. |


## <a name="related-topics"></a>Rubriquesassociées

* [Démarrage rapide: Envoyer une notification toast et gérer l’activation](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [Bibliothèque Notifications sur GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)