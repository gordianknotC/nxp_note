## addActivityResultListener

```java


```




```mermaid
graph TB
	subgraph Native FlutterActivity
		pluginRegister
		lifeCycles
		pluginRegister -.- onCreate
		subgraph lifeCycles
			onCreate
			
		end
	end
	subgraph Mediator FlutterPlugin
		MStart
		MStartResult
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
eyJoaXN0b3J5IjpbLTU1ODQzNzgwMCwtMjg0MzgyOTEzLC0xMD
U4NDg2NjYzLC00MzQ5MzE3MDhdfQ==
-->