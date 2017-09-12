---
author: DelfCo
Description: "Placez les ressources de type chaîne de votre interface utilisateur dans des fichiers de ressources. Vous pouvez ensuite référencer ces chaînes dans votre code ou votre balisage."
title: "Placer des chaînes d’interface utilisateur dans des ressources"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: a3e224fc51245a5f91c29da2d745a3740029cda9
ms.sourcegitcommit: 11664964e548a2af30d6e176c515cdbf330934ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2017
---
# <a name="put-ui-strings-into-resources"></a>Placer des chaînes d’interface utilisateur dans des ressources
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Placez les ressources de type chaîne de votre interface utilisateur dans des fichiers de ressources. Vous pouvez ensuite référencer ces chaînes dans votre code ou votre balisage.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)</li>
<li>[**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)</li>
</ul>
</div>


Cette rubrique décrit la procédure requise pour ajouter plusieurs ressources de chaîne de langue à votre applicationWindows universelle et pour les tester brièvement.

## <a name="put-strings-into-resource-files-instead-of-putting-them-directly-in-code-or-markup"></a>Placez les chaînes dans des fichiers de ressources au lieu de les insérer directement dans le code ou dans le balisage.


1.  Ouvrez votre solution (ou créez-en une) dans VisualStudio.

2.  Ouvrez package.appxmanifest dans VisualStudio, accédez à l’onglet **Application**, puis (pour cet exemple) définissez la langue par défaut sur «en-US». Si votre solution comporte plusieurs fichiers package.appxmanifest, effectuez cette opération pour chacun d’eux.
    <br>**Remarque** Cette opération spécifie la langue par défaut du projet. Les ressources linguistiques par défaut sont utilisées si la langue par défaut de l’utilisateur ou les langues d’affichage ne correspondent pas aux ressources linguistiques fournies dans l’application.
3.  Créez un dossier destiné à contenir les fichiers de ressources.
    1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet (projet partagé si votre solution en contient plusieurs) et sélectionnez **Ajouter** &gt; **Nouveau dossier**.
    2.  Nommez le nouveau dossier «Strings».
    3.  Si le nouveau dossier ne figure pas dans l’Explorateur de solutions, sélectionnez **Projet** &gt; **Afficher tous les fichiers** dans le menu Microsoft Visual Studio en veillant à ce que le projet soit toujours sélectionné.

4.  Créez un sous-dossier et un fichier de ressources pour l’anglais (États-Unis).
    1.  Cliquez avec le bouton droit sur le dossier Strings et ajoutez un dossier sous celui-ci. Nommez-le «en-US». Le fichier de ressources doit être placé dans un dossier qui a été nommé pour la balise de langue [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Pour en savoir plus sur le qualificateur de langue et pour obtenir une liste des balises de langue usuelles, voir [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).
    2.  Cliquez avec le bouton droit sur le dossier en-US, puis sélectionnez **Ajouter** &gt; **Nouvel élément…**.
    3.  SélectionnezResources File (.resw).

    4.  Cliquez sur **Ajouter**. Un fichier de ressources est ajouté, avec le nom par défaut Resources.resw. Nous vous recommandons d’utiliser le nom de fichier par défaut. Les applications peuvent partitionner leurs ressources dans d’autres fichiers, mais vous devez veiller à y faire correctement référence (voir [Comment charger des ressources de type chaîne](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)).
    5.  Si vous disposez de fichiers .resx incluant uniquement des ressources de type chaîne qui proviennent de projets .NET précédents, sélectionnez **Ajouter** &gt; **Élément existant…**, ajoutez le fichier .resx, puis donnez-lui l’extension .resw.
    6.  Ouvrez le fichier et utilisez l’éditeur pour ajouter les ressources suivantes:


        Strings/en-US/Resources.resw ![add resource, english](images/addresource-en-us.png) Dans cet exemple, «Greeting.Text» et «Farewell» identifient les chaînes qui doivent être affichées. «Greeting.Width» identifie la propriété Width de la chaîne «Greeting». Les commentaires permettent de donner des instructions particulières aux traducteurs qui localisent les chaînes dans d’autres langues.

## <a name="associate-controls-to-resources"></a>Associez des contrôles aux ressources.

Vous devez associer chaque contrôle nécessitant du texte localisé au fichier .resw. Pour ce faire, utilisez l’attribut **x:Uid** sur vos éléments XAML comme suit:

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

Attribuez la valeur d’attribut **Uid** au nom de ressource, et spécifiez quelle propriété doit recevoir la chaîne traduite (dans ce cas, la propriété Text). Vous pouvez spécifier d’autres propriétés/valeurs pour d’autres langues (à l’image de Greeting.Width) mais utilisez avec prudence ces propriétés relatives à la disposition. Vous devez veiller à permettre aux contrôles de s’organiser de manière dynamique en fonction de l’écran du périphérique.

Notez que les propriétés jointes sont traitées différemment dans des fichiers resw comme AutomationPeer.Name. Vous devez écrire l’espace de noms explicitement, comme suit:

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="add-string-resource-identifiers-to-code-and-markup"></a>Ajoutez des identificateurs de ressource de type chaîne dans le code et le balisage.

Dans votre code, vous pouvez référencer les chaînes de façon dynamique:

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


## <a name="add-folders-and-resource-files-for-two-additional-languages"></a>Ajoutez des dossiers et des fichiers de ressources pour deux langues supplémentaires.


1.  Ajoutez un dossier dans le dossier Strings pour l’allemand. Nommez le dossier «de-DE», signifiant «Deutsch (Deutschland)».
2.  Créez un autre fichier de ressources dans le dossier de-DE, puis ajoutez la ligne suivante:

    strings/de-DE/Resources.resw

    ![Ajouter une ressource (pour l’allemand)](images/addresource-de-de.png)


3.  Créez un autre dossier nommé «fr-FR» pour «Français (France)». Créez un fichier de ressources et ajoutez la ligne suivante:

    strings/fr-FR/Resources.resw
    
    ![Ajouter une ressource, français](images/addresource-fr-fr.png)

## <a name="build-and-run-the-app"></a>Générez et exécutez l’application.


Testez l’application dans votre langue d’affichage par défaut.

1.  Appuyez sur la touche F5 pour générer et exécuter l’application.
2.  Notez que les chaînes « greeting » et « farewell » sont affichées dans la langue d’affichage par défaut de l’utilisateur.
3.  Quittez l’application.

Testez l’application avec les autres langues.

1.  Accédez à **Paramètres** sur votre appareil.
2.  Sélectionnez **Heure et langue**.
3.  Sélectionnez **Région et langue** (ou sur un téléphone ou un émulateur de téléphone, **Langue**).
4.  Notez que la langue qui était affichée lorsque vous avez exécuté l’application est la langue principale, c’est-à-dire Anglais, Allemand ou Français. Si votre langue principale n’est pas l’une de ces trois langues, l’application passe à la langue suivante dans la liste qui est prise en charge par l’application.
5.  Si vous ne disposez pas de ces troislangues sur votre appareil, ajoutez les langues manquantes en cliquant sur **Ajouter une langue** pour les ajouter à la liste.
6.  Pour tester l’application avec une autre langue, sélectionnez cette langue dans la liste, puis cliquez sur **Définir par défaut** (ou sur un téléphone ou un émulateur de téléphone, appuyez longuement sur la langue dans la liste, puis appuyez sur **Monter** jusqu’à ce que la langue apparaisse au sommet de la liste). Exécutez ensuite l’application.

## <a name="related-topics"></a>Rubriques connexes


* [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [Comment charger des ressources de type chaîne](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [Balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 



