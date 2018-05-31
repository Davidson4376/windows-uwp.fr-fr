---
author: QuinnRadich
title: Style d’écriture
description: Utiliser une voix et un ton adaptés est essentiel pour que le texte de votre application semble faire naturellement partie de sa conception.
keywords: UWP, Windows10, texte, écriture, voix, ton, conception, interface utilisateur, expérience utilisateur
ms.author: quradic
ms.date: 1/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d38f22f896e31a925f07c5d6dffd357c211b3336
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
ms.locfileid: "1573245"
---
# <a name="writing-style"></a>Style d’écriture

La façon dont vous formulez un message d’erreur, la façon dont vous rédigez la documentation d’aide et même le texte que vous choisissez pour un bouton ont un impact important sur la facilité d’utilisation de votre application. 

Le style d’écriture peut faire une grande différence entre une expérience utilisateur désastreuse...

![Windows7 Blue Screen of Death](images/writing/bluescreen_old.png)

...Et une meilleure expérience.

![Windows10 Blue Screen of Death](images/writing/bluescreen_new.png)


## <a name="writing-with-a-natural-voice-and-tone"></a>Écrire avec un style et un ton naturels

La recherche montre que les utilisateurs réagissent mieux à un style d’écriture convivial, efficace et concis. 

## <a name="be-friendly"></a>Être convivial

Par-dessus tout, vous ne voulez pas effrayer l’utilisateur. Soyez informel, neutre, et n’utilisez pas des termes qu’il ne comprendra pas. Même lorsque la situation va mal, ne rejetez pas la faute sur l’utilisateur. Votre application doit assumer la responsabilité et donner des conseils bienveillants qui privilégient les actions de l’utilisateur.

![Avant: une erreur s’est produite et votre image n’a pas pu être téléchargée. Si les tentatives suivantes échouent également, l’application devra peut-être être réinitialisée. Après: nous n’avons pas pu télécharger votre image, car une erreur s’est produite. Veuillez réessayer, et si le problème persiste, essayez de relancer l’application.](images/writing/friendly_example.png)

## <a name="be-helpful"></a>Être efficace

Veillez toujours à expliquer la situation et à fournir les informations pertinentes dont l’utilisateur a besoin, sans le surcharger d’informations inutiles.

![Avant: impossible de se connecter au réseau. Après: votre appareil est connecté à un réseau, mais le réseau n’est pas connecté à Internet. Essayez de relancer votre routeur ou modem. Si vous utilisez un point d’accès public, vérifiez que vous êtes connecté.](images/writing/helpful_example.png)

## <a name="be-clear-and-concise"></a>Être clair et concis

La plupart du temps, le texte n’est pas l’élément principal d’une application. Il permet de guider les utilisateurs, de leur indiquer ce qui se passe et ce qu’ils doivent faire ensuite. Ne perdez pas cela de vue lorsque vous écrivez du texte dans votre application. Utilisez un langage qui sera familier à votre public. Cela signifie généralement de choisir des mots ordinaires et quotidiens, mais les applications ayant des usages spécifiques auront leurs propres normes. Vos utilisateurs ne doivent jamais avoir à se demander ce que votre texte signifie. Optez donc pour la simplicité si vous n’êtes pas sûr du ton à utiliser.

![Avant: un problème s’est produit et nous n’avons pas pu enregistrer votre fichier. L’emplacement n’est peut-être pas accessible à cet appareil, ou il se peut que l’espace ne soit pas suffisant. Après: impossible d’enregistrer votre fichier à cet emplacement. Réessayez avec un autre emplacement.](images/writing/concise_example.png)

## <a name="writing-tips"></a>Conseils d’écriture

Il existe plusieurs façons d’implémenter ces principes généraux dans l’espace limité de votre application. Voici quelques stratégies que nous trouvons utiles pour mettre ces principes en action:

### <a name="lead-with-whats-important"></a>Commencez par ce qui est important

Les utilisateurs doivent pouvoir lire et comprendre votre texte d'un seul coup d'œil. N’étouffez pas vos mots sous des introductions inutiles. Donnez un maximum de visibilité à vos points clés et présentez toujours le fond d’une idée avant d’ajouter des précisions.

**Bon:** sélectionnez des «filtres» pour ajouter des effets à votre image.

**Mauvais:** si vous souhaitez ajouter des effets visuels ou des modifications à votre image, sélectionnez «filtres».

### <a name="emphasize-action"></a>Mettre les actions en évidence

Les applications sont définies par des actions. Les utilisateurs agissent en utilisant l’application et celle-ci agit en répondant à leurs actions. Assurez-vous que votre texte utilise la *voix active* tout au long de l'application. Les utilisateurs et les fonctions doivent être décrits comme faisant quelque chose, non comme si l'on faisait les choses pour eux.

**Bon:** Relancez l’application pour voir vos modifications.

**Mauvais:** Les modifications seront appliquées lors du redémarrage de l’application.

### <a name="short-and-sweet"></a>Bref et concis

Les utilisateurs survolent le texte et en ignorent souvent des blocs entiers. Ne sacrifiez ni les informations indispensables ni la présentation, mais n’utilisez pas plus de mots que nécessaire. Cela implique parfois d'utiliser de nombreuses phrases plus courtes ou de simples fragments. Dans d’autres cas, il faudra choisir avec beaucoup de soin les mots et la structure de phrases plus longues.

**Bon:** Désolés, nous n’avons pas pu charger l’image. Si cela se produit à nouveau, essayez de relancer l’application. Mais ne vous inquiétez pas: vous retrouverez votre image à votre retour.

**Mauvais:** Une erreur s’est produite et nous n’avons pas pu charger l’image. Veuillez réessayer et, si vous rencontrez à nouveau ce problème, vous devrez peut-être relancer l’application. Mais ne vous inquiétez pas: nous avons enregistré votre travail en local et il vous attendra à votre retour.

## <a name="style-conventions"></a>Conventions de style

Si vous ne vous considérez pas comme un rédacteur, il peut être intimidant d’essayer de mettre en œuvre ces principes et recommandations. Mais ne vous inquiétez pas, l’utilisation d’un langage simple et direct est un excellent moyen de fournir une expérience utilisateur efficace. Et si vous ne savez toujours pas comment structurer vos mots, voici quelques conseils utiles.

### <a name="addressing-the-user"></a>S’adresser à l’utilisateur

Parlez directement à l’utilisateur.

* Utilisez «vous» lorsque vous vous adressez à l’utilisateur.

* Utilisez «nous» pour faire référence à votre propre point de vue. Cela est plus chaleureux et donne à l’utilisateur l’impression de faire partie de l’expérience.

* N’utilisez pas «je» ou «moi» pour faire référence au point de vue de l’application, même si vous êtes le seul à la créer.

> Impossible d’enregistrer votre fichier à cet emplacement.

### <a name="abbreviations"></a>Abréviations

Les abréviations peuvent être utiles lorsque vous devez faire référence à des produits, des emplacements ou des concepts techniques plusieurs fois dans toute votre application. Ils peuvent économiser de l’espace et sont plus naturelles, tant que l’utilisateur les comprend.

* Ne partez pas du principe que les utilisateurs connaissent déjà les abréviations, même si vous pensez qu’il s’agit d’abréviations courantes.

* Donnez toujours la définition d’une nouvelle abréviation la première fois que l’utilisateur la voit.

* N’utilisez pas des abréviations qui se ressemblent trop.

> Le guide de conception pour la plateforme Windows universelle (UWP) est une ressource destinée à vous aider à concevoir et créer des applications belles et élégantes. Avec les fonctionnalités de conception incluses dans chaque application UWP, vous pouvez créer des interfaces utilisateur qui s’adaptent à toute une gamme d’appareils.

### <a name="contractions"></a>Contractions

Les lecteurs ont l'habitude des contractions et s'attendent à en trouver. En les évitant, vous risquez de donner à votre application un aspect trop austère ou même qui manque de naturel.

* Utilisez des contractions lorsqu’elles s’adaptent naturellement au texte.

* N’utilisez pas des contractions qui ne sont pas naturelles juste pour économiser de l’espace, ou s’ils rendent vos propos moins accessibles.

> Lorsque vous êtes satisfait de votre image, appuyez sur «Enregistrer» pour l’ajouter à votre bibliothèque. Vous pourrez ensuite la partager avec vos amis.

### <a name="periods"></a>Points

Terminer un texte par un point implique que ce texte est une phrase complète. Utilisez un point pour les blocs de texte plus grands, et évitez d’en mettre lorsque le texte est plus court qu’une phrase complète.

* Utilisez un point à la fin de phrases complètes dans les info-bulles, les messages d’erreur et les boîtes de dialogue.

* N’utilisez pas de point à la fin du texte des boutons, des cases d’option, des étiquettes ou des cases à cocher.

> **Vous n’êtes pas connecté**
> * Vérifiez que vos câbles réseau sont branchés.
> * Vérifiez que vous n’êtes pas en mode avion.
> * Vérifiez si votre commutateur sans fil est activé.
> * Relancez votre routeur.

### <a name="capitalization"></a>Mise en majuscules

Les majuscules sont importantes, mais il est facile d’en abuser.

* Mettez en majuscules les noms propres.

* Mettez en majuscule le début de chaque chaîne de texte dans votre application: le début de chaque phrase, étiquette et titre.

> **Quelle partie vous pose des problèmes?**
> * J’ai oublié mon mot de passe
> * Mon mot de passe n’est pas accepté
> * Une autre personne utilise peut-être mon compte

## <a name="error-messages"></a>Messages d’erreur

Lorsqu’un problème se produit dans votre application, les utilisateurs font plus attention. Comme ils peuvent se sentir confus ou frustrés lorsqu’ils rencontrent un message d’erreur, un style et un ton adaptés peuvent avoir un impact particulièrement important.

Avant tout, il est important que votre message d’erreur ne blâme pas l’utilisateur. Mais il est également important de ne pas le surcharger d'informations qu’il ne comprend pas. La plupart du temps, un utilisateur qui rencontre une erreur souhaite juste revenir à ce qu’il était en train de faire aussi rapidement et facilement que possible. Par conséquent, un message d’erreur que vous rédigez doit:

* **Être convivial** en assumant la responsabilité de ce qui s’est passé et en évitant les termes peu familiers et le jargon technique.

* **Être efficace** en expliquant de votre mieux à l’utilisateur ce qui s’est passé, en lui indiquant ce qui va se passer ensuite et en fournissant une solution réaliste qu’il peut effectuer.

* **Être clair et concis** en éliminant les informations superflues.

![Exemple de message d’erreur correct](images/writing/connection_error.png)

## <a name="buttons"></a>Boutons

Le texte des boutons doit être suffisamment concis pour que les utilisateurs puissent le lire d’un seul coup d’œil et suffisamment clair pour que la fonction du bouton soit immédiatement évidente. Le texte d’un bouton ne doit pas dépasser quelques mots, et nombre d’entre eux devrait être raccourcis.

Lorsque vous écrivez le texte des boutons, gardez à l’esprit que chaque bouton représente une action. Veillez à utiliser la *voix active* dans le texte du bouton et à utiliser des mots qui représentent des actions plutôt que des réactions.

![Exemple de bouton correct](images/writing/install_button.png)

## <a name="dialogs"></a>Boîtes de dialogue

La plupart des conseils fournis pour écrire des messages d’erreur s’applique également à la création du texte des boîtes de dialogue de votre application. Les boîtes de dialogue sont attendues par l’utilisateur, mais elles interrompent le flux normal de l’application; elles doivent donc être utiles et concises afin de l’utilisateur puisse revenir à ce qu’il était en train de faire.

Mais le plus important est «la question et la réponse» entre le titre d’une boîte de dialogue et ses boutons. Vérifiez que vos boutons sont des réponses claires à la question posée par le titre, et que leur format est cohérent dans l’ensemble de votre application.

![Exemple de boîte de dialogue correcte](images/writing/password_dialog.png)

## <a name="spoken-experiences"></a>Expériences vocales

Les mêmes recommandations et principes généraux s’appliquent à l’écriture de texte pour les expériences vocales, comme Cortana. Dans ces fonctionnalités, les principes d’écriture efficace sont encore plus importants, car vous ne pouvez pas fournir aux utilisateurs d’autres éléments de conception visuelle pour compléter les paroles.

* **Être convivial** en vous adressant à l’utilisateur sur un ton normal. Plus que dans tout autre domaine, il est vital qu’une expérience vocale soit chaleureuse et conviviale pour que les utilisateurs se sentent à l’aise pour parler.

* **Être efficace** en faisant d’autres suggestions lorsque l’utilisateur demande l’impossible. Comme dans un message d’erreur, si un problème s’est produit et que votre application ne peut pas traiter la demande, elle doit fournir à l’utilisateur une alternative réaliste à essayer.

* **Être clair et concis** en utilisant un langage simple. Les expériences vocales ne sont pas adaptées aux phrases longues ou aux mots compliqués.

## <a name="accessibility-and-localization"></a>Accessibilité et localisation

Votre application peut atteindre un public plus large si elle est écrite en tenant compte de l’accessibilité et de la localisation. Cela n’est possible qu’au moyen de texte, même si un langage simple et convivial est un très bon début. Pour plus d’informations, consultez notre [vue d’ensemble de l’accessibilité](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) et nos [recommandations en matière de localisation](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Être convivial et efficace** en tenant compte de différentes expériences. Évitez les expressions qui peuvent ne pas avoir de sens pour un public international et n’utilisez pas des mots qui créent des hypothèses sur ce que l’utilisateur peut ou ne peut pas faire.

* **Être clair et concis** en évitant les mots inhabituels et spécialisés lorsqu’ils sont inutiles. Plus votre texte est simple, plus il est facile à localiser.


## <a name="techniques-for-non-writers"></a>Techniques pour les personnes qui ne sont pas des rédacteurs

Vous n’avez pas besoin d’être un rédacteur formé ou expérimenté pour fournir une expérience efficace à vos utilisateurs. Choisissez des mots avec lesquels vous êtes à l'aise. L’utilisateur les appréciera également. Mais parfois, cela n’est pas aussi facile qu’il y paraît. Si vous êtes bloqué, les techniques suivantes peuvent vous aider. 

* Imaginez que vous parlez à un ami de votre application. Comment lui expliquerez-vous l’application? Comment souhaitez vous parler de ses fonctionnalités ou lui fournir des instructions? Mieux encore, expliquez l’application à une personne qui ne l’a pas encore utilisée. 

* Imaginez comment vous décririez une application complètement différente. Par exemple, si vous créez un jeu, pensez à ce que vous diriez ou écririez pour décrire une application financière ou journalistique. Le contraste dans le langage et la structure que vous utilisez peut vous aider à trouver les bons mots pour illustrer vos propos.

* Examinez des applications similaires pour vous inspirer. 

Trouver les mots adaptés est un problème pour beaucoup de gens. Ne vous inquiétez donc pas s’il est difficile de parvenir à ce qui vous paraîtrait naturel.