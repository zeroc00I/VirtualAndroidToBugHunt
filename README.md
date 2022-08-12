# How to install burp cert on android device using adb

### Requirements:

1. apt-get install android-sdk
2. https://developer.android.com/studio#command-tools OR
```
From Android Studio --> SDK Manager --> Check "Show Obsolete packages" --> Download platform-tools
```

## 1 - Go to Android Studio website and download of the project (865 MB):
https://developer.android.com/studio?hl=pt-br

## 2 - After downloading the android studio, you have to download an image to your android simulator doing the following command:
```
~/Android/Sdk/tools/bin/sdkmanager install "system-images;android-23;google_apis;x86"
```
## You can list all images availables using: 
```
~/Android/Sdk/tools/bin/sdkmanager --list --verbose

```
## 3 - In this step, you will be able to create your AVD (Android Virtual Device), doing the command bellow:
```
~/Android/Sdk/tools/bin/avdmanager create avd -n test -k "system-images;android-23;google_apis;x86" -b x86 -c 100M -d 7 -f
```
## 4 - Run your virtual device: 
```
~/Android/Sdk/tools/emulator -writable-system -avd test
```
## 5 - Through your local computer (outside the android device) 
Download your burp certificate accessing http://127.0.0.1:8080 using your favorite browser

## 6 - Click on CA Certificate on top right of burp local page
Downloading the cacert.der file.

## 7 - The following commands have to be made inside of your terminal, in the same path that you have the cacert.der
```
openssl x509 -inform DER -in cacert.der -out cacert.pem
openssl x509 -inform PEM -subject_hash_old -in cacert.pem |head -1 (Will be displayed a HASH)
mv cacert.pem HASH.0
```
## 8 - Still using the terminal:
```
adb root
adb remount
adb push HASH.0 /system/etc/security/cacerts
adb shell
chmod 644 /system/etc/security/cacerts/HASH.0
exit
adb reboot
```

## 9 - At this point, you can intercept any request from your virual android device!

# How to get some app from Google Playstore:

### Install this extension:
https://chrome.google.com/webstore/detail/apk-downloader/fgljidimohbcmjdabiecfeikkmpbjegm
### It will be required that you fill an input with a link app from Google Play Store: https://play.google.com/store
#### E.g: https://play.google.com/store/apps/details?id=com.facebook.katana
----
References
https://blog.ropnop.com/configuring-burp-suite-with-android-nougat/

Greetings to SK o/
https://github.com/Sh4d0wKnuckl3s


