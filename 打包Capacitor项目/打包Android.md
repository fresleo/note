1确保有javahome的环境变量，可以改动的部分 -alias -storepass  -keypass

&  "$env:JAVA_HOME\bin\keytool.exe"  -genkeypair -v -keystore "E:\jianzhan\dockerProje\houxiansheng\my-vue-capacitor-app\android\app\src\myapp.jks"  -alias "Houxiansheng"  -keyalg RSA -keysize 2048  -validity 36500  -storepass "123Cyfcyf"  -keypass "123Cyfcyf"  -dname "CN=Your Name, OU=Dev, O=YourOrg, L=City, S=Province, C=CN"

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMzM2OTEwMiwxNjc4NDQxMDEyXX0=
-->