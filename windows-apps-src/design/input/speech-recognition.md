---
Description: La reconnaissance vocale permet de fournir une saisie vocale, de spécifier une action ou une commande et d’accomplir différentes tâches.
title: Reconnaissance vocale
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0692207046c999dc55a56bd3f0948f3f5b93fecd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365232"
---
# <a name="speech-recognition"></a>Reconnaissance vocale


La reconnaissance vocale permet de fournir une saisie vocale, de spécifier une action ou une commande et d’accomplir différentes tâches.

> **API importantes** : [**Windows.Media.SpeechRecognition**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition)

La reconnaissance vocale se compose d’un runtime de fonctions vocales, d’API de reconnaissance vocale pour programmer le runtime, de grammaires prêtes à l’emploi pour la dictée et la recherche web, ainsi que d’une interface utilisateur système par défaut permettant aux utilisateurs de découvrir et d’utiliser les fonctionnalités de reconnaissance vocale.

## <a name="configure-speech-recognition"></a>Configurer la reconnaissance vocale

Pour prendre en charge la reconnaissance vocale avec votre application, l’utilisateur doit se connecter et activer un microphone sur leur appareil et accepter la politique de confidentialité de Microsoft l’octroi d’autorisation pour votre application pour l’utiliser.

Pour inviter automatiquement l’utilisateur avec une boîte de dialogue système demande l’autorisation d’accéder à et utiliser des flux audio du microphone (exemple à partir de la [la reconnaissance vocale et exemple de synthèse vocale](https://go.microsoft.com/fwlink/p/?LinkID=619897) illustré ci-dessous), définissez simplement le  **Microphone** [fonctionnalité de l’appareil](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) dans le [manifeste de package d’application](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). Pour plus d’informations, consultez [déclarations des fonctionnalités d’application](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

![Politique de confidentialité pour l’accès au microphone](images/speech/privacy.png)

Si l’utilisateur clique sur Oui pour accorder l’accès au microphone, votre application est ajoutée à la liste des applications approuvées sur les paramètres -> confidentialité -> page de Microphone. Toutefois, comme l’utilisateur peut choisir de désactiver ce paramètre à tout moment, vous devez confirmer que votre application a accès à l’aide du microphone avant de tenter de l’utiliser.

Si vous souhaitez également prendre en charge de la dictée, de Cortana ou d’autres services de reconnaissance vocale (comme un [prédéfinis grammaire](#predefined-grammars) définies dans une contrainte de rubrique), vous devez également confirmer que **la reconnaissance vocale en ligne** (Paramètres -> confidentialité -> vocale) est activée.

Cet extrait de code montre comment votre application peut vérifier si un microphone est présent et si elle a l’autorisation de l’utiliser.

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>Reconnaître la saisie vocale

Une *contrainte* définit les mots et expressions (vocabulaire) qu’une application reconnaît dans la saisie vocale. Les contraintes sont au centre de la reconnaissance vocale. Elles permettent à votre application de contrôler sa précision.

Vous pouvez utiliser les types suivants de contraintes de reconnaissance de la saisie vocale.

### <a name="predefined-grammars"></a>Grammaires prédéfinies

Les grammaires de dictée et de recherche web prédéfinies vous permettent d’activer la reconnaissance vocale dans votre application sans créer votre propre grammaire. Si vous optez pour ces grammaires, la reconnaissance vocale est effectuée par un service web distant qui renvoie les résultats à l’appareil.

La grammaire de dictée de texte libre par défaut peut reconnaître la plupart des mots et expressions prononcés par un utilisateur dans une langue spécifique. Elle est optimisée pour reconnaître les expressions courtes. La grammaire de dictée prédéfinie est utilisée si vous ne spécifiez pas de contrainte pour votre objet [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer). La dictée de texte libre est utile si vous ne souhaitez pas limiter ce qu’un utilisateur peut dire. Elle est généralement utilisée pour créer des notes ou dicter le contenu d’un message.

La grammaire de recherche web, comme une grammaire de dictée, contient un grand nombre de mots et expressions qu’un utilisateur peut dire. Toutefois, elle est optimisée pour reconnaître les termes que les personnes utilisent généralement lors des recherches sur le web.

**Remarque**  car des grammaires de la dictée et recherche de web prédéfinis peuvent être volumineux, et qu’ils sont en ligne (pas sur l’appareil), performances ne peuvent pas être aussi rapides qu’avec une syntaxe personnalisée installée sur l’appareil.     

Ces grammaires prédéfinies peuvent être utilisées pour reconnaître jusqu’à 10 secondes de saisie vocale et ne nécessitent aucun effort de création de votre part. Toutefois, elles requièrent une connexion à un réseau.

Pour utiliser les contraintes de service web, vous devez activer la prise en charge de la saisie vocale et de la dictée dans **Paramètres** en activant l’option « Reconnaître ma voix » dans la page **Paramètres -> Confidentialité -> Voix, entrée manuscrite et frappe**.

Nous indiquons ici comment vérifier si la saisie vocale est activée et comment ouvrir la page Paramètres -> Confidentialité -> Voix, entrée manuscrite et frappe si cette fonction n’est pas activée.

Nous commençons par initialiser une variable globale (HResultPrivacyStatementDeclined) sur la valeur HResult de 0x80045509. Consultez [gestion des exceptions pour dans C\# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10)).

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

Nous interceptons ensuite les exceptions standard au cours de la reconnaissance continue et nous testons si la valeur de [**HResult**](https://docs.microsoft.com/uwp/api/Windows.Foundation.HResult) est égale à celle de la variable HResultPrivacyStatementDeclined. Dans l’affirmative, nous affichons un avertissement et un appel `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` d’ouverture de la page de paramètres.

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
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

Consultez [ **SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint).

### <a name="programmatic-list-constraints"></a>Contraintes de la liste par programmation 

Les contraintes de liste de programmation permettent de créer facilement une grammaire simple sous la forme d’une liste de mots ou d’expressions. Une contrainte de liste fonctionne correctement pour la reconnaissance d’expressions distinctes courtes. En indiquant explicitement des mots dans une grammaire, vous améliorez également la précision de la reconnaissance, car le traitement de la parole par le moteur de reconnaissance se limite à la confirmation d’une correspondance. La liste peut également être mise à jour par programme.

Une contrainte de liste se compose d’un tableau de chaînes qui correspondent à la saisie vocale acceptée par votre application en vue d’une opération de reconnaissance vocale. Pour créer une contrainte de liste dans votre application, vous devez créer un objet de contrainte de liste de reconnaissance vocale et lui passer un tableau de chaînes. Vous devez ensuite ajouter cet objet à la collection de contraintes du moteur de reconnaissance vocale. La reconnaissance vocale fonctionne quand le moteur de reconnaissance vocale reconnaît l’une des chaînes du tableau.

Consultez [ **SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint).

### <a name="srgs-grammars"></a>Grammaires SRGS

Contrairement à une contrainte de liste de programmation, une grammaire SRGS (Speech Recognition Grammar Specification) est un document statique au format XML défini par la norme [SRGS version 1.0](https://go.microsoft.com/fwlink/p/?LinkID=262302). Une grammaire SRGS permet de contrôler au maximum l’expérience de la reconnaissance vocale en capturant plusieurs significations sémantiques dans une même reconnaissance.

 Consultez [ **SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint).

### <a name="voice-command-constraints"></a>Contraintes de commandes vocales

Utilisez un fichier XML VCD (Voice Command Definition) pour définir les commandes que l’utilisateur peut prononcer pour lancer des actions au moment de l’activation de votre application. Pour plus d’informations, consultez [activer une application de premier plan avec les commandes vocales via Cortana](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).

Consultez [ **SpeechRecognitionVoiceCommandDefinitionConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**Remarque**  le type du type de contrainte que vous utilisez dépend de la complexité de l’expérience de reconnaissance que vous souhaitez créer. Un type de contrainte peut être mieux adapté à une tâche de reconnaissance vocale particulière, mais vous pouvez aussi combiner tous les types de contrainte dans votre application.
Pour apprendre à utiliser des contraintes, voir [Définir des contraintes de reconnaissance vocale personnalisées](define-custom-recognition-constraints.md).

La grammaire de dictée prédéfinie d’une application Windows universelle reconnaît la plupart des mots et des expressions courtes dans une langue donnée. Elle est activée par défaut lorsqu’un objet du moteur de reconnaissance vocale est instancié sans contraintes personnalisées.

Dans cet exemple, nous montrons comment effectuer les opérations suivantes :

- créer un moteur de reconnaissance vocale ;
- compiler les contraintes de l’application Windows universelle par défaut (aucune grammaire n’a été ajoutée à l’ensemble de grammaires du moteur de reconnaissance vocale) ;
- commencer à écouter les données vocales en utilisant l’interface utilisateur de reconnaissance vocale de base et le retour de la conversion de texte par synthèse vocale (TTS) fourni par la méthode [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync). Utilisez la méthode [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) si l’interface utilisateur par défaut n’est pas requise.

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


Quand votre application tente une reconnaissance vocale en appelant la méthode [**SpeechRecognizer.RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync), plusieurs écrans s’affichent dans l’ordre suivant.

Si vous utilisez une contrainte basée sur une grammaire prédéfinie (dictée ou recherche web) :

-   Écran **Écoute**
-   Écran **Réflexion**
-   Écran **Nous vous avons entendu dire** ou écran de notification d’erreur

Si vous utilisez une contrainte basée sur une liste de mots ou d’expressions ou une contrainte basée sur un fichier de grammaire SRGS :

-   Écran **Écoute**
-   Écran **Nous vous avons entendu dire**, si l’interprétation de ce que l’utilisateur a prononcé pourrait avoir plusieurs résultats éventuels
-   Écran **Nous vous avons entendu dire** ou écran de notification d’erreur

L’image suivante présente un exemple du flux entre des écrans d’un moteur de reconnaissance vocale qui utilise une contrainte basée sur un fichier de grammaire SGRS. Dans cet exemple, la reconnaissance vocale a réussi.

![Écran initial de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-initial.png)

![Écran intermédiaire de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-intermediate.png)

![Écran final de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-complete.png)

L’écran **Listening** peut fournir des exemples de mot ou d’expression que l’application peut reconnaître. Nous montrons ici comment utiliser les propriétés de la classe [**SpeechRecognizerUIOptions**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions) (obtenue en appelant la propriété [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions)) pour personnaliser le contenu de l’écran **Listening**.

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
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
* [Recommandations en matière de conception de fonctions vocales](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)
**Exemples**
* [La reconnaissance vocale et exemple de synthèse vocale](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




