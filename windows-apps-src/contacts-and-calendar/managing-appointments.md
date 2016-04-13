---
L’espace de noms Windows.ApplicationModel.Appointments vous permet de créer et gérer des rendez-vous dans l’application Calendrier d’un utilisateur.
Gérer des rendez-vous
ms.assetid: 292E9249-07C3-4791-B32C-6EC153C2B538
---

# Gérer des rendez-vous

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


L’espace de noms [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/Dn263359) vous permet de créer et gérer des rendez-vous dans l’application Calendrier d’un utilisateur. Ici, nous allons vous montrer comment créer un rendez-vous, l’ajouter à l’application Calendrier, le remplacer dans l’application Calendrier et le supprimer de l’application Calendrier. Nous allons également vous montrer comment afficher une période de temps pour une application Calendrier et créer un objet appointment-recurrence.

## Créer un rendez-vous et lui ajouter des données

Créez un objet [**Windows.ApplicationModel.Appointments.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn297221) et affectez-le à une variable. Appliquez ensuite à **Appointment** les propriétés de rendez-vous fournies par un utilisateur dans l’interface utilisateur.

```cs
private void Create-Click(object sender, RoutedEventArgs e)
{
    bool isAppointmentValid = true;
    var appointment = new Windows.ApplicationModel.Appointments.Appointment();

    // StartTime
    var date = StartTimeDatePicker.Date;
    var time = StartTimeTimePicker.Time;
    var timeZoneOffset = TimeZoneInfo.Local.GetUtcOffset(DateTime.Now);
    var startTime = new DateTimeOffset(date.Year, date.Month, date.Day, time.Hours, time.Minutes, 0, timeZoneOffset);
    appointment.StartTime = startTime;

    // Subject
    appointment.Subject = SubjectTextBox.Text;

    if (appointment.Subject.Length &gt; 255)
    {
        isAppointmentValid = false;
        ResultTextBlock.Text = &quot;The subject cannot be greater than 255 characters.&quot;;
    }

    // Location
    appointment.Location = LocationTextBox.Text;

    if (appointment.Location.Length &gt; 32768)
    {
        isAppointmentValid = false;
        ResultTextBlock.Text = &quot;The location cannot be greater than 32,768 characters.&quot;;
    }

    // Details
    appointment.Details = DetailsTextBox.Text;

    if (appointment.Details.Length &gt; 1073741823)
    {
        isAppointmentValid = false;
        ResultTextBlock.Text = &quot;The details cannot be greater than 1,073,741,823 characters.&quot;;
    }

    // Duration
    if (DurationComboBox.SelectedIndex == 0)
    {
        // 30 minute duration is selected
        appointment.Duration = TimeSpan.FromMinutes(30);
    }
    else
    {
        // 1 hour duration is selected
        appointment.Duration = TimeSpan.FromHours(1);
    }

    // All Day
    appointment.AllDay = AllDayCheckBox.IsChecked.Value;

    // Reminder
    if (ReminderCheckBox.IsChecked.Value)
    {
        switch (ReminderComboBox.SelectedIndex)
        {
            case 0:
                appointment.Reminder = TimeSpan.FromMinutes(15);
                break;
            case 1:
                appointment.Reminder = TimeSpan.FromHours(1);
                break;
            case 2:
                appointment.Reminder = TimeSpan.FromDays(1);
                break;
        }
    }

    //Busy Status
    switch (BusyStatusComboBox.SelectedIndex)
    {
        case 0:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.Busy;
            break;
        case 1:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.Tentative;
            break;
        case 2:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.Free;
            break;
        case 3:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.OutOfOffice;
            break;
        case 4:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.WorkingElsewhere;
            break;
    }

    // Sensitivity
    switch (SensitivityComboBox.SelectedIndex)
    {
        case 0:
            appointment.Sensitivity = Windows.ApplicationModel.Appointments.AppointmentSensitivity.Public;
            break;
        case 1:
            appointment.Sensitivity = Windows.ApplicationModel.Appointments.AppointmentSensitivity.Private;
            break;
    }

    // Uri
    if (UriTextBox.Text.Length &gt; 0)
    {
        try
        {
            appointment.Uri = new System.Uri(UriTextBox.Text);
        }
        catch (Exception)
        {
            isAppointmentValid = false;
            ResultTextBlock.Text = &quot;The Uri provided is invalid.&quot;;
        }
    }

    // Organizer
    // Note: Organizer can only be set if there are no invitees added to this appointment.
    if (OrganizerRadioButton.IsChecked.Value)
    {
        var organizer = new Windows.ApplicationModel.Appointments.AppointmentOrganizer();

        // Organizer Display Name
        organizer.DisplayName = OrganizerDisplayNameTextBox.Text;

        if (organizer.DisplayName.Length &gt; 256)
        {
            isAppointmentValid = false;
            ResultTextBlock.Text = &quot;The organizer display name cannot be greater than 256 characters.&quot;;
        }
        else
        {
            // Organizer Address (e.g. Email Address)
            organizer.Address = OrganizerAddressTextBox.Text;

            if (organizer.Address.Length &gt; 321)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = &quot;The organizer address cannot be greater than 321 characters.&quot;;
            }
            else if (organizer.Address.Length == 0)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = &quot;The organizer address must be greater than 0 characters.&quot;;
            }
            else
            {
                appointment.Organizer = organizer;
            }
        }
    }

    // Invitees
    // Note: If the size of the Invitees list is not zero, then an Organizer cannot be set.
    if (InviteeRadioButton.IsChecked.Value)
    {
        var invitee = new Windows.ApplicationModel.Appointments.AppointmentInvitee();

        // Invitee Display Name
        invitee.DisplayName = InviteeDisplayNameTextBox.Text;

        if (invitee.DisplayName.Length &gt; 256)
        {
            isAppointmentValid = false;
            ResultTextBlock.Text = &quot;The invitee display name cannot be greater than 256 characters.&quot;;
        }
        else
        {
            // Invitee Address (e.g. Email Address)
            invitee.Address = InviteeAddressTextBox.Text;

            if (invitee.Address.Length &gt; 321)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = &quot;The invitee address cannot be greater than 321 characters.&quot;;
            }
            else if (invitee.Address.Length == 0)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = &quot;The invitee address must be greater than 0 characters.&quot;;
            }
            else
            {
                // Invitee Role
                switch (RoleComboBox.SelectedIndex)
                {
                    case 0:
                        invitee.Role = Windows.ApplicationModel.Appointments.AppointmentParticipantRole.RequiredAttendee;
                        break;
                    case 1:
                        invitee.Role = Windows.ApplicationModel.Appointments.AppointmentParticipantRole.OptionalAttendee;
                        break;
                    case 2:
                        invitee.Role = Windows.ApplicationModel.Appointments.AppointmentParticipantRole.Resource;
                        break;
                }

                // Invitee Response
                switch (ResponseComboBox.SelectedIndex)
                {
                    case 0:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.None;
                        break;
                    case 1:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Tentative;
                        break;
                    case 2:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Accepted;
                        break;
                    case 3:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Declined;
                        break;
                    case 4:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Unknown;
                        break;
                }

                appointment.Invitees.Add(invitee);
            }
        }
    }

    if (isAppointmentValid)
    {
        ResultTextBlock.Text = &quot;The appointment was created successfully and is valid.&quot;;
    }
}
```

## Ajouter un rendez-vous au calendrier d’un utilisateur

Créez un objet [**Windows.ApplicationModel.Appointments.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn297221) et affectez-le à une variable. Appelez ensuite la méthode [**AppointmentManager.ShowAddAppointmentAsync(Appointment, Rect, Placement)**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showaddappointmentasync-253292089) pour afficher l’interface utilisateur add-appointment du fournisseur de rendez-vous par défaut, pour permettre à l’utilisateur d’ajouter un rendez-vous. Si l’utilisateur a cliqué sur **Ajouter**, l’exemple affiche l’identificateur du rendez-vous renvoyé par **ShowAddAppointmentAsync**.

```cs
private async void Add-Click(object sender, RoutedEventArgs e)
{
    // Create an Appointment that should be added the user's appointments provider app.
    var appointment = new Windows.ApplicationModel.Appointments.Appointment();

    // Get the selection rect of the button pressed to add this appointment
    var rect = GetElementRect(sender as FrameworkElement);

    // ShowAddAppointmentAsync returns an appointment id if the appointment given was added to the user's calendar.
    // This value should be stored in app data and roamed so that the appointment can be replaced or removed in the future.
    // An empty string return value indicates that the user canceled the operation before the appointment was added.
    String appointmentId = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowAddAppointmentAsync(
                           appointment, rect, Windows.UI.Popups.Placement.Default);
    if (appointmentId != String.Empty)
    {
        ResultTextBlock.Text = "Appointment Id: " + appointmentId;
    }
    else
    {
        ResultTextBlock.Text = "Appointment not added.";
    }
}
```

**Remarque** Dans les applications du Windows Phone Store, l’élément [**ShowAddAppointment**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showaddappointmentasync) fonctionne comme [**ShowEditNewAppointment**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showeditnewappointmentasync), c’est-à-dire que la boîte de dialogue affichée pour ajouter le rendez-vous est modifiable.

## Remplacer un rendez-vous dans le calendrier d’un utilisateur

Créez un objet [**Windows.ApplicationModel.Appointments.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn297221) et affectez-le à une variable. Appelez ensuite la méthode [**AppointmentManager.ShowReplaceAppointmentAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showreplaceappointmentasync) appropriée pour afficher l’interface utilisateur replace-appointment du fournisseur de rendez-vous par défaut, pour permettre à l’utilisateur de remplacer un rendez-vous. L’utilisateur fournit également l’identificateur du rendez-vous qu’il souhaite remplacer. Cet identificateur a été renvoyé à partir de [**AppointmentManager.ShowAddAppointmentAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showaddappointmentasync). Si l’utilisateur a cliqué sur **Remplacer**, l’exemple indique que l’identificateur du rendez-vous a été mis à jour.

```cs
private async void Replace-Click(object sender, RoutedEventArgs e)
{
    // The appointment id argument for ReplaceAppointmentAsync is typically retrieved from AddAppointmentAsync and stored in app data.
    String appointmentIdOfAppointmentToReplace = AppointmentIdTextBox.Text;

    if (String.IsNullOrEmpty(appointmentIdOfAppointmentToReplace))
    {
        ResultTextBlock.Text = &quot;The appointment id cannot be empty&quot;;
    }
    else
    {
        // The Appointment argument for ReplaceAppointmentAsync should contain all of the Appointment' s properties including those that may have changed.
        var appointment = new Windows.ApplicationModel.Appointments.Appointment();

        // Get the selection rect of the button pressed to replace this appointment
        var rect = GetElementRect(sender as FrameworkElement);

        // ReplaceAppointmentAsync returns an updated appointment id when the appointment was successfully replaced.
        // The updated id may or may not be the same as the original one retrieved from AddAppointmentAsync.
        // An optional instance start time can be provided to indicate that a specific instance on that date should be replaced
        // in the case of a recurring appointment.
        // If the appointment id returned is an empty string, that indicates that the appointment was not replaced.
        String updatedAppointmentId;
        if (InstanceStartDateCheckBox.IsChecked.Value)
        {
            // Replace a specific instance starting on the date provided.
            var instanceStartDate = InstanceStartDateDatePicker.Date;
            updatedAppointmentId = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowReplaceAppointmentAsync(
                                   appointmentIdOfAppointmentToReplace, appointment, rect, Windows.UI.Popups.Placement.Default, instanceStartDate);
        }
        else
        {
            // Replace an appointment that occurs only once or in the case of a recurring appointment, replace the entire series.
            updatedAppointmentId = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowReplaceAppointmentAsync(
                                   appointmentIdOfAppointmentToReplace, appointment, rect, Windows.UI.Popups.Placement.Default);
        }

        if (updatedAppointmentId != String.Empty)
        {
            ResultTextBlock.Text = &quot;Updated Appointment Id: &quot; + updatedAppointmentId;
        }
        else
        {
            ResultTextBlock.Text = &quot;Appointment not replaced.&quot;;
        }
    }
}
```

## Supprimer un rendez-vous du calendrier d’un utilisateur

Appelez la méthode [**AppointmentManager.ShowRemoveAppointmentAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showremoveappointmentasync) appropriée pour afficher l’interface utilisateur remove-appointment du fournisseur de rendez-vous par défaut, pour permettre à l’utilisateur de supprimer un rendez-vous. L’utilisateur fournit également l’identificateur du rendez-vous qu’il souhaite supprimer. Cet identificateur a été renvoyé à partir de [**AppointmentManager.ShowAddAppointmentAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showaddappointmentasync). Si l’utilisateur a cliqué sur **Supprimer**, l’exemple indique que le rendez-vous spécifié par cet identificateur du rendez-vous a été supprimé.

```cs
private async void Remove-Click(object sender, RoutedEventArgs e)
{
    // The appointment id argument for ShowRemoveAppointmentAsync is typically retrieved from AddAppointmentAsync and stored in app data.
    String appointmentId = AppointmentIdTextBox.Text;

    // The appointment id cannot be null or empty.
    if (String.IsNullOrEmpty(appointmentId))
    {
        ResultTextBlock.Text = &quot;The appointment id cannot be empty&quot;;
    }
    else
    {
        // Get the selection rect of the button pressed to remove this appointment
        var rect = GetElementRect(sender as FrameworkElement);

        // ShowRemoveAppointmentAsync returns a boolean indicating whether or not the appointment related to the appointment id given was removed.
        // An optional instance start time can be provided to indicate that a specific instance on that date should be removed
        // in the case of a recurring appointment.
        bool removed;
        if (InstanceStartDateCheckBox.IsChecked.Value)
        {
            // Remove a specific instance starting on the date provided.
            var instanceStartDate = InstanceStartDateDatePicker.Date;
            removed = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowRemoveAppointmentAsync(
                      appointmentId, rect, Windows.UI.Popups.Placement.Default, instanceStartDate);
        }
        else
        {
            // Remove an appointment that occurs only once or in the case of a recurring appointment, replace the entire series.
            removed = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowRemoveAppointmentAsync(
                      appointmentId, rect, Windows.UI.Popups.Placement.Default);
        }

        if (removed)
        {
            ResultTextBlock.Text = &quot;Appointment removed&quot;;
        }
        else
        {
            ResultTextBlock.Text = &quot;Appointment not removed&quot;;
        }
    }
}
```

## Afficher une période de temps pour le fournisseur de rendez-vous

Appelez la méthode [**AppointmentManager.ShowTimeFrameAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showtimeframeasync) pour afficher une période de temps spécifique pour l’interface utilisateur principale du fournisseur de rendez-vous par défaut si l’utilisateur a cliqué sur **Afficher**. L’exemple indique que le fournisseur de rendez-vous par défaut s’affiche à l’écran.

```cs 
private async void Show-Click(object sender, RoutedEventArgs e)
{
    var dateToShow = new DateTimeOffset(2015, 6, 12, 18, 32, 0, 0, TimeSpan.FromHours(-8));
    var duration = TimeSpan.FromHours(1);
    await Windows.ApplicationModel.Appointments.AppointmentManager.ShowTimeFrameAsync(dateToShow, duration);
    ResultTextBlock.Text = &quot;The default appointments provider should have appeared on screen.&quot;;
}
```

## Créer un objet appointment-recurrence et lui ajouter des données

Créez un objet [**Windows.ApplicationModel.Appointments.AppointmentRecurrence**](https://msdn.microsoft.com/library/windows/apps/Dn297221recurrence) et affectez-le à une variable. Appliquez ensuite à **AppointmentRecurrence** les propriétés de périodicité fournies par un utilisateur dans l’interface utilisateur.

```cs
private void Create-Click(object sender, RoutedEventArgs e)
{
    bool isRecurrenceValid = true;
    var recurrence = new Windows.ApplicationModel.Appointments.AppointmentRecurrence();

    // Unit
    switch (UnitComboBox.SelectedIndex)
    {
        case 0:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Daily;
            break;
        case 1:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Weekly;
            break;
        case 2:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Monthly;
            break;
        case 3:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.MonthlyOnDay;
            break;
        case 4:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Yearly;
            break;
        case 5:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.YearlyOnDay;
            break;
    }

    // Occurrences
    // Note: Occurrences and Until properties are mutually exclusive.
    if (OccurrencesRadioButton.IsChecked.Value)
    {
        recurrence.Occurrences = (uint)OccurrencesSlider.Value;
    }

    // Until
    // Note: Until and Occurrences properties are mutually exclusive.
    if (UntilRadioButton.IsChecked.Value)
    {
        recurrence.Until = UntilDatePicker.Date;
    }

    // Interval
    recurrence.Interval = (uint)IntervalSlider.Value;

    // Week of the month
    switch (WeekOfMonthComboBox.SelectedIndex)
    {
        case 0:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.First;
            break;
        case 1:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Second;
            break;
        case 2:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Third;
            break;
        case 3:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Fourth;
            break;
        case 4:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Last;
            break;
    }

    // Days of the Week
    // Note: For Weekly, MonthlyOnDay or YearlyOnDay recurrence unit values, at least one day must be specified.
    if (SundayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Sunday; }
    if (MondayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Monday; }
    if (TuesdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Tuesday; }
    if (WednesdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Wednesday; }
    if (ThursdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Thursday; }
    if (FridayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Friday; }
    if (SaturdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Saturday; }

    if (((recurrence.Unit == Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Weekly) ||
         (recurrence.Unit == Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.MonthlyOnDay) ||
         (recurrence.Unit == Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.YearlyOnDay)) &amp;&amp;
        (recurrence.DaysOfWeek == Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.None))
    {
        isRecurrenceValid = false;
        ResultTextBlock.Text = &quot;The recurrence specified is invalid. For Weekly, MonthlyOnDay or YearlyOnDay recurrence unit values, 
                                at least one day must be specified.&quot;;
    }

    // Month of the year
    recurrence.Month = (uint)MonthSlider.Value;

    // Day of the month
    recurrence.Day = (uint)DaySlider.Value;

    if (isRecurrenceValid)
    {
        ResultTextBlock.Text = &quot;The recurrence specified was created successfully and is valid.&quot;;
    }
}
```

## Ajouter un rendez-vous modifiable

[
            **ShowEditNewAppointmentAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showeditnewappointmentasync) fonctionne comme [**ShowAddAppointmentAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showaddappointmentasync), sauf que la boîte de dialogue qui permet d’ajouter le rendez-vous est modifiable. Ainsi, l’utilisateur peut changer les données du rendez-vous avant de les enregistrer.

``` cs
private async void AddAndEdit-Click(object sender, RoutedEventArgs e)
{
    // Create an Appointment that should be added the user' s appointments provider app.
    var appointment = new Windows.ApplicationModel.Appointments.Appointment();

    appointment.StartTime = DateTime.Now + TimeSpan.FromDays(1);
    appointment.Duration = TimeSpan.FromHours(1);
    appointment.Location = &quot;Meeting location&quot;;
    appointment.Subject = &quot;Meeting subject&quot;;
    appointment.Details = &quot;Meeting description&quot;;
    appointment.Reminder = TimeSpan.FromMinutes(15); // Remind me 15 minutes prior


    // ShowAddAppointmentAsync returns an appointment id if the appointment given was added to the user' s calendar.
    // This value should be stored in app data and roamed so that the appointment can be replaced or removed in the future.
    // An empty string return value indicates that the user canceled the operation before the appointment was added.
    String appointmentId =
        await Windows.ApplicationModel.Appointments.AppointmentManager.ShowEditNewAppointmentAsync(appointment);
    
    if (appointmentId != String.Empty)
    {
        ResultTextBlock.Text = &quot;Appointment Id: &quot; + appointmentId;
    }
    else
    {
        ResultTextBlock.Text = &quot;Appointment not added.&quot;;
    }
}
```

## Afficher les détails des rendez-vous

[
            **ShowAppointmentDetailsAsync**](https://msdn.microsoft.com/library/windows/apps/Dn297221manager-showappointmentdetailsasync) indique au système d’afficher les détails relatifs au rendez-vous spécifié. Une application qui implémente des calendriers d’application peut s’activer pour afficher les détails des rendez-vous dans ses propres calendriers. Sinon, le système affiche les détails des rendez-vous. Une surcharge de la méthode qui accepte un argument de date de début est fournie pour afficher les détails d’une instance de rendez-vous périodique.

```cs
private async void ShowAppointmentDetails-Click(object sender, RoutedEventArgs e)
{

    if (instanceStartTime == null)
    {
        await Windows.ApplicationModel.Appointments.AppointmentManager.ShowAppointmentDetailsAsync(
            currentAppointment.LocalId);
    }
    else
    {
        // Specify a start time to show an instance of a recurring appointment
        await Windows.ApplicationModel.Appointments.AppointmentManager.ShowAppointmentDetailsAsync(
            currentAppointment.LocalId, instanceStartTime);
    }

}
```

## Récapitulatif et étapes suivantes

Vous disposez désormais des notions de base pour gérer les rendez-vous. Téléchargez les [Exemples d’applications Windows universelles](http://go.microsoft.com/fwlink/p/?linkid=619979) dans GitHub pour voir d’autres exemples illustrant comment gérer des rendez-vous.

## Rubriques connexes

* [Exemple d’API de rendez-vous](http://go.microsoft.com/fwlink/p/?linkid=309836)
 

 






<!--HONumber=Mar16_HO1-->


