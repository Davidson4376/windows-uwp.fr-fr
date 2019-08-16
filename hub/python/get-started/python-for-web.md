---
title: Développement Web avec Python sur Windows
description: Comment prendre en main Python pour le développement Web sur Windows, y compris pour la configuration d’infrastructures comme flacons et Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python sur Windows, Python Web avec WSL, application Web Python avec sous-système Windows pour Linux, développement Web Python sur Windows, application flacon sur Windows, application Django sur Windows, Python Web, Web dev sur Windows, Django Web dev sur Windows, Windows Web dev avec Python, vs code python Web dev, extension WSL distante, Ubuntu, WSL, venv, PIP, extension Python Microsoft, exécuter Python sur Windows, utiliser Python sur Windows, générer avec Python sur Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fa6da9f5151d9457aafd255c9d10c91e3d219cee
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959083"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Prise en main de Python pour le développement Web sur Windows

Vous trouverez ci-dessous un guide pas à pas pour vous familiariser avec l’utilisation de Python pour le développement Web sur Windows, à l’aide du sous-système Windows pour Linux (WSL).

## <a name="set-up-your-development-environment"></a>Configurer votre environnement de développement

Nous vous recommandons d’installer Python sur WSL lors de la création d’applications Web. Un grand nombre des didacticiels et des instructions pour le développement Web Python sont écrits pour les utilisateurs Linux et utilisent des outils d’installation et d’empaquetage basés sur Linux. La plupart des applications Web sont également déployées sur Linux, ce qui permet de garantir la cohérence entre vos environnements de développement et de production.

Si vous utilisez Python pour un autre langage que le développement Web, nous vous recommandons d’installer python directement sur Windows 10 à l’aide du Microsoft Store. WSL ne prend pas en charge les applications ou les ordinateurs de bureau GUI (tels que PyGame, GNOME, KDE, etc.). Dans ce cas, installez et utilisez python directement sur Windows. Si vous débutez avec Python, consultez notre guide: [Commencez à utiliser Python sur Windows pour les débutants](./python-for-education.md). Si vous souhaitez automatiser des tâches courantes sur votre système d’exploitation, consultez notre guide: [Prise en main de Python sur Windows pour l’écriture de scripts et l’automatisation](./python-for-scripting.md). Pour certains scénarios avancés, vous souhaiterez peut-être télécharger une version de Python spécifique directement à partir de [Python.org](https://www.python.org/downloads/windows/) ou envisager d’installer une [alternative](https://www.python.org/download/alternatives), telle que Anaconda, Jython, PyPy, WinPython, IronPython, etc. Nous vous recommandons de le faire uniquement si vous êtes un programmeur Python plus avancé avec une raison spécifique de choisir une autre implémentation.

## <a name="enable-windows-subsystem-for-linux"></a>Activer le sous-système Windows pour Linux

WSL vous permet d’exécuter un environnement GNU/Linux, y compris la plupart des outils en ligne de commande, des utilitaires et des applications, directement sur Windows, non modifiés et entièrement intégrés à votre système de fichiers Windows et à des outils favoris comme Visual Studio Code. Avant d’activer WSL, vérifiez que vous disposez [de la version la plus récente de Windows 10](https://www.microsoft.com/software-download/windows10).

Pour activer WSL sur votre ordinateur, vous devez:

1. Accédez à votre menu **Démarrer** (icône Windows en bas à gauche), tapez «activer ou désactiver des fonctionnalités Windows», puis sélectionnez le lien vers le **panneau** de configuration pour ouvrir le menu contextuel **fonctionnalités de Windows** . Recherchez «sous-système Windows pour Linux» dans la liste et cochez la case pour activer la fonctionnalité.

2. Redémarrez votre ordinateur lorsque vous y êtes invité.

## <a name="install-a-linux-distribution"></a>Installer une distribution Linux

Plusieurs distributions Linux peuvent être exécutées sur WSL. Vous pouvez rechercher et installer votre favori dans le Microsoft Store. Nous vous recommandons de commencer avec [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , tel qu’il est actuel, populaire et bien pris en charge.

1. Ouvrez ce lien [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , ouvrez le Microsoft Store, puis sélectionnez **acquérir**. *(Il s’agit d’un téléchargement assez volumineux et l’installation peut prendre un certain temps.)*

2. Une fois le téléchargement terminé, sélectionnez **lancer** à partir de la Microsoft Store ou lancez en tapant «Ubuntu 18,04 LTS» dans le menu **Démarrer** .

3. Vous êtes invité à créer un nom de compte et un mot de passe lorsque vous exécutez la distribution pour la première fois. Après cela, vous serez automatiquement connecté en tant que cet utilisateur par défaut. Vous pouvez choisir n’importe quel nom d’utilisateur et mot de passe. Ils n’ont aucun impact sur votre nom d’utilisateur Windows.

Vous pouvez vérifier la distribution Linux que vous utilisez actuellement en entrant ce qui `lsb_release -d`suit:. Pour mettre à jour votre distribution Ubuntu, `sudo apt update && sudo apt upgrade`utilisez:. Nous vous recommandons de procéder à une mise à jour régulière pour vous assurer que vous disposez des packages les plus récents. Windows ne gère pas automatiquement cette mise à jour. Pour obtenir des liens vers d’autres distributions Linux disponibles dans le Microsoft Store, des méthodes d’installation alternatives ou la résolution des problèmes, consultez le [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="set-up-visual-studio-code"></a>Configurer Visual Studio Code

Tirez parti d' [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), de la [défragmentation](https://code.visualstudio.com/docs/python/linting), de la [prise en chargedu débogage](https://code.visualstudio.com/docs/python/debugging), des [extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) et des [tests unitaires](https://code.visualstudio.com/docs/python/unit-testing) à l’aide de vs code. VS Code s’intègre parfaitement au sous-système Windows pour Linux, en fournissant un [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) pour établir un flux de travail transparent entre votre éditeur de code et votre ligne de commande, en plus de la prise en charge [de Git pour le contrôle de version](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) avec git commun les commandes (Add, Commit, push, pull) sont intégrées directement dans l’interface utilisateur.

1. [Téléchargez et installez vs code pour Windows](https://code.visualstudio.com). VS Code est également disponible pour Linux, mais le sous-système Windows pour Linux ne prend pas en charge les applications GUI. nous devons donc l’installer sur Windows. Ne vous inquiétez pas, vous serez toujours en mesure de l’intégrer à vos outils et à la ligne de commande Linux à l’aide de l’extension WSL distante.

2. Installez l' [extension WSL à distance](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) sur vs code. Cela vous permet d’utiliser WSL comme environnement de développement intégré et de gérer la compatibilité et le chemin d’accès pour vous. [Plus d’informations](https://code.visualstudio.com/docs/remote/remote-overview)

> [!IMPORTANT]
> Si vous avez déjà VS Code installé, vous devez vous assurer que vous disposez de la [version 1,35](https://code.visualstudio.com/updates/v1_35) ou ultérieure pour pouvoir installer l' [extension WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)distante. Nous vous déconseillons d’utiliser WSL dans VS Code sans l’extension WSL distante, car vous perdrez la prise en charge de la saisie semi-automatique, du débogage, du découpage, etc. Fait amusant: Cette extension WSL est installée dans $HOME/.vscode-Server/Extensions.

## <a name="create-a-new-project"></a>Créer un nouveau projet

Nous allons créer un nouveau répertoire de projet sur notre système de fichiers Linux (Ubuntu) sur lequel nous travaillerons avec les applications et les outils Linux à l’aide de VS Code.

1. Fermez VS Code et ouvrez Ubuntu 18,04 (votre ligne de commande WSL) en accédant à votre menu **Démarrer** (icône Windows en bas à gauche) et en tapant ce qui suit: «Ubuntu 18,04».

2. Dans votre ligne de commande Ubuntu, accédez à l’emplacement où vous souhaitez placer votre projet et créez un répertoire pour celui `mkdir HelloWorld`-ci:.

![Terminal Ubuntu](../../images/ubuntu-terminal.png)

> [!TIP]
> Une chose importante à retenir lors de l’utilisation du sous-système Windows pour Linux (WSL) est que **vous travaillez maintenant entre deux systèmes de fichiers différents**: 1) votre système de fichiers Windows, et 2) votre système de fichiers Linux (WSL), qui est Ubuntu pour notre exemple. Vous devez faire attention à l’emplacement d’installation des packages et des fichiers de stockage. Vous pouvez installer une version d’un outil ou d’un package dans le système de fichiers Windows et une version complètement différente dans le système de fichiers Linux. La mise à jour de l’outil dans le système de fichiers Windows n’a aucun effet sur l’outil dans le système de fichiers Linux, et vice versa. WSL monte les lecteurs fixes sur votre ordinateur dans le dossier/mnt/<drive> de votre distribution Linux. Par exemple, votre lecteur Windows C: est monté sous `/mnt/c/`. Vous pouvez accéder à vos fichiers Windows à partir du terminal Ubuntu et utiliser des applications et des outils Linux sur ces fichiers, et vice-versa. Nous vous recommandons de travailler dans le système de fichiers Linux pour le développement Web Python, étant donné que la plupart des outils Web sont écrits à l’origine pour Linux et déployés dans un environnement de production Linux. Il évite également de mélanger la sémantique du système de fichiers (comme Windows ne respectant pas la casse en ce qui concerne les noms de fichiers). Cela dit, WSL prend désormais en charge le passage entre les systèmes de fichiers Linux et Windows, ce qui vous permet d’héberger vos fichiers sur l’un ou l’autre. [Plus d’informations](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/) Nous sommes également ravis de partager cette WSL2 qui sera [bientôt disponible pour Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) et offrira des améliorations exceptionnelles. Vous pouvez l' [essayer maintenant sur Windows Insiders build 18917](https://docs.microsoft.com/windows/wsl/wsl2-install).

## <a name="install-python-pip-and-venv"></a>Installer Python, PIP et venv

Ubuntu 18,04 LTS est fourni avec Python 3,6 déjà installé, mais il n’est pas fourni avec certains des modules que vous pouvez vous attendre à obtenir avec d’autres installations Python. Nous devons toujours installer **PIP**, le gestionnaire de package standard pour Python et **venv**, le module standard utilisé pour créer et gérer des environnements virtuels légers.  

1. Vérifiez que Python3 est déjà installé en ouvrant votre terminal Ubuntu et en entrant `python3 --version`:. Le numéro de votre version de Python doit être retourné. Si vous devez mettre à jour votre version de Python, commencez par mettre à jour votre version `sudo apt update && sudo apt upgrade`d’Ubuntu en entrant: `sudo apt upgrade python3`, puis mettez à jour Python à l’aide de.

2. Installez **PIP** en entrant: `sudo apt install python3-pip`. PIP vous permet d’installer et de gérer des packages supplémentaires qui ne font pas partie de la bibliothèque standard Python.

3. Installez **venv** en entrant: `sudo apt install python3-venv`.

## <a name="create-a-virtual-environment"></a>Créer un environnement virtuel

L’utilisation d’environnements virtuels est une bonne pratique recommandée pour les projets de développement Python. En créant un environnement virtuel, vous pouvez isoler vos outils de projet et éviter les conflits de contrôle de version avec les outils pour vos autres projets. Par exemple, vous pouvez conserver un ancien projet Web qui requiert l’infrastructure Web Django 1,2, mais un nouveau projet passionnant s’accompagne de l’utilisation de Django 2,2. Si vous mettez à jour Django globalement, en dehors d’un environnement virtuel, vous risquez de rencontrer des problèmes de version plus tard. En plus de prévenir les conflits de contrôle de version accidentels, les environnements virtuels vous permettent d’installer et de gérer des packages sans privilèges administratifs.

1. Ouvrez votre terminal et, à l’intérieur de votre dossier de projet *HelloWorld* , utilisez la commande suivante pour créer un environnement virtuel `python3 -m venv .venv`nommé **. venv**:.

2. Pour activer l’environnement virtuel, entrez: `source .venv/bin/activate`. Si cela fonctionne, vous devez voir **(. venv)** avant l’invite de commandes. Vous disposez maintenant d’un environnement autonome prêt pour l’écriture de code et l’installation de packages. Lorsque vous avez terminé avec votre environnement virtuel, entrez la commande suivante pour le désactiver: `deactivate`.

    ![Créer un environnement virtuel](../../images/wsl-venv.png)

> [!TIP]
> Nous vous recommandons de créer l’environnement virtuel dans le répertoire dans lequel vous prévoyez de disposer de votre projet. Étant donné que chaque projet doit avoir son propre répertoire distinct, chacun d’eux aura son propre environnement virtuel. il n’est donc pas nécessaire d’attribuer un nom unique. Nous vous suggérons d’utiliser le nom **. venv** pour suivre la Convention Python. Certains outils (tels que pipenv) ont également par défaut ce nom si vous installez dans le répertoire de votre projet. Vous ne souhaitez pas utiliser **. env** comme ce qui est en conflit avec les fichiers de définition de variable d’environnement. En règle générale, nous ne recommandons pas de noms de début non-point, `ls` car vous n’avez pas besoin de vous rappeler que le répertoire existe. Nous vous recommandons également d’ajouter **. venv** à votre fichier. gitignore. (Voici le [modèle gitignore par défaut de GitHub pour Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) pour référence.) Pour plus d’informations sur l’utilisation des environnements virtuels dans VS Code, consultez [utilisation d’environnements python dans vs code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Ouvrir une fenêtre WSL-Remote

VS Code utilise l’extension WSL distante (installée précédemment) pour traiter votre sous-système Linux en tant que serveur distant. Cela vous permet d’utiliser WSL comme environnement de développement intégré. [Plus d’informations](https://code.visualstudio.com/docs/remote/wsl) 

1. Ouvrez votre dossier de projet dans vs code à partir de votre terminal Ubuntu `code .` en entrant: (le «.» indique à vs code d’ouvrir le dossier actif).

2. Une alerte de sécurité s’affiche à partir de Windows Defender, sélectionnez «autoriser l’accès». Une fois vs code s’ouvre, l’indicateur de l’hôte de connexion à distance doit s’afficher dans le coin inférieur gauche, vous indiquant que vous **modifiez sur WSL: Ubuntu-18,04**.

    ![VS Code indicateur de l’hôte de connexion à distance](../../images/wsl-remote-extension.png)

3. Fermez votre terminal Ubuntu. En avançant, nous utilisons le terminal WSL intégré dans VS Code.

4. Ouvrez le terminal WSL dans vs code en appuyant sur **Ctrl + '** (en utilisant le caractère d’impulsion) ou en sélectionnant **Afficher** > le**Terminal**. Une ligne de commande bash (WSL) ouverte dans le chemin d’accès au dossier du projet que vous avez créé dans votre terminal Ubuntu s’ouvre.

    ![VS Code avec Terminal WSL](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Installer l’extension Microsoft python

Vous devez installer les extensions de VS Code pour votre WSL à distance. Les extensions déjà installées localement sur VS Code ne seront pas automatiquement disponibles. [Plus d’informations](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)

1. Ouvrez la fenêtre extensions de vs code en entrant **Ctrl + Maj + X** (ou utilisez le menu pour accéder aux**Extensions**de **vue** > ).

2. Dans la zone premières **extensions de recherche de la place de marché** , entrez:  **Python**.

3. Recherchez l’extension **Python (MS-Python. Python) par Microsoft** , puis sélectionnez le bouton vert **install** .

4. Une fois l’installation de l’extension terminée, vous devrez sélectionner le bouton Blue **Reload required** . Cela permet de recharger vs code et d’afficher **un WSL: La section Ubuntu-18,04** -installed de la fenêtre extensions de vs code indique que vous avez installé l’extension Python.

## <a name="run-a-simple-python-program"></a>Exécuter un programme python simple

Python est un langage interprété et prend en charge différents types d’interpréteurs (Python2, Anaconda, PyPy, etc.). VS Code doit avoir comme valeur par défaut l’interpréteur associé à votre projet. Si vous avez une raison de la modifier, sélectionnez l’interpréteur actuellement affiché dans la barre bleue au bas de la fenêtre de vs code ou ouvrez la **palette de commandes** (Ctrl + Maj + P) et **entrez la commande Python: Sélectionnez interpréteur**. Cette opération affiche une liste des interpréteurs python que vous avez installés actuellement. [En savoir plus sur la configuration des environnements python](https://code.visualstudio.com/docs/python/environments).

Nous allons créer et exécuter un programme python simple en tant que test et vérifier que l’interpréteur python correct est sélectionné.

1. Ouvrez la fenêtre Explorateur de fichiers vs code en entrant **CTRL + MAJ + E** (ou utilisez le menu pour accéder à l'**Explorateur**d' **affichage** > ).

2. S’il n’est pas déjà ouvert, ouvrez votre terminal WSL intégré en appuyant sur **Ctrl + Maj + '** et assurez-vous que votre dossier de projet python **HelloWorld** est sélectionné.

3. Créez un fichier Python en entrant: `touch test.py`. Le fichier que vous venez de créer s’affiche dans la fenêtre de l’Explorateur sous les dossiers. venv et. vscode qui se trouvent déjà dans le répertoire de votre projet.

4. Sélectionnez le fichier **test.py** que vous venez de créer dans votre fenêtre d’explorateur pour l’ouvrir dans vs code. Étant donné que le fichier. py dans notre nom de fichier indique VS Code qu’il s’agit d’un fichier Python, l’extension Python que vous avez chargée précédemment choisit et charge automatiquement un interpréteur Python qui s’affichera en bas de votre fenêtre de VS Code.

    ![Sélectionner l’interpréteur python dans VS Code](../../images/interpreterselection.gif)

5. Collez ce code python dans votre fichier test.py, puis enregistrez le fichier (Ctrl + S): 

    ```python
    print("Hello World")
    ```

6. Pour exécuter le programme «Hello World» python que nous venons de créer, sélectionnez le fichier **test.py** dans la fenêtre de l’explorateur de vs code, puis cliquez avec le bouton droit sur le fichier pour afficher un menu d’options. Sélectionnez **exécuter le fichier python dans le terminal**. Vous pouvez également, dans votre fenêtre de terminal WSL intégrée, `python test.py` entrer: pour exécuter votre programme «Hello World». L’interpréteur python affiche «Hello World» dans votre fenêtre de terminal.

Félicitations ! Vous êtes tous configurés pour créer et exécuter des programmes Python! Essayons à présent de créer une application Hello World avec deux des frameworks web python les plus populaires: Flacon et Django.

## <a name="hello-world-tutorial-for-flask"></a>Didacticiel Hello World pour le flacon

Le [ballon](http://flask.pocoo.org/) est une infrastructure d’application Web pour Python. Dans ce bref didacticiel, vous allez créer une petite application de ballon «Hello World» à l’aide de VS Code et WSL.

1. Ouvrez Ubuntu 18,04 (votre ligne de commande WSL) en accédant à votre menu **Démarrer** (icône Windows en bas à gauche) et en tapant ce qui suit: «Ubuntu 18,04».

2. Créez un répertoire pour votre projet: `mkdir HelloWorld-Flask`, puis `cd HelloWorld-Flask` pour entrer le répertoire.

3. Créez un environnement virtuel pour installer les outils de votre projet:`python3 -m venv .venv`

4. Ouvrez votre projet **HelloWorld-flacon** dans vs code en entrant la commande suivante:`code .`

5. Dans VS Code, ouvrez votre terminal WSL intégré (appelé bash) en entrant **Ctrl + Maj + '** (votre dossier de projet de **ballon HelloWorld** doit déjà être sélectionné). *Fermez votre ligne de commande Ubuntu, car nous allons travailler dans le terminal WSL intégré à VS Code progresser.*

6. Activez l’environnement virtuel que vous avez créé à l’étape #3 à l’aide de votre `source .venv/bin/activate`terminal bash dans vs code:. Si cela fonctionne, vous devez voir (. venv) avant l’invite de commandes.

7. Installez le flacon dans l’environnement virtuel en entrant `python3 -m pip install flask`:. Vérifiez qu’il est installé en entrant: `python3 -m flask --version`.

8. Créez un nouveau fichier pour votre code Python:`touch app.py`

9. Ouvrez votre fichier **app.py** dans l’Explorateur de fichiers de`Ctrl+Shift+E`vs code (, puis sélectionnez votre fichier app.py). L’extension Python est alors activée pour choisir un interpréteur. Il doit par défaut avoir la valeur **python 3.6.8 64-bit («. venv»: venv)** . Notez qu’il a également détecté votre environnement virtuel.

    ![Environnement virtuel activé](../../images/virtual-environment.png)

10. Dans **app.py**, ajoutez le code pour importer le flacon et créez une instance de l’objet de la fiole:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. En outre, dans **app.py**, ajoutez une fonction qui retourne du contenu, dans ce cas une chaîne simple. Utilisez l’élément décoratif **app. route** du flacon pour mapper l’itinéraire de l’URL «/» à cette fonction:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Vous pouvez utiliser plusieurs décorateurs sur la même fonction, un par ligne, selon le nombre d’itinéraires que vous souhaitez mapper à la même fonction.

12. Enregistrez le fichier **app.py** (**CTRL + S**).

13. Dans le terminal, exécutez l’application en entrant la commande suivante:

    ```python
    python3 -m flask run
    ```

    Le serveur de développement de la fiole est exécuté. Le serveur de développement recherche **app.py** par défaut. Quand vous exécutez le flacon, vous devez obtenir une sortie similaire à ce qui suit:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Ouvrez votre navigateur Web par défaut sur la page affichée, **Ctrl + clic sur** l' http://127.0.0.1:5000/ URL dans le terminal. Le message suivant doit s’afficher dans votre navigateur:

    ![Bonjour, fiole!](../../images/hello-flask.png)

15. Notez que lorsque vous visitez une URL telle que «/», un message s’affiche dans le terminal de débogage indiquant la requête HTTP:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Arrêtez l’application à l’aide de la **combinaison de touches Ctrl + C** dans le terminal.

> [!TIP]
> Si vous souhaitez utiliser un nom de fichier différent de **app.py**, tel que **Program.py**, définissez une variable d’environnement nommée **FLASK_APP** et définissez sa valeur sur le fichier que vous avez choisi. Le serveur de développement du flacon utilise ensuite la valeur de **FLASK_APP** à la place du fichier par défaut **app.py**. Pour plus d’informations, consultez la [documentation de l’interface de ligne de commande du flacon](http://flask.pocoo.org/docs/1.0/cli/).

Félicitations, vous avez créé une application Web de ballon à l’aide de Visual Studio Code et du sous-système Windows pour Linux! Pour obtenir un didacticiel plus détaillé sur l’utilisation de VS Code et de ballons, consultez le [didacticiel sur les fioles dans Visual Studio code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Didacticiel Hello World pour Django

[Django](https://www.djangoproject.com) est une infrastructure d’application Web pour Python. Dans ce bref didacticiel, vous allez créer une petite application «Hello World» Django à l’aide de VS Code et WSL.

1. Ouvrez Ubuntu 18,04 (votre ligne de commande WSL) en accédant à votre menu **Démarrer** (icône Windows en bas à gauche) et en tapant ce qui suit: «Ubuntu 18,04».

2. Créez un répertoire pour votre projet: `mkdir HelloWorld-Django`, puis `cd HelloWorld-Django` pour entrer le répertoire.

3. Créez un environnement virtuel pour installer les outils de votre projet:`python3 -m venv .venv`

4. Ouvrez votre projet **HelloWorld-Django** dans vs code en entrant la commande suivante:`code .`

5. Dans VS Code, ouvrez votre terminal WSL intégré (appelé bash) en entrant **Ctrl + Maj + '** (votre dossier de projet **HelloWorld-Django** doit déjà être sélectionné). *Fermez votre ligne de commande Ubuntu, car nous allons travailler dans le terminal WSL intégré à VS Code progresser.*

6. Activez l’environnement virtuel que vous avez créé à l’étape #3 à l’aide de votre `source .venv/bin/activate`terminal bash dans vs code:. Si cela fonctionne, vous devez voir (. venv) avant l’invite de commandes.

7. Installez Django dans l’environnement virtuel avec la commande: `python3 -m pip install django`. Vérifiez qu’il est installé en entrant: `python3 -m django --version`.

8. Ensuite, exécutez la commande suivante pour créer le projet Django:

    ```bash
    django-admin startproject web_project .
    ```

    La `startproject` commande suppose (à `.` la fin) que le dossier actif est votre dossier de projet et crée les éléments suivants dans celui-ci:

    - `manage.py`: Utilitaire d’administration en ligne de commande Django pour le projet. Vous exécutez des commandes d’administration pour le `python manage.py <command> [options]`projet à l’aide de.

    - Un sous-dossier `web_project`nommé, qui contient les fichiers suivants:
        - `__init__.py`: fichier vide qui indique à python que ce dossier est un package Python.
        - `wsgi.py`: un point d’entrée pour les serveurs Web compatibles WSGI pour servir votre projet. En règle générale, vous laissez ce fichier tel quel car il fournit les raccordements pour les serveurs Web de production.
        - `settings.py`: contient les paramètres du projet Django, que vous modifiez au cours du développement d’une application Web.
        - `urls.py`: contient une table des matières pour le projet Django, que vous modifiez également dans le cadre du développement.

9. Pour vérifier le projet Django, démarrez le serveur de développement Django à l' `python3 manage.py runserver`aide de la commande. Le serveur s’exécute sur le port par défaut 8000, et une sortie similaire à la sortie suivante doit s’afficher dans la fenêtre de terminal:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Quand vous exécutez le serveur la première fois, il crée une base de données SQLite par défaut `db.sqlite3`dans le fichier, qui est destinée à des fins de développement, mais peut être utilisée en production pour les applications Web à faible volume. En outre, le serveur Web intégré de Django est destiné *uniquement* à des fins de développement local. Toutefois, lorsque vous déployez sur un hôte Web, Django utilise le serveur Web de l’hôte à la place. Le `wsgi.py` module dans le projet Django s’occupe du raccordement aux serveurs de production.

    Si vous souhaitez utiliser un autre port que le port par défaut 8000, spécifiez le numéro de port sur la ligne de `python3 manage.py runserver 5000`commande, par exemple.

10. `Ctrl+click``http://127.0.0.1:8000/` URL dans la fenêtre de sortie de terminal pour ouvrir votre navigateur par défaut à cette adresse. Si Django est installé correctement et que le projet est valide, une page par défaut s’affiche. La fenêtre sortie de terminal VS Code affiche également le journal du serveur.

11. Lorsque vous avez terminé, fermez la fenêtre du navigateur et arrêtez le serveur dans vs code `Ctrl+C` à l’aide de, comme indiqué dans la fenêtre sortie de terminal.

12. Maintenant, pour créer une application Django, exécutez la `startapp` commande de l’utilitaire d’administration dans le dossier de votre projet (où `manage.py` réside):

    ```bash
    python3 manage.py startapp hello
    ```

    La commande crée un dossier nommé `hello` qui contient un certain nombre de fichiers de code et un sous-dossier. Parmi celles-ci, vous travaillez `views.py` fréquemment avec (qui contient les fonctions qui définissent des pages dans `models.py` votre application Web) et (qui contient des classes définissant vos objets de données). Le `migrations` dossier est utilisé par l’utilitaire d’administration de Django pour gérer les versions de base de données, comme indiqué plus loin dans ce didacticiel. Il y a également les `apps.py` fichiers (configuration d’application `admin.py` ), (pour la création d’une interface `tests.py` d’administration) et (pour les tests), qui ne sont pas couverts ici.

13. Modifiez `hello/views.py` pour qu’il corresponde au code suivant, qui crée une vue unique pour la page d’hébergement de l’application:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Créez un fichier, `hello/urls.py`, avec le contenu ci-dessous. Le `urls.py` fichier est l’emplacement où vous spécifiez des modèles pour acheminer des URL différentes vers leurs vues appropriées. Le code ci-dessous contient un itinéraire pour mapper l’URL racine de`""`l’application ( `views.home` ) à la fonction que vous `hello/views.py`venez d’ajouter à:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. Le `web_project` dossier contient également un `urls.py` fichier, où le routage d’URL est effectivement géré. Ouvrez `web_project/urls.py` et modifiez-le pour qu’il corresponde au code suivant (vous pouvez conserver les commentaires instructifs si vous le souhaitez). Ce code extrait l’utilisation `hello/urls.py` `django.urls.include`de l’application, ce qui permet de conserver les itinéraires de l’application contenus dans l’application. Cette séparation est utile lorsqu’un projet contient plusieurs applications.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Enregistrez tous les fichiers modifiés.

17. Dans le terminal vs code, exécutez le serveur de développement `python manage.py runserver` avec et ouvrez un navigateur `http://127.0.0.1:8000/` pour afficher une page qui affiche «Hello, Django».

Félicitations, vous avez créé une application Web Django à l’aide de VS Code et du sous-système Windows pour Linux! Pour obtenir un didacticiel plus détaillé sur l’utilisation de VS Code et Django, consultez le [didacticiel Django dans Visual Studio code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Didacticiel Python avec vs code](https://code.visualstudio.com/docs/python/python-tutorial): Didacticiel d’introduction à VS Code en tant qu’environnement Python, principalement comment modifier, exécuter et déboguer du code.
- [Prise en charge de git dans vs code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Découvrez comment utiliser les concepts de base du contrôle de version git dans VS Code.  
- [En savoir plus sur les mises à jour bientôt disponibles avec WSL 2!](https://docs.microsoft.com/windows/wsl/wsl2-index): Cette nouvelle version modifie la façon dont les distributions Linux interagissent avec Windows, ce qui améliore les performances du système de fichiers et ajoute la compatibilité complète des appels système.
- [Utilisation de plusieurs distributions Linux sur Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Découvrez comment gérer plusieurs distributions Linux différentes sur votre machine Windows.
