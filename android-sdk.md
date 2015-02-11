# Folr Business Android SDK

## Introduction

The Folr Android SDK allows app developers to include location tracking functionality in their own apps with just a few lines of code. 

All logged locations are automatically stored in the Folr cloud repository, and can be viewed either in the web dashboard, or via the Folr API.

It requires the download of the FolrSDK .jar library file, and inclusion of this library in the Android project's dependancies.


## Installation

1. Please download the Folr SDK Jar file [here](https://folr.com).

2. Once downloaded, add the FolrSDK jar file as a dependancy to your android application.

3.The Folr Android SDK depends on the Google Play library to be included in your Android application project.

Instructions on how to download and include the Google Play library in your app can be found [here] (http://developer.android.com/google/play-services/setup.html).

4. Include the following permissions into your project's manifest XML file:
 * android.permission.INTERNET
 * android.permission.ACCESS_NETWORK_STATE
 * android.permission.WRITE_EXTERNAL_STORAGE
 * android.permission.WAKE_LOCK
 * android.permission.ACCESS_FINE_LOCATION
 * android.permission.ACCESS_COARSE_LOCATION
 * android.permission.ACCESS_NETWORK_STATE
 * android.permission.ACCESS_BACKGROUND_SERVICE
 * com.google.android.c2dm.permission.RECEIVE
 * android.permission.SET_DEBUG_APP
 * com.google.android.providers.gsf.permission.READ_GSERVICES

5. Include the following in your projects manifest XML file: 

        <<receiver android:name="com.folr.sdk.locationpoller.LocationReceiver" />
        <receiver android:name="com.folr.sdk.locationpoller.LocationPoller" />
        <receiver android:name="com.folr.sdk.locationpoller.ArchiverAlarmReceiver" />
        <receiver
            android:name="com.folr.sdk.locationpoller.LocationProviderChangedReceiver"
            android:exported="false" >
            <intent-filter>
                <action android:name="android.location.PROVIDERS_CHANGED" />

                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </receiver>

        <service android:name="com.folr.sdk.locationpoller.LocationPollerService" />

6. If you are using Android Studio, please include the following libraries in the dependencies section of your project's build.gradle file:

*compile 'com.loopj.android:android-async-http:1.4.6'*
*compile 'com.google.code.gson:gson:2.3.1'*

The following four main methods exist in the SDK:

### 1. activateSDK

``` 
**initiates the SDK within your app**

parameters expected:


* **context** : (Context). The context of the activity or fragment calling this method.

* **read_access_token** : (String). The read access token required for API call, retrieved from your application details in the web dashboard. Required.

* **read_access_token_secret** : (String). The read access token secret required for API calls, retrieved from your application details in the web dashboard. Required.

* **write_access_token** : (String). The write access token required for API calls, retrieved from your application details in the web dashboard. Required.

* **write_access_token_secret** : (String). The write access token secret required for API calls, retrieved from your application details in the web dashboard. Required.

```

Code example:

**************************

*>import com.folr.sdk.main.FolrSDK;*

*private void activateFolrSDK(String readToken, String readTokenSecret, String writeToken, String writeTokenSecret){*

    FolrSDK.activateSDK(getActivity().getApplicationContext(),
                        readToken, readTokenSecret, writeToken, writeTokenSecret);

*}*

****************************

### 2. startLocationPolling

``` 
**starts location tracking on the device**

parameters expected:

* **context** : (Context). The context of the activity or fragment calling this method.

* **period** : (int) The frequency in minutes that the device's location is polled and recorded. Required.

* **app_user** : The application user's name that the location records are stored against. Application users are created in the web dashboard, or via API. Optional.

```

Code example:

**************************

*>import com.folr.sdk.main.FolrSDK;*

*private void startLocationPolling(int refreshTimeInMinutes, String appUserName){*

    FolrSDK.startLocationPolling(getActivity().getApplicationContext(),
                        refreshTimeInMinutes, appUserName);

*}*

****************************


### 3. stopLocationPolling

``` 
**stops location tracking on the device**

parameters expected:

* **context** : (Context). The context of the activity or fragment calling this method.

```

Code example:

**************************

*>import com.folr.sdk.main.FolrSDK;*

*private void stopLocationPolling(){*

    FolrSDK.stopLocationPolling(getActivity());

*}*

****************************


### 4. deactivateSDK

``` 
**de-activates the Folr SDK on the device**

parameters expected:

* **context** : (Context). The context of the activity or fragment calling this method.

```

Code example:

**************************

*>import com.folr.sdk.main.FolrSDK;*

*private void deactivateSDK(){*

    FolrSDK.deactivateSDK(getActivity());

*}*

****************************







