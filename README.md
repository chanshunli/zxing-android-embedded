## Building locally => 本地Maven安装Gradle的库  

#### `[com.journeyapps/zxing-android-embedded "3.6.0" :extension "aar"]` OK


    ./gradlew assemble

To deploy the artifacts the your local Maven repository:

    ./gradlew publishToMavenLocal

You can then use your local version by specifying in your `build.gradle` file:

    repositories {
        mavenLocal()
    }
    
### 1. Android Studio Shell

```bash

➜  zxing-android-embedded git:(master) ✗ 
➜  zxing-android-embedded git:(master) ✗ ls
CHANGES.md                    ISSUE_TEMPLATE.md             build.gradle                  gradlew                       proguard-android-optimize.txt settings.gradle
COPYING                       README.md                     gradle                        gradlew.bat                   sample                        zxing-android-embedded
EMBEDDING.md                  build                         gradle.properties             local.properties              sample-nosupport              zxing-android-embedded.iml
➜  zxing-android-embedded git:(master) ✗ pwd
/Users/stevechan/AndroidLib/zxing-android-embedded
➜  zxing-android-embedded git:(master) ✗ ./gradlew assemble
Starting a Gradle Daemon, 1 incompatible Daemon could not be reused, use --status for details

> Configure project :sample
Specify keystore.file, keystore.alias and keystore.password in local.properties to enable release signing.
Configuration 'compile' in project ':sample' is deprecated. Use 'implementation' instead.
Configuration 'debugCompile' in project ':sample' is deprecated. Use 'debugImplementation' instead.
Configuration 'releaseCompile' in project ':sample' is deprecated. Use 'releaseImplementation' instead.

> Configure project :sample-nosupport
Configuration 'compile' in project ':sample-nosupport' is deprecated. Use 'implementation' instead.

> Configure project :zxing-android-embedded
Configuration 'compile' in project ':zxing-android-embedded' is deprecated. Use 'implementation' instead.
Configuration 'testCompile' in project ':zxing-android-embedded' is deprecated. Use 'testImplementation' instead.

.................

BUILD SUCCESSFUL in 37s
152 actionable tasks: 152 executed

➜  zxing-android-embedded git:(master) ✗ ./gradlew publishToMavenLocal

> Configure project :sample
Specify keystore.file, keystore.alias and keystore.password in local.properties to enable release signing.
Configuration 'compile' in project ':sample' is deprecated. Use 'implementation' instead.
Configuration 'debugCompile' in project ':sample' is deprecated. Use 'debugImplementation' instead.
Configuration 'releaseCompile' in project ':sample' is deprecated. Use 'releaseImplementation' instead.

> Configure project :sample-nosupport
Configuration 'compile' in project ':sample-nosupport' is deprecated. Use 'implementation' instead.

> Configure project :zxing-android-embedded
Configuration 'compile' in project ':zxing-android-embedded' is deprecated. Use 'implementation' instead.
Configuration 'testCompile' in project ':zxing-android-embedded' is deprecated. Use 'testImplementation' instead.


BUILD SUCCESSFUL in 1s
27 actionable tasks: 3 executed, 24 up-to-date
➜  zxing-android-embedded git:(master) ✗ 


```

### 2. Move local aar to `/Users/stevechan/Library/Android/sdk/extras/android/m2repository/`
    
```bash

➜  zxing-android-embedded cat maven-metadata-local.xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
  <groupId>com.journeyapps</groupId>
  <artifactId>zxing-android-embedded</artifactId>
  <versioning>
    <release>3.6.0</release>
    <versions>
      <version>3.6.0</version>
    </versions>
    <lastUpdated>20180407055741</lastUpdated>
  </versioning>
</metadata>
➜  zxing-android-embedded ls  -lh  maven-metadata-local.xml
-rw-r--r--  1 stevechan  staff   317B  4  7 13:57 maven-metadata-local.xml
➜  zxing-android-embedded
➜  zxing-android-embedded ls
3.5.0                    3.6.0                    maven-metadata-local.xml
➜  zxing-android-embedded tree
.
├── 3.5.0
├── 3.6.0
│   ├── zxing-android-embedded-3.6.0-sources.jar
│   ├── zxing-android-embedded-3.6.0.aar
│   └── zxing-android-embedded-3.6.0.pom
└── maven-metadata-local.xml

2 directories, 4 files
➜  zxing-android-embedded pwd
/Users/stevechan/.m2/repository/com/journeyapps/zxing-android-embedded
➜  zxing-android-embedded ls
3.5.0                    3.6.0                    maven-metadata-local.xml
➜  zxing-android-embedded cp -r * /Users/stevechan/Library/Android/sdk/extras/android/m2repository/com/journeyapps/zxing-android-embedded/
➜  zxing-android-embedded

```

```bash

➜  zxing-android-embedded cd /Users/stevechan/Library/Android/sdk/extras/android/m2repository
➜  m2repository ls
NOTICE.txt        com               package.xml       source.properties
➜  m2repository fg zxing-android-embedded
➜  m2repository fg zxing-android-embedded
➜  m2repository fg "*zxing-android-embedded*"
➜  m2repository
➜  m2repository ls
NOTICE.txt        com               package.xml       source.properties
➜  m2repository cd com
➜  com ls
android
➜  com mkdir -p journeyapps/zxing-android-embedded

```
-----

# ZXing Android Embedded

Barcode scanning library for Android, using [ZXing][2] for decoding.

The project is loosely based on the [ZXing Android Barcode Scanner application][2], but is not affiliated with the official ZXing project.

Features:

1. Can be used via Intents (little code required).
2. Can be embedded in an Activity, for advanced customization of UI and logic.
3. Scanning can be performed in landscape or portrait mode.
4. Camera is managed in a background thread, for fast startup time.

A sample application is available in [Releases](https://github.com/journeyapps/zxing-android-embedded/releases).

By default, Android SDK 19+ is required because of `zxing:core` 3.3.2.
To support SDK 14+, downgrade `zxing:core` to 3.3.0 -
see [instructions](#adding-aar-dependency-with-gradle).

## Adding aar dependency with Gradle

From version 3.6.0, only Android SDK 19+ is supported by default.

Add the following to your `build.gradle` file:

```groovy
repositories {
    jcenter()
}

dependencies {
    compile 'com.journeyapps:zxing-android-embedded:3.6.0'
    compile 'com.android.support:appcompat-v7:25.3.1'   // Minimum 23+ is required
}

android {
    buildToolsVersion '27.0.3' // Older versions may give compile errors
}

```

For Android 14+ support, downgrade `zxing:core` to 3.3.0 or earlier:

```groovy
repositories {
    jcenter()
}

dependencies {
    compile('com.journeyapps:zxing-android-embedded:3.6.0') { transitive = false }
    compile 'com.android.support:appcompat-v7:25.3.1'   // Version 23+ is required
    compile 'com.google.zxing:core:3.3.0'
}

android {
    buildToolsVersion '27.0.3' // Older versions may give compile errors
}

```

## Hardware Acceleration

Hardware accelation is required since TextureView is used.

Make sure it is enabled in your manifest file:

```xml
    <application android:hardwareAccelerated="true" ... >
```

## Usage with IntentIntegrator

Launch the intent with the default options:
```java
new IntentIntegrator(this).initiateScan(); // `this` is the current Activity


// Get the results:
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    IntentResult result = IntentIntegrator.parseActivityResult(requestCode, resultCode, data);
    if(result != null) {
        if(result.getContents() == null) {
            Toast.makeText(this, "Cancelled", Toast.LENGTH_LONG).show();
        } else {
            Toast.makeText(this, "Scanned: " + result.getContents(), Toast.LENGTH_LONG).show();
        }
    } else {
        super.onActivityResult(requestCode, resultCode, data);
    }
}
```

Use from a Fragment:
```java
IntentIntegrator.forFragment(this).initiateScan(); // `this` is the current Fragment

// If you're using the support library, use IntentIntegrator.forSupportFragment(this) instead.
```

Customize options:
```java
IntentIntegrator integrator = new IntentIntegrator(this);
integrator.setDesiredBarcodeFormats(IntentIntegrator.ONE_D_CODE_TYPES);
integrator.setPrompt("Scan a barcode");
integrator.setCameraId(0);  // Use a specific camera of the device
integrator.setBeepEnabled(false);
integrator.setBarcodeImageEnabled(true);
integrator.initiateScan();
```

See [IntentIntegrator][5] for more options.

### Generate Barcode example

While this is not the primary purpose of this library, it does include basic support for
generating some barcode types:

```java
try {
  BarcodeEncoder barcodeEncoder = new BarcodeEncoder();
  Bitmap bitmap = barcodeEncoder.encodeBitmap("content", BarcodeFormat.QR_CODE, 400, 400);
  ImageView imageViewQrCode = (ImageView) findViewById(R.id.qrCode);
  imageViewQrCode.setImageBitmap(bitmap);
} catch(Exception e) {

}
```

### Changing the orientation

To change the orientation, specify the orientation in your `AndroidManifest.xml` and let the `ManifestMerger` to update the Activity's definition.

Sample:

```xml
<activity
		android:name="com.journeyapps.barcodescanner.CaptureActivity"
		android:screenOrientation="fullSensor"
		tools:replace="screenOrientation" />
```

```java
IntentIntegrator integrator = new IntentIntegrator(this);
integrator.setOrientationLocked(false);
integrator.initiateScan();
```

### Customization and advanced options

See [EMBEDDING](EMBEDDING.md).

For more advanced options, look at the [Sample Application](https://github.com/journeyapps/zxing-android-embedded/blob/master/sample/src/main/java/example/zxing/MainActivity.java),
and browse the source code of the library.

## Android Permissions

The camera permission is required for barcode scanning to function. It is automatically included as
part of the library. On Android 6 it is requested at runtime when the barcode scanner is first opened.

When using BarcodeView directly (instead of via IntentIntegrator / CaptureActivity), you have to
request the permission manually before calling `BarcodeView#resume()`, otherwise the camera will
fail to open.

## Building locally

    ./gradlew assemble

To deploy the artifacts the your local Maven repository:

    ./gradlew publishToMavenLocal

You can then use your local version by specifying in your `build.gradle` file:

    repositories {
        mavenLocal()
    }

## Sponsored by

[JourneyApps][1] - Creating business solutions with mobile apps. Fast.


## License

Licensed under the [Apache License 2.0][7]

	Copyright (C) 2012-2018 ZXing authors, Journey Mobile

	Licensed under the Apache License, Version 2.0 (the "License");
	you may not use this file except in compliance with the License.
	You may obtain a copy of the License at

	    http://www.apache.org/licenses/LICENSE-2.0

	Unless required by applicable law or agreed to in writing, software
	distributed under the License is distributed on an "AS IS" BASIS,
	WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	See the License for the specific language governing permissions and
	limitations under the License.



[1]: http://journeyapps.com
[2]: https://github.com/zxing/zxing/
[3]: https://github.com/zxing/zxing/wiki/Scanning-Via-Intent
[4]: https://github.com/journeyapps/zxing-android-embedded/blob/2.x/README.md
[5]: zxing-android-embedded/src/com/google/zxing/integration/android/IntentIntegrator.java
[7]: http://www.apache.org/licenses/LICENSE-2.0
