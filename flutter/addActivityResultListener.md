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
		FonPause -->FonResume

		Fcontainer --> FonIntent
		Fcontainer --> FonResult
		Fcontainer --> FonReceive
	end


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg2Nzc5NjU3LC0yODQzODI5MTMsLTEwNT
g0ODY2NjMsLTQzNDkzMTcwOF19
-->