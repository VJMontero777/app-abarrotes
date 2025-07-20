# Setup Guide

This document provides detailed steps to configure and deploy the professional grocery app.

## Prerequisites
- Android Studio Flamingo or later
- Java JDK 17+
- Node.js 18+ (for backend)
- Firebase account
- Gradle 8+ (Android Studio includes this)

## Android Project Structure
```
app-abarrotes/
  app/
    src/main/java/com/example/abarrotes/
    src/main/res/
  build.gradle
  settings.gradle
```

## Dependencies
Add the following dependencies to `app/build.gradle`:
```gradle
implementation 'com.google.firebase:firebase-auth:22.3.0'
implementation 'com.google.firebase:firebase-firestore:25.0.0'
implementation 'com.google.android.material:material:1.11.0'
implementation 'androidx.navigation:navigation-fragment-ktx:2.7.5'
implementation 'androidx.navigation:navigation-ui-ktx:2.7.5'
```
Apply the Google services plugin in the root `build.gradle` and include `google-services.json` obtained from Firebase Console.

## Firebase Setup
1. Go to [Firebase Console](https://console.firebase.google.com/).
2. Create a new project.
3. Register the Android app package name (`com.example.abarrotes`).
4. Download the `google-services.json` and place it under `app/`.
5. Enable Email/Password, Google and Phone authentication in the Authentication tab.
6. Enable Firestore database in production mode.

## Payment SDKs
To integrate payment methods available in Peru:
- **Yape**: Follow official documentation at [Yape Developers](https://dev.yape.com.pe/), request API keys and include the provided SDK.
- **Plin**: Contact your bank to obtain access to the API or SDK.
- **BCP, Interbank, BBVA**: Register for merchant accounts and follow each bank's integration guide. Typically they provide a REST API or mobile SDK.
- **Cards**: Use [Culqi](https://www.culqi.com/) or [Mercado Pago](https://www.mercadopago.com.pe/developers/) SDK for credit/debit card processing.

Add the required dependencies and configure callbacks in your checkout screen. Never store card data yourself.

## Backend
You can implement the backend using Node.js:
```bash
npm init -y
npm install express firebase-admin cors bcrypt jsonwebtoken
```
Create a `.env` file to store keys and database credentials. Example server start:
```javascript
const express = require('express');
const admin = require('firebase-admin');
admin.initializeApp({ credential: admin.credential.applicationDefault() });
const app = express();
app.use(express.json());
app.listen(3000, () => console.log('API running'));
```
Use HTTPS in production and store secrets securely.

## Permissions
Add these to `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

## Signing and Publishing
1. In Android Studio, go to **Build > Generate Signed Bundle/APK**.
2. Create or select a keystore (keep it safe).
3. Build a **Release** APK or AAB.
4. Create a Google Play Console account and follow the steps to upload a new app.
5. Provide screenshots, app icon (512x512), feature graphic, privacy policy URL, and store listing.
6. Complete content rating and pricing.

## Security Practices
- Use Firebase security rules to restrict data by user role.
- Enable two-factor authentication for administrator accounts.
- Never commit `google-services.json` with production keys to public repos.
- Use encrypted HTTPS endpoints for all network operations.
- Limit login attempts by monitoring failed sign-ins with Firebase.

## Expected Price
A professional grocery app with the described features can be sold in Peru around **USD 5,000 - 7,000** depending on customization and ongoing support.
