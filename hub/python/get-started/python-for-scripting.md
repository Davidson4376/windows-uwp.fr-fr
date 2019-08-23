---
title: Écriture de scripts et automatisation avec Python sur Windows
description: Comment prendre en main Python pour l’écriture de scripts, l’automatisation et l’administration de systèmes sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, l’administration de système Python, l’automatisation de fichier Python, les scripts Python sur Windows, la configuration de Python sur Windows, l’environnement de développement Python sur Windows, l’environnement de développement Python sur Windows, Python avec PowerShell, scripts Python pour tâches du système de fichiers
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: dbb7a60103c27f648ca8bf23f87dee06923f0cd9
ms.sourcegitcommit: e9dc2711f0a0758727468f7ccd0d0f0eee3363e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69979329"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Prise en main de Python sur Windows pour l’écriture de scripts et l’automatisation

Voici un guide pas à pas pour la configuration de votre environnement de développement et la prise en main de Python pour l’écriture de scripts et l’automatisation des opérations de système de fichiers sur Windows.

> [!NOTE]
> Cet article traite de la configuration de votre environnement pour utiliser certaines bibliothèques utiles dans Python qui peuvent automatiser des tâches sur plusieurs plateformes, telles que la recherche dans votre système de fichiers, l’accès à Internet, l’analyse des types de fichiers, etc., à partir d’une approche centrée sur Windows. Pour les opérations spécifiques à Windows, consultez [ctypes](https://docs.python.org/3/library/ctypes.html), une bibliothèque de fonctions étrangères compatible C pour Python, [winreg](https://docs.python.org/3/library/winreg.html), et les fonctions qui exposent l’API du Registre Windows à python, et [python/WinRT](https://pypi.org/project/winrt/), qui permettent d’accéder aux API Windows Runtime à partir de Software.

## <a name="set-up-your-development-environment"></a>Configurer votre environnement de développement

Lorsque vous utilisez Python pour écrire des scripts qui effectuent des opérations de système de fichiers, nous vous recommandons [d’installer Python à partir de la Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). L’installation via le Microsoft Store utilise l’interpréteur de base Python3, mais gère la configuration de vos paramètres de chemin d’accès pour l’utilisateur actuel (évitant ainsi la nécessité d’un accès administrateur), en plus de fournir des mises à jour automatiques.

Si vous utilisez Python pour le **développement Web** sur Windows, nous vous recommandons une configuration différente à l’aide du sous-système Windows pour Linux. Vous trouverez une procédure pas à pas dans notre guide: [Prise en main de Python pour le développement Web sur Windows](./python-for-web.md). Si vous êtes novice en Python, essayez notre guide: [Commencez à utiliser Python sur Windows pour les débutants](./python-for-education.md). Pour certains scénarios avancés (tels que la nécessité d’accéder aux fichiers installés de Python ou de les modifier, de créer des copies de fichiers binaires ou d’utiliser directement des dll Python), vous pouvez télécharger une version de Python spécifique directement à partir de [Python.org](https://www.python.org/downloads/) ou envisager d’installer une [alternative](https://www.python.org/download/alternatives), telle que Anaconda, Jython, PyPy, WinPython, IronPython, etc. Nous vous recommandons de le faire uniquement si vous êtes un programmeur Python plus avancé avec une raison spécifique de choisir une autre implémentation.

## <a name="install-python"></a>Installer python

Pour installer Python à l’aide de la Microsoft Store:

1. Accédez à votre menu **Démarrer** (icône Windows en bas à gauche), tapez «Microsoft Store», puis sélectionnez le lien pour ouvrir le magasin.

2. Une fois le magasin ouvert, sélectionnez **Rechercher** dans le menu supérieur droit, puis entrez «Python». Ouvrez «python 3,7» à partir des résultats sous applications. Sélectionnez **récupérer**.

3. Une fois que Python a terminé le processus de téléchargement et d’installation, ouvrez Windows PowerShell à l’aide du menu **Démarrer** (icône Windows en bas à gauche). Une fois PowerShell ouvert, entrez `Python --version` pour confirmer que Python3 a été installé sur votre ordinateur.

4. L’installation Microsoft Store de Python comprend **PIP**, le gestionnaire de package standard. PIP vous permet d’installer et de gérer des packages supplémentaires qui ne font pas partie de la bibliothèque standard Python. Pour confirmer que le PIP est également disponible pour l’installation et la gestion des `pip --version`packages, entrez.

## <a name="install-visual-studio-code"></a>Installer Visual Studio Code

En utilisant vs code comme éditeur de texte/environnement de développement intégré (IDE), vous pouvez tirer parti d'[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (une aide à la saisie semi-automatique du code), de la [Linting](https://code.visualstudio.com/docs/python/linting) (permet d’éviter d’effectuer des erreurs dans votre code), de la [prise en charge du débogage](https://code.visualstudio.com/docs/python/debugging) (vous aide à trouver des erreurs dans votre code après l’avoir exécuté), des [extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modèles pour les petits blocs de code réutilisables) et des [tests unitaires](https://code.visualstudio.com/docs/python/unit-testing) (test de l’interface de votre code avec différents types d’entrée).

Téléchargez VS Code pour Windows et suivez les instructions d’installation [https://code.visualstudio.com](https://code.visualstudio.com):.

## <a name="install-the-microsoft-python-extension"></a>Installer l’extension Microsoft python

Vous devez installer l’extension Python Microsoft afin de tirer parti des fonctionnalités de prise en charge de VS Code. [Plus d’informations](https://code.visualstudio.com/docs/languages/python)

1. Ouvrez la fenêtre extensions de vs code en entrant **Ctrl + Maj + X** (ou utilisez le menu pour accéder aux**Extensions**de **vue** > ).

2. Dans la zone premières **extensions de recherche de la place de marché** , entrez:  **Python**.

3. Recherchez l’extension **Python (MS-Python. Python) par Microsoft** , puis sélectionnez le bouton vert **install** .

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Ouvrez le terminal PowerShell intégré dans VS Code

VS Code contient un [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) qui vous permet d’ouvrir une ligne de commande Python avec PowerShell, en établissant un flux de travail transparent entre votre éditeur de code et la ligne de commande.

1. Ouvrez le terminal dans vs code, sélectionnez **Afficher** > le**Terminal**ou utilisez le raccourci **Ctrl + '** (à l’aide du caractère de soulignement).

    > [!NOTE]
    > Le terminal par défaut doit être PowerShell, mais si vous devez le modifier, utilisez **Ctrl + Maj + P** pour accéder à la commande palette. Entrer **le terminal: Sélectionnez l’interpréteur** de commandes par défaut et une liste d’options de terminal s’affiche avec PowerShell, invite de commandes, WSL, etc. Sélectionnez celui que vous souhaitez utiliser et **Appuyez sur Ctrl + Maj + '** (à l’aide de la touche de raccourci) pour créer un nouveau terminal.

2. À l’intérieur de votre terminal VS Code, ouvrez Python en entrant:`python`

3. Essayez l’interpréteur Python en entrant: `print("Hello World")`. Python renverra votre instruction «Hello World».

    ![Ligne de commande python dans VS Code](../../images/python-in-vscode.png)

4. Pour quitter Python, vous pouvez entrer `exit()`, `quit()`ou sélectionner Ctrl-Z.

## <a name="install-git-optional"></a>Installer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes sur votre code python ou d’héberger votre projet sur un site Open source (comme GitHub), VS Code prend en charge [le contrôle de version avec git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet contrôle de code source de VS Code effectue le suivi de toutes vos modifications et contient des commandes git communes (ajouter, valider, pousser, tirer) intégrées directement dans l’interface utilisateur. Vous devez d’abord installer Git pour alimenter le panneau de configuration de la source.

1. Téléchargez et installez git pour Windows à partir [du site Web git-SCM](https://git-scm.com/download/win).

2. Vous trouverez un assistant d’installation qui vous posera une série de questions sur les paramètres de votre installation git. Nous vous recommandons d’utiliser tous les paramètres par défaut, sauf si vous avez une raison particulière de modifier un objet.

3. Si vous n’avez jamais travaillé avec git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à commencer.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Exemple de script pour afficher la structure de votre répertoire de système de fichiers

Les tâches courantes d’administration du système peuvent prendre beaucoup de temps, mais avec un script Python, vous pouvez automatiser ces tâches afin qu’elles ne prennent aucun moment. Par exemple, Python peut lire le contenu du système de fichiers de votre ordinateur et effectuer des opérations telles que l’impression d’un plan de vos fichiers et répertoires, le déplacement de dossiers d’un répertoire vers un autre, ou le changement de nom de centaines de fichiers. Normalement, les tâches comme celles-ci peuvent prendre beaucoup de temps si vous deviez les exécuter manuellement. Utilisez plutôt un script Python!

Commençons par un script simple qui parcourt une arborescence de répertoires et affiche la structure de répertoires.

1. Ouvrez PowerShell à l’aide du menu **Démarrer** (icône Windows en bas à gauche).

2. Créez un répertoire pour votre projet: `mkdir python-scripts`, puis ouvrez le répertoire suivant `cd python-scripts`:.

3. Créez quelques répertoires à utiliser avec notre exemple de script:

    ```powershell
    mkdir food, food/fruits, food/fruits/apples, food/fruits/oranges, food/vegetables
    ```

4. Créez quelques fichiers dans ces répertoires à utiliser avec notre script:

    ```powershell
    new-item food/fruits/banana.txt, food/fruits/strawberry.txt, food/fruits/blueberry.txt, food/fruits/apples/honeycrisp.txt, food/fruits/oranges/mandarin.txt, food/vegetables/carrot.txt
    ```

5. Créez un nouveau fichier python dans votre répertoire python-scripts:

    ```powershell
    mkdir src
    new-item src/list-directory-contents.py
    ```

6. Ouvrez votre projet dans VS Code en entrant:`code .`

7. Ouvrez la fenêtre Explorateur de fichiers vs code en entrant **CTRL + MAJ + E** (ou utilisez le menu pour accéder à l'**Explorateur**d' **affichage** > ) et sélectionnez le fichier List-Directory-contents.py que vous venez de créer. L’extension Python Microsoft chargera automatiquement un interpréteur Python. Vous pouvez voir quel interpréteur a été chargé en bas de votre fenêtre de VS Code.

    > [!NOTE]
    > Python est un langage interprété, ce qui signifie qu’il joue le rôle d’ordinateur virtuel, en émulant un ordinateur physique. Vous pouvez utiliser différents types d’interpréteurs Python: Python 2, Python 3, Anaconda, PyPy, etc. Afin d’exécuter le code Python et d’accéder à python IntelliSense, vous devez indiquer VS Code l’interpréteur à utiliser. Nous vous recommandons d’utiliser l’interpréteur que VS Code choisit par défaut (Python 3 dans notre cas), à moins que vous n’ayez une raison particulière de choisir un autre nom. Pour modifier l’interpréteur Python, sélectionnez l’interpréteur actuellement affiché dans la barre bleue au bas de la fenêtre de vs code ou ouvrez la **palette de commandes** (Ctrl + Maj + P) **et entrez la commande Python: Sélectionnez interpréteur**. Cette opération affiche une liste des interpréteurs python que vous avez installés actuellement. [En savoir plus sur la configuration des environnements python](https://code.visualstudio.com/docs/python/environments).

    ![Sélectionner l’interpréteur python dans VS Code](../../images/interpreterselection.gif)

8. Collez le code suivant dans votre fichier list-directory-contents.py, puis sélectionnez **Enregistrer**:

    ```python
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory: ' + directory)
        for name in subdir_list:
            print ('Subdirectory: ' + name)
        for name in file_list:
            print('File: ' + name)
        print(os.linesep)
    ```

9. Ouvrez le terminal intégré VS Code (**Ctrl + '** , à l’aide du caractère d’impulsion) et entrez le répertoire src dans lequel vous venez d’enregistrer votre script Python:

    ```powershell
    cd src
    ```

10. Exécutez le script dans PowerShell avec:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    La sortie doit ressembler à ceci:

    ```powershell
    Directory: ../food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ../food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ../food\fruits\apples
    File: honeycrisp.txt

    Directory: ../food\fruits\oranges
    File: mandarin.txt

    Directory: ../food\vegetables
    File: carrot.txt
    ```

11. Utilisez Python pour imprimer la sortie de répertoire du système de fichiers dans son propre fichier texte en entrant cette commande directement dans votre terminal PowerShell:`python3 list-directory-contents.py > food-directory.txt`

Félicitations ! Vous venez d’écrire un script d’administration de systèmes automatisé qui lit le répertoire et les fichiers que vous avez créés et utilise Python pour afficher, puis imprimer, la structure de répertoires dans son propre fichier texte.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Exemple de script pour modifier tous les fichiers d’un répertoire

Cet exemple utilise les fichiers et répertoires que vous venez de créer, en renommant chacun des fichiers en ajoutant la date de dernière modification du fichier au début du nom de fichier.

1. Dans le dossier **src** de votre répertoire **python-scripts** , créez un nouveau fichier Python pour votre script:

    ```powershell
    new-item update-filenames.py
    ```

2. Ouvrez le fichier update-filenames.py, collez le code suivant dans le fichier, puis enregistrez-le:

    > [!NOTE]
    > OS. getMTime retourne un horodatage en graduations, ce qui n’est pas facile à lire. Elle doit d’abord être convertie en chaîne DateTime standard.

    ```python
    import datetime
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = '%s%s%s' % (directory, os.path.sep, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = '%s%s%s_%s' % (directory, os.path.sep, modified_date, name)

            print ('Renaming: %s to: %s' % (source_name, target_name))

            os.rename(source_name, target_name)
    ```

3. Testez votre script Update-FileNames.py en l’exécutant `python3 update-filenames.py` : puis en exécutant à nouveau votre script List-Directory-contents.py:`python3 list-directory-contents.py`

4. La sortie doit ressembler à ceci:

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    ~/src/python-scripting/src$ python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Utilisez Python pour imprimer les nouveaux noms de répertoires de système de fichiers avec l’horodateur Last-modified dans son propre fichier texte en entrant cette commande directement dans votre terminal PowerShell:`python3 list-directory-contents.py > food-directory-last-modified.txt`

Nous espérons que vous avez appris quelques choses amusantes sur l’utilisation de scripts Python pour automatiser les tâches d’administration de base des systèmes. Il y a bien sûr un grand nombre à savoir, mais nous espérons que cela vous a fait démarrer dans le bon pied. Nous avons partagé quelques ressources supplémentaires pour poursuivre l’apprentissage ci-dessous.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Documents Python: Accès aux fichiers et](https://docs.python.org/3.7/library/filesys.html)aux répertoires: Documentation Python sur l’utilisation des systèmes de fichiers et l’utilisation de modules pour lire les propriétés des fichiers, manipuler des chemins d’accès de manière portable et créer des fichiers temporaires.
- [Découvrez Python: Didacticiel](https://www.learnpython.org/en/String_Formatting)String_Formatting: En savoir plus sur l’utilisation de l’opérateur «%» pour la mise en forme des chaînes.
- [10 méthodes de système de fichiers Python à connaître](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): Article de taille moyenne sur la manipulation de fichiers `os` et `shutil`de dossiers avec et.
- [Guide Hitchhikers pour Python: Administration](https://docs.python-guide.org/scenarios/admin/)des systèmes: Un «guide consignes strictes» qui offre des vues d’ensemble et des meilleures pratiques sur les sujets liés à python. Cette section traite des outils et infrastructures d’administration système. Ce guide est hébergé sur GitHub pour vous permettre de créer des fichiers et de faire des contributions.
