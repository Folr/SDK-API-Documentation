#Folr API Documentation

This document gives details of the Folr API, which allows application developers to harness the power of Folr's location tracking functionality directly in their own smarphone apps.

The API is meant to be used in conjunction with one of the Folr SDKs for Android and iOS.

SDK documentation can be found here:

1. [Android SDK Documentation](android-sdk.md)
2. [iOS SDK Documentation](ios-sdk.md)

There are two important elements to the Folr API:

1. **Applications**: Applications are used by the API to group tracked users together. You can create as many applications as required.
2. **Application users**: each application consists of app users whose locations are tracked. They are identified through a *username*, which must be unique per user in the application.

#Invoking the API: application tokens

To use the Folr API, you first need to create one or more Applications in the [Folr Admin Dashboard](https://folr.com/) via the 'Applications' menu in the left. 

Each application that you create contains two sets of tokens:

* a token and token secret for **writing** data to the API
* a token and token secret for **reading** data from the API.

#### Example of a token:
``` 
3LmTyBKfFAgFei3oAf4vRg
```

#### Example of token secret:
``` 
yoh-CQemDj8dMndTm79xfEdf63cJ94TzeTqdcGXU3kg
```

Each application consists of one or more **users**, representing each user whose locations is tracked via the API. 
Adding application users can either be done inside the web dashboard, or via the API methods.

##API Methods

## 1. ```POST``` api/app_users 

*creates an application user*


###Form Parameters

 - **app_username**        - *the application user's name/id*  (string, required)


###Example 

```
curl -H "Authorization: access_token=_A7mrS5eiDxVb3AHfRwVfQ,access_token_secret=y8-xGymM1ta63Ra3arxRxQ_G9WZTazd3TUyw8dGkayM" --data "app_username=JoeBloggs" https://folr.com/api/app_users
```

*(remember to include the WRITE token and token secret in the header)*

###Return JSON

**200 OK**

```
app_user: 
  {
    "id": 213,
    "state": "online",
    "app_username": "JoeBloggs",
    "app_id": 459,
    "last_location": null,
    "company_id": 12
  }
```

**400 Bad Request**

```
{
  "error": "general_error",
  "message": "Validation failed: App username has already been taken"
}
```

*app_user fields explained:*
 - **id** : the Folr id of the app user
 - **state** : "online" means the user's location is currently being tracked; "offline" means the user is not currently being tracked
 - **app_username** : the application user's name/ID
 - **last_location** : the user's latest tracked location
 - **company_id** : the id of the company that the application user belongs to.

##2. ```GET``` api/app_users   

*returns a list of application users*

###Example 

```
curl -H "Authorization: access_token=xCBxHdPAp7RdCpVGUrQCeg,access_token_secret=s7EW8kUjxxCCf4mi6a1SDZgAGqqK11aZCCAV_QzRSio" -H "Accept: application/json" -H "Content-Type: application/json" https://folr.com/api/app_users
```

*(remember to include the READ token and token secret in the header)*

###Return JSON:

**200 OK**

```
{
  "app_users": [
    {
      "id": 12345,
      "state": "online",
      "app_username": "app user 1",
      "app_id": 103312,
      "company_id": 49,
      "last_location": null
    },
    {
      "id": 45678,
      "state": "online",
      "app_username": "app user 2",
      "app_id": 103312,
      "company_id": 49,
      "last_location": {
          id = 475,
          user_id = 45678
          latitude = "-33.987699",
          longitude = "18.483523",
          captured_at = "2015-02-12T15:18:42+02:00",
          captured_at_time = "2015-02-12T15:18:42",
          captured_at_zone = "+02:00",
          created_at = "2015-02-12T13:18:42+00:00",
          address = "115 Garfield Rd, Claremont, Cape Town",
          accuracy = "126.0",
          speed = "0.0",
          duration = 35,
          battery_level = "43.0",
          zones
          [
            {
              id = "1",
              name = "zone 1"
            },
            {
              id = "2",
              name = "zone 2"
            }
          ]
        }
      }
  ]
}
```

**400 bad request**

```
{
  "error": "user_not_found",
  "message": ""
}
```

*last_location fields explained:*
 - **id** : the location log id
 - **user_id** : the Folr id of the user
 - **latitude** : the latitude of the logged location. 
 - **longitude** : the longitude of the logged location. 
 - **captured_at** : the date and time that the location was captured, in the format yyyy-mm-ddThh:mm:ss[timezone]. The timezone portion is written as UTC offset ([http://en.wikipedia.org/wiki/UTC_offset]).
 - **captured_at_time** : the date and time that the location was captured, without the timezone information. In the format yyyy-mm-ddThh:mm:ss
 - **captured_at_zone** : the time zone of the location log, written as UTC offset ([http://en.wikipedia.org/wiki/UTC_offset]).
 - **created_at** : the date and time that the location was captured, always at the UTC+0 timezone. This is included to allow comparison of location logs taken in different time zones. 
 - **address** : the street address of the logged location. 
 - **accuracy** : the accuracy of the logged location, in metres.
 - **speed** : the speed, in metres per second, that the device was travelling when location was logged.
 - **duration** : the number of minutes that the device was at this location.
 - **battery_level** : the battery level of the device when the location was logged.
 - **zones** : an array of zones that this logged location falls into.

##3. ```GET``` api/app_users/id   

*returns details of a single application user*

###GET Parameters

 - **id**        - *the Folr id of the application user*  (int, required)

###Example 

```
curl -H "Authorization: access_token=xCBxHdPAp7RdCpVGUrQCeg,access_token_secret=s7EW8kUjxxCCf4mi6a1SDZgAGqqK11aZCCAV_QzRSio" -H "Accept: application/json" -H "Content-Type: application/json" https://folr.com/api/app_users/213
```

*(remember to include the READ token and token secret in the header)*

###Return JSON:

**200 OK**

```
{
  "app_user": {
    "id": 213,
    "state": "online",
    "app_username": "JoeBloggs",
    "app_id": 459,
    "company_id": 12,
    "last_location": {
          id = 4675,
          user_id = 213
          latitude = "-33.987699",
          longitude = "18.483523",
          captured_at = "2015-02-12T15:18:42+02:00",
          captured_at_time = "2015-02-12T15:18:42",
          captured_at_zone = "+02:00",
          created_at = "2015-02-12T13:18:42+00:00",
          address = "115 Garfield Rd, Claremont, Cape Town",
          accuracy = "126.0",
          speed = "0.0",
          duration = 35,
          battery_level = "43.0",
          zones
          [
            {
              id = "1",
              name = "zone 1"
            },
            {
              id = "2",
              name = "zone 2"
            }
          ]
        }
  }
}
```

##4. ```GET``` api/app_users/   

*returns details of a single application user*

###GET Parameters

 - **app_username**        - *the application user's name/id*  (string, required)

###Example 

```
curl -H "Authorization: access_token=MsKxQs3xLEL-wHjQ1UXxPw,access_token_secret=NHpc18yYuqQFDZ7FzL3DsCs_8P-5KmxUf5tDYv9jxxx" -H "Accept: application/json" -H "Content-Type: application/json" https://folr.com/api/app_users/?app_username=JoeBloggs
```

*(remember to include the READ token and token secret in the header)*

###Return JSON:

**200 OK**

```
{
  "app_user": {
    "id": 213,
    "state": "online",
    "app_username": "JoeBloggs",
    "app_id": 459,
    "company_id": 12,
    "last_location": {
          id = 4675,
          user_id = 213
          latitude = "-33.987699",
          longitude = "18.483523",
          captured_at = "2015-02-12T15:18:42+02:00",
          captured_at_time = "2015-02-12T15:18:42",
          captured_at_zone = "+02:00",
          created_at = "2015-02-12T13:18:42+00:00",
          address = "115 Garfield Rd, Claremont, Cape Town",
          accuracy = "126.0",
          speed = "0.0",
          duration = 35,
          battery_level = "43.0",
          zones
          [
            {
              id = "1",
              name = "zone 1"
            },
            {
              id = "2",
              name = "zone 2"
            }
          ]
        }
  }
}
```


##5. ```GET``` api/tracking   

*returns a list of locations for a particular application user*

###GET Parameters

 - **from**        - *the from-date, in iso8601 format format*  (string, required)
 
 - **to**        - *the to-date, in iso8601 format format*  (string, required)

- **app_username**        - *the application user's name/id*  (string, required)

###Example 

```
curl -H "Authorization: access_token=xCBxHdPAp7RdCpVGUrQCeg,access_token_secret=s7EW8kUjxxCCf4mi6a1SDZgAGqqK11aZCCAV_QzRSio" -H "Accept: application/json" -H "Content-Type: application/json" https://folr.com/api/tracking?from=2014-07-15T08:00:00+08:00&to=2014-07-15T17:46:51+08:00&app_username=JoeBloggs
```

*(remember to include the READ token and token secret in the header)*

###Return JSON:

**200 OK**

```
{
  "history": [
    {
        user_id: 213,
        captured_at_time: "2014-07-08T27:34:08",
        captured_at_zone: "+08:00",
        captured_at: "2014-07-08T27:34:08+08:00",
        address: "22 York St, Cape Town City Centre, Cape Town",
        accuracy: 28.0,
        speed: 0.0,
        latitude: 33.918635465",
        longitude: "18.41953321",
        battery_level: 96.0
    },
    {
        user_id: 213,
        captured_at_time: "2014-07-15T17:46:51",
        captured_at_zone: "+08:00",
        captured_at: "2014-07-15T17:46:51+08:00",
        address: "19A Buitengracht St, Cape Town City Centre, Cape Town",
        accuracy: 16.0,
        speed: 0.0,
        latitude: 33.91864735",
        longitude: "18.41956561",
        battery_level: 65.0
    }
  ]
}
```

**400 bad request**

```
{
  "error": "user_not_found",
  "message": ""
}
```


##6. ```POST``` api/zones   

*creates a zone*

###Form Parameters

 - **name** - *Name of zone*  (string, required)

 - **latitude** - *Latitude of zone's center*  (string, required)

 - **longitude** - *Longitude of zone's center*  (string, required)

 - **radius** - *Radius of zone in metres*  (float, required)

###Example 

```
curl -H "Authorization: access_token=_A7mrS5eiDxVb3AHfRwVfQ,access_token_secret=y8-xGymM1ta63Ra3arxRxQ_G9WZTazd3TUyw8dGkayM" --data "name=Zone1&latitude=33.91864735&longitude=18.41956561&radius=2500" https://folr.com/api/zones
```

*(remember to include the WRITE token and token secret in the header)*

###Return JSON

**200 OK**

```
    {
      "zone": {
      "id": 25,
      "name": "Zone1",
      "latitude": "33.91864735",
      "longitude": "18.41956561",
      "radius": 2500.0,
      "address": null,
      "company_id": 12
    }
```


**400 Bad Request**

```
{
  "error": "general_error",
  "message": "Validation failed: Invalid token"
}
```

