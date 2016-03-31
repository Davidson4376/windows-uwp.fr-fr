---
Description: Une bonne connaissance des appareils qui prennent en charge les applications UWP peut vous aider à offrir la meilleure expérience utilisateur pour chaque facteur de forme.
title: Notions fondamentales sur les appareils pour les applications de plateforme Windows universelle (UWP)
ms.assetid: 7665044E-F007-495D-8D56-CE7C2361CDC4
label: Notions fondamentales sur les appareils
template: detail.hbs
---

#  Notions fondamentales sur les appareils pour les applications de plateforme Windows universelle (UWP)


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


![appareils fonctionnant sous Windows](images/device-primer/1894834-hig-device-primer-01-500.png)

Une bonne connaissance des appareils qui prennent en charge les applications UWP peut vous aider à offrir la meilleure expérience utilisateur pour chaque facteur de forme. Lors de la conception d’une application pour un appareil particulier, vous devez surtout prendre en compte l’affichage de l’application dans cet appareil, ainsi que le moment, l’endroit ou la façon dont l’application sera utilisée sur cet appareil et le type d’interaction de l’utilisateur avec ce dernier.

## <span id="Phones_and_phablets"> </span> <span id="phones_and_phablets"> </span> <span id="PHONES_AND_PHABLETS"> </span>Téléphones et phablettes


Appareils informatiques parmi les plus utilisés actuellement, les téléphones peuvent proposer de nombreuses fonctions, malgré un espace à l’écran réduit et des commandes d’entrée limitées. Les téléphones sont disponibles dans différentes tailles. Les téléphones de grande taille sont appelés des phablettes. L’expérience liée à l’utilisation de l’application sur une phablette est similaire à celle que propose un téléphone, mais l’espace disponible sur l’écran d’une phablette permet d’apporter des modifications significatives au comportement de consommation du contenu de l’utilisateur.

Avec Continuum pour téléphones, une nouvelle expérience pour les appareils mobiles Windows 10 compatibles, les utilisateurs peuvent connecter leurs téléphones à un moniteur et même à une souris et un clavier pour les utiliser comme un ordinateur portable. (Pour plus d’informations, voir l’article [Continuum pour téléphones](http://go.microsoft.com/fwlink/p/?LinkID=699431).)

Tailles d’écran
-   De 4 à 5 pouces pour les téléphones
-   De 5,5 à 7 pouces pour les phablettes

![windows Phone](images/device-primer/1894834-hig-device-primer-03.png)

Utilisation type
-   Le mode portrait est le mode le plus utilisé, avant tout parce qu’il permet de tenir facilement son téléphone dans une main tout en interagissant avec l’ensemble des fonctionnalités. Toutefois, certaines expériences sont plus pratiques en mode paysage, comme afficher des photos et des vidéos, lire un livre ou rédiger un texte.
-   La plupart du temps, les téléphones sont utilisés par une seule personne : leur propriétaire.
-   Toujours à portée de main, généralement au fond d’une poche ou d’un sac.
-   On utilise généralement son téléphone sur de courtes durées.
-   Les utilisateurs effectuent souvent plusieurs tâches lorsqu’ils utilisent leur téléphone.
-   L’utilisateur saisit son texte par salves.

Considérations relatives à l’interface utilisateur
-   La taille réduite des écrans de téléphone permet l’affichage d’une seule image à la fois, en mode portrait ou paysage. Tous les modèles de navigation hiérarchique d’un téléphone utilisent le modèle de type « extraction », l’utilisateur pouvant se déplacer d’une couche d’interface utilisateur à une seule image à l’autre.

-   Comme les téléphones, les phablettes peuvent uniquement afficher une image à la fois en orientation portrait. Cependant, l’espace disponible étant plus grand sur l’écran d’une phablette, les utilisateurs peuvent passer de l’orientation portrait à l’orientation paysage, en conservant cette dernière. Ainsi, deux images de l’application peuvent être visibles en même temps.

-   Dans les deux orientations, assurez-vous que l’espace disponible à l’écran est suffisant pour la barre d’application lorsque le clavier visuel est affiché.

Entrées
-   Interaction tactile
-   Voix

Fonctionnalités de l’appareil type
-   Microphone
-   Appareil photo
-   Capteurs de mouvement
-   Capteurs d’emplacement

 

## <span id="Tablets"> </span> <span id="tablets"> </span> <span id="TABLETS"> </span>Tablettes


Les tablettes PC ultra-portables sont dotées d’un écran tactile, d’un appareil photo, de microphones et d’accéléromètres. En général, leur écran mesure entre 7 et 13,3 pouces.

Tailles d’écran
-   De 7 à 13,3 pouces

![appareil de type tablette](images/device-primer/1894834-hig-device-primer-05.png)

Utilisation type
-   Le propriétaire d’une tablette l’utilise à 80 % environ, les 20 % restants étant réservés au partage.
-   On l’utilise souvent à la maison, en tant que complément, en regardant la télévision.
-   Les tablettes sont utilisées pendant de plus longues périodes que les téléphones et les phablettes.
-   L’utilisateur saisit son texte par salves.

Considérations relatives à l’interface utilisateur
-   Dans les orientations paysage et portrait, les tablettes autorisent l’affichage de deux images à la fois.
-   La fonction système de retour se trouve sur la barre de navigation.

Entrées
-   Interaction tactile
-   Stylet
-   Clavier externe (parfois)
-   Souris (parfois)
-   Voix (parfois)

Fonctionnalités de l’appareil type
-   Appareil photo
-   Microphone
-   Capteurs de mouvement
-   Capteurs d’emplacement

 

## <span id="PCs_and_laptops"> </span> <span id="pcs_and_laptops"> </span> <span id="PCS_AND_LAPTOPS"> </span>PC et ordinateurs portables


Les PC et portables Windows comprennent une large gamme d’appareils et de tailles d’écran. En règle générale, les PC et les portables peuvent afficher davantage d’informations que les téléphones ou les tablettes.

Tailles d’écran
-   13 pouces et plus

![pc](images/device-primer/1894834-hig-device-primer-06-349.png)

Utilisation type
-   Les applications installées sur les ordinateurs de bureau et les portables présentent une utilisation partagée mais sont utilisées par un seul utilisateur à la fois, généralement sur des périodes plus longues.

Considérations relatives à l’interface utilisateur
-   Les applications peuvent inclure un affichage fenêtré, dont la taille est déterminée par l’utilisateur. Selon la taille des fenêtres, une à trois images peuvent s’afficher. Sur les moniteurs de grande taille, l’application peut inclure plus de trois images.

-   Lorsqu’il utilise une application sur un ordinateur de bureau ou portable, l’utilisateur contrôle les fichiers de l’application. En tant que concepteur d’applications, veillez à fournir les mécanismes permettant de gérer le contenu de votre application. Vous pouvez envisager d’inclure des fonctions et commandes comme « Enregistrer sous », « Fichiers récents », etc.

-   L’arrière-plan système est facultatif. Lorsqu’un développeur d’application choisit de l’afficher, celui-ci apparaît dans la barre de titre de l’application.

Entrées
-   Souris
-   Clavier
-   Écran tactile sur ordinateur portable et ordinateur tout-en-un.
-   Des boîtiers de commande, tels que des manettes Xbox, sont parfois utilisés.

Fonctionnalités de l’appareil type
-   Appareil photo
-   Microphone

 

## <span id="Surface_Hub_devices"> </span> <span id="surface_hub_devices"> </span> <span id="SURFACE_HUB_DEVICES"> </span>Appareils Surface Hub


L’appareil de collaboration d’équipe à écran large Microsoft Surface Hub est conçu pour être utilisé simultanément par plusieurs utilisateurs.

Tailles d’écran
-   55 et 84 pouces

![surface hub](images/device-primer/1894834-hig-device-primer-07-349.png)

Utilisation type
-   Les applications installées sur Surface Hub présentent une utilisation partagée sur de courtes périodes, telles que des réunions.

-   Les appareils Surface Hub sont généralement fixes et sont rarement déplacés.

Considérations relatives à l’interface utilisateur
-   Les applications installées sur Surface Hub peuvent présenter quatre états : remplissage (affichage fixe occupant tout l’espace disponible à l’écran), complet (affichage plein écran standard), ancré (affichage variable occupant le côté gauche ou droit de l’écran) et arrière-plan (masqué lorsque l’application est en cours d’exécution, disponible dans le sélecteur de tâches).
-   En mode ancré ou en mode remplissage, le système affiche la barre latérale Skype et réduit l’application horizontalement.
-   L’arrière-plan système est facultatif. Lorsqu’un développeur d’application choisit de l’afficher, celui-ci apparaît dans la barre de titre de l’application.

Entrées
-   Commandes tactiles
-   Stylet
-   Voix
-   Clavier
-   Pavé tactile (à distance)

Fonctionnalités classiques d’appareil
-   Appareil photo
-   Microphone

 

## <span id="Windows_IoT_devices"> </span> <span id="windows_iot_devices"> </span> <span id="WINDOWS_IOT_DEVICES"> </span>Appareils IoT Windows


Les appareils IoT Windows sont une nouvelle catégorie d’appareils centrés sur l’ajout de petits composants électroniques, de capteurs et de la connectivité aux objets physiques. Ces appareils sont généralement connectés à un réseau ou à Internet afin d’envoyer les données captées et d’interagir avec elles, le cas échéant. Ces appareils peuvent être dépourvus d’écran (on dit alors de ces appareils qu’ils sont « sans périphérique de contrôle ») ou connectés à un écran de petite taille (on parle alors d’appareils « avec périphérique de contrôle ») avec une taille d’écran généralement de 3,5 pouces ou plus petite.

Tailles d’écran
-   3,5 pouces ou inférieure
-   Certains appareils ne possèdent pas d’écran

![appareil iot](images/device-primer/1894834-hig-device-primer-02.png)

Utilisation type
-   Ces appareils sont généralement connectés à un réseau ou à Internet afin d’envoyer les données captées et d’interagir avec elles, le cas échéant.
-   Ils ne peuvent exécuter qu’une seule application à la fois, à la différence des téléphones ou d’autres appareils de plus grande taille.
-   Ces appareils ne sont pas destinés à être utilisés en permanence : ils sont présents quand vous en avez besoin et s’éclipsent lorsque vous n’en avez pas l’utilité.
-   Les applications ne disposent nécessairement pas d’une affordance d’arrière-plan : cet élément est déterminé par chaque développeur.

Considérations relatives à l’interface utilisateur
-   Les appareils dits « sans périphérique de contrôle » ne possèdent pas d’écran.
-   L’affichage des appareils dits « avec périphérique de contrôle » est minimal, et ne comprend que le nécessaire compte tenu de l’espace restreint et des fonctionnalités limitées de l’écran.
-   L’orientation est la plupart du temps verrouillée, afin que votre application n’utilise qu’un seul sens d’affichage.

Entrées
-   Variable, dépend de l’appareil

Fonctionnalités de l’appareil type
-   Variable, dépend de l’appareil

 

\[Cet article contient des informations propres aux applications UWP et à Windows 10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

 

 




<!--HONumber=Mar16_HO1-->
