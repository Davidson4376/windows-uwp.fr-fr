---
author: Karl-Bridge-Microsoft
Description: "Recevez, traitez et gérez les données d’entrée à partir de dispositifs de pointage, tels que l’interaction tactile, la souris, le stylet et le pavé tactile, dans les applications UWP."
title: "Gérer les entrées du pointeur"
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 2204e8f3ddce067cf2cbc24ce89cbdcea5b361bf

---

# Gérer les entrées du pointeur

Recevez, traitez et gérez les données d’entrée à partir de dispositifs de pointage, tels que l’interaction tactile, la souris, le stylet et le pavé tactile, dans les applications UWP.

**API importantes**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


**Important**  
Si vous implémentez votre propre prise en charge d’interaction, gardez à l’esprit que les utilisateurs s’attendent à disposer d’une expérience intuitive impliquant une interaction directe avec les éléments d’interface utilisateur de votre application. Nous vous recommandons de modéliser vos interactions personnalisées dans la [liste des contrôles](https://msdn.microsoft.com/library/windows/apps/mt185406) par souci de cohérence et de possibilité de détection. Les contrôles de plateforme fournissent l’intégralité de l’expérience d’interaction utilisateur de la plateforme Windows universelle (UWP), notamment pour les interactions standard, les effets physiques animés, le retour visuel et l’accessibilité. Ne créez des interactions personnalisées que pour répondre à des exigences claires et bien définies, notamment en l’absence d’interactions de base prenant en charge votre scénario.


## Pointeurs


De nombreuses expériences d’interaction impliquent que l’utilisateur identifie l’objet avec lequel il souhaite interagir en pointant dessus à l’aide de périphériques d’entrée comme l’interaction tactile, la souris, le stylo/stylet et le pavé tactile. Les données brutes HID (périphérique d’interface utilisateur) fournies par ces périphériques d’entrée ont de nombreuses propriétés communes. Par conséquent, les informations sont promues dans une pile d’entrée unifiée et exposées sous la forme de données de pointeur consolidées, indépendantes du périphérique. Vos applications UWP peuvent alors utiliser ces données quel que soit le périphérique d’entrée utilisé.

**Remarque** Les informations propres au périphérique sont également promues à partir des données brutes HID si votre application l’exige.

 

Chaque point (ou contact) d’entrée sur la pile d’entrée est représenté par un objet [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968) exposé via le paramètre [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) fourni par différents événements de pointeur. En cas d’entrées multistylets ou tactiles multipoints, chaque contact est considéré comme un point d’entrée unique.

## Événements de pointeur


Les événements de pointeur exposent des informations de base, telles que l’état de détection (dans la plage ou au contact) et le type de périphérique. Ils exposent également des informations détaillées, telles que l’emplacement, la pression et la géométrie du contact. D’autres propriétés de périphérique spécifiques sont également disponibles. Elles indiquent sur quel bouton de souris l’utilisateur a appuyé ou si la gomme du stylet a été utilisée. Si une différenciation entre les périphériques d’entrée et leurs fonctionnalités est nécessaire dans le cadre de votre application, voir [Identifier des périphériques d’entrée](identify-input-devices.md).

Les applications UWP peuvent écouter les événements de pointeur suivants :

**Remarque** Appelez [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) pour limiter les entrées de pointeur à un élément d’interface utilisateur spécifique. Lorsqu’un pointeur est capturé par un élément, seul cet objet reçoit les événements d’entrée de pointeur, même lorsque le pointeur se déplace à l’extérieur de la zone de délimitation de l’objet. Vous capturez généralement le pointeur dans un gestionnaire d’événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), car [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976) (bouton de souris enfoncé, interaction tactile ou stylet en contact) doit être défini sur true pour que **CapturePointer** réussisse.

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Événement</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[<strong>PointerCanceled</strong>](https://msdn.microsoft.com/library/windows/apps/br208964)</p></td>
<td align="left"><p>Se produit lorsqu’un pointeur est annulé par la plateforme.</p>
<ul>
<li>Les pointeurs tactiles sont annulés quand un stylet est détecté dans la plage de la surface d’entrée.</li>
<li>Aucun contact actif n’est détecté pendant plus de 100 ms.</li>
<li>Le moniteur/l’écran est modifié (résolution, paramètres, configuration à plusieurs écrans).</li>
<li>Le bureau est verrouillé ou l’utilisateur s’est déconnecté.</li>
<li>Le nombre de contacts simultanés a dépassé le nombre pris en charge par le périphérique.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerCaptureLost</strong>](https://msdn.microsoft.com/library/windows/apps/br208965)</p></td>
<td align="left"><p>Se produit lorsqu’un autre élément d’interface utilisateur capture le pointeur, lorsque le pointeur est libéré ou lorsqu’un autre pointeur est capturé par programme.</p>
<div class="alert">
<strong>Remarque</strong> Il n’existe aucun événement de capture de pointeur correspondant.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerEntered</strong>](https://msdn.microsoft.com/library/windows/apps/br208968)</p></td>
<td align="left"><p>Se produit lorsqu’un pointeur entre dans la zone de délimitation d’un élément. Cela peut se produire de manière légèrement différente pour les entrées par interaction tactile, pavé tactile, souris et stylet.</p>
<ul>
<li>Pour déclencher cet événement, l’interaction tactile requiert un contact du doigt directement sur l’élément ou par déplacement dans la zone de délimitation de l’élément.</li>
<li>La souris et le pavé tactile ont un curseur à l’écran qui est toujours visible et qui déclenche cet événement même si aucun bouton de la souris ni aucun bouton du pavé tactile n'est enfoncé.</li>
<li>Comme l’interaction tactile, le stylet déclenche cet événement par contact direct du stylet sur l’élément ou par déplacement dans la zone de délimitation de l’élément. Toutefois, le stylet présente aussi un état de pointage ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) qui, défini sur true, déclenche cet événement.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerExited</strong>](https://msdn.microsoft.com/library/windows/apps/br208969)</p></td>
<td align="left"><p>Se produit lorsqu’un pointeur quitte la zone de délimitation d’un élément. Cela peut se produire de manière légèrement différente pour les entrées par interaction tactile, pavé tactile, souris et stylet.</p>
<ul>
<li>L’interaction tactile nécessite un contact du doigt et déclenche cet événement lorsque le pointeur se déplace hors de la zone de délimitation de l’élément.</li>
<li>La souris et le pavé tactile ont un curseur à l’écran qui est toujours visible et qui déclenche cet événement même si aucun bouton de la souris ni aucun bouton du pavé tactile n'est enfoncé.</li>
<li>Comme l’interaction tactile, le stylet déclenche cet événement lorsqu’il sort de la zone de délimitation de l’élément. Toutefois, le stylet présente également un état de pointage ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) qui déclenche cet événement lorsque l’état passe de la valeur true à false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)</p></td>
<td align="left"><p>Se produit lorsqu’un pointeur change les coordonnées, l’état d’un bouton, la pression, l’inclinaison ou la géométrie de contact (par exemple, largeur et hauteur) dans la zone de délimitation d’un élément. Cela peut se produire de manière légèrement différente pour les entrées par interaction tactile, pavé tactile, souris et stylet.</p>
<ul>
<li>L’interaction tactile nécessite un contact du doigt et déclenche cet événement uniquement en cas de contact au sein de la zone de délimitation de l’élément.</li>
<li>La souris et le pavé tactile ont un curseur à l’écran qui est toujours visible et qui déclenche cet événement même si aucun bouton de la souris ni aucun bouton du pavé tactile n'est enfoncé.</li>
<li>Comme l’interaction tactile, le stylet déclenche cet événement en cas de contact au sein de la zone de délimitation de l’élément. Toutefois, le stylet présente également un état de pointage ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) qui, lorsqu’il est défini sur true et se trouve dans la zone de délimitation de l’élément, déclenche cet événement.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerPressed</strong>](https://msdn.microsoft.com/library/windows/apps/br208971)</p></td>
<td align="left"><p>Se produit lorsque le pointeur indique une action d’appui (par exemple, une pression par interaction tactile, sur un bouton de souris, sur un stylet ou sur un bouton du pavé tactile) dans la zone de délimitation d’un élément.</p>
<p>[<strong>CapturePointer</strong>](https://msdn.microsoft.com/library/windows/apps/br208918) doit être appelé par le gestionnaire de cet événement.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerReleased</strong>](https://msdn.microsoft.com/library/windows/apps/br208972)</p></td>
<td align="left"><p>Se produit lorsque le pointeur indique une action de libération (par exemple, arrêt de l’interaction tactile, relâchement du bouton de la souris, du stylet ou du bouton du pavé tactile) dans la zone de délimitation d’un élément ou, lorsque le pointeur est capturé, en dehors de la zone de délimitation.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerWheelChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208973)</p></td>
<td align="left"><p>Se produit lors de la rotation de la roulette de la souris.</p>
<p>L’entrée de la souris est associée à un seul pointeur affecté lors de la première détection de l’entrée de la souris. Cliquer sur un bouton de souris (gauche, roulette ou droit) crée une association secondaire entre le pointeur et ce bouton via l’événement [<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970).</p></td>
</tr>
</tbody>
</table>

 

## Exemple


Voici des exemples de code d’une application de base de suivi du pointeur. Ils montrent comment écouter et gérer les événements de pointeur, mais aussi comment obtenir différentes propriétés pour les pointeurs actifs.

### Créer l’interface utilisateur

Dans le cadre de cet exemple, nous utilisons un rectangle (`targetContainer`) comme objet cible de l’entrée du pointeur. La couleur de la cible change lorsque l’état du pointeur change.

Les détails pour chaque pointeur sont affichés dans un bloc de texte flottant qui se déplace avec le pointeur. Les événements de pointeur eux-mêmes sont affichés à gauche du rectangle (pour signaler la séquence d’événements).

Voici le code XAML (Extensible Application Markup Language) correspondant à cet exemple.

```XAML
<Page
    x:Class="PointerInput.MainPage"
    IsTabStop="false"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:PointerInput"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Name="page">

    <Grid Background="{StaticResource ApplicationForegroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="69.458" />
            <ColumnDefinition Width="80.542"/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="320" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Canvas Name="Container" 
                Grid.Column="0"
                Grid.Row="1"
                HorizontalAlignment="Center" 
                VerticalAlignment="Center" 
                Margin="245,0" 
                Height="320"  Width="640">
            <Rectangle Name="Target" 
                       Fill="#FF0000" 
                       Stroke="Black" 
                       StrokeThickness="0"
                       Height="320" Width="640" />
        </Canvas>
        <Button Name="buttonClear"
                Foreground="White"
                Width="100"
                Height="100">
            clear
        </Button>
        <TextBox Name="eventLog" 
                 Grid.Column="1"
                 Grid.Row="0"
                 Grid.RowSpan="3" 
                 Background="#000000" 
                 TextWrapping="Wrap" 
                 Foreground="#FFFFFF" 
                 ScrollViewer.VerticalScrollBarVisibility="Visible" 
                 BorderThickness="0" Grid.ColumnSpan="2"/>
    </Grid>
</Page>
```

### Écouter des événements de pointeur

Dans la plupart des cas, nous vous conseillons d’obtenir les informations sur le pointeur via la classe [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) du gestionnaire d’événements.

Si l’argument d’événement n’expose pas les détails du pointeur nécessaires, vous pouvez obtenir l’accès aux informations détaillées sur [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) via les méthodes [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) et [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

Dans le cadre de cet exemple, nous utilisons un rectangle (`targetContainer`) comme objet cible de l’entrée du pointeur. La couleur de la cible change lorsque l’état du pointeur change.

Le code suivant comporte la configuration de l’objet cible, la déclaration des variables globales et l’identification des divers détecteurs d’événements de pointeur relatifs à la cible.

```CSharp
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }

```

### Gérer des événements de pointeur

Nous allons maintenant utiliser le retour d’interface utilisateur dans le cadre de la démonstration des gestionnaires d’événements de pointeur de base.

-   Ce gestionnaire gère un événement [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). L’événement est ajouté au journal des événements, le pointeur est ajouté au tableau de pointeurs utilisé pour le suivi des pointeurs d’intérêt et les détails de pointeur sont affichés.

    **Remarque** Les événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) et [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) ne se produisent pas toujours ensemble. L’application doit écouter et gérer tout événement susceptible de conclure une action de pointeur appuyé (comme [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) et [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)).

     

```    CSharp
        // PointerPressed and PointerReleased events do not always occur in pairs. 
            // Your app should listen for and handle any event that might conclude a pointer down action 
            // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
            // For this example, we track the number of contacts in case the 
            // number of contacts has reached the maximum supported by the device.
            // Depending on the device, additional contacts might be ignored 
            // (PointerPressed not fired). 
            void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nDown: " + ptr.PointerId;

                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Lock the pointer to the target.
                Target.CapturePointer(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer captured: " + ptr.PointerId;

                // Check if pointer already exists (for example, enter occurred prior to press).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }
                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Ce gestionnaire gère un événement [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968). L’événement est ajouté au journal des événements, le pointeur est ajouté à la collection de pointeurs et les détails de pointeur sont affichés.

```    CSharp
        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nEntered: " + ptr.PointerId;

                if (contacts.Count == 0)
                {
                    // Change background color of target when pointer contact detected.
                    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
                }

                // Check if pointer already exists (if enter occurred prior to down).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }

                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Ce gestionnaire gère un événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970). L’événement est ajouté au journal des événements et les détails de pointeur sont mis à jour.

    **Important** L’entrée de souris est associée à un seul pointeur affecté lors de la première détection de l’entrée de souris. Cliquer sur un bouton de souris (gauche, roulette ou droit) crée une association secondaire entre le pointeur et ce bouton via l’événement [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). L’événement [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) est déclenché uniquement lorsque ce même bouton de souris est relâché (aucun autre bouton ne peut être associé au pointeur tant que cet événement n’est pas terminé). En raison de cette association exclusive, les autres clics de bouton de souris sont routés via l’événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970).

     

```    CSharp
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Multiple, simultaneous mouse button inputs are processed here.
        // Mouse input is associated with a single pointer assigned when 
        // mouse input is first detected. 
        // Clicking additional mouse buttons (left, wheel, or right) during 
        // the interaction creates secondary associations between those buttons 
        // and the pointer through the pointer pressed event. 
        // The pointer released event is fired only when the last mouse button 
        // associated with the interaction (not necessarily the initial button) 
        // is released. 
        // Because of this exclusive association, other mouse button clicks are 
        // routed through the pointer move event.          
        if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // To get mouse state, we need extended pointer details.
            // We get the pointer info through the getCurrentPoint method
            // of the event argument. 
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            if (ptrPt.Properties.IsLeftButtonPressed)
            {
                eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsMiddleButtonPressed)
            {
                eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsRightButtonPressed)
            {
                eventLog.Text += "\nRight button: " + ptrPt.PointerId;
            }
        }

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        updateInfoPop(e);
    }
```

-   Ce gestionnaire gère un événement [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973). L’événement est ajouté au journal des événements, le pointeur est ajouté au tableau de pointeurs (si nécessaire) et les détails de pointeur sont affichés.

```    CSharp
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

        // Check if pointer already exists (for example, enter occurred prior to wheel).
        if (contacts.ContainsKey(ptr.PointerId))
        {
            return;
        }

        // Add contact to dictionary.
        contacts[ptr.PointerId] = ptr;
        ++numActiveContacts;

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        createInfoPop(e);
    }
```

-   Ce gestionnaire gère un événement [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) lorsque le contact avec le numériseur est interrompu. L’événement est ajouté au journal des événements, le pointeur est supprimé de la collection de pointeurs et les détails de pointeur sont mis à jour.

```    CSharp
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nUp: " + ptr.PointerId;

        // If event source is mouse or touchpad and the pointer is still 
        // over the target, retain pointer and pointer details.
        // Return without removing pointer from pointers dictionary.
        // For this example, we assume a maximum of one mouse pointer.
        if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // Update target UI.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

            destroyInfoPop(ptr);

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Release the pointer from the target.
            Target.ReleasePointerCapture(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer released: " + ptr.PointerId;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }
        else
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
        }

    }
```

-   Ce gestionnaire gère un événement [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) lorsque le contact avec le numériseur est maintenu. L’événement est ajouté au journal des événements, le pointeur est supprimé du tableau de pointeurs et les détails de pointeur sont mis à jour.

```    CSharp
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer exited: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        // Update the UI and pointer details.
        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
        }
        
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Ce gestionnaire gère un événement [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964). L’événement est ajouté au journal des événements, le pointeur est supprimé du tableau de pointeurs et les détails de pointeur sont mis à jour.

```    CSharp
// Fires for for various reasons, including: 
    //    - Touch contact canceled by pen coming into range of the surface.
    //    - The device doesn&#39;t report an active contact for more than 100ms.
    //    - The desktop is locked or the user logged off. 
    //    - The number of simultaneous contacts exceeded the number supported by the device.
    private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Ce gestionnaire gère un événement [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965). L’événement est ajouté au journal des événements, le pointeur est supprimé du tableau de pointeurs et les détails de pointeur sont mis à jour.

    **Remarque** [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) peut se produire au lieu de [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972). La capture de pointeur peut être interrompue pour diverses raisons.

     

```    CSharp
// Fires for for various reasons, including: 
    //    - User interactions
    //    - Programmatic capture of another pointer
    //    - Captured pointer was deliberately released
    // PointerCaptureLost can fire instead of PointerReleased. 
    private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

### Obtenir les propriétés du pointeur

Comme indiqué précédemment, vous devez obtenir les informations de pointeur les plus détaillées via un objet [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) obtenu par le biais des méthodes [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) et [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

-   Nous commençons par créer un nouvel élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) pour chaque pointeur.

```    CSharp
        void createInfoPop(PointerRoutedEventArgs e)
            {
                TextBlock pointerDetails = new TextBlock();
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                pointerDetails.Name = ptrPt.PointerId.ToString();
                pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                pointerDetails.Text = queryPointer(ptrPt);

                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;

                Container.Children.Add(pointerDetails);
            }
```

-   Ensuite, nous fournissons un moyen de mettre à jour les informations de pointeur dans un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) existant associé au pointeur.

```    CSharp
        void updateInfoPop(PointerRoutedEventArgs e)
            {
                foreach (var pointerDetails in Container.Children)
                {
                    if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                    {
                        TextBlock _TextBlock = (TextBlock)pointerDetails;
                        if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                        {
                            // To get pointer location details, we need extended pointer info.
                            // We get the pointer info through the getCurrentPoint method
                            // of the event argument. 
                            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                            TranslateTransform x = new TranslateTransform();
                            x.X = ptrPt.Position.X + 20;
                            x.Y = ptrPt.Position.Y + 20;
                            pointerDetails.RenderTransform = x;
                            _TextBlock.Text = queryPointer(ptrPt);
                        }
                    }
                }
            }
```

-   Pour finir, diverses propriétés de pointeur font l’objet d’une requête.

```    CSharp
         String queryPointer(PointerPoint ptrPt)
             {
                 String details = "";

                 switch (ptrPt.PointerDevice.PointerDeviceType)
                 {
                     case Windows.Devices.Input.PointerDeviceType.Mouse:
                         details += "\nPointer type: mouse";
                         break;
                     case Windows.Devices.Input.PointerDeviceType.Pen:
                         details += "\nPointer type: pen";
                         if (ptrPt.IsInContact)
                         {
                             details += "\nPressure: " + ptrPt.Properties.Pressure;
                             details += "\nrotation: " + ptrPt.Properties.Orientation;
                             details += "\nTilt X: " + ptrPt.Properties.XTilt;
                             details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                             details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
                         }
                         break;
                     case Windows.Devices.Input.PointerDeviceType.Touch:
                         details += "\nPointer type: touch";
                         details += "\nrotation: " + ptrPt.Properties.Orientation;
                         details += "\nTilt X: " + ptrPt.Properties.XTilt;
                         details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                         break;
                     default:
                         details += "\nPointer type: n/a";
                         break;
                 }

                 GeneralTransform gt = Target.TransformToVisual(page);
                 Point screenPoint;

                 screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
                 details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                     "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                     "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
                 return details;
             }
```

### Exemple complet

Le code C# correspondant à cet exemple est indiqué ci-après. Pour des liens vers des exemples plus complexes, voir la section Articles connexes, en bas de cette page.

```CSharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Input;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=234238

namespace PointerInput
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }


        // PointerPressed and PointerReleased events do not always occur in pairs. 
        // Your app should listen for and handle any event that might conclude a pointer down action 
        // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
        // For this example, we track the number of contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nDown: " + ptr.PointerId;

            // Change background color of target when pointer contact detected.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Lock the pointer to the target.
            Target.CapturePointer(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer captured: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to press).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }
            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Display pointer details.
            createInfoPop(e);
        }

        void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nUp: " + ptr.PointerId;

            // If event source is mouse or touchpad and the pointer is still 
            // over the target, retain pointer and pointer details.
            // Return without removing pointer from pointers dictionary.
            // For this example, we assume a maximum of one mouse pointer.
            if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // Update target UI.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

                destroyInfoPop(ptr);

                // Remove contact from dictionary.
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    contacts[ptr.PointerId] = null;
                    contacts.Remove(ptr.PointerId);
                    --numActiveContacts;
                }

                // Release the pointer from the target.
                Target.ReleasePointerCapture(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer released: " + ptr.PointerId;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;
            }
            else
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

        }

        private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Multiple, simultaneous mouse button inputs are processed here.
            // Mouse input is associated with a single pointer assigned when 
            // mouse input is first detected. 
            // Clicking additional mouse buttons (left, wheel, or right) during 
            // the interaction creates secondary associations between those buttons 
            // and the pointer through the pointer pressed event. 
            // The pointer released event is fired only when the last mouse button 
            // associated with the interaction (not necessarily the initial button) 
            // is released. 
            // Because of this exclusive association, other mouse button clicks are 
            // routed through the pointer move event.          
            if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // To get mouse state, we need extended pointer details.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                if (ptrPt.Properties.IsLeftButtonPressed)
                {
                    eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsMiddleButtonPressed)
                {
                    eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsRightButtonPressed)
                {
                    eventLog.Text += "\nRight button: " + ptrPt.PointerId;
                }
            }

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            updateInfoPop(e);
        }

        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nEntered: " + ptr.PointerId;

            if (contacts.Count == 0)
            {
                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

            // Check if pointer already exists (if enter occurred prior to down).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to wheel).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        // Fires for for various reasons, including: 
        //    - User interactions
        //    - Programmatic capture of another pointer
        //    - Captured pointer was deliberately released
        // PointerCaptureLost can fire instead of PointerReleased. 
        private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        // Fires for for various reasons, including: 
        //    - Touch contact canceled by pen coming into range of the surface.
        //    - The device doesn&#39;t report an active contact for more than 100ms.
        //    - The desktop is locked or the user logged off. 
        //    - The number of simultaneous contacts exceeded the number supported by the device.
        private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer exited: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Update the UI and pointer details.
            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
            }
            
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        void createInfoPop(PointerRoutedEventArgs e)
        {
            TextBlock pointerDetails = new TextBlock();
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            pointerDetails.Name = ptrPt.PointerId.ToString();
            pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
            pointerDetails.Text = queryPointer(ptrPt);

            TranslateTransform x = new TranslateTransform();
            x.X = ptrPt.Position.X + 20;
            x.Y = ptrPt.Position.Y + 20;
            pointerDetails.RenderTransform = x;

            Container.Children.Add(pointerDetails);
        }

        void destroyInfoPop(Windows.UI.Xaml.Input.Pointer ptr)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == ptr.PointerId.ToString())
                    {
                        Container.Children.Remove(pointerDetails);
                    }
                }
            }
        }

        void updateInfoPop(PointerRoutedEventArgs e)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                    {
                        // To get pointer location details, we need extended pointer info.
                        // We get the pointer info through the getCurrentPoint method
                        // of the event argument. 
                        Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                        TranslateTransform x = new TranslateTransform();
                        x.X = ptrPt.Position.X + 20;
                        x.Y = ptrPt.Position.Y + 20;
                        pointerDetails.RenderTransform = x;
                        _TextBlock.Text = queryPointer(ptrPt);
                    }
                }
            }
        }

         String queryPointer(PointerPoint ptrPt)
         {
             String details = "";

             switch (ptrPt.PointerDevice.PointerDeviceType)
             {
                 case Windows.Devices.Input.PointerDeviceType.Mouse:
                     details += "\nPointer type: mouse";
                     break;
                 case Windows.Devices.Input.PointerDeviceType.Pen:
                     details += "\nPointer type: pen";
                     if (ptrPt.IsInContact)
                     {
                         details += "\nPressure: " + ptrPt.Properties.Pressure;
                         details += "\nrotation: " + ptrPt.Properties.Orientation;
                         details += "\nTilt X: " + ptrPt.Properties.XTilt;
                         details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                         details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
                     }
                     break;
                 case Windows.Devices.Input.PointerDeviceType.Touch:
                     details += "\nPointer type: touch";
                     details += "\nrotation: " + ptrPt.Properties.Orientation;
                     details += "\nTilt X: " + ptrPt.Properties.XTilt;
                     details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                     break;
                 default:
                     details += "\nPointer type: n/a";
                     break;
             }

             GeneralTransform gt = Target.TransformToVisual(page);
             Point screenPoint;

             screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
             details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                 "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                 "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
             return details;
         }
    }
}
```

## Articles connexes


**Exemples**
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée : exemple d’événements d’entrée utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrée : exemple de fonctionnalités de périphériques](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple de manipulations et de mouvements (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Entrée : exemple de test de positionnement tactile](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : exemple d’entrée manuscrite simplifiée](http://go.microsoft.com/fwlink/p/?linkid=246570)
 

 







<!--HONumber=Jun16_HO4-->


