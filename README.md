
This project contains a detailed guide on how to add Google Maps to your 
Android Project. You can clone/download the repo and try it out yourself
with the attached `signing-key.jks` file. 


#Configuring Google Maps key in your Android Project
 
 To configure Google Maps in your app, you will need to follow these steps. 
 You must have a Google Cloud account. Register one at `https://console.developers.google.com/`
 
 1) In your Google Cloud console, create a new project or enter an existing one
 2) In the Credentials section, press the `Create credentials` button and choose API Key. When done,
 you will get a key with the following format: `AIzaSyCRmSvEVRi2BF83MVl-8IVRWfM6CXMPmmM`. All keys 
 start with `Alza` string. 
 3) Go into the Dashboard section, and press the `Enable API` button at the top. From there, look for
`Google Maps APIs` and select `Android API`. Press `Enable` to enable maps for your project.
 4) WAIT AT LEAST 5 MINUTES for changes to take effect. This is important. Your app could not show the map
 and only a gray area because the configuration hasn't been replicated yet throughout Google servers.
 
 5) Go into your Android Project and add this key to the manifest. You should add the key at the `application` level.
 ```xml
 <meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_KEY_HERE" />
 ```
 
 *NOTE: You can place the API in your strings.xml file and reference it here also.*
 
 Following these steps you will get Google Maps running on your emulator and device during development.
 For production you must take into account the following steps.
 
 #Configuring Google Maps key for Production
 
 Once you have generated a release.apk for your app, you have to register your apk's SHA1 footprint
 in Google Cloud console. To get the SHA1 footprint you must run the following command from your terminal
 window:
 
 ```bash
 keytool -exportcert -keystore production/keystore/path -list -v
 ```
 
You will need to provide the `production/keystore/path` and the keystore password when prompted.
 
 When you run the command you will get a similar output on the terminal
 ```bash
 Certificate fingerprints:
 	 MD5:  D1:F9:4D:42:80:82:0F:5C:6C:3A:F7:65:EE:98:70:24
 	 SHA1: DA:76:7D:70:4F:F6:22:A9:70:34:C2:76:2A:6A:10:BD:14:B2:50:4F
 	 SHA256: 02:72:99:54:68:AD:45:C7:87:FE:34:E3:FE:B5:EE:11:39:D3:C4:75:91:2E:30:90:76:F0:74:41:E7:E4:E9:08
 	 Signature algorithm name: SHA256withRSA
 	 Version: 3
 ```
 
 We will copy the SHA1 value for the next step (`DA:76:7D:70:4F:F6:22:A9:70:34:C2:76:2A:6A:10:BD:14:B2:50:4F`)
 
 Open Google Cloud console, select your project, go into Credentials and enter the API Key we defined 
 previously. Once in, there is a `Key Restrictions` section where we will select the `Android Apps` option.
 
 Once we select it, we have to add our SHA1 fingerprint and package name to the allowed apps to use our 
 API KEY. Press `Add package name and fingerprint` and add that information.
 For example
 Package name: `com.andressantibanez.mapstest`
 SHA1: `DA:76:7D:70:4F:F6:22:A9:70:34:C2:76:2A:6A:10:BD:14:B2:50:4F`
 
 Once we confirm the information, we have to WAIT AT LEAST 5 MINUTES for the change to take place. Install
  the release apk in your phone and start the app after this time. You should see the map working in 
  PRODUCTION. If you don't see the map, check the Logcat in Android Studio to see if you have provided 
  the right SHA1 and package name. Remember to wait for the new configuration to kick when you update 
   values in Google Cloud console. Also, it is a good practice to shutdown your app completely (kill process, run again)
   to check if the map is working.
  
  #I cannot run the Map in Debug mode anymore
  
  This happens because we restricted the API KEY for Android Apps when we set it up for PRODUCTION.
  To continue using this key in DEBUG mode, add the SHA1 fingerprint for DEBUG as we did for PRODUCTION.
  To get the DEBUG SHA1 simply run the following command in your terminal:
  
  ```bash      
keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore -list -v
  ```
  
  Press `ENTER` when asked for the keystore password. The debug store has a `blank` password.
  
  #I cannot run the Map in Production. It appears gray.
  
  This sometimes happen if you don't wait the estimated time for your configuration to propagate
  on Google servers.
  
  It can also happen if you added your Map Activity/Fragment using Android Studio helpers. When you
  use these helpers they usually create two `values/google_maps_api.xml` files: one for DEBUG and one
  for RELEASE (production). Check that both files contain the API key. 