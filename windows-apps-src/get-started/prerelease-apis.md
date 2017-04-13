---
author: drewbat
ms.assetid: 
title: "Développement d’applications UWP à l’aide d’API de version préliminaire"
description: "Découvrez les avantages et les inconvénients de l’utilisation des API de version préliminaire incluses dans les versions d’évaluation des Kit de développement logiciel UWP."
ms.openlocfilehash: ede7e8d5e9cce39850edbdb70a715d76c78f0c01
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-uwp-apps-with-pre-release-apis"></a>Développement d’applications UWP à l’aide d’API de version préliminaire

Microsoft publie des versions d’évaluation du Kit de développement logiciel UWP, mises à profit par les développeurs pour se familiariser avec les nouvelles fonctionnalités de plateforme préalablement à leur finalisation. Cela vous permet d’anticiper l’intégration des fonctionnalités dans votre application et de publier votre application plus tôt, après la publication de la version RTM officielle du Kit de développement logiciel. En utilisant des versions préliminaires d’API, vous êtes dans la position de communiquer un retour à Microsoft, qui peut modifier en fonction le développement des versions futures de la plateforme.

## <a name="important-limitations-on-the-use-of-pre-release-apis"></a>Limitations importantes sur l’utilisation des versions préliminaires d’API
Avant d’utiliser une version préliminaire d’API dans votre application, tenez compte des points essentiels suivants: 
* Les applications qui utilisent des versions préliminaires d’API ne peuvent pas être soumises au Windows Store avant la publication officielle des API dans une version RTM. Nous vous recommandons fortement de conserver le code de développement des versions préliminaires séparé du code de vos applications actuellement publiées. 
* Les applications développées à l’aide d’une version préliminaire de Kit de développement logiciel ne peuvent pas être soumises au Windows Store, même aucune version préliminaire d’API n’est utilisée. Vous avez tout intérêt à installer les outils de versions préliminaires sur un ordinateur ou une machine virtuelle séparés de la machine de production utilisée pour votre développement principal. 
* Les versions préliminaires d’API sont susceptibles d’être modifiées avant la version RTM. Lorsque des API sont incluses dans une version préliminaire de Kit de développement logiciel, il est probable que la fonctionnalité ou le scénario pris en charge seront intégrés dans le Kit de développement logiciel final. Cependant, les noms, les signatures et le comportement des API spécifiques peuvent changer avant la version finale. Les API peuvent également être intégralement supprimées. 

## <a name="how-to-identify-a-prerelease-api"></a>Comment identifier une version préliminaire d’API 
Dans la documentation de référence sur les API pour UWP, les API en version préliminaire sont marquées avec le texte suivant: 

Cet article traite d’une version préliminaire d’API ou de fonctionnalité susceptible d’être modifiée en profondeur avant la publication commerciale. Vous pouvez d’ores et déjà utiliser cette fonctionnalité pour les activités de développement et de test, mais les applications l’employant ne peuvent pas être soumises au Windows Store avant la publication commerciale. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici. Pour plus d’informations sur le développement à l’aide de versions préliminaires d’API, voir la page relative au développement des applications UWP à l’aide de versions préliminaires d’API. 

## <a name="get-the-latest-sdk-insider-preview"></a>Obtenir la dernière version d’évaluation du Kit de développement logiciel du programme Insider 
1. [Inscrivez-vous au programme Windows Insider](https://insider.windows.com/) afin de bénéficier d’un accès aux versions d’évaluation du Kit de développement logiciel. 
3. Avant d’installer la version préliminaire des outils de développement, consultez les [notes de publication relatives au Kit de développement logiciel (SDK) et à l’émulateur Mobile](http://go.microsoft.com/fwlink/?LinkId=829180).
4. Installez [SDK Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).
5. Consultez le [forum de la communauté Windows Insider Preview](http://go.microsoft.com/fwlink/p/?LinkId=507620)
