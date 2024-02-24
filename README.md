# Setting Up Continuous Integration for SFML Android Builds: A Step-by-Step Guide

Based on [SFML examples](https://github.com/SFML/SFML) the [SFML Android template](https://github.com/acsbendi/Build-SFML-For-Android-On-Linux) and the [SFML Android build guide](https://medium.com/@ssimo/build-sfml-for-android-on-ubuntu-6d28b303dcc6).

We'll use the main branch of the [SFML repository](https://github.com/SFML/SFML) as the source of the code to build. 

## SFML build

The compilation of SFML is done using the [Build SFML For Android On Linux](https://github.com/acsbendi/Build-SFML-For-Android-On-Linux) scripts.

There are however a couple of changes that need to be made to the scripts to make them work with GitHub Actions. The changes are:
 - `rebuild-all.sh` has to be modified to make the builds sequentially
 - `rebuild.sh` one flag needs to be changed

### rebuild-all.sh
I've changes the script to execute sequentially and removed all abis except `arm64-v8a` from the `rebuild-all.sh` script.
### rebuild.sh
I've changed the flag `-stdlib=libc++` with `-DCMAKE_CXX_FLAGS="-stdlib=libc++"` and set the `DCMAKE_BUILD_TYPE` to Release.
## GitHub Action
The process is a refined version of the [SFML Android build guide](https://medium.com/@ssimo/build-sfml-for-android-on-ubuntu-6d28b303dcc6).

We start by installing the necessary dependencies, then we accept sdkmanager licenses and download ndk version 26.1.10909125.

Then we use the modified scripts to build SFML for Android.

Finally we install gladle (8.5), build the android example and upload the apk as an artifact.

## Bugs and Limitations
The build process is nowhre near perfect.
Using [act](https://github.com/nektos/act) to test the GitHub Action locally, sometimes the build fails while accepting the licenses.
I've tried to use `yes | sdkmanager --licenses` command, but it always fails. 

To use `act` the path of the ndk needs to be changed to `/opt/android-sdk/ndk/26.1.10909125`. There should be a way to use a path varible but I didn't find it.

I've yet to figure out how to build using the 2.6.1 version of SFML.