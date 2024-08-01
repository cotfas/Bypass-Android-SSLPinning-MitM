### Bypassing the SSL Pinning for Twitter/X Android App to see the exchanged data between client-server
#### Man in the Middle attack (MitM) method, by using Frida inject and ProxyMan

#### Requirements: Mac OS computer;
- Install ProxyMan from https://proxyman.io/;
- Download binaries of Frida server from https://github.com/frida/frida/releases ( in our case frida-server-16.3.3-android-x86.xz, don't forget to un-pack it );
- Download emulator from https://genymotion.com/;
- Download ARM Translation v8 https://github.com/m9rco/Genymotion_ARM_Translation ( in our case file Genymotion-ARM-Translation_for_8.0.zip );
- Download Python latest installer for Mac ( https://www.python.org/downloads/macos/ );
- X-plore file manager ( https://play.google.com/store/apps/details?id=com.lonelycatgames.Xplore );
- Download frida-inject.js from ( GitHub link: https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/frida-inject.js );

</br>

⚠️ When you first try to do a SSL sniffing, you will most probably get to an error like: SSL Handshake Failed:

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/07-ssl-handshake-failed.png)

💡 The reason why this happens is the fact that developers add an extra layer of security embedded into the client application, by SSL Pinning the server certificate to each request that is made, this way doing MitM ( Man in the Middle attacks ), are harder to be achieved.

🧩 First and foremost, install Genymotion, Python,  ProxyMan and do the proper setup for proxying. *you can follow this article if you don't know how to do it: https://proxyman.io/android-emulator

👉 Next step is to install the proper command lines:

```
pip install Frida
pip install objection
pip install frida-tools
```

👉 Install on the Genymotion emulator:
`- Samsung Galaxy S8 - Android 8.0 ( api 26 ) *the reason I've chosen this device is because with newer versions of Android there are extra layers of security to bypass, but if the APK Android application has support lower to this version, you are good to go.`

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/01-genymotion.png)

👉 Install Open GApps from the right menu of Genymotion:

<img align="center" width="40%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/03-install-gapps.png"/>

👉 Drag & drop the "ARM Translation v8" zip file over Genymotion emulator:

<img align="center" width="40%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/04-install-arm-translation.png"/>

👉 Install X-plore, and grant root privileges:

<img align="center" width="40%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/02-emulator-rooted.png"/>

👉 Export the ProxyMan certificate to your local computer, and rename it to cert-der-proxyman.crt

👉 Setup the ProxyMan SSL certificates over the emulator:

<img align="center" width="60%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/00-steps-setup-proxyman.png"/>

👉 Copy the frida-server and the cert-der-proxyman.crt certificate to your emulator (you can also use drag&drop), then move the files to: `device location: /data/local/tmp/`

<img align="center" width="40%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/08-xplore-folder-structure.png"/>

👉 Execute proper shell access:

```
adb shell chmod 755 /data/local/tmp/frida-server
adb shell chmod 755 /data/local/tmp/cert-der-proxyman.crt
```

👉 Execute the FRIDA server locally and let it run:

```
adb shell /data/local/tmp/frida-server
```

<img align="center" width="80%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/09-run-frida-server.png"/>

👉 Check if the FRIDA connection works by doing a PS ( process list ):
```
frida-ps -U
```
<img align="center" width="80%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/10-check-frida-connection.png"/>

♻️ Start the Android App as SSL nuked:
```
frida -U -f com.twitter.android -l frida-inject.js
```
<img align="center" width="100%" src="https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/11-hijack-certificate.png"/>

🏆 Voila.

🎯 Results:

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/12-certificate-nuked-success.png)

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/13-results01.png)

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/14-results02.png)

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/15-results03.png)

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/16-results04.png)

![insert image](https://github.com/cotfas/Bypass-Android-SSLPinning-MitM/blob/main/screenshots/17-results05.png)


