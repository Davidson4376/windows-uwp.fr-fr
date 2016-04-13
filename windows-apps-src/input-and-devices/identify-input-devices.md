---
Description: Identifiez les appareils d’entrée connectés à un appareil de plateforme Windows universelle (UWP), ainsi que leurs fonctionnalités et attributs.
title: Identifier des appareils d’entrée
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identifier des appareils d’entrée
template: detail.hbs
---

# Identifier des appareils d’entrée


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

Identifiez les appareils d’entrée connectés à un appareil de plateforme Windows universelle (UWP), ainsi que leurs fonctionnalités et attributs.


## <span id="Retrieve_mouse_properties"> </span> <span id="retrieve_mouse_properties"> </span> <span id="RETRIEVE_MOUSE_PROPERTIES"> </span>Récupérer les propriétés de la souris


L’espace de noms [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contient la classe [**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626), utilisée pour récupérer les propriétés exposées par une ou plusieurs souris connectées. Créez simplement un objet **MouseCapabilities** et obtenez les propriétés qui vous intéressent.

**Remarque** Les valeurs renvoyées par les propriétés abordées ici sont basées sur l’ensemble des souris détectées : les propriétés booléennes renvoient une valeur différente de zéro si au moins une souris prend en charge une fonctionnalité spécifique, tandis que les propriétés numériques renvoient la valeur maximale exposée par n’importe quelle souris.

 

Le code suivant utilise une série d’éléments [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) permettant d’afficher les propriétés et valeurs de chaque souris.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <span id="Retrieve_keyboard_properties"> </span> <span id="retrieve_keyboard_properties"> </span> <span id="RETRIEVE_KEYBOARD_PROPERTIES"> </span>Récupérer les propriétés du clavier


L’espace de noms [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contient la classe [**KeyboardCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225623), utilisée pour savoir si un clavier est connecté. Créez simplement un objet **KeyboardCapabilities** et obtenez la propriété [**KeyboardPresent**](https://msdn.microsoft.com/library/windows/apps/br225625).

Le code suivant utilise un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) permettant d’afficher la propriété et la valeur du clavier.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <span id="Retrieve_touch_properties"> </span> <span id="retrieve_touch_properties"> </span> <span id="RETRIEVE_TOUCH_PROPERTIES"> </span>Récupérer les propriétés de l’entrée tactile


L’espace de noms [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contient la classe [**TouchCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225644), utilisée pour savoir si un numériseur d’entrée tactile est connecté. Créez simplement un objet **TouchCapabilities** et obtenez les propriétés qui vous intéressent.

**Remarque** Les valeurs renvoyées par les propriétés abordées ici sont basées sur l’ensemble des numériseurs d’entrée tactile détectés : les propriétés booléennes renvoient une valeur différente de zéro si au moins un numériseur prend en charge une fonctionnalité spécifique, tandis que les propriétés numériques renvoient la valeur maximale exposée par n’importe quel numériseur.

 

Le code suivant utilise une série d’éléments [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) permettant d’afficher les propriétés et valeurs de l’entrée tactile.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <span id="Retrieve_pointer_properties"> </span> <span id="retrieve_pointer_properties"> </span> <span id="RETRIEVE_POINTER_PROPERTIES"> </span>Récupérer les propriétés du pointeur


L’espace de noms [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) contient la classe [**PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633), utilisée pour savoir si l’un des appareils détectés prend en charge l’entrée de pointeur (appareil tactile, pavé tactile, souris ou stylet). Créez simplement un objet **PointerDevice** et obtenez les propriétés qui vous intéressent.

**Remarque** Les valeurs renvoyées par les propriétés abordées ici sont basées sur l’ensemble des appareils de pointage détectés : les propriétés booléennes renvoient une valeur différente de zéro si au moins un appareil prend en charge une fonctionnalité spécifique, tandis que les propriétés numériques renvoient la valeur maximale exposée par n’importe quel appareil de pointage.

 

Le code suivant utilise un tableau permettant d’afficher les propriétés et valeurs de chaque appareil de pointage.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <span id="related_topics"> </span>Articles connexes


**Exemples**
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
**Exemples d’archive**
* [Entrée : exemple de fonctionnalités d’appareils](http://go.microsoft.com/fwlink/p/?linkid=231530)
 

 






<!--HONumber=Mar16_HO1-->


