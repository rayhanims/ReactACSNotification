# ReactACSNotification

Insert the appropriate keys in the sections "YOUR_DATA_HERE"

Changed files from the init project.

\android\app\src\main\java\com\imsapp\MainApplication.java
Initial connection and authentication with AEM.

MobileCore.setApplication(this);
MobileCore.setLogLevel(LoggingMode.DEBUG);
```
try{
  Campaign.registerExtension();
  UserProfile.registerExtension();
  Identity.registerExtension();
  Lifecycle.registerExtension();
  Signal.registerExtension();

  MobileCore.start(new AdobeCallback () {
    @Override
    public void call(Object o) {
        MobileCore.configureWithAppID("KEY_HERE");
    }
  });
} catch (InvalidInitException e) {
    Log.d("TEST", "exception");
}
```
App.js
Mechanism to register with pushid and receive the messages.

```
const register = async () => {
  if (!Firebase.apps.length) {
    await Firebase.initializeApp({
      apiKey: "YOUR_DATA_HERE",
      appId : "YOUR_DATA_HERE",
      projectId: "YOUR_DATA_HERE",
      messagingSenderId: "YOUR_DATA_HERE",
      databaseURL : "YOUR_DATA_HERE",
      storageBucket : "YOUR_DATA_HERE"
    });

  }else {
    Firebase.app();

    messaging().setBackgroundMessageHandler(async remoteMessage => {
      console.log('Message handled in the background!', remoteMessage);
    });

    const unsubscribe = messaging().onMessage(async remoteMessage => {
      console.log('A new FCM message arrived!', JSON.stringify(remoteMessage));
    });
  }
  
  const fcmToken = await messaging().getToken();
  
  if (fcmToken) {
    console.log(fcmToken)
    ACPCore.setPushIdentifier(fcmToken);
    ACPCore.collectPii({
      CustomerID : "123456",
      pushPlatform : "fcm"
    });
  } 
}
```
