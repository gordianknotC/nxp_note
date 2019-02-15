 

Original System
: **A1**: Action1, **A2**: Action2, **Sys**: internal system
```mermaid
sequenceDiagram
participant A1
participant A2
participant Sys

Note over A1, A2: Scenario1: Implicit intent
A1 -->> A1: prepare intent
A1 -->> A1: startActivity with explicit target
A1 ->>  A2: onCreate

Note over A1, Sys: Scenario2: Explicit intent
A1 -->> A1: prepare intent
A1 -->> Sys: startActivity Implicitly
Sys-->> Sys: filterIntent
Sys-->> Sys: startActivity with explicit target
Sys ->> A2: onCreate
``` 

--------------------------------------------------------

Replace Sys with Mediator
: **A1**: Action1, **A2**: Action2, **Mediator**: customized mediator for mimic-behaviors of internal system
```mermaid
sequenceDiagram
participant A1
participant A2
participant Mediator

Note over A1, A2: Scenario1: Implicit intent
A1 -->> A1: prepare intent
A1 -->> Mediator: startActivity with explicit target
Mediator -->> Mediator: prepare target
Mediator->>  A2: onCreate

Note over A1, Mediator: Scenario2: Explicit intent
A1 -->> A1: prepare intent
A1 -->> Mediator: startActivity Implicitly
Mediator-->> Mediator: prepare target
Mediator->> A2: onCreate
```

--------------------------------------------------------

**Mediator In Detail**
>**MOn**: Mediator Events  |  **MAct**: Mediator Actions  |  **FOn**: Events of FakeActivity 
>**FDo**: Logic of Fake Activity  |  **JDo**: Logic of Core Lib
```mermaid
sequenceDiagram
participant MUi
participant MEvent
participant MProc
participant MAct
participant .

MProc-->>MProc: initialize plugins ❓
Note over MProc,MAct: A1) instantiate all FakeActivity<br>A2) setup intentFilters for each FakeActivity<br>

MProc-->>MProc: prepare intenFilter mappings ❓

Note over MProc, MAct: B1) each fakeActivity has several intentFilters<br>B2) each fakeActivity map to a unique string action<br>__represents an explicit activity request.<br>_<br> find out who is requesting from and to.

MProc-->>MEvent: Ready!


Note over MUi, .: 👷 Scenario1: startActivity while discovering Tag<br>.<br>.
MEvent-->>MEvent: onNewIntent

MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityExplicit
MProc-->>MProc: createFakeActivity
MProc->> . : onCreate
. -->> . : process ❓
Note left of .: e.g. vitration, initiate Ntag, startDemo,<br>show auth dialog
. ->> MUi: UI response, listen for result [O] [X]
MUi -->> MUi: user interact or none
MUi -->> MEvent: listen for result  [O] [X]
. ->> MAct: startFakeActivity
MAct -->> MAct: startFakeActivity
MAct -->> MProc: getTargetByIntent
MProc -->> MProc: getTargetByIntent
MProc -->> MProc: startExplicitActivity
MProc -->> MProc: createFakeActivity
MProc ->> . : onCreate


Note over MUi, .: 👷 Scenario2: startActivityForResult while trigger auth request<br>.<br>.
MEvent-->>MEvent: onNewIntent
MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityForResultExplicit
MProc-->>MProc: createFakeActivityForResult
MProc->> . : onCreate - open auth
. -->> . : process ❓
Note left of .: e.g. show auth dialog
. ->> MUi: UI response, listen for result [O] [X]
MUi -->> MUi: user interact or none
MUi -->> MEvent: listen for result  [O] [X]
MEvent -->> MEvent: onActivityResult, result passed from dart to platform
MEvent ->> . : call onActivityResult with result
. -->> . : onActivityResult [pass] [failed]
. ->> MAct: startFakeActivity
MAct -->> MAct: startFakeActivity
MAct -->> MProc: getTargetByIntent
MProc -->> MProc: getTargetByIntent
MProc -->> MProc: startExplicitActivity
MProc -->> MProc: createFakeActivity
MProc ->> . : onCreate  


```

**brainstorming in A2:**
- what we already have
	- intent matching logic; but it may not work while exposing to customized mediator
	- matching logic only works under some circumstances ---- applied after intentFilter written in manifest.xml
- prepare intentFilter for each FakeActivity while initializing each FakeActivity

-------------------------------------------------------
**FakeActivity in Detail**

```mermaid
sequenceDiagram
participant .
participant FEvent
participant FAct
participant Core



```



--------------------------------------------------------



>**MOn**: Mediator Events  |  **MAct**: Mediator Actions  |  **FOn**: Events of FakeActivity 
>**FDo**: Logic of Fake Activity  |  **JDo**: Logic of Core Lib
```mermaid
sequenceDiagram

participant MOn
participant MAct
participant FOn
participant FDo
participant JDo

Note over MOn, JDo: 👷 Scenario 1: startActivity while discovering Tag
MOn-->>MOn: onNewIntent, tag discovered
MOn-->>MAct: startActivity
MAct-->>MAct: make intent explicit, prepare target
MAct->>FOn: onCreate
FOn-->>FDo: process
FDo->>JDo: call core function
JDo->>FDo: response
FDo->>MOn: send response data to ui via flutter plugin

Note over MOn, JDo: 👷 Scenario 2: startActivity while user triggering action
MAct-->>MAct: Flutter Channel
MAct->>FOn: onCreate
loop scenario1
	MOn-->JDo: 
end

Note over MOn, JDo: 👷 Scenario 3: startNewIntent while user triggering action
MAct-->>MAct: Flutter Channel
MAct-->>MOn: onNewIntent
loop scenario1
	MOn-->JDo: 
end
```
>**MOn**: Mediator Events  |  **MAct**: Mediator Actions  |  **FOn**: Events of FakeActivity 
>**FDo**: Logic of Fake Activity  |  **JDo**: Logic of Core Lib


----------------

```mermaid
sequenceDiagram
participant Activity
participant Mediator
participant Fake
participant Jar


Note over Activity, Fake: 👷 Scenario1: Before Ready

Activity -->> Mediator: A1 initialize plugin
Mediator -->> Fake: A2 initialize fake activity
Fake -->> Fake: prepare intent logic
Fake -->> Mediator: A3 setup handlers to mediator
Mediator -->> Mediator: prepare dart channel
Mediator -->> Activity: A4 Ready, set initial states
Activity -->> Activity: A5 delegate lifecycle
Activity -->> Mediator: Finish handing over lifecycle control flow to Mediator


Note over Mediator, Jar: 👷 Scenario2: Ready - call Jar while discovering tag 

Mediator-->> Mediator: B0 discover tag, onNewIntent, findout receiver
Mediator->>Jar: B1 do work directly without starting another activity
Jar->>Mediator: B2 response
Mediator-->>Mediator: B3 Flutter UI


Note over Mediator, Jar: 👷 Scenario3: Ready - startActivity while discovering tag

Mediator-->> Mediator: C0 discover tag, onNewIntent, findout receiver
Mediator-->>Mediator: C0 startActivity
Mediator->>Fake: C1 onCreate
Fake-->>Fake: C2 do work
Fake->>Jar:C3 call Jar
Jar->>Fake:C4 response
Fake-->>Fake:C5 prepare intent
Fake->>Mediator: C6 startActivity from Fake
Mediator-->>Mediator: C7 startActivity->onNewIntent ...
Mediator->>Fake: C8 onCreate


Note over Mediator, Jar: 👷 Scenario4: Ready - startActivity triggered by Fake layer

Fake->>Mediator: D0 startActivity by user or...


Note over Mediator: Jump to Scenario 2
Note over Mediator: Jump to Scenario 3


Note over Mediator, Jar: 👷 Scenario5: Ready - startActivityForResult while discovering tag
Mediator-->> Mediator: E0 discover tag, onNewIntent, findout receiver
Mediator-->>Mediator:E1 startActivityForResult
Mediator-->>Fake: E2 onCreate
Fake-->>Fake: E3 do work
Fake-->>Jar:E4 call Jar
Jar-->>Fake:E5
Fake->>Mediator: onActivityResult
Mediator-->>Mediator: UI 
```

------------------



```mermaid
graph TB
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
			Fcontainer --- FonIntent
			Fcontainer --- FonResult
			Fcontainer --- FonReceive
		end
	end

	subgraph Mediators-Flutter-UI-Plugin
		MStartActivity --> FonCreate
		MStartResult --> FonResult
		MonNIntent -.- MStartResult
		MonReceive -.- MonNIntent
		MonNIntent -.- MStartActivity
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
	Fcontainer --> MStartActivity
	Fcontainer --> MStartResult
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM4ODMzNjk1NywxMzc4MjE5NzA4LC0xMj
E0NDkwNzM1LC0zMDgyMzA0ODQsNjAzODAzNjQ4LC0xNDUzODgw
MDA4LC0xNDc2MjYyMzgzLC0xMTI0Mzg3MDY2LC00NTc2NjMxMT
csODU4NTQ2MDIyXX0=
-->