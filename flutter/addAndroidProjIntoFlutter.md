
## Add Android Project Into Flutter
![structure](https://gordianknot1981.github.io/nxp_note/flutter/flutter_android_structure.png )

### Structure
- FlutterPrj >
	- 📰 pubspec.yaml 
	- 📁 lib[^f1]
	-  📁 android[^f2]
	-  📁 ios[^f3]
	-  📁 res[^f4]
-------------------------------------
[f1]:  libraries written in dart.
[f2]:  an android project wrapper for flutter, which contains following structure
- 📁 app[^ap]
		- 📰 build.gradle [^ab]
		- 📰 local.properties [^al]
- 📁res[^ar]
- 📁assets[^aa]

[2]: "app" is a default convention name in flutter. If you have an existing android project wanna port to flutter, it must rename to "app" or modify it in build.gradle
following is structure of "app"



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMTg4OTM5OSw4MDEyNDMyNDYsLTM2ND
Y4MDMyMSwtMTkzNjQ3OTI1NSwtMTc3NDY5Njg4Ml19
-->