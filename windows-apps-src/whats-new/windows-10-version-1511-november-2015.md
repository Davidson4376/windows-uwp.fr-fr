---
La version 1511 de Windows 10 et les mises à jour des outils de développeurs continuent à fournir les fonctionnalités, expériences et outils pris en charge par la plateforme Windows universelle.
Nouveautés pour les développeurs dans Windows 10, version 1511 - Novembre 2015
---

# Nouveautés pour les développeurs dans Windows 10, version 1511 - Novembre 2015

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

La version 1511 de Windows 10 et les mises à jour des outils de développeurs continuent à fournir les fonctionnalités, expériences et outils pris en charge par la plateforme Windows universelle. Après avoir [installé les outils et le Kit de développement logiciel](https://dev.windows.com/downloads) sur la version 1511 de Windows 10, vous êtes prêt à [créer une application Windows universelle](https://msdn.microsoft.com/library/windows/apps/bg124288) ou à découvrir comment utiliser votre [code d’application existant sur Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

## Expérience utilisateur

Les nouvelles classes <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpList</a> et <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpListItem</a> fournissent des applications permettant de sélectionner par programmation le type de liste de raccourcis gérée par le système souhaité, et d’ajouter des points d’entrée de tâche et des groupes personnalisés à leur liste de raccourcis.

## Entrée
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx">Intercepteur de saisie de clavier</a>
                                        
    Permet à une application de passer outre le traitement système des entrées brutes de clavier, notamment des touches de raccourcis, des touches d’accès et des touches d’application, à l’exception des combinaisons de clés appelées séquences de touches sécurisée (SAS).

    Ces séquences, dont font partie « Ctrl-Alt-Suppr » et « Windows + L », continuent à être traitées par le système.
                                        
* Chaînage interprocessus des entrées pointeurs pour les <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx">applications UWP</a> et les <a href="https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx">applications Windows classiques</a>.
                                        
    Nouveaux événements de pointeur prenant en charge le chaînage interprocessus des entrées.    
                                        
* <a href="https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx">Présentateur d’entrée manuscrite pour les applications de bureau classiques</a>
                                        
    Grâce aux API de présentateur d’entrée manuscrite, les applications Microsoft Win32 sont en mesure de gérer l’entrée, le traitement et le rendu des entrées manuscrites (standard ou modifiées) via un objet <a href="https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx">InkPresenter</a> inséré dans l’arborescence visuelle <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx">DirectComposition</a>.    
                                    
## Mise en réseau
                                                                        
Pour les utilisateurs WebSockets : <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">MessageWebSocket.OutputStream.FlushAsync</a> et <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">StreamWebSocket.OutputStream.FlushAsync</a> ont été entièrement implémentés, et sont en attente de l’exécution des appels WriteAsync précédemment émis. Notez que cette configuration peut entraîner la levée d’une exception par le code existant, si l’objet WebSocket présente un état non valide lorsque vous appelez <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">FlushAsync</a>.    

Une nouvelle propriété, <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">CookieUsageBehavior</a>, a été ajoutée à la classe existante <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">Windows.Web.Http.Filters.HttpBaseProtocolFilter</a>. Ainsi, les développeurs contrôlent le traitement par le système des cookies.    
                                    
## ORTC
                                    
Microsoft Edge implémente désormais <a href="https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx">ORTC (Object Real-Time Communications)</a>, qui permet les appels multimédias (audio et/ou vidéo) en temps réel directement entre les navigateurs web, les appareils mobiles et les serveurs via des API Javascript natives. Les développeurs sont désormais en mesure de développer des applications avancées de communication audio/vidéo en temps réel sur le navigateur Microsoft Edge à l’aide de l’API ORTC, avec une prise en charge des appels vidéo groupés, de la diffusion simultanée, de la norme SVC (Scalable Video Coding), et plus encore.    

Pour obtenir une démonstration d’un appel audio/vidéo 1:1 via l’API ORTC entre les navigateurs Microsoft Edge, consultez la page <a href="/microsoft-edge/testdrive/demos/ortcdemo/">Sites et démos Test Drive</a>. Pour consulter une vue d’ensemble et une procédure pas à pas des exemples de code, consultez l’entrée <a href="https://msdn.microsoft.com/library/mt588497(v=vs.85).aspx">ORTC du Guide du développeur</a>.
                                        
## Microsoft Edge - Outils de développement F12
                                                                        
Microsoft Edge introduit de formidables améliorations aux outils de développement F12, relatives notamment à certaines des fonctionnalités les plus demandées de <a href="https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer">UserVoice</a>. Découvrez les nouvelles fonctionnalités offertes par l’Explorateur DOM, la console, le débogueur, le réseau, les performances, la mémoire, l’émulation ainsi qu’un nouvel outil Expériences, grâce auquel vous pouvez essayer de nouvelles fonctionnalités puissantes avant la fin de leur développement. Les nouveaux outils, développés dans TypeScript, sont toujours exécutés. Dès lors, aucune recharge n’est requise. En outre, la documentation sur les outils de développement F12 fait désormais partie du <a href="http://dev.modern.ie/">site de développement Microsoft Edge</a> ; elle est intégralement disponible sur <a href="https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation">GitHub</a>. À partir de ce stade, les documents ne sont pas affectés uniquement par vos commentaires, mais nous vous invitons néanmoins à nous communiquer votre opinion et à contribuer à l’établissement de notre documentation. Pour consulter une brève vidéo sur les outils de développement F12, consultez la page <a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools">One Dev Minute de Channel 9</a>.    
                                    
## Windows Hello
                                    
Grâce à Windows Hello, votre application peut prendre en charge la reconnaissance faciale ou l’identification par empreinte digitale pour la connexion sur un système ou un appareil Windows.

Les API des fournisseurs donnent les moyens aux fabricants de matériel et aux fabricants OEM d’exposer des caméras de profondeur, infrarouges et couleur (et les métadonnées associées) prenant en charge la vision de l’ordinateur au sein de la plateforme Windows universelle, et de désigner une caméra en tant que participante de l’authentification faciale Windows Hello. L’espace de noms <a href="http://go.microsoft.com/fwlink/?LinkId=691697">Windows.Devices.Perception</a> contient les API client qui permettent à une application UWP d’accéder aux données de couleurs, de profondeur et infrarouges des caméras de l’ordinateur.
                                    
## Nouvelles API de jeux

Utilisez la nouvelle classe Windows.Gaming.UI.GameBar afin de recevoir les notifications lorsque la barre de jeux est affichée ou masquée.    
                            
                                    
## API Bluetooth
                                    
Plusieurs API ont été ajoutées et mises à jour afin d’étendre la prise en charge de Bluetooth LE, de l’énumération de périphériques et d’autres fonctionnalités Bluetooth. Consultez l’espace de noms <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx">Windows.Devices.Bluetooth</a>.    
                                   
## API de cartes à puce ## 

Plusieurs API SmartCardCryptogram APIs ont été ajoutées à l’espace de noms <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx">Windows.Devices.SmartCards</a> afin de prendre en charge des protocoles de paiement sécurisés par cryptogramme. Les applications de paiement utilisant l’émulation de carte hôte pour la prise en charge de la fonctionnalité de paiement au toucher peuvent valoriser ces API, pour une sécurité et des performances supérieures. Les applications peuvent créer une clé et protéger les clés de transaction à usage limité à l'aide du module de plateforme sécurisée. Les applications peuvent également valoriser la structure NGC (Next Generation Credentials) afin de protéger les clés avec le code PIN de l’utilisateur. Pour des performances supérieures, ces API délèguent la génération des cryptogrammes au système. Cela prévient par ailleurs tout accès d’autres applications aux clés et aux cryptogrammes.    
                                    
## API de stockage mises à jour ## 
    
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx">Classe Windows.Storage.DownloadsFolder</a><br />
Votre application peut désormais <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx">créer un fichier</a> ou <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx">créer un dossier</a> dans le dossier Téléchargements d’un <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Utilisateur</a> spécifique.
                                            
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx">Classe Windows.Storage.StorageLibrary</a><br />
Votre application peut désormais <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx">récupérer une bibliothèque définie</a> pour un <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Utilisateur</a> spécifique.
                                    
## Kit de certification des applications Windows ## 
                                    
Le Kit de certification des applications Windows a été mis à jour avec des tests améliorés. Pour obtenir la liste complète des mises à jour, consultez la page <a href="/develop/app-certification-kit">Kit de certification des applications Windows</a>.    
                                    
## Téléchargement de conceptions ## 

Découvrez nos nouveaux modèles de conception d’applications UWP pour Adobe Photoshop. Nous avons également mis à jour nos modèles Microsoft PowerPoint et Adobe Illustrator et rendu disponible une version PDF de nos recommandations. <a href="/design/assets">Consultez la page Téléchargements de conceptions</a>.    




<!--HONumber=Mar16_HO5-->


