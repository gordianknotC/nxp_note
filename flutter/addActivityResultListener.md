## addActivityResultListener

```java


```




```mermaid
graph TB
	subgraph Native FlutterActivity
		pluginRegister
		pluginRegister -.- onCreate
		subgraph lifeCycles
			onCreate -.- onIntent
			onIntent -.- onResult
			onResult -.- onPause
			onResume -.- onPause
			onCreate -.- onStop
		end
	end
	subgraph Mediators - FlutterPlugin
		
		subgraph M1
			M1intent --- M1target
			M1intent --- M1data
		end
		MStart
		MStartResult
		MStart -.- MStartResult
		MStartResult -.- M1intent
	end
	
	subgraph FActivity
		FonCreate
		FonPause 
		FonResume 
		FonResult 
		FonIntent 
		FonReceive 
		
		subgraph fake activity
			FonPause -.- FonResume
			FonResume -.- FonCreate
			FonCreate --> Fcontainer
		end
		
		subgraph listeners
			Fcontainer --> FonIntent
			Fcontainer --> FonResult
			Fcontainer --> FonReceive
		end
		
		
		
	end


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MTA1MDU2MjQsLTI4NDM4MjkxMywtMT
A1ODQ4NjY2MywtNDM0OTMxNzA4XX0=
-->