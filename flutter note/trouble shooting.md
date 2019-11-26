

#### 
⚠👷📂🗁📰💼 
📌⚡⚠🎧📧 📝
📓 📘 ⏳🛑☠️⚡🛑
✒️🖋️🏴󠁴󠁷󠁰󠁥󠁮󠁿🖆 ✍️ 🔏 🖊️ 
🔥📛


# Trouble Shooting  
  
### Flutter Constaantly Rebuild  
  
to infer why flutter constantly rebuilds, consider following   
situation for further analysis.  
  
- #### ⚡ UniqueKey Wrongly Used:   
   uniqueley implicates that ___it is unique to itself___ which means its unique on   
    every rebuild, hence highly incorrespondent to StatefulWidget. If wrongly  
    used uniquekey to statefulWidget, it would force statefulWIdget to  
    recreate without saving any state...  
      
    __📝 example:__      
  > none...
  
  #### ⚡ Widget Constantly Changed on It's Child Visually  
  
  When parent doesn't have any visual constraints to it's  
    child, it's possible that some visually changed on child   
    could entail reconstruction of it's parent.  
  
    __📝 example:__  
    >A Container, which constantly change it's size, nested inside   
    another Container with no width been assigned to. This implicity  
    means the parent Container will forced to re-render on   
    constantly visually changed by it's child, since it doesn't  
    specify any width constraints to flutter.  
     >
    > 📝 Since width is unassigned in parent [Container], this would   
    force it's parent widget to re-render, hence rebuild occurs...  
      
	      
	```dart  
	Future.delayed(Duration(milliseconds: 500), (){  
		property_progress += 0.01; setState((){});  });  
		return Container(  
		 key: ValueKey('empty one...'), 
		 height: widget.height, 
		 /// width is unassigned here, this would force 
		 /// it's parent widget to re-render, hence rebuild 
		 /// occurs... 
		 // width: widget.width, 
		 color: Colors.blueGrey, 
		 alignment: AlignmentDirectional.centerStart, 
		 child: Container( 
			 height: widget.height, 
			 width: widget.width * min(property_progress, 1), 
			 color: MColors.random ));   
	```  
	
  __Solution:__  
  Assign width in parent [Container].  
      
	```dart  
	Future.delayed(Duration(milliseconds: 500), (){  
		property_progress += 0.01; setState((){});  });  
		return Container(  
		 key: ValueKey('empty one...'), 
		 height: widget.height, 
		 width: widget.width, 
		 color: Colors.blueGrey, 
		 alignment: AlignmentDirectional.centerStart, 
		 child: Container( 
			 height: widget.height, 
			 width: widget.width * min(property_progress, 1), 
			 color: MColors.random ));      
	```  
	
### Rendering Problems  
  
1) ### Accessing Size Information at Build Time  
	 __message:__  
	 
	 The following assertion was thrown building [WIDGET]  
	 <span style="color:blue">__Cannot get size during build.__</span>  
	         
	        ___Circumstance:___   
	> 1) pass newly created constraints from parent into child within build stage
	> 2) access context.size from build stage ?? 
	         
	       ___Solution___  
	 > 1) use layout builder to fetch constrains when you need to get  
	> size information dynamically  
	> 2) pass a constant constraints from parent into child  
          
2) ### LayoutBuilder Nested in IntrinsicHeight Widget

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTg2MjQzNzIxLDE0MDQ4NDUxOTEsLTExNT
ExNTY4MV19
-->