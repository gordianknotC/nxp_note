## addActivityResultListener

```java


```




```mermaid
graph TB
	S(hello)
	subgraph Mediator
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
		FonCreate --> Fcontainer
		FonPause -.-> FonResume
		FonResume --- FonPause
		Fcontainer
	end


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzg2ODg1NzkyLC0yODQzODI5MTMsLTEwNT
g0ODY2NjMsLTQzNDkzMTcwOF19
-->