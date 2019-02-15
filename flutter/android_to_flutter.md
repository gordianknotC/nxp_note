 










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
Note over MProc,MAct: A1) instantiate all FakeActivity and strip useless code in onCreate<br>A2) setup intentFilters for each FakeActivity<br>📙 Note: brainstorming-A2

MProc-->>MProc: prepare intenFilter mappings ❓

Note over MProc, MAct: B1) each fakeActivity has several intentFilters<br>B2) each fakeActivity map to a unique string action<br>__represents an explicit activity request.<br>_<br> find out who is requesting from and to.
MProc-->> .: onCreate for initialization and intial detection
MProc-->>MEvent: Ready!



Note over MUi, .: 👷 Scenario1: startActivity while discovering Tag<br>.<br>.

Note over MEvent, .: 👷 Scenario1-A: onNewIntent [O]<br>. . . .call onNewIntent or not by comparing current fakeActivity with intent new one<br>. . . .if it's the same then call onNewIntent, if it's not then call onCreate<br>📙 Note: brainstorming-A1
MEvent-->>MEvent: onNewIntent [O]
MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityExplicit - already created
MProc-->>. : onNewIntent [O]
. -->> . : onNewIntent[O] process ❓

Note over MEvent, .: 👷 Scenario1-B: onNewIntent [X]<br>.
MEvent-->>MEvent: onNewIntent [X]
MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityExplicit - not created
MProc-->>MProc: createFakeActivity
MProc->> . : onCreate
. -->> . : process ❓
Note left of .: e.g. vitration, initiate Ntag, startDemo,<br>show auth dialog

Note over MProc, .: 👷 Scenario1-B1: delegate ui to user<br>.
. ->> MUi: UI response, listen for result [O] [X]
MUi -->> MEvent: listen for result  [O] [X]
MUi -->> MUi: user interact or none
MUi -->> MEvent: onResult
MEvent -->> MEvent: onResult
MEvent -->> . : onActivityResult


Note over MProc, .: 👷 Scenario1-B2: direct ui to user<br>.
. ->> MAct: startFakeActivity
MAct -->> MAct: startFakeActivity
MAct -->> MProc: getTargetByIntent
MProc -->> MProc: getTargetByIntent
MProc -->> MProc: startExplicitActivity
MProc -->> MProc: createFakeActivity
MProc ->> . : onCreate
. -->> MUi : Ui response



Note over MUi, .: 👷 Scenario2: startActivityForResult while trigger auth request<br>.<br>.

Note over MEvent, .: 👷 Scenario2-A: onNewIntent [O]<br>. . . . 
MEvent-->>MEvent: onNewIntent [O]
MEvent-->>. : onNewIntent [O]
. -->> . : onNewIntent[O] process ❓

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
**brainstorming in A1 -  how onNewIntent in behavior - IDEATION**
- fakeActivity has no ui resources
	- once instantiated never mind disposing for release just like normal library
		- only create once - onCreate only called once
			- ❓ allways call onNewIntent while new receiving intent requests???
	- in real Activity: each Activity might hold its own resources, but fakeActivity should not operate like that
		- some resources can be shared
			- current intent & nfcintent
				- difference between the two ❓
			- demo library
			- 

**narrow down**:
- instantiate each fakeActivity without constructor
- dependencies thru injection
- DI thru mediator
	- ntag demo, intent, nfcadapter 
- remember nfcintent - tag detected
- call onNewIntent while already instantiated


**brainstorming in A2 - setup intentFilter for each FakeActivity - IDEATION**
- what we already have
	- intent matching logic; but it may not work while exposing to customized mediator
	- matching logic only works under some circumstances ---- applied after intentFilter written in manifest.xml
- prepare intentFilter for each FakeActivity while initializing each FakeActivity

**narrow down**
- implement intentFilter manually instread of using the one in manifest.xml




-------------------------------------------------------
**FakeActivity in Detail**

```mermaid
sequenceDiagram
participant .
participant FEvent
participant FAct
participant Core

Note over ., Core: Scenario1: Initialize All FakeActivities
. ->> FEvent :onCreate
FEvent -->> FEvent: onCreate, initial detection
FEvent -->> Core: Initialize NtagDemo
Core -->> .: share dependency

Note over ., Core: Scenario2: startActivity
. ->> FEvent: onNewIntent
FEvent -->> FEvent: onNewIntent
FEvent -->> FAct: doProcess
FAct -->> FAct: process
FAct -->> Core: ntag
Core -->> FAct: response
FAct -->> FAct: parse
FAct ->> . : UI response or start Activity, listen for Result [O] [X]


```



--------------------------------------------------------



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

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk5NTE5NDg1NywyMDM2MDc5MjgyLC0xMz
Y4NjQ0MTY2LC0xNDYwMTYzODA2LC0xNzIxOTQxOTIzLDE1NDU1
NzU3NCw3Mjc1MTk3OCwxODUwMTU4OTM2LC0xMTQyNjEzOTkzLC
0zODgzMzY5NTcsMTM3ODIxOTcwOCwtMTIxNDQ5MDczNSwtMzA4
MjMwNDg0LDYwMzgwMzY0OCwtMTQ1Mzg4MDAwOCwtMTQ3NjI2Mj
M4MywtMTEyNDM4NzA2NiwtNDU3NjYzMTE3LDg1ODU0NjAyMl19

-->