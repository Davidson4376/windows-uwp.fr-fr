---
description: Découvrez comment rendre votre application inclusive et accessible aux personnes dans le monde entier.
keywords: accessibilité des applications UWP, globalisation, concevoir des applications inclusives, exigences des applications d’accessibilité
title: Facilité d’utilisation des apps UWP - Développement d’apps Windows
layout: LandingPage
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: a6912932b7ad71fd3d04c038eab7e0aa4dd6cb11
ms.sourcegitcommit: 2fa2d2236870eaabc95941a95fd4e358d3668c0c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076390"
---
# <a name="usability-for-uwp-apps"></a>Facilité d’utilisation des applications UWP

Ce sont les petits détails qui peuvent transformer une bonne expérience utilisateur en une expérience utilisateur véritablement inclusive qui réponde aux besoins des utilisateurs du monde entier.

Grâce aux instructions de conception et de codage de cette section, vous pourrez rendre votre application UWP plus inclusive en incluant des fonctionnalités d’accessibilité, de globalisation et de localisation, la possibilité de personnaliser l’expérience utilisateur et l’aide fournie aux utilisateurs lorsqu’ils en ont besoin.

## <a name="accessibility"></a>Accessibilité

L’accessibilité consiste à rendre vos applications utilisables par des personnes ayant des limites qui empêchent ou entravent l’utilisation d’interfaces utilisateur conventionnelles. Pour certaines situations, les exigences en matière d’accessibilité sont imposées par la loi. Il est toutefois préférable de gérer les aspects liés à l’accessibilité quelles que soient les exigences juridiques, afin que votre application ait l’audience la plus étendue possible.

[Portail d’accessibilité](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your UWP app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>Paramètres d’application

Les paramètres d’application permettent à l’utilisateur de personnaliser votre application afin de l’adapter à ses propres besoins et préférences. Grâce à une configuration et à un stockage adéquats des paramètres, une expérience utilisateur de bonne qualité peut devenir encore meilleure.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">Recommandations</a></b><br/>Meilleures pratiques pour créer et afficher des paramètres d’application</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">Stocker et récupérer des données d’application</a></b><br/>Décrit comment stocker et récupérer des données d’application locales, itinérantes et temporaires.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="globalization-and-localization"></a>Globalisation et localisation

Windows est utilisé dans le monde entier par des personnes de diverses cultures, régions et langues. Vos utilisateurs parlent différentes langues dans divers pays et régions. Certains utilisateurs parlent plusieurs langues. Par conséquent, votre application s’exécute sur des configurations qui impliquent de nombreux changements de paramètres système de langue, de région et de culture. Élargissez le marché potentiel de votre application en la rendant facilement adaptable avec les fonctionnalités d’*internationalisation* et de *localisation*.

<a href="../globalizing/globalizing-portal.md">Portail d’internationalisation et de localisation</a>

## <a name="in-app-help"></a>Aide dans l’application
Même si votre application a été très bien conçue, certains utilisateurs auront besoin d’un peu d’aide.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">Recommandations relatives à l’aide des applications</a></b><br/>Du fait de la complexité de certaines applications, la fourniture d’une aide efficace sur ces dernières peut améliorer considérablement l’expérience des utilisateurs.
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">Interface utilisateur d’instruction</a></b><br/>Dans certains cas, il peut être utile de former l’utilisateur aux fonctions les plus subtiles de votre application, telles que des interactions tactiles spécifiques. Vous devez alors fournir des instructions par le biais de l’interface utilisateur pour signaler à l’utilisateur les fonctionnalités dont il risque de ne pas encore avoir eu connaissance.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">Aide dans l’application</a></b><br/>Dans la plupart des cas, il est préférable d’afficher l’aide au sein de l’application et à la demande de l’utilisateur. Tenez compte des recommandations suivantes lors de la création d’une aide dans l’application.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">Aide externe</a></b><br/>Dans la plupart des cas, il est préférable d’afficher l’aide au sein de l’application et à la demande de l’utilisateur. Tenez compte des recommandations suivantes lors de la création d’une aide dans l’application.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul>

