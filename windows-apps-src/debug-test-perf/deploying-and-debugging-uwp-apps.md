---
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: Cet article vous guide tout au long des étapes nécessaires pour cibler différents objectifs de déploiement et de débogage.
title: Déploiement et débogage des applications de plateforme Windows universelle (UWP)
---

# Déploiement et débogage des applications de plateforme Windows universelle (UWP)

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cet article vous guide tout au long des étapes nécessaires pour cibler différents objectifs de déploiement et de débogage.

Microsoft Visual Studio vous permet de déployer et de déboguer des applications UWP sur de nombreux appareils Windows 10. Visual Studio gère le processus de génération et d’inscription de l’application sur l’appareil cible.

## Sélection d’une cible de déploiement

Pour sélectionner une cible, accédez à la liste déroulante des cibles de débogage située en regard du bouton **Démarrer le débogage**, puis sélectionnez la cible vers laquelle vous voulez déployer votre application. Une fois la cible sélectionnée, choisissez **Démarrer le débogage (F5)** pour procéder au déploiement et au débogage sur cette cible, ou appuyez sur **Ctrl+F5** pour déployer simplement vers cette cible.

![](images/debug-device-target-list.png)

-   **Ordinateur local** : l’application est déployée vers l’ordinateur de développement actif. Cette option est disponible uniquement si la **Version minimale de la plateforme cible** de votre application est inférieure ou égale au système d’exploitation sur votre ordinateur de développement.
-   **Simulateur** : l’application est déployée vers un environnement simulé de l’ordinateur de développement actif. Cette option est disponible uniquement si la **Version minimale de la plateforme cible** de votre application est inférieure ou égale au système d’exploitation sur votre ordinateur de développement.
-   **Appareil** : l’application est déployée vers un appareil connecté USB. L’appareil doit être déverrouillé par le développeur et son écran doit être déverrouillé.
-   Une cible **Émulateur** permet de démarrer et de déployer l’application sur un émulateur avec la configuration spécifiée dans le nom. Les émulateurs sont disponibles uniquement sur les ordinateurs Hyper-V exécutant Windows 8.1 ou supérieur.
-   L’option **Ordinateur distant** vous permet de spécifier une cible distante pour déployer l’application. Pour plus d’informations sur le déploiement vers un ordinateur distant, voir la section [Spécification d’un appareil distant](#specifying-a-remote-device).

## Spécification d’un appareil distant

### C# et Microsoft Visual Basic

Pour spécifier un ordinateur distant pour des applications en C# ou Microsoft Visual Basic, sélectionnez **Ordinateur distant** dans la liste déroulante des cibles de débogage. La boîte de dialogue **Connexions à distance** s’affiche et vous permet d’indiquer une adresse IP ou de sélectionner un appareil détecté. Par défaut, le mode d’authentification **universelle** est sélectionné. Pour déterminer le mode d’authentification à utiliser, voir [Modes d’authentification](#authentication-modes).

![](images/debug-remote-connections.png)

Pour revenir à cette boîte de dialogue, vous pouvez ouvrir les propriétés du projet et accéder à l’onglet **Déboguer**. Sélectionnez **Rechercher...** en regard de l’option **Ordinateur distant** :

![](images/debug-remote-machine-config.png)

Pour déployer une application vers un PC distant, vous devez également télécharger et installer les outils de contrôle à distance Visual Studio sur le PC cible. Voir [Instructions pour un PC distant](#remote-pc-instructions) pour obtenir des instructions complètes.

### C++ et JavaScript

Pour spécifier une cible d’ordinateur distant pour une application UWP en C++ ou JavaScript, accédez aux propriétés de projet en cliquant avec le bouton droit sur le projet dans l’**Explorateur de solutions**, puis en cliquant sur **Propriétés**. Accédez aux paramètres de **Débogage** et remplacez **Débogueur à lancer** par **Ordinateur distant**. Renseignez alors la zone **Nom de l’ordinateur** (ou cliquez sur **Rechercher…** pour en trouver un) et définissez la propriété **Type d’authentification**.

![](images/debug-property-pages.png)
Une fois que l’ordinateur est spécifié, vous pouvez sélectionner **Ordinateur distant** dans la liste déroulante des cibles de débogage pour revenir à cet ordinateur spécifié. Un seul ordinateur distant peut être sélectionné à la fois.

### Instructions pour un PC distant

Pour effectuer un déploiement sur un PC distant, les outils de contrôle à distance Visual Studio doivent être installés sur ce PC cible. Le PC distant doit également exécuter une version de Windows supérieure ou égale à la propriété **Version minimale de la plateforme cible** de vos applications. Une fois que vous avez installé les outils de contrôle à distance, vous devez lancer le débogueur distant sur le PC cible. Pour ce faire, recherchez **Débogueur distant** dans le menu **Démarrer** pour le lancer, et si vous y êtes invité, autorisez le débogueur à configurer vos paramètres de pare-feu. Par défaut, le débogueur est lancé avec l’authentification Windows. Cela requiert les informations d’identification de l’utilisateur si l’utilisateur connecté n’est pas le même sur les deux PC. Pour le définir sur **Pas d’authentification**, accédez à **Outils** &gt; **Options** dans le **Débogueur distant** et définissez-le sur **Pas d’authentification**. Une fois le débogueur distant configuré, vous pouvez procéder au déploiement à partir de votre ordinateur de développement.

Pour plus d’informations, voir la page de téléchargement [Outils de contrôle à distance de Microsoft Visual Studio]( http://go.microsoft.com/fwlink/?LinkId=717039).

## Modes d’authentification

Il existe trois modes d’authentification de déploiement sur un ordinateur distant :

- **Universel (protocole non chiffré)** : utilisez ce mode d’authentification chaque fois que vous effectuez un déploiement sur un appareil distant qui n’est pas un PC Windows (ordinateur de bureau ou portable). Actuellement, il s’agit uniquement des appareils IoT. Le mode Universel (protocole non chiffré) doit uniquement être utilisé sur les réseaux approuvés. La connexion de débogage est vulnérable aux utilisateurs malveillants qui peuvent intercepter les données transmises entre la machine de développement et la machine distante, et les modifier.
- **Windows** : ce mode d’authentification est destiné uniquement à être utilisé pour le déploiement sur PC distant (ordinateur de bureau ou portable). Utilisez ce mode d’authentification lorsque vous avez accès aux informations d’identification de l’utilisateur connecté de l’ordinateur cible. Il s’agit du canal le plus sécurisé pour le déploiement à distance.
- **Aucun** : ce mode d’authentification est destiné uniquement à être utilisé pour le déploiement sur PC distant (ordinateur de bureau ou portable). Utilisez ce mode d’authentification lorsque vous disposez d’un ordinateur de test configuré dans un environnement dans lequel un compte de test est connecté, et que vous ne pouvez pas entrer les informations d’identification. Assurez-vous que les paramètres du débogueur distant sont définis pour ne pas accepter d’authentification.

## Options de débogage

Sur Windows 10, les performances de démarrage des applications UWP sont améliorées en les lançant de manière proactive, puis en les suspendant dans une technique dite de [prélancement](https://msdn.microsoft.com/library/windows/apps/Mt593297). Nombre d’applications fonctionnent dans ce mode sans rien de spécial, mais certaines d’entre elles devront peut-être ajuster leur comportement. Pour permettre le débogage des problèmes dans ces chemins de code, vous pouvez commencer à déboguer l’application à partir de Visual Studio en mode de prélancement. Le débogage est pris en charge à la fois à partir d’un projet Visual Studio (**Déboguer** -&gt; **Autres cibles de débogage** -> **Déboguer le prélancement d’application Windows universelle**) et pour les applications déjà installées sur l’ordinateur (**Déboguer** -&gt; **Autres cibles de débogage** -&gt; **Déboguer le package d’application installé**, puis cochez la case **Activer l’application par prélancement**). Pour plus d’informations, voir la page web [Debug UWP Prelaunch]( http://go.microsoft.com/fwlink/?LinkId=717245) (en anglais).

Vous pouvez définir les options de déploiement suivantes dans la page de propriétés de **débogage** du projet de démarrage.

**Autoriser le bouclage réseau**

Pour des raisons de sécurité, une application UWP qui est installée de manière standard n’est pas autorisée à effectuer des appels réseau vers l’appareil sur lequel elle est installée. Par défaut, le déploiement de Visual Studio crée une exemption à cette règle pour l’application déployée. Cette exemption vous permet de tester les procédures de communication sur un seul et même ordinateur. Avant de soumettre votre application au Windows Store, vous devez la tester sans l’exemption.

Pour supprimer l’exemption de bouclage réseau de l’application :

-   Dans la page de propriétés de **débogage** en C# et Visual Basic, décochez la case **Autoriser le bouclage réseau**.
-   Dans la page de propriétés de **débogage** en JavaScript et C++, définissez la valeur **Autoriser le bouclage réseau** sur **Non**.

**Ne pas lancer, mais déboguer mon code au démarrage (C# et Visual Basic)/Lancer l’application (JavaScript et C++)**

Pour configurer le déploiement afin de démarrer automatiquement une session de débogage au lancement de l’application :

-   Dans la page de propriétés de **débogage** en C# et Visual Basic, cochez la case **Ne pas lancer, mais déboguer mon code au démarrage**.
-   Dans la page de propriétés de **débogage** en JavaScript et C++, définissez la valeur **Lancer l’application** sur **Oui**.




<!--HONumber=Mar16_HO1-->


