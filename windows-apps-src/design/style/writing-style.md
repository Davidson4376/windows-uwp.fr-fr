---
author: QuinnRadich
title: Style d’écriture
description: Utiliser une voix et un ton adaptés est essentiel pour que le texte de votre application semble faire naturellement partie de sa conception.
keywords: UWP, Windows10, texte, écriture, voix, ton, conception, interface utilisateur, expérience utilisateur
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8022b3bb5ca312be259c554f46dc9f432ea3caeb
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5750057"
---
# <a name="writing-style"></a>Style d’écriture

![image de l’en-tête](images/header-writing-style.gif)

La façon dont vous formulez un message d’erreur, dont vous rédigez la documentation d’aide et même le texte que vous choisissez pour un bouton ont un impact important sur la facilité d’utilisation de votre application. Le style d’écriture peut faire une grande différence entre une expérience utilisateur désastreuse et une meilleure.

## <a name="voice-and-tone-principles"></a>Principes régissant la voix et le ton

Les études montrent que les gens réagissent mieux à un style d’écriture convivial, efficace et concis. Dans le cadre de cette étude, Microsoft a développé trois principes en matière de voix et de ton que nous appliquons à tout notre contenu et qui font partie intégrante de la conception Fluent.

### <a name="be-warm-and-relaxed"></a>Soyez chaleureux et détendu

Par-dessus tout, vous ne voulez pas effrayer l’utilisateur. Soyez informel, neutre et n’utilisez pas des termes qu’il ne comprendra pas. Même lorsque la situation va mal, ne rejetez pas la faute sur l’utilisateur. Votre application doit se montrer responsable et donner des conseils bienveillants qui privilégient les actions de l’utilisateur.

### <a name="be-ready-to-lend-a-hand"></a>Soyez prêt à aider

Faites toujours preuve d'empathie dans ce que vous écrivez. Veillez à expliquer la situation et à fournir les informations pertinentes dont l’utilisateur a besoin, sans le surcharger d’informations inutiles. Et, si possible, offrez toujours une solution en cas de problème.

### <a name="be-crisp-and-clear"></a>Soyez clair et concis

La plupart du temps, le texte n’est pas l’élément principal d’une application. Il permet de guider les utilisateurs, de leur indiquer ce qui se passe et ce qu’ils doivent faire ensuite. Ne perdez pas cela de vue lorsque vous écrivez le texte de votre application et ne supposez pas que les utilisateurs liront chaque mot. Utilisez un langage que connaît votre public, assurez-vous qu’il est facile à comprendre d'un coup d'œil.

## <a name="lead-with-whats-important"></a>Commencez par ce qui est important

Les utilisateurs doivent pouvoir lire et comprendre votre texte d'un seul coup d'œil. N’étouffez pas vos mots sous des introductions inutiles. Donnez un maximum de visibilité à vos points clés et présentez toujours le fond d’une idée avant d’ajouter des précisions.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Select **filters** to add effects to your image.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        If you want to add visual effects or alterations to your image, select **filters.**
    :::column-end:::
:::row-end:::

## <a name="emphasize-action"></a>Mettez les actions en évidence

Les applications sont définies par des actions. Les utilisateurs agissent en utilisant l’application et celle-ci agit en répondant à leurs actions. Assurez-vous que votre texte utilise la *voix active* tout au long de l'application. Les utilisateurs et les fonctions doivent être décrits comme faisant quelque chose, non comme si l'on faisait les choses pour eux.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Restart the app to see your changes.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        The changes will be applied when the app is restarted.
    :::column-end:::
:::row-end:::

## <a name="short-and-sweet"></a>Bref et concis

Les utilisateurs survolent le texte et en ignorent souvent des blocs entiers. Ne sacrifiez ni les informations indispensables ni la présentation, mais n’utilisez pas plus de mots que nécessaire. Cela implique parfois d'utiliser de nombreuses phrases plus courtes ou de simples fragments. Dans d’autres cas, il faudra choisir avec beaucoup de soin les mots et la structure de phrases plus longues.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't upload the picture. If this happens again, try restarting the app. But don't worry — your picture will be waiting when you come back.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        An error occured, and we weren't able to upload the picture. Please try again, and if you encounter this problem again, you may need to restart the app. But don't worry — we've saved your work locally, and it'll be waiting for you when you come back.
    :::column-end:::
:::row-end:::

## <a name="style-conventions"></a>Conventions de style

Si vous ne vous considérez pas comme un écrivain, il peut être intimidant d’essayer de mettre en œuvre ces principes et recommandations. Mais ne vous inquiétez pas, l’utilisation d’un langage simple et direct est un excellent moyen de fournir une expérience utilisateur efficace. Et si vous ne savez toujours pas comment structurer vos mots, voici quelques conseils utiles. Et si vous souhaitez plus d’informations, consultez le [Guide de Style Microsoft](https://docs.microsoft.com/style-guide/welcome/).

### <a name="addressing-the-user"></a>S’adresser à l’utilisateur

Parlez directement à l’utilisateur.
* Utilisez «vous» lorsque vous vous adressez à l’utilisateur.
* Utilisez «nous» pour faire référence à votre propre point de vue. Cela est plus chaleureux et donne à l’utilisateur l’impression de faire partie de l’expérience.
* N’utilisez pas «je» ou «moi» pour faire référence au point de vue de l’application, même si vous êtes le seul à la créer.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't save your file to that location.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="abbreviations"></a>Abréviations

 Les abréviations peuvent être utiles lorsque vous devez faire référence à des produits, des emplacements ou des concepts techniques plusieurs fois dans toute votre application. Elles peuvent économiser de l’espace et sont plus naturelles, tant que l’utilisateur les comprend.
* Ne partez pas du principe que les utilisateurs connaissent déjà les abréviations, même si vous pensez qu’il s’agit d’abréviations courantes.
* Donnez toujours la définition d’une nouvelle abréviation la première fois que l’utilisateur la voit.
* N’utilisez pas des abréviations qui se ressemblent trop.
* N’utilisez pas d'abréviations si vous localisez votre application ou si vos utilisateurs parlent l’anglais comme seconde langue.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        The Universal Windows Platform (UWP) design guidance is a resource to help you design and build beautiful, polished apps. With the design features that are included in every UWP app, you can build user interfaces (UI) that scale across a range of devices.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="contractions"></a>Contractions

Les lecteurs ont l'habitude des contractions et s'attendent à en trouver. En les évitant, vous risquez de donner à votre application un aspect trop austère ou même qui manque de naturel.
* Utilisez des contractions lorsqu’elles s’adaptent naturellement au texte.
* N’utilisez pas des contractions anti-naturelles juste pour économiser de l’espace ou si elles rendent vos propos moins accessibles.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        When you're happy with your image, select **save** to add it to your gallery. From there, you'll be able to share it with friends.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="periods"></a>Points

 Terminer un texte par un point implique que ce texte est une phrase complète. Utilisez un point pour les blocs de texte plus grands, et évitez d’en mettre lorsque le texte est plus court qu’une phrase complète.
* Utilisez un point à la fin de phrases complètes dans les info-bulles, les messages d’erreur et les boîtes de dialogue.
* N’utilisez pas de point à la fin du texte des boutons, des cases d’option, des étiquettes ou des cases à cocher.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="capitalization"></a>Mise en majuscules

Les majuscules sont importantes, mais il est facile d’en abuser.
* Mettez en majuscules les noms propres.
* Mettez en majuscule le début de chaque chaîne de texte dans votre application: le début de chaque phrase, étiquette et titre.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        * I forgot your password.
        * It won't accept password.
        * Someone else might be using my account.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="error-messages"></a>Messages d’erreur

Lorsqu’un problème se produit dans votre application, les utilisateurs font plus attention. Comme ils peuvent se sentir confus ou frustrés lorsqu’ils rencontrent un message d’erreur, un style et un ton adaptés peuvent avoir un impact particulièrement important.

Avant tout, il est important que votre message d’erreur ne blâme pas l’utilisateur. Mais il est également important de ne pas le surcharger d'informations qu’il ne comprend pas. La plupart du temps, un utilisateur qui rencontre une erreur souhaite juste revenir à ce qu’il était en train de faire aussi rapidement et facilement que possible. Par conséquent, un message d’erreur que vous rédigez doit:

* **Soyez chaleureux et détendu** en utilisant le ton de la conversation et en évitant les termes inconnus et le jargon technique.

* **Être prêt à aider** en expliquant de votre mieux à l’utilisateur ce qui s’est passé, en lui indiquant ce qui va se passer ensuite et en fournissant une solution réaliste qu’il peut effectuer.

* **Soyez clair et concis** en éliminant les informations superflues.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end::: 

## <a name="dialogs"></a>Boîtes de dialogue

:::row:::
    :::column:::
        Many of the same advice for writing error messages also applies when creating the text for any dialogs in your app. While dialogs are expected by the user, they still interrupt the normal flow of the app, and need to be helpful and concise so the user can get back to what they were doing.

        But most important is the "call and response" between the title of a dialog and its buttons. Make sure that your buttons are clear answers to the question posed by the title, and that their format is consistent across your app.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        1. I forgot my password
        2. It won't accept my password
        3. Someone else might be using my account
    :::column-end:::
:::row-end:::

## <a name="buttons"></a>Boutons

:::row:::
    :::column:::
        Text on buttons needs to be concise enough that users can read it all at a glance and clear enough that the button's function is immediately obvious. The longest the text on a button should ever be is a couple short words, and many should be shorter than that.
        When writing the text for buttons, remember that every button represents an action. Be sure to use the *active voice* in button text, to use words that represent actions rather than reactions.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        * Install now
        * Share
    :::column-end:::
:::row-end:::

## <a name="spoken-experiences"></a>Expériences vocales

Les mêmes recommandations et principes généraux s’appliquent à l’écriture de texte pour les expériences vocales, comme Cortana. Dans ces fonctionnalités, les principes d’écriture efficace sont encore plus importants, car vous ne pouvez pas fournir aux utilisateurs d’autres éléments de conception visuelle pour compléter les paroles.

* **Soyez chaleureux et détendu** en vous adressant aux utilisateurs sur le ton de la conversation. Plus que dans tout autre domaine, il est vital qu’une expérience vocale soit chaleureuse et conviviale pour que les utilisateurs se sentent à l’aise pour parler.

* **Soyez prêt à aider** en faisant d’autres suggestions lorsque l’utilisateur demande l’impossible. Comme dans un message d’erreur, si un problème s’est produit et que votre application ne peut pas traiter la demande, elle doit fournir à l’utilisateur une alternative réaliste à essayer.

* **Soyez clair et concis** en utilisant un langage simple. Les expériences vocales ne sont pas adaptées aux phrases longues ou aux mots compliqués.

## <a name="accessibility-and-localization"></a>Accessibilité et localisation

Votre application peut atteindre un public plus large si elle est écrite en tenant compte de l’accessibilité et de la localisation. Cela n’est possible qu’au moyen de texte, même si un langage simple et convivial est un très bon début. Pour plus d’informations, consultez notre [vue d’ensemble de l’accessibilité](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) et nos [recommandations en matière de localisation](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Soyez prêt à aider** en tenant compte de la multiplicité des expériences. Évitez les expressions qui peuvent ne pas avoir de sens pour un public international et n’utilisez pas de mots qui créent des hypothèses sur ce que l’utilisateur peut ou ne peut pas faire.

* **Soyez clair et concis** en évitant les mots inhabituels et spécialisés lorsqu’ils sont inutiles. Plus votre texte est simple, plus il est facile à localiser.


## <a name="techniques-for-non-writers"></a>Techniques pour les personnes qui ne sont pas des rédacteurs

Vous n’avez pas besoin d’être un rédacteur formé ou expérimenté pour fournir une expérience efficace à vos utilisateurs. Choisissez des mots avec lesquels vous êtes à l'aise. Les utilisateurs les apprécieront également. Mais parfois, cela n’est pas aussi facile qu’il y paraît. Si vous êtes bloqué, les techniques suivantes peuvent vous aider. 

* Imaginez que vous parlez à un ami de votre application. Comment lui expliquerez-vous l’application? Comment souhaitez vous parler de ses fonctionnalités ou lui fournir des instructions? Mieux encore, expliquez l’application à une personne qui ne l’a pas encore utilisée. 

* Imaginez comment vous décririez une application complètement différente. Par exemple, si vous créez un jeu, pensez à ce que vous diriez ou écririez pour décrire une application financière ou journalistique. Le contraste dans le langage et la structure que vous utilisez peut vous aider à trouver les bons mots pour illustrer vos propos.

* Examinez des applications similaires pour vous inspirer. 

Trouver les mots adaptés est un problème pour beaucoup de gens. Ne vous inquiétez donc pas s’il est difficile de parvenir à ce qui vous paraîtrait naturel.