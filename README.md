# AppLinks
Android AppLinks with Flutter and Firebase

### Steps to integrate:
1. create and host the assetlinks.json file
  > - Generate a statement file on your site to enable App Linking https://developers.google.com/digital-asset-links/tools/generator.
  > - Hosting site domain should be the firebase hosting domain like:
>    -     dummy-app-c6162.web.app
  > - For hosting to firebase follow the document https://medium.com/androiddevelopers/android-app-links-deploy-assetlinks-json-in-minutes-d7082dffcac.
  > - Replace assetlinks.json file content with the statement file content and redeploy.
  > - statement file should look something like this

    [{
      "relation": ["delegate_permission/common.handle_all_urls"],
      "target" : { "namespace": "android_app", "package_name": "com.mycompany.dummyapp",
                   "sha256_cert_fingerprints": ["17:C0:BD:99:6C:FE:51:F8:ED:29:7F:04:14:47:D0:D8:11:99:**:**:**:**:**:**:**:**:**:**:**:**:**:**"] }
    }]

2. update Manifest.xml file.
> add internet permission

        <uses-permission android:name="android.permission.INTERNET"/>
> add intent filters

    <activity android:name=".MainActivity"
       android:exported="true">
       <intent-filter>
           <action android:name="android.intent.action.MAIN" />
           <category android:name="android.intent.category.LAUNCHER" />
       </intent-filter>
       <intent-filter android:autoVerify="true">
           <action android:name="android.intent.action.VIEW" />
           <category android:name="android.intent.category.DEFAULT" />
           <category android:name="android.intent.category.BROWSABLE" />
           <data android:host="dummy-app-c6162.web.app" android:scheme="http"
               android:pathPrefix="/welcome" />
           <data android:host="dummy-app-c6162.web.app" android:scheme="https"
               android:pathPrefix="/welcome" />
       </intent-filter>
    </activity>

3. update MainActivity.kt file.
> 

    package com.mycompany.dummyapp
  
    import android.net.Uri
    import android.os.Bundle
    import android.widget.Toast
    import io.flutter.embedding.android.FlutterActivity
    
    class MainActivity : FlutterActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            // setContentView(R.layout.main)  // Ensure you have a main.xml in res/layout
    
            val data: Uri? = intent?.data
            val toastMessage = data?.toString() ?: "No data received"
            val toast = Toast.makeText(this, toastMessage, Toast.LENGTH_LONG)
            toast.show()
        }
    }


4. command to add uni_links package to pubspec.yaml file.
>

    flutter pub add uni_links
