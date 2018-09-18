---
author: andrewleader
Description: The following article describes all of the properties and elements within tile content.
title: Schéma du contenu de la vignette
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 07/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, vignette, notification par vignette, contenu de vignette, schéma, charge utile de vignette
ms.localizationpriority: medium
ms.openlocfilehash: d2baa2e2d7b8d68505159eb480ea3be78750f507
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4020669"
---
# <a name="tile-content-schema"></a>Schéma du contenu de la vignette

 

L’article suivant décrit toutes les propriétés et tous les éléments du contenu de la vignette.

Si vous préférez utiliser du code XML brut plutôt que la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consultez la rubrique [schéma XML](../tiles-and-notifications/adaptive-tiles-schema.md).

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#TileBindingContentAdaptive)
    * [TileBindingContentIconic](#TileBindingContentIconic)
    * [TileBindingContentContact](#TileBindingContentContact)
    * [TileBindingContentPeople](#TileBindingContentPeople)
    * [TileBindingContentPhotos](#TileBindingContentPhotos)


## <a name="tilecontent"></a>TileContent
TileContent est l’objet de premier niveau qui décrit le contenu d’une notification par vignette, notamment les éléments visuels.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Élément visuel** | [ToastVisual](#tilevisual) | true | Décrit la partie visuelle de la notification par vignette. |


## <a name="tilevisual"></a>TileVisual
La partie visuelle des vignettes contient les spécifications visuelles de toutes les tailles de vignette et plus de propriétés visuelles.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | Fournit une petite liaison facultative pour spécifier le contenu de la petite taille de vignette. |
| **TileMedium** | [TileBinding](#tilebinding) | false | Fournit une liaison moyenne facultative pour spécifier le contenu de la taille de vignette moyenne. |
| **TileWide** | [TileBinding](#tilebinding) | false | Fournit une liaison large facultative pour spécifier le contenu de la taille de vignette large. |
| **TileLarge** | [TileBinding](#tilebinding) | false | Fournit une grande liaison facultative pour spécifier le contenu de la grande taille de vignette. |
| **Branding** | [TileBranding](#tilebranding) | false | La forme que la vignette doit utiliser pour afficher la marque de l’application. Par défaut, hérite de la personnalisation de la vignette par défaut. |
| **DisplayName** | chaîne | false | Chaîne facultative pour remplacer le nom d’affichage de la vignette en affichant cette notification. |
| **Arguments** | chaîne | false | Nouveauté dans la mise à jour anniversaire: données définies par l’application qui sont retransmises à votre application via la propriété TileActivatedInfo sur LaunchActivatedEventArgs lorsque l’utilisateur lance votre application à partir de la vignette dynamique. Cela vous permet de savoir quelles sont les notifications de vignette que votre utilisateur a vues lorsqu'il a appuyé sur votre Vignette dynamique. Sur les appareils non équipés de la mise à jour anniversaire, cela sera simplement ignoré. |
| **LockDetailedStatus1** | chaîne | false | Si vous spécifiez cette option, vous devez également fournir une liaison TileWide. Il s’agit de la première ligne de texte qui s’affiche sur l’écran de verrouillage si l’utilisateur a sélectionné votre vignette comme son application d’état détaillée. |
| **LockDetailedStatus2** | chaîne | false | Si vous spécifiez cette option, vous devez également fournir une liaison TileWide. Il s’agit de la deuxième ligne de texte qui s’affiche sur l’écran de verrouillage si l’utilisateur a sélectionné votre vignette comme son application d’état détaillée. |
| **LockDetailedStatus3** | chaîne | false | Si vous spécifiez cette option, vous devez également fournir une liaison TileWide. Il s’agit de la troisième ligne de texte qui s’affiche sur l’écran de verrouillage si l’utilisateur a sélectionné votre vignette comme son application d’état détaillée. |
| **BaseUri** | Uri | false | Une URL de base par défaut qui est combinée aux URL relatives contenues dans l’attribut source des images. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |
| **Language**| chaîne | false | Les paramètres régionaux cibles de la charge utile visuelle lors de l’utilisation de ressources localisées, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans le texte ou la liaison. Si les paramètres régionaux ne sont pas spécifiés, les paramètres régionaux système sont utilisés à la place. |


## <a name="tilebinding"></a>TileBinding
L’objet de liaison contient le contenu visuel pour une taille de vignette spécifique.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Contenu** | [ITileBindingContent](#itilebindingcontent) | false | Le contenu visuel à afficher sur la vignette. Un des [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#TileBindingContentIconic), [TileBindingContentContact](#TileBindingContentContact), [TileBindingContentPeople](#TileBindingContentPeople) ou [TileBindingContentPhotos](#TileBindingContentPhotos). |
| **Branding** | [TileBranding](#tilebranding) | false | La forme que la vignette doit utiliser pour afficher la marque de l’application. Par défaut, hérite de la personnalisation de la vignette par défaut. |
| **DisplayName** | chaîne | false | Chaîne facultative pour remplacer le nom d’affichage de la vignette pour cette taille de vignette. |
| **Arguments** | chaîne | false | Nouveauté dans la mise à jour anniversaire: données définies par l’application qui sont retransmises à votre application via la propriété TileActivatedInfo sur LaunchActivatedEventArgs lorsque l’utilisateur lance votre application à partir de la vignette dynamique. Cela vous permet de savoir quelles sont les notifications de vignette que votre utilisateur a vues lorsqu'il a appuyé sur votre Vignette dynamique. Sur les appareils non équipés de la mise à jour anniversaire, cela sera simplement ignoré. |
| **BaseUri** | Uri | false | Une URL de base par défaut qui est combinée aux URL relatives contenues dans l’attribut source des images. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |
| **Language**| chaîne | false | Les paramètres régionaux cibles de la charge utile visuelle lors de l’utilisation de ressources localisées, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans le texte ou la liaison. Si les paramètres régionaux ne sont pas spécifiés, les paramètres régionaux système sont utilisés à la place. |


## <a name="itilebindingcontent"></a>ITileBindingContent
Interface de marqueurs pour le contenu de liaison de la vignette. Ils vous permettent de choisir en quoi vous souhaitez spécifier les éléments visuels de votre vignette: adaptatif, ou un des modèles spéciaux.

| Implémentations |
| --- |
| [TileBindingContentAdaptive](#TileBindingContentAdaptive) |
| [TileBindingContentIconic](#TileBindingContentIconic) |
| [TileBindingContentContact](#TileBindingContentContact) |
| [TileBindingContentPeople](#TileBindingContentPeople) |
| [TileBindingContentPhotos](#TileBindingContentPhotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
Pris en charge dans toutes les tailles. Il s’agit de la méthode recommandée pour spécifier le contenu de votre vignette. Les modèles de vignette adaptative sont une nouveauté de Windows10 et vous pouvez créer une grande diversité de vignettes personnalisées au moyen des modèles adaptatifs.

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Children** | IList<[ITileBindingContentAdaptiveChild](#ITileBindingContentAdaptiveChild)> | false | Les éléments visuels en ligne. Des objets [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage) et [AdaptiveGroup](#adaptivegroup) peuvent être ajoutés. Les enfants sont affichés sous forme de StackPanel vertical. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | Une image d’arrière-plan facultatif qui est affichée derrière tout le contenu de la vignette, en pleine page. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | Une image furtive facultative qui s'anime à partir du haut de la vignette. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | Contrôle l’empilement de texte (alignement vertical) du contenu des enfants dans son intégralité. |


## <a name="adaptivetext"></a>AdaptiveText
Un élément de texte adaptatif.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Text** | chaîne | false | Le texte à afficher. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | Le style contrôle la taille, l’épaisseur et l’opacité de la police de texte. |
| **HintWrap** | bool? | false | Définissez sur «true» pour activer la fonctionnalité de renvoi de texte à la ligne. La valeur par défaut est false. |
| **HintMaxLines** | int? | false | Le nombre maximum de lignes que l’élément de texte est autorisé à afficher. |
| **HintMinLines** | int? | false | Le nombre minimum de lignes que l’élément de texte doit afficher. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alignement horizontal du texte. |
| **Language** | chaîne | false | Les paramètres régionaux cibles de la charge utile XML, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Les paramètres régionaux spécifiés ici remplacent tous les autres paramètres régionaux spécifiés, comme ceux spécifiés dans la liaison ou l’élément visuel. Si cette valeur est une chaîne littérale, cet attribut a pour valeur par défaut la langue d’interface utilisateur de l’utilisateur. Si cette valeur est une référence de chaîne, cet attribut a pour valeur par défaut les paramètres régionaux choisis par Windows Runtime pour la résolution de la chaîne. |


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
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Contrôle le rognage désiré de l’image. |
| **HintRemoveMargin** | bool? | false | Par défaut, les images placées à l’intérieur de groupes/sous-groupes ont une marge de 8px autour d’elles. Vous pouvez supprimer cette marge en définissant cette propriété sur «true». |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alignement horizontal de l’image. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification par vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


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
Les groupes indiquent au niveau sémantique si le contenu du groupe doit s’afficher dans son intégralité, ou ne pas s’afficher s’il ne tient pas. Les groupes permettent également de créer plusieurs colonnes.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Enfants** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Les sous-groupes s’affichent en colonnes verticales. Vous devez utiliser des sous-groupes pour ajouter du contenu dans un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Les sous-groupes sont des colonnes verticales qui peuvent contenir du texte et des images.

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


## <a name="tilebackgroundimage"></a>TileBackgroundImage
Une image d’arrière-plan affichée en pleine page sur la vignette.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Source** | chaîne | true | URL de l’image. ms-appx, ms-appdata et http(s) sont pris en charge. La taille maximale des images Http est 200Ko. |
| **HintOverlay** | int? | false | Une superposition noire sur l'image d’arrière-plan. Cette valeur contrôle l’opacité de la superposition noire, 0 étant aucune superposition et 100 complètement noire. Défini par défaut sur 20. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | Nouveauté de la version1511: spécifiez le type de rognage que vous souhaitez appliquer à l’image. Dans les versions antérieures à1511, cela sera ignoré et l'image d’arrière-plan s’affichera sans rognage. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification par vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
Contrôle le rognage de l’image d'arrière-plan.

| Valeur | Signification |
|---|---|
| **Default** | Le rognage utilise le comportement par défaut du convertisseur. |
| **None** | L’image n’est pas rognée, et s’affiche en carré. |
| **Circle** | L’image est rognée pour former un cercle. |


## <a name="tilepeekimage"></a>TilePeekImage
Une image furtive qui s'anime à partir du haut de la vignette.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Source** | chaîne | true | URL de l’image. ms-appx, ms-appdata et http(s) sont pris en charge. La taille maximale des images Http est 200Ko. |
| **HintOverlay** | int? | false | Nouveauté de la version1511: une superposition noire sur l’image furtive. Cette valeur contrôle l’opacité de la superposition noire, 0 étant aucune superposition et 100 complètement noire. Défini par défaut sur 20. Dans les versions précédentes, cette valeur sera ignorée et l'image furtive s’affichera avec la superposition 0. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | Nouveauté de la version1511: spécifiez le type de rognage que vous souhaitez appliquer à l’image. Dans les versions antérieures à1511, cela sera ignoré et l'image furtive s’affichera sans rognage. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification par vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
Contrôle le rognage de l’image furtive.

| Valeur | Signification |
|---|---|
| **Default** | Le rognage utilise le comportement par défaut du convertisseur. |
| **None** | L’image n’est pas rognée, et s’affiche en carré. |
| **Circle** | L’image est rognée pour former un cercle. |


### <a name="tiletextstacking"></a>TileTextStacking
L'empilement de texte spécifie l’alignement vertical du contenu.

| Valeur | Signification |
|---|---|
| **Default** | Valeur par défaut. Le convertisseur sélectionne automatiquement l’alignement vertical par défaut. |
| **Top** | Aligner vers le haut. |
| **Center** | Centrer verticalement. |
| **Bottom** | Aligner verticalement vers le bas. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Pris en charge sur les petites et moyennes. Active un modèle de vignette Icône, où vous pouvez afficher une icône et un badge l'un à côté de l'autre sur la vignette, dans le vrai style classique de Windows Phone. Le numéro en regard de l’icône est obtenu par le biais d’une notification de badge distinct.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Icône** | [TileBasicImage](#tilebasicimage) | true | Au minimum, pour prendre en charge à la fois les vignettes Bureau et mobiles, petites et moyennes, fournissez une image de forme carrée d'une résolution de 200x200, au format PNG, transparente et sans autre couleur que le blanc. Pour plus d’informations, voir: [Modèles de vignette spéciaux](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
Mobile uniquement. Pris en charge sur les petites, moyennes et larges.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Image** | [TileBasicImage](#tilebasicimage) | true | Image à afficher. |
| **Texte** | [TileBasicText](#tilebasictext) | false | Une ligne de texte qui s’affiche. Non affiché sur une petite vignette. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
Nouveauté de la version1511: pris en charge sur moyenne, large et grande (bureau et mobile). Précédemment réservé aux mobiles et uniquement moyennes et larges.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | Images qui s'enrouleront en cercles. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
S'anime via un diaporama de photos. Pris en charge dans toutes les tailles.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | Jusqu'à 12images peuvent être fournies (Mobile en affichera uniquement jusqu'à 9), qui seront utilisées pour le diaporama. Si vous en ajoutez plus de 12, cela lève une exception. |


### <a name="tilebasicimage"></a>TileBasicImage
Image utilisée dans les différents modèles spéciaux.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Source** | chaîne | true | URL de l’image. ms-appx, ms-appdata et http(s) sont pris en charge. La taille maximale des images Http est 200Ko. |
| **AlternateText** | chaîne | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | bool? | false | Définissez sur «true» pour permettre à Windows d’ajouter une chaîne de requête à l’URL d’image fournie dans la notification par vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer les chaînes de requête, en récupérant une variante d’image avec les chaînes de requête, ou en ignorant la chaîne de requête et en retournant l’image comme spécifié sans la chaîne de requête. Cette chaîne de requête spécifie l’échelle, le paramètre de contraste et la langue; par exemple, la valeur «www.siteweb.com/images/hello.png» fournie dans la notification devient «www.siteweb.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=fr-fr». |


### <a name="tilebasictext"></a>TileBasicText
Un élément de texte de base utilisé sur différents modèles spéciaux.

| Propriété | Type | Requis |Description |
|---|---|---|---|
| **Text** | chaîne | false | Le texte à afficher. |
| **Language** | chaîne | false | Les paramètres régionaux cibles de la charge utile XML, spécifiés en balises de langue BCP-47 comme «en-US» ou «fr-FR». Les paramètres régionaux spécifiés ici remplacent tous les autres paramètres régionaux spécifiés, comme ceux spécifiés dans la liaison ou l’élément visuel. Si cette valeur est une chaîne littérale, cet attribut a pour valeur par défaut la langue d’interface utilisateur de l’utilisateur. Si cette valeur est une référence de chaîne, cet attribut a pour valeur par défaut les paramètres régionaux choisis par Windows Runtime pour la résolution de la chaîne. |


## <a name="related-topics"></a>Rubriques associées

* [Démarrage rapide: Envoyer une notification par vignette locale](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Bibliothèque Notifications sur GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)