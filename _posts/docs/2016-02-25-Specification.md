---
layout: page
title: "Specification"
category: docs
order: 1
---

### Terms

* **Correlation Id:** A unique piece of text (e.g. guid) captured to use for correlation of historical events across systems. (example: 34802ee2-c35e-46e5-941e-42b9c30e77e6)
* **Activity Scope:** One or many operations which when grouped together logically result in an activity (example: Login of a user)   

### Correlation across services

For a service to support CorrelatorSharp:

1. The service must check for the presence of a `X-Correlation-Id` header/metadata value (in e.g. HTTP header)
2. The service must capture that as its current Activity Scope and include it in requests to dependent services.

#### Standards

|-----------------------------|----------------------------------------------|
| HTTP(s)                     | `X-Correlation-Id` HTTP header               |



### Correlation within a single service/component/app for context-aware logging

 1. For consistency between different implementations of the *Activity Scope* pattern the following pseudo structure and property naming is recommended:
 
    ```
    class ActivityScope {
        Id: string, // Id for correlation purposes
        ParentId: string, //  of the parent ActivityScope if any
        Name: string, // name/description of the current activity
    }
    ```
 2. The current Activity Scope should be accessible within the service and application, so that it can be used for context-aware logging.

### The Goal

At the end of the day the goal is to roughly achieve this kind of traceability through correlation:

Log from application server:

```json
{
  "system": "Web server",

  "message": "Error Logging in user",
  
  "activityScope": "7268d313-88f8-462a-bc30-63c0a14a5359",
  "parentActivityScope": null,
  "activityName": "User login",
}
```

Log from auth server:

```json
[
{
  "system": "Auth server",
  
  "message": "Login requested",
  
  "activityScope": "7268d313-88f8-462a-bc30-63c0a14a5359",
},
{
  "system": "Auth server",
  
  "message": "Checking credentials",
  
  "activityScope": "7268d313-88f8-462a-bc30-63c0a14a5359",
},
{
  "system": "Auth server",
  
  "message": "Error - user database down",
  
  "activityScope": "7268d313-88f8-462a-bc30-63c0a14a5359",
}
]
```