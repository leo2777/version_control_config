# version_control_config

基于 [LeoFastDevMvpKotlin](https://github.com/leo2777/LeoFastDevMvpKotlin) 快速开发框架的 config.gradle 依赖管理 demo

## 统一依赖管理

框架采用 config.gradle 进行依赖管理，详细请看此篇文章 [《android - 统一依赖管理(config.gradle)》](https://juejin.cn/post/7224007334513770551) 这里仅仅说明操作步骤：

> - 新建或者下载项目的 「config.gradle」 文件并编写引用
> - 根目录的 「build.gradle」文件引入 「config.gradle」也就是 **apply from "config.gradle"**
> - 在所有module当中的 「build.gradle」文件下，添加活着依赖代码

这里提供对应的示例代码：「config.gradle」

```groovy

ext {

    /**
     * 基础配置 对应 build.gradle 当中 android 括号里面的值
     */
    android = [
            compileSdk               : 32,
            minSdk                   : 21,
            targetSdk                : 32,
            versionCode              : 1,
            versionName              : "1.0.0",
            testInstrumentationRunner: "androidx.test.runner.AndroidJUnitRunner",
            consumerProguardFiles    : "consumer-rules.pro"
    ]

    /**
     * 版本号 包含每一个依赖的版本号，仅仅作用于下面的 dependencies
     */
    version = [
            coreKtx              : "1.7.0",
            appcompat            : "1.6.1",
            material             : "1.8.0",
            constraintLayout     : "2.1.3",
            navigationFragmentKtx: "2.3.5",
            navigationUiKtx      : "2.3.5",
            junit                : "4.13.2",
            testJunit            : "1.1.5",
            espresso             : "3.4.0",
    ]

    /**
     * 项目依赖 可根据项目增加删除，但是可不删除本文件里的，在 build.gradle 不写依赖即可
     * 因为MVP框架默认依赖的也在次文件中，建议只添加，不要删除
     */
    dependencies = [

            coreKtx                    : "androidx.core:core-ktx:$version.coreKtx",
            appcompat                  : "androidx.appcompat:appcompat:$version.appcompat",
            material                   : "com.google.android.material:material:$version.material",
            constraintLayout           : "androidx.constraintlayout:constraintlayout:$version.constraintLayout",
            navigationFragmentKtx      : "androidx.navigation:navigation-fragment-ktx:$version.navigationFragmentKtx",
            navigationUiKtx            : "androidx.navigation:navigation-ui-ktx:$version.navigationUiKtx",
            junit                      : "junit:junit:$version.junit",
            testJunit                  : "androidx.test.ext:junit:$version.testJunit",
            espresso                   : "androidx.test.espresso:espresso-core:$version.espresso",
    ]

}

```

根目录的 「build,gradle」

```groovy

plugins {
    id 'com.android.application' version '7.2.2' apply false
    id 'com.android.library' version '7.2.2' apply false
    id 'org.jetbrains.kotlin.android' version '1.7.10' apply false
}


apply from:"config.gradle"

```
项目 app 下的 「build.gradle」:

```groovy

plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
}

android {
    namespace 'leo.dev.mvp.kt'
    compileSdk rootProject.ext.android.compileSdk

    defaultConfig {
        applicationId "leo.dev.mvp.kt"
        minSdk rootProject.ext.android.minSdk
        targetSdk rootProject.ext.android.targetSdk
        versionCode rootProject.ext.android.versionCode
        versionName rootProject.ext.android.versionName

        testInstrumentationRunner rootProject.ext.android.testInstrumentationRunner

    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    buildFeatures{
        viewBinding true
    }
}

dependencies {

    implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation project(':lib_fast_dev_mvp_kt')

    implementation rootProject.ext.dependencies.coreKtx
    implementation rootProject.ext.dependencies.appcompat
    implementation rootProject.ext.dependencies.material

    testImplementation rootProject.ext.dependencies.junit
    androidTestImplementation rootProject.ext.dependencies.testJunit
    androidTestImplementation rootProject.ext.dependencies.espresso
}

```

以上就是 简单使用，如果想看详细使用，请看这个文章：[《android - 统一依赖管理(config.gradle)》](https://juejin.cn/post/7224007334513770551)
此外还有其他 依赖管理的文章，敬请期待
