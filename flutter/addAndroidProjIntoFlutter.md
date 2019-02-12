
## Add Android Project Into Flutter
![structure](https://gordianknot1981.github.io/nxp_note/flutter/flutter_android_structure.png )

### Structure
- FlutterPrj >
	-  pubspec.yaml 
	- 📁 lib [^1] 
	-  📁android
		- 📁 app[^2]
			- build.gradle
			- local.properties 
	-  ios >
-------------------------------------
[1]:  libraries written in dart.
[2]: "app" is a default convention name in flutter. If you have an existing android project wanna port to flutter, it must rename to "app" or modify it in build.gradle


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMzk1NzExNzgsODAxMjQzMjQ2LC0zNj
Q2ODAzMjEsLTE5MzY0NzkyNTUsLTE3NzQ2OTY4ODJdfQ==
-->