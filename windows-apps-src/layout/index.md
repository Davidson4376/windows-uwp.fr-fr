---
description: "Découvrez comment concevoir et coder une application UWP intuitive qui s’affiche parfaitement sur un grand nombre d’appareils et sur des écrans de différentes tailles."
title: "Conception de disposition pour les app. UWP - Dév. d’app. Windows"
author: mijacobs
keywords: "disposition d’application uwp, plateforme windows universelle, conception d’application, interface"
label: Layout
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 1aa12606-8a99-4db3-8311-90e02fde9cf1
ms.openlocfilehash: 1034588565032301cb0746d79a122e8388dad8f9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="layout-for-uwp-apps"></a>Disposition pour les applications UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


La structure de l’application, la mise en page et la navigation sont les trois éléments clés de l’expérience utilisateur de votre application. Les articles de cette section vous aident à créer une application intuitive qui s’affiche parfaitement sur un grand nombre d’appareils et sur des écrans de différentes tailles.

## <a name="intro"></a>Introduction

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[Introduction à la conception de l’interface utilisateur d’une application](design-and-ui-intro.md)</b><br />
Lorsque vous concevez une application UWP, vous créez une interface utilisateur adaptée à une multitude d’appareils dotés d’écrans de tailles différentes. Cet article fournit un aperçu des fonctionnalités et des avantages des applications UWP en matière d’interface utilisateur, ainsi que quelques conseils et astuces pour concevoir une interface utilisateur réactive. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Application exécutée sur plusieurs appareils](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## <a name="app-layout-and-structure"></a>Disposition et structure d’application
Consultez ces recommandations pour structurer votre application et exploiter les trois types d’éléments d’interface utilisateur: navigation, commande et contenu.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[Notions de base sur la navigation](navigation-basics.md)</b><br/>
La navigation dans les applications UWP est basée sur un modèle flexible de structures de navigation, d’éléments de navigation et de fonctionnalités au niveau du système. Cet article présente ces composants et vous explique comment les combiner pour créer une bonne expérience de navigation.
</p>
<p>
<b>[Notions de base sur le contenu](content-basics.md)</b><br/>
Le rôle principal de toute application est d’offrir un accès à un contenu. Dans une application de retouche photo, le contenu correspond aux photos; dans une application de voyage, le contenu comprend les cartes et les informations sur les destinations, etc. Cet article fournit des recommandations de conception de contenu pour les troisscénarios de contenu: consommation, création et interaction.
</p> 
  </div>
  <div class="side-by-side-content-right">
<p><b>[Notions de base sur les commandes](commanding-basics.md)</b> <br />
Les éléments de commande sont les éléments d’interface utilisateur interactifs qui permettent à l’utilisateur d’effectuer des actions telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire. Cet article décrit les éléments de commande, comme les boutons et les cases à cocher, les interactions qu’ils prennent en charge, ainsi que les surfaces de commandes (telles que les barres de commandes et les menus contextuels) pouvant les accueillir.</p>
  </div>
</div>
</div>

## <a name="page-layout"></a>Mise en page 
Grâce à ces articles, vous pourrez créer une interface utilisateur flexible, qui s’adapte à différentes tailles d’écran et de fenêtre, résolutions et orientations. 


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Tailles d’écran et points d’arrêt](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
Le nombre d’appareils cibles et de tailles d’écran dans l’écosystème Windows10 est trop élevé pour pouvoir optimiser votre interface utilisateur pour chacun d’eux. À la place, nous vous recommandons de concevoir une application pour plusieurs largeurs principales (également appelées «points d’arrêt»): 360, 640, 1024 et 1366epx.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Définir des dispositions avec XAML](layouts-with-xaml.md)</b> <br/>
Comment utiliser les propriétés XAML et les panneaux de disposition pour que votre application soit réactive et adaptative.</p>
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Panneaux de disposition](layout-panels.md)</b> <br />
Découvrez les types de panneaux de disposition et apprenez à les utiliser pour disposer les éléments d’interface utilisateur XAML.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Alignement, marge et espacement](alignment-margin-padding.md)</b> <br />
Outre les propriétés de dimension (largeur, hauteur et contraintes), les éléments peuvent aussi avoir des propriétés d’alignement, de marge et d’espacement qui influencent le comportement de disposition lorsqu’un élément subit une passe de disposition et est affiché dans une interface utilisateur.</p> 
  </div>
</div>
</div>


