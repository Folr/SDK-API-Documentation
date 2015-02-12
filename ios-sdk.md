# Folr iOS SDK

## Introduction

The Folr iOS SDK allows app developers to include location tracking functionality in their own iOS apps with just a few lines of code. 

All logged locations are automatically stored in the Folr cloud repository, and location records can be viewed or retrieved either in the web dashboard, or via the Folr API.

## ￼Installation

1. Download the **F​olrBusinessSDK.framework*​* [here](https://folr.com).

1. Add **F​olrBusinessSDK.framework*​* into your project by dragging the
FolrBusinessSDK.framework from Finder and dropping it inside your project.

2. Add Apple’s **C​oreLocation.framework​** into your project by going to 
project setting => Build phases => Link Binary With Libraries => Click the + button and search for CoreLocation.framework => Add.

3. Add Apple’s **S​ecurity.framework​** using the same procedure as above.

4. In your *info.plist* file, add new row with Key:
**NSLocationAlwaysUsageDescription​** and value being the message you want the app user to see when the app prompts for location service permission. 
For example “App ABC wants to access your location in the background to help you ..."

5. To update to a new version of **F​olrBusinessSDK.framework**, j​ust delete the old one and add the new version.

## Usage

1. In your AppDelegate.m file import **<​FolrBusinessSDK/FolrBusinessSDK.h>**
and in the method ­​**-application:didFinishLaunchingWithOptions:** a​dd this line 

``` 
[FolrBusinessSDK initializeWithAPIKey:@"your apps' api WRITE token" andAPISecret:@"your app's api WRITE token secret"];
```

2. After your user logins, obtain the user id or user name that you would like to track and call this method to start tracking:

```
[FolrBusinessSDK startTrackingUserId:@"the user id of your choice"];
```

3. After your user logs out, call this method to stop tracking:

```
[FolrBusinessSDK stopTracking];
```

