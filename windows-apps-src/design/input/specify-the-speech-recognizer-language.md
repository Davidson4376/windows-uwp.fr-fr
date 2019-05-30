---
Description: Découvrez comment sélectionner une langue installée à utiliser pour la reconnaissance vocale.
title: Spécifier la langue de reconnaissance vocale
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 778aa04861fa7704f4235763a429bb77f92a8b65
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365327"
---
# <a name="specify-the-speech-recognizer-language"></a>Spécifier la langue de reconnaissance vocale


Découvrez comment sélectionner une langue installée à utiliser pour la reconnaissance vocale.

> **API importantes** : [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


Nous avons répertorié les langues installées sur un système. Identifiez la langue par défaut et sélectionnez une autre langue pour la reconnaissance.

**Conditions préalables :**

Cette rubrique s’appuie sur l’article [Reconnaissance vocale](speech-recognition.md).

Vous devez posséder des connaissances de base sur la reconnaissance vocale et ses contraintes.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Recommandations pour l’expérience utilisateur :**

Pour obtenir de précieux conseils concernant la conception d’une application dotée de fonctions vocales à la fois utile et conviviale, voir [Recommandations en matière de conception](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions).

## <a name="identify-the-default-language"></a>Identifiez la langue par défaut.


La reconnaissance vocale utilise la langue du système en tant que langue de reconnaissance par défaut. Cette langue est définie par l’utilisateur sur l’écran Paramètres &gt; Système &gt; Parole &gt; Langue vocale de l’appareil.

Nous identifions la langue par défaut en vérifiant la propriété statique [**SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmer une langue installée


Les langues installées peuvent varier entre les appareils. Vérifiez l’existence d’une langue avant de vous en servir pour une contrainte particulière.

**Remarque**  un redémarrage est nécessaire après l’installation d’un nouveau pack de langue. Une exception avec le code d’erreur SPERR\_pas\_trouvé (0x8004503a) est générée si la langue spécifiée n’est pas pris en charge ou n’a pas terminé l’installation.

 

Déterminez les langues prises en charge sur un appareil en vérifiant l’une des deux propriétés statiques de la classe [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) :

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)— la collection de [ **langage** ](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) objets utilisés avec la dictée prédéfinie et les grammaires de recherche web.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)— la collection de [ **langage** ](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) objets utilisés avec une contrainte de liste ou un fichier Speech Recognition Grammar Specification (SRGS).

## <a name="specify-a-language"></a>Spécifier une langue


Pour spécifier une langue, passez un objet [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) dans le constructeur [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).

Ici, nous spécifions « en-US » comme langue de reconnaissance.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Notes


Une contrainte de rubrique peut être configurée en ajoutant une [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) à la collection [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de la [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), puis en appelant [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Un [**speechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** est renvoyé si le module de reconnaissance n’est pas initialisé avec une langue de rubrique prise en charge.

Une contrainte de liste peut être configurée en ajoutant une [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) à la collection [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de la [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), puis en appelant [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Vous ne pouvez pas spécifier directement la langue d’une liste personnalisée. La liste est traitée à l’aide de la langue du module de reconnaissance.

Une grammaire SRGS est un format XML standard ouvert représenté par la classe [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint). Contrairement aux listes personnalisées, vous pouvez spécifier la langue de la grammaire dans le balisage SRGS. [**CompileConstraintsAsync** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) échoue avec un [ **SpeechRecognitionResultStatus** ](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** si le module de reconnaissance n’est pas initialisée avec la même langue que la balise SRGS.

## <a name="related-articles"></a>Articles connexes

**Développeurs**

* [Interactions vocales](speech-interactions.md)

**Concepteurs**

* [Recommandations en matière de conception de fonctions vocales](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**Exemples**

* [La reconnaissance vocale et exemple de synthèse vocale](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




