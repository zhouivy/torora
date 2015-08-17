## Download the Source ##

```
    cd $HOME
    git clone git://github.com/mwenge/torora.git
    cd /$HOME/torora/
    git checkout android
```


## Download and Install Necessitas SDK ##

  * [Download and Install Necessitas](http://sourceforge.net/p/necessitas/wiki/How%20to%20install%20Necessitas%20SDK/)
  * [Set up the Necessitas SDK for compiling](http://sourceforge.net/p/necessitas/wiki/Setup%20QtCreator/)
  * [Read the general set up advice for compiling Qt apps for Android](http://sourceforge.net/p/necessitas/wiki/How%20to%20write%20Qt%20apps%20for%20Android/)

## Set up Torora to Build on Necessitas ##
  * Open the torora.pro project file from the downloaded source.
  * Use the following as your guide to set up the Projects->Build section:
    * ![http://roberthogan.net/images/android-build.png](http://roberthogan.net/images/android-build.png)
> > Note the extra 'defines arguments', DEFINES+=TORORA DEFINES+=ANDROID:
```
qmake /home/robert/Development/torora/torora.pro -r -spec android-g++ DEFINES+=TORORA DEFINES+=ANDROID

```
  * Use the following as your guide to set up the Projects->Run section:
    * ![http://roberthogan.net/images/android-run.png](http://roberthogan.net/images/android-run.png)
  * Ensure you have an emulator configured to run the android package on:
    * ![http://roberthogan.net/images/android-emulator.png](http://roberthogan.net/images/android-emulator.png)
  * Note that there's a bug in the Necessitas deployment process if you are running the executable on an emulator. You need to copy libtorora.so to the appropriate place each time you do a build:
```
cd ../torora-build-android/
cp libtorora.so src/
```
  * Note that [Necessitas does not deploy a CA cert bundle to the Android emulator and cannot read the native CA cert bundle on Android devices](https://sourceforge.net/p/necessitas/tickets/27/). This means you currently need to copy the SSL CA cert bundles from your machine to the emulator or device. You can do this with:
```
./android-sdk-linux_x86/platform-tools/adb push /etc/ssl/certs/ /data/data/eu.licentia.necessitas.ministro/files/qt/ssl/
```