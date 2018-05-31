---
author: QuinnRadich
title: Voix et ton
description: Utiliser une voix et un ton adaptés est essentiel pour que le texte de votre application semble faire naturellement partie de sa conception.
keywords: UWP, Windows10, voix, ton, texte, écriture, conception, interface utilisateur, expérience utilisateur
ms.author: quradic
ms.date: 8/52/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c95359afb148c6c77f3297aaacd5cb55e366c51
ms.sourcegitcommit: db09dcb08da5995c46c2729896e56be3774ee5ba
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2018
ms.locfileid: "1554536"
---
# <a name="voice-and-tone"></a>Voix et ton

Des instructions détaillées jusqu'aux info-bulles simples, le texte est un composant nécessaire de presque toutes les applications. En tant que partie essentielle de l’expérience utilisateur, le texte doit apporter un plus à la conception d’une application, en s'ajustant à sa fonction et à sa présentation visuelle.

Comme pour tout type d'écrit, vous devez décider d'un choix de voix et de ton pour créer le texte de votre application. Ce sont deux notions similaires mais distinctes: pensez à la voix comme à votre attitude envers l’objet, le ton étant sa mise en œuvre. Puisque le texte de votre application est écrit pour l’utilisateur, votre voix doit toujours être conviviale et naturelle, ce qui peut permettre de créer un lien immédiat. Votre ton peut être plus souple, pourvu qu’il suive quelques recommandations générales et fonctionne correctement avec cette voix et avec les besoins de votre application. En choisissant soigneusement des mots et des expressions adaptés, vous pouvez améliorer l’expérience utilisateur dégagée par l’application et rendre des situations négatives (une erreur, par exemple) plus faciles et plus agréables à vivre pour l’utilisateur.

## <a name="creating-a-friendly-natural-voice"></a>Création d’une voix conviviale et naturelle

La voix de votre application est une extension naturelle et intuitive de son objectif et de sa conception. Choisissez des mots avec lesquels vous êtes à l'aise. L’utilisateur les appréciera également.

Une stratégie utile consiste à imaginer que vous parlez à un utilisateur hypothétique de votre application. Quelqu'un que vous connaissez déjà, peut-être un membre de la famille ou un associé. Comment lui expliquerez-vous l’application? Comment souhaitez vous parler de ses fonctionnalités, comment souhaitez-vous fournir des instructions à cet utilisateur? Vous savez modifier votre voix et votre ton en fonction du sujet lorsque vous parlez. Cela peut vous aider aussi pour écrire.

Inversement, il peut également être utile d’imaginer comment vous décririez une application entièrement différente. Si vous créez un jeu, pensez à ce que vous diriez ou écririez pour décrire une application financière ou journalistique. Si vous créez une application conçue pour stimuler la productivité, imaginez comment parler d'une application éducative. En étudiant la langue et la structure utilisables pour discuter de différents sujets, vous pouvez parfois discerner quoi faire pour traiter le sujet sur lequel vous écrivez.

Pour trouver d'autres idées, jetez un coup d’œil à des applications similaires pour vous en inspirer ou faites des recherches sur le web pour avoir d'autres conseils. Trouver une voix adaptée est un problème pour beaucoup de gens. Ne vous inquiétez donc pas s’il est difficile de parvenir à ce qui vous paraîtrait idéal.

## <a name="general-tone-advice-for-uwp-apps"></a>Conseils généraux sur le ton à adopter pour les applications UWP

L'espace est limité dans les applications. Il est important de bien l’utiliser. Quel que soit le ton spécifique que nécessite votre application, nous vous recommandons de garder ces principes en tête lors de l’écriture de son texte:

#### <a name="lead-with-whats-important"></a>Commencez par ce qui est important

Les utilisateurs doivent pouvoir lire et comprendre votre texte d'un seul coup d'œil. N’étouffez pas vos mots sous des introductions inutiles. Donnez un maximum de visibilité à vos points clés et présentez toujours le fond d’une idée avant d’ajouter des précisions.

**Bon:** sélectionnez des «filtres» pour ajouter des effets à votre image.

**Mauvais:** si vous souhaitez ajouter des effets visuels ou des modifications à votre image, sélectionnez «filtres».

#### <a name="clarity-is-key"></a>La clarté est essentielle

Utilisez un langage qui sera familier à votre public. Cela signifie généralement de choisir des mots ordinaires et quotidiens, mais les applications ayant des usages spécifiques auront leurs propres normes. Vos utilisateurs doivent jamais avoir à se demander ce que votre texte signifie. Optez donc pour la simplicité si vous n’êtes pas sûr du ton à utiliser.

**Bon:** sélectionnez «Paramètres» pour modifier la façon dont l’application enregistre vos photos.

**Mauvais:** le comportement par défaut de l’application peut être configuré dans le menu «Paramètres».

#### <a name="contractions-arent-a-problem"></a>Un langage familier n'est pas un problème

Les lecteurs ont l'habitude des éléments de langage familiers et s'attendent à en trouver. En les évitant, vous risquez de donner à votre application un aspect trop austère ou même qui manque de naturel.

**Bon:** lorsque vous êtes satisfait de votre image, appuyez sur «Enregistrer» pour l’ajouter à votre bibliothèque. Vous pourrez ensuite la partager avec vos amis.

**Mauvais:** Lorsque les modifications que vous avez apportées à l'image vous satisfont, appuyez sur «Enregistrer» pour ajouter cette image à votre bibliothèque. Cela vous permettra de la partager ensuite avec vos amis.

#### <a name="emphasize-action"></a>Mettre les actions en évidence

Les applications sont définies par des actions. Les utilisateurs agissent en utilisant l’application et celle-ci agit en répondant à leurs actions. Assurez-vous que votre texte utilise la *voix active* tout au long de l'application. Les utilisateurs et les fonctions doivent être décrits comme faisant quelque chose, non comme si l'on faisait les choses pour eux.

**Bon:** Relancez l’application pour voir vos modifications.

**Mauvais:** Les modifications seront appliquées lors du redémarrage de l’application.

#### <a name="short-and-sweet"></a>Bref et concis

Les utilisateurs survolent le texte et en ignorent souvent des blocs entiers. Ne sacrifiez ni les informations indispensables ni la présentation, mais n’utilisez pas plus de mots que nécessaire. Cela implique parfois d'utiliser de nombreuses phrases plus courtes ou de simples fragments. Dans d’autres cas, il faudra choisir avec beaucoup de soin les mots et la structure de phrases plus longues.

**Bon:** Désolés, nous n’avons pas pu charger l’image. Si cela se produit à nouveau, essayez de relancer l’application. Mais ne vous inquiétez pas: vous retrouverez votre image à votre retour.

**Mauvais:** Une erreur s’est produite et nous n’avons pas pu charger l’image. Veuillez réessayer et, si vous rencontrez à nouveau ce problème, vous devrez peut-être relancer l’application. Mais ne vous inquiétez pas: nous avons enregistré votre travail en local et il vous attendra à votre retour.

