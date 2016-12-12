---
author: mcleanbyron
Description: In this walkthrough, you will create, run, and manage your first experiment with A/B testing.
title: Create and run your first experiment with A/B testing
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 65785e53b2e5ba788dbda533373737e193e1c792

---

# <a name="create-and-run-your-first-experiment-with-ab-testing"></a>Create and run your first experiment with A/B testing

In this walkthrough, you will:
* Create an experimentation [project](run-app-experiments-with-a-b-testing.md#terms) on the Dev Center dashboard that defines several remote variables that represent the text and color of an app button.
* Create an app with code that retrieves the remote variable values, uses this data to change the background color of a button, and logs view and conversion event data back to Dev Center.
* Create an experiment in the project to test whether changing the background color of the app button successfully increases the number of button clicks.
* Run the app to gather experiment data.
* Review the experiment results on the Dev Center dashboard, choose a variation to enable for all users of the app, and complete the experiment.

For an overview of A/B testing with Dev Center, see [Run app experiments with A/B testing](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Prerequisites

To follow this walkthrough, you must have a Windows Dev Center account and you must configure your development computer as described in [Run app experiments with A/B testing](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-windows-dev-center"></a>Create a project with remote variables in Windows Dev Center

1. Sign in to the [Dev Center dashboard](https://dev.windows.com/overview).
2. If you already have an app in Dev Center that you want to use to create an experiment, select that app in your dashboard. If you do not yet have an app in your dashboard, [create a new app by reserving a name](../publish/create-your-app-by-reserving-a-name.md) and then select that app in your dashboard.
3. In the navigation pane, click **Services** and then click **Experimentation**.
4. In the **Projects** section of the next page, click the **New project** button.
5. In the **New project** page, enter the name **Button Click Experiments** for your new project.
6. Expand the **Remote variables** section and click **Add variable** four times. You should now have four empty variable rows.
  * In the first row, type **buttonText** for the variable name and type **Grey Button** in the **Default value** column.
  * In the second row, type **r** for the variable name and type **128** in the **Default value** column.
  * In the third row, type **g** for the variable name and type **128** in the **Default value** column.
  * In the fourth row, type **b** for the variable name and type **128** in the **Default value** column.
7. Click **Save** and make note of the [project ID](run-app-experiments-with-a-b-testing.md#terms) value that appears in the **SDK integration** section. In the next section, you will update your app code and reference this value in your code.

## <a name="code-the-experiment-in-your-app"></a>Code the experiment in your app

1. In Visual Studio 2015, create a new Universal Windows Platform project using Visual C#. Name the project **SampleExperiment**.
2. In Solution Explorer, expand your project node, right-click **References**, and click **Add Reference**.
3. In **Reference Manager**, expand **Universal Windows** and click **Extensions**.
4. In the list of SDKs, select the check box next to **Microsoft Engagement Framework** and click **OK**.
5. In **Solution Explorer**, double-click MainPage.xaml to open the designer for the main page in the app.
6. Drag a **Button** from **Toolbox** to the page.
7. Double-click the button on the designer to open the code file and add an event handler for the **Click** event.  
8. Replace the entire contents of the code file with the following code. Assign the ```projectId``` variable to the [project ID](run-app-experiments-with-a-b-testing.md#terms) value that you obtained from the Dev Center dashboard in the previous section.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

10. Save the code file and build the project.

## <a name="create-the-experiment-in-windows-dev-center"></a>Create the experiment in Windows Dev Center

1. Return to the **Button Click Experiments** project page in the Windows Dev Center dashboard.
2. In the **Experiments** section, click the **New experiment** button.
5. In the **Experiment details** section, type the name **Optimize Button Clicks** in the **Experiment name** field.
6. In the **View event** section, type **userViewedButton** in the **View event name** field. Note that this name matches the view event string that you logged in the code you added in the previous section.
7. In the **Goals and conversion events** section, enter the following values:
  * In the **Goal name** field, type **Increase Button Clicks**.
  * In the **Conversion event name** field, type the name **userClickedButton**. Note that this name matches the conversion event string that you logged in the code you added in the previous section.
  * In the **Objective** field, choose **Maximize**.
8. In the **Remote variables and variations** section, confirm that the **Distribute equally** check box is selected so that the variations will be distributed equally to your app.
9. Add variables to your experiment:
  9. Click the drop-down control, choose **buttonText**, and click **Add variable**. The string **Grey Button** should automatically appear in the **Variation A** column (this value is derived from the project settings). In the **Variation B** column, type **Blue Button**.
  9. Click the drop-down control again, choose **r**, and click **Add variable**. The string **128** should automatically appear in the **Variation A** column. In the **Variation B** column, type **1**.
  9. Click the drop-down control again, choose **g**, and click **Add variable**. The string **128** should automatically appear in the **Variation A** column. In the **Variation B** column, type **1**.  
  9. Click the drop-down control again, choose **b**, and click **Add variable**. The string **128** should automatically appear in the **Variation A** column. In the **Variation B** column, type **255**.  
10. Click **Save** and then click **Activate**.

> **Important**&nbsp;&nbsp;After you activate an experiment, you can no longer modify the experiment parameters unless it you clicked the **Editable experiment** check box when you created the experiment. Typically, we recommend that you code the experiment in your app before activating your experiment.

## <a name="run-the-app-to-gather-experiment-data"></a>Run the app to gather experiment data

1. Run the **SampleExperiment** app you created earlier.
2. Confirm that you see either a grey or blue button. Click the button and then close the app.
3. Repeat the above steps several times on the same computer to confirm that your app shows the same button color.

## <a name="review-the-results-and-complete-the-experiment"></a>Review the results and complete the experiment

Wait at least several hours after completing the previous section, and then follow these steps to review the results of your experiment and complete the experiment.

> **Note**&nbsp;&nbsp;As soon as you activate an experiment, Dev Center immediately starts collecting data from any apps that are instrumented to log data for your experiment. However, it can take several hours for experiment data to appear in the dashboard.

1. In Dev Center, return to the **Experimentation** page for your app.
2. In the **Active experiments** section, click **Optimize Button Clicks** to go to the page for this experiment.
3. Confirm that the results shown in the **Results summary** and **Results details** sections matches what you expect to see. For more details about these sections, see [Manage your experiment in the Dev Center dashboard](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Note**&nbsp;&nbsp;Dev Center reports only the first conversion event for each user in a 24-hour time period. If a user triggers multiple conversion events in your app within a 24-hour period, only the first conversion event is reported. This is intended to help prevent a single user with many conversion events from skewing the experiment results for a sample group of users.
4. Now you are ready to end the experiment. In the **Results summary** section, in the **Variation B** column, click **Switch**. This switches all users of your app to the blue button.
5. Click **OK** to confirm that you want to end the experiment.
6. Run the **SampleExperiment** app you created in the previous section.
7. Confirm that you see a blue button. Note that it may take up to two minutes for your app to receive an updated variation assignment.

## <a name="related-topics"></a>Related topics

* [Create a project and define remote variables in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Code your app for experimentation](code-your-experiment-in-your-app.md)
* [Define your experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Manage your experiment in the Dev Center dashboard](manage-your-experiment.md)
* [Run app experiments with A/B testing](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Dec16_HO1-->


