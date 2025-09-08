# 在Android项目中使用FaceUnity SDK的正确方法

## 更新：v1.0.6 已解决问题！

从v1.0.6版本开始，我们正确地通过JitPack发布AAR文件，保留了所有原生库和必需组件。

## 解决方案

### 方法1：使用JitPack（推荐，v1.0.6+）

1. **添加JitPack仓库**
   ```gradle
   allprojects {
       repositories {
           google()
           mavenCentral()
           maven { url 'https://jitpack.io' }
       }
   }
   ```

2. **添加依赖（两个都需要）**
   ```gradle
   dependencies {
       implementation 'com.github.sqmyeren:fu-core:v1.0.6'
       implementation 'com.github.sqmyeren:fu-model:v1.0.6'
   }
   ```

### 方法2：直接使用本地AAR文件

1. **下载AAR文件**
   - 下载 `fu-core-1.0.0.aar`
   - 下载 `fu-model-1.0.0.aar`

2. **放入项目**
   ```
   your-android-project/
   └── app/
       └── libs/
           ├── fu-core-1.0.0.aar
           └── fu-model-1.0.0.aar
   ```

3. **配置build.gradle**
   ```gradle
   android {
       // ... 其他配置
   }
   
   dependencies {
       // 引入本地AAR文件
       implementation files('libs/fu-core-1.0.0.aar')
       implementation files('libs/fu-model-1.0.0.aar')
       
       // 其他依赖...
   }
   ```

### 方法2：使用flatDir仓库

1. **在项目级build.gradle添加**
   ```gradle
   allprojects {
       repositories {
           google()
           mavenCentral()
           // 添加本地AAR目录
           flatDir {
               dirs 'libs'
           }
       }
   }
   ```

2. **在app级build.gradle添加**
   ```gradle
   dependencies {
       implementation(name: 'fu-core-1.0.0', ext: 'aar')
       implementation(name: 'fu-model-1.0.0', ext: 'aar')
   }
   ```

## 为什么JAR不行？

JAR版本会出现以下错误：
```
ClassNotFoundException: com.faceunity.wrapper.faceunity
```

原因：
- `com.faceunity.wrapper.faceunity` 是JNI包装类
- 这个类由原生库（.so文件）动态创建
- JAR无法包含.so文件
- 没有.so文件，JNI包装类就无法创建

## 检查AAR是否正确加载

运行应用后，检查是否有以下文件被解压到APK中：
```
lib/
├── arm64-v8a/
│   ├── libCNamaSDK.so
│   └── libfuai.so
├── armeabi-v7a/
│   ├── libCNamaSDK.so
│   └── libfuai.so
└── x86/x86_64/
    └── ... (其他架构)
```

## 常见问题

### Q: 能否通过Maven远程仓库分发？
A: 可以，但需要：
1. 使用支持AAR的仓库（如GitHub Packages）
2. 不能使用JitPack的JAR模式
3. 必须保持AAR格式

### Q: 为什么其他库可以用JitPack？
A: 只有纯Java/Kotlin库才适合用JAR通过JitPack分发。包含原生代码的Android库必须使用AAR格式。

### Q: 有没有其他选择？
A: 
1. 使用官方的Maven仓库（如果FaceUnity提供）
2. 搭建私有Maven仓库
3. 使用GitHub Packages或JFrog Artifactory
