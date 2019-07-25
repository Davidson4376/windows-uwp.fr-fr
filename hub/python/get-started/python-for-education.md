---
title: Prise en main de Python sur Windows pour les débutants
description: Guide pour vous aider à commencer si votre toute nouvelle utilisation de Python sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: Python, Windows 10, Microsoft, Learning Python, Python sur Windows pour les débutants, installer Python avec Microsoft Store, Python avec vs code, pygame sur Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9ef2349b296e5518d6bbb85a035526d7de25ea5c
ms.sourcegitcommit: 210034519678ba1a59744bc3a0b613b000921537
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68473682"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Prise en main de Python sur Windows pour les débutants

Voici un guide pas-à-pas pour les débutants intéressés par l’apprentissage de Python à l’aide de Windows 10.

## <a name="set-up-your-development-environment"></a>Configurer votre environnement de développement

Pour les débutants qui découvrent Python, nous vous recommandons [d’installer Python à partir de la Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). L’installation via le Microsoft Store utilise l’interpréteur de base Python3, mais gère la configuration de vos paramètres de chemin d’accès pour l’utilisateur actuel (évitant ainsi la nécessité d’un accès administrateur), en plus de fournir des mises à jour automatiques. Cela s’avère particulièrement utile si vous êtes dans un environnement éducatif ou dans une organisation qui restreint les autorisations ou l’accès administratif sur votre ordinateur.

Si vous utilisez Python sur Windows pour le **développement Web**, nous vous recommandons d’utiliser une configuration différente pour votre environnement de développement. Plutôt que d’installer directement sur Windows, nous vous recommandons d’installer et d’utiliser python via le sous-système Windows pour Linux. Pour obtenir de l’aide, consultez: [Prise en main de Python pour le développement Web sur Windows](./python-for-web.md). Si vous souhaitez automatiser des tâches courantes sur votre système d’exploitation, consultez notre guide: [Prise en main de Python sur Windows pour l’écriture de scripts et l’automatisation](./python-for-scripting.md). Pour certains scénarios avancés (tels que la nécessité d’accéder aux fichiers installés de Python ou de les modifier, de créer des copies de fichiers binaires ou d’utiliser directement des dll Python), vous pouvez télécharger une version de Python spécifique directement à partir de [Python.org](https://www.python.org/downloads/) ou envisager d’installer une [alternative](https://www.python.org/download/alternatives), telle que Anaconda, Jython, PyPy, WinPython, IronPython, etc. Nous vous recommandons de le faire uniquement si vous êtes un programmeur Python plus avancé avec une raison spécifique de choisir une autre implémentation.

## <a name="install-python"></a>Installer python

Pour installer Python à l’aide de la Microsoft Store:

1. Accédez à votre menu **Démarrer** (icône Windows en bas à gauche), tapez «Microsoft Store», puis sélectionnez le lien pour ouvrir le magasin.

2. Une fois le magasin ouvert, sélectionnez **Rechercher** dans le menu supérieur droit, puis entrez «Python». Ouvrez «python 3,7» à partir des résultats sous applications. Sélectionnez **récupérer**.

3. Une fois que Python a terminé le processus de téléchargement et d’installation, ouvrez Windows PowerShell à l’aide du menu **Démarrer** (icône Windows en bas à gauche). Une fois PowerShell ouvert, entrez `Python --version` pour confirmer que Python3 a été installé sur votre ordinateur.

4. L’installation Microsoft Store de Python comprend **PIP**, le gestionnaire de package standard. PIP vous permet d’installer et de gérer des packages supplémentaires qui ne font pas partie de la bibliothèque standard Python. Pour confirmer que le PIP est également disponible pour l’installation et la gestion des `pip --version`packages, entrez.

## <a name="install-visual-studio-code"></a>Installer Visual Studio Code

En utilisant vs code comme éditeur de texte/environnement de développement intégré (IDE), vous pouvez tirer parti d' [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (une aide à la saisie semi-automatique du code), de la [(permet](https://code.visualstudio.com/docs/python/linting) d’éviter d’effectuer des erreurs dans votre code), de la [prise en charge](https://code.visualstudio.com/docs/python/debugging) du débogage (vous aide à trouver des erreurs dans votre code après l’avoir exécuté), des [extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modèles pour les petits blocs de code réutilisables) et des [tests unitaires](https://code.visualstudio.com/docs/python/unit-testing) (test de l’interface de votre code avec différents types d’entrée).

VS Code contient également un [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) qui vous permet d’ouvrir une ligne de commande Python à l’aide de l’invite de commandes Windows, de PowerShell ou de tout ce que vous préférez, en établissant un flux de travail transparent entre votre éditeur de code et la ligne de commande.

1. Pour installer VS Code, téléchargez VS Code pour Windows: [https://code.visualstudio.com](https://code.visualstudio.com).

2. Python est un langage interprété, et pour exécuter du code Python, vous devez indiquer VS Code l’interpréteur à utiliser. Nous vous recommandons d’utiliser python 3,7, sauf si vous avez une raison particulière de choisir un autre nom. Sélectionnez un interpréteur python 3 en ouvrant la **palette de commandes** (Ctrl + Maj + P), puis commencez **à taper la commande Python: Sélectionnez interpréteur** à rechercher, puis sélectionnez la commande. Vous pouvez également utiliser l’option **Sélectionner un environnement python** dans la barre d’État inférieure si elle est disponible (il est possible qu’elle affiche déjà un interpréteur sélectionné). La commande présente la liste des interpréteurs disponibles que VS Code peut trouver automatiquement, y compris les environnements virtuels. Si vous ne voyez pas l’interpréteur souhaité, consultez [Configuration des environnements python](https://code.visualstudio.com/docs/python/environments).

    ![Sélectionner l’interpréteur python dans VS Code](../../images/interpreterselection.gif)

3. Pour ouvrir le terminal dans vs code, sélectionnez **Afficher** > le**Terminal**, ou utilisez le raccourci **Ctrl + '** (en utilisant le caractère d’impulsion). Le terminal par défaut est PowerShell.

4. À l’intérieur de votre terminal VS Code, ouvrez Python en entrant simplement la commande:`python`

5. Essayez l’interpréteur Python en entrant: `print("Hello World")`. Python renverra votre instruction «Hello World».

    ![Ligne de commande python dans VS Code](../../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Installer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes sur votre code python ou d’héberger votre projet sur un site Open source (comme GitHub), VS Code prend en charge [le contrôle de version avec git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet contrôle de code source de VS Code effectue le suivi de toutes vos modifications et contient des commandes git communes (ajouter, valider, pousser, tirer) intégrées directement dans l’interface utilisateur. Vous devez d’abord installer Git pour alimenter le panneau de configuration de la source.

1. Téléchargez et installez git pour Windows à partir [du site Web git-SCM](https://git-scm.com/download/win).

2. Vous trouverez un assistant d’installation qui vous posera une série de questions sur les paramètres de votre installation git. Nous vous recommandons d’utiliser tous les paramètres par défaut, sauf si vous avez une raison particulière de modifier un objet.

3. Si vous n’avez jamais travaillé avec git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à commencer.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Didacticiel Hello World pour certains concepts de base de Python

Python, en fonction de son créateur Guido van Rossum, est un «langage de programmation de haut niveau, et sa philosophie de conception principale concerne la lisibilité du code et une syntaxe qui permet aux programmeurs d’exprimer des concepts dans quelques lignes de code.»

Python est un langage interprété. Contrairement aux langages compilés, dans lesquels le code que vous écrivez doit être traduit en code machine afin d’être exécuté par le processeur de votre ordinateur, le code Python est transmis directement à un interpréteur et exécuté directement. Il vous suffit de taper votre code et de l’exécuter. Essayons!

1. Une fois la ligne de commande PowerShell ouverte `python` , entrez pour exécuter l’interpréteur python 3. (Certaines instructions préfèrent utiliser la commande `py` ou `python3`, elles doivent également fonctionner). Vous saurez que vous êtes bien en raison d’un > > > invite avec trois symboles supérieurs.

2. Il existe plusieurs méthodes intégrées qui vous permettent d’apporter des modifications aux chaînes dans Python. Créez une variable, avec: `variable = 'Hello World!'`. Appuyez sur entrée pour une nouvelle ligne.

3. Imprimez votre variable `print(variable)`avec:. Le texte «Hello World!» s’affiche.

4. Déterminez la longueur, le nombre de caractères utilisés, de votre variable de chaîne avec `len(variable)`:. Cela indique qu’il y a 12 caractères utilisés. (Notez que l’espace blanc est compté comme un caractère dans la longueur totale.)

5. Convertissez votre variable de chaîne en lettres majuscules `variable.upper()`:. Convertissez maintenant votre variable de chaîne en minuscules: `variable.lower()`.

6. Compter le nombre de fois que la lettre «l» est utilisée dans votre variable `variable.count("l")`de chaîne:.

7. Recherchez un caractère spécifique dans votre variable de chaîne, recherchons le point d’exclamation, avec: `variable.find("!")`. Cela indique que le point d’exclamation se trouve dans le caractère onzième position de la chaîne.

8. Remplacez le point d’exclamation par un point d’interrogation `variable.replace("!", "?")`:.

9. Pour quitter Python, vous pouvez entrer `exit()`, `quit()`ou sélectionner Ctrl-Z.

![Capture d’écran PowerShell de ce didacticiel](../../images/hello-world-basics.png)

Nous espérons que vous avez amusant à utiliser certaines des méthodes de modification de chaîne intégrées de Python. Essayez maintenant de créer un fichier programme Python et de l’exécuter avec VS Code.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Didacticiel Hello World pour l’utilisation de Python avec VS Code

L’équipe VS Code a rassemblé un excellent [prise en main avec](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) le didacticiel Python, qui explique comment créer un programme Hello World avec Python, exécuter le fichier programme, configurer et exécuter le débogueur, et installer des packages comme *matplotlib* et  *numpy* pour créer un graphique à l’intérieur d’un environnement virtuel.

1. Ouvrez PowerShell et créez un dossier vide appelé «Hello», accédez à ce dossier et ouvrez-le dans VS Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Une fois que VS Code s’ouvre, en affichant votre nouveau dossier *Hello* dans la fenêtre de l' **Explorateur** de gauche, ouvrez une fenêtre de ligne de commande dans le volet inférieur de vs code en appuyant sur **Ctrl + '** (en utilisant le caractère d’impulsion) ou en sélectionnant **affichage**  >  **Terminal**. En démarrant VS Code dans un dossier, ce dossier devient votre «espace de travail». VS Code stocke les paramètres spécifiques à cet espace de travail dans. vscode/Settings. JSON, qui sont distincts des paramètres utilisateur stockés globalement.

3. Poursuivez le didacticiel dans la documentation de VS Code: [Créez un fichier de code source Python Hello World](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Créer un jeu simple avec pygame

![Pygame exécution d’un exemple de jeu](../../images/pygame-shmup.jpg)

Pygame est un package Python populaire pour l’écriture de jeux qui encourage les élèves à apprendre la programmation tout en créant un événement amusant. Pygame affiche les graphiques dans une nouvelle fenêtre. il ne fonctionne donc pas sous l’approche de ligne de commande WSL. Toutefois, si vous avez installé python via le Microsoft Store comme détaillé dans ce didacticiel, il fonctionnera correctement.

1. Une fois que vous avez installé python, installez pygame à partir de la ligne de commande (ou du terminal à `python -m pip install -U pygame --user`partir de vs code) en tapant.

2. Testez l’installation en exécutant un exemple de jeu:`python -m pygame.examples.aliens`

3. Tout se passe bien, le jeu ouvre une fenêtre. Fermez la fenêtre lorsque vous avez terminé.

Voici comment commencer à écrire votre propre jeu.

1. Ouvrez PowerShell (ou l’invite de commandes Windows) et créez un dossier vide appelé «Bounce». Accédez à ce dossier et créez un fichier nommé «bounce.py». Ouvrez le dossier dans VS Code:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. À l’aide de VS Code, entrez le code python suivant (ou copiez-le et collez-le):

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. Enregistrez-le sous `bounce.py`:.

4. À partir du terminal PowerShell, exécutez-le en `python bounce.py`entrant:.

    ![Pygame en cours d’exécution la prochaine étape importante](../../images/pygame.jpg)

Essayez d’ajuster certains nombres pour voir l’effet qu’ils ont sur votre boule de rebond.

Pour en savoir plus sur l’écriture de jeux avec pygame, consultez [pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Ressources pour l’apprentissage continu

Nous vous recommandons de prendre en charge les ressources suivantes pour vous familiariser avec le développement Python sur Windows.

### <a name="online-courses-for-learning-python"></a>Cours en ligne pour l’apprentissage de Python

- [Présentation de Python sur Microsoft Learn](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/): Essayez la plateforme de Microsoft Learn interactive et gagnez des points d’expérience pour terminer ce module en couvrant les principes fondamentaux de l’écriture de code python de base, de la déclaration de variables et de l’utilisation de l’entrée et de la sortie de la console. L’environnement du bac à sable (sandbox) interactif permet de démarrer facilement pour les personnes qui n’ont pas encore installé leur environnement de développement Python.

- [Python sur Pluralsight: 8 cours, 29 heures](https://app.pluralsight.com/paths/skills/python): Le parcours d’apprentissage Python sur Pluralsight offre des cours en ligne couvrant une variété de sujets liés à python, y compris un outil permettant de mesurer vos compétences et de trouver vos lacunes.

- [Didacticiels LearnPython.org](https://www.learnpython.org/): Commencez à apprendre à utiliser Python sans avoir besoin d’installer ou de définir d’autres éléments à l’aide de ces didacticiels interactifs interactifs de la part des gens de DataCamp.

- [Les didacticiels Python.org](https://docs.python.org/3/tutorial/index.html): Introduit le lecteur de manière informelle avec les concepts et fonctionnalités de base du langage et du système Python.

- [Apprentissage de Python sur Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Présentation de base de Python.

### <a name="working-with-python-in-vs-code"></a>Utilisation de Python dans VS Code

- [Modification de Python dans vs code](https://code.visualstudio.com/docs/python/editing): En savoir plus sur la façon de tirer parti de la saisie semi-automatique d’VS Code et de la prise en charge d’IntelliSense pour Python, notamment comment personnaliser leurs behvior... ou simplement les désactiver.

- [Python Lint](https://code.visualstudio.com/docs/python/linting): Le détournement est le processus qui consiste à exécuter un programme qui analysera le code pour identifier les erreurs potentielles. En savoir plus sur les différentes formes de prise en charge du délintment VS Code fournit pour Python et comment le configurer.

- [Débogage de Python](https://code.visualstudio.com/docs/python/debugging): Le débogage est le processus qui consiste à identifier et à supprimer les erreurs d’un programme informatique. Cet article explique comment initialiser et configurer le débogage de Python avec VS Code, comment définir et valider des points d’arrêt, comment attacher un script local, effectuer un débogage pour différents types d’applications ou sur un ordinateur distant, ainsi qu’un dépannage de base.

- [Test unitaire python](https://code.visualstudio.com/docs/python/unit-testing): Décrit un arrière-plan expliquant le fonctionnement des tests unitaires, un exemple de procédure pas à pas, l’activation d’une infrastructure de test, la création et l’exécution de vos tests, le débogage des tests et les paramètres de configuration de test. 
