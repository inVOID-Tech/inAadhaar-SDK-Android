# Aadhaar-SDK-Android
Enables your user to download compliant UIDAI Aadhaar XML inside your existing Android App

This SDK (Android) provides a set of screens and functionality to let your user download their Aadhaar XML inside your Android Application itself. This reduces customer drop off as they do not need to navigate to UIDAI Aadhaar Website to download the same.
Aadhaar Offline is the only valid method to submit your Aadhaar identity to any RBI Regulated Entity in order to complete KYC. [inVOID](https://www.invoid.co) SDK/[API](https://api.invoid.co) provides an easy to use Verification suite which will enable the most seamless customer onboarding.

## Steps in the SDK
1. User is guided to the UIDAI website to download the paperless e-KYC (Aadhaar .xml)
2. Inputs for "Aadhaar Number" & Captcha are filled by the end user.
3. On continuing
    - [x] User is prompted to enter a temporary pin (share code) in order to encrypt the to be downloaded xml using a password.
    - [x] An OTP is received by the end user which is then auto read by the SDK. The inVOID SDK only reads the then received OTP message through the screen.
4. Once the details entered are authenticated, the Aadhaar .xml is downloaded in a .zip which is password(share code) protected

## Minimum Requirements
- `minSdkVersion 19` 
- `AndroidX`

## Getting Started

Add following lines in your root ```build.gradle```
```
allprojects {
    repositories {
        ...
        maven { url "https://jitpack.io" }
        maven { url "https://gitlab.com/api/v4/projects/24251481/packages/maven" }
    }
}
```

Add following lines in your module level ```build.gradle```
```
android {
    ...
    compileOptions {
       sourceCompatibility = 1.8
       targetCompatibility = 1.8
    }
}
dependencies {
    ....
    implementation 'co.invoid.android:offlineaadhaar:1.0.6'
}
```

This library also uses some common android libraries. So if you are not already using them then make sure you add these libraries to your module level `build.gradle`
- `androidx.appcompat:appcompat:1.2.0`

## Initialize SDK

```
yourinitbutton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
            
            //Share code entering enable
                OfflineAadhaarHelper.with(YourActivity.this, "Your Company Name to be displayed to the user while doing the process ", "YourAuthkey", "cust ID", "user ID").start();
                
            //Share code entering disabled. Share code is prefilled.    
                OfflineAadhaarHelper.with(YourActivity.this,  "Your Company Name to be displayed to the user while doing the process ", "YourAuthkey", "cust ID", "user ID")
                    .prefillShareCode("Share code you want to set").start();
            }
        });
```

## Authorization 
To Obtain your organisation's authkey, contact us at hello@invoid.co

## Customization 
- To customize the theme of the activity use following theme in your `styles.xml` file
    ```
    <style name="InvoidAppTheme.Custom">
        ...
    </style>
    
    <!--To make text in ActionBar dark, use this. Dark text is default-->
    <style name="InvoidAppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Dark.ActionBar" />
    
    <!--To make text in ActionBar white, use this.-->
    <style name="InvoidAppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Light" />
    ```   
- Following colors are customizable. Add them in your `colors.xml`  
    ```
    <!--Customize color of action bar in Activity-->
    <color name="invoidActionBarColor">@color/yourColor</color> 
    
    <!--Customize color of buttons in Activity-->
    <color name="invoidButtonColor">@color/yourColor</color> 
    ```  
- Error message show to the user can be customized by defining following strings in your `strings.xml` file
    ```
    <!--Error message shown in case of OfflineAadhaarHelper.UIDAI_ERROR-->
    <string name="invoid_uidai_error">Server down, please try after some time</string>
    
    <!--Error message shown in case of OfflineAadhaarHelper.INVOID_AUTH_ERROR-->
    <string name="invoid_auth_error">Not authorized</string>
    
    <!--Error message shown in case of OfflineAadhaarHelper.INTERNET_ERROR-->
    <string name="invoid_check_internet_error">Please check your internet connection</string>
    ```

## Response returned from the SDK
- xml file uri
- zip file uri
- share code to open zip file
- processedJSON
- Error result

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if(requestCode == OfflineAadhaarHelper.AADHAAR_DATA_REQ_CODE) {
            if(resultCode == Activity.RESULT_OK) {
                if (data != null) {
                    AadhaarData aadhaarData = data.getParcelableExtra(OfflineAadhaarHelper.AADHAAR_DATA);
                    Log.d(TAG, "onActivityResult: json: "+aadhaarData.getJsonString());
                    Log.d(TAG, "onActivityResult: file uri: "+aadhaarData.getXmlFileUri().toString());
                    Log.d(TAG, "onActivityResult: zip file uri: "+aadhaarData.getZipFileUri().toString());
                    Log.d(TAG, "onActivityResult: share code: "+aadhaarData.getShareCode.toString());
                }
            } else if (resultCode == Activity.RESULT_CANCELED) {
                Log.d(TAG, "onActivityResult: cancelled by user");
                
            } else if(resultCode == OfflineAadhaarHelper.INTERNET_ERROR) {
                Log.d(TAG, "onActivityResult: internet error");
                
            } else if(resultCode == OfflineAadhaarHelper.INVOID_AUTH_ERROR) {
                Log.d(TAG, "onActivityResult: not authorized to use this SDK");
                
            } else if(resultCode == OfflineAadhaarHelper.UIDAI_ERROR) {
                Log.d(TAG, "onActivityResult: UIDAI Site error");
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
```

