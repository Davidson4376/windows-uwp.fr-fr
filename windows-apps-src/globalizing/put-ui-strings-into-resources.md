---
author: DelfCo
Description: "Placez les ressources de type chaîne de votre interface utilisateur dans des fichiers de ressources. Vous pouvez ensuite référencer ces chaînes dans votre code ou votre balisage."
title: "Placer des chaînes d’interface utilisateur dans des ressources"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: b44d9235e34b8d4c75f663029d1dde3f87bd0eb7

---

# Placer des chaînes d’interface utilisateur dans des ressources





**API importantes**

-   [**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)
-   [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)

Placez les ressources de type chaîne de votre interface utilisateur dans des fichiers de ressources. Vous pouvez ensuite référencer ces chaînes dans votre code ou votre balisage.

Cette rubrique décrit la procédure requise pour ajouter plusieurs ressources de chaîne de langue à votre application Windows universelle et pour les tester brièvement.

## <span id="put_strings_into_resource_files__instead_of_putting_them_directly_in_code_or_markup."></span><span id="PUT_STRINGS_INTO_RESOURCE_FILES__INSTEAD_OF_PUTTING_THEM_DIRECTLY_IN_CODE_OR_MARKUP."></span>Placez les chaînes dans des fichiers de ressources au lieu de les insérer directement dans le code ou dans le balisage.


1.  Ouvrez votre solution (ou créez-en une) dans Visual Studio.

2.  Ouvrez package.appxmanifest dans Visual Studio, accédez à l’onglet **Application**, puis (pour cet exemple) définissez la langue par défaut sur « en-US ». Si votre solution comporte plusieurs fichiers package.appxmanifest, effectuez cette opération pour chacun d’eux.
    <br>**Remarque** Cette opération spécifie la langue par défaut du projet. Les ressources linguistiques par défaut sont utilisées si la langue par défaut de l’utilisateur ou les langues d’affichage ne correspondent pas aux ressources linguistiques fournies dans l’application.
3.  Créez un dossier destiné à contenir les fichiers de ressources.
    1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet (projet partagé si votre solution contient plusieurs projets) et sélectionnez **Ajouter**&gt;**Nouveau dossier**.
    2.  Nommez le nouveau dossier « Strings ».
    3.  Si le nouveau dossier n’apparaît pas dans l’Explorateur de solutions, sélectionnez **Projet**&gt;**Afficher tous les fichiers** dans le menu Microsoft Visual Studio en veillant à ce que le projet soit toujours sélectionné.

4.  Créez un sous-dossier et un fichier de ressources pour l’anglais (États-Unis).
    1.  Cliquez avec le bouton droit sur le dossier Strings et ajoutez un dossier sous celui-ci. Nommez-le « en-US ». Le fichier de ressources doit être placé dans un dossier qui a été nommé pour la balise de langue [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Pour obtenir plus de détails sur le qualificateur de langue ainsi qu’une liste des balises de langue usuelles, voir [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).
    2.  Cliquez avec le bouton droit sur le dossier en-US et sélectionnez **Ajouter**&gt;**Nouvel élément...**
    3.  **XAML :** sélectionnez Resources File (.resw).
        <br>**HTML :** sélectionnez Resources File (.resjson).

    4.  Cliquez sur **Ajouter**. Un fichier de ressources est ajouté avec le nom par défaut Resources.resw (pour **XAML**) ou resources.rejson (pour **HTML**). Nous vous recommandons d’utiliser ce nom de fichier par défaut. Les applications peuvent partitionner leurs ressources dans d’autres fichiers. Toutefois, elles doivent y faire référence correctement (voir [Comment charger des ressources de type chaîne](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)).
    5.  **XAML uniquement :**si vous disposez de fichiers .resx contenant uniquement des ressources de type chaîne provenant de précédents projets .NET, sélectionnez **Ajouter**&gt;**Élément existant...**, ajoutez le fichier .resx, puis renommez-le avec l’extension .resw.
    6.  Ouvrez le fichier et utilisez l’éditeur pour ajouter les ressources suivantes :

        **XAML :**

        Strings/en-US/Resources.resw ![add resource, english](images/addresource-en-us.png) Dans cet exemple, « Greeting.Text » et « Farewell » identifient les chaînes qui doivent être affichées. « Greeting.Width » identifie la propriété Width de la chaîne « Greeting ». Les commentaires permettent de donner des instructions particulières aux traducteurs qui localisent les chaînes dans d’autres langues.

        **HTML :**

        Le nouveau fichier contient du contenu par défaut. Remplacez le contenu par ce qui suit (qui peut être très semblable au contenu par défaut) :

        Strings/en-US/resources.resjson

        ```        json
        {
                "greeting"              : "Hello",
                "_greeting.comment"     : "A welcome greeting",

                "farewell"              : "Goodbye",
                "_farewell.comment"     : "A goodbye"
        }
        ```

        Il s’agit d’une syntaxe JSON (JavaScript Object Notation) stricte, selon laquelle une virgule doit être placée après chaque paire nom/valeur, sauf la dernière. Dans cet exemple, « greeting » et « farewell » identifient les chaînes à afficher. Les autres paires (« \_greeting.comment » et « \_farewell.comment ») sont des commentaires qui décrivent les chaînes. Les commentaires permettent de donner des instructions particulières aux traducteurs qui localisent les chaînes dans d’autres langues.

## <span id="associate_controls_to_resources."></span><span id="ASSOCIATE_CONTROLS_TO_RESOURCES."></span>Associez des contrôles aux ressources.


**XAML uniquement :**

Vous devez associer chaque contrôle nécessitant du texte localisé au fichier .resw. Pour ce faire, utilisez l’attribut **x:Uid** sur vos éléments XAML comme suit :

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

Attribuez la valeur d’attribut **Uid** au nom de ressource, et spécifiez quelle propriété doit recevoir la chaîne traduite (dans ce cas, la propriété Text). Vous pouvez spécifier d’autres propriétés/valeurs pour d’autres langues (à l’image de Greeting.Width) mais utilisez avec prudence ces propriétés relatives à la disposition. Vous devez veiller à permettre aux contrôles de s’organiser de manière dynamique en fonction de l’écran du périphérique.

Notez que les propriétés jointes sont traitées différemment dans des fichiers resw comme AutomationPeer.Name. Vous devez écrire explicitement l’espace de noms comme suit :

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <span id="add_string_resource_identifiers_to_code_and_markup."></span><span id="ADD_STRING_RESOURCE_IDENTIFIERS_TO_CODE_AND_MARKUP."></span>Ajoutez des identificateurs de ressource de type chaîne dans le code et le balisage.


**XAML :**

Dans votre code, vous pouvez faire référence aux chaînes de façon dynamique :

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```

**HTML :**

1.  Ajoutez des références à la Bibliothèque Windows pour JavaScript dans votre fichier HTML, si elles ne s’y trouvent pas déjà.

    **Remarque** Le code ci-après présente le code HTML pour le fichier default.html du projet Windows qui est généré lorsque vous créez un projet JavaScript **Application vide (Windows universel)** dans Visual Studio. Notez que le fichier contient déjà les références à WinJS.

    ```    HTML
    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>
    ```

2.  Dans le code JavaScript qui accompagne votre fichier HTML, appelez la fonction [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864) lorsque votre code HTML est chargé.

    ```    JavaScript
    WinJS.Application.onloaded = function(){
        WinJS.Resources.processAll();
    }
    ```
    
    Si du code HTML supplémentaire est chargé dans un objet [**WinJS.UI.Pages.PageControl**](https://msdn.microsoft.com/library/windows/apps/jj126158), appelez [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)(*element*) dans la méthode [**IPageControlMembers.ready**](https://msdn.microsoft.com/library/windows/apps/hh770590) du contrôle de la page, où *element* est l’élément HTML (et ses éléments enfants) en cours de chargement. Cet exemple est basé sur le scénario 6 des [exemples de localisation et de ressources d’application](http://go.microsoft.com/fwlink/p/?linkid=227301) :

    ```    JavaScript
    var output;
    
    var page = WinJS.UI.Pages.define("/html/scenario6.html", {
        ready: function (element, options) {
            output = element.querySelector('#output');
            WinJS.Resources.processAll(output); // Refetch string resources
            WinJS.Resources.addEventListener("contextchanged", refresh, false);
        }
        unload: function () { 
            WinJS.Resources.removeEventListener("contextchanged", refresh, false); 
        } 
    });
    ```

3.  En HTML, utilisez des identificateurs de ressources (« greeting » et « farewell ») pour faire référence aux ressources de chaîne dans les fichiers de ressources.
    ```    HTML
    <h2><span data-win-res="{textContent: 'greeting';}"></span></h2>
    <h2><span data-win-res="{textContent: 'farewell'}"></span></h2>
    ```

4.  Faites référence aux ressources de chaîne pour les attributs.

    ```    HTML
    <div data-win-res="{attributes: {'aria-label'; : 'String1'}}" >
    ```

    Pour le remplacement HTML, le modèle général de l’attribut data-win-res est data-win-res="{*propertyname1*: ’*resource ID*’, *propertyname2*: ’*resource ID2*’}".

    **Remarque** Si la chaîne ne peut pas contenir de balisage, liez la ressource chaque fois que possible à la propriété textContent à la place de innerHTML. La propriété textContent est beaucoup plus rapide à remplacer que innerHTML.

5.  Faites référence aux ressources de chaîne en JavaScript.
    <span codelanguage="JavaScript"></span>
    ```    JavaScript
    var el = element.querySelector('#header');
    var res = WinJS.Resources.getString('greeting');
    el.textContent = res.value;
    el.setAttribute('lang', res.lang);
    ```

## <span id="add_folders_and_resource_files_for_two_additional_languages."></span><span id="ADD_FOLDERS_AND_RESOURCE_FILES_FOR_TWO_ADDITIONAL_LANGUAGES."></span>Ajoutez des dossiers et des fichiers de ressources pour deux langues supplémentaires.


1.  Ajoutez un dossier dans le dossier Strings pour l’allemand. Nommez le dossier « de-DE », signifiant « Deutsch (Deutschland) ».
2.  Créez un autre fichier de ressources dans le dossier de-DE, puis ajoutez la ligne suivante :

    **XAML :**

    strings/de-DE/Resources.resw

    ![Ajouter une ressource (pour l’allemand)](images/addresource-de-de.png)

    **HTML :**

    strings/de-DE/resources.resjson

    ```    json
    {
        "greeting"              : "Hallo",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Auf Wiedersehen",
        "_farewell.comment"     : "A goodbye."
    }
    ```

3.  Créez un autre dossier nommé « fr-FR » pour « Français (France) ». Créez un fichier de ressources et ajoutez la ligne suivante :

    **XAML :**

    strings/fr-FR/Resources.resw ![ajouter une ressource, pour le français](images/addresource-fr-fr.png)
    **HTML :**

    strings/fr-FR/resources.resjson

    ```    json
    {
        "greeting"              : "Bonjour",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Au revoir",
        "_farewell.comment"     : "A goodbye."
    }
    ```

## <span id="build_and_run_the_app."></span><span id="BUILD_AND_RUN_THE_APP."></span>Générez et exécutez l’application.


Testez l’application pour votre langue d’affichage par défaut.

1.  Appuyez sur la touche F5 pour générer et exécuter l’application.
2.  Notez que les chaînes « greeting » et « farewell » sont affichées dans la langue d’affichage par défaut de l’utilisateur.
3.  Quittez l’application.

Testez l’application avec les autres langues.

1.  Accédez à **Paramètres** sur votre appareil.
2.  Sélectionnez **Heure et langue**.
3.  Sélectionnez **Région et langue** (ou sur un téléphone ou un émulateur de téléphone, **Langue**).
4.  Notez que la langue qui était affichée lorsque vous avez exécuté l’application est la langue principale, c’est-à-dire Anglais, Allemand ou Français. Si votre langue principale n’est pas l’une de ces trois langues, l’application passe à la langue suivante dans la liste qui est prise en charge par l’application.
5.  Si vous ne disposez pas de ces trois langues sur votre appareil, ajoutez les langues manquantes en cliquant sur **Ajouter une langue** pour les ajouter à la liste.
6.  Pour tester l’application avec une autre langue, sélectionnez cette langue dans la liste, puis cliquez sur **Définir par défaut** (ou sur un téléphone ou un émulateur de téléphone, appuyez longuement sur la langue dans la liste, puis appuyez sur **Monter** jusqu’à ce que la langue apparaisse au sommet de la liste). Exécutez ensuite l’application.

## <span id="related_topics"></span>Rubriques connexes


* [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [Comment charger des ressources de type chaîne](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [Balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 






<!--HONumber=Jun16_HO4-->


