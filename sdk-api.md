#Folr API Documentation

There are two important elements to the Folr API:

1. **Applications**: Applications are used by the API to group tracked users together. You can create as many applications as required.
2. **Application users**: each application consists of app users whose locations are tracked. They are identified through a *username*, which must be unique per user in the application.



#Invoking the API: application tokens

In order to use the Folr API, you need to create one or more Applications in the [Folr Business Dashboard](https://folr.com/business) via the admin menu in the left. 

Each application you create contains two sets of tokens:

* a token and secret for **writing** data to the API
* a token and secret for **reading** data from the API.

## Example of application tokens:

#### read token:
``` 
3LmTyBKfFAgFei3oAf4vRg
```

#### read token secret:
``` 
yoh-CQemDj8dMndTm79xfEdf63cJ94TzeTqdcGXU3kg
```

#### write token:
``` 
cXdGvmqgvxgo4_T_P_edft
```

#### write token secret:
``` 
yCQ_jbj5xxcyBrz8z5xEcbyRTWUNxV3rAqrdGHSHw9ao
```

*(Please note these tokens above are examples only, to retrieve your application's tokens, please log into the web dashboard at [Folr Business Dashboard](https://folr.com/business)).*

Each application can contain one or more application users, representing users whose locations are tracked. Adding application users can either be done inside the web dashboard, or via the API.

##API Methods

## 1. ```POST``` api/app_users 

*creates an application user*


###Form Parameters

 - **app_username**        - *unique username*  (string, required)


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
    "country_code": null,
    "phone_number": null,
    "email": null,
    "name": null,
    "avatar": null,
    "photo": null,
    "state": "online",
    "has_password": false,
    "terms_accepted_at": null,
    "email_verified": true,
    "type": "AppUser",
    "app_username": "JoeBloggs",
    "app_id": 459,
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

Your application should take note of the **id** of the user, as it is required for editing or deleting the user.

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
      "id": 213,
      "country_code": null,
      "phone_number": null,
      "email": null,
      "name": null,
      "avatar": null,
      "photo": null,
      "state": "offline",
      "has_password": false,
      "terms_accepted_at": null,
      "email_verified": true,
      "type": "AppUser",
      "app_username": "JoeBloggs",
      "app_id": 459,
      "company_id": 12
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

##3. ```GET``` api/app_users/id   

*returns details of a single application user*

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
    "country_code": null,
    "phone_number": null,
    "email": null,
    "name": null,
    "avatar": null,
    "photo": null,
    "state": "offline",
    "has_password": false,
    "terms_accepted_at": null,
    "email_verified": true,
    "type": "AppUser",
    "app_username": "JoeBloggs",
    "app_id": 459,
    "company_id": 12
  }
}
```


##4. ```POST``` api/tracking   

*creates a location tracking entry for an application user*

###Form Parameters

 - **app_username** - *unique username*  (string, required)

 - **captured_at** - *Date of tracking entry*  (Time in iso8601 format ex: '2014-07-15T17:46:51+0800', required)

 - **latitude** - *Latitude*  (string, required)

 - **longitude** - *Longitude*  (string, required)

 - **address** - *Address. Can pass 2-letter locale code (such as 'en') if not found.*  (string, optional)

 - **accuracy** - *GPS accuracy in metres*  (float, optional)
 
 - **speed** - *Moving speed (in meters/second)*  (float, optional)

 - **battery_level** - *Battery level (0.0-100.0)*  (float, optional)

###Example 

```
curl -H "Authorization: access_token=_A7mrS5eiDxVb3AHfRwVfQ,access_token_secret=y8-xGymM1ta63Ra3arxRxQ_G9WZTazd3TUyw8dGkayM" --data "app_username=JoeBloggs&captured_at=2014-07-15T17:46:51+0800&latitude=33.91864735&longitude=18.41956561&address=19A Buitengracht St, Cape Town City Centre, Cape Town&accuracy=16.0&speed=0.0&battery_level=65.0" https://folr.com/api/tracking
```

*(remember to include the WRITE token and token secret in the header)*

###Return JSON

**200 OK**

```
    tracking_entry: {
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
```


**400 Bad Request**

```
{
  "error": "general_error",
  "message": "Validation failed: Invalid token"
}
```





##5. ```GET``` api/tracking   

*returns a list of locations for a particular application user*

###Example 

```
curl -H "Authorization: access_token=xCBxHdPAp7RdCpVGUrQCeg,access_token_secret=s7EW8kUjxxCCf4mi6a1SDZgAGqqK11aZCCAV_QzRSio" -H "Accept: application/json" -H "Content-Type: application/json" https://folr.com/api/tracking?from=2014-07-15T08:00:00+08:00&to=2014-07-15T17:46:51+08:00
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

 - **latitude** - *Latitude of zone center*  (string, required)

 - **longitude** - *Longitude of zone center*  (string, required)

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

