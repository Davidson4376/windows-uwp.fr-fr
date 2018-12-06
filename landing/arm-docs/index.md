---
layout: LandingPage
description: Cette page fournit les informations pour pouvoir commencer à développer win32 ARM64 et les applications UWP.
title: Windows10 sur ARM
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: Windows 10 sur ARM, ARM, créer des applications win32 ARM64, génération de pilotes ARM64
ms.openlocfilehash: 83f2a0d03040a682e6965558174294fe27e21bfb
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748616"
---
# <a name="windows-10-on-arm"></a>Windows10 sur ARM
Windows 10 s’exécute sur les ordinateurs munis de processeurs ARM. Cette page fournit les informations pour en savoir plus sur la plateforme et de commencer à développer des applications. Nous vous encourageons également à fournir vos commentaires en utilisant les liens en bas de la page.

## <a name="introductory-videos"></a>Vidéos d’introduction
Regardez et découvrez comment Windows 10 s’exécute sur ARM.

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>Création d’applications de C++ Win32 ARM64</h3><p>Découvrez comment installer les outils de ARM64 pour Visual Studio. Puis nous allons vous guidons à travers les étapes de création et de compilation d’un nouveau projet 64 ARM.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>Build 2018 Windows 10 sur ARM pour les développeurs</h3><p>En savoir plus sur Windows 10 sur appareils ARM, comment la magie de x86 émulation fonctionne et enfin l’envoi et de créer des applications pour Windows 10 sur ARM. Nous être sera montrant comment créer des applications de ARM64 pour les ordinateurs de bureau et UWP.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>Communauté Windows quotidienne avec David Gallo</h3><p>Acquérir une connaissance approfondie de la façon dont Windows 10 s’exécute sur ARM64 et obtenir une idée des applications et des expériences sur cette plateforme.</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>Comprendre Windows 10 sur ARM
Découvrez la plateforme en fonction de ces ressources.

<ul class="cardsF panelContent cols cols2">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm" title="Prise en main" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Get started icon" src="/media/common/i_get-started.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Vous familiariser avec Windows 10 sur ARM</h3>
                    <p class="x-hidden-focus">Consultez la documentation pour comprendre les notions de base.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="Rubrique environ x86 émulation" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Découvrez comment x86 works d’émulation</h3>
                    <p class="x-hidden-focus">Découvrez tout sur cette fonctionnalité clée de Windows 10 sur ARM.</p>
                </div>
            </div>
        </div>
    </li>
    <!--<li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.msdn.microsoft.com/harip/" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="" src="/media/common/i_blog.svg?branch=master" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Read the Kernel blog</h3>
                    <p class="x-hidden-focus">Get a deep understanding of the Windows by reading articles that are written by the creators of the kernel.</p>
                </div>
            </div>
        </div>
    </li>-->
</ul>

## <a name="developing-for-windows-10-on-arm"></a>Développement pour Windows 10 sur ARM
Démarrez la personnalisation de vos applications vers Windows 10 sur ARM et tirer parti des fonctionnalités disponibles il.  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="Création d’applications ARM64" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>Création d’applications ARM64 avec le SDK</h3>
                    <p class="x-hidden-focus">Consultez ce billet de blog où nous vous guidons par le biais de la compilation de vos applications en tant qu’ARM64 à s’exécuter en mode natif sur Windows 10 sur ARM.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="Résolution des problèmes liés aux applications arm32" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Applications UWP sur ARM</h3>
                    <p class="x-hidden-focus">Suivez ces conseils pour définir vos applications de plateforme Windows universelle (UWP) pour la réussite.</p>                    
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/debugger/debugging-arm64" title="Débogage des applications ARM64" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="Debugging on ARM icon" src="/media/common/i_debug.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Débogage sur ARM</h3>
                    <p class="x-hidden-focus">Obtenir votre code en cours d’exécution effectue en toute transparence sur Windows 10 sur ARM.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/develop/building-arm64-drivers" title="Génération de pilotes ARM64" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Building ARM64 drivers icon" src="/media/common/i_drivers.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Génération de pilotes ARM64 avec le kit WDK</h3>
                    <p class="x-hidden-focus">Recompiler vos pilotes pour ARM64.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="Résolution des problèmes de x86 applications" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>x86 applications sur ARM</h3>
                    <p class="x-hidden-focus">Développer votre x86 les applications pour effectuer leurs mieux sur Windows 10 sur ARM.</p>
                </div>
            </div>
        </div>
    </li>
</ul>

<!--## Other videos
<ul class="cols cols4">
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
</ul>-->

## <a name="let-us-know-if-you-have-feedback"></a>Faites-nous savoir si vous avez des commentaires
Nous lançons continuellement notre produit par des commentaires en exploitant de vous et nos clients existants. Si vous avez une idée, sont bloqués sur un problème ou que vous souhaitez simplement partager l’importance votre expérience est, ces liens vous aidera.

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>Utiliser le hub de commentaires</h3>
                <p>Nous avons manqué quelque chose? Vous disposez d’une bonne idée? Faites-le nous savoir dans le Hub de commentaires.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>Signaler un bogue</h3>
                <p>Trouvé un bogue dans notre plateforme? Envoyez-nous un e-mail contenant les détails.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>Envoyer vos commentaires doc</h3>
                <p>Ont rencontré un problème avec notre documentation? Voulez-vous nous faire quelque chose de plus claire? Créer un problème sur notre référentiel GitHub de documents.</p>
            </div>
        </a>
    </li>
</ul>