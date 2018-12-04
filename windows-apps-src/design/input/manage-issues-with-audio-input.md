---
Description: Learn how to manage issues with speech-recognition accuracy caused by audio-input quality.
title: Gérer les problèmes liés aux entrées audio
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5c33e2890ce962f1321ef40ca0e0605e0f4e1f5f
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8467477"
---
# <a name="manage-issues-with-audio-input"></a>Gérer les problèmes liés aux entrées audio


Découvrez comment gérer les problèmes liés à la précision de la reconnaissance vocale qu’entraîne une baisse de qualité des entrées audio.

> **API importantes**: [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243), [**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)


## <a name="assess-audio-input-quality"></a>Évaluer la qualité de l’entrée audio


Lorsque la reconnaissance vocale est active, utilisez l’événement [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) de votre moteur de reconnaissance vocale pour déterminer si un ou plusieurs problèmes audio peuvent interférer avec la saisie vocale. L’argument d’événement ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) fournit la propriété [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431), qui décrit les problèmes détectés liés à l’entrée audio.

En effet, la fonction de reconnaissance vocale peut être affectée par un trop grand bruit de fond, un microphone muet ainsi que le volume ou la vitesse du haut-parleur.

Ici, nous configurons un module de reconnaissance vocale et nous commençons à écouter l’événement [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243).

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
    speechRecognizer.UIOptions.ExampleText = "Ex. 'weather for London'";
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

## <a name="manage-the-speech-recognition-experience"></a>Gérer l’expérience de la fonction de reconnaissance vocale


Utilisez la description fournie par la propriété [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) pour améliorer l’expérience de l’utilisateur en matière de reconnaissance vocale.

Ici, nous créons un gestionnaire pour l’événement [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) qui recherche un faible volume. Nous pouvons ensuite utiliser un objet [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) pour suggérer que l’utilisateur essaye de parler plus fort.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>Articles connexes


* [Interactions vocales](speech-interactions.md)

**Exemples**
* [Exemple de reconnaissance vocale et de synthèse vocale](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




