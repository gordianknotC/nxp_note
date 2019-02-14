## addActivityResultListener

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
	subgraph Mediators-Flutter-UI-Plugin
		subgraph M1
			M1intent --- M1target
			M1intent --- M1data
			M1data --- M1filter
			M1filter --- M1target
		end
		MStartActivity
		MStartResult
		MStartActivity -.- MStartResult
		MStartResult -.- M1intent
	end
	
	subgraph FakeActivity
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
		
		
		MStartActivity --> FonCreate
		MStartResult --> FonResult
	end
```

```mermaid
sequenceDiagram
Native -->> Mediator: A1-initialize plugin
Mediator -->> Fake: A2-initialize fake activity
loop A2
	Fake -->> Fake: prepare intent logic
end
Fake -->> Mediator: A3-setup handlers to mediator
loop A3
	Mediator -->> Mediator: prepare dart channel
end
Mediator -->> Native: A4-I'm ready
Mediator -->> Native: A5-send initial states
Native ->> Mediator: B1-StartActivity
loop B1
	Mediator ->> Mediator: 
end
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzIyNzgzNTk2LC0yODQzODI5MTMsLTEwNT
g0ODY2NjMsLTQzNDkzMTcwOF19
-->