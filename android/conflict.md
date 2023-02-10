
## Trouble Shoot - A

### Error:Program type already present:
It's a dependencies conflict 相依衝突，多半為不同的相依間存在不同版本的相依

---------------------------------------------------------
### INSTALL_FAILED_VERSION_DOWNGRADE
安卓新版本的 ADT 在安裝apk的時候，會檢查當前要運行的版本 AndroidManifest.xml 中的 versionCode 和手機中已經安裝過的該程序的 versionCode 做對比，如果手機中的 versionCode 比較高，ADT 就會提示出錯：INSTALL_FAILED_VERSION_DOWNGRADE，可修改versionCode 或者uninstall apk

---------------------------------------------------------
### Failed linking file resources | failed processing manifest error
可能是AndroidManifest等XML 文件中所引用的參考不存在，如引用了@mipmap/ic_launch，但是res folder下卻沒有mipmap這個folder，或者當專案有母子project的狀態下，母子project各自指向的res位置出錯或者被子project給覆寫。如下(為子project)
```groovy
ndroid {  
	sourceSets {  
		main {  
			res.srcDirs = ['src']  
```

---------------------------------------------------------
### Cannot read packageName from path/to/AndroidManifest.xml
1. AndroidManifest.xml 放錯地方，如在sourceSet 設定了 main.manifest.srcFile 但卻與實體位置不同
	```groovy
	android {  
	sourceSets {  
		main {  
			manifest.srcFile "src/main/AndroidManifest.xml"  
	```
3. 在AndroidManifest.xml 中沒有設定package
	- 可在defaultConfig中填上applicationId
	```groovy
	defaultConfig {  
		// applicationId will replace package name specified in AndroidManifest.xml  
		applicationId "com.gknot"  
	}
	```

---------------------------------------------------------
### AndroidManifest.xml: Error: 'A' is not a valid file-based resource name character
當我們誤把 AndroidManifest.xml 放到 res 所指定的地方時，程式會把AndroidManifest.xml 視為 resource並套用resource的命名規則，如下設定
```groovy
ndroid {  
	sourceSets {  
		main {  
		manifest.srcFile "src/main/AndroidManifest.xml"  
		res.srcDirs = ['src']  
```

--------------------------------------
### strings_en.arb: Error: The file name must end with .xml
strings_en.arb - 為 Flutter i18n 插件所產生的


----------------------------------------

