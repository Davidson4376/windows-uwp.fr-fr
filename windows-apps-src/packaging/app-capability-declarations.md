---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: Déclarations des fonctionnalités d’application
description: Pour pouvoir accéder à certaines API, ressources (images ou musique) ou appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le manifeste du package de votre application de plateforme Windows universelle (UWP).
---
# Déclarations des fonctionnalités d’application

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Pour pouvoir accéder à certaines API, ressources (images ou musique) ou appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le [manifeste du package](https://msdn.microsoft.com/library/windows/apps/BR211474) de votre application de plateforme Windows universelle (UWP).

Vous demandez l’accès à des ressources ou API spécifiques en déclarant des fonctionnalités dans le [manifeste du package](https://msdn.microsoft.com/library/windows/apps/BR211474) de votre application. Vous pouvez déclarer les fonctionnalités générales à l’aide du [concepteur de manifeste](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx) dans Microsoft Visual Studio ou vous pouvez les ajouter manuellement. Pour plus d’informations, voir [Comment spécifier des fonctionnalités dans un manifeste de package](https://msdn.microsoft.com/library/windows/apps/BR211477). Il est important de savoir que lorsque des clients acquièrent votre application dans le Windows Store, toutes les fonctionnalités déclarées par l’application leur sont notifiées. Évitez de déclarer des fonctionnalités dont votre application n’a pas besoin.

Certaines fonctionnalités permettent aux applications d’accéder à des *ressources sensibles*. Ces ressources sont considérées comme « sensibles » parce qu’elles ont accès aux données personnelles de l’utilisateur ou ont un coût pour celui-ci. Les paramètres de confidentialité, gérés par l’application Paramètres, permettent à l’utilisateur de contrôler de façon dynamique l’accès aux ressources sensibles. Il est donc important que votre application ne présume pas de la disponibilité d’une ressource sensible. Pour plus d’informations sur l’accès aux ressources sensibles, voir [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/Hh768223). Les fonctionnalités qui fournissent des applications avec un accès à une *ressource* sensible sont marquées d’un astérisque (*) en regard du scénario de fonctionnalité.

Cet article passe en revue les quatre catégories de fonctionnalités décrites ci-dessous.

-   Les fonctionnalités à usage général qui s’appliquent aux scénarios d’application les plus courants.

-   Les fonctionnalités de périphérique qui permettent à votre application d’accéder à des périphériques et à des dispositifs internes.

-   Les fonctionnalités à usage spécial qui nécessitent un compte d’entreprise spécial pour les soumettre au Windows Store et les utiliser. Pour plus d’informations sur les comptes d’entreprise, voir [Types de comptes, emplacements et frais](https://msdn.microsoft.com/library/windows/apps/JJ863494).

-   Les fonctionnalités restreintes qui sont uniquement disponibles pour Microsoft et ses partenaires.

## Fonctionnalités à usage général

Les fonctionnalités à usage général s’appliquent aux scénarios d’application les plus courants.

<table>
        <thead>
            <tr>
                <th>Scénario de fonctionnalité</th>
                <th>Utilisation de la fonctionnalité</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**Musique***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/Capacités&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Images***</td>
                <td>
                    La fonctionnalité **picturesLibrary** fournit un accès par programme aux images de l’utilisateur, permettant à l’application d’énumérer tous les fichiers dans la bibliothèque et d’y accéder sans interaction de l’utilisateur. Cette fonctionnalité est généralement utilisée dans les applications de photos qui utilisent la bibliothèque d’images entière.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Vidéos***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Stockage amovible**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Internet et réseaux publics***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Contacts***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Génération de code**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**Dossier d’appels enregistrés***</td>
                <td>
                    <p>La fonctionnalité d’appareil **recordedCallsFolder** permet aux applications d’accéder au dossier d’appels enregistrés.</p>
                    <p>La fonctionnalité **recordedCallsFolder** doit inclure l’espace de noms **mobile** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Informations sur le compte d’utilisateur***</td>
                <td>
                    <p>La fonctionnalité **userAccountInformation** offre aux applications la possibilité d’accéder au nom et à l’image de l’utilisateur.</p>
                    <p>Cette fonctionnalité est nécessaire pour accéder à certaines API de l’espace de noms Windows.System.User.</p>
                    <p>La fonctionnalité **userAccountInformation** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Appel VOIP**</td>
                <td>
                    <p>La fonctionnalité **voipCall** permet aux applications d’accéder aux API d’appel VoIP de l’espace de noms [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266).</p>
                    <p>La fonctionnalité **voipCall** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Objets 3D**</td>
                <td>
                    <p>La fonctionnalité **objects3d** dote les applications d’un accès par programme aux fichiers objets 3D. Cette fonctionnalité est généralement utilisée dans les applications et les jeux 3D qui ont besoin d’accéder à l’intégralité de la bibliothèque d’objets 3D.</p>
                    <p>Cette fonctionnalité est nécessaire pour accéder au dossier contenant les objets 3D à l’aide des API de l’espace de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346).</p>
                    <p>La fonctionnalité **objects3D** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Lecture des messages bloqués***</td>
                <td>
                    <p>La fonctionnalité **blockedChatMessages** permet aux applications de lire les messages SMS et MMS bloqués par l’application de filtre de courrier indésirable.</p>
                    <p>Cette fonctionnalité est nécessaire pour accéder aux messages bloqués à l’aide des API de l’espace de noms [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p>
                    <p>La fonctionnalité **blockedChatMessages** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Accès aux messages de conversation**</td>
                <td>
                    <p>La fonctionnalité **chat** permet aux applications de lire et de supprimer des SMS. Cette fonctionnalité permet également aux applications de stocker des messages de conversation dans le magasin de données système.</p>
                    <p>Cette fonctionnalité est nécessaire pour utiliser certaines API de l’espace de noms [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p>
                    <p>La fonctionnalité **chat** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Matériel de bus de bas niveau IoT**</td>
                <td>
                    <p>La fonctionnalité **lowLevelDevices** permet aux applications qui s’exécutent sur des appareils IoT d’accéder au matériel de bus de bas niveau tel que GPIO, I2C, SPI, ADC et PWM.</p>
                    <p>Cette fonctionnalité est nécessaire pour accéder à certaines API de l’espace de noms [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178).</p>
                    <p>La fonctionnalité **lowLevelDevices** doit inclure l’espace de noms **iot** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Administration système IoT**</td>
                <td>
                    <p>La fonctionnalité **systemManagement** permet aux applications de disposer de privilèges administratifs système de base tels que l’arrêt ou le redémarrage, les paramètres régionaux et le fuseau horaire.</p>
                    <p>Cette fonctionnalité est requise pour accéder à certaines API de l’espace de noms [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814).</p>
                    <p>La fonctionnalité **systemManagement** doit inclure l’espace de noms **iot** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## Fonctionnalités de l’appareil

Les fonctionnalités d’appareil permettent à votre application d’accéder à des appareils et à des dispositifs internes. Les fonctionnalités d’appareil sont spécifiées avec l’élément **DeviceCapability** dans le manifeste du package de votre application. Cet élément peut exiger des éléments enfants supplémentaires et certaines fonctionnalités d’appareil doivent être ajoutées manuellement au manifeste du package. Pour plus d’informations, voir [Comment spécifier des fonctionnalités de périphérique dans un manifeste de package](https://msdn.microsoft.com/library/windows/apps/Dn263092) et [**DeviceCapability Schema reference**](https://msdn.microsoft.com/library/windows/apps/BR211430).

| Scénario de fonctionnalité | Utilisation de la fonctionnalité |
|---------------------|------------------|
| **Localisation**\* | La fonctionnalité **location** donne accès aux fonctionnalités de géolocalisation obtenues à partir d’un matériel dédié, tel qu’un capteur GPS dans l’ordinateur, ou dérivées des informations réseau disponibles. Les applications doivent gérer le cas où l’utilisateur a désactivé les services de localisation à partir de l’icône **Paramètres**. |
| **Microphone** | La fonctionnalité **microphone** donne accès au flux audio du microphone, ce qui permet à l’application d’effectuer un enregistrement audio à partir de microphones connectés. Les applications doivent gérer le cas où l’utilisateur a désactivé le microphone à partir de l’icône **Paramètres**. |
| **Proximité** | La fonctionnalité **proximity** permet à plusieurs appareils très proches les uns des autres de communiquer entre eux. Cette fonctionnalité est généralement utilisée dans des jeux multijoueurs simples et dans des applications qui échangent des informations. Les périphériques tentent d’utiliser la technologie de communication qui offre la meilleure connexion possible, notamment Bluetooth, le WiFi et Internet. Cette fonctionnalité est utilisée uniquement pour initier la communication entre les appareils. |
| **Webcam** | La fonctionnalité **webcam** fournit un accès au flux vidéo d’une caméra intégrée ou d’une webcam externe, ce qui permet à l’application de capturer des photos et des vidéos. Sous Windows, les applications doivent gérer le cas où l’utilisateur a désactivé la caméra à partir de l’icône **Paramètres**.<br/>La fonctionnalité **webcam** ne donne accès qu’au flux vidéo. Pour autoriser l’accès au flux audio, la fonctionnalité **microphone** doit être ajoutée. |
| **USB** | La fonctionnalité **usb** permet d’accéder aux API dans [Mise à jour du package du manifeste de l’application pour un périphérique USB](http://go.microsoft.com/fwlink/p/?LinkId=302259). |
| **Périphérique d’interface utilisateur (HID)** | La fonctionnalité de périphérique **humaninterfacedevice** prend en charge l’accès aux API dans [Comment spécifier des fonctionnalités de périphérique pour un périphérique d’interface utilisateur](https://msdn.microsoft.com/library/windows/apps/Dn263091). |
| **Point de vente (PDV)** | La fonctionnalité d’appareil **pointOfService** permet d’accéder aux API de l’espace de noms [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071). Cet espace de noms permet à votre application d’accéder aux scanneurs de codes-barres et aux lecteurs de bande magnétique de points de vente (PDV). L’espace de noms offre une interface indépendante du fournisseur qui permet d’accéder aux appareils PDV de différents constructeurs à partir d’une application du Windows Store. |
| **Bluetooth** | La fonctionnalité d’appareil **bluetooth** permet aux applications de communiquer avec des appareils Bluetooth déjà jumelés par le biais du protocole d’attribut générique (GATT) ou de taux de base classique (RFCOMM).<br/>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413). |
| **Réseau Wi-Fi** | La fonctionnalité d’appareil **wiFiControl** permet aux applications de rechercher les réseaux Wi-Fi disponibles et de s’y connecter.<br/>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224). |
| **État radio** | La fonctionnalité d’appareil **radios** permet aux applications d’activer et de désactiver les radios Wi-Fi et Bluetooth.<br/>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447).  |
| **Disque optique** | La fonctionnalité d’appareil **optical** permet aux applications d’accéder aux fonctions des lecteurs de disque optiques (CD, DVD, Blu-ray, etc.).<br/>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667). |
| **Mouvements** | La fonctionnalité d’appareil **activity** permet aux applications de détecter le mouvement actuel de l’appareil.<br/>Cette fonctionnalité est nécessaire pour utiliser certaines API de l’espace de noms [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408). |

## Fonctionnalités spéciales et restreintes

**Important**  
Les fonctionnalités spéciales et restreintes sont destinées à des scénarios très spécifiques. L’utilisation de ces fonctionnalités est extrêmement limitée et sujette à un examen et une stratégie d’accueil Windows Store supplémentaires.

Dans certains cas, de telles fonctionnalités sont nécessaires et appropriées, telles que des applications bancaires avec une authentification à deux facteurs, dans lesquelles les utilisateurs fournissent une carte à puce dotée d’un certificat numérique qui confirme leur identité. D’autres applications peuvent être conçues principalement pour des clients professionnels et peuvent avoir besoin d’accéder à des ressources d’entreprise auxquelles il n’est pas possible d’accéder sans les informations d’identification de domaine de l’utilisateur.

Les applications qui appliquent les fonctionnalités d’utilisation spécifiques requièrent un compte d’entreprise pour pouvoir être envoyées au Windows Store. En revanche, les fonctionnalités restreintes ne nécessitent pas de compte d’entreprise spécial pour le Windows Store, car elles ne sont pas à la disposition des développeurs. Les fonctionnalités restreintes sont disponibles uniquement pour les applications qui sont développées par Microsoft et ses partenaires. Pour plus d’informations sur les comptes d’entreprise, voir [Types de comptes, emplacements et frais](https://msdn.microsoft.com/library/windows/apps/JJ863494).

À la différence d’autres fonctionnalités, toutes les fonctionnalités restreintes doivent inclure l’espace de noms **rescap** lorsque vous les déclarez dans le manifeste du package de votre application. L’exemple suivant vous montre comment déclarer la fonctionnalité **appCaptureSettings**.

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

Vous devez également ajouter la déclaration d’espace de noms **xmlns:rescap** en haut du fichier Package.appxmanifest, comme illustré ci-dessous.

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Scénario de fonctionnalité</th>
<th align="left">Utilisation de la fonctionnalité</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Entreprise**</td>
<td align="left"><p>Les informations d’identification de domaine Windows permettent à un utilisateur de se connecter à des ressources distantes à l’aide de ses informations d’identification, comme si l’utilisateur avait fourni son propre nom d’utilisateur et mot de passe. La fonctionnalité spéciale **enterpriseAuthentication** est généralement utilisée dans des applications métiers qui se connectent à des serveurs au sein d’une entreprise.</p>
<p>Vous n’avez pas besoin de cette capacité pour une communication générique sur Internet.</p>

<p>La fonctionnalité spéciale **enterpriseAuthentication** a été conçue pour prendre en charge des applications métiers courantes. Ne la déclarez pas dans des applications qui n’ont pas besoin d’accéder à des ressources d’entreprise. Le [**sélecteur de fichiers**](https://msdn.microsoft.com/library/windows/apps/BR207847) fournit un mécanisme d’interface utilisateur robuste qui permet aux utilisateurs d’ouvrir les fichiers sur un partage réseau à utiliser avec une application. Déclarez la fonctionnalité spéciale **enterpriseAuthentication** uniquement quand votre application se trouve dans une situation nécessitant un accès par programme qui ne peut pas être réalisé à l’aide du **sélecteur de fichiers**.</p>
<p>La fonctionnalité **enterpriseAuthentication** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>La fonctionnalité **enterpriseDataPolicy** permet aux applications de définir et d’utiliser des stratégies d’entreprise pour l’appareil. Elle est requise pour l’utilisation de tous les membres des classes suivantes.</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**Certificats utilisateur partagés**</td>
<td align="left"><p>La fonctionnalité spéciale **sharedUserCertificates** permet à une application d’ajouter des certificats de logiciel et de matériel (par exemple, des certificats stockés sur une carte à puce) dans le magasin de certificats utilisateur partagés et d’accéder à ces derniers. Cette fonctionnalité est généralement utilisée pour des applications financières ou d’entreprise qui ont besoin d’une carte à puce pour l’authentification.</p>
<p>La fonctionnalité **sharedUserCertificates** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**Documents***</td>
<td align="left"><p>La fonctionnalité spéciale **documentsLibrary** fournit un accès par programme aux documents de l’utilisateur, filtrés en fonction des associations de types de fichiers déclarées dans le manifeste du package, pour prendre en charge l’accès hors connexion à OneDrive. Par exemple, si une application de type lecteur DOC a déclaré une association de type de fichier .doc, elle peut ouvrir les fichiers .doc dans les documents, mais pas d’autres types de fichiers.</p>
<p>Les applications qui déclarent la fonctionnalité spéciale **documentsLibrary** ne peuvent pas accéder aux documents sur les ordinateurs d’un groupe résidentiel. Le [file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174) fournit un mécanisme d’interface utilisateur robuste qui permet aux utilisateurs d’ouvrir les fichiers à utiliser avec une application. Déclarez la fonctionnalité spéciale **documentsLibrary** uniquement quand vous ne pouvez pas utiliser le sélecteur de fichiers.</p>
<p>Pour pouvoir utiliser la fonctionnalité spéciale **documentsLibrary**, une application doit :</p>
<ul>
<li>faciliter l’accès hors connexion interplateforme à du contenu OneDrive spécifique à l’aide d’URL ou d’ID de ressource OneDrive valides ;</li>
<li>enregistrer automatiquement les fichiers ouverts sur le OneDrive de l’utilisateur en mode hors connexion.</li>
</ul>
<p>Les applications qui utilisent la fonctionnalité spéciale **documentsLibrary** dans ces deux objectifs ont également la possibilité de l’utiliser pour ouvrir du contenu incorporé dans un autre document. Seules les utilisations ci-dessus de la fonctionnalité spéciale **documentsLibrary** sont acceptées.</p>
<ul>
<li><p>Votre application ne peut pas accéder à la bibliothèque Documents sur le support de stockage interne du téléphone. Toutefois, si une autre application crée un dossier Documents sur la carte SD optionnelle, votre application peut accéder à ce dossier.</p></li>
</ul>
<p>La fonctionnalité **documentsLibrary** doit inclure l’espace de noms **uap** lorsque vous la déclarez dans le manifeste du package de votre application, comme illustré ci-dessous.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**Paramètres de jeux DVR**</td>
<td align="left"><p>La fonctionnalité restreinte **appCaptureSettings** permet aux applications de contrôler les paramètres utilisateur pour la fonction Jeux DVR.</p>
<p>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738).</p></td>
</tr>
<tr class="odd">
<td align="left">**Cellulaire**</td>
<td align="left"><p>La fonctionnalité restreinte **cellularDeviceControl** permet aux applications de contrôler l’appareil cellulaire.</p>
<p>La fonctionnalité **cellularDeviceIdentity** permet aux applications d’accéder aux informations d’identification cellulaires.</p>
<p>La fonctionnalité **cellularMessaging** permet aux applications d’utiliser les SMS et le RCS.</p>
<p>Ces fonctionnalités sont nécessaires pour utiliser certaines API des espaces de noms [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567).</p>
<p>À compter de Windows 10, les applications appelant [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996)).</p></td>
</tr>
<tr class="even">
<td align="left">**Déverrouillage d’appareil**</td>
<td align="left"><p>La fonctionnalité restreinte **deviceUnlock** permet aux applications de déverrouiller un appareil pour les scénarios de chargement indépendant de développeur et d’entreprise.</p></td>
</tr>
<tr class="odd">
<td align="left">**Vignettes double SIM**</td>
<td align="left"><p>La fonctionnalité restreinte **dualSimTiles** permet aux applications de créer une entrée supplémentaire dans la liste des applications sur les appareils dotés de plusieurs cartes SIM.</p>
<p>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235).</p></td>
</tr>
<tr class="even">
<td align="left">**Stockage partagé d’entreprise**</td>
<td align="left"><p>La fonctionnalité restreinte **enterpriseDeviceLockdown** permet aux applications d’utiliser l’API de verrouillage de l’appareil et d’accéder aux dossiers de stockage partagé de l’entreprise.</p></td>
</tr>
<tr class="odd">
<td align="left">**Injection d’entrée système**</td>
<td align="left"><p>La fonctionnalité restreinte **inputInjection** permet aux applications d’injecter différentes formes d’entrée (HID, interface tactile, stylet, clavier, souris, etc.) par programme dans le système. Cette fonctionnalité est généralement utilisée pour les applications de collaboration qui peuvent prendre le contrôle du système.</p>
<div class="alert">
**Remarque** Pour un PC, les entrées injectées par une application disposant de cette fonctionnalité sont reçues uniquement par les processus résidant dans le même conteneur d’application.
</div>
</td>
</tr>
<tr class="even">
<td align="left">**Observation d’entrée***</td>
<td align="left"><p>La fonctionnalité restreinte **inputObservation** permet aux applications d’observer différentes formes d’entrée brute (HID, interface tactile, stylet, clavier, souris, etc.) reçues par le système, quelle que soit leur destination finale.</p></td>
</tr>
<tr class="odd">
<td align="left">**Suppression d’entrée**</td>
<td align="left"><p>La fonctionnalité restreinte **inputSuppression** permet aux applications de supprimer différentes formes d’entrée brute (HID, interface tactile, stylet, clavier, souris, etc.) reçues par le système.</p></td>
</tr>
<tr class="even">
<td align="left">**Application VPN**</td>
<td align="left"><p>La fonctionnalité restreinte **networkingVpnProvider** permet aux applications de disposer d’un accès complet aux fonctions de réseau privé virtuel (VPN, Virtual Private Network), y compris la possibilité de gérer les connexions et de fournir les fonctionnalités de plug-in VPN.</p>
<p>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040).</p></td>
</tr>
<tr class="odd">
<td align="left">**Gestion d’autres applications**</td>
<td align="left"><p>La fonctionnalité restreinte **packageManagement** permet aux applications de gérer directement d’autres applications.</p>
<p>La fonctionnalité de périphérique **packageQuery** permet aux applications de collecter des informations sur les autres applications.</p>
<p>Ces fonctionnalités sont requises pour l’accès à certaines méthodes et propriétés de la classe [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960).</p></td>
</tr>
<tr class="even">
<td align="left">**Projection d’écran**</td>
<td align="left"><p>La fonctionnalité restreinte **screenDuplication** permet aux applications de projeter l’écran sur un autre appareil.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms DirectX.</p></td>
</tr>
<tr class="odd">
<td align="left">**Nom d’utilisateur principal**</td>
<td align="left"><p>La fonctionnalité restreinte **userPrincipalName** permet aux applications d’accéder au cache de miniatures des photos et de le modifier.</p>
<p>Cette fonctionnalité est requise pour l’appel de la fonction [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435).</p></td>
</tr>
<tr class="even">
<td align="left">**Portefeuille**</td>
<td align="left"><p>La fonctionnalité restreinte **walletSystem** permet aux applications de disposer d’un accès complet aux cartes de renseignements médicaux (au format portefeuille) stockées.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610).</p></td>
</tr>
<tr class="odd">
<td align="left">**Historique de localisation**</td>
<td align="left"><p>La fonctionnalité restreinte **locationHistory** permet aux applications d’accéder à l’historique de localisation de l’appareil.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603).</p></td>
</tr>
<tr class="even">
<td align="left">**Confirmation de fermeture d’application**</td>
<td align="left"><p>La fonctionnalité restreinte **confirmAppClose** permet aux applications de se fermer par elles-mêmes, de fermer leurs propres fenêtres et de retarder leur fermeture.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295).</p></td>
</tr>
<tr class="odd">
<td align="left">**Historique des appels***</td>
<td align="left"><p>La fonctionnalité restreinte **phoneCallHistory** permet aux applications de lire l’historique des appels et d’en supprimer des entrées.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="even">
<td align="left">**Accès de niveau système aux rendez-vous**</td>
<td align="left"><p>La fonctionnalité restreinte **appointmentsSystem** permet aux applications de lire et de modifier tous les rendez-vous du calendrier de l’utilisateur.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359).</p></td>
</tr>
<tr class="odd">
<td align="left">**Accès de niveau système aux messages de conversation***</td>
<td align="left"><p>La fonctionnalité restreinte **chatSystem** permet aux applications de lire et d’écrire tous les messages SMS et MMS.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="even">
<td align="left">**Accès de niveau système aux contacts**</td>
<td align="left"><p>La fonctionnalité restreinte **contactsSystem** permet aux applications de lire les informations de contact marquées comme confidentielles ou sensibles et de modifier les informations de contacts existantes.</p>
<p>Cette fonctionnalité est nécessaire pour utiliser les API de l’espace de noms [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="odd">
<td align="left">**Accès aux e-mails***</td>
<td align="left"><p>La fonctionnalité restreinte **email** permet aux applications de lire, de trier et d’envoyer les e-mails de l’utilisateur.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285).</p></td>
</tr>
<tr class="even">
<td align="left">**Accès de niveau système aux e-mails**</td>
<td align="left"><p>La fonctionnalité restreinte **email** permet aux applications de lire, de trier et d’envoyer des e-mails confidentiels ou sensibles de l’utilisateur.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285).</p></td>
</tr>
<tr class="odd">
<td align="left">**Accès de niveau système à l’historique des appels**</td>
<td align="left"><p>La fonctionnalité restreinte **phoneCallHistorySystem** permet aux applications de modifier complètement l’historique des appels en modifiant et créant des entrées.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266).</p></td>
</tr>
<tr class="even">
<td align="left">**Envoi de SMS***</td>
<td align="left"><p>La fonctionnalité restreinte **smsSend** permet aux applications d’envoyer des messages SMS et MMS.</p>
<p>Cette fonctionnalité est requise pour l’utilisation des API de l’espace de noms [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="odd">
<td align="left">**Accès de niveau système à toutes les données utilisateur**</td>
<td align="left"><p>La fonctionnalité restreinte **userDataSystem** permet aux applications d’accéder à la banque de données système des données utilisateur.</p></td>
</tr>
<tr class="even">
<td align="left">**Fonctionnalités d’aperçu du Windows Store**</td>
<td align="left"><p>La fonctionnalité restreinte **previewStore** permet aux applications de récupérer et d’acheter des références (SKU) de produits intégrés à l’application.</p>
<p>Cette fonctionnalité est requise pour l’utilisation de certaines API de l’espace de noms [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546).</p></td>
</tr>
<tr class="odd">
<td align="left">**Paramètres de première connexion**</td>
<td align="left"><p>La fonctionnalité restreinte **firstSignInSettings** permet aux applications d’accéder aux paramètres utilisateur définis lors de la première connexion de l’utilisateur à son appareil.</p></td>
</tr>
<tr class="even">
<td align="left">**Expérience d’équipe Windows**</td>
<td align="left"><p>La fonctionnalité restreinte **teamEditionExperience** permet aux applications d’accéder aux API internes qui contrôlent de nombreux aspects expérimentaux d’une session d’équipe Windows. Une session d’équipe Windows peut s’exécuter sur un appareil collaboratif tel qu’un Microsoft Surface Hub.</p></td>
</tr>
<tr class="odd">
<td align="left">**Déverrouillage à distance**</td>
<td align="left"><p>La fonctionnalité restreinte **remotePassportAuthentication** permet aux applications d’accéder aux informations d’identification qui peuvent être utilisées pour déverrouiller un PC distant.</p></td>
</tr>
<tr class="even">
<td align="left">**Composition d’aperçu**</td>
<td align="left"><p>La fonctionnalité restreinte **previewUiComposition** permet aux applications d’afficher un aperçu de l’espace de noms [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) pour leur interface utilisateur afin de pouvoir fournir des commentaires sur l’API avant la fin de son exécution. Pour plus d’informations, contactez wincomposition@microsoft.com.</p></td>
</tr>
<tr class="odd">
<td align="left">**Verrouillage de sécurisation des évaluations**</td>
<td align="left"><p>La fonctionnalité restreinte **secureAssessment** permet aux applications de verrouiller Windows en mode Application unique pour sécuriser les évaluations.</p></td>
</tr>
<tr class="even">
<td align="left">**Approvisionnement du Gestionnaire des connexions**</td>
<td align="left"><p>La fonctionnalité restreinte **networkConnectionManagerProvisioning** permet aux applications de définir les stratégies régissant la connexion de l’appareil avec des interfaces de réseau étendu sans fil (WWAN, Wireless Wide Area Network) et de réseau local sans fil (WLAN, Wireless Local Area Network). Des applications utilisant cette fonctionnalité sont créées par des opérateurs mobiles pour gérer la connexion d’appareils à leur réseau mobile.</p></td>
</tr>
<tr class="odd">
<td align="left">**Approvisionnement de forfait de données**</td>
<td align="left"><p>La fonctionnalité restreinte **networkDataPlanProvisioning** permet aux applications de collecter des informations concernant les forfaits de données sur l’appareil et de lire l’utilisation du réseau. Des applications utilisant cette fonctionnalité sont créées par des opérateurs mobiles pour intégrer la consommation réelle des données par leurs clients dans le paramètre de consommation des données du système d’exploitation.</p></td>
</tr>
<tr class="even">
<td align="left">**Gestion de licences des logiciels**</td>
<td align="left"><p>La fonctionnalité restreinte **slapiQueryLicenseValue** permet aux applications d’interroger les stratégies de licence des logiciels.</p></td>
</tr>
<tr class="odd">
<td align="left">**Exécution étendue**</td>
<td align="left"><p>La fonctionnalité restreinte **extendedExecutionBackgroundAudio** permet aux applications de lire l’audio quand l’application n’est pas au premier plan.</p>
<p>La fonctionnalité restreinte **extendedExecutionCritical** permet aux applications de commencer une session d’exécution étendue critique.</p>
<p>La fonctionnalité restreinte **extendedExecutionUnconstrained** permet aux applications de démarrer une session d’exécution étendue sans contraintes.</p></td>
</tr>
<tr class="even">
<td align="left">**Gestion des appareils mobiles**</td>
<td align="left"><p>La fonctionnalité restreinte **deviceManagementDmAccount** permet aux applications d’approvisionner et de configurer les comptes MO OMA-DM (Mobile Operator Open Mobile Aliance - Device Management).</p>
<p>La fonctionnalité restreinte **deviceManagementFoundation** permet aux applications de disposer d’un accès de base à l’infrastructure du fournisseur de services de configuration (CSP) dans la gestion des périphériques mobiles (GPM) sur l’appareil. Notez que d’autres fonctionnalités sont nécessaires pour accéder à des CSP spécifiques.</p>
<p>La fonctionnalité restreinte **deviceManagementWapSecurityPolicies** permet aux applications de configurer des services basés sur le protocole WAP (Wireless Application Protocol), tels que les MMS, la stratégie des messages d’indication de service (IS)/chargement de service (CS), et OMA-CP (Open Mobile Alliance - Client Provisioning).</p>
<p>La fonctionnalité restreinte **deviceManagementEmailAccount** permet à des applications créées par des opérateurs mobiles d’ajouter et de gérer un compte de messagerie sur des appareils fournis aux utilisateurs.</p></td>
</tr>
<tr class="odd">
<td align="left">**Contrôle de stratégie de package**</td>
<td align="left"><p>La fonctionnalité restreinte **packagePolicySystem** permet aux applications de contrôler les stratégies système liées aux applications installées sur l’appareil.</p></td>
</tr>
<tr class="even">
<td align="left">**Liste de jeux**</td>
<td align="left"><p>La fonctionnalité restreinte **gameList** permet aux applications d’obtenir la liste des jeux connus installés sur le système.</p></td>
</tr>
<tr class="odd">
<td align="left">**Accessoire Xbox**</td>
<td align="left"><p>La fonctionnalité restreinte **xboxAccessoryManagement** permet aux applications de gérer directement les appareils Xbox conformément aux spécifications matérielles Xbox.</p></td>
</tr>
<tr class="even">
<td align="left">**Reconnaissance vocale des accessoires**</td>
<td align="left"><p>La fonctionnalité restreinte **cortanaSpeechAccessory** permet aux applications d’appeler et de transmettre des commandes à Cortana.</p></td>
</tr>
<tr class="odd">
<td align="left">**Gestion des accessoires**</td>
<td align="left"><p>La fonctionnalité restreinte **accessoryManager** permet aux applications de s’inscrire en tant qu’applications pour accessoire et d’opter pour des notifications d’application spécifiques afin que celles-ci puissent être transférées à des accessoires et s’afficher à l’intention de l’utilisateur.</p></td>
</tr>
<tr class="even">
<td align="left">**Accès aux pilotes**</td>
<td align="left"><p>La fonctionnalité restreinte **interopServices** permet aux applications d’interagir directement avec les pilotes.</p></td>
</tr>
<tr class="odd">
<td align="left">**Observation de premier plan**</td>
<td align="left"><p>La fonctionnalité restreinte **inputForegroundObservation** permet aux applications de premier plan d’intercepter les saisies au clavier et de contourner tous les traitement de saisie au clavier non liés aux applications. Les combinaisons SAS ne peuvent pas être interceptées par cette fonctionnalité. Cette dernière est requise pour l’accès aux membres de la classe [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395).</p></td>
</tr>
<tr class="even">
<td align="left">**Applications de fabricants d’ordinateurs OEM et d’opérateurs mobiles partenaires**</td>
<td align="left"><p>La fonctionnalité restreinte **oemDeployment** permet aux applications qui sont créées par les partenaires Microsoft d’installer les nouvelles applications et d’interroger les applications actuellement installées sur l’appareil.</p>
<p>La fonctionnalité restreinte **oemPublicDirectory** permet aux applications qui sont créées par les partenaires Microsoft d’avoir accès au dossier des applications partagées.</p></td>
</tr>
<tr class="odd">
<td align="left">**Licence d’application**</td>
<td align="left"><p>La fonctionnalité restreinte **appLicensing** permet aux applications de s’exécuter sans nécessiter de licence. Vous ne pouvez pas soumettre votre application au Windows Store si vous déclarez cette fonctionnalité dans votre manifeste. Les demandes d’accès à cette fonctionnalité pour l’envoi au Windows Store seront toujours refusées.</p></td>
</tr>
<tr class="even">
<td align="left">**Système Localisation**</td>
<td align="left"><p>La fonctionnalité restreinte **locationSystem** permet aux applications d’effectuer certaines configurations d’emplacement privilégiées, comme la définition de l’emplacement par défaut de l’appareil. Vous ne pouvez pas soumettre votre application au Windows Store si vous déclarez cette fonctionnalité dans votre manifeste. Les demandes d’accès à cette fonctionnalité pour l’envoi au Windows Store seront toujours refusées.</p></td>
</tr>
</tbody>
</table>

**Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui écrivent des applications de plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Rubriques connexes

* [Concepteur du manifeste](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [Comment spécifier des fonctionnalités dans un manifeste de package](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [Comment spécifier des fonctionnalités de périphérique dans un manifeste de package](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 


<!--HONumber=Mar16_HO5-->


