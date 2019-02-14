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

		Fcontainer
	end


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5ODA4NDIyMjIsLTI4NDM4MjkxMywtMT
A1ODQ4NjY2MywtNDM0OTMxNzA4XX0=
-->