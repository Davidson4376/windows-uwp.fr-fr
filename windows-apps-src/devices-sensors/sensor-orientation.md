---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Orientation de capteur
description: Les données du capteur provenant des classes Accelerometer, Gyrometer, Compass, Inclinometer et OrientationSensor sont définies par leurs axes de référence. Ces axes sont définis par l’orientation paysage de l’appareil et pivotent avec celui-ci à mesure que l’utilisateur le fait tourner.
ms.date: 05/24/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bfd84cd2f2255138b738ecb6dd7f6dab824d7ec4
ms.sourcegitcommit: d1ef530ef4dfa34db7bc429ab5a0c19fc405885f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71247453"
---
# <a name="sensor-orientation"></a>Orientation de capteur

Les données du capteur provenant des classes [**Accelerometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer), [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer), [**Compass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Compass), [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) et [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) sont définies par leurs axes de référence. Ces axes sont définis par l’image de référence de l’appareil, et pivotent avec celui-ci à mesure que l’utilisateur le fait tourner. Si votre application prend en charge la rotation automatique et se réoriente pour s’adapter à l’appareil à mesure que l’utilisateur le fait pivoter, vous devez ajuster vos données du capteur par rapport à la rotation avant de l’utiliser.

### <a name="important-apis"></a>API importantes

- [**Windows. Devices. Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
- [**Windows. Devices. Sensors. Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>Orientation de l’affichage et orientation de l’appareil

Pour comprendre les axes de référence pour les capteurs, vous devez distinguer l’orientation de l’affichage de l’orientation de l’appareil. L’orientation de l’affichage correspond au sens dans lequel le texte et les images sont affichés à l’écran, alors que l’orientation de l’appareil correspond au positionnement physique de l’appareil.

Dans les diagrammes suivants, l’orientation de l’appareil et de l’affichage est en [paysage](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) (les axes de capteur sont spécifiques à l’orientation paysage), avec l’axe z positif qui s’étend à partir de l’appareil.

Ce diagramme illustre l’affichage et l’orientation de l’appareil en [mode paysage](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations).

![Orientation de l’affichage et de l’appareil en mode Landscape](images/sensor-orientation-a.PNG)

Le diagramme suivant montre l’orientation de l’affichage et de l’appareil dans [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations).

![Orientation de l’affichage et de l’appareil en mode LandscapeFlipped](images/sensor-orientation-b.PNG)

Ce diagramme final montre l’orientation d’affichage en paysage alors que l’orientation de l’appareil est [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations).

![Orientation d’affichage en mode Landscape tandis que l’orientation de l’appareil est en mode LandscapeFlipped.](images/sensor-orientation-c.PNG)

Vous pouvez effectuer une requête sur les valeurs d’orientation dans la classe [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) à l’aide de la méthode [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) avec la propriété [**CurrentOrientation**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.currentorientation). Vous pouvez ensuite créer une logique en comparant ces valeurs avec l’énumération [**DisplayOrientations**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations). Souvenez-vous que, pour chaque orientation que vous prenez en charge, vous devez prendre en charge une conversion des axes de référence dans cette orientation.

## <a name="landscape-first-vs-portrait-first-devices"></a>Appareils à priorité Paysage ou à priorité Portrait

Les fabricants produisent des appareils à priorité Paysage ou Portrait. Le cadre de référence varie selon que les appareils sont à priorité Paysage (tels que les ordinateurs de bureau et ordinateurs portables) ou à priorité Portrait (comme les téléphones et certaines tablettes). Le tableau suivant montre les axes de capteur pour les appareils à priorité Paysage et Portrait.

| Orientation | Priorité Paysage | Priorité Portrait |
|-------------|-----------------|----------------|
| **Jardin** | ![Appareil à priorité Paysage en mode d’orientation Landscape](images/sensor-orientation-0.PNG) | ![Appareil à priorité Portrait en mode d’orientation Landscape](images/sensor-orientation-1.PNG) |
| **Aperçu** | ![Appareil à priorité Paysage en mode d’orientation Portrait](images/sensor-orientation-2.PNG) | ![Appareil à priorité Portrait en mode d’orientation Portrait](images/sensor-orientation-3.PNG) |
| **LandscapeFlipped** | ![Appareil à priorité Paysage en mode d’orientation LandscapeFlipped](images/sensor-orientation-4.PNG) | ![Appareil à priorité Portrait en mode d’orientation LandscapeFlipped](images/sensor-orientation-5.PNG) | 
| **PortraitFlipped** | ![Appareil à priorité Paysage en mode d’orientation PortraitFlipped](images/sensor-orientation-6.PNG)| ![Appareil à priorité Portrait en mode d’orientation PortraitFlipped](images/sensor-orientation-7.PNG) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>Appareils diffusant leur affichage et appareils sans affichage

Certains appareils sont capables de diffuser leur affichage vers un autre appareil. Par exemple, vous pouvez avoir une tablette capable de diffuser son affichage vers un projecteur en orientation paysage. Dans cette situation, il est important de garder à l’esprit que l’orientation de l’appareil est basée sur celle de l’appareil d’origine et non sur celle de l’appareil qui affiche. Par conséquent, un accéléromètre indiquerait les données pour la tablette.

En outre, certains appareils n’ont pas d’affichage. L’orientation par défaut de ces appareils est le mode Portrait.

## <a name="display-orientation-and-compass-heading"></a>Orientation de l’affichage et orientation de la boussole

L’orientation de la boussole dépend des axes de référence. Elle change donc avec l’orientation de l’appareil. Vous compensez en vous appuyant sur le tableau suivant (en supposant que l’utilisateur est orienté au nord).

| Orientation de l’affichage | Axe de référence pour l’orientation de la boussole | Orientation de la boussole de l’API avec une orientation au nord (à priorité Paysage) | Orientation de la boussole de l’API avec une orientation au nord (à priorité Portrait) |Compensation de l’orientation de la boussole (à priorité Paysage) | Compensation de l’orientation de la boussole (à priorité Portrait) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Paysage           | -Z | 0   | 270 | Orientation               | (Orientation + 90) % 360  |
| Portrait            |  Y | 90  | 0   | (Orientation + 270) % 360 |  Orientation              |
| LandscapeFlipped    |  Z | 180 | 90  | (Orientation + 180) % 360 | (Orientation + 270) % 360 |
| PortraitFlipped     |  Y | 270 | 180 | (Orientation + 90) % 360  | (Orientation + 180) % 360 |

Modifiez le cap de la boussole tel qu’indiqué dans le tableau pour afficher correctement le cap. L’extrait de code suivant montre comment procéder.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>Orientation de l’affichage avec l’accéléromètre et le gyromètre

Le tableau suivant convertit les données de l’accéléromètre et du gyromètre pour obtenir l’orientation de l’affichage.

| Axes de référence        |  X |  Y | Z |
|-----------------------|----|----|---|
| **Jardin**         |  X |  Y | Z |
| **Aperçu**          |  Y | -X | Z |
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

L’exemple de code suivant applique ces conversions au gyromètre.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>Orientation d’affichage et orientation d’appareil

Les données [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) doivent être modifiées d’une autre façon. Considérez ces différentes orientations comme des rotations dans le sens inverse des aiguilles d’une position vers l’axe Z. nous devons donc inverser la rotation pour revenir à l’orientation de l’utilisateur. Pour les données de quaternion, nous pouvons utiliser la formule d’Euler pour définir une rotation avec un quaternion de référence. Nous pouvons également utiliser une matrice de rotation de référence.

![Formule d’Euler](images/eulers-formula.png)

Pour obtenir l’orientation relative souhaitée, multipliez l’objet de référence par l’objet absolu. Notez que cette formule mathématique n’est pas commutative.

![Multipliez l’objet de référence par l’objet absolu](images/orientation-formula.png)

Dans l’expression précédente, l’objet absolu est retourné par les données du capteur.

| Orientation de l’affichage  | Rotation dans le sens inverse des aiguilles d’une montre autour de Z | Quaternion de référence (rotation inverse) | Matrice de rotation de référence (rotation inverse) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Jardin**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Aperçu**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |
