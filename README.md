# firebase-analytics
Google Firebase (Firebase Analytics)
## How To Install

### Add the lines below to `Packages/manifest.json`

for version `10.6.0`
```csharp
"com.google.firebase.analytics": "https://github.com/google-for-unity/firebase-analytics.git#10.6.0",
```

dependency `external-dependency-manager-1.2.175`, `firebase-app-core-10.6.0`
```csharp
"com.google.firebase.core": "https://github.com/google-for-unity/firebase-app-core.git#10.6.0",
"com.google.external-dependency-manager": "https://github.com/google-for-unity/external-dependency-manager-for-unity.git#1.2.175",
```

## Maybe you need
<details><summary>Template code here</summary>


```
using System;
using Firebase;
using Firebase.Extensions;
using UnityEngine;
using Firebase.Analytics;

public class FirebaseAnalyticsManager
{
    static DependencyStatus dependencyStatus = DependencyStatus.UnavailableOther;

    //Init firebase
    public static void Initialize()
    {
        FirebaseApp.CheckAndFixDependenciesAsync().ContinueWithOnMainThread(task =>
        {
            dependencyStatus = task.Result;
            if (dependencyStatus == DependencyStatus.Available)
            {
                InitFirebase();
                FirebaseAnalytics.LogEvent(FirebaseAnalytics.EventLogin);
            }
            else
            {
                Debug.LogError("Could not resolve all Firebase dependencies: " + dependencyStatus);
            }
        });
    }

    static async void InitFirebase()
    {
        FirebaseAnalytics.SetAnalyticsCollectionEnabled(true);
    }

//Example LogEvent No Parameter
/*
 public static void LogEventABC()
    {
        MethodBase methodBase = MethodBase.GetCurrentMethod();
        LogEvent(methodBase.Name);
    }
 */


//Example LogEvent Has Parameter
/*
 public static void LogEventDEF(int level)
    {
        MethodBase methodBase = MethodBase.GetCurrentMethod();
        Parameter[] parameter =
        {
            new Parameter("level_DEF", level)
        };
        LogEvent(methodBase.Name, parameter);
    }
 */


    #region Base function LogEvent

    public static void LogEvent(string prameterName)
    {
        if (IsMobile())
        {
            try
            {
                FirebaseAnalytics.LogEvent(prameterName);
            }
            catch (Exception e)
            {
                Debug.LogError("FirebaseAnalytics log error: " + e.ToString());
                throw;
            }
        }
    }

    public static void LogEvent(string parameterName, Parameter[] parameters)
    {
        if (IsMobile())
        {
            try
            {
                FirebaseAnalytics.LogEvent(parameterName, parameters);
            }
            catch (Exception e)
            {
                Debug.LogError("FirebaseAnalytics log error: " + e.ToString());
                throw;
            }
        }
    }

    public static bool IsMobile()
    {
        if (Application.platform == RuntimePlatform.Android ||
            Application.platform == RuntimePlatform.IPhonePlayer) return true;
        return false;
    }

    #endregion
}

```
