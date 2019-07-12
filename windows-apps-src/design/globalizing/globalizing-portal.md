---
Description: En savoir plus sur les avantages de la globalisation et localisation de votre application, et exactement ce que signifient ces termes.
Search.SourceType: Video
title: Globalisation et localisation
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 12/07/2018
ms.topic: article
keywords: windows 10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 466747cbbfe6f599f42a39f0d2766f78775ec6c8
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820590"
---
# <a name="globalization-and-localization"></a>Globalisation et localisation

Windows est utilisé dans le monde entier par des personnes de diverses cultures, régions et langues. Vos utilisateurs parlent différentes langues dans divers pays et régions. Certains utilisateurs parlent plusieurs langues. Ainsi, votre application s’exécute sur des configurations qui impliquent de nombreuses permutations des paramètres de langue, de région et de culture du système. Pour étendre le marché potentiel de votre application, vous pouvez la rendre facilement adaptable grâce aux fonctionnalités de *globalisation* et de *localisation*.

Cette vidéo fournit une brève introduction sur la préparation de votre application pour le monde entier : [Introduction à la globalisation et localisation](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

La **globalisation** implique le processus de conception et de développement de votre application de manière à ce qu'elle fonctionne correctement dans les différents marchés internationaux (sur les systèmes de configurations de langues et de cultures différentes), sans avoir besoin de modification ou de personnalisation spécifique à la langue.

- Lors de la manipulation des chaînes, il faut tenir compte de la culture. Par exemple, il ne faut pas modifier la casse des chaînes avant de les avoir comparés.
- Utilisez les calendriers adaptés à la culture actuelle.
- Utilisez API de globalisation pour afficher les données associées à une mise en forme adéquate pour le pays ou la région, notamment les chiffres, les dates, les heures et les devises.
- Tenez compte que les différentes cultures ont différentes règles d'assemblage (triage) du texte et d'autres données.

Votre code doit fonctionner de manière égale dans toutes les cultures que vous avez déterminées pour la prise en charge de votre application. Dans l'idéal, votre code fonctionnera de manière égale dans le contexte de *toute* langue, région ou culture. Le moyen le plus efficace de globaliser les fonctions de votre application consiste à utiliser le concepts de cultures/localités. Une culture/localité est un ensemble de règles et un ensemble de données spécifiques à une langue donnée dans une zone géographique donnée. Ces règles et données comprennent des informations concernant les éléments suivants.

- Classification des caractères
- Système d'écriture
- Formatage de date et d’heure
- Conventions numériques, de devise, de poids et de mesure
- Règles de tri

>[!NOTE]
> Pour obtenir la liste des codes de pays/région standard utilisés par Microsoft, consultez le [liste de pays/région officielle](https://globalready.azurewebsites.net/marketreadiness/OfficialCountryregion).


L'**adaptabilité** constitue le processus de préparation d'une application globalisée pour la localisation et/ou la vérification que l'application est prête pour la localisation. Le fait de rendre une application correctement localisable signifie que le processus de localisation ultérieur sera dépourvu de défaillance dans l'application. Le caractère localisable de l'application nécessite que son code exécutable soit proprement séparé des ressources localisables de l'application.

- La longueur des chaînes traduites peut grandement varier selon la langue. De ce fait, il convient de concevoir votre interface utilisateur pour accommoder les différentes longueurs de texte et tailles de police, ainsi que les contrôles de saisie de texte.
- Essayez d'éviter les images sensibles en matière de texte et de culture.
- Il convient de ne pas coder en dure les chaînes et images dépendantes de la culture dans le code et le balisage de votre application. Il convient plutôt de les stocker en tant que ressources de chaîne et d'image afin de pouvoir les adapter aux différents marchés locaux, quels que soient les codes binaires de votre application.
- Peuso-localisez votre application pour révéler tout problèmes d'adaptabilité.

La **localisation** constitue le processus d'adaptation ou de traduction des ressources localisables de votre application afin de correspondre à la langue, à la culture et aux exigences politiques des marchés locaux spécifiques qui seront pris en charge par l'application. Une fois l'étape de localisation de l'application atteinte, si votre application est localisable, vous n'aurez pas besoin de modifier la logique au cours de ce processus.

- Traduisez les chaînes et autres ressources de l'application pour le nouveau marché.
- Modifiez les images selon la culture, le cas échéant.
- Les fichiers peuvent également varier en fonction de la région de l’utilisateur, indépendamment de la langue. Par exemple, une carte peut présenter plusieurs frontières selon la localisation de l'utilisateurs, mais les étiquettes doivent respecter la langue favorite de l'utilisateur.

La plupart des équipes de localisation utilisent des outils spéciaux pour faciliter ce processus. Par exemple, par le recyclage des traductions du texte récurrent.

| Article | Description |
|---------|-------------|
| [Instructions pour la globalisation](guidelines-and-checklist-for-globalizing-your-app.md) | Concevez et développez votre application afin qu'elle fonctionne de manière appropriée sur les systèmes avec des configurations de langue et de culture différentes. |
| [Comprendre les langues de profil utilisateur et les langues de manifeste d’application](manage-language-and-region.md) | Cette rubrique défini les termes « liste de langues de profil utilisateur », « liste de langues de manifeste d'application » et « liste de langues d'exécution de l'application ». Nous utiliserons ces termes dans cette rubriques et parmi d'autres concernant les fonctionnalités. Il est donc important que vous en compreniez le sens. |
| [Globaliser vos formats de date/heure/nombre](use-global-ready-formats.md) | Concevez votre application dans une perspective de globalisation en mettant correctement en forme dates, heures, nombres, numéros de téléphone et devises. Plus tard, vous serez en mesure d’adapter votre application à d’autres cultures, à d’autres régions et à d’autres langues pour le marché international. |
| [Utilisez des modèles et des modèles pour formater les dates et heures](use-patterns-to-format-dates-and-times.md) | Utilisez le classes de l'espace de noms [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) avec des modèles personnalisés pour afficher les dates et heures dans le format que vous désirez. |
| [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md) | Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux DàG (de droite à gauche). |
| [Valeurs NumeralSystem](glob-numeralsystem-values.md) | Cette rubrique répertorie les valeurs disponibles pour la propriété **NumeralSystem** des différentes classes de l’espace de noms [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live). |
| [Rendez votre application localisable](prepare-your-app-for-localization.md) | Une application localisée est une application qui peut être localisée pour d'autres marchés, langues ou régions sans présenter de défaillance fonctionnelle. Le caractère localisable de l'application nécessite que son code exécutable soit proprement séparé de ses ressources localisables. |
| [Polices internationales](loc-international-fonts.md) | Cette rubrique répertorie les polices disponibles pour les applications UWP qui sont traduites dans des langues autres que des États-Unis Anglais. |
| [Concevez votre application pour le texte bidirectionnel](design-for-bidi-text.md) | Concevez votre application afin de fournir un support de texte bidirectionnel (BiDi). Ainsi, vous pourrez combiner les scripts des systèmes d'écriture de gauche à droite et de droite à gauche. |
| [Utilisez le Multilingual App Toolkit 4.0](use-mat.md) | Le Multilingual App Toolkit (MAT) 4.0 s’intègre avec Microsoft Visual Studio 2017 et versions ultérieures pour fournir des applications UWP avec la prise en charge, la gestion de fichier de traduction et outils d’édition. |
| [Multilingual App Toolkit 4.0 FAQ et dépannage](mat-faq-troubleshooting.md) | Cette rubrique présente des réponses aux questions fréquentes et aux problèmes relatives au kit de ressources Multilingual App Toolkit 4.0. |
| [Préparer votre application pour la modification de l’ère japonais](japanese-era-change.md) | En savoir plus sur le changement d’ère du Japon de mai 2019 et comment préparer votre application. |
