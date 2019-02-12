


#### Error:Program type already present:
it's a dependencies conflict 相依衝突，多半為不同的相依間存在不同版本的相依

#### INSTALL_FAILED_VERSION_DOWNGRADE
安卓新版本的 ADT 在安裝apk的時候，會檢查當前要運行的版本 AndroidManifest.xml 中的 versionCode 和手機中已經安裝過的該程序的 versionCode 做對比，如果手機中的 versionCode 比較高，ADT 就會提示出錯：INSTALL_FAILED_VERSION_DOWNGRADE，可修改versionCode 或者uninstall apk
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNTU5NDU0NDVdfQ==
-->