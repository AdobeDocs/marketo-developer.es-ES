---
title: Perfiles de usuario
feature: Mobile Marketing, Users and Roles
description: Aprenda a crear y actualizar perfiles de usuario en Marketo Mobile SDK en iOS y Android con Objective-C Swift y Java, campos estándar y personalizados, AssociateLead
exl-id: 1b2cfb7f-d678-4022-8cd9-a56004a1ac46
TQID: https://experienceleague.adobe.com/famIZ1O17Z7TTe2SBHqWSLLL-pp6Vx9M6xXhy2SbB-0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 116
ht-degree: 1%

---

# Perfiles de usuario

Cree o actualice perfiles de usuario con Marketo Mobile SDK para iOS o Android:

1. [Creación de perfiles de usuario en iOS](#ios_user_profiles)
1. [Creación de perfiles de usuario en Android](#android_user_profiles)

## Creación de perfiles de usuario en iOS {#ios_user_profiles}

Cree un perfil y rellene sus campos de usuario.

```objectivec
MarketoLead *profile = [[MarketoLead alloc] init];

// Get user profile from network and populate
[profile setEmail:@"jd@makesomething.com"];
[profile setFirstName:@"John"];
[profile setLastName:@"Doe"];
[profile setAddress:@"1234KingFishSt"];
[profile setCity:@"SouthPadreIsland"];
[profile setState:@"CA"];
[profile setPostalCode:@"78596"];
[profile setCountry:@"USA"];
[profile setGender:@"male"];
[profile setLeadSource:@"_facebook_ads"];
[profile setBirthDay:@"01/01/1985"];
[profile setFacebookId:@"facebookid"];
[profile setFacebookProfileURL:@"facebook.com/profile"];
[profile setFacebookProfilePicURL:@"faceboook.com/profile/pic"];
[profile setLinkedInId:@"linkedinid"];
[profile setTwitterId:@"twitterid"];
```

```swift
let profile =  MarketoLead()

// Get user profile from network and populate
profile.setEmail("jd@makesomething.com")
profile.setFirstName("John")
profile.setLastName("Doe")
profile.setAddress("1234KingFishSt")
profile.setCity("SouthPadreIsland")
profile.setState("CA")
profile.setPostalCode("78596")
profile.setCountry("USA")
profile.setGender("male")
profile.setLeadSource("_facebook_ads")
profile.setBirthDay("01/01/1985")
profile.setFacebookId("facebookid")
profile.setFacebookProfileURL("facebook.com/profile")
profile.setFacebookProfilePicURL("faceboook.com/profile/pic")
profile.setLinkedInId("linkedinid")
profile.setTwitterId("twitterid")
```

Agregar otros [campos estándar](../rest-api/list-of-standard-fields.md).

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
// Add other custom fields
[profile setFieldName:@"mobilePhone"withValue:@"123.456.7890"];
[profile setFieldName:@"numberOfEmployees"withValue:@"10"];
[profile setFieldName:@"phone"withValue:@"123.456.7890"];
```

>[!TAB Swift]

```swift
// Add other custom fields
profile.setFieldName("mobilePhone" , withValue :"123.456.7890");
profile.setFieldName("numberOfEmployees", withValue: "10");
profile.setFieldName("phone", withValue:"123.456.7890");
```

>[!ENDTABS]

Informe del perfil de usuario para crearlo o actualizarlo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

// This method will update user profile
[sharedInstance associateLead:profile];
```

>[!TAB Swift]

```swift
let marketo = Marketo.sharedInstance()

// This method will update user profile
marketo.associateLead(profile)
```

>[!ENDTABS]

## Creación de perfiles de usuario en Android {#android_user_profiles}

1. Cree un perfil de usuario.

   Cree un perfil y rellene sus campos de usuario.

   ```java
   MarketoLead profile = new MarketoLead();
   
   // Get user profile from network and populate
   try {
       profile.setEmail("htcone3@gmail.com");
       profile.setFirstName("Mike");
       profile.setLastName("Gray");
       profile.setFacebookId("facebookid");
       profile.setAddress("1234 King Fish Blvd");
   }
   catch (MktoException e) {
       e.printStackTrace();
   }
   ```

1. Agregar otros [campos estándar](../rest-api/list-of-standard-fields.md).

   ```java
   // Add other custom fields
   profile.setCustomField("mobilePhone", "123.456.7890");
   profile.setCustomField("numberOfEmployees", "10");
   profile.setCustomField("phone", "123.456.7890");
   profile.setCustomField("rating", "R");
   profile.setCustomField("facebookDisplayName", "mini");
   profile.setCustomField("facebookReach", "10");
   profile.setCustomField("facebookReferredEnrollments", "100");
   profile.setCustomField("facebookReferredVisits", "9998");
   profile.setCustomField("lastReferredEnrollment", "03/01/2015");
   profile.setCustomField("lastReferredVisit", "03/01/2015");
   profile.setCustomField("linkedInDisplayName", "Android");
   ```

1. Informe del perfil de usuario para crearlo o actualizarlo.

   ```java
   MarketoLead profile = new MarketoLead();
   
   // This method will update user profile
   marketoSdk.associateLead(profile);
   ```
