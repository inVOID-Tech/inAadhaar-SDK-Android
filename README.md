# Aadhaar-SDK-Android
Enables your user to download compliant UIDAI Aadhaar XML inside your existing Android App

[[Download SDK](www.invoid.co)](1.0.0)

This SDK (Android) provides a set of screens and functionality to let your user download their Aadhaar XML inside your Android Application itself. This reduces customer drop off as they do not need to navigate to UIDAI Aadhaar Website to download the same.
Aadhaar Offline is the only valid method to submit your Aadhaar identity to any RBI Regulated Entity in order to complete KYC. inVOID's SDK/API provides an easy to use Verification suite which will enable the most seamless customer onboarding.

## Steps in the SDK
1. User is guided to the UIDAI website to download the paperless e-KYC (Aadhaar .xml)
2. Inputs for "Aadhaar Number" & Captcha are filled by the end user.
3. On continuing
    - [x] User is prompted to enter a temporary pin (share code) in order to encrypt the to be downloaded xml using a password.
    - [x] An OTP is received by the end user which is then auto read by the SDK. The inVOID SDK only reads the then received OTP message through the screen.
4. Once the details entered are authenticated, the Aadhaar .xml is downloaded in a .zip which is password(share code) protected

## Getting Started

```
android {
    ...
    defaultConfig {
       minSdkVersion 21
    }
    ...
    compileOptions {
       sourceCompatibility = 1.8
       targetCompatibility = 1.8
    }
}
```
## Main Activity Code
To initialise the view
```
yourinitbutton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                OfflineAadhaarHelper.with(Activity.this, "Your Company Name to be displayed while the process ", "Your Authkey").start();
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

