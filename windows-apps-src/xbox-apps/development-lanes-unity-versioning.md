---
title&#58; Unity&#58; Contrôle de version de votre projet UWP - Auteur&#58; JordanEllis6809
---

# Unity&#58; Contrôle de version de votre projet UWP

Vous n’avez pas toujours pas créé votre jeuUnity pour Xbox avec UWP?  Commencez par lire [cet article](development-lanes-unity.md).

Plusieurs raisons peuvent motiver votre volonté d’ajouter des parties à votre répertoire UWP généré au contrôle de version, notamment l’ajout de dépendances (p.ex., le Kit de développement logiciel XboxLive).  Nous allons utiliser ce scénario comme exemple dans le cadre de ce didacticiel, et nous espérons qu’il vous sera utile pour répondre aux besoins spécifiques de votre projet.

***Exclusion de responsabilité: nous allons utiliser Git comme solution de contrôle de version.  Si votre cas de figure est différent, les concepts doivent tout de même s’appliquer.***

Pour vous rafraîchir la mémoire, voici comment se présente actuellement le répertoire de notre jeu, ***ScrapyardPhoenix***:

![Dossier de destination du build](images/build-destination.png)

Et voici à quoi ressemble notre répertoire UWP:

![Solution Visual Studio UWP](images/uwp-vs-solution.png)

Dans ce répertoire, nous ne sommes concernés que par un seul dossier, le dossier ***ScrapyardPhoenix*** (insérez le nom de votre jeu ici).  Tous les autres éléments peuvent être ignorés dans notre contrôle de version:

***Vous ne savez pas ce qu’est un fichier .gitignore?  En savoir plus [ici](https://git-scm.com/docs/gitignore).***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

Nous allons sélectionner quelques fichiers et dossiers différents depuis le dossier `UWP/ScrapyardPhoenix` à ajouter à notre contrôle de version.  Tout d’abord, examinons la chose dans son intégralité et en détail:

![Répertoire des builds UWP](images/uwp-build-directory.png)  

## Dossiers  

`Assets` | 
            ***Include*** | Contient les images du Windows Store.  
`Data`   | 
            ***Ignore*** | Où Unity compile votre projet (scènes, nuanceurs, scripts, préfabriqués, etc).  
`Dependencies` | 
            ***Include*** | J’ai créé ce dossier pour y conserver toutes les dépendances UWP (p.ex., XboxLiveSDK.dll).  
`Properties` | 
            ***Include*** | Contient des paramètres plus avancés qui peuvent être modifiés par le développeur.  
`Unprocessed` | 
            ***Ignore*** | Contient les fichiers Unity `.dll` et `.pdb`.  

## Fichiers  

`App.cs` | 
            ***Include*** | Point d’entrée pour votre application UWP.  Peut être modifié et étendu avec d’autres fichiers source.  
`Package.appxmanifest` | 
            ***Include*** | Manifeste du package pour votre AppX.  
`project.json` | 
            ***Include*** | Décrit les packages NuGet dont votre `*.csproj` dépend.  
`ScrapyardPhoenix.csproj` | 
            ***Include*** | Décrit la cible de votre build UWP.  Si vous ajoutez des dépendances supplémentaires à votre projet UWP, ce fichier `*.csproj` va contenir ces informations.  
`ScrapyardPhoenix.csproj.user` | 
            ***Ignore*** | Ce fichier contient des informations sur l’utilisateur local.

## Fichier .gitignore obtenu

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

Et voilà, maintenant vos coéquipiers seront synchronisés avec le projet UWP que vous avez généré.  Maintenant, n’hésitez pas à ajouter des ressources, sources et dépendances supplémentaires à votre projet UWP.

Quelques exemples supplémentaires de contrôle de version du dossier UWP sont disponibles [ici](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## Ajout de dépendances à notre application UWP

L’ajout de dépendances à votre application UWP s’effectue comme dans tout autre projet Visual Studio.  Ici, la seule partie qui est un peu moins courante est la recherche du projet auquel ajouter des dépendances.  Voici une image indiquant le projet à sélectionner (surligné):

![Solution UWP](images/uwp-solution.png)


            ***ScrapyardPhoenix (Windows universel)*** est le projet auquel vous ajoutez une référence, par exemple, le Kit de développement logiciel XboxLive.



<!--HONumber=Jul16_HO1-->


