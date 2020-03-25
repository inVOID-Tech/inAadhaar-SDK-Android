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
        maven { url "https://dl.bintray.com/invoidandroid12/android/" }
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
    implementation 'co.invoid.android:offlineaadhaar:1.0.2rc'
}
```

This library also uses some common android libraries. So if you are not already using them then make sure you add these libraries to your module level `build.gradle`
- `androidx.appcompat:appcompat:1.1.0`
- `androidx.constraintlayout:constraintlayout:1.1.3`
- `com.google.android.material:material:1.0.0`


## Initialize SDK

```
yourinitbutton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                OfflineAadhaarHelper.with(Activity.this, "Your Company Name to be displayed to the user while doing the process ", "Your Authkey", "cust ID", "user ID").start();
            }
        });
```
## Authorization 
To Obtain your organisation's authkey, contact us at hello@invoid.co

## Customization 
Coming soon

## Response returned from the SDK
- .zip file path
- sharecode
- processedJSON

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if(requestCode == OfflineAadhaarHelper.AADHAAR_DATA_REQ_CODE) {
            if(resultCode == RESULT_OK) {
                if (data != null) {
                    AadhaarData aadhaarData = data.getParcelableExtra(OfflineAadhaarHelper.AADHAAR_DATA);
                    Log.d(TAG, "onActivityResult: json: "+aadhaarData.getJsonString());
                    Log.d(TAG, "onActivityResult: shareCode: "+aadhaarData.getShareCode());
                    Log.d(TAG, "onActivityResult: file uri: "+aadhaarData.getFileUri().toString());
                }
            } else if(resultCode == RESULT_CANCELED){
                Log.d(TAG, "onActivityResult: cancelled by user");
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
```

