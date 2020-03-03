# WatchmanSDK for Android
WatchmanSDK for Android helps developers to integrate our products into their own systems. Developed by a core team of engineers at Watchman Door, these components enable a reliable development workflow.

## Requirements
* Api 21 or higher
* Kotlin 1.3 or higher
* AndroidX libraries
* DataBinding

### Gradle project requirements
Contact with development department to get your credentials

```kotlin
allprojects {
    repositories {
        maven {
            url 'http://maven.watchmandoor.com:8081/repository/maven-public/'
            credentials {
                username = "USER"
                password = "PASSWORD"
            }
        }
    }
}
```
### Gradle app requirements

```kotlin
android {
    ...
    
    dataBinding {
        enabled = true
    }

    packagingOptions {
        exclude 'META-INF/DEPENDENCIES'
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    ...
    // Architecture Components
    implementation "androidx.lifecycle:lifecycle-extensions:$archLifecycleVersion"
    implementation "androidx.lifecycle:lifecycle-runtime:$archLifecycleVersion"
    implementation "android.arch.lifecycle:common-java8:1.1.1"
    implementation 'com.google.android.material:material:1.0.0'
    kapt "androidx.lifecycle:lifecycle-compiler:$archLifecycleVersion"
    // Dagger
    implementation "com.google.dagger:dagger:$daggerVersion"
    implementation "com.google.dagger:dagger-android-support:$daggerSupportVersion"
    kapt "com.google.dagger:dagger-compiler:$daggerVersion"
    kapt "com.google.dagger:dagger-android-processor:$daggerVersion"
    /*Retrofit lib*/
    implementation "com.squareup.retrofit2:retrofit:$retrofitVersion"
    implementation "com.squareup.retrofit2:converter-gson:$retrofitGsonVersion"
    //Watchman
    implementation 'com.watchmandoor.libraries:common:1.21@aar'
    implementation 'com.watchmandoor.libraries:network:1.23@aar'
    implementation 'com.watchmandoor.libraries:connectivity:1.51@aar'
    implementation 'com.watchmandoor.libraries:sdk:1.5.8@aar'
    // BLE library
    implementation 'no.nordicsemi.android:ble:2.1.1'
    implementation 'no.nordicsemi.android.support.v18:scanner:1.4.2'
}
```

## Usage
### Typical use: 

```kotlin
class MainActivity : AppCompatActivity() {

    var apiManager: WDApiManager? = null
    var loginManager: WDLoginManager? = null
    
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        
        apiManager = WDApiManager(CLIENT_ID, CLIENT_SECRET)
        loginManager = WDLoginManager(object : WDLoginInterface {
            override fun loginResponse(loginOK: Boolean, error: Int?) {
                if (loginOK) {
                    WDLog.i(TAG, "LOGIN OK!")
                } else if (error != null) {
                    WDLog.i(TAG, "LOGIN ERROR: $error")
                }
            }

            override fun userClosedSession() {
                WDLog.i(TAG, "THE USER HAS LOGGED OUT!")
            }
        }, this)
        
        ...
        
        loginManager!!.doLogin("user@mail.es", "password")
        
    }
    
}
```
