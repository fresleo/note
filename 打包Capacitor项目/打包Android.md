1确保有javahome的环境变量，可以改动的部分 -alias -storepass  -keypass

&  "$env:JAVA_HOME\bin\keytool.exe"  -genkeypair -v -keystore "E:\jianzhan\dockerProje\houxiansheng\my-vue-capacitor-app\android\app\src\myapp.jks"  -alias "Houxiansheng"  -keyalg RSA -keysize 2048  -validity 36500  -storepass "123Cyfcyf"  -keypass "123Cyfcyf"  -dname "CN=Your Name, OU=Dev, O=YourOrg, L=City, S=Province, C=CN"


运行打包的bat

    @echo  off
    
    setlocal EnableExtensions EnableDelayedExpansion
    
      
    
    REM ============================================================
    
    REM One-click Android packaging script for Capacitor + Vite (Vue)
    
    REM Usage:
    
    REM scripts\pack-android.bat [debug|release|aab]
    
    REM Defaults to 'debug'
    
    REM
    
    REM Optional signing for release APK:
    
    REM Configure below or via environment variables before running:
    
    REM KEYSTORE_FILE=C:\keys\myapp.jks
    
    REM KEYSTORE_ALIAS=myapp
    
    REM KEYSTORE_PASSWORD=yourPass
    
    REM KEY_PASSWORD=yourKeyPass (optional if same as keystore)
    
    REM
    
    REM Requirements:
    
    REM - Node.js & npm
    
    REM - Android SDK (via ANDROID_SDK_ROOT or android\local.properties)
    
    REM - JDK 17 used by Gradle
    
    REM ============================================================
    
      
    
    REM Resolve project root from script location
    
    set  "SCRIPT_DIR=%~dp0"
    
    set  "PROJECT_ROOT=%SCRIPT_DIR%.."
    
    pushd  "%PROJECT_ROOT%"  1>nul  2>nul  || (echo [ERROR] Cannot change to project root &  exit /b 1)
    
      
    
    REM Mode
    
    set  "MODE=%~1"
    
    if  "%MODE%"==""  set  "MODE=debug"
    
    set  "MODE=%MODE:"=%"
    
      
    
    REM ----- Signing configuration (configured as requested) -----
    
    REM NOTE: Replace these with your real keystore path and passwords.
    
    set  "KEYSTORE_FILE=E:\jianzhan\dockerProje\houxiansheng\my-vue-capacitor-app\android\app\src\myapp.jks"
    
    set  "KEYSTORE_ALIAS=Houxiansheng"
    
    set  "KEYSTORE_PASSWORD=123Cyfcyf"
    
    REM set "KEY_PASSWORD=yourKeyPass"
    
      
    
    if  defined KEYSTORE_FILE (
    
    if  not  exist  "%KEYSTORE_FILE%" (
    
    echo [WARN] Keystore not found: "%KEYSTORE_FILE%". Will skip manual signing if Gradle not configured.
    
    set  "KEYSTORE_FILE="
    
    )
    
    )
    
      
    
    REM ----- Build web app -----
    
    call npm run build
    
    if  errorlevel  1  goto :fail
    
      
    
    REM ----- Sync Capacitor -----
    
    call npx cap sync android
    
    if  errorlevel  1  goto :fail
    
      
    
    pushd android 1>nul  2>nul  ||  goto :fail
    
      
    
    set  "ARTIFACT="
    
    if /i "%MODE%"=="debug"  goto :debug
    
    if /i "%MODE%"=="release"  goto :release
    
    if /i "%MODE%"=="aab"  goto :aab
    
      
    
    echo [ERROR] Unknown mode: %MODE%  ^(use: debug ^| release ^| aab^)
    
    goto :fail
    
      
    
    :debug
    
    echo [INFO] Assembling Debug APK...
    
    call .\gradlew assembleDebug
    
    if  errorlevel  1  goto :fail
    
    set  "ARTIFACT=app\build\outputs\apk\debug\app-debug.apk"
    
    if  not  exist  "%ARTIFACT%"  goto :missing
    
    goto :done
    
      
    
    :release
    
    echo [INFO] Assembling Release APK...
    
    call .\gradlew assembleRelease
    
    if  errorlevel  1 (
    
    echo [ERROR] Gradle assembleRelease failed. Ensure signingConfig is set or use "aab" mode.
    
    goto :fail
    
    )
    
    if  exist  "app\build\outputs\apk\release\app-release.apk" (
    
    set  "ARTIFACT=app\build\outputs\apk\release\app-release.apk"
    
    ) else  if  exist  "app\build\outputs\apk\release\app-release-unsigned.apk" (
    
    set  "ARTIFACT=app\build\outputs\apk\release\app-release-unsigned.apk"
    
    if  defined KEYSTORE_FILE if  defined KEYSTORE_ALIAS if  defined KEYSTORE_PASSWORD (
    
    echo [INFO] Attempting to sign unsigned APK via apksigner...
    
    set  "SIGNED=app\build\outputs\apk\release\app-release-signed.apk"
    
      
    
    set  "APKSIGNER="
    
    if  defined ANDROID_SDK_ROOT (
    
    if  exist  "%ANDROID_SDK_ROOT%\build-tools" (
    
    for /f "delims="  %%D  in ('dir /b /ad "%ANDROID_SDK_ROOT%\build-tools"  ^| sort') do  set  "LATEST_BT=%%D"
    
    if  defined LATEST_BT (
    
    if  exist  "%ANDROID_SDK_ROOT%\build-tools\!LATEST_BT!\apksigner.bat"  set  "APKSIGNER=%ANDROID_SDK_ROOT%\build-tools\!LATEST_BT!\apksigner.bat"
    
    if  exist  "%ANDROID_SDK_ROOT%\build-tools\!LATEST_BT!\apksigner"  set  "APKSIGNER=%ANDROID_SDK_ROOT%\build-tools\!LATEST_BT!\apksigner"
    
    )
    
    )
    
    )
    
    if  not  defined APKSIGNER (
    
    echo [WARN] apksigner not found. APK remains unsigned: "%ARTIFACT%"
    
    ) else (
    
    if  defined KEY_PASSWORD (
    
    call  "%APKSIGNER%" sign --ks "%KEYSTORE_FILE" --ks-key-alias "%KEYSTORE_ALIAS" --ks-pass "pass:%KEYSTORE_PASSWORD%" --key-pass "pass:%KEY_PASSWORD%" --out "%SIGNED%"  "%ARTIFACT%"
    
    ) else (
    
    call  "%APKSIGNER%" sign --ks "%KEYSTORE_FILE" --ks-key-alias "%KEYSTORE_ALIAS" --ks-pass "pass:%KEYSTORE_PASSWORD%" --out "%SIGNED%"  "%ARTIFACT%"
    
    )
    
    if  errorlevel  1 (
    
    echo [WARN] apksigner failed. Using unsigned APK: "%ARTIFACT%"
    
    ) else (
    
    set  "ARTIFACT=%SIGNED%"
    
    )
    
    )
    
    ) else (
    
    echo [WARN] No signing env provided; APK remains unsigned: "%ARTIFACT%"
    
    )
    
    ) else (
    
    echo [ERROR] Release APK not found in expected location.
    
    goto :fail
    
    )
    
    goto :done
    
      
    
    :aab
    
    echo [INFO] Building Release App Bundle ^(AAB^)...
    
    call .\gradlew bundleRelease
    
    if  errorlevel  1  goto :fail
    
    set  "ARTIFACT=app\build\outputs\bundle\release\app-release.aab"
    
    if  not  exist  "%ARTIFACT%"  goto :missing
    
    goto :done
    
      
    
    :missing
    
    echo [ERROR] Expected artifact not found: "%ARTIFACT%"
    
    goto :fail
    
      
    
    :done
    
    popd  1>nul  2>nul
    
    popd  1>nul  2>nul
    
    echo [SUCCESS] Artifact generated: android\%ARTIFACT%
    
    exit /b 0
    
      
    
    :fail
    
    popd  1>nul  2>nul
    
    popd  1>nul  2>nul
    
    exit /b 1

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTIxMTYzMjQ0LDE2Nzg0NDEwMTJdfQ==
-->