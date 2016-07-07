---
author: Mtoepke
title: "Présentation des applications multi-utilisateur"
description: 
area: Xbox
ms.sourcegitcommit: f225811bd18be22807160e8670a1b7b8d51e4b10
ms.openlocfilehash: 20f84783131122343fd01e6cb1f5a60cf50158cd

---

# Présentation des applications multi-utilisateur

Cette rubrique est une simple présentation générale du modèle multi-utilisateur Xbox.

> **Remarque** &nbsp;&nbsp;Dans cette première version préliminaire pour développeurs, les applications multi-utilisateur ne sont pas activées. Ces applications seront activées dans une future version préliminaire pour développeurs, et nous publierons alors une documentation, des recommandations et des exemples plus détaillés sur le sujet. 

Le modèle utilisateur XboxOne répond aux exigences d’une console de jeu qui prend en charge plusieurs utilisateurs jouant ensemble sur un seul et même appareil. Ce modèle permet à plusieurs utilisateurs, disposant chacun de leur propre manette, de se connecter et d’accéder à la console simultanément dans le cadre d’une même session interactive. Cette technique diffère de celle mise en œuvre sur d’autres appareils Windows. Par exemple:
* Les **PC de bureau Windows** permettent à plusieurs utilisateurs d’utiliser le même appareil, mais chaque utilisateur dispose de sa propre session interactive, et chacune des sessions est totalement indépendante des autres sessions sur l’appareil.
* Les **téléphones Windows** ne sont utilisables que par une seule personne. Cet utilisateur unique est déterminé lors de la phase OOBE (out-of-box experience) et ne peut plus se déconnecter une fois connecté. En effet, si une autre personne veut utiliser l’appareil, ce dernier doit être réinitialisé. 
* Les **consoles Xbox One** permettent à plusieurs utilisateurs de se connecter et d’utiliser l’appareil en même temps au cours d’une seule et même session interactive.

Chaque utilisateur du modèle utilisateur XboxOne dispose d’un compte d’utilisateur local. Ce compte d’utilisateur local est associé à un compte XboxLive (et donc à un compte Microsoft). Cela signifie qu’il existe un mappage strict un-à-un entre un compte d’utilisateur Xbox et un compte XboxLive ou un compte Microsoft.

## Applications mono-utilisateur
Par défaut, une application UWP s’exécute dans le contexte de l’utilisateur ayant lancé l’application. Ces «applications mono-utilisateur» ne reconnaissent que cet utilisateur unique et s’exécutent dans un mode compatible avec le modèle utilisateur sur d’autres appareils Windows. Le modèle utilisateur Xbox gère l’utilisateur associé à l’application et garantit qu’un utilisateur est connecté au moment où l’application est lancée. Dans ce modèle, les auteurs d’applications et de jeux UWP n’ont aucune opération spéciale à effectuer pour exécuter ces programmes sur Xbox. 

## Applications multi-utilisateur
Les jeux UWP peuvent choisir de souscrire au modèle multi-utilisateur XboxOne. Ces «applications multi-utilisateur» s’exécutent dans le contexte d’un compte système (appelé compte par défaut) et peuvent tirer pleinement parti de la flexibilité et de la puissance du modèle utilisateur XboxOne. Dans le cas de ces jeux, le modèle utilisateur Xbox ne gère pas l’utilisateur associé au jeu et n’exige même pas qu’un utilisateur soit connecté pour que le jeu s’exécute. Cela implique que ces jeux soient écrits pour reconnaître explicitement et gérer leurs exigences vis-à-vis des utilisateurs, notamment la nécessité ou non qu’un utilisateur soit connecté, l’éventuelle implémentation du concept d’utilisateur actuel, l’autorisation ou l’interdiction des entrées simultanées par plusieurs utilisateurs, etc.
   
Pour choisir le modèle multi-utilisateur, procédez comme suit:   
1. Ouvrez votre projet dans VisualStudio.   
2. Sélectionnez le fichier package.appxmanifest.xml.   
3. Cliquez avec le bouton droit sur ce dernier et sélectionnez Afficher le code.   
4. Ajoutez la ligne suivante dans la section `<Properties></Properties>`:

`<uap:SupportedUsers>multiple</uap:SupportedUsers>`

##Aide concernant le modèle à utiliser
Toutes les applications UWP et la majorité des jeux pour joueur unique peuvent être écrits sous la forme d’applications mono-utilisateur. Le choix du modèle multi-utilisateur XboxOne n’est recommandé que pour les jeux multijoueur coopératifs. Nous fournirons une documentation, des recommandations et des exemples plus détaillés à ce sujet dans une future version préliminaire pour développeurs.

## Voir aussi
- [UWP sur XboxOne](index.md)



<!--HONumber=Jun16_HO5-->


