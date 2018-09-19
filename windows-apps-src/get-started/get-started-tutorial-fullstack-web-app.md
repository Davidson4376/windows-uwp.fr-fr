---
author: libbymc
title: Créer une application Web avec page unique avec une API REST principale
description: Utiliser des technologies Web courantes pour créer une application Web hébergée pour le Microsoft Store
keywords: application web hébergée, HWA, API REST, application avec page unique, SPA
ms.author: libbymc
ms.date: 05/10/2017
ms.topic: article
ms.prod: Microsoft Edge, Azure, Visual Studio Code
ms.technology: web
ms.localizationpriority: medium
ms.openlocfilehash: 42f11cbdd749a44c4ba0d8bc1a0397a4f2882257
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4054175"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>Créer une application Web avec page unique avec une API REST principale

**Créer une application Web hébergée pour le Microsoft Store avec des technologies web populaires fullstack**

![Jeu de mémoire simple en tant qu'application Web à page unique](images/fullstack.png)

Ce didacticiel en deux parties offre une présentation rapide des technologies de développement Web fullstack modernes lorsque vous créez un jeu de mémoire simple qui fonctionne à la fois dans le navigateur et en tant qu’application Web hébergée pour le Microsoft Store. Dans la partie I, vous allez générer un service REST API simple pour le serveur principal du jeu. En hébergeant la logique du jeu dans le cloud en tant que service API, vous conservez l’état du jeu afin que votre utilisateur puisse continuer à jouer à sa même instance de jeu sur différents appareils. Dans la partie II, vous allez générer l’interface utilisateur frontale en tant qu'application Web à page unique avec une disposition réactive.

Nous allons utiliser certaines des technologies Web les plus populaires, y compris l'exécution [Node.js](https://nodejs.org/en/) et [Express](http://expressjs.com/) pour le développement côté serveur, l'infrastructure d'interface utilisateur [Bootstrap](http://getbootstrap.com/), le modèle de moteur [Pug](https://www.npmjs.com/package/pug) et [Swagger](http://swagger.io/tools/) pour la création des API RESTful. Vous gagnerez également en expérience au niveau du [portail Azure](https://ms.portal.azure.com/) pour l’hébergement dans le cloud et l’utilisation de l'éditeur [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Prérequis

Si vous ne disposez pas encore de ces ressources sur votre ordinateur, suivez ces liens de téléchargement:

 - [Node.js](https://nodejs.org/en/download/) - Veillez à sélectionner l’option pour ajouter un Node à votre environnement PATH.

 - [Générateur Express](http://expressjs.com/en/starter/generator.html)- Après avoir installé Node, installez Express en exécutant `npm install express-generator -g`

 - [VisualStudioCode](https://code.visualstudio.com/)

Si vous souhaitez terminer les dernières étapes de l’hébergement de votre service de l'API et de l'application de la mémoire de jeu sur MicrosoftAzure, vous devez [créer un compte Azure gratuit](https://azure.microsoft.com/en-us/free/) si vous ne l’avez pas déjà fait.

Si vous décidez de quitter (ou de reporter) la partie relative à Azure, ignorez simplement les sections finales des parties I et II, qui portent sur l’hébergement Azure et l’emballage de votre application du Microsoft Store. L’application du service de l'API et de l'application Web que vous générez sera toujours exécutée localement (à partir de `http://localhost:8000`et de `http://localhost:3000`, respectivement) sur votre ordinateur.

## <a name="part-i-build-a-rest-api-backend"></a>Partie I: créer un serveur principal de l’API REST

Nous allons tout d’abord créer une API de jeu simple pour alimenter l'application Web de la mémoire de jeu. Nous allons utiliser [Swagger](http://swagger.io/) pour définir notre API et générer le code de la structure ainsi qu'une interface utilisateur Web pour le test manuel.

Si vous souhaitez ignorer cette partie et passer directement à la [Partie II: Créer une application Web à page unique](#part-ii-build-a-single-page-web-appl), voici le [code finalisé pour la partie I](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend). Suivez les instructions *README* pour obtenir le code opérationnel localement ou consultez *5. Héberger votre service API sur Azure et activer CORS* pour l’exécuter à partir d’Azure.

### <a name="game-overview"></a>Vue d’ensemble du jeu

*Mémoire* (également appelée [*Concentration*](https://en.wikipedia.org/wiki/Concentration_(game)) et [*Pelmanism*](https://en.wikipedia.org/wiki/Pelmanism_(system))), est un jeu simple constitué d’un jeu de paires de cartes. Les cartes sont placées face vers le bas sur le tableau, et le joueur inspecte les valeurs des cartes, deux à la fois, à la recherche de correspondances. Les cartes sont replacées face vers le bas après chaque tour, sauf si une paire correspondante est trouvée, auquel cas ces deux cartes sont effacées du jeu. L’objectif du jeu consiste à trouver toutes les paires de carte en un nombre minimal de tours.

À des fins de démonstration, la structure de jeu que nous utiliserons est très simple: il s’agit d’un jeu unique, à joueur unique. Toutefois, la logique de jeu est côté serveur (et non pas côté client) pour conserver l’état du jeu, afin que vous puissiez continuer de jouer au même jeu sur les différents appareils.

La structure de données pour un jeu de mémoire se compose simplement d’un tableau d’objets JavaScript, chacun d'eux représentant une seule carte, avec des index dans le tableau agissant en tant qu’ID de carte. Sur le serveur, chaque objet de carte a une valeur et un drapeau **désactivé**. Par exemple, un tableau avec 2 correspondances (4cartes) peut être généré de manière aléatoire et sérialisé comme suit.

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

Lorsque le tableau est transmis au client, les clés de valeur sont supprimées à partir du tableau pour empêcher toute tricherie (par exemple, en inspectant le corps de réponse HTTP à l’aide d'outils du navigateur F12). Voici comment ce même nouveau jeu se présente à un client appelant le point de terminaison REST **OBTENIR /jeu**:

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

À propos des points de terminaison, le service de jeu de la mémoire est constitué de trois API REST.

#### <a name="post-new"></a>PUBLIER /nouveau
Initialise un nouveau tableau de jeu de la taille spécifiée (nombre de correspondances).

| Paramètre | Description |
|-----------|-------------|
| int *taille* |Nombre de correspondances à mélanger dans le «tableau» du jeu. Exemple: `http://memorygameapisample/new?size=2`|

| Réponse | Description |
|----------|-------------|
| 200 OK | Le nouveau jeu de mémoire à la taille demandée est prêt.|
| 400 REQUÊTES INCORRECTES| La taille demandée se situe en dehors de la plage acceptable.|


#### <a name="get-game"></a>OBTENIR /jeu
Récupère l’état actuel du tableau du jeu de mémoire.

*Aucun paramètre*

| Réponse | Description |
|----------|-------------|
| 200 OK | Renvoie un tableau JSON des objets de carte. Chaque carte est associée à une propriété **désactivée** indiquant si sa correspondance a déjà été trouvée. Les cartes mises en correspondance indiquent également leur **valeur**. Exemple: `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PLACER /deviner
Spécifie une carte à révéler et recherche une correspondance pour la carte révélée précédemment.

| Paramètre | Description |
|-----------|-------------|
| int *carte* | ID de carte (indice dans le tableau de jeu) de la carte à révéler. Chaque «devinette» finalisée se compose de deux cartes spécifiées (par exemple, deux appels à **/deviner** avec des valeurs de *carte* valides et uniques). Exemple: `http://memorygameapisample/guess?card=0`|

| Réponse | Description |
|----------|-------------|
| 200 OK | Renvoie JSON avec l'**id** et la **valeur** de la carte spécifiée. Exemple: `[{"id":0,"value":1}]`|
| 400 REQUÊTES INCORRECTES |  Erreur avec la carte spécifiée. Reportez-vous au corps de réponse HTTP pour en savoir plus.|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. Spécification de l’API et génération de codes stubs

Nous allons utiliser [Swagger](http://swagger.io/) pour transformer la structure de l'API de notre jeu de mémoire pour en faire un code de serveur Node.js opérationnel. Voici comment vous pouvez définir nos [API de jeu de mémoire en tant que métadonnées Swagger](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json). Nous l’utiliserons pour générer des stubs de code serveur.

1. Créez un nouveau dossier (dans votre répertoire local *GitHub*, par exemple) et téléchargez le fichier [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) contenant nos définitions d’API du jeu la mémoire. Assurez-vous que le nom de votre dossier ne contient pas d’espaces.

2. Ouvrez votre interpréteur de commandes favori ([ou utilisez le terminal intégré de Visual Studio Code](https://code.visualstudio.com/docs/editor/integrated-terminal)) pour ce dossier et exécutez la commande NPM (Node Package Manager) suivante pour installer l'outil de génération de code [Yeoman](http://yeoman.io/) (yo) et le générateur Swagger pour votre environnement Node global (**-g**):

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. Nous pouvons désormais générer le code de la structure de serveur à l’aide de Swagger:

    ```
    yo swaggerize
    ```

4. La commande **swaggerize** vous posera plusieurs questions.
    - Chemin d’accès (ou URL) vers le document swagger: **api.json**
    - Infrastructure: **express**
    - Nom que vous souhaitez donner à ce projet (YourFolderNameHere): **[Entrée]**

    Répondez à tous les autres éléments de votre choix; les informations sont principalement destinées à renseigner le fichier *package.json* avec vos coordonnées afin que vous puissiez distribuer votre code comme package NPM.

5. Enfin, installez toutes les dépendances (répertoriées dans *package.json*) pour la prise en charge de votre nouveau projet et de [l’interface utilisateur Swagger](http://swagger.io/swagger-ui/).

    ```
    npm install
    npm install swaggerize-ui
    ```

    Ouvrez maintenant Code Visual Studio et **Fichier** > **Ouvrir le dossier... **et accédez au répertoire MemoryGameAPI. Il s'agit du serveur d’API Node.js que vous venez de créer! Celui-ci utilise l'infrastructure d'application Web [ExpressJS](http://expressjs.com/en/4x/api.html) courante pour structurer et exécuter votre projet.

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. Personnalisez le code du serveur et configurez le débogage

Le fichier *server.js* dans la racine de votre projet opère comme la fonction «principale» de votre serveur. Ouvrez-le dans Visual Studio Code et copiez-y les éléments suivants. Les lignes modifiées dans le code généré sont commentées avec des explications plus détaillées.

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

Il est temps d’exécuter votre serveur! Et tant que nous y sommes, nous allons configurer Code Visual Studio pour le débogage de Node. Sélectionnez l'icône du panneau **Déboguer** (Ctrl + Maj + D), puis l’icône d’engrenage (ouvrez launch.json) et modifiez les «configurations» comme suit:

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

Appuyez maintenant sur F5 et ouvrez votre navigateur sur [http://localhost:8000](http://localhost:8000). La page doit s’ouvrir et afficher l’interface utilisateur Swagger pour notre API de jeu de mémoire, et à partir de là, vous pourrez développer les détails et renseigner les champs de chacune des méthodes. Vous pouvez même essayer d’appeler les API, bien que leurs réponses contiendront uniquement des données fictives (fournies par le module [Swagmock](https://www.npmjs.com/package/swagmock)). Il est temps d’ajouter notre logique de jeu pour rendre ces API effectives.

### <a name="3-set-up-your-route-handlers"></a>3. Configurer vos gestionnaires d’itinéraire

Le fichier Swagger (config\swagger.json) indique à notre serveur comment gérer les diverses demandes des clients HTTP en cartographiant chaque chemin d’accès URL qu'il définit pour un fichier de gestionnaire (dans \handlers) et chaque méthode définie pour ce chemin d’accès à notre serveur (par exemple, **OBTENIR**, **PUBLIER**) pour une `operationId` (fonction) dans le fichier gestionnaire.

Dans cette couche de notre programme, nous allons ajouter une vérification d’entrée avant de transmettre les diverses demandes des clients à notre modèle de données. Télécharger (ou copier et coller):

 - Ce code [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D) à votre fichier **handlers\game.js**
 - Ce code [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D) à votre fichier **handlers\guess.js**
 - Ce code [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D) à votre fichier **handlers\new.js**

 Vous pouvez filtrer les commentaires dans ces fichiers pour obtenir plus d’informations sur les modifications apportées, mais fondamentalement, ceux-ci examinent les entrées de base à la recherche d'éventuelles erreurs (par exemple, le client demande un nouveau jeu avec moins d’une correspondance) et envoient des messages d’erreur descriptifs, si besoin. Les gestionnaires acheminent également les demandes de client valides vers leurs fichiers de données correspondants (dans \data) en vue d'un traitement ultérieur. Nous allons travailler sur ces derniers.

### <a name="4-set-up-your-data-model"></a>4. Configurer votre modèle de données

Il est temps de remplacer l'espace réservé du service de simulation de données par un modèle de données réelles de notre tableau de jeu de mémoire.

Cette couche de notre programme représente les cartes mémoire elles-mêmes et fournit la majeure partie de la logique de jeu, y compris le «mélange» du jeu de cartes en vue d'un nouveau jeu, en identifiant les cartes mises en correspondance et en établissant le suivi de l'état du jeu. Copier-coller:

 - Ce code [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D) à votre fichier **data\game.js**
 - Ce code [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D) à votre fichier **data\guess.js**
 - Ce code [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D) à votre fichier **data\new.js**

Par souci de simplicité, nous stockons notre tableau de jeu dans une variable globale (`global.board`) sur notre serveur Node. Toutefois dans la pratique, vous utiliseriez le stockage dans le Cloud (comme le [Cloud Datastore](https://cloud.google.com/datastore/) de Google ou Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/)) pour faire de cette installation un service d’API de mémoire de jeu viable, prenant en charge simultanément plusieurs jeux et joueurs.

Assurez-vous d’avoir enregistré toutes les modifications dans VS Code, lancez à nouveau votre serveur (F5 dans VS Code ou `npm start` depuis le shell, puis accédez à [http://localhost:8000](http://localhost:8000)) pour tester l’API de jeu.

Chaque fois que vous appuyez sur le bouton **Essayez!** de l'une des fonctions **/jeu**, **/deviner**, ou **/nouveau**, vérifiez le **Corps de réponse** et le **Code de réponse** subséquents ci-dessous pour vérifier que tout fonctionne comme prévu.

Essayez: 

1. Création d'un nouveau `size=2` jeu.

    ![Démarrez un nouveau jeu de mémoire à partir de l’interface utilisateur Swagger](images/swagger_new.png)

2. Devinette de deux valeurs.

    ![Deviner une carte à partir de l’interface utilisateur Swagger](images/swagger_guess.png)

3. Vérification du tableau de jeu au fil de son déroulement.

    ![Vérifier l’état du jeu à partir de l’interface utilisateur Swagger](images/swagger_game.png)

Si tout s’affiche correctement, votre service de l’API est prêt à héberger sur Azure. Si vous rencontrez des problèmes, essayez de commenter les lignes suivantes dans \data\game.js.

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

Avec cette modification, la méthode **OBTENIR /jeu** retourne toutes les valeurs de carte (y compris celles qui n’ont pas été désactivées). Il s’agit d’une astuce de débogage utile à conserver lorsque vous créez le serveur frontal du jeu de mémoire.

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (Facultatif) Héberger votre service de l'API sur Azure et activer CORS

La documentation Azure vous guidera à travers les étapes suivantes:

 - [Enregistrement d'une nouvelle *application API* dans le portail Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [Configurer le déploiement Git pour votre application API](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git), et
 - [Déploiement de votre code d’application API vers Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

Lorsque vous enregistrez votre application, essayez de différencier votre *nom de l’application* (pour éviter les conflits de nom avec d’autres utilisateurs demandant des variantes sur l’URL *http://memorygameapi.azurewebsites.net* URL).

Si vous avez réussi toutes les étapes jusqu'à présent et qu'Azure diffuse désormais votre interface utilisateur Swagger, il ne vous reste plus qu'une étape à effectuer sur le système principal du jeu de mémoire. Sur le [portail Azure](https://portal.azure.com), sélectionnez votre *Service d’application* que vous venez de créer puis sélectionnez ou recherchez l'option **CORS** (Cross-Origin Resource Sharing). Sous **Origines autorisées**, ajoutez un astérisque (`*`) et cliquez sur **Enregistrer **. Cela vous permet de passer des appels à origines multiples vers votre service de l’API à partir du serveur frontal de votre jeu de mémoire tandis que vous le développez sur votre ordinateur local. Une fois que vous aurez finalisé le serveur frontal du jeu de mémoire et que vous l'aurez déployé vers Azure, vous pourrez remplacer cette entrée par l’URL spécifique de votre application Web.

### <a name="going-further"></a>Aller plus loin

Pour faire de l’API du jeu de mémoire un service principal viable pour une application de production, vous devez développer le code pour prendre en charge plusieurs jeux et joueurs. Pour ce faire, vous devrez probablement raccorder l'[authentification](http://swagger.io/docs/specification/authentication/) (pour la gestion des identités des joueurs), une [base de données NoSQL](https://docs.microsoft.com/en-us/azure/documentdb/) (pour le suivi des jeux et des joueurs) et certains éléments de base [tests d'unité](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis) correspondant à votre API.

Voici quelques ressources utiles pour aller plus loin:

 - [Débogage Node.js avancé avec Code Visual Studio](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Documentation Azure Web + Mobile](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Documentation Azure DocumentDB](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-application"></a>Partie II: Générer une application web à page unique

Maintenant que vous avez créé (ou [téléchargé](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)) le [serveur principal de l’API REST](#part-i-build-a-rest-api-backend) dans la partie I, vous êtes prêt à créer le serveur frontal du jeu de mémoire à page unique avec l'environnement [Node](https://nodejs.org/en/), [Express](http://expressjs.com/) et [Bootstrap ](http://getbootstrap.com/).

La partie II de ce didacticiel vous permettra de vous familiariser avec: 

* [Node.js](https://nodejs.org/en/): pour créer le serveur hébergeant votre jeu
* [jQuery](http://jquery.com/): une bibliothèque JavaScript
* [Express](http://expressjs.com/): pour l’infrastructure d’application web
* [Pug](https://pugjs.org/): (anciennement Jade) pour le moteur de création de modèles
* [Bootstrap](http://getbootstrap.com/): pour la disposition réactive
* [Visual Studio Code](https://code.visualstudio.com/): pour la création, l'affichage de markdown et le débogage de code

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. Création d’une application Node.js à l’aide d'Express

Commençons par créer le projet Node.js à l’aide d'Express.

1. Ouvrez une invite de commandes et accédez au répertoire où vous souhaitez stocker votre jeu. 
2. Utilisez le Générateur Express pour créer une nouvelle application appelée *mémoire* en utilisant le moteur de création de modèles, *Pug *.

    ```
    express --view=pug memory
    ```

3. Dans le répertoire de la mémoire, installez les dépendances répertoriées dans le fichier package.json. Le fichier package.json est créé dans la racine de votre projet. Ce fichier contient les modules requis pour votre application Node.js.  

    ```
    cd memory
    npm install
    ```

    Après avoir exécuté cette commande, vous devriez voir un dossier appelé node_modules qui contient tous les modules nécessaires pour cette application. 

4. Maintenant, exécutez votre application.

    ```
    npm start
    ```

5. Affichez votre application en accédant à [http://localhost:3000/](http://localhost:3000/).

    ![Capture d’écran de http://localhost:3000/](./images/express.png)

6. Modifiez le titre par défaut de votre application Web en modifiant index.js dans le répertoire memory\routes. Modifiez `Express`dans la ligne ci-dessous pour `Memory Game` (ou un autre titre de votre choix).

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. Pour actualiser l’application afin de voir votre nouveau titre, interrompez votre application en appuyant sur **Ctrl + C**, **Y** dans l’invite de commandes, puis redémarrez-la avec `npm start`.

### <a name="2-add-client-side-game-logic-code"></a>2. Ajoutez le code de la logique de jeu côté client
Vous pouvez trouver les fichiers dont vous avez besoin pour cette partie du didacticiel dans le dossier [Démarrer](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start). Si vous êtes perdu, le code finalisé est disponible dans le dossier [Final](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final). 

1. Copiez le fichier scripts.js dans le dossier [Démarrer](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) et collez-le dans memory\public\javascripts. Ce fichier contient toutes les logiques de jeu nécessaires pour exécuter le jeu, notamment:

    * Création/lancement d'un nouveau jeu.
    * Restauration d’un jeu stocké sur le serveur.
    * Définition du tableau et des cartes du jeu selon la sélection de l'utilisateur.
    * Retournement des cartes.

2. Dans script.js, commençons par modifier la fonction `newGame()`. Cette fonction:

    * Gère la taille de la sélection de jeu à partir de l’utilisateur.
    * Extrait le [tableau](#part-i-build-a-rest-api-backend) à partir du serveur.
    * Appelle la fonction `drawGameBoard()` pour placer le tableau du jeu à l’écran.

    Ajoute le code suivant dabs `newGame()`après le commentaire `// Add code from Part 2.2 here`.

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    Ce code récupère la valeur à partir du menu déroulant avec `id="selectGameSize"` (que nous créerons ultérieurement) et le stocke dans une variable (`size`).  Nous utilisons la fonction [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) pour analyser la valeur de chaîne dans la liste déroulante afin de retourner un entier, et donc de pouvoir transmettre la `size` du tableau du jeu demandée au serveur. 

    Nous utilisons la méthode [`/new`](#part-i-build-a-rest-api-backend) créée dans la partie I de ce didacticiel pour publier la taille du tableau sélectionnée sur le serveur. Cette méthode renvoie un jeu de cartes JSON unique et des valeurs `true/false` indiquant si les cartes ont été mises en correspondance. 

3. Ensuite, renseignez la fonction `restoreGame()` qui restaure le dernier jeu joué. Par souci de simplicité, l’application charge toujours le dernier jeu joué. Si aucun jeu n'est stocké sur le serveur, utilisez le menu déroulant pour démarrer un nouveau jeu. 

    Copiez et collez ce code dans `restoreGame()`.

   ``` javascript 
   // reset the game
   gameBoardSize = 0;
   cardsFlipped = 0;

   // fetch the game state from the server 
   $.get("http://localhost:8000/game", function (response) {
       // store game board size
       gameBoardSize = response.length;

       // draw the game board
       drawGameBoard(response);
   });
   ```

    Le jeu va à présent extraire son état à partir du serveur. Pour plus d’informations sur la méthode [`/game`](#part-i-build-a-rest-api-backend) utilisée dans cette étape, reportez-vous à la partie I de ce didacticiel. Si vous utilisez Azure (ou un autre service) pour héberger le système API principal, remplacez l’adresse *localhost* ci-dessus par votre URL de production.

4. Maintenant, nous allons créer la fonction `drawGameBoard()`.  Cette fonction:

    * Détecte la taille du jeu et applique des styles CSS spécifiques.
    * Génère les cartes en HTML.
    * Ajoute les cartes à la page.

    Copiez et collez ce code dans la fonction `drawGameBoard()`de *scripts.js *:

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. Ensuite, nous devons lancer la fonction `flipCard()`.  Cette fonction prend en charge la majeure partie de la logique de jeu, y compris la récupération des valeurs des cartes sélectionnées à partir du serveur à l’aide de la méthode [`/guess`](#part-i-build-a-rest-api-backend) développée dans la partie I de ce didacticiel. N’oubliez pas de remplacer l'adresse *localhost* par votre URL de production si vous hébergez le serveur principal de l’API REST dans le cloud.

    Dans la fonction `flipCard()`, supprimez ce code:

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> Si vous utilisez Visual Studio Code, sélectionnez toutes les lignes de code pour lesquelles vous souhaitez supprimer les commentaires, et appuyez sur Ctrl + K, U

Nous utiliserons ici [`jQuery.ajax()`](http://api.jquery.com/jQuery.ajax/) et la méthode **PLACER**[`/guess`](#part-i-build-a-rest-api-backend) créée dans la partie I. 

Ce code s’exécute dans l’ordre suivant.

* L'`id`de la première carte sélectionnée par l’utilisateur est ajouté en tant que première valeur du tableau [] selectedCards: `selectedCards[0]` 
* La valeur (`id`) dans `selectedCards[0]`est validée sur le serveur à l’aide de la méthode [`/guess`](#part-i-build-a-rest-api-backend)
* Le serveur répond avec la `value`de cette carte (un entier)
* Un [glyphicon d’amorçage](http://getbootstrap.com/components/) est ajouté à l’arrière de la carte dont `id`est `selectedCards[0]`
* La`value` de la première carte (à partir du serveur) est stockée dans le `selectedCardsValues[]`tableau:`selectedCardsValues[0]`. 

La seconde devinette de l’utilisateur suit la même logique. Si les cartes sélectionnées par l’utilisateur ont les mêmes ID (par exemple, `selectedCards[0] == selectedCards[1]`), elles correspondent! La classe CSS `.matched`est ajoutée aux cartes correspondantes (qui deviennent vertes) et les cartes restent retournées.

Maintenant, nous devons ajouter la logique pour vérifier que l’utilisateur a fait correspondre toutes les cartes et a gagné le jeu. Dans la fonction `flipCard()`, ajoutez les lignes de code suivantes sous le commentaire `//check if the user won the game`. 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

Si le nombre de cartes retournées correspond à la taille du tableau de jeu (par exemple, `cardsFlipped == gameBoardSize`), il n’y a plus aucune carte à retourner et l’utilisateur a gagné. Nous allons ajouter du code HTML simple au `div`avec `id="game-board"`pour informer l’utilisateur qu’il a gagné et qu'il peut jouer à nouveau.  

### <a name="3-create-the-user-interface"></a>3. Création de l’interface utilisateur 
Nous allons désormais observer ce code en action en créant l’interface utilisateur. Dans ce didacticiel, nous utilisons le moteur de création de modèles [Pug](https://pugjs.org/) (officiellement Jade).  *Pug* est la syntaxe propre et sensible aux espaces blancs pour l’écriture de code HTML. Voici un exemple. 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

devient

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. Remplacez le fichier layout.pug dans memory\views avec le fichier layout.pug fourni dans le dossier de démarrage. Dans le fichier layout.pug, vous verrez des liens vers:

    * Amorçage
    * jQuery
    * Un fichier CSS personnalisé
    * Le fichier JavaScript que nous venons de modifier

2. Ouvrez le fichier nommé index.pug dans le répertoire memory\views.
Ce fichier étend le fichier layout.pug et donnera le rendu de notre jeu. Dans le fichier layout.pug, collez les lignes de code suivants:

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> N’oubliez pas: Pug est sensible aux espaces blancs. Assurez-vous que toutes vos mises en retrait sont correctes!

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. Utilisation du système de grille de Bootstrap pour créer une disposition réactive
Le [système de grille](http://getbootstrap.com/css/#grid) de Bootstrap est un système de grille fluide qui s’adapte à une grille au fil des modifications de la fenêtre d’affichage d’un appareil. Les cartes dans ce jeu utilisent des classes de système de grille Bootstrap prédéfinies pour la disposition, notamment:
* `.container-fluid`: spécifie le conteneur fluide pour la grille
* `.row-fluid`: spécifie les lignes fluides
* `.col-xs-3`: spécifie le nombre de colonnes

Le système de grille Bootstrap offre un système de grille qui se réduit en une seule colonne verticale, semblable à celles que vous verriez dans le menu de navigation d'un appareil mobile.  Toutefois, compte tenu du fait que nous souhaitons que notre jeu dispose toujours de colonnes, nous utilisons la classe prédéfinie `.col-xs-3`, qui maintient la grille à l’horizontale à tout moment. 

Le système de grille permet de créer jusqu’à 12colonnes. Dans la mesure où nous voulons uniquement 4colonnes dans le jeu, nous utilisons la classe `.col-xs-3`. Cette classe spécifie que nous avons besoin de chacune de nos colonnes pour étendre la largeur des 3colonnes disponibles sur les 12mentionnées plus haut. Cette image montre une grille de 12colonnes et une grille de 4colonnes, comme celle utilisée dans ce jeu.

![Grille Bootstrap avec 12colonnes et 4colonnes](./images/grid.png)

1. Ouvrez scripts.js et recherchez la fonction `drawGameBoard()`.  Dans le bloc de code où nous générons le code HTML pour chaque carte, vous pouvez repérer l'élément `div` avec `class="col-xs-3"`? 

2. À l’intérieur d'index.pug, nous allons ajouter les classes Bootstrap prédéfinies que nous avons mentionnées précédemment pour créer notre disposition fluide.  Modifiez index.pug comme suit.

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. Ajouter une animation de retournement de carte avec des transformations CSS
Remplacez le fichier style.css dans memory\public\stylesheets par le fichier style.css à partir du dossier de démarrage.

L'ajout d’un mouvement de retournement à l’aide de la [transformation CSS](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms) confère aux cartes un mouvement de retournement réaliste, en 3D. Les cartes du jeu sont créées à l’aide de la structure de code HTML suivante et ajoutées de manière programmée au tableau du jeu (dans la fonction `drawGameBoard()` illustrée précédemment).

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. Tout d'abord, donnez de la [perspective](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) au conteneur parent de l’animation (`.flipContainer`).  Cela donne une illusion de profondeur pour ses éléments enfants: plus la valeur est élevée, plus l'élément apparaîtra éloigné de l'utilisateur. Ajoutons la perspective suivante à la classe `.flipContainer` dans style.css.

    ``` css
    perspective: 1000px; 
    ```

2. Maintenant, ajoutons les propriétés suivantes à la classe `.cards` dans style.css. Le `.cards``div`est l’élément qui effectuera l’animation de retournement, en montrant l’avant ou l’arrière de la carte. 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    La propriété [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) établit un contexte de rendu en 3D et les enfants de la catégorie `.cards` (`.front` et `.back`) font partie de l’espace 3D. L'ajout de la propriété [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) spécifie le nombre de secondes jusqu'à la fin de l'animation. 

3.  À l’aide de la propriété [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform), nous pouvons faire pivoter la carte autour de l’axe Y.  Ajoutez le CSS suivant à `cards.flip`.

    ``` css
    transform: rotateY(180deg);
    ```

    Le style défini dans `cards.flip`est activé et désactivé dans la fonction `flipCard` à l’aide de [`.toggleClass()`](http://api.jquery.com/toggleClass/). 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    Désormais, lorsqu’un utilisateur clique sur une carte, celle-ci pivote à 180degrés.

### <a name="6-test-and-play"></a>6. Tester et jouer
Félicitations! Vous avez terminé de créer l’application Web! Testons-la. 

1. Ouvrez une invite de commandes dans le répertoire de votre mémoire et entrez la commande suivante: `npm start`

2. Dans votre navigateur, accédez à [http://localhost:3000/](http://localhost:3000/) et jouez!

3. Si vous rencontrez des erreurs, vous pouvez utiliser les outils de débogage Node.js de Visual Studio Code en appuyant sur la touche F5 de votre clavier et en saisissant `Node.js`. Pour plus d’informations sur le débogage dans Visual Studio Code, consultez cet [article ](http://code.visualstudio.com/docs/editor/debugging#_launch-configurations). 

    Vous pouvez également comparer votre code à celui fourni dans le dossier Final.

4. Pour arrêter le jeu, saisissez: **Ctrl + C**, **Y ** dans l’invite de commande. 

### <a name="going-further"></a>Aller plus loin

Vous pouvez désormais déployer votre application Azure (ou tout autre service d’hébergement dans le cloud) en vue de tests sur divers facteurs de forme d'appareils, tels que des appareils mobiles, tablettes et bureau. (N’oubliez pas de tester plusieurs navigateurs également!) Une fois que votre application est prête pour la production, vous pouvez l’empaqueter facilement en tant qu’*application Web hébergée* (HWA) pour la *plateforme Windows universelle* (UWP) et la distribuer depuis le Microsoft Store.

La procédure de base pour la publication dans le Microsoft Store est la suivante:

 1. Créer un compte [Développeur Windows](https://developer.microsoft.com/en-us/store/register)
 2. Utiliser la [liste de vérification](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions) de soumission d'applications
 3. Soumettre votre application en vue d'une [certification](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)

Voici quelques ressources utiles pour aller plus loin:

 - [Déployer votre projet de développement d’applications pour les sites Web Azure](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [Convertir votre applicationweb en une application UWP (plateforme Windows universelle)](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [Publier des applications Windows](https://developer.microsoft.com/en-us/store/publish-apps)
