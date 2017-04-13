---
author: Karl-Bridge-Microsoft
Description: "La reconnaissance vocale permet de fournir une saisie vocale, de spécifier une action ou une commande et d’accomplir différentes tâches."
title: Reconnaissance vocale
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: "voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 49cd1e7ac0fceff7e39679f337ea4c029fa98806
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="speech-recognition"></a>Reconnaissance vocale
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

La reconnaissance vocale permet de fournir une saisie vocale, de spécifier une action ou une commande et d’accomplir différentes tâches.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)</li>
</ul>
</div>

La reconnaissance vocale se compose d’un runtime de fonctions vocales, d’API de reconnaissance vocale pour programmer le runtime, de grammaires prêtes à l’emploi pour la dictée et la recherche web, ainsi que d’une interface utilisateur système par défaut permettant aux utilisateurs de découvrir et d’utiliser les fonctionnalités de reconnaissance vocale.


## <a name="set-up-the-audio-feed"></a>Configurer le flux audio


Assurez-vous que votre appareil est équipé d’un microphone ou d’un dispositif équivalent.

Définissez la fonctionnalité de l’appareil **Microphone** ([**DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/br211430)) dans le [manifeste du package d’application](https://msdn.microsoft.com/library/windows/apps/br211474) (fichier **package.appxmanifest**) pour accéder au flux audio du microphone. Cela permet à l’application d’enregistrer des données audio à partir des microphones connectés.

Voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/library/windows/apps/mt270968).

## <a name="recognize-speech-input"></a>Reconnaître la saisie vocale


Une *contrainte* définit les mots et expressions (vocabulaire) qu’une application reconnaît dans la saisie vocale. Les contraintes sont au centre de la reconnaissance vocale. Elles permettent à votre application de contrôler sa précision.

Vous pouvez utiliser différents types de contraintes lors de l’exécution d’opérations de reconnaissance vocale:

1.  **Grammaires prédéfinies** ([**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)).

    Les grammaires de dictée et de recherche web prédéfinies vous permettent d’activer la reconnaissance vocale dans votre application sans créer votre propre grammaire. Si vous optez pour ces grammaires, la reconnaissance vocale est effectuée par un service web distant qui renvoie les résultats à l’appareil.

    La grammaire de dictée de texte libre par défaut peut reconnaître la plupart des mots et expressions prononcés par un utilisateur dans une langue spécifique. Elle est optimisée pour reconnaître les expressions courtes. La grammaire de dictée prédéfinie est utilisée si vous ne spécifiez pas de contrainte pour votre objet [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226). La dictée de texte libre est utile si vous ne souhaitez pas limiter ce qu’un utilisateur peut dire. Elle est généralement utilisée pour créer des notes ou dicter le contenu d’un message.

    La grammaire de recherche web, comme une grammaire de dictée, contient un grand nombre de mots et expressions qu’un utilisateur peut dire. Toutefois, elle est optimisée pour reconnaître les termes que les personnes utilisent généralement lors des recherches sur le web.

    **Remarque** Étant donné que les grammaires de dictée et de recherche web prédéfinies peuvent être volumineuses et qu’elles sont hébergées en ligne (elles ne se trouvent pas sur l’appareil), les performances obtenues peuvent ne pas être aussi bonnes qu’avec des grammaires personnalisées qui sont installées sur l’appareil.

     

    Ces grammaires prédéfinies peuvent être utilisées pour reconnaître jusqu’à 10secondes de saisie vocale et ne nécessitent aucun effort de création de votre part. Toutefois, elles requièrent une connexion à un réseau.

    Pour utiliser les contraintes de service web, vous devez activer la prise en charge de la saisie vocale et de la dictée dans **Paramètres** en activant l’option Reconnaître ma voix dans la page Paramètres -&gt; Confidentialité -&gt; Voix, entrée manuscrite et frappe.

    Nous indiquons ici comment vérifier si la saisie vocale est activée et comment ouvrir la page Paramètres -&gt; Confidentialité -&gt; Voix, entrée manuscrite et frappe si cette fonction n’est pas activée.

    Nous commençons par initialiser une variable globale (HResultPrivacyStatementDeclined) sur la valeur HResult de 0x80045509. Voir [Gestion des exceptions pour les applications enC# ou VisualBasic](https://msdn.microsoft.com/library/windows/apps/dn532194).

```    CSharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

    We then catch any standard exceptions during recogntion and test if the [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) value is equal to the value of the HResultPrivacyStatementDeclined variable. If so, we display a warning and call `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` to open the Settings page.

    
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
catch (Exception exception)
    {
      // Handle the speech privacy policy error.
      if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
      {
        resultTextBlock.Visibility = Visibility.Visible;
        resultTextBlock.Text = "The privacy statement was declined. 
          Go to Settings -> Privacy -> Speech, inking and typing, and ensure you 
          have viewed the privacy policy, and &#39;Get To Know You&#39; is enabled.";
        // Open the privacy/speech, inking, and typing settings page.
        await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
      }
      else
      {
        var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
        await messageDialog.ShowAsync();
      }
    }
```

2.  **Contraintes de liste de programmation** ([**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)).

    Les contraintes de liste de programmation permettent de créer facilement une grammaire simple sous la forme d’une liste de mots ou d’expressions. Une contrainte de liste fonctionne correctement pour la reconnaissance d’expressions distinctes courtes. En indiquant explicitement des mots dans une grammaire, vous améliorez également la précision de la reconnaissance, car le traitement de la parole par le moteur de reconnaissance se limite à la confirmation d’une correspondance. La liste peut également être mise à jour par programme.

    Une contrainte de liste se compose d’un tableau de chaînes qui correspondent à la saisie vocale acceptée par votre application en vue d’une opération de reconnaissance vocale. Pour créer une contrainte de liste dans votre application, vous devez créer un objet de contrainte de liste de reconnaissance vocale et lui passer un tableau de chaînes. Vous devez ensuite ajouter cet objet à la collection de contraintes du moteur de reconnaissance vocale. La reconnaissance vocale fonctionne quand le moteur de reconnaissance vocale reconnaît l’une des chaînes du tableau.

3.  **Grammaires SRGS** ([**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)).

    Contrairement à une contrainte de liste de programmation, une grammaire SRGS (Speech Recognition Grammar Specification) est un document statique au format XML défini par la norme [SRGS version1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302). Une grammaire SRGS permet de contrôler au maximum l’expérience de la reconnaissance vocale en capturant plusieurs significations sémantiques dans une même reconnaissance.

4.  **Contraintes de commandes vocales** ([**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220))

    Utilisez un fichier XML VCD (Voice Command Definition) pour définir les commandes que l’utilisateur peut prononcer pour lancer des actions au moment de l’activation de votre application. Pour plus d’informations, voir [Lancer une application au premier plan avec les commandes vocales de Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).

**Remarque** Le type de contrainte que vous utilisez dépend de la complexité de l’expérience de reconnaissance que vous voulez créer. Un type de contrainte peut être mieux adapté à une tâche de reconnaissance vocale particulière, mais vous pouvez aussi combiner tous les types de contrainte dans votre application.
Pour apprendre à utiliser des contraintes, voir [Définir des contraintes de reconnaissance vocale personnalisées](define-custom-recognition-constraints.md).

 

La grammaire de dictée prédéfinie d’une application Windows universelle reconnaît la plupart des mots et des expressions courtes dans une langue donnée. Elle est activée par défaut lorsqu’un objet du moteur de reconnaissance vocale est instancié sans contraintes personnalisées.

Dans cet exemple, nous montrons comment effectuer les opérations suivantes :

-   créer un moteur de reconnaissance vocale ;
-   compiler les contraintes de l’application Windows universelle par défaut (aucune grammaire n’a été ajoutée à l’ensemble de grammaires du moteur de reconnaissance vocale) ;
-   commencer à écouter les données vocales en utilisant l’interface utilisateur de reconnaissance vocale de base et le retour de la conversion de texte par synthèse vocale (TTS) fourni par la méthode [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245). Utilisez la méthode [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) si l’interface utilisateur par défaut n’est pas requise.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>Personnaliser l’interface utilisateur de reconnaissance vocale


Quand votre application tente une reconnaissance vocale en appelant la méthode [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245), plusieurs écrans s’affichent dans l’ordre suivant.

Si vous utilisez une contrainte basée sur une grammaire prédéfinie (dictée ou recherche web):

-   Écran **Écoute**
-   Écran **Réflexion**
-   Écran **Nous vous avons entendu dire** ou écran de notification d’erreur

Si vous utilisez une contrainte basée sur une liste de mots ou d’expressions ou une contrainte basée sur un fichier de grammaire SRGS:

-   Écran **Écoute**
-   Écran **Nous vous avons entendu dire**, si l’interprétation de ce que l’utilisateur a prononcé pourrait avoir plusieurs résultats éventuels
-   Écran **Nous vous avons entendu dire** ou écran de notification d’erreur

L’image suivante présente un exemple du flux entre des écrans d’un moteur de reconnaissance vocale qui utilise une contrainte basée sur un fichier de grammaire SGRS. Dans cet exemple, la reconnaissance vocale a réussi.

![Écran initial de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-initial.png)

![Écran intermédiaire de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-intermediate.png)

![Écran final de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-complete.png)

L’écran **Listening** peut fournir des exemples de mot ou d’expression que l’application peut reconnaître. Nous montrons ici comment utiliser les propriétés de la classe [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234) (obtenue en appelant la propriété [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254)) pour personnaliser le contenu de l’écran **Listening**.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="related-articles"></a>Articles connexes


**Développeurs**
* [Interactions vocales](speech-interactions.md)
**Concepteurs**
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)
**Exemples**
* [Exemple de reconnaissance vocale et de synthèse vocale](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




