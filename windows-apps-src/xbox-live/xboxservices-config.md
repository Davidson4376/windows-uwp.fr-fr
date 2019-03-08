---
title: XboxServices.config
description: Décrit le fichier XboxServices.config permettant d’associer votre jeu UWP avec une configuration de Xbox Live.
ms.date: 03/29/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, configuration du service, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606684"
---
# <a name="xboxservicesconfig-file-description"></a>Description du fichier XboxServices.config

Lorsque vous développez une Xbox Live activé jeu UWP, votre projet doit inclure un fichier XboxServices.config.  Ce fichier permet de Xbox Live SDK associer votre jeu avec votre application de partenaires et de la configuration de vos services Xbox Live. Ce fichier contient un objet JSON de ces informations plus d’informations, notamment l’ID de configuration de service, un ID de titre, un périphérique.

Si vous utilisez Unity pour concevoir un jeu de programme Xbox Live Creators à l’aide de la Xbox Live plug-in, ce fichier est automatiquement créé pour vous par l’Assistant Association de Xbox Live.

## <a name="xboxservicesconfig-fields"></a>XboxServices.config fields

>[!NOTE]
> Le fichier créé par l’Assistant Association de Xbox Live peut inclure des champs supplémentaires au-delà de celles décrites ci-dessous, mais ils ne sont pas utilisés par le service.

Les champs suivants sont définis dans l’objet JSON dans le fichier de configuration :

Champ | Description
--- | ---
PrimaryServiceConfigId  |  La configuration du service Xbox Live ID (SCID). Dans [partenaires](https://partner.microsoft.com/dashboard), vous pouvez trouver cette valeur sur le **Xbox Live** page (pour les créateurs de programme) ou **le programme d’installation de Xbox Live** page (pour complètes Xbox Live games), sous la **Services** section pour votre application.
TitleId  |  L’ID de titre décimal pour votre application. Dans [partenaires](https://partner.microsoft.com/dashboard), vous pouvez trouver cette valeur sur le **Xbox Live** page (pour les créateurs de programme) ou **le programme d’installation de Xbox Live** page (pour complètes Xbox Live games), sous la **Services** section pour votre application.
XboxLiveCreatorsTitle  |  Si « true », indique que l’application est une application de programme Xbox Live Creators. Sinon, « false ».
Étendue  |  **(Facultatif)**  Définit l’étendue de fonctionnalités utilisées par l’application. Voir ci-dessous pour une description approfondie.

### <a name="scope-field"></a>Champ de portée

Le **étendue** champ est un champ facultatif que vous pouvez utiliser pour indiquer les fonctionnalités utilisées par votre jeu.


Si le **étendue** champ n’est pas spécifié, puis l’étendue est définie sur une valeur par défaut qui dépend de la valeur de la **XboxLiveCreatorsTitle** champ, comme décrit dans le tableau suivant :

XboxLiveCreatorsTitle value | Valeur d’étendue par défaut
--- | ---
« true »  |  « xbl.signin xbl.friends »
« false »  |  "xboxlive.signin"



La liste suivante décrit les valeurs valides pour le **étendue** champ.

Valeur d’étendue | Description
--- | ---
xbl.signin  | Inclut le signe de fonctionnalité pour les jeux de programme Creators. Requis pour les jeux de programme Creators.
xbl.friends | Inclut les amis et les fonctionnalités sociales classements pour les jeux de programme Creators.
xboxlive.signin | Inclut le signe de fonctionnalité pour les jeux qui accèdent à toutes les fonctionnalités de Xbox Live. Requis pour les jeux de programme non - Creators.

Actuellement, la seule raison pour spécifier le **étendue** champ est si vous effectuez un programme Xbox Live Creators jeu et que votre jeu n’a pas besoin accéder aux listes d’amis ou leaderboards social (tableaux de résultats qui est limitées à vos amis). Si c’est le cas, vous pouvez ajouter la ligne suivante à votre fichier XboxServices.config :

```
  "Scope" : "xbl.signin"
```

Ajout de cette ligne empêche l’application UWP à partir de la demande l’autorisation d’accéder aux listes lorsque vous démarrez l’application pour la première fois.

## <a name="example-xboxservicesconfig-file"></a>Exemple de fichier XboxServices.config

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
