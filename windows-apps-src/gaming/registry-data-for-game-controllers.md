---
title: Données de Registre pour les contrôleurs de jeu
description: En savoir plus sur les données qu’il est possible d’ajouter au Registre de l’ordinateur afin de pouvoir utiliser votre contrôleur dans les jeux UWP.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 06/25/2018
ms.topic: article
keywords: Windows10, uwp, jeux, entrée, registre, personnalisé
ms.localizationpriority: medium
ms.openlocfilehash: 3d30c19a7fd7641d76e810912d33a96dbbeb3132
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8752621"
---
# <a name="registry-data-for-game-controllers"></a>Données de Registre pour les contrôleurs de jeu

> [!NOTE]
> Cette rubrique est destinée aux fabricants de contrôleurs de jeu Windows10compatibles et ne s’applique pas à la majorité des développeurs.

L’[espace de noms Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) permet aux fabricants de matériel d’ajouter des données dans le Registre de l’ordinateur pour que leurs appareils s’affichent en tant que [boîtiers de commande](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad), [RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick), et [UINavigationControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller) selon les besoins. Tous les fabricants de matériel doivent ajouter ces données pour leurs contrôleurs compatibles. Ce faisant, tous les jeux UWP (et les jeux de bureau utilisant l’API WinRT) seront en mesure de prendre en charge votre manette de jeu.

## <a name="mapping-scheme"></a>Schéma de mappage

Les mappages relatifs à un appareil avec les ID de fournisseur (VID) **VVVV**, l’ID de produit (PID) **PPPP**, la page d’utilisation **UUUU**et l’ID d’utilisation **XXXX**, seront lus à partir de cet emplacement dans le Registre:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

Le tableau ci-dessous décrit les valeurs attendues sous l’emplacement racine de l’appareil:

<table>
    <tr>
        <th>Nom</th>
        <th>Type</th>
        <th>Obligatoire?</th>
        <th>Infos</th>
    </tr>
    <tr>
        <td>Désactivé</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>
            <p>Indique que cet appareil en particulier doit être désactivé.</p>
            <ul>
                <li><b>0</b>: l’appareil n’est pas désactivé.</li>
                <li><b>1</b>: l’appareil est désactivé.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Description</td>
        <td>REG_SZ <td>Non</td>
        <td>Brève description de l’appareil.</td>
    </tr>
</table>

Le programme d’installation de l’appareil doit ajouter ces données au Registre (via le programme d’installation ou un [fichier INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)).

Les sous-clés situées sous l’emplacement racine de l’appareil sont détaillées dans les sections suivantes.

### <a name="gamepad"></a>Boîtier de commande

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **Gamepad**:

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Obligatoire?</th>
        <th>Infos</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>Oui</td>
        <td rowspan="18" style="vertical-align: middle;">Voir <a href="#button-mapping">Mappage de bouton</a></td>
    </tr>
    <tr>
        <td>Afficher</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>A</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>B</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>X</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Y</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>Oui</td>
        <td rowspan="6" style="vertical-align: middle;">Voir <a href="#axis-mapping">Mappage des axes</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>Oui</td>
    </tr>
</table>

> [!NOTE]
> Si vous ajoutez votre contrôleur de jeu sous forme d’un **boîtier de commande** pris en charge, nous vous recommandons vivement de l’ajouter également sous forme de  **UINavigationController** pris en charge.

### <a name="racingwheel"></a>RacingWheel

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **RacingWheel**:

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Obligatoire?</th>
        <th>Infos</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>Oui</td>
        <td rowspan="30" style="vertical-align: middle;">Voir <a href="#button-mapping">Mappage de bouton</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Wheel</td>
        <td>Oui</td>
        <td rowspan="5" style="vertical-align: middle;">Voir <a href="#axis-mapping">Mappage des axes</a></td>
    </tr>
    <tr>
        <td>Limite de bande passante</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Frein</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Embrayage</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>Oui</td>
        <td>Voir <a href="#properties-mapping">Mappage des propriétés</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **ArcadeStick**:

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Obligatoire?</th>
        <th>Infos</th>
    </tr>
    <tr>
        <td>Action1</td>
        <td>Oui</td>
        <td rowspan="12" style="vertical-align: middle;">Voir <a href="#button-mapping">Mappage de bouton</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>Oui</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **ArcadeStick**:

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Obligatoire?</th>
        <th>Infos</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>Oui</td>
        <td rowspan="2" style="vertical-align: middle;">Voir <a href="#button-mapping">Mappage de bouton</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Annuler</td>
        <td>Oui</td>
        <td rowspan="4" style="vertical-align: middle;">Voir <a href="#axis-mapping">Mappage des axes</a></td>
    </tr>
    <tr>
        <td>Inclinaison</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Lacet</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Limite de bande passante</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>Oui</td>
        <td>Voir <a href="#switch-mapping">Mappage de commutateur</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **UINavigation**:

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Obligatoire?</th>
        <th>Infos</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>Oui</td>
        <td rowspan="24" style="vertical-align: middle;">Voir <a href="#button-mapping">Mappage de bouton</a></td>
    </tr>
    <tr>
        <td>Afficher</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Accepter</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Annuler</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>PageUp</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>PageDown</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>Non</td>
    </tr>
</table>

Pour plus d’informations sur les contrôleurs de navigation de l’interface utilisateur et les commandes ci-dessus, voir [contrôleur de navigation de l’interface utilisateur](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller).

## <a name="keys"></a>Clés

Les sections suivantes expliquent le contenu de chaque sous-clé figurant sous les clés **Gamepad**, **RacingWheel**, **ArcadeStick**, **FlightStick**, et **UINavigation**.

### <a name="button-mapping"></a>Mappage de bouton

Le tableau ci-dessous répertorie les valeurs qui sont nécessaires au mappage d’un bouton. Par exemple, si vous appuyez sur **DPadUp** sur le contrôleur de jeu, le mappage de **DPadUp** doit contenir la valeur **ButtonIndex** (**Source** correspond à **Button**). Si **DPadUp** doit être mappé à partir d’une position de commutateur, le mappage **DPadUp** doit contenir les valeurs **SwitchIndex** et **SwitchPosition** (**Source** correspond à **Switch**).

<table>
    <tr>
        <th>Source</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Obligatoire?</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td>Bouton</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index du tableau des boutons du <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Axe</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index du tableau des axes du <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>Indique que la valeur de l’axe doit être inversée avant l’application des facteurs <b>ThresholdPercent</b> et <b>DebouncePercent</b>.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Indique la position de l’axe à laquelle la valeur du bouton mappé passe de l’état «enfoncé» à «relâché». La valeur doit être comprise entre 0et100. Le bouton est considéré comme enfoncé si la valeur de l’axe est supérieure ou égale à cette valeur.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>
            <p>Définit la taille de la fenêtre autour de la valeur <b>ThresholdPercent</b> utilisée pour afficher l’état du bouton signalé. La valeur doit être comprise entre 0et100. Des transitions d’état de bouton peuvent se produire uniquement lorsque la valeur de l’axe dépasse les limites supérieures ou inférieures de la fenêtre de réponse. Par exemple, un <b>ThresholdPercent</b> de 50 et un <b>DebouncePercent</b> de 10donnent des limites de réponse à 45% et 55% des valeurs d’axe de plage complètes. Le bouton ne peut pas passer à l’état «enfoncé» tant que la valeur d’axe n’a pas atteint 55% ou plus et ne peut pas revenir à l’état «relâché» tant que cette valeur n’est pas égale à 45% ou moins.</p>
            <p>Les limites de la fenêtre de réponse calculées sont comprises entre 0 et 100%. Par exemple, un seuil de 5% et une fenêtre de réponse de 20% entraînerait la réduction des limites de la fenêtre de réponse à 0 et 15%. L’état du bouton relatif aux valeurs d’axe comprises entre 0 et 100% correspond respectivement à «relâché» ou «enfoncé», quelles que soient les valeurs de seuil et de réponse.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Commutateur</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index du tableau des commutateurs du <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>Oui</td>
        <td>
            <p>Indique la position de commutateur qui entraîne le signalement par le bouton mappé de son état «enfoncé». Les valeurs de position peuvent être une des chaînes suivantes:</p>
            <ul>
                <li>Up</li> 
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>Indique que les positions de commutateur adjacentes entraînent également le signalement par le bouton mappé de son état «enfoncé».</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Mappage des axes

Le tableau ci-dessous répertorie les valeurs nécessaires au mappage d’un axe:

<table>
    <tr>
        <th>Source</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Obligatoire?</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Bouton</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>
            <p>Index du tableau des boutons du <b>RawGameController</b> qui est converti dans la valeur de l’axe unidirectionnel mappé.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1,0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>
            <p>Indique que l’axe mappé est bidirectionnel. Les valeurs de <b>MaxButton</b> et <b>MinButton</b> sont combinées en un axe bidirectionnel unique, comme illustré ci-dessous.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0,5</td>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>TRUE</td>
                    <td>1,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0,5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Axe</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index du tableau des axes du <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>Indique que la valeur de l’axe mappé doit être inversée avant d’être renvoyée.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Commutateur</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index du tableau des commutateurs du <b>RawGameController</b>.
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>Oui</td>
        <td>
            <p>Une des chaînes suivantes:</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>Elle indique la position du commutateur entraînant le signalement de la valeur de l’axe mappé comme correspondant à 1,0. La direction opposée de <b>MaxValueSwitchPosition</b> est traitée comme correspondant à 0,0. Par exemple, si <b>MaxValueSwitchPosition</b> est <b>Up</b>, la conversion de la valeur de l’axe est indiquée ci-dessous:</p>
            <table>
                <tr>
                    <th>Position du commutateur</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1,0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0,5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>
            <p>Indique que les positions de commutateur adjacentes entraînent également le signalement de 1,0 par l’axe mappé. Dans l’exemple ci-dessus, si <b>IncludeAdjacent</b> est définie, la conversion de l’axe s’effectue comme suit:</p>
            <table>
                <tr>
                    <th>Position du commutateur</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1,0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1,0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1,0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0,5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Mappage de commutateur

Les positions de commutateur peuvent être mappées à partir d’un ensemble de boutons du tableau des boutons du **RawGameController** ou à partir d’un index du tableau des commutateurs. Les positions de commutateur ne peuvent pas être mappées à partir des axes.

<table>
    <tr>
        <th>Source</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Bouton</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 ou 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> ou <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Voir <a href="#buttonindex-values">* Valeurs de ButtonIndex</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Axe</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> ou <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> est toujours présent. <b>XAxisIndex</b> s’affiche uniquement lorsque <b>SwitchKind</b> est <b>FourWay</b> ou <b>EightWay</b>.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indique la taille de la zone morte autour de la position centrale des axes.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Définissez la taille des fenêtres autour des limites inférieures et supérieures de la zone morte utilisées pour afficher l’état du commutateur signalé.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indiquez que les valeurs d’axe correspondantes doivent être inversées avant l’application des calculs relatifs à la zone morte et à la fenêtre de réponse.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Commutateur</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Index du tableau des commutateurs du <b>RawGameController</b>.
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>Indique que le commutateur signale ses positions dans l’ordre inverse des aiguilles d’une montre, plutôt que dans le sens par défaut (sens des aiguilles d’une montre).</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>Décale le point de départ de la manière dont les positions sont indiquées selon le montant spécifié. <b>PositionBias</b> est toujours comptabilisé dans le sens des aiguilles d’une montre à partir du point de départ d’origine et est appliqué avant l’inversement de l’ordre des valeurs.</p>
            <p>Par exemple, un commutateur qui indique des positions commençant à partir de <b>DownRight</b> dans le sens inverse des aiguilles d’une montre peut être normalisé en définissant l’indicateur <b>Invert</b> et en spécifiant <b>PositionBias</b> sur 5:</p>
            <table>
                <tr>
                    <th>Position</th>
                    <th>Valeur signalée</th>
                    <th>Après les indicateurs PositionBias et Invert</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>* Valeurs de ButtonIndex

\*Index des valeurs ButtonIndex du tableau des boutons du **RawGameController**:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>Mappage des propriétés

Il s’agit de valeurs de mappages statiques pour différents types de mappages.

<table>
    <tr>
        <th>Mappage</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>Indique l’angle de molette physique maximal pris en charge par la molette dans un seul sens. Par exemple, une molette avec une rotation possible de -90degrés à 90degrés indiquerait 90.</td>
    </tr>
</table>

## <a name="labels"></a>Libellés

Les libellés doivent être présents sous la clé **Labels** située sous la racine de l’appareil. **Libellés** peut avoir 3sous-clés: **Buttons**, **Axes**, et **Switches**.

### <a name="button-labels"></a>Libellé des boutons

La clé **Boutons** mappe chacune des positions du bouton du tableau des boutons du **RawGameController** avec une chaîne. Chaque chaîne est mappée en interne avec la valeur d’énumération [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) correspondante. Par exemple, si un boîtier de commande comporte dix boutons et si l’ordre dans lequel le **RawGameController** analyse les boutons et les affiche dans le rapport sur les boutons ressemble à ce qui suit:

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

Les libellés doivent apparaître dans cet ordre sous la clé **Buttons**:

<table>
    <tr>
        <th>Nom</th>
        <th>Valeur (type: REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>Menu</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>View</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>Libellés d’axes

La clé **Axes** mappe chacune des positions de l’axe du tableau des axes de **RawGameController**avec un des libellés figurant dans [Énumération GameControllerButtonLabel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) conformément aux libellés des boutons. Reportez-vous à l’exemple figurant dans [Libellés de boutons](#button-labels).

### <a name="switch-labels"></a>Libellés de commutateurs

La clé **Switches** mappe les positions de commutateur avec les libellés. Les valeurs suivent cette convention d’affectation de noms: pour libeller la position d’un commutateur, dont l’index est *x* dans le tableau des commutateurs du **RawGameController**, ajoutez ces valeurs sous la sous-clé **Switches**: 

* SwitchxUp 
* SwitchxUpRight 
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

Le tableau suivant présente un exemple d’ensemble de libellés pour les postes de commutateur d’un commutateur à 4voies qui s’affiche à l’index 0dans le **RawGameController**: 

<table>
    <tr>
        <th>Nom</th>
        <th>Valeur (type: REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>Exemple de fichier de Registre

Pour afficher la manière dont l’ensemble de ces mappages et valeurs s’associent, voici un exemple de fichier de Registre pour un **RacingWheel** spécifique:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>Articles associés

* [Espace de noms Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Espace de noms Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)
* [Fichiers INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)