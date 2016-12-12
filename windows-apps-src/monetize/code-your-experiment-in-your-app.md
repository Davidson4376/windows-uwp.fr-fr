---
author: mcleanbyron
Description: To run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must code the experiment in your app.
title: Code your app for experimentation
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: cc32e2688bce636e1f4bda02aade4ed1d94f3e28

---

# <a name="code-your-app-for-experimentation"></a>Code your app for experimentation

After you [create a project and define remote variables in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), you are ready to update the code in your Universal Windows Platform (UWP) app to:
* Receive remote variable values from Windows Dev Center.
* Use remote variables to configure app experiences for your users.
* Log events to Dev Center that indicate when users have viewed your experiment and performed a desired action (also called a *conversion*).

To add this behavior to your app, you'll use APIs provided by the Microsoft Store Services SDK.

The following sections describe the general process of getting variations for your experiment and logging events to Dev Center. After you code your app for experimentation, you can [define an experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md). For a walkthrough that demonstrates the end-to-end process of creating and running an experiment, see [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md).

>**Note**&nbsp;&nbsp;Some of the experimentation APIs in the Windows Store Services SDK use the [asynchronous pattern](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) to retrieve data from Dev Center. This means that part of the execution of these methods may take place after the methods are invoked, so your app's UI can remain responsive while the operations complete. The asynchronous pattern requires your app to use the **async** keyword and **await** operator when calling the APIs, as demonstrated by the code examples in this article. By convention, asynchronous methods end with **Async**.

## <a name="configure-your-project"></a>Configure your project

To get started, install the Microsoft Store Services SDK on your development computer and add the necessary references to your project.

1. [Install the Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Open your project in Visual Studio.
3. In Solution Explorer, expand your project node, right-click **References**, and click **Add Reference**.
3. In **Reference Manager**, expand **Universal Windows** and click **Extensions**.
4. In the list of SDKs, select the check box next to **Microsoft Engagement Framework** and click **OK**.

>**Note**&nbsp;&nbsp;The code examples in this article assume that your code file has **using** statements for the **System.Threading.Tasks** and **Microsoft.Services.Store.Engagement** namespaces.

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Get variation data and log the view event for your experiment

In your project, locate the code for the feature that you want to modify in your experiment. Add code that retrieves data for a variation, use this data to modify the behavior of the feature you are testing, and then log the view event for your experiment to the A/B testing service in Dev Center.

The specific code you need will depend on your app, but the following example demonstrates the basic process. For a complete code example, see [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!div class="tabbedCodeSnippets"]
[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

The following steps describe the important parts of this process in detail.

1. Declare a [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object that represents the current variation assignment and a [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) object that you will use to log view and conversion events to Dev Center.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

1. Declare a string variable that is assigned to the [project ID](run-app-experiments-with-a-b-testing.md#terms) for the experiment you want to retrieve.
  >**Note**&nbsp;&nbsp;You obtain a project ID when you [create a project in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). The project ID shown below is for example purposes only.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

2. Get the current cached variation assignment for your experiment by calling the static [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) method, and pass the project ID for your experiment to the method. This method returns a [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) object that provides access to the variation assignment via the [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx) property.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Check the [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) property to determine whether the cached variation assignment needs to be refreshed with a remote variation assignment from the server. If it does need to be refreshed, call the static [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) method to check for an updated variation assignment from the server and refresh the local cached variation.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Use the [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx), [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx), or [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) methods of the [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object to get the values for the variation assignment. In each method, the first parameter is the name of the variation that you want to retrieve (this is the same name of a variation that you enter in your Dev Center dashboard). The second parameter is the default value that the method should return if it is not able to retrieve the specified value from Dev Center (for example, if there is no network connectivity), and a cached version of the variation is not available.

  The following example uses [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) to get a variable named *buttonText* and specifies a default variable value of **Grey Button**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

4. In your code, use the variable values to modify the behavior of the feature you are testing. For example, the following code assigns the *buttonText* value to the content of a button in your app. This example assumes you have already defined this button elsewhere in your project.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

5. Finally, log the [view event](run-app-experiments-with-a-b-testing.md#terms) for your experiment to the A/B testing service in Dev Center. Initialize the ```logger``` field to a [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) object and call the [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) method. Pass the [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object that represents the current variation assignment (this object provides context about the event to Dev Center) and the name of the view event for your experiment. This must match the view event name that you enter for your experiment in the Dev Center dashboard. Your code should log the view event when the user starts viewing a variation that is part of your experiment.

  The following example shows how to log a view event named **userViewedButton**. In this example, the goal of the experiment is to get the user to click a button in the app, so the view event is logged after the app has retrieved the variation data (in this case, the button text) and assigned it to the content of the button.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-dev-center"></a>Log conversion events to Dev Center

Next, add code that logs [conversion events](run-app-experiments-with-a-b-testing.md#terms) to the A/B testing service in Dev Center. Your code should log a conversion event when the user reaches an objective for your experiment. The specific code you need will depend on your app, but here are the general steps. For a complete code example, see [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md).

1. In code that runs when the user reaches an objective for one of the goals of the experiment, call the [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) method again and pass the [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object and the name of a conversion event for your experiment. This must match one of the conversion event names that you enter for your experiment in the Dev Center dashboard.

  The following example logs a conversion event named **userClickedButton** from the **Click** event handler for a button. In this example, the goal of the experiment is to get the user to click the button.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Next steps

After you code the experiment in your app, you are ready for the following steps:
1. [Define your experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md). Create an experiment that defines the view events, conversion events, and unique variations for your A/B test.
2. [Run and manage your experiment in the Dev Center dashboard](manage-your-experiment.md).


## <a name="related-topics"></a>Related topics

* [Create a project and define remote variables in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Define your experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Manage your experiment in the Dev Center dashboard](manage-your-experiment.md)
* [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Run app experiments with A/B testing](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Dec16_HO1-->


