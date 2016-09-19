---
author: mcleanbyron
Description: To run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must code the experiment in your app.
title: Code your app for experimentation
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# Code your app for experimentation

After you [create a project and define remote variables in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), you are ready to update the code in your Universal Windows Platform (UWP) app to:
* Receive remote variable values from Windows Dev Center.
* Use remote variables to configure app experiences for your users.
* Log events to Dev Center that indicate when users have viewed your experiment and performed a desired action (also called a *conversion*).

The following sections describe the general process of getting variations for your experiment and logging events to Dev Center. After you code your app for experimentation, you can [define an experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md). For a walkthrough that demonstrates the end-to-end process of creating and running an experiment, see [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configure your project

To get started, install the Microsoft Store Services SDK on your development computer and add the necessary references to your project.

1. Install the [Microsoft Store Services SDK](http://aka.ms/store-em-sdk).
2. Open your project in Visual Studio.
3. In Solution Explorer, expand your project node, right-click **References**, and click **Add Reference**.
3. In **Reference Manager**, expand **Universal Windows** and click **Extensions**.
4. In the list of SDKs, select the check box next to **Microsoft Engagement Framework** and click **OK**.

## Add code to get variation data

In your project, locate the code for the feature that you want to modify in your experiment. Add code that retrieves data for a variation, and use this data to modify the behavior of the feature you are testing. The specific code you need will depend on your app, but here are the general steps. For complete code example, see [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Declare a [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object that represents the current variation assignment and a [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) object that you will use to log view and conversion events to Dev Center.
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. Get the current cached variation assignment for your experiment by calling the static [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) method, and pass the [project ID](run-app-experiments-with-a-b-testing.md#terms) for your experiment to the method. This method returns a [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) object that provides access to the variation assignment via the [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx) property.
  >**Note**&nbsp;&nbsp;You obtain a project ID when you [create a project in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). The project ID shown below is for example purposes only.

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. Check the [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) property to determine whether the cached variation assignment needs to be refreshed with a remote variation assignment from the server. If it does need to be refreshed, call the static [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) method to check for an updated variation assignment from the server and refresh the local cached variation.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
{
      result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

      // If the call succeeds, use the new result. Otherwise, use the cached value.
      if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
      {
          variation = result.ExperimentVariation;
      }
}
```

5. Use the [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx), [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx), or [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) methods of the [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object to get the values for the variation assignment. In each method, the first parameter is the name of the variation that you want to retrieve (this is the same name of a variation that you enter in your Dev Center dashboard). The second parameter is the default value that the method should return if it is not able to retrieve the specified value from Dev Center (for example, if there is no network connectivity), and a cached version of the variation is not available.

  The following example uses [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) to get a variable named *buttonText* and specifies a default variable value of **Grey Button**.
```CSharp
var buttonText = variation.GetString("buttonText", "Grey Button");
```
4. In your code, use the variable values to modify the behavior of the feature you are testing. For example, the following code assigns the *buttonText* value to the content of a button.
```CSharp
button.Content = buttonText;
```

## Add code to log view and conversion events to Dev Center

Next, add code that logs [view events](run-app-experiments-with-a-b-testing.md#terms) and [conversion events](run-app-experiments-with-a-b-testing.md#terms) to the A/B testing service in Dev Center. Your code should log the view event when the user starts viewing a variation that is part of your experiment, and it should log the conversion event when the user reaches an objective for your experiment.

The specific code you need will depend on your app, but here are the general steps. For complete code example, see [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md).

1. In code that runs when the user starts viewing a variation, initialize the ```logger``` field to a [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) object and call the [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) method. Pass the [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object that represents the current variation assignment (this object provides context about the event to Dev Center) and the name of the view event (this is the same name of a view event that you enter in your Dev Center dashboard). The following example logs a view event named **userViewedButton**.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. In code that runs when the user reaches an objective for one of the goals of the experiment, call the [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) method again and pass the [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) object and the name of a conversion event (this is the same name of a conversion event that you enter in your Dev Center dashboard). The following example logs a conversion event named **userClickedButton**.
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## Next steps

After you code the experiment in your app, you are ready for the following steps:
1. [Define your experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md). Create an experiment that defines the view events, conversion events, and unique variations for your A/B test.
2. [Run and manage your experiment in the Dev Center dashboard](manage-your-experiment.md).


## Related topics

* [Create a project and define remote variables in the Dev Center dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Define your experiment in the Dev Center dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Manage your experiment in the Dev Center dashboard](manage-your-experiment.md)
* [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Run app experiments with A/B testing](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


