---
hidden: false
---

# Java 11 → Java 21 Migration Guide (Restructured)

## Overview

This guide documents the migration of Android Registration Client modules from Java 11 to Java 21, AGP 7.x to 8.3.2, and Gradle 7.5 to 8.5. The migration is organized by **phases** and **module paths** to make it easier to follow and less overwhelming.

**Estimated total time:** 4–6 hours (varies by module scope)

---

## Quick Start: Choose Your Path

### Decision Tree

**Q1: Which modules are you migrating?**

- **Option A:** Just the app module (no Android libraries)  
  → Follow: **Path A: App Module Only** (1.5–2 hours)

- **Option B:** keymanager only (crypto & JSON work)  
  → Follow: **Path B: Crypto Module** + **Shared Phases** (2–3 hours)

- **Option C:** All modules (clientmanager, keymanager, packetmanager, transliterationmanager, app)  
  → Follow: **All Phases** (full 4–6 hours)

### Pre-Migration Checklist

- [ ] Install JDK 21 (verify with `java -version`)
- [ ] Create a feature branch: `git checkout -b feature/java21-migration`
- [ ] Backup your current build.gradle files
- [ ] Note current versions of key dependencies (take a screenshot of root ext{})
- [ ] Close Android Studio and clear build caches: `./gradlew clean`

---

## Phase 1: Toolchain & SDK Setup (All Paths) ⏱️ 30–45 min

This phase is **mandatory for all migration paths**. It establishes the foundation.

### Step 1.1: Install and Configure JDK 21

**Windows — System Environment Variables**

```
JAVA_HOME = C:\Program Files\Java\jdk-21.x.x-hotspot
PATH      = %JAVA_HOME%\bin;%PATH%
```

Verify installation:
```bash
java -version
# Expected: openjdk version "21.x.x"
```

**Android Studio Configuration**

1. Open Android Studio
2. Go to: _File > Settings > Build, Execution, Deployment > Build Tools > Gradle > Gradle JDK_
3. Set to **JDK 21**

---

### Step 1.2: Upgrade Gradle Wrapper

**File:** `android/gradle/wrapper/gradle-wrapper.properties`

```properties
# Before
distributionUrl=https\://services.gradle.org/distributions/gradle-7.5-bin.zip

# After
distributionUrl=https\://services.gradle.org/distributions/gradle-8.5-all.zip
```

---

### Step 1.3: Upgrade Android Gradle Plugin (AGP)

**File:** `android/build.gradle` (root level, in the plugins section)

```gradle
// Before
classpath 'com.android.tools.build:gradle:7.x.x'

// After
classpath 'com.android.tools.build:gradle:8.3.2'
```

---

### Step 1.4: Update SDK Versions

**File:** `android/build.gradle` (root ext{} block)

```gradle
// Before
ext {
    compileSdkVersion = 32
    targetSdkVersion  = 32
}

// After
ext {
    compileSdkVersion = 34
    buildToolsVersion = "34.0.0"
    targetSdkVersion  = 34
}
```

---

### Step 1.5: Set Java 21 Compiler Options

Apply to **all library modules** and the **app module**: clientmanager, keymanager, packetmanager, transliterationmanager, app

**File:** `[module]/build.gradle`

```gradle
android {
    compileOptions {
        // Before
        sourceCompatibility JavaVersion.VERSION_11
        targetCompatibility JavaVersion.VERSION_11

        // After
        sourceCompatibility JavaVersion.VERSION_21
        targetCompatibility JavaVersion.VERSION_21
    }
}
```

---

## Phase 2: Android Manifest → Namespace Migration (All Paths) ⏱️ 20–30 min

**Why:** AGP 8.x rejects the `package=` attribute in AndroidManifest.xml. The package identifier must be declared as `namespace` in build.gradle.

### Step 2.1: Update AndroidManifest.xml

Apply to **all modules** with a manifest: clientmanager, keymanager, packetmanager, transliterationmanager, app

**File:** `[module]/src/main/AndroidManifest.xml`

```xml
<!-- Before -->
<manifest package="io.mosip.registration.clientmanager" ...>

<!-- After -->
<manifest ...>
```

Simply **remove the `package` attribute** from the root `<manifest>` tag.

---

### Step 2.2: Add Namespace to build.gradle

**File:** `[module]/build.gradle`

Apply to the same modules as Step 2.1:

```gradle
android {
    namespace 'io.mosip.registration.clientmanager'  // Add this; adjust package name per module
    compileSdkVersion 34
    // ... rest of android block
}
```

**Module → Namespace Mapping:**

| Module | Namespace |
|--------|-----------|
| clientmanager | `io.mosip.registration.clientmanager` |
| keymanager | `io.mosip.registration.keymanager` |
| packetmanager | `io.mosip.registration.packetmanager` |
| transliterationmanager | `io.mosip.registration.transliterationmanager` |
| app | `io.mosip.registration` |

---

### Step 2.3: Auto-Fallback for Third-Party Flutter Plugins

**File:** `android/build.gradle` (root level)

Add this block to handle plugins that still use the old package= pattern:

```gradle
subprojects {
    plugins.withId("com.android.library") {
        def androidExt = project.extensions.findByName("android")
        if (androidExt != null && androidExt.namespace == null) {
            androidExt.namespace = "com.auto.${project.name}"
        }
    }
}
```

This automatically assigns namespaces to any library plugin that doesn't have one, preventing build errors.

---

## Phase 3: Module-Specific Migrations

Choose the paths that apply to your modules.

### Path A: Crypto Module (keymanager only) ⏱️ 45–60 min

> ℹ️ **Applies to:** keymanager

#### Step 3A.1: Upgrade BouncyCastle

**Why:** The `bcprov-jdk15on` and `spongycastle` artifacts are **not published for JDK 18+**. You must use `bcprov-jdk18on`.

**File:** `keymanager/build.gradle`

```gradle
dependencies {
    // Before
    implementation 'org.bouncycastle:bcprov-jdk15on:1.x'
    implementation 'org.bouncycastle:bcpkix-jdk15on:1.x'
    // or spongycastle:
    implementation 'com.madgag.spongycastle:core:1.x'

    // After
    implementation "org.bouncycastle:bcprov-jdk18on:1.78.1"
    implementation "org.bouncycastle:bcpkix-jdk18on:1.78.1"
}
```

---

#### Step 3A.2: Update Imports in LocalClientCryptoServiceImpl.java

Replace all `spongycastle` imports with `bouncycastle`:

**File:** `keymanager/src/[...]/LocalClientCryptoServiceImpl.java`

```java
// Before
import org.spongycastle.crypto.InvalidCipherTextException;
import org.spongycastle.crypto.digests.SHA256Digest;
import org.spongycastle.crypto.encodings.OAEPEncoding;
import org.spongycastle.crypto.engines.RSAEngine;
import org.spongycastle.crypto.params.RSAKeyParameters;

// After
import org.bouncycastle.crypto.InvalidCipherTextException;
import org.bouncycastle.crypto.digests.SHA256Digest;
import org.bouncycastle.crypto.encodings.OAEPEncoding;
import org.bouncycastle.crypto.engines.RSAEngine;
import org.bouncycastle.crypto.params.RSAKeyParameters;
```

---

#### Step 3A.3: Replace PEMWriter with JcaPEMWriter

`PEMWriter` was removed in `bcpkix-jdk18on`.

**File:** `keymanager/src/[...]/CertificateManagerUtil.java`

```java
// Before
import org.bouncycastle.openssl.PEMWriter;

try (PEMWriter pemWriter = new PEMWriter(stringWriter)) {
    // ... write certificate
}

// After
import org.bouncycastle.openssl.jcajce.JcaPEMWriter;

try (JcaPEMWriter pemWriter = new JcaPEMWriter(stringWriter)) {
    // ... write certificate
}
```

---

### Path B: JSON Serialization & Jackson (keymanager only) ⏱️ 30–40 min

> ℹ️ **Applies to:** keymanager

#### Step 3B.1: Upgrade Jackson Dependencies

**File:** `android/build.gradle` (root ext{} block or individual module)

```gradle
ext {
    // Before
    jacksonVersion = '2.x'

    // After
    jacksonVersion = '2.16.2'
}
```

Then in `keymanager/build.gradle`:

```gradle
dependencies {
    implementation "com.fasterxml.jackson.core:jackson-core:$jacksonVersion"
    implementation "com.fasterxml.jackson.core:jackson-databind:$jacksonVersion"
    implementation "com.fasterxml.jackson.core:jackson-annotations:$jacksonVersion"
}
```

---

#### Step 3B.2: Remove jackson-module-afterburner

`afterburner` uses bytecode generation that is incompatible with JDK 21.

**File:** `keymanager/build.gradle`

```gradle
dependencies {
    // Remove this line entirely
    // implementation "com.fasterxml.jackson.module:jackson-module-afterburner:$jacksonVersion"
}
```

---

#### Step 3B.3: Update JsonUtils.java

**File:** `keymanager/src/[...]/JsonUtils.java`

```java
// Before
import com.fasterxml.jackson.module.afterburner.AfterburnerModule;

static {
    objectMapper = new ObjectMapper();
    objectMapper.registerModule(new AfterburnerModule());  // Remove this
    objectMapper.registerModule(new JavaTimeModule());
}

// After
import com.fasterxml.jackson.databind.json.JsonMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

static {
    objectMapper = JsonMapper.builder().build();
    objectMapper.registerModule(new JavaTimeModule());
    objectMapper.enable(SerializationFeature.INDENT_OUTPUT);
    objectMapper.disable(SerializationFeature.FAIL_ON_EMPTY_BEANS);
}
```

---

#### Step 3B.4: Add Jetifier Ignorelist

**File:** `android/gradle.properties`

```properties
android.jetifier.ignorelist=jackson-core, fastdoubleparser
```

This prevents Jetifier from incorrectly converting jackson artifacts.

---

### Path C: Build Features Configuration (All Modules) ⏱️ 20–30 min

> ℹ️ **Applies to:** clientmanager, keymanager, packetmanager, transliterationmanager, app

#### Step 3C.1: Enable BuildConfig Generation

AGP 8.x disables BuildConfig class generation by default. Enable it if your modules reference `BuildConfig.*` constants.

**File:** `[module]/build.gradle`

```gradle
android {
    buildFeatures {
        buildConfig true  // Required by AGP 8.x
    }
}
```

Apply to modules that use `BuildConfig.BASE_URL`, `BuildConfig.CLIENT_VERSION`, etc.

---

#### Step 3C.2: Enable Core Library Desugaring (app module only)

**Why:** Java 21 APIs like `java.time.*` and streams don't exist on older Android devices. Desugaring backports them into the APK.

**File:** `app/build.gradle`

```gradle
android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_21
        targetCompatibility JavaVersion.VERSION_21
        coreLibraryDesugaringEnabled true  // Add this
    }
}

dependencies {
    coreLibraryDesugaring "com.android.tools:desugar_jdk_libs:2.0.4"
}
```

> ℹ️ **Note:** Version 2.0.4 is required for AGP 8.x compatibility. Do not use older versions.

---

## Phase 4: Testing & Dependencies (All Modules) ⏱️ 60–90 min

### Step 4.1: Upgrade Testing Dependencies

**File:** `android/build.gradle` (root ext{} block)

Update these versions centrally:

```gradle
ext {
    // Before
    mockitoVersion = '4.x'
    robolectricVersion = '4.x'
    lombokVersion = '1.18.x'
    jacocoVersion = '< 0.8.9'

    // After
    mockitoVersion = '5.3.1'
    robolectricVersion = '4.13'
    lombokVersion = '1.18.32'
    jacocoVersion = '0.8.12'
}
```

Then update in each module's `build.gradle`:

```gradle
dependencies {
    // Remove mockito-inline (built into mockito-core 5.x now)
    testImplementation "org.mockito:mockito-core:$mockitoVersion"
    testImplementation "org.mockito:mockito-android:$mockitoVersion"
    // NO mockito-inline needed
    
    testImplementation "org.robolectric:robolectric:$robolectricVersion"
    compileOnly "org.projectlombok:lombok:$lombokVersion"
    annotationProcessor "org.projectlombok:lombok:$lombokVersion"
    testImplementation "org.jacoco:org.jacoco.core:$jacocoVersion"
}
```

---

### Step 4.2: Add JVM Arguments for Tests

**Why:** Mockito and byte-buddy need access to JDK internal APIs, which are restricted under JPMS in Java 21.

Apply to **all modules**: clientmanager, keymanager, packetmanager, transliterationmanager, app

**File:** `[module]/build.gradle`

```gradle
android {
    testOptions {
        unitTests {
            all {
                jvmArgs(
                    '-XX:+EnableDynamicAgentLoading',
                    '-Dnet.bytebuddy.experimental=true',
                    '--add-opens', 'java.base/java.lang=ALL-UNNAMED',
                    '--add-opens', 'java.base/java.util=ALL-UNNAMED',
                    '--add-opens', 'java.base/java.lang.reflect=ALL-UNNAMED'
                )
            }
        }
    }
}
```

---

### Step 4.3: Kotlin jvmTarget Alignment (Flutter Plugins)

**Why:** Third-party Flutter plugins declare `sourceCompatibility = 1.8`, but Kotlin 1.9.x on JDK 21 defaults `jvmTarget` to 21, causing build warnings.

**File:** `android/build.gradle` (root level)

```gradle
subprojects {
    plugins.withId('org.jetbrains.kotlin.android') {
        tasks.withType(org.jetbrains.kotlin.gradle.tasks.KotlinJvmCompile).configureEach {
            kotlinOptions.jvmTarget = project.android.compileOptions.sourceCompatibility.toString()
        }
    }
}
```

**File:** `android/gradle.properties`

```properties
kotlin.jvm.target.validation.mode=WARNING
```

---

### Step 4.4: Disable Broken Third-Party Plugin Tests

Some Flutter plugins use Mockito incorrectly and cannot be fixed (source not modifiable). Disable their unit tests:

**File:** `android/build.gradle` (root level)

```gradle
def thirdPartyPluginsWithBrokenTests = [
    'flutter_plugin_android_lifecycle',
    'geolocator_android',
    'image_picker_android',
    'url_launcher_android',
    'shared_preferences_android',
    'webview_flutter_android'
]

if (thirdPartyPluginsWithBrokenTests.any { project.name.contains(it) }) {
    project.tasks.whenTaskAdded { task ->
        if ((task.name.contains("UnitTest") || task.name.contains("unitTest"))
                && !task.name.contains("LintModel")) {
            task.enabled = false
        }
    }
}
```

---

## Phase 5: Troubleshooting & Workarounds ⏱️ As Needed

### Issue 1: AGP 8.x Rejects flutter_config Manifest

**Symptom:** Build error: `Attribute package= not allowed here`

**Cause:** flutter_config 2.0.2 still uses the old `package=` pattern in its AndroidManifest.xml.

**Solution:** Patch it at build time.

**File:** `android/build.gradle` (root level)

```gradle
subprojects { project ->
    if (project.name == 'flutter_config') {
        project.afterEvaluate {
            def manifest = new File(project.projectDir, "src/main/AndroidManifest.xml")
            if (manifest.exists()) {
                def original = manifest.text
                def patched  = original.replaceAll(/ package="[^"]*"/, '')
                if (original != patched) manifest.text = patched
            }
        }
    }
}
```

---

### Issue 2: BuildConfig Not Found

**Symptom:** Compilation error: `BuildConfig symbol not found`

**Cause:** AGP 8.x disables BuildConfig by default.

**Solution:** See **Step 3C.1** above. Add:

```gradle
buildFeatures {
    buildConfig true
}
```

---

### Issue 3: Mockito Tests Fail with "Internal API Access" Errors

**Symptom:** `java.lang.IllegalAccessError` or `MockedStatic initialization failed`

**Cause:** Mockito needs JDK internal API access under JPMS.

**Solution:** See **Step 4.2** above. Add the JVM arguments block.

---

### Issue 4: Jetifier Errors on jackson-core

**Symptom:** Build warning or error during Jetifier processing of jackson artifacts

**Cause:** Jetifier is trying to convert modern Jackson versions, which don't need conversion.

**Solution:** See **Step 3B.4** above. Add to `gradle.properties`:

```properties
android.jetifier.ignorelist=jackson-core, fastdoubleparser
```

---

## Phase 6: Validation & Sign-Off ⏱️ 30–45 min

### Pre-Validation Checklist

- [ ] All phases relevant to your module path completed
- [ ] All gradle files saved and formatted
- [ ] Git diff reviewed for unexpected changes
- [ ] Local environment clean (`./gradlew clean`)

### Validation Commands

Run these in order:

**1. Test Individual Modules**

```bash
# Test each library module
./gradlew :clientmanager:testDebugUnitTest
./gradlew :keymanager:testDebugUnitTest
./gradlew :packetmanager:testDebugUnitTest
./gradlew :transliterationmanager:testDebugUnitTest
```

**2. Full Project Build**

```bash
./gradlew build
```

**3. Generate Debug APK**

```bash
./gradlew assembleDebug
```

**4. Refresh Flutter & Build APK**

```bash
flutter clean
flutter pub get
flutter build apk --debug
```

**5. Build Release APK**

```bash
flutter build apk --release
```

APK location: `build/app/outputs/flutter-apk/`

---

### Post-Validation Checklist

- [ ] All module unit tests pass
- [ ] Full build completes without errors or warnings
- [ ] Debug APK builds successfully
- [ ] Release APK builds successfully
- [ ] No unexpected changes in generated APK size
- [ ] Code review passed
- [ ] Feature branch ready for merge

---

## Migration Summary Table

| Category | Before | After |
|----------|--------|-------|
| **JDK** | 11 | 21 |
| **Gradle** | 7.5 | 8.5 |
| **AGP** | 7.x | 8.3.2 |
| **compileSdkVersion** | 32 | 34 |
| **targetSdkVersion** | 32 | 34 |
| **BouncyCastle** | bcprov-jdk15on | bcprov-jdk18on 1.78.1 |
| **Jackson** | 2.x | 2.16.2 |
| **Mockito** | 4.x | 5.3.1 |
| **Robolectric** | 4.x | 4.13 |
| **Lombok** | 1.18.x | 1.18.32 |
| **Jacoco** | < 0.8.9 | 0.8.12 |

---

## Rollback Plan

If the migration fails critically:

```bash
# Discard all changes
git reset --hard HEAD

# Or revert the feature branch commit
git revert [commit-hash]

# Return to JDK 11 in Android Studio:
# File > Settings > Build Tools > Gradle > Gradle JDK → JDK 11
```

---

## Support & References

- [Android Gradle Plugin 8.x Release Notes](https://developer.android.com/build/releases/gradle-plugin)
- [BouncyCastle 1.78 Migration Guide](https://www.bouncycastle.org/)
- [Mockito 5.x Release Notes](https://github.com/mockito/mockito/releases/tag/v5.0.0)
- [Core Library Desugaring](https://developer.android.com/studio/write/java11-default-toolchain-guide)
