# Folr Android SDK

## Introduction

The Folr Android SDK allows app developers to include location tracking functionality in their own Android apps with just a few lines of code. 

All logged locations are automatically stored in the Folr cloud repository, and location records can be viewed or retrieved either in the web dashboard, or via the Folr API.

It requires the download of the FolrSDK .jar library file, and inclusion of this library in the Android project's dependancies.


## Installation

1. Please download the Folr SDK .jar file [here](https://folr.com).

2. Once downloaded, add the FolrSDK .jar file as a dependancy in your Android application.

3. The SDK requires that the *Google Play* library is included in your Android application project. Instructions on how to download and include the Google Play library in your app can be found [here] (http://developer.android.com/google/play-services/setup.html).

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

        <receiver android:name="com.folr.sdk.locationpoller.LocationReceiver" />
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

6. If you are using **Android Studio**, please include the following libraries in the dependencies section of your project's build.gradle file:

*  com.loopj.android:android-async-http
*  com.google.code.gson:gson

The following four main methods exist in the Folr SDK:

### 1. activateSDK

#### Initiates the SDK within your app

``` 

parameters expected:

1. context : (Android Context). The context of the activity or fragment calling this method. Required.

2. read_access_token : (String). The read access token required for API call, retrieved from your application details in the web dashboard. Required.

3. read_access_token_secret : (String). The read access token secret required for API calls, retrieved from your application details in the web dashboard. Required.

4. write_access_token : (String). The write access token required for API calls, retrieved from your application details in the web dashboard. Required.

5. write_access_token_secret : (String). The write access token secret required for API calls, retrieved from your application details in the web dashboard. Required.

```

Code example:

**************************

import com.folr.sdk.main.FolrSDK;

private void activateFolrSDK(){

    FolrSDK.activateSDK(getActivity().getApplicationContext(),
                        "MsKxQs3xLEL-wHjQ1UXxPw", "NHpc18yYuqQFDZ7FzL3DsCs_8P-5KmxUf5tDYv9jxxx", "v-rwQ5yXz1GUskDseQ-pFQ", "7cgVnhoAPq1BPbh_wPUT8Hsf6ND3mg1ztDXREewHYGz");

}

****************************

### 2. startLocationPolling

####Starts location tracking on the device

``` 

parameters expected:

1. context : (Android Context). The context of the activity or fragment calling this method. Required.

2. period : (int) The frequency in minutes that the device's location is polled and updated. Required.

3. app_user : This is the id or name of the user you wish to start tracking. App_users can created in the web dashboard, or via the API. *However*, if the app_user is not found, it will automatically be created. Required.

```

Code example:

**************************

import com.folr.sdk.main.FolrSDK;

private void startLocationPolling(){

    FolrSDK.startLocationPolling(getActivity().getApplicationContext(),
                        15, "Employee 123");

}

****************************


### 3. stopLocationPolling

####Stops location tracking on the device

```
parameters expected:

1. context : (Android Context). The context of the activity or fragment calling this method. Required.

```

Code example:

**************************

import com.folr.sdk.main.FolrSDK;

private void stopLocationPolling(){

    FolrSDK.stopLocationPolling(getActivity().getApplicationContext());

}

****************************
