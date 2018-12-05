---
Description: Learn about the benefits of globalizing and localizing your app, and exactly what these terms mean.
Search.SourceType: Video
title: Globalisation et localisation
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 49ee09374db54678dbe0152b996537771fd1bfd1
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8738843"
---
# <a name="globalization-and-localization"></a>Globalisation et localisation

Windows est utilisé dans le monde entier, par des publics de diverses langues, régions et cultures. Vos utilisateurs parlent différentes langues dans divers pays et régions. Certains utilisateurs parlent plusieurs langues. Ainsi, votre application s’exécute sur des configurations qui impliquent de nombreuses permutations des paramètres de langue, de région et de culture du système. Pour étendre le marché potentiel de votre application, vous pouvez la rendre facilement adaptable grâce aux fonctionnalités de *globalisation* et de *localisation*.

Cette vidéo vous informe brièvement comment préparer votre application à une utilisation dans le monde entier : [Introduction à la globalisation et à la localisation](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

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
| [Directives en matière de globalisation](guidelines-and-checklist-for-globalizing-your-app.md) | Concevez et développez votre application afin qu'elle fonctionne de manière appropriée sur les systèmes avec des configurations de langue et de culture différentes. |
| [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md) | Cette rubrique défini les termes «liste de langues de profil utilisateur», «liste de langues de manifeste d'application» et «liste de langues d'exécution de l'application». Nous utiliserons ces termes dans cette rubriques et parmi d'autres concernant les fonctionnalités. Il est donc important que vous en compreniez le sens. |
| [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md) | Concevez votre application dans une perspective de globalisation en mettant correctement en forme dates, heures, nombres, numéros de téléphone et devises. Plus tard, vous serez en mesure d’adapter votre application à d’autres cultures, à d’autres régions et à d’autres langues pour le marché international. |
| [Utiliser des modèles de format des dates et heures](use-patterns-to-format-dates-and-times.md) | Utilisez le classes de l'espace de noms [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) avec des modèles personnalisés pour afficher les dates et heures dans le format que vous désirez. |
| [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md) | Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux DàG (de droite à gauche). |
| [Valeurs de NumeralSystem](glob-numeralsystem-values.md) | Cette rubrique répertorie les valeurs disponibles pour la propriété **NumeralSystem** des différentes classes de l’espace de noms [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live). |
| [Rendre votre application localisable](prepare-your-app-for-localization.md) | Une application localisée est une application qui peut être localisée pour d'autres marchés, langues ou régions sans présenter de défaillance fonctionnelle. Le caractère localisable de l'application nécessite que son code exécutable soit proprement séparé de ses ressources localisables. |
| [Polices internationales](loc-international-fonts.md) | Cette rubrique répertorie les polices disponibles pour les applications UWP qui sont localisées dans des langues autres que l’anglais des États-Unis. |
| [Conception de votre application pour le texte bidirectionnel](design-for-bidi-text.md) | Concevez votre application afin de fournir un support de texte bidirectionnel (BiDi). Ainsi, vous pourrez combiner les scripts des systèmes d'écriture de gauche à droite et de droite à gauche. |
| [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md) | Le kit de ressources MultilingualAppToolkit(MAT)4.0 est intégré à MicrosoftVisualStudio2017 pour fournir aux applications UWP un support de traduction, la gestion des fichiers de traduction et des outils d'éditeur. |
| [FAQ et résolution des problèmes du kit de ressources MultilingualAppToolkit4.0](mat-faq-troubleshooting.md) | Cette rubrique présente des réponses aux questions fréquentes et aux problèmes relatives au kit de ressources MultilingualAppToolkit4.0. |
