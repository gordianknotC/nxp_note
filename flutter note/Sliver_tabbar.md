


### Hiddable SliverAppbar


### TabBar Nested in SliverAppBar
#### NestedScrollView / SliverAppBar / SliverPersistentHeader / 

```mermaid
graph LR
	fdk((flutter sdk))
	adk((android sdk))
	
	 
	
	
subgraph one
	subgraph path resolving
		fdk -.-> loc
		adk -.-> loc
	end

	loc -.read path information .-> b2
	subgraph android project resolving
		b2 -.- abuild
	end
	dfp -.- b2
	subgraph projectPath&plugin resolving
		set-.->dfp
		set -.- fpgn
	end
	dandroid -.- b2
	subgraph resolving flutter build
		b1 -.- dfb
		
		dflutter -.- b1
		dsub -.- b1
		dflutter -.- dsub
		dsub-.- dandroid
	end
end
```
```dart
Widget build(BuildContext context) {
      return Scaffold(
        body: DefaultTabController(
          length: 2,
          child: NestedScrollView(
            headerSliverBuilder: (BuildContext context, bool innerBoxIsScrolled) {
              return <Widget>[
                SliverAppBar(
                  expandedHeight: 200.0,
                  floating: false,
                  pinned: true,
                  flexibleSpace: FlexibleSpaceBar(
                      centerTitle: true,
                      title: Text("Collapsing Toolbar",
                          style: TextStyle(
                            color: Colors.white,
                            fontSize: 16.0,
                          )),
                      background: Image.network(
                        "https://images.pexels.com/photos/396547/pexels-photo-396547.jpeg?auto=compress&cs=tinysrgb&h=350",
                        fit: BoxFit.cover,
                      )),
                ),
                SliverPersistentHeader(
                  delegate: _SliverAppBarDelegate(
                    TabBar(
                      labelColor: Colors.black87,
                      unselectedLabelColor: Colors.grey,
                      tabs: [
                        Tab(icon: Icon(Icons.info), text: "Tab 1"),
                        Tab(icon: Icon(Icons.lightbulb_outline), text: "Tab 2"),
                      ],
                    ),
                  ),
                  pinned: true,
                ),
              ];
            },
            body: Center(
              child: Text("Sample text"),
            ),
          ),
        ),
      );
    }
```

```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0MTIyOTY4N119
-->